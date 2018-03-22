---
title: "Zprostředkovatelé databáze - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: d7313451f324a5e26ae327478996861e31364e7d
ms.sourcegitcommit: 89edee21606083d01154766e0c4249cec38957f7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="database-providers"></a>Zprostředkovatelé databáze

Entity Framework Core můžete přístup k mnoha různým databázím pomocí modulu plug-in knihoven nazývané poskytovatelé databáze.

## <a name="current-providers"></a>Aktuální zprostředkovatelů
> [!IMPORTANT]  
> EF hlavními zprostředkovateli jsou vytvořeny pomocí různých zdrojů. Ne všichni poskytovatelé se udržují v rámci [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore). Při výběru poskytovatele, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům. Ujistěte se také že zkontrolovat každého zprostředkovatele dokumentace pro verzi podrobné informace o kompatibilitě.

| Balíček NuGet                                                                                                        | Podporované databázové stroje | Funkce Maintainer nebo dodavatele                                                           | Poznámky k nebo požadavky             | Užitečné odkazy                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 onwards    | [EF základní projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                  | [Dokumentace](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | 3.7 SQLite a vyšší         | [EF základní projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                  | [Dokumentace](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Databáze v paměti jádra EF | [EF základní projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Určené jenom pro testování                 | [Dokumentace](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)      | PostgreSQL                 | [Npgsql vývojový tým](https://github.com/npgsql)                          |                                  | [Dokumentace](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              |                                  | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Server               | [Pomelo Foundation projektu](https://github.com/PomeloFoundation)              | Předběžné verze, až jádro EF 1.1   | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                   | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                   | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL projektu](http://dev.mysql.com) (Oracle)                                | Předběžné verze                      | [Dokumentace](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                                                |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 a 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | Základní EF 2.0 a vyšší, předběžné verze | [blog](https://www.tabsoverspaces.com/233653-preview-of-entity-framework-core-2-0-support-for-firebird-and-firebirdclient-6-0/)                                                                    |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 a 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | A vyšší EF základní 2.0              | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Verze systému Windows                  | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Linux verze                    | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | verze systému macOS                    | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 a vyšší     | [DevArt](https://www.devart.com/)                                             | Placené                             | [Dokumentace](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 a vyšší     | [DevArt](https://www.devart.com/)                                             | Placené                             | [Dokumentace](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 a vyšší           | [DevArt](https://www.devart.com/)                                             | Placené                             | [Dokumentace](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 a vyšší            | [DevArt](https://www.devart.com/)                                             | Placené                             | [Dokumentace](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Soubory aplikace Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | Základní EF 2.0, .NET Framework      | [readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |

## <a name="future-providers"></a>Budoucí zprostředkovatelů

### <a name="cosmos-db"></a>Cosmos DB

Byla jsme vývoj poskytovatele EF jádra pro rozhraní API DocumentDB v Cosmos DB. To bude první zprostředkovatel dokončení orientované dokumentu databáze, které jsme je tvořen a learnings z tohoto cvičení se chystáte informovat o tom, vylepšení v návrhu následné verze po 2.1. Stávající plán je publikovat časné zprostředkovatele do 2.1 časový rámec.

### <a name="oracle"></a>Oracle
Týmem Oracle .NET oznámila, že jsou plánování k uvolnění první strany poskytovatele pro EF základní 2.0 přibližně v třetí čtvrtletí 2018. V tématu jejich [příkaz směr pro .NET Core a Entity Framework Core](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) Další informace.
Prosím směrovat všechny dotazy týkající se tohoto zprostředkovatele, včetně na časové ose verze [web komunity Oracle](https://community.oracle.com/).

Mezitím se vytváří týmem EF [zprostředkovatel EF základní ukázka pro Oracle – databáze](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md). Účelem projektu není k vytvoření poskytovatele EF základní vlastnictví společnosti Microsoft, ale a pomoci tak identifikovat mezery v EF základní relační a základní funkce, která potřebujeme k vyřešení proto, aby lépe podporovaly Oracle a základní informace vývoj jiných Oracle zprostředkovatelé pro základní EF Oracle nebo třetí strany.

Jsme zvažovat příspěvky, které vylepšují Vzorová implementace. Také jsme by Vítejte a podporovat úsilí komunity pro vytvoření poskytovatele open-source Oracle pro základní EF, pomocí ukázku jako výchozí bod.

## <a name="adding-a-database-provider-to-your-application"></a>Přidání poskytovatele databáze do vaší aplikace

Většina zprostředkovatelů databáze pro základní EF distribuují jako balíčky NuGet. To znamená, že můžete nainstalovat pomocí `dotnet` nástroj na příkazovém řádku:

``` console
dotnet add package provider_package_name
```

Nebo v sadě Visual Studio pomocí konzoly Správce balíčků NuGet:

``` powershell
install-package provider_package_name
```

Po instalaci můžete nakonfigurovat poskytovatele v vaše `DbContext`, buď v `OnConfiguring` metoda nebo v `AddDbContext` metoda, pokud používáte kontejner vkládání závislostí. Například následující řádek konfiguruje zprostředkovatele SQL Server s předaný připojovací řetězec:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Zprostředkovatelé databáze můžete rozšířit EF jádra k povolení funkcí, které jsou jedinečné pro konkrétní databáze. Některé pojmy jsou společné pro většinu databází a jsou součástí primární EF základních komponent. Tyto koncepty zahrnují vyjadřující dotazy v technologii LINQ, transakce a sledování změn pro objekty, jakmile jsou načteny z databáze. Některé pojmy jsou specifické pro konkrétního poskytovatele. Například zprostředkovatele SQL Server umožňuje [konfigurace paměťově optimalizované tabulky](xref:core/providers/sql-server/memory-optimized-tables) (funkce specifické pro systém SQL Server). Další koncepty jsou specifické pro třídu zprostředkovatelů. Například EF hlavními zprostředkovateli pro relační databáze sestavení na nejběžnější `Microsoft.EntityFrameworkCore.Relational` knihovny, která poskytuje rozhraní API pro konfiguraci mapování tabulky a sloupce, omezení cizích klíčů, atd. Poskytovatelé jsou obvykle distribuován jako balíčky NuGet.

> [!IMPORTANT]  
> Po vydání nové verze oprava EF jádra, často obsahuje aktualizace `Microsoft.EntityFrameworkCore.Relational` balíčku. Když přidáte poskytovatele relační databáze, tento balíček stane přenositelné závislostí vaší aplikace. Ale mnoho poskytovatelů vydávají nezávisle z EF jádra a nemusí být aktualizován závislý na novější verze opravy tento balíček. Pokud chcete mít jistotu, zobrazí se všechny opravy chyb, je vhodné přidat oprav verze `Microsoft.EntityFrameworkCore.Relational` jako přímý závislost vaší aplikace.
