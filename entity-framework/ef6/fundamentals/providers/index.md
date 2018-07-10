---
title: Entity Framework poskytovatelé - EF6
author: divega
ms.date: 2018-06-27
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
caps.latest.revision: 3
ms.openlocfilehash: 8bd5a5a420d741accd1167845575e23c09579ae1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914294"
---
# <a name="entity-framework-6-providers"></a>Zprostředkovatelé Entity Framework 6
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Rozhraní Entity Framework je nyní vyvíjených v licenci open source a EF6 a nad nebude odeslaná jako součást rozhraní .NET Framework. To má mnoho výhod, ale také vyžaduje, že EF poskytovatelé znovu sestavit na EF6 sestavení. To znamená, že EF poskytovatelů pro EF5 a pod nebudou fungovat s EF6, dokud se po opětovném sestavení.

## <a name="which-providers-are-available-for-ef6"></a>Poskytovatelů, kteří jsou k dispozici pro EF6?

Víme o poskytovatele, který po opětovném sestavení pro EF6 patří:

*   **Zprostředkovatel Microsoft SQL Server**
    *   Od [Entity Framework otevřete základu zdrojového kódu](http://github.com/aspnet/EntityFramework6)
    *   Dodán jako součást [balíček NuGet objektu EntityFramework](http://nuget.org/packages/EntityFramework)
*   **Zprostředkovatel Microsoft SQL Server Compact Edition**
    *   Od [Entity Framework otevřete základu zdrojového kódu](http://github.com/aspnet/EntityFramework6)
    *   Součástí [balíček EntityFramework.SqlServerCompact NuGet](http://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Devart dotConnect zprostředkovatelů dat.**](http://www.devart.com/dotconnect/)
    *   Existují poskytovatele třetí strany z [Devart](http://www.devart.com/) pro širokou škálu databází včetně Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 a SQL Server
*   [**Poskytovatelé softwaru CData**](http://www.cdata.com/ado/)
    *   Existují poskytovatele třetí strany z [CData softwaru](http://www.cdata.com/ado/) pro širokou škálu úložišť dat, včetně Salesforce, Azure Table Storage, MySql a mnoho dalších
*   **Firebird poskytovatele**
    *   K dispozici jako [balíček NuGet](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)
*   **Vizuální Fox Pro zprostředkovatele**
    *   K dispozici jako [balíček NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/)
*   **PostgreSQL**
    *   Je k dispozici jako Npgsql [balíček NuGet](http://www.nuget.org/packages/Npgsql.EF6/)
*   **Oracle**
    *   Je k dispozici jako ODP.NET [balíček NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Všimněte si, že zařazení v tomto seznamu nenaznačuje, že úroveň funkce nebo podporu pro daného zprostředkovatele, pouze to, že sestavení pro EF6 byl zpřístupněn.

## <a name="registering-ef-providers"></a>Registrace zprostředkovatele EF

Od zprostředkovatele Entity Framework 6 EF lze registrovat pomocí konfigurace buď založený na kódu nebo v konfiguračním souboru aplikace.

### <a name="config-file-registration"></a>Registrace konfiguračního souboru

Registrace poskytovatele EF v souboru app.config nebo web.config má následující formát:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Všimněte si, že často poskytovateli EF při instalaci z NuGet, pak balíček NuGet automaticky přidá tuto registraci do konfiguračního souboru. Pokud balíček NuGet nainstalovat do projektu, který není spouštěný projekt pro vaši aplikaci, budete muset zkopírujte registrace do konfiguračního souboru pro spouštěný projekt.

"InvariantName" v tomto registraci je stejný neutrální název používaný k identifikaci poskytovatele ADO.NET. To lze nalézt jako atribut "Výchozí" v registraci DbProviderFactories a jako atribut "providerName" v registraci řetězec připojení. Výchozí název položky používat také být součástí dokumentace pro zprostředkovatele. Příklady invariantní názvů, které jsou "System.Data.SqlClient" pro SQL Server a "System.Data.SqlServerCe.4.0" pro SQL Server Compact.

"Type" v tomto registraci je název kvalifikovaný pro sestavení typu poskytovatele, který je odvozen od "System.Data.Entity.Core.Common.DbProviderServices". Řetězec, který má použít pro SQL Compact je například "System.Data.Entity.SqlServerCompact.SqlCeProviderServices EntityFramework.SqlServerCompact". Typ použití zde by měl být součástí dokumentace pro zprostředkovatele.

### <a name="code-based-registration"></a>Registrace na úrovni kódu

Od verze Entity Framework 6 celou aplikaci konfigurace pro EF můžete zadat v kódu. Úplné podrobnosti najdete v tématu  _[Entity Framework Code-Based konfigurace](https://msdn.microsoft.com/en-us/data/jj680699)_. Obvyklým způsobem zaregistrovat poskytovatele EF pomocí konfigurace založená na kódu je vytvořte novou třídu, která je odvozena z System.Data.Entity.DbConfiguration a jeho následné uložení do stejného sestavení jako vaší třídy DbContext. Vaše třída DbConfiguration by pak zaregistrujte poskytovatele 've svém konstruktoru. Například k registraci SQL Compact poskytovatele DbConfiguration třídy vypadá takto:

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

V tomto kódu "SqlCeProviderServices.ProviderInvariantName" je v zájmu usnadnění pro SQL Server Compact výchozí název zprostředkovatele řetězec ("System.Data.SqlServerCe.4.0") a SqlCeProviderServices.Instance vrátí instanci typu singleton SQL Compact EF zprostředkovatele.

## <a name="what-if-the-provider-i-need-isnt-available"></a>Co když se poskytovatel budu potřebovat není k dispozici?

Pokud se k dispozici v předchozích verzích sady EF, pak doporučujeme obraťte se na vlastníka poskytovatele a požádejte ho, chcete-li vytvořit ve verzi EF6. Měli byste zahrnout odkaz na [dokumentaci podle modelu poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Můžete mi psát zprostředkovatele vlastní?

Je určitě možné vytvořit poskytovatele EF, sami sebe i když by neměly být zahrnuté triviální podniku. Výše uvedený odkaz o podle modelu poskytovatele EF6 je dobrým začátkem. Vám může být také vhodné k použití tohoto kódu pro zprostředkovatele SQL Server a SQL CE součástí [EF opensourcových codebase](https://github.com/aspnet/EntityFramework6) jako výchozí bod nebo odkaz.

Všimněte si, že počínaje EF6 poskytovateli EF je menší těsně spjat s podkladového zprostředkovatele ADO.NET. Díky tomu je snazší psát poskytovatele EF aniž byste museli napsat nebo zabalovat třídy rozhraní ADO.NET.
