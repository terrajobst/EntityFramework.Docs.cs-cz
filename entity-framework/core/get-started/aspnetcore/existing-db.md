---
title: Začínáme s ASP.NET Core – existující databáze – EF Core
author: rowanmiller
description: Začínáme s EF Core ASP.NET Core se stávající databází
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: eeebd75bebe85994c6439e06243e113f2bda814c
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565225"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Začínáme s EF Core ASP.NET Core se stávající databází

V tomto kurzu vytvoříte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core. Můžete provést zpětnou analýzu existující databáze a vytvořit model Entity Framework.

[Podívejte se na ukázku tohoto článku na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

* [Visual Studio 2017 15,7](https://www.visualstudio.com/downloads/) s těmito úlohami:
  * **Vývoj pro ASP.NET a web** (v části **Web & Cloud**)
  * **Vývoj pro různé platformy .NET Core** (v **jiných sad nástrojů**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Vytvořit blogovací databázi

V tomto kurzu se jako existující databáze používá **blogovací** databáze na instanci LocalDB. Pokud jste již vytvořili **blogovací** databázi jako součást jiného kurzu, přeskočte tyto kroky.

* Otevřít Visual Studio
* **Nástroje – > se připojit k databázi...**
* Vyberte **Microsoft SQL Server** a klikněte na **pokračovat** .
* Jako **název serveru** zadejte **(LocalDB) \mssqllocaldb** .
* Jako **název databáze** zadejte **Master** a klikněte na **OK** .
* Hlavní databáze se teď zobrazuje v části **datová připojení** v **Průzkumník serveru**
* Klikněte pravým tlačítkem na databázi v **Průzkumník serveru** a vyberte **Nový dotaz** .
* Zkopírujte níže uvedený skript do editoru dotazů.
* Klikněte pravým tlačítkem na Editor dotazů a vyberte **Spustit** .

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Open Visual Studio 2017
* **Soubor > Nový > projektu...**
* V nabídce vlevo vyberte **nainstalované > Visual C# > Web** .
* Vyberte šablonu projektu **ASP.NET Core webové aplikace**
* Jako název zadejte **EFGetStarted. AspNetCore. ExistingDb** (musí přesně odpovídat oboru názvů, který se později použil v kódu) a klikněte na **OK** . 
* Počkejte, než se zobrazí dialogové okno **nové webové aplikace ASP.NET Core** .
* Ujistěte se, že rozevírací seznam cílové rozhraní je nastavený na **.NET Core**, a rozevírací seznam verze je nastavený na **ASP.NET Core 2,1** .
* Výběr šablony **webové aplikace (model-zobrazení-kontroler)**
* Ujistěte se, že **ověřování** je nastaveno na **bez ověřování** .
* Klikněte na **OK** .

## <a name="install-entity-framework-core"></a>Nainstalovat Entity Framework Core

Chcete-li nainstalovat EF Core, nainstalujte balíček pro zprostředkovatele EF Corech databází, které chcete cílit. Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databáze](../../providers/index.md). 

Pro účely tohoto kurzu nemusíte instalovat balíček poskytovatele, protože tento kurz používá SQL Server. Balíček poskytovatele SQL Server je součástí [Microsoft. AspnetCore. app Metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Zpětná analýza modelu

Nyní je čas vytvořit model EF založený na vaší stávající databázi.

* **Nástroje – > Správce balíčků NuGet – > konzolu Správce balíčků**
* Spuštěním následujícího příkazu vytvořte model z existující databáze:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Pokud se zobrazí chyba s oznámením `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.

> [!TIP]  
> Přidáním `-Tables` argumentu do výše uvedeného příkazu můžete určit, pro které tabulky chcete generovat entity. Například, `-Tables Blog,Post`.

Proces zpětného analýz vytvořil třídy entit (`Blog.cs` & `Post.cs`) a odvozený kontext (`BloggingContext.cs`) založený na schématu existující databáze.

 Třídy entit jsou jednoduché C# objekty, které reprezentují data, která budete dotazovat a ukládat. Tady jsou `Blog` třídy entit `Post` a:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Chcete-li povolit opožděné načítání, můžete vytvořit `virtual` navigační vlastnosti (blog.post a post. blog).

 Kontext představuje relaci s databází a umožňuje dotazování a ukládání instancí tříd entit.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Registrace kontextu pomocí injektáže závislosti

Koncept vkládání závislostí je střední až ASP.NET Core. Služby (například `BloggingContext`) jsou registrovány se vkládáním závislostí během spuštění aplikace. Služby, které vyžadují tyto služby (například řadiče MVC), pak poskytují tyto služby prostřednictvím parametrů nebo vlastností konstruktoru. Další informace o vkládání závislostí najdete v článku o [Injektáže závislosti](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) na webu ASP.NET.

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrace a konfigurace kontextu v Startup.cs

Pokud ho `BloggingContext` chcete zpřístupnit pro řadiče MVC, zaregistrujte ho jako službu.

* Otevřít **Startup.cs**
* Na začátek souboru `using` přidejte následující příkazy.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Nyní můžete použít `AddDbContext(...)` metodu k její registraci jako službu.
* `ConfigureServices(...)` Vyhledejte metodu
* Přidejte následující zvýrazněný kód pro registraci kontextu jako služby.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> V reálné aplikaci byste obvykle vložili připojovací řetězec do konfiguračního souboru nebo proměnné prostředí. V zájmu jednoduchosti vám tento kurz definovali v kódu. Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Vytvoření kontroleru a zobrazení

* Klikněte pravým tlačítkem na složku Controllers v **Průzkumník řešení** a vyberte **Add-> Controller...**
* Vyberte **kontroler MVC se zobrazeními, pomocí Entity Framework** a klikněte na **OK** .
* Nastavení **třídy modelu** na **blog** a **třídu kontextu dat** na **BloggingContext**
* Klikněte na **Přidat** .

## <a name="run-the-application"></a>Spuštění aplikace

Teď můžete aplikaci spustit, aby se zobrazila v akci.

* **Ladění – > Spustit bez ladění**
* Aplikace se vytvoří a otevře ve webovém prohlížeči.
* Přejděte na `/Blogs`
* Klikněte na **vytvořit nový** .
* Zadejte **adresu URL** nového blogu a klikněte na **vytvořit** .

  ![Vytvoření stránky](_static/create.png)

  ![Stránka indexu](_static/index-existing-db.png)

## <a name="next-steps"></a>Další postup

Další informace o tom, jak vytvořit generátory kontextu a třídy entit, najdete v následujících článcích:
* [Zpětná analýza](xref:core/managing-schemas/scaffolding)
* [Referenční informace k nástrojům pro Entity Framework Core – .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Referenční informace o nástrojích Entity Framework Core Tools – konzola správce balíčků](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
