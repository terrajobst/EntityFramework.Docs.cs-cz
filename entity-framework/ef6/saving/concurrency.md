---
title: Zpracování konfliktů souběžnosti – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419690"
---
# <a name="handling-concurrency-conflicts"></a>Zpracování konfliktů souběžnosti
Optimistická souběžnost zahrnuje optimistický pokus o uložení vaší entity do databáze v případě, že data, která se od načtení entity nezměnila. Pokud dojde k tomu, že se data změnila, je vyvolána výjimka a před dalším pokusem o uložení je nutné konflikt vyřešit. Toto téma popisuje, jak zpracovat takové výjimky v Entity Framework. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

Tento příspěvek není vhodným místem pro úplnou diskuzi o optimistické souběžnosti. V následujících částech se předpokládá několik znalostí o řešení souběžnosti a zobrazení vzorů pro běžné úlohy.  

Mnohé z těchto vzorů využívají témata popsaná v tématu [práce s hodnotami vlastností](~/ef6/saving/change-tracking/property-values.md).  

Řešení problémů souběžnosti při používání nezávislých přidružení (kde cizí klíč není namapovaný na vlastnost ve vaší entitě) je mnohem obtížnější než při použití přidružení cizího klíče. Proto pokud v aplikaci provedete překlad souběžnosti, doporučujeme, abyste vždy namapovali cizí klíče do svých entit. Všechny níže uvedené příklady předpokládají, že používáte přidružení cizího klíče.  

Rozhraní SaveChanges vyvolá DbUpdateConcurrencyException při zjištění výjimky optimistického zpracování při pokusu o uložení entity, která používá přidružení cizího klíče.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Řešení optimistických výjimek souběžnosti s opětovným načtením (databáze WINS)  

Metodu reload lze použít k přepsání aktuálních hodnot entity o hodnoty nyní v databázi. Entita se pak většinou vrátí uživateli v nějakém formuláři a musí se pokusit znovu provést změny a znovu je uložit. Příklad:  

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

Dobrým způsobem, jak simulovat výjimku souběžnosti, je nastavit zarážku na volání metody SaveChanges a pak upravit entitu, která je ukládána v databázi pomocí jiného nástroje, jako je například SQL Management Studio. Můžete také vložit řádek před metodou SaveChanges a aktualizovat databázi přímo pomocí metody SqlCommand. Příklad:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Metoda Entries v DbUpdateConcurrencyException vrací instance DbEntityEntry pro entity, které se nepodařilo aktualizovat. (Tato vlastnost v současné době vždy vrací jedinou hodnotu pro problémy souběžnosti. Může vrátit více hodnot pro obecné výjimky aktualizace.) Alternativou v některých situacích může být získání položek pro všechny entity, které mohou být nutné z databáze znovu načteny, a volání pro každý z nich.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Řešení optimistických výjimek souběžnosti jako služby WINS klienta  

Výše uvedený příklad, který používá opětovné načtení, se někdy nazývá databáze WINS nebo ukládá službu WINS, protože hodnoty v entitě jsou přepsány hodnotami z databáze. Někdy můžete chtít provést opak a přepsat hodnoty v databázi hodnotami, které jsou aktuálně v entitě. Tato situace se někdy označuje jako klient WINS a je možné ji provést získáním aktuálních hodnot databáze a jejich nastavením jako původní hodnoty pro entitu. (Informace o aktuálních a původních hodnotách naleznete v tématu [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) .) Například:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Vlastní rozlišení optimistických výjimek souběžnosti  

Někdy můžete chtít kombinovat hodnoty, které jsou aktuálně v databázi, s hodnotami, které jsou aktuálně v entitě. To obvykle vyžaduje určitou vlastní logiku nebo interakci s uživatelem. Například můžete zobrazit formulář pro uživatele obsahující aktuální hodnoty, hodnoty v databázi a výchozí sadu vyřešených hodnot. Uživatel pak podle potřeby upraví vyřešené hodnoty a bude to vyřešené hodnoty, které se uloží do databáze. To se dá udělat pomocí objektů DbPropertyValues vrácených z CurrentValues a GetDatabaseValues v záznamu entity. Příklad:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Vlastní rozlišení optimistické výjimky souběžnosti pomocí objektů  

Výše uvedený kód používá instance DbPropertyValues pro předávání aktuálních, databázových a vyřešených hodnot. V některých případech může být pro tuto situaci snazší použít instance typu entity. To lze provést pomocí metod ToObject a SetValues DbPropertyValues. Příklad:  

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
