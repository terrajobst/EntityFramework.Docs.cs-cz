---
title: Poskytovatelé databází – EF Core
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 6f058698f78c787fc6c313486874b0af2183f97a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949312"
---
# <a name="database-providers"></a>Poskytovatelé databází

Entity Framework Core můžete přístup k mnoha různým databázím pomocí modulu plug-in knihoven, které se nazývají poskytovatelé databáze.

## <a name="current-providers"></a>Aktuální zprostředkovatelů
> [!IMPORTANT]  
> EF Core poskytovatelé jsou vytvořené pomocí široké škály zdrojů. Ne všichni poskytovatelé se zachovají v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore). Při zvažování zprostředkovatele, nezapomeňte vyhodnotit kvality, licencování, podpora, atd. a ujistěte se, že splňují vaše požadavky. Ujistěte se také, abyste že si přečetli dokumentaci každého poskytovatele podrobné informace o verzi kompatibility.

| Balíček NuGet                                                                                                        | Podporované databázové stroje | Funkce Maintainer / dodavatele                                                           | Poznámky a požadavky           | Užitečné odkazy                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:-------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 a novější    | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [dokumentace](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | 3.7 SQLite a vyšší         | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [dokumentace](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Databáze v paměti EF Core | [EF Core projektu](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Pouze pro testování               | [dokumentace](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Vývojový tým Npgsql](https://github.com/npgsql)                          |                                | [dokumentace](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              |                                | [Soubor Readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT serveru               | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              | Předběžné verze, až EF Core 1.1 | [Soubor Readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Lázecký](https://github.com/ErikEJ/)                             | .NET Framework                 | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Lázecký](https://github.com/ErikEJ/)                             | .NET Framework                 | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projekt MySQL](http://dev.mysql.com) (Oracle)                                | Předběžné verze                    | [dokumentace](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                                                |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 a 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | A vyšší EF Core 2.0            | [dokumentace](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 a 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | A vyšší EF Core 2.0            | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze Windows                | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze Linuxu                  | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | verze macOS                  | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 a vyšší     | [DevArt](https://www.devart.com/)                                             | Placené                           | [dokumentace](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 a vyšší     | [DevArt](https://www.devart.com/)                                             | Placené                           | [dokumentace](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 a vyšší           | [DevArt](https://www.devart.com/)                                             | Placené                           | [dokumentace](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 a vyšší            | [DevArt](https://www.devart.com/)                                             | Placené                           | [dokumentace](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Soubory aplikace Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | EF Core 2.0, rozhraní .NET Framework    | [Soubor Readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |

## <a name="future-providers"></a>Další zprostředkovatelé

### <a name="cosmos-db"></a>Cosmos DB

Jsme se vývojem poskytovatele EF Core pro rozhraní DocumentDB API ve službě Cosmos DB. Bude jím první poskytovatel kompletní dokumentově orientované databáze, kterou jsme vytvořili, a poznatky z tohoto cvičení se chystáte informovat vylepšení v návrhu následné verze po 2.1. Aktuální plán, je publikovat předběžnou verzi zprostředkovatele v 2.1 časový rámec.

### <a name="oracle"></a>Oracle
Tým Oracle .NET oznámil, že jsou plánování vydat vlastní zprostředkovatele pro EF Core 2.0 přibližně ve třetím čtvrtletí roku 2018. Zobrazit jejich [příkaz směr pro .NET Core a Entity Framework Core](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) Další informace.
Sdělte jakékoliv otázky týkající se tohoto zprostředkovatele, včetně na časové ose verze [web komunity Oracle](https://community.oracle.com/).

Mezitím vytvořil tým EF [EF Core zprostředkovateli ukázek pro databáze Oracle](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md). Účelem projektu není pro vytvoření poskytovatele EF Core vlastnictví společnosti Microsoft, ale nám identifikovat mezery v EF Core relační a základní funkce, které potřebujeme pro řešení pro zajištění lepší podpory pro Oracle a rychle zprovozněte vývoj dalších Oracle zprostředkovatelé pro jádro EF Core Oracle nebo třetími stranami.

Posoudíme příspěvky, které zlepšují ukázková implementace. Také by Vítáme a podporu komunity snahy o vytvoření poskytovatele open-source systému Oracle pro jádro EF Core pomocí ukázky jako výchozí bod.

## <a name="adding-a-database-provider-to-your-application"></a>Přidání poskytovatele databáze do vaší aplikace

Většina poskytovatelů databázi pro EF Core se distribuují jako balíčky NuGet. To znamená, že můžete nainstalovat s použitím `dotnet` nástroj na příkazovém řádku:

``` console
dotnet add package provider_package_name
```

Nebo v sadě Visual Studio pomocí konzoly Správce balíčků NuGet:

``` powershell
install-package provider_package_name
```

Po instalaci, které budete konfigurovat poskytovatele v vaše `DbContext`, buď v `OnConfiguring` metoda nebo v `AddDbContext` metody, pokud používáte kontejner vkládání závislostí. Například následující řádek nakonfiguruje zprostředkovatele SQL Server předaný připojovacím řetězcem:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Poskytovatelé databází můžete rozšířit EF Core povolit funkce, které jsou jedinečné pro konkrétní databáze. Některé koncepty jsou společné pro většinu databází a jsou zahrnuty v primární součásti EF Core. Tyto koncepty zahrnují vyjádření dotazy v LINQ, transakce a sledování změn na objekty, jakmile jsou načteny z databáze. Některé koncepty jsou specifické pro konkrétního poskytovatele. Například zprostředkovatele SQL Server umožňuje [konfigurace paměťově optimalizovaných tabulek](xref:core/providers/sql-server/memory-optimized-tables) (funkce specifické pro systém SQL Server). Další koncepty jsou specifické pro třídu zprostředkovatelů. Například sestavit EF Core zprostředkovatelé pro relační databáze na společné `Microsoft.EntityFrameworkCore.Relational` knihovny, která poskytuje rozhraní API pro konfiguraci mapování tabulky a sloupce, omezení cizího klíče, atd. Poskytovatelé se obvykle distribuují jako balíčky NuGet.

> [!IMPORTANT]  
> Po vydání nové verze opravy EF Core, často obsahuje aktualizace `Microsoft.EntityFrameworkCore.Relational` balíčku. Při přidání poskytovatele relační databáze, bude tento balíček tranzitivní závislostí vaší aplikace. Ale mnoho poskytovatelů vydávají nezávisle z EF Core a nejspíš nepůjde aktualizovat na závisí na novější verzi tohoto balíčku opravy. Pokud chcete mít jistotu, zobrazí se všechny opravy chyb, se doporučuje přidat oprav verze `Microsoft.EntityFrameworkCore.Relational` jako přímou závislost aplikace.
