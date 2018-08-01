---
title: Začínáme v rozhraní .NET Framework – existující databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388465"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Začínáme s EF Core na rozhraní .NET Framework s existující databáze

V tomto návodu vytvoříte konzolovou aplikaci, která provádí základní přístup k datům pro databázi serveru Microsoft SQL Server používá nástroj Entity Framework. Zpětná analýza použije k vytvoření Entity Framework model založený na existující databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) na Githubu.

## <a name="prerequisites"></a>Požadavky

Následující požadované součásti jsou potřeba k dokončení tohoto návodu:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) – minimální verze 15.3

* [Nejnovější verzi správce balíčků NuGet](https://dist.nuget.org/index.html)

* [Nejnovější verzi prostředí Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Blogovací databáze](#blogging-database)

### <a name="blogging-database"></a>Blogovací databáze

Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi.

> [!TIP]  
> Pokud jste již vytvořili **blogovací** databázi jako součást další kurz, můžete přeskočit tyto kroky.

* Otevřít Visual Studio

* Nástroje > připojení k databázi...

* Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**

* Zadejte **(localdb) \mssqllocaldb** jako **název serveru**

* Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**

* Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**

* Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**

* Zkopírujte skript uvedený níže, do editoru dotazů

* Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Vytvoření nového projektu

* Otevřít Visual Studio

* Soubor > Nový > projekt...

* V levé nabídce vyberte šablony > Visual C# > Windows

* Vyberte **konzolovou aplikaci** šablony projektu

* Zkontrolujte cílovou **rozhraní .NET Framework 4.6.1** nebo novější

* Pojmenujte projekt a klikněte na tlačítko **OK**

## <a name="install-entity-framework"></a>Nainstalujte rozhraní Entity Framework

Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit. Tento návod používá systém SQL Server. Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).

* Nástroje > Správce balíčků NuGet > Konzola správce balíčků

* Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Povolit zpětnou analýzu z existující databáze potřebujeme příliš nainstalovat několik dalších balíčků.

* Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Provést zpětnou analýzu modelu

Nyní je čas vytvořit EF model založený na existující databázi.

* Balíček NuGet –> nástroje Správce –> Konzola správce balíčků

* Spusťte následující příkaz pro vytvoření modelu z existující databáze

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Zpětná analýza procesu vytvoření tříd entit a odvozené kontext na základě schématu existující databázi. Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání.

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

Kontext představuje relaci s databází a umožňuje dotazování a uložit instancí tříd entit.

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

Teď můžete svůj model přístup k datům.

* Otevřít *Program.cs*

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

* Ladit > Spustit bez ladění

Uvidíte, že blogů se uloží do databáze a pak se podrobnosti o všech blogy vytisknou na konzole.

![obrázek](_static/output-existing-db.png)
