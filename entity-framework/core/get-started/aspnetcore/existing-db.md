---
title: Začínáme na základní EF ASP.NET Core - existující databázi –
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Začínáme s EF základní na ASP.NET Core s existující databázi

V tomto návodu vytvoříte aplikaci ASP.NET MVC jádra, která provádí základní data přístup pomocí rozhraní Entity Framework. Zpětná analýza použije pro vytvoření modelu Entity Framework založené na existující databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) s tyto úlohy:
  * **Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)
  * **Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)
* [Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Databáze blogu](#blogging-database)

### <a name="blogging-database"></a>Databáze blogu

Tento kurz používá **blogu** databáze ve vaší instanci LocalDb jako existující databáze.

> [!TIP]  
> Pokud jste již vytvořili **blogu** databázi jako součást jiného kurzu, můžete přeskočit tyto kroky.

* Open Visual Studio
* **Nástroje -> připojit k databázi...**
* Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**
* Zadejte **\mssqllocaldb (localdb)** jako **název serveru**
* Zadejte **hlavní** jako **název databáze** a klikněte na tlačítko **OK**
* Hlavní databázi se nyní zobrazí v části **připojení dat** v **Průzkumníka serveru**
* Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**
* Zkopírujte skript, uvedené níže, do editoru dotazů
* Klikněte pravým tlačítkem na editoru dotazů a vyberte **spouštění**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřete Visual Studio 2017
* **Soubor -> New -> projekt...**
* V levé nabídce vyberte **nainstalovaná -> šablony pro -> Visual C# -> Web**
* Vyberte **webové aplikace ASP.NET Core (.NET Core)** šablona projektu
* Zadejte **EFGetStarted.AspNetCore.ExistingDb** jako název a klikněte na tlačítko **OK**
* Počkejte **nové webové aplikace ASP.NET Core** zobrazit dialogové okno
* V části **technologii ASP.NET 2.0 šablony základní** vyberte **webové aplikace (Model-View-Controller)**
* Ujistěte se, že **ověřování** je nastaven na **bez ověřování**
* Klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit. Tento návod používá SQL Server. Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Použijeme některé nástroje Entity Framework pro vytvoření modelu z databáze. Proto se nainstaluje balíček nástroje:

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

Nemůžeme vytvořit kontrolery a zobrazení později na pomocí některé nástroje ASP.NET Core generování uživatelského rozhraní. Proto se nainstaluje tohoto návrhu balíčku:

* Spustit `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Provést zpětnou analýzu modelu

Nyní je čas vytvořit model EF založený na existující databázi.

* **Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků**
* Spusťte následující příkaz pro vytvoření modelu z existující databáze:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Pokud se zobrazí chybová zpráva, `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.

> [!TIP]  
> Můžete určit, které tabulky, kterou chcete vygenerovat entity pro přidáním `-Tables` argument výše uvedeného příkazu. Například `-Tables Blog,Post`.

Proces zpětné analýzy vytvořen tříd entit (`Blog.cs` & `Post.cs`) a odvozené kontextu (`BloggingContext.cs`) na základě schématu existující databáze.

 Třídy entity jsou jednoduché C# objekty, které představují data, budete mít dotazování a ukládání.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Kontext představuje relaci s databází a umožňuje dotazování a uložit instance tříd entity.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

## <a name="register-your-context-with-dependency-injection"></a>Zaregistrovat váš kontext vkládání závislostí

Koncept vkládání závislostí je důležitá pro ASP.NET Core. Služby (například `BloggingContext`) jsou registrovány vkládání závislostí při spuštění aplikace. Součásti, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím konstruktor parametry nebo vlastnosti. Další informace o vkládání závislostí v tématu [vkládání závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) článku na webu technologie ASP.NET.

### <a name="remove-inline-context-configuration"></a>Odebrat konfiguraci vložený kontext

V ASP.NET Core, konfigurace se obvykle provádí v **Startup.cs**. Tak, aby odpovídala tento vzor, jsme se přesune o konfiguraci zprostředkovatele databáze na **Startup.cs**.

* Otevřete `Models\BloggingContext.cs`
* Odstranit `OnConfiguring(...)` – metoda

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Přidejte následující konstruktor, který vám umožní konfigurace, které mají být předány do kontextu pomocí vkládání závislostí

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Zaregistrovat a nakonfigurovat v souboru Startup.cs váš kontext

V pořadí pro naše řadiče MVC, chcete-li použít `BloggingContext` přidáme a zaregistrujte ho jako služba.

* Otevřete **Startup.cs**
* Přidejte následující `using` příkazy na začátku souboru

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Nyní můžeme použít `AddDbContext(...)` metody pro registraci jako služba.
* Vyhledejte `ConfigureServices(...)` – metoda
* Přidejte následující kód k registraci kontextu jako služby

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> V reálné aplikaci byste obvykle umístili připojovací řetězec v konfiguračním souboru. Z důvodu zjednodušení jsme se jeho definováním v kódu. Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Vytvořit řadič

Dál jsme budete povolte generování uživatelského rozhraní v našem projektu.

* Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat -> řadiče...**
* Vyberte **úplné závislosti** a klikněte na tlačítko **přidat**
* Můžete ignorovat podle pokynů `ScaffoldingReadMe.txt` soubor, který otevírá

Teď, když je povoleno generování uživatelského rozhraní, jsme vygenerovat řadič pro `Blog` entity.

* Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat -> řadiče...**
* Vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **Ok**
* Nastavit **třída modelu** k **Blog** a **třída kontextu dat** k **BloggingContext**
* Klikněte na tlačítko **přidat**

## <a name="run-the-application"></a>Spuštění aplikace

Nyní můžete spustit aplikaci najdete v části v akci.

* **-Ladění > Spustit bez ladění**
* Aplikace bude sestavovat a otevřete ve webovém prohlížeči
* Přejděte na `/Blogs`
* Klikněte na tlačítko **vytvořit nový**
* Zadejte **Url** pro nové blog a klikněte na tlačítko **vytvořit**

![obrázek](_static/create.png)

![obrázek](_static/index-existing-db.png)
