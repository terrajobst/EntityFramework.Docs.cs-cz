---
title: Poskytovatelé databáze – EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: db06906e6af518a27a21f30b12d722ce06e9bd52
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813512"
---
# <a name="database-providers"></a>Poskytovatelé databází

Entity Framework Core mají přístup k mnoha různým databázím prostřednictvím knihoven modulů plug-in nazývaných poskytovatelé databáze.

## <a name="current-providers"></a>Aktuální zprostředkovatelé
> [!IMPORTANT]  
> Poskytovatelé EF Core jsou postavené na nejrůznějších zdrojích. Ne všichni poskytovatelé se uchovávají jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Při zvažování poskytovatele nezapomeňte vyhodnotit kvalitu, licencování, podporu atd., abyste zajistili, že splňují vaše požadavky. Také se ujistěte, že v dokumentaci pro každého poskytovatele najdete podrobné informace o kompatibilitě verzí.

| Balíček NuGet                                                                                                        | Podporované databázové stroje | Údržba/dodavatel                                                           | Poznámky/požadavky | Užitečné odkazy                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 a vyšší    | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) Microsoft |                      | [doc](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3,7 a vyšší         | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) Microsoft |                      | [doc](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Databáze EF Core v paměti | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) Microsoft | Pouze pro testování     | [doc](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | SQL API služby Azure Cosmos DB    | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) Microsoft |                      | [doc](xref:core/providers/cosmos/index)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Vývojový tým Npgsql](https://github.com/npgsql)                          |                      | [doc](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Projekt pomelo Foundation](https://github.com/PomeloFoundation)              |                      | [Tool](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Projekt pomelo Foundation](https://github.com/PomeloFoundation)              | Pouze předběžné verze      | [Tool](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2,5 a 3. x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [doc](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2,5 a 3. x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](http://dev.mysql.com) Oracle                                |                      | [doc](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11,2 a vyšší     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Předběžná verze           | [webu](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze Windows      | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze systému Linux        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | verze macOS        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Soubory aplikace Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [Tool](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | OpenEdge průběhu          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [Tool](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart. data. Oracle. EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 a vyšší  | [DevArt](https://www.devart.com/)                                             | Hrazen                 | [doc](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8,0 a vyšší     | [DevArt](https://www.devart.com/)                                             | Hrazen                 | [doc](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 onwards           | [DevArt](https://www.devart.com/)                                             | Hrazen                 | [doc](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 a vyšší            | [DevArt](https://www.devart.com/)                                             | Hrazen                 | [doc](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="adding-a-database-provider-to-your-application"></a>Přidání poskytovatele databáze do aplikace

Většina poskytovatelů databáze pro EF Core je distribuována jako balíčky NuGet a lze ji nainstalovat následujícím způsobem:

# <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package provider_package_name
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Po instalaci nakonfigurujete poskytovatele v `DbContext`, a to buď `OnConfiguring` v metodě, nebo v `AddDbContext` metodě, pokud používáte kontejner pro vkládání závislostí.
Například následující řádek konfiguruje poskytovatele SQL Server s předaným připojovacím řetězcem:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Poskytovatelé databáze mohou rozšířeně EF Core, aby povolily funkce jedinečné pro konkrétní databáze.
Některé koncepce jsou společné pro většinu databází a jsou součástí primárních EF Core součástí.
Tyto koncepty zahrnují vyjádření dotazů v LINQ, transakcích a sledování změn objektů, jakmile jsou načteny z databáze.
Některé koncepce jsou specifické pro konkrétního poskytovatele.
Poskytovatel SQL Server například umožňuje [Konfigurovat paměťově optimalizované tabulky](xref:core/providers/sql-server/memory-optimized-tables) (funkce specifické pro SQL Server).
Další koncepty jsou specifické pro třídu zprostředkovatelů.
Například poskytovatele EF Core pro relační databáze sestavují na společné `Microsoft.EntityFrameworkCore.Relational` knihovně, která poskytuje rozhraní API pro konfiguraci mapování tabulek a sloupců, omezení cizího klíče atd. Poskytovatelé jsou obvykle distribuováni jako balíčky NuGet.

> [!IMPORTANT]  
> Při vydání nové verze opravy EF Core obsahuje často aktualizace `Microsoft.EntityFrameworkCore.Relational` balíčku.
> Když přidáte poskytovatele relační databáze, tento balíček se bude přenositelným závislostí vaší aplikace.
> Ale mnoho poskytovatelů je vydaných nezávisle na EF Core a nemusí být aktualizované na základě novější verze opravy tohoto balíčku.
> Abyste se ujistili, že budete dostávat všechny opravy chyb, doporučujeme přidat opravu verze `Microsoft.EntityFrameworkCore.Relational` jako přímou závislost vaší aplikace.
