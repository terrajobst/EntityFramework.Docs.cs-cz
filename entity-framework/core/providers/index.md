---
title: Poskytovatelé databáze – EF Core
author: ajcvickers
ms.date: 12/17/2019
uid: core/providers/index
ms.openlocfilehash: daf2e06c76ed55213243f5728548fdfd4be0e5e2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417825"
---
# <a name="database-providers"></a>Poskytovatelé databází

Entity Framework Core mají přístup k mnoha různým databázím prostřednictvím knihoven modulů plug-in nazývaných poskytovatelé databáze.

## <a name="current-providers"></a>Aktuální zprostředkovatelé

> [!IMPORTANT]  
> Poskytovatelé EF Core jsou postavené na nejrůznějších zdrojích. Ne všichni poskytovatelé se uchovávají jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Při zvažování poskytovatele nezapomeňte vyhodnotit kvalitu, licencování, podporu atd., abyste zajistili, že splňují vaše požadavky. Také se ujistěte, že v dokumentaci pro každého poskytovatele najdete podrobné informace o kompatibilitě verzí.

> [!IMPORTANT]  
> Poskytovatelé EF Core obvykle pracují napříč podverzemi, ale ne napříč hlavními verzemi. Například poskytovatel vydaný pro EF Core 2,1 by měl pracovat s EF Core 2,2, ale nebude fungovat s EF Core 3,0. 

| Balíček NuGet                                                                                                        | Podporované databázové stroje | Údržba/dodavatel                                                           | Poznámky/požadavky | Sestaveno pro verzi | Užitečné odkazy                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 a vyšší    | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [doc](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3,7 a vyšší         | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [doc](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Databáze EF Core v paměti | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | [Omezení](xref:core/miscellaneous/testing/in-memory)                 | 3.1               | [doc](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Rozhraní API služby Azure Cosmos DB pro SQL    | [EF Core projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [doc](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql. EntityFrameworkCore. PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Vývojový tým Npgsql](https://github.com/npgsql)                          |                      | 3.1               | [doc](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo. EntityFrameworkCore. MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Projekt pomelo Foundation](https://github.com/PomeloFoundation)              |                      | 3.1               | [Tool](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Devart. data. MySql. EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 a vyšší            | [DevArt](https://www.devart.com/)                                             | Placené                 | 3,0               | [doc](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [Devart. data. Oracle. EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 a vyšší  | [DevArt](https://www.devart.com/)                                             | Placené                 | 3,0               | [doc](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart. data. PostgreSql. EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8,0 a vyšší     | [DevArt](https://www.devart.com/)                                             | Placené                 | 3,0               | [doc](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart. data. SQLite. EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 onwards           | [DevArt](https://www.devart.com/)                                             | Placené                 | 3,0               | [doc](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [FileContextCore](https://www.nuget.org/packages/FileContextCore/)                                                   | Ukládá data do souborů.       | [Morris Janatzek](https://github.com/morrisjdev)                              | Pro účely vývoje | 3,0               | [Tool](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |
| [EntityFrameworkCore. Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Soubory aplikace Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | 2.2               | [Tool](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2.2               | [komunity](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2.2               | [komunity](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2,5 a 3. x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | 2.2               | [doc](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [Teradata. EntityFrameworkCore](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                         | Databáze Teradata 16,10 a vyšší | [Teradata](https://downloads.teradata.com/download/connectivity/net-data-provider-for-teradata) | | 2.2               |[webu](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                                                            |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2,5 a 3. x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | 2.1               | [komunity](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | OpenEdge průběhu          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | 2.1               | [Tool](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [MySql. data. EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](https://dev.mysql.com) (Oracle)                               |                      | 2.1               | [doc](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle. EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11,2 a vyšší     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   |                      | 2.1               | [webu](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze systému Windows      | 2.0               | [webový](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze systému Linux        | 2.0               | [webový](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore – OSX](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | verze macOS        | 2.0               | [webový](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Projekt pomelo Foundation](https://github.com/PomeloFoundation)              | Pouze předběžné verze      | 1.1               | [Tool](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |

## <a name="adding-a-database-provider-to-your-application"></a>Přidání poskytovatele databáze do aplikace

Většina poskytovatelů databáze pro EF Core je distribuována jako balíčky NuGet a lze ji nainstalovat následujícím způsobem:

## <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Po instalaci zprostředkovatele nakonfigurujete ve svém `DbContext`, a to buď v metodě `OnConfiguring`, nebo v metodě `AddDbContext`, pokud používáte kontejner pro vkládání závislostí.
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
Například poskytovatele EF Core pro relační databáze sestavují na Common `Microsoft.EntityFrameworkCore.Relational` Library, která poskytuje rozhraní API pro konfiguraci mapování tabulek a sloupců, omezení cizího klíče atd. Poskytovatelé jsou obvykle distribuováni jako balíčky NuGet.

> [!IMPORTANT]  
> Když je vydána nová verze opravy EF Core, často zahrnuje aktualizace balíčku `Microsoft.EntityFrameworkCore.Relational`.
> Když přidáte poskytovatele relační databáze, tento balíček se bude přenositelným závislostí vaší aplikace.
> Ale mnoho poskytovatelů je vydaných nezávisle na EF Core a nemusí být aktualizované na základě novější verze opravy tohoto balíčku.
> Abyste se ujistili, že budete dostávat všechny opravy chyb, doporučujeme, abyste přidali verzi opravy `Microsoft.EntityFrameworkCore.Relational` jako přímou závislost vaší aplikace.
