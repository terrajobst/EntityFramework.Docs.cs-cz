---
title: Začínáme v rozhraní .NET Framework – stávající databázi - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812622"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Začínáme s EF základní na rozhraní .NET Framework s existující databázi

V tomto návodu vytvoříte konzolovou aplikaci, která provádí základní data přístup ověřit v databázi Microsoft SQL Server pomocí rozhraní Entity Framework. Zpětná analýza použije pro vytvoření modelu Entity Framework založené na existující databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Nejnovější verze Správce balíčků NuGet](https://dist.nuget.org/index.html)

* [Nejnovější verzi prostředí Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Databáze blogu](#blogging-database)

### <a name="blogging-database"></a>Databáze blogu

Tento kurz používá **blogu** databáze ve vaší instanci LocalDb jako existující databáze.

> [!TIP]  
> Pokud jste již vytvořili **blogu** databázi jako součást jiného kurzu, můžete přeskočit tyto kroky.

* Otevřete Visual Studio

* Nástroje > připojit k databázi...

* Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**

* Zadejte **\mssqllocaldb (localdb)** jako **název serveru**

* Zadejte **hlavní** jako **název databáze** a klikněte na tlačítko **OK**

* Hlavní databázi se nyní zobrazí v části **připojení dat** v **Průzkumníka serveru**

* Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**

* Zkopírujte skript, uvedené níže, do editoru dotazů

* Klikněte pravým tlačítkem na editoru dotazů a vyberte **spouštění**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřete Visual Studio

* Soubor > Nový > projekt...

* V levé nabídce vyberte šablony > Visual C# > Windows

* Vyberte **konzolové aplikace** šablona projektu

* Ujistěte se, kterou cílíte **rozhraní .NET Framework 4.5.1** nebo novější

* Pojmenujte projekt a klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit. Tento návod používá SQL Server. Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).

* Nástroje > Správce balíčků NuGet > Konzola správce balíčků

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Povolit zpětnou analýzu z existující databáze je potřeba nainstalovat několik dalších balíčků příliš.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Provést zpětnou analýzu modelu

Nyní je čas vytvořit model EF založený na existující databázi.

* Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků

* Spusťte následující příkaz pro vytvoření modelu z existující databáze

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Proces zpětné analýzy vytvořili tříd entit a odvozené kontext na základě schématu existující databáze. Třídy entity jsou jednoduché C# objekty, které představují data, budete mít dotazování a ukládání.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

Kontext představuje relaci s databází a umožňuje dotazování a uložit instance tříd entity.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
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

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Použití modelu

Nyní můžete modelu provádí přístup k datům.

* Otevřete *Program.cs*

* Obsah souboru nahraďte následujícím kódem

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
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

![obrázek](_static/output-existing-db.png)
