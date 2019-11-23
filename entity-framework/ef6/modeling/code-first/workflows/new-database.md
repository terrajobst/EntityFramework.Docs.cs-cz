---
title: Code First k nové databázi – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182571"
---
# <a name="code-first-to-a-new-database"></a>Code First k nové databázi
Toto video a podrobný návod vám poskytnou Úvod do Code Firstho vývoje, který cílí na novou databázi. Tento scénář zahrnuje cílení na databázi, která neexistuje, Code First vytvoří nebo prázdnou databázi, do které Code First přidat nové tabulky. Code First umožňuje definovat model pomocí tříd C\# nebo VB.Net. Další konfiguraci můžete volitelně provést pomocí atributů u tříd a vlastností nebo pomocí rozhraní API Fluent.

## <a name="watch-the-video"></a>Přehrát video
Toto video poskytuje Úvod do Code First vývoje cílící na novou databázi. Tento scénář zahrnuje cílení na databázi, která neexistuje, Code First vytvoří nebo prázdnou databázi, do které Code First přidat nové tabulky. Code First umožňuje definovat model pomocí C# tříd nebo VB.NET. Další konfiguraci můžete volitelně provést pomocí atributů u tříd a vlastností nebo pomocí rozhraní API Fluent.

**Prezentující**: [Rowan Miller](https://romiller.com/)

**Video**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Předpoklady

Abyste mohli dokončit tento návod, budete muset mít nainstalovanou aspoň Visual Studio 2010 nebo Visual Studio 2012.

Pokud používáte Visual Studio 2010, budete také muset nainstalovat [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

## <a name="1-create-the-application"></a>1. Vytvoření aplikace

Aby se zajistilo něco jednoduchého, vytvoříme základní konzolovou aplikaci, která používá Code First k provádění přístupu k datům.

-   Otevřít Visual Studio
-   **Soubor –&gt; projekt New-&gt;...**
-   V levé nabídce a v **konzolové aplikaci** vyberte **Windows** .
-   Jako název zadejte **CodeFirstNewDatabaseSample** .
-   Vybrat **OK**

## <a name="2-create-the-model"></a>2. vytvoření modelu

Pojďme definovat velmi jednoduchý model pomocí tříd. Jenom jsme definovali v souboru Program.cs, ale v reálné aplikaci byste své třídy rozdělili do samostatných souborů a potenciálně samostatného projektu.

Pod definicí třídy programu v Program.cs přidejte následující dvě třídy.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Všimněte si, že vytváříme dvě navigační vlastnosti (blog. post a post. blog) Virtual. To umožňuje funkci opožděného načítání Entity Framework. Opožděné načítání znamená, že obsah těchto vlastností bude automaticky načten z databáze při pokusu o přístup k nim.

## <a name="3-create-a-context"></a>3. vytvoření kontextu

Nyní je čas definovat odvozený kontext, který představuje relaci s databází, a můžeme nám umožnit dotazování a ukládání dat. Definujeme kontext, který je odvozen z typu System. data. entity. DbContext a zpřístupňuje typ Negenerickými&lt;TEntity&gt; pro každou třídu v našem modelu.

Teď Začínáme používat typy z Entity Framework, takže musíme přidat balíček NuGet EntityFramework.

-   **Projekt –&gt; spravovat balíčky NuGet...**
    Poznámka: Pokud nemáte **balíčky pro správu NuGet...** možnost, měli byste nainstalovat [nejnovější verzi nugetu](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .
-   Vyberte **online** kartu.
-   Výběr balíčku **EntityFramework**
-   Klikněte na **nainstalovat** .

Do horní části Program.cs přidejte příkaz using pro System. data. entity.

``` csharp
using System.Data.Entity;
```

Pod třídou post v Program.cs přidejte následující odvozený kontext.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Tady je úplný seznam toho, co by měl teď Program.cs obsahovat.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

To je celý kód, který potřebujeme k zahájení ukládání a načítání dat. Zjevně se trochu děje na pozadí a my se podíváme na to chvilku, ale nejdřív to uvidíme v akci.

## <a name="4-reading--writing-data"></a>4. čtení & zápisu dat

Implementujte metodu Main v Program.cs, jak je znázorněno níže. Tento kód vytvoří novou instanci našeho kontextu a pak ho použije k vložení nového blogu. Pak použije dotaz LINQ k načtení všech blogů z databáze seřazené abecedně podle názvu.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Nyní můžete spustit aplikaci a otestovat ji.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Kde jsou moje data?

Podle konvence DbContext pro vás vytvořila databázi.

-   Pokud je k dispozici místní instance SQL Express (ve výchozím nastavení nainstalovaná v rámci sady Visual Studio 2010), pak Code First vytvořila databázi v této instanci.
-   Pokud SQL Express není k dispozici, Code First se pokusí a použije [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (ve výchozím nastavení nainstalované ve Visual Studiu 2012).
-   Databáze je pojmenována za plně kvalifikovaný název odvozeného kontextu, v našem případě **CodeFirstNewDatabaseSample. BloggingContext.**

Jedná se pouze o výchozí konvence a existují různé způsoby, jak změnit databázi, kterou Code First používá. Další informace jsou k dispozici ve **způsobu, jakým DbContext zjistí téma připojení modelu a databáze** .
K této databázi se můžete připojit pomocí Průzkumník serveru v aplikaci Visual Studio.

-   **Zobrazení-&gt; Průzkumník serveru**
-   Klikněte pravým tlačítkem na **datová připojení** a vyberte **Přidat připojení...**
-   Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat

    ![Výběr zdroje dat](~/ef6/media/selectdatasource.png)

-   Připojte se k LocalDB nebo SQL Express v závislosti na tom, který z nich máte nainstalovanou.

Nyní můžeme zkontrolovat schéma, které Code First vytvořili.

![Počáteční schéma](~/ef6/media/schemainitial.png)

DbContext rozpracovali třídy, které se mají zahrnout do modelu, a Prohlédněte si vlastnosti Negenerickými, které jsme definovali. Pak používá výchozí sadu Code First konvence k určení názvů tabulek a sloupců, určování datových typů, hledání primárních klíčů atd. Později v tomto návodu se podíváme na to, jak můžete tyto konvence přepsat.

## <a name="5-dealing-with-model-changes"></a>5. zvládnutí změn modelu

Teď je čas udělat v našem modelu nějaké změny. když tyto změny provedeme, musíme také aktualizovat schéma databáze. K tomu slouží funkce s názvem Migrace Code First nebo migrace pro krátké.

Migrace nám umožňuje mít seřazenou sadu kroků, které popisují postup upgradu (a downgrade) našeho schématu databáze. Každý z těchto kroků, který se označuje jako migrace, obsahuje kód, který popisuje změny, které se mají použít. 

Prvním krokem je povolení Migrace Code First pro naše BloggingContext.

-   **Nástroje – Správce balíčků knihovny&gt; –&gt; konzolu Správce balíčků**
-   Spuštění příkazu **Povolit – migrace** v konzole správce balíčků
-   Do projektu se přidala nová složka migrace, která obsahuje dvě položky:
    -   **Configuration.cs** – tento soubor obsahuje nastavení, která migrace použije pro migraci BloggingContext. Pro tento návod nemusíte nic měnit, ale tady je místo, kde můžete zadat počáteční data, registrovat poskytovatele pro další databáze, měnit obor názvů, v němž jsou tyto migrace generovány atd.
    -   **&lt;časové razítko&gt;\_InitialCreate.cs** – Toto je vaše první migrace, představuje změny, které již byly pro databázi aplikovány, aby se převzaly do prázdné databáze, která obsahuje tabulky Blogy a příspěvky. I když umožníme Code First automaticky vytvořit tyto tabulky pro nás, teď jsme se rozhodli, že jsme se rozhodli pro migrace, které jsme převedli na migraci. Code First také zaznamenal v naší místní databázi, že tato migrace již byla použita. Časové razítko v názvu souboru se používá pro účely řazení.

    Teď provedeme změnu našeho modelu a přidat do třídy blogu vlastnost URL:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Spusťte příkaz **Add-Migration AddUrl** v konzole správce balíčků.
    Příkaz pro přidání migrace kontroluje od poslední migrace změny a vytvoří novou migraci s případnými změnami, které byly nalezeny. Migraci můžeme pojmenovat. v tomto případě zavoláme migraci "AddUrl".
    Generovaný kód říká, že musíme přidat sloupec URL, který může uchovávat data řetězce, do dbo. Tabulka blogů V případě potřeby můžeme upravit kód vygenerovaného kódu, ale v tomto případě to není nutné.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Spusťte příkaz **Update-Database** v konzole správce balíčků. Tento příkaz použije všechny nedokončené migrace do databáze. Naše migrace InitialCreate již byla použita, takže migrace budou pouze uplatňovat naši novou migraci AddUrl.
    Tip: při volání metody Update-Database můžete použít přepínač **– verbose** a zobrazit tak SQL, který se spouští proti databázi.

Nový sloupec adresa URL je nyní přidán do tabulky Blogy v databázi:

![Schéma s adresou URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. datové poznámky

Zatím jsme zjistili, že se v EF zjistil model pomocí svých výchozích konvencí, ale v případě, že naše třídy nedodržují konvence, je potřeba, abyste mohli provést další konfiguraci. Existují dvě možnosti. Podíváme se na poznámky k datům v této části a potom rozhraní Fluent API v další části.

-   Pojďme do našeho modelu přidat třídu uživatelů

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Musíme také přidat sadu do našeho odvozeného kontextu.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Při pokusu o přidání migrace se zobrazí chyba*s informací, že uživatel EntityType nemá definován žádný klíč. Definujte klíč pro tento typ EntityType.* vzhledem k tomu, že EF nemá žádný způsob, jak poznáte, že uživatelské jméno by mělo být primární klíč uživatele.
-   Nyní budeme používat poznámky k datům, takže musíme přidat příkaz using na začátek Program.cs

```csharp
using System.ComponentModel.DataAnnotations;
```

-   Nyní zadejte poznámku na vlastnost username, abyste identifikovali, že se jedná o primární klíč.

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Použití příkazu **Add-Migration adduser** k vytvoření uživatelského rozhraní migrace pro použití těchto změn v databázi
-   Spuštění příkazu **Update-Database** pro použití nové migrace do databáze

Nová tabulka je nyní přidána do databáze:

![Schéma s uživateli](~/ef6/media/schemawithusers.png)

Úplný seznam poznámek podporovaných v EF jsou:

-   [Attribute – atribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Fluent API

V předchozí části jsme se podívali na používání datových poznámek k doplnění nebo přepsání toho, co zjistila konvence. Druhým způsobem, jak model nakonfigurovat, je prostřednictvím rozhraní Code First Fluent API.

Většinu konfigurací modelů můžete provádět pomocí jednoduchých datových poznámek. Rozhraní Fluent API je pokročilejším způsobem určení konfigurace modelu, která zahrnuje všechno, co poznámky k datům můžou dělat kromě pokročilejších konfigurací, které nejsou možné u datových poznámek. Datové poznámky a rozhraní API Fluent lze použít společně.

Pokud chcete získat přístup ke službě Fluent API, přepište metodu OnModelCreating v DbContext. Řekněme, že jsme chtěli Přejmenovat sloupec, na který je uživatel. DisplayName uložený, aby se zobrazil název\_.

-   Přepsat metodu OnModelCreating na BloggingContext pomocí následujícího kódu

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Použijte příkaz **Add-Migration ChangeDisplayName** pro vytvoření uživatelského rozhraní migrace a použijte tyto změny v databázi.
-   Spusťte příkaz **Update-Database** , který použije novou migraci do databáze.

Sloupec DisplayName se teď přejmenoval na zobrazení\_název:

![Schéma se přejmenováním zobrazovaného názvu](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na Code First vývoj s využitím nové databáze. Definovali jsme model pomocí tříd a pak jsme tento model použili k vytvoření databáze a uložení a načtení dat. Po vytvoření databáze jsme použili Migrace Code First ke změně schématu v podobě vývoje našeho modelu. Zjistili jsme také, jak nakonfigurovat model pomocí datových poznámek a rozhraní Fluent API.
