---
title: Zprostředkovatelé databáze – EF Core
author: ajcvickers
ms.date: 12/17/2019
uid: core/providers/index
ms.openlocfilehash: daf2e06c76ed55213243f5728548fdfd4be0e5e2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417825"
---
# <a name="database-providers"></a>Poskytovatelé databází

Entity Framework Core můžete přistupovat k mnoha různých databází prostřednictvím knihoven plug-in s názvem zprostředkovatelé databáze.

## <a name="current-providers"></a>Současní poskytovatelé

> [!IMPORTANT]  
> Zprostředkovatelé EF Core jsou sestaveni z různých zdrojů. Ne všichni zprostředkovatelé jsou udržovány jako součást [hlavního projektu entity framework .](https://github.com/aspnet/EntityFrameworkCore) Při zvažování poskytovatele, ujistěte se, že vyhodnotit kvalitu, licencování, podpora, atd., aby zajistily, že splňují vaše požadavky. Také zkontrolujte dokumentaci každého poskytovatele pro podrobné informace o kompatibilitě verzí.

> [!IMPORTANT]  
> Zprostředkovatelé EF Core obvykle pracují napříč dílčími verzemi, ale ne napříč hlavními verzemi. Například zprostředkovatel propuštěn pro EF Core 2.1 by měl pracovat s EF Core 2.2, ale nebude pracovat s EF Core 3.0. 

| Balíček NuGet                                                                                                        | Podporované databázové moduly | Správce / dodavatel                                                           | Poznámky / Požadavky | Vytvořeno pro verzi | Užitečné odkazy                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 a dále    | [Základní projekt EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokumenty](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 a dále         | [Základní projekt EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokumenty](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.entityFrameworkCore.inmemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core v paměti databáze | [Základní projekt EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | [Omezení](xref:core/miscellaneous/testing/in-memory)                 | 3.1               | [Dokumenty](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Rozhraní API služby Azure Cosmos DB pro SQL    | [Základní projekt EF](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokumenty](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Vývojový tým Npgsql](https://github.com/npgsql)                          |                      | 3.1               | [Dokumenty](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Projekt Nadace Pomelo](https://github.com/PomeloFoundation)              |                      | 3.1               | [Soubor readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 a dále            | [DevArt](https://www.devart.com/)                                             | Zaplaceno                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 a dále  | [DevArt](https://www.devart.com/)                                             | Zaplaceno                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 a dále     | [DevArt](https://www.devart.com/)                                             | Zaplaceno                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 a dále           | [DevArt](https://www.devart.com/)                                             | Zaplaceno                 | 3.0               | [Dokumenty](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [FileContextCore](https://www.nuget.org/packages/FileContextCore/)                                                   | Ukládá data do souborů       | [Morris Janatzek](https://github.com/morrisjdev)                              | Pro účely vývoje | 3.0               | [Soubor readme](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Soubory aplikace Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | 2,2               | [Soubor readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2,2               | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.sqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2,2               | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2,5 a 3,x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | 2,2               | [Dokumenty](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [Teradata.EntityFrameworkCore](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                         | Teradata Databáze 16.10 a dále | [Teradata](https://downloads.teradata.com/download/connectivity/net-data-provider-for-teradata) | | 2,2               |[Webové stránky](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                                                            |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2,5 a 3,x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | 2.1               | [Wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | 2.1               | [Soubor readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](https://dev.mysql.com) (Oracle)                               |                      | 2.1               | [Dokumenty](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 a dále     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   |                      | 2.1               | [Webové stránky](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [Ibm. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze systému Windows      | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Ibm. EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Linuxová verze        | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Ibm. EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | verze macOS        | 2.0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Projekt Nadace Pomelo](https://github.com/PomeloFoundation)              | Pouze předběžná verze      | 1.1               | [Soubor readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |

## <a name="adding-a-database-provider-to-your-application"></a>Přidání zprostředkovatele databáze do aplikace

Většina zprostředkovatelů databáze pro EF Core jsou distribuovány jako balíčky NuGet a lze nainstalovat následujícím způsobem:

## <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Po instalaci nakonfigurujete zprostředkovatele ve vašem `DbContext`, buď v metodě, `OnConfiguring` nebo v metodě, `AddDbContext` pokud používáte kontejner vkládání závislostí.
Například následující řádek konfiguruje zprostředkovatele serveru SQL Server s předaným připojovacím řetězcem:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Zprostředkovatelé databáze můžete rozšířit EF Core povolit funkce jedinečné pro konkrétní databáze.
Některé koncepty jsou společné pro většinu databází a jsou zahrnuty v primární součásti EF Core.
Tyto koncepty zahrnují vyjádření dotazů v LINQ, transakce a sledování změn objektů po jejich načtení z databáze.
Některé koncepty jsou specifické pro konkrétního zprostředkovatele.
Například zprostředkovatel sql serveru umožňuje [konfigurovat tabulky optimalizované pro paměť](xref:core/providers/sql-server/memory-optimized-tables) (funkce specifické pro SQL Server).
Jiné koncepty jsou specifické pro třídu poskytovatelů.
Například zprostředkovatelé EF Core pro relační `Microsoft.EntityFrameworkCore.Relational` databáze sestaví na společné knihovně, která poskytuje řešení API pro konfiguraci mapování tabulek a sloupců, omezení cizího klíče atd. Zprostředkovatelé jsou obvykle distribuovány jako balíčky NuGet.

> [!IMPORTANT]  
> Když je vydána nová verze opravy EF Core, `Microsoft.EntityFrameworkCore.Relational` často obsahuje aktualizace balíčku.
> Když přidáte zprostředkovatele relační databáze, tento balíček se stane přenositelnou závislostí vaší aplikace.
> Ale mnoho poskytovatelů jsou uvolněny nezávisle na EF Core a nemusí být aktualizovány tak, aby závisely na novější verzi opravy tohoto balíčku.
> Chcete-li se ujistit, že získáte všechny opravy chyb, doporučujeme přidat verzi opravy `Microsoft.EntityFrameworkCore.Relational` jako přímou závislost vaší aplikace.
