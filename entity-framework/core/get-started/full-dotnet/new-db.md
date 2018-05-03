---
title: Začínáme v rozhraní .NET Framework – nové databáze - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Začínáme s EF základní na rozhraní .NET Framework s novou databázi

V tomto návodu vytvoříte konzolovou aplikaci, která provádí základní data přístup ověřit v databázi Microsoft SQL Server pomocí rozhraní Entity Framework. Migrace použije k vytvoření databáze z modelu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Nejnovější verze Správce balíčků NuGet](https://dist.nuget.org/index.html)

* [Nejnovější verzi prostředí Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřete Visual Studio

* Soubor > Nový > projekt...

* V levé nabídce vyberte šablony > Visual C# > klasické ploše systému Windows

* Vyberte **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu

* Ujistěte se, kterou cílíte **rozhraní .NET Framework 4.5.1** nebo novější

* Pojmenujte projekt a klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit. Tento návod používá SQL Server. Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).

* Nástroje > Správce balíčků NuGet > Konzola správce balíčků

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Dále v tomto návodu také použijeme některé nástroje Entity Framework pro správnou údržbu databáze. Proto se nainstaluje balíček nástroje.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>Vytvoření modelu

Nyní je čas k definování kontextu a entity tříd, které tvoří modelu.

* Projekt > přidejte třídu...

* Zadejte *Model.cs* jako název a klikněte na tlačítko **OK**

* Obsah souboru nahraďte následujícím kódem

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> V reálné aplikaci byste put každá třída v samostatném souboru a chápat připojovací řetězec `App.Config` souboru a přečtěte si ho použití `ConfigurationManager`. Z důvodu zjednodušení jsme jsou uvedení všechno, co v souboru jednoho kódu pro účely tohoto kurzu.

## <a name="create-your-database"></a>Vytvoření databáze

Teď, když máte model, můžete k vytvoření databáze pro vás migrace.

* Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků

* Spustit `Add-Migration MyFirstMigration` chcete vygenerovat migraci k vytvoření počáteční sadu tabulek pro váš model.

* Spustit `Update-Database` použít nové migrace do databáze. Protože vaše databáze ještě neexistuje, vytvoří se vám před použitím migrace.

> [!TIP]  
> Pokud provedete budoucí změny modelu, můžete použít `Add-Migration` příkaz chcete vygenerovat nový migraci, aby schéma odpovídající změn v databázi. Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny provedené), můžete použít `Update-Database` příkaz k použití změn do databáze.
>
>Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, která již byla do databáze použít.

## <a name="use-your-model"></a>Použití modelu

Nyní můžete modelu provádí přístup k datům.

* Otevřete *Program.cs*

* Obsah souboru nahraďte následujícím kódem

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Ladění > Spustit bez ladění

Zobrazí se, že blogů se ukládá do databáze a pak budou vytištěny podrobnosti o všech blogy ke konzole.

![obrázek](_static/output-new-db.png)
