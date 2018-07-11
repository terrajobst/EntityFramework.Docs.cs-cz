---
title: Začínáme v ASP.NET Core – existující databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949150"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Začínáme s EF Core v ASP.NET Core s existující databáze

V tomto návodu vytvoříte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Frameworku. Zpětná analýza použije k vytvoření Entity Framework model založený na existující databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující požadované součásti jsou potřeba k dokončení tohoto návodu:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) se tyto úlohy:
  * **Vývoj pro ASP.NET a web** (v části **Web a Cloud**)
  * **Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)
* [.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Blogovací databáze](#blogging-database)

### <a name="blogging-database"></a>Blogovací databáze

Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi.

> [!TIP]  
> Pokud jste již vytvořili **blogovací** databázi jako součást další kurz, můžete přeskočit tyto kroky.

* Otevřít Visual Studio
* **Nástroje -> připojení k databázi...**
* Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**
* Zadejte **(localdb) \mssqllocaldb** jako **název serveru**
* Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**
* Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**
* Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**
* Zkopírujte skript uvedený níže, do editoru dotazů
* Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřít Visual Studio 2017
* **Soubor -> Nový -> projekt...**
* V levé nabídce vyberte **nainstalováno -> šablony -> Visual C# -> Web**
* Vyberte **webová aplikace ASP.NET Core (.NET Core)** šablony projektu
* Zadejte **EFGetStarted.AspNetCore.ExistingDb** jako název a klikněte na **OK**
* Počkejte **nová webová aplikace ASP.NET Core** zobrazit dialogové okno
* V části **2.0 šablony ASP.NET Core** vyberte **webové aplikace (Model-View-Controller)**
* Ujistěte se, že **ověřování** je nastavena na **bez ověřování**
* Klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento návod používá systém SQL Server. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* **Nástroje > Správce balíčků NuGet > Konzola správce balíčků**

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Použijeme některé Entity Framework Tools pro vytvoření modelu z databáze. Proto se nainstaluje balíček nástroje:

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

Jsme k vytvoření kontrolerů a zobrazení později pomocí některé nástroje pro generování uživatelského rozhraní ASP.NET Core. Proto se nainstaluje tohoto návrhu balíčku:

* Spustit `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

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

 Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Kontext představuje relaci s databází a umožňuje dotazování a uložit instancí tříd entit.

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

## <a name="register-your-context-with-dependency-injection"></a>Kontext zaregistrovat injektáž závislostí

Koncept vkládání závislostí je zásadním ASP.NET Core. Služby (například `BloggingContext`) jsou registrované pomocí vkládání závislostí při spuštění aplikace. Komponenty, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru. Další informace o vkládání závislostí v tématu [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) článku na webu ASP.NET.

### <a name="remove-inline-context-configuration"></a>Odebrat konfiguraci kontextu vložené

V ASP.NET Core, konfigurace se obvykle provádí v **Startup.cs**. Tak, aby odpovídal na tento model přesuneme konfigurace poskytovatele databáze za účelem **Startup.cs**.

* Otevřít `Models\BloggingContext.cs`
* Odstranit `OnConfiguring(...)` – metoda

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Přidejte následující konstruktor, který vám umožní konfigurace mají být předány do kontextu pomocí vkládání závislostí

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrace a konfigurace v souboru Startup.cs kontext

Aby naše kontrolery MVC, aby využívání `BloggingContext` budeme registrovat jako službu.

* Otevřít **Startup.cs**
* Přidejte následující `using` příkazy na začátku souboru

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Teď můžeme použít `AddDbContext(...)` metody pro registraci jako služba.
* Vyhledejte `ConfigureServices(...)` – metoda
* Přidejte následující kód k registraci kontextu jako služba

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> V reálné aplikaci byste obvykle vložte připojovací řetězec v konfiguračním souboru. Z důvodu zjednodušení můžeme ji definování v kódu. Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Vytvoření kontroleru

Dále jsme v našem projektu budete povolit generování uživatelského rozhraní.

* Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníka řešení** a vyberte **Přidat -> řadiče...**
* Vyberte **úplné závislosti** a klikněte na tlačítko **přidat**
* Můžete ignorovat podle pokynů `ScaffoldingReadMe.txt` soubor, který otevírá

Teď, když je povoleno generování uživatelského rozhraní, jsme generování uživatelského rozhraní pro řadič `Blog` entity.

* Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníka řešení** a vyberte **Přidat -> řadiče...**
* Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **Ok**
* Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**
* Klikněte na tlačítko **přidat**

## <a name="run-the-application"></a>Spuštění aplikace

Nyní můžete spustit aplikaci sledujte v akci.

* **Ladění -> spuštění bez ladění**
* Aplikace bude sestavení a otevře ve webovém prohlížeči
* Přejděte na `/Blogs`
* Klikněte na tlačítko **vytvořit nový**
* Zadejte **Url** nového blogu a klikněte na **Create**

![obrázek](_static/create.png)

![obrázek](_static/index-existing-db.png)
