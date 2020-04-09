---
title: Poskytovatelé entity frameworku - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419363"
---
# <a name="entity-framework-6-providers"></a>Zprostředkovatelé rozhraní entity 6
> [!NOTE]
> **EF6 Pouze dále** – funkce, rozhraní API atd. Pokud používáte starší verzi, některé nebo všechny informace se nevztahují.

Entity Framework je nyní vyvíjen pod open source licence a EF6 a výše nebudou dodávány jako součást rozhraní .NET Framework. To má mnoho výhod, ale také vyžaduje, aby zprostředkovatelé EF znovu sestavit proti sestavení EF6. To znamená, že zprostředkovatelé EF pro EF5 a nižší nebudou pracovat s EF6, dokud nebudou znovu sestaveny.

## <a name="which-providers-are-available-for-ef6"></a>Kteří zprostředkovatelé jsou k dispozici pro EF6?

Poskytovatelé jsme si vědomi, že byly přestavěny pro EF6 patří:

*   **Zprostředkovatel serveru Microsoft SQL Server**
    *   Vytvořeno z [open source kódu entity Framework](https://github.com/aspnet/EntityFramework6)
    *   Dodáno jako součást [balíčku EntityFramework NuGet](https://nuget.org/packages/EntityFramework)
*   **Zprostředkovatel microsoft SQL Server Compact Edition**
    *   Vytvořeno z [open source kódu entity Framework](https://github.com/aspnet/EntityFramework6)
    *   Dodáno v [balíčku EntityFramework.SqlServerCompact NuGet](https://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Zprostředkovatelé dat Devart dotConnect**](https://www.devart.com/dotconnect/)
    *   Existují externí poskytovatelé od [Společnosti Devart](https://www.devart.com/) pro různé databáze včetně Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 a SQL Server
*   [**Poskytovatelé softwaru CData**](https://www.cdata.com/ado/)
    *   Existují externí poskytovatelé z [CData Software](https://www.cdata.com/ado/) pro různá úložiště dat, včetně Salesforce, Azure Table Storage, MySql a mnoho dalších
*   **Firebird poskytovatel**
    *   K dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)
*   **Poskytovatel visual fox pro**
    *   K dispozici jako [balíček NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [MySQL Konektor/NET pro entity frameworku](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)
*   **Oracle**
    *   ODP.NET je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Všimněte si, že zahrnutí v tomto seznamu neznamená úroveň funkčnosti nebo podpory pro daného zprostředkovatele, pouze, že sestavení pro EF6 byla k dispozici.

## <a name="registering-ef-providers"></a>Registrace zprostředkovatelů EF

Počínaje entity Framework 6 zprostředkovatelé EF lze zaregistrovat pomocí konfigurace založené na kódu nebo v konfiguračním souboru aplikace.

### <a name="config-file-registration"></a>Registrace konfiguračního souboru

Registrace zprostředkovatele EF v souboru app.config nebo web.config má následující formát:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Všimněte si, že často pokud je poskytovatel EF nainstalován z NuGet, pak balíček NuGet automaticky přidá tuto registraci do konfiguračního souboru. Pokud nainstalujete balíček NuGet do projektu, který není projekt spuštění pro vaši aplikaci, pak možná budete muset zkopírovat registraci do konfiguračního souboru pro projekt spuštění.

"InvariantName" v této registraci je stejný invariantní název používaný k identifikaci zprostředkovatele ADO.NET. To lze nalézt jako atribut "invariant" v registraci DbProviderFactories a jako atribut "providerName" v registraci připojovacího řetězce. Název invariant, který chcete použít, by měl být také zahrnut v dokumentaci pro zprostředkovatele. Příklady invariantních názvů jsou "System.Data.SqlClient" pro SQL Server a "System.Data.SqlServerCe.4.0" pro SQL Server Compact.

"Typ" v této registraci je název typu zprostředkovatele s kvalifikací sestavení, který je odvozen z "System.Data.Entity.Core.Common.DbProviderServices". Například řetězec, který se má použít pro SQL Compact, je "System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact". Typ, který se má použít zde by měly být zahrnuty v dokumentaci pro zprostředkovatele.

### <a name="code-based-registration"></a>Registrace založená na kódu

Počínaje entity Framework 6 konfigurace pro celou aplikaci EF lze zadat v kódu. Podrobné informace naleznete _[v tématu Konfigurace založená na kódu entity frameworku](https://msdn.microsoft.com/data/jj680699)_. Normální způsob registrace zprostředkovatele EF pomocí konfigurace založené na kódu je vytvořit novou třídu, která je odvozena od System.Data.Entity.DbConfiguration a umístit ji do stejného sestavení jako vaše dbcontext třídy. Vaše DbConfiguration třída by pak zaregistrovat zprostředkovatele v jeho konstruktoru. Chcete-li například zaregistrovat zprostředkovatele SQL Compact, třída DbConfiguration vypadá takto:

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

V tomto kódu "SqlCeProviderServices.ProviderInvariantName" je pohodlí pro SQL Server Compact kompaktní název řetězce ("System.Data.SqlServerCe.4.0") a SqlCeProviderServices.Instance vrátí instanci singleton poskytovatele SQL Compact.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Co když poskytovatel, který potřebuji, není k dispozici?

Pokud je poskytovatel k dispozici pro předchozí verze EF, doporučujeme kontaktovat vlastníka zprostředkovatele a požádat je o vytvoření verze EF6. Měli byste zahrnout odkaz na [dokumentaci pro model zprostředkovatele EF6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Mohu napsat poskytovatel sám?

Je jistě možné vytvořit poskytovatele EF sami, i když by neměl být považován za triviální podnik. Výše uvedený odkaz o modelu zprostředkovatele EF6 je dobrým místem pro začátek. Může také být užitečné použít kód pro SQL Server a SQL CE zprostředkovatele zahrnuté v [EF open source codebase](https://github.com/aspnet/EntityFramework6) jako výchozí bod nebo pro referenci.

Všimněte si, že počínaje EF6 zprostředkovatele ef je méně úzce spojena s poskytovatelem základní ADO.NET. To usnadňuje zápis zprostředkovatele EF bez nutnosti zápisu nebo zabalení ADO.NET třídy.
