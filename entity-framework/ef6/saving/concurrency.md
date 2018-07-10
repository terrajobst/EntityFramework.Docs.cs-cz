---
title: Zpracování konfliktů souběžnosti - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
caps.latest.revision: 3
ms.openlocfilehash: b8608dbb4cadd60ff4ff4984583f8a9d843b3949
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912019"
---
# <a name="handling-concurrency-conflicts"></a>Zpracování konfliktů souběžnosti
Optimistická souběžnost zahrnuje optimisticky pokusu o uložení vaší entity k databázi v naději, že nedošlo ke změně tamní data od entita byla načtena. Ukázalo se, která byla data změněna, je vyvolána výjimka, a musíte vyřešit konflikt, než se pokusíte uložit znovu. Toto téma popisuje způsob zpracování těchto výjimek v rozhraní Entity Framework. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

Tento příspěvek není vhodné místo pro úplnou diskusi o optimistického řízení souběžnosti. Následující části se předpokládá základní znalost souběžnosti řešení a zobrazení schémat pro běžné úlohy.  

Ujistěte se, mnohé z těchto vzorců použití témata v [práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md).  

Řešení potíží s souběžnosti, při použití nezávislé přidružení (kde cizí klíč není namapována na vlastnost ve vaší entitě) je mnohem obtížnější než při použití přidružení cizího klíče. Proto pokud se chystáte provést rozlišení souběžnosti v aplikaci se doporučuje vždy mapování cizí klíče do entity. Následující příklady předpokládají, že používáte přidružení cizího klíče.  

DbUpdateConcurrencyException je vyvolané SaveChanges zjištěném optimistického řízení souběžnosti výjimka při pokusu o uložení entity, která využívá přidružení cizího klíče.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Řešení výjimek optimistického řízení souběžnosti s Reload (databáze služby wins)  

Znovu načíst metodu je možné přepsat aktuální hodnoty entity s hodnotami v databázi. Entita potom obvykle dostane zpět uživateli v nějaké podobě a musí pokusit znovu proveďte své změny a znovu ho uložit. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Nastavení zarážky na volání SaveChanges a následně upravit entitu, která se ukládá v databázi pomocí jiného nástroje, jako jsou SQL Management Studio je dobrým způsobem, jak simulovat výjimky souběžnosti. Může také vložení řádku před SaveChanges aktualizace databáze přímo pomocí třídy SqlCommand. Příklad:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Metoda položky na DbUpdateConcurrencyException vrátí instance DbEntityEntry pro entity, které se nepovedlo aktualizovat. (Tato vlastnost aktuálně vždy vrátí jednu hodnotu pro problémy s souběžnosti. Může vrátit více hodnot pro výjimky obecné aktualizace.) Alternativu v některých situacích může být k získání položek pro všechny entity, které může být potřeba znovu načíst z databáze a znovu načtěte volání pro každý z nich.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Řešení výjimek optimistického řízení souběžnosti jako klient služby wins  

V příkladu výše, který používá nové načtení se někdy označuje jako databáze wins nebo úložiště služby wins, protože hodnoty v této entitě jsou přepsány hodnoty z databáze. Někdy můžete chtít provést opak a přepsání hodnot v databázi s hodnotami aktuálně v dané entitě. To se někdy označuje jako klient wins a je možné díky získání aktuální hodnoty v databázi a nastavit je jako původní hodnoty pro entitu. (Viz [práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md) informace o aktuální a původní hodnoty.) Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Vlastní řešení výjimek optimistického řízení souběžnosti  

Někdy můžete chtít kombinací hodnot aktuálně v databázi s hodnotami aktuálně v dané entitě. To obvykle vyžaduje některé vlastní logiku nebo uživatelské interakce. Například mohou představovat formulář obsahující aktuálních hodnot, hodnot v databázi, uživateli a výchozí sadu přeložit hodnoty. Uživatel by pak upravte přeložit hodnoty podle potřeby a bylo by tyto přeložit hodnoty, které se neuloží do databáze. To lze provést pomocí objektů DbPropertyValues vrácená CurrentValues a GetDatabaseValues na záznam entity. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Vlastní řešení pomocí objektů výjimek optimistického řízení souběžnosti  

Výše uvedený kód používá DbPropertyValues instance pro předávání kolem aktuálního, databáze a vyřešení hodnoty. V některých případech může být jednodušší použít instance typu entity pro tuto. To lze provést pomocí metody ToObject a SetValues DbPropertyValues. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
