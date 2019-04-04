---
title: Poskytovatelé databází – EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: 3748496db89c110d55a0876727e33e1f3ec987d9
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914088"
---
# <a name="database-providers"></a>Poskytovatelé databází

Entity Framework Core můžete přístup k mnoha různým databázím pomocí modulu plug-in knihoven, které se nazývají poskytovatelé databáze.

## <a name="current-providers"></a>Aktuální zprostředkovatelů
> [!IMPORTANT]  
> EF Core poskytovatelé jsou vytvořené pomocí široké škály zdrojů. Ne všichni poskytovatelé se zachovají v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore). Při zvažování zprostředkovatele, nezapomeňte vyhodnotit kvality, licencování, podpora, atd. a ujistěte se, že splňují vaše požadavky. Ujistěte se také, abyste že si přečetli dokumentaci každého poskytovatele podrobné informace o verzi kompatibility.

| Balíček NuGet                                                                                                        | Podporované databázové stroje | Funkce Maintainer / dodavatele                                                           | Poznámky a požadavky | Užitečné odkazy                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 onwards    | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [Dokumentace](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | 3.7 SQLite a vyšší         | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [Dokumentace](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Databáze v paměti EF Core | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Pouze pro testování     | [Dokumentace](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | SQL API služby Azure Cosmos DB    | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Pouze ve verzi Preview         | [blog](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Vývojový tým Npgsql](https://github.com/npgsql)                          |                      | [Dokumentace](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              |                      | [Soubor Readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              | Pouze předběžné verze      | [Soubor Readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 a 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [Dokumentace](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 a 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](http://dev.mysql.com) (Oracle)                                |                      | [Dokumentace](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 a vyšší     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Předběžná verze           | [Web](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze Windows      | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze Linuxu        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | verze macOS        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access files     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [Soubor Readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Průběh OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [Soubor Readme](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 a vyšší  | [DevArt](https://www.devart.com/)                                             | Placené                 | [Dokumentace](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 a vyšší     | [DevArt](https://www.devart.com/)                                             | Placené                 | [Dokumentace](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 onwards           | [DevArt](https://www.devart.com/)                                             | Placené                 | [Dokumentace](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 a vyšší            | [DevArt](https://www.devart.com/)                                             | Placené                 | [Dokumentace](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="future-providers"></a>Další zprostředkovatelé

### <a name="cosmos-db"></a>Databáze Cosmos

Jsme se vývojem poskytovatele EF Core pro rozhraní SQL API ve službě Cosmos DB.
To bude první poskytovatel kompletní dokumentově orientované databáze, kterou jsme vytvořili, a poznatky z tohoto cvičení se chystáte informovat vylepšení v návrhu budoucí verze EF Core a případně dalších nerelačních poskytovatelů.
Ve verzi preview je k dispozici na [Galerie NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos).

### <a name="oracle-first-party-provider"></a>Vlastní zprostředkovatel Oracle
Tým Oracle .NET publikoval beta verze [Zprostředkovatel Oracle pro jádro EF Core](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/).
Sdělte jakékoliv otázky týkající se tohoto zprostředkovatele, včetně na časové ose verze [web komunity Oracle](https://community.oracle.com/).

## <a name="adding-a-database-provider-to-your-application"></a>Přidání poskytovatele databáze do vaší aplikace

Většina poskytovatelů databázi pro EF Core se distribuují jako balíčky NuGet. To znamená, že můžete nainstalovat s použitím `dotnet` nástroj na příkazovém řádku:

``` console
dotnet add package provider_package_name
```

Nebo v sadě Visual Studio pomocí konzoly Správce balíčků NuGet:

``` powershell
install-package provider_package_name
```

Po instalaci, které budete konfigurovat poskytovatele v vaše `DbContext`, buď v `OnConfiguring` metoda nebo v `AddDbContext` metody, pokud používáte kontejner vkládání závislostí.
Například následující řádek nakonfiguruje zprostředkovatele SQL Server předaný připojovacím řetězcem:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Poskytovatelé databází můžete rozšířit EF Core povolit funkce, které jsou jedinečné pro konkrétní databáze.
Některé koncepty jsou společné pro většinu databází a jsou zahrnuty v primární součásti EF Core.
Tyto koncepty zahrnují vyjádření dotazy v LINQ, transakce a sledování změn na objekty, jakmile jsou načteny z databáze.
Některé koncepty jsou specifické pro konkrétního poskytovatele.
Například zprostředkovatele SQL Server umožňuje [konfigurace paměťově optimalizovaných tabulek](xref:core/providers/sql-server/memory-optimized-tables) (funkce specifické pro systém SQL Server).
Další koncepty jsou specifické pro třídu zprostředkovatelů.
Například sestavit EF Core zprostředkovatelé pro relační databáze na společné `Microsoft.EntityFrameworkCore.Relational` knihovny, která poskytuje rozhraní API pro konfiguraci mapování tabulky a sloupce, omezení cizího klíče, atd. Poskytovatelé se obvykle distribuují jako balíčky NuGet.

> [!IMPORTANT]  
> Po vydání nové verze opravy EF Core, často obsahuje aktualizace `Microsoft.EntityFrameworkCore.Relational` balíčku.
> Při přidání poskytovatele relační databáze, bude tento balíček tranzitivní závislostí vaší aplikace.
> Ale mnoho poskytovatelů vydávají nezávisle z EF Core a nejspíš nepůjde aktualizovat na závisí na novější verzi tohoto balíčku opravy.
> Pokud chcete mít jistotu, zobrazí se všechny opravy chyb, se doporučuje přidat oprav verze `Microsoft.EntityFrameworkCore.Relational` jako přímou závislost aplikace.
