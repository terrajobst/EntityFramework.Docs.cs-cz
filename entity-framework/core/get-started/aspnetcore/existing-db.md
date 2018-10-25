---
title: Začínáme v ASP.NET Core – existující databáze – EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: bba2742c3f3b6da93dd4b4f170a3878fc0473bc8
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022194"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Začínáme s EF Core v ASP.NET Core s existující databáze

V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core. Můžete provést zpětnou analýzu stávající databázi k vytvoření modelu Entity Framework.

[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Požadavky

Nainstalujte následující software:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) se tyto úlohy:
  * **Vývoj pro ASP.NET a web** (v části **Web a Cloud**)
  * **Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Vytvoření databáze blogovací

Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi. Pokud jste již vytvořili **blogovací** databáze jako součást další kurz, přeskočit tyto kroky.

* Otevřít Visual Studio
* **Nástroje -> připojení k databázi...**
* Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**
* Zadejte **(localdb) \mssqllocaldb** jako **název serveru**
* Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**
* Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**
* Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**
* Zkopírujte skript níže do editoru dotazů
* Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřít Visual Studio 2017
* **Soubor > Nový > projekt...**
* V levé nabídce vyberte **nainstalováno > Visual C# > Web**
* Vyberte **webové aplikace ASP.NET Core** šablony projektu
* Zadejte **EFGetStarted.AspNetCore.ExistingDb** jako název a klikněte na **OK**
* Počkejte **nová webová aplikace ASP.NET Core** zobrazit dialogové okno
* Ujistěte se, že rozevírací seznam cílové rozhraní framework je nastaven na **.NET Core**, a rozevírací seznam verze je nastavena na **ASP.NET Core 2.1**
* Vyberte **webové aplikace (Model-View-Controller)** šablony
* Ujistěte se, že **ověřování** je nastavena na **bez ověřování**
* Klikněte na tlačítko **OK**

## <a name="install-entity-framework-core"></a>Nainstalujte Entity Framework Core

Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md). 

Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server. Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Provést zpětnou analýzu modelu

Nyní je čas vytvořit EF model založený na existující databázi.

* **Balíček NuGet –> nástroje Správce –> Konzola správce balíčků**
* Spusťte následující příkaz pro vytvoření modelu z existující databáze:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Pokud se zobrazí chyba `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.

> [!TIP]  
> Můžete určit, které tabulky, kterou chcete vygenerovat tak, že přidáte entity pro `-Tables` argument výše uvedeného příkazu. Například `-Tables Blog,Post`.

Zpětná analýza procesu vytvoření tříd entit (`Blog.cs` & `Post.cs`) a odvozené kontextu (`BloggingContext.cs`) na základě schématu existující databázi.

 Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání. Tady jsou `Blog` a `Post` tříd entit:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Povolit opožděné načtení, můžete nastavit vlastnosti navigace `virtual` (Blog.Post a Post.Blog).

 Kontext představuje relaci s databází a umožňuje dotazování a uložit instancí tříd entit.

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

## <a name="register-your-context-with-dependency-injection"></a>Kontext zaregistrovat injektáž závislostí

Koncept vkládání závislostí je zásadním ASP.NET Core. Služby (například `BloggingContext`) jsou registrované pomocí vkládání závislostí při spuštění aplikace. Komponenty, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru. Další informace o vkládání závislostí v tématu [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) článku na webu ASP.NET.

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrace a konfigurace v souboru Startup.cs kontext

Chcete-li `BloggingContext` k dispozici pro kontrolery MVC, zaregistrujte ho jako služba.

* Otevřít **Startup.cs**
* Přidejte následující `using` příkazy na začátku souboru

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Teď můžete použít `AddDbContext(...)` metody pro registraci jako služba.
* Vyhledejte `ConfigureServices(...)` – metoda
* Přidejte následující zvýrazněný kód k registraci kontextu jako služba

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> V reálné aplikaci obvykle vložíte připojovací řetězec do proměnné typu konfigurační soubor nebo prostředí. Z důvodu zjednodušení se v tomto kurzu můžete definovat v kódu. Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Vytvoření kontroleru a zobrazení

* Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníka řešení** a vyberte **Přidat -> řadiče...**
* Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **Ok**
* Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**
* Klikněte na tlačítko **přidat**

## <a name="run-the-application"></a>Spuštění aplikace

Nyní můžete spustit aplikaci sledujte v akci.

* **Ladění -> spuštění bez ladění**
* Aplikace vytvoří a otevře ve webovém prohlížeči
* Přejděte na `/Blogs`
* Klikněte na tlačítko **vytvořit nový**
* Zadejte **Url** nového blogu a klikněte na **Create**

  ![Vytvoření stránky](_static/create.png)

  ![Indexová stránka](_static/index-existing-db.png)

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak generování uživatelského rozhraní třídy kontextu a entity najdete v následujících článcích:
* [Reference – rozhraní příkazového řádku .NET nástroje Entity Framework Core](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Referenční dokumentace nástrojů v Entity Framework Core - Konzola správce balíčků](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)