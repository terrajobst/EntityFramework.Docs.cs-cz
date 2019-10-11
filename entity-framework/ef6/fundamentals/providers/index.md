---
title: Entity Framework Zprostředkovatelé – EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: bf07296503e4bb5d1e13f5f6f29e7118cbbde61d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181687"
---
# <a name="entity-framework-6-providers"></a>Poskytovatelé Entity Framework 6
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Entity Framework se teď vyvíjí v rámci open-source licence a EF6 a výše se nedodává jako součást .NET Framework. Má spoustu výhod, ale také vyžaduje, aby se poskytovatelé EF znovu vytvořili proti EF6 sestavením. To znamená, že zprostředkovatelé EF pro EF5 a níže nebudou pracovat s EF6, dokud nebudou znovu sestaveny.

## <a name="which-providers-are-available-for-ef6"></a>Kteří poskytovatelé jsou k dispozici pro EF6?

Poskytovatelé, o kterých jsme se znovu vytvořili pro EF6, obsahují tyto informace:

*   **Poskytovatel Microsoft SQL Server**
    *   Sestaveno z [Entity Framework základ kódu Open Source](https://github.com/aspnet/EntityFramework6)
    *   Dodává se jako součást [balíčku NuGet EntityFramework](https://nuget.org/packages/EntityFramework) .
*   **Poskytovatel edice Microsoft SQL Server Compact**
    *   Sestaveno z [Entity Framework základ kódu Open Source](https://github.com/aspnet/EntityFramework6)
    *   Dodává se v [balíčku NuGet EntityFramework. SqlServerCompact.](https://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Devart dotConnect – poskytovatelé dat**](https://www.devart.com/dotconnect/)
    *   Existují poskytovatelé třetích stran od [Devart](https://www.devart.com/) pro nejrůznější databáze, mezi které patří Oracle, MySQL, PostgreSQL, SQLite, SALESFORCE, DB2 a SQL Server
*   [**CData poskytovatelé softwaru**](https://www.cdata.com/ado/)
    *   Existují poskytovatelé třetích stran od [CDATA softwaru](https://www.cdata.com/ado/) pro nejrůznější úložiště dat, včetně Salesforce, Azure Table Storage, MySQL a spousty dalších.
*   **Poskytovatel Firebird**
    *   K dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)
*   **Poskytovatel Visual Fox pro**
    *   K dispozici jako [balíček NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [Konektor MySQL/síť pro Entity Framework](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   **PostgreSQL**
    *   Npgsql je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/) .
*   **Oracle**
    *   ODP.NET je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) .

Všimněte si, že zahrnutí do tohoto seznamu neindikuje úroveň funkčnosti nebo podpory pro daného zprostředkovatele, ale k dispozici je pouze sestavení pro EF6.

## <a name="registering-ef-providers"></a>Registrace zprostředkovatelů EF

Počínaje Entity Framework 6 je možné zaregistrovat poskytovatele EF buď pomocí konfigurace založené na kódu, nebo v konfiguračním souboru aplikace.

### <a name="config-file-registration"></a>Registrace konfiguračního souboru

Registrace poskytovatele EF v App. config nebo Web. config má následující formát:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Počítejte s tím, že pokud je zprostředkovatel EF nainstalovaný z NuGet, pak balíček NuGet tuto registraci automaticky přidá do konfiguračního souboru. Pokud nainstalujete balíček NuGet do projektu, který není spouštěným projektem aplikace, může být nutné zkopírovat registraci do konfiguračního souboru pro svůj projekt po spuštění.

V této registraci je "název" "invariantní" stejný neutrální název, který slouží k identifikaci poskytovatele ADO.NET. To lze najít jako "neutrální" atribut v registraci DbProviderFactories a jako atribut "providerName" v registraci připojovacího řetězce. Invariantní název, který se má použít, by měl být také zahrnut v dokumentaci pro poskytovatele. Příklady neutrálních názvů jsou "System. data. SqlClient" pro SQL Server a "System. data. SqlServerCe. 4.0" pro SQL Server Compact.

"Type" v této registraci je kvalifikovaný název sestavení typu zprostředkovatele, který je odvozený od "System. data. entity. Core. Common. DbProviderServices". Například řetězec, který se má použít pro SQL Compact, je "System. data. entity. SqlServerCompact. SqlCeProviderServices, EntityFramework. SqlServerCompact". Typ, který se má použít tady, by měl být zahrnut v dokumentaci pro poskytovatele.

### <a name="code-based-registration"></a>Registrace na základě kódu

Od Entity Framework 6 se konfigurace pro EF v rámci aplikace může zadat v kódu. Úplné podrobnosti najdete v tématu _[Entity Framework konfigurace na základě kódu](https://msdn.microsoft.com/data/jj680699)_ . Běžný způsob, jak zaregistrovat poskytovatele EF pomocí konfigurace založené na kódu, je vytvořit novou třídu, která je odvozena z třídy System. data. entity. DbConfiguration a umístit ji do stejného sestavení jako třídu DbContext. Vaše třída DbConfiguration by potom měla zaregistrovat poskytovatele ve svém konstruktoru. Chcete-li například zaregistrovat poskytovatele SQL Compact, třída DbConfiguration bude vypadat takto:

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

V tomto kódu "SqlCeProviderServices. ProviderInvariantName" je pohodlí pro řetězec neutrálního názvu poskytovatele SQL Server Compact ("System. data. SqlServerCe. 4.0") a SqlCeProviderServices. instance vrací instanci typu Singleton služby SQL Compact. Zprostředkovatel EF

## <a name="what-if-the-provider-i-need-isnt-available"></a>Co když není potřebný poskytovatel k dispozici?

Pokud je poskytovatel k dispozici pro předchozí verze EF, doporučujeme, abyste se obrátili na vlastníka poskytovatele a požádejte ho, aby vytvořil verzi EF6. Měli byste zahrnout odkaz na [dokumentaci pro model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Můžu poskytovatele napsat sami?

Je sice možné vytvořit poskytovatele EF sami, přestože by neměl být považován za triviální podnik. Výše uvedený odkaz o modelu poskytovatele EF6 je dobrým místem, kde začít. Může být také užitečné použít kód pro SQL Server a poskytovatele SQL CE, který je součástí [základu kódu open source v EF](https://github.com/aspnet/EntityFramework6) jako výchozí bod nebo pro referenci.

Všimněte si, že počínaje EF6 je poskytovatel EF méně těsně spojený s podkladovým poskytovatelem ADO.NET. To usnadňuje psaní poskytovatele EF bez nutnosti psát nebo zabalit třídy ADO.NET.
