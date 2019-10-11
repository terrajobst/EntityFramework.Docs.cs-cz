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
# <a name="entity-framework-6-providers"></a><span data-ttu-id="2b074-102">Poskytovatelé Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2b074-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="2b074-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2b074-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="2b074-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="2b074-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2b074-105">Entity Framework se teď vyvíjí v rámci open-source licence a EF6 a výše se nedodává jako součást .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2b074-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="2b074-106">Má spoustu výhod, ale také vyžaduje, aby se poskytovatelé EF znovu vytvořili proti EF6 sestavením.</span><span class="sxs-lookup"><span data-stu-id="2b074-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="2b074-107">To znamená, že zprostředkovatelé EF pro EF5 a níže nebudou pracovat s EF6, dokud nebudou znovu sestaveny.</span><span class="sxs-lookup"><span data-stu-id="2b074-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="2b074-108">Kteří poskytovatelé jsou k dispozici pro EF6?</span><span class="sxs-lookup"><span data-stu-id="2b074-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="2b074-109">Poskytovatelé, o kterých jsme se znovu vytvořili pro EF6, obsahují tyto informace:</span><span class="sxs-lookup"><span data-stu-id="2b074-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="2b074-110">**Poskytovatel Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="2b074-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="2b074-111">Sestaveno z [Entity Framework základ kódu Open Source](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="2b074-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="2b074-112">Dodává se jako součást [balíčku NuGet EntityFramework](https://nuget.org/packages/EntityFramework) .</span><span class="sxs-lookup"><span data-stu-id="2b074-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="2b074-113">**Poskytovatel edice Microsoft SQL Server Compact**</span><span class="sxs-lookup"><span data-stu-id="2b074-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="2b074-114">Sestaveno z [Entity Framework základ kódu Open Source](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="2b074-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="2b074-115">Dodává se v [balíčku NuGet EntityFramework. SqlServerCompact.](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="2b074-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="2b074-116">**Devart dotConnect – poskytovatelé dat**</span><span class="sxs-lookup"><span data-stu-id="2b074-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="2b074-117">Existují poskytovatelé třetích stran od [Devart](https://www.devart.com/) pro nejrůznější databáze, mezi které patří Oracle, MySQL, PostgreSQL, SQLite, SALESFORCE, DB2 a SQL Server</span><span class="sxs-lookup"><span data-stu-id="2b074-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="2b074-118">**CData poskytovatelé softwaru**</span><span class="sxs-lookup"><span data-stu-id="2b074-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="2b074-119">Existují poskytovatelé třetích stran od [CDATA softwaru](https://www.cdata.com/ado/) pro nejrůznější úložiště dat, včetně Salesforce, Azure Table Storage, MySQL a spousty dalších.</span><span class="sxs-lookup"><span data-stu-id="2b074-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="2b074-120">**Poskytovatel Firebird**</span><span class="sxs-lookup"><span data-stu-id="2b074-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="2b074-121">K dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)</span><span class="sxs-lookup"><span data-stu-id="2b074-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="2b074-122">**Poskytovatel Visual Fox pro**</span><span class="sxs-lookup"><span data-stu-id="2b074-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="2b074-123">K dispozici jako [balíček NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="2b074-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="2b074-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="2b074-124">**MySQL**</span></span>
    *   [<span data-ttu-id="2b074-125">Konektor MySQL/síť pro Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2b074-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="2b074-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="2b074-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="2b074-127">Npgsql je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/) .</span><span class="sxs-lookup"><span data-stu-id="2b074-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="2b074-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="2b074-128">**Oracle**</span></span>
    *   <span data-ttu-id="2b074-129">ODP.NET je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/) .</span><span class="sxs-lookup"><span data-stu-id="2b074-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="2b074-130">Všimněte si, že zahrnutí do tohoto seznamu neindikuje úroveň funkčnosti nebo podpory pro daného zprostředkovatele, ale k dispozici je pouze sestavení pro EF6.</span><span class="sxs-lookup"><span data-stu-id="2b074-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="2b074-131">Registrace zprostředkovatelů EF</span><span class="sxs-lookup"><span data-stu-id="2b074-131">Registering EF providers</span></span>

<span data-ttu-id="2b074-132">Počínaje Entity Framework 6 je možné zaregistrovat poskytovatele EF buď pomocí konfigurace založené na kódu, nebo v konfiguračním souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b074-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="2b074-133">Registrace konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="2b074-133">Config file registration</span></span>

<span data-ttu-id="2b074-134">Registrace poskytovatele EF v App. config nebo Web. config má následující formát:</span><span class="sxs-lookup"><span data-stu-id="2b074-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="2b074-135">Počítejte s tím, že pokud je zprostředkovatel EF nainstalovaný z NuGet, pak balíček NuGet tuto registraci automaticky přidá do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="2b074-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="2b074-136">Pokud nainstalujete balíček NuGet do projektu, který není spouštěným projektem aplikace, může být nutné zkopírovat registraci do konfiguračního souboru pro svůj projekt po spuštění.</span><span class="sxs-lookup"><span data-stu-id="2b074-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="2b074-137">V této registraci je "název" "invariantní" stejný neutrální název, který slouží k identifikaci poskytovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2b074-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="2b074-138">To lze najít jako "neutrální" atribut v registraci DbProviderFactories a jako atribut "providerName" v registraci připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="2b074-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="2b074-139">Invariantní název, který se má použít, by měl být také zahrnut v dokumentaci pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="2b074-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="2b074-140">Příklady neutrálních názvů jsou "System. data. SqlClient" pro SQL Server a "System. data. SqlServerCe. 4.0" pro SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="2b074-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="2b074-141">"Type" v této registraci je kvalifikovaný název sestavení typu zprostředkovatele, který je odvozený od "System. data. entity. Core. Common. DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="2b074-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="2b074-142">Například řetězec, který se má použít pro SQL Compact, je "System. data. entity. SqlServerCompact. SqlCeProviderServices, EntityFramework. SqlServerCompact".</span><span class="sxs-lookup"><span data-stu-id="2b074-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="2b074-143">Typ, který se má použít tady, by měl být zahrnut v dokumentaci pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="2b074-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="2b074-144">Registrace na základě kódu</span><span class="sxs-lookup"><span data-stu-id="2b074-144">Code-based registration</span></span>

<span data-ttu-id="2b074-145">Od Entity Framework 6 se konfigurace pro EF v rámci aplikace může zadat v kódu.</span><span class="sxs-lookup"><span data-stu-id="2b074-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="2b074-146">Úplné podrobnosti najdete v tématu _[Entity Framework konfigurace na základě kódu](https://msdn.microsoft.com/data/jj680699)_ .</span><span class="sxs-lookup"><span data-stu-id="2b074-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="2b074-147">Běžný způsob, jak zaregistrovat poskytovatele EF pomocí konfigurace založené na kódu, je vytvořit novou třídu, která je odvozena z třídy System. data. entity. DbConfiguration a umístit ji do stejného sestavení jako třídu DbContext.</span><span class="sxs-lookup"><span data-stu-id="2b074-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="2b074-148">Vaše třída DbConfiguration by potom měla zaregistrovat poskytovatele ve svém konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="2b074-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="2b074-149">Chcete-li například zaregistrovat poskytovatele SQL Compact, třída DbConfiguration bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2b074-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="2b074-150">V tomto kódu "SqlCeProviderServices. ProviderInvariantName" je pohodlí pro řetězec neutrálního názvu poskytovatele SQL Server Compact ("System. data. SqlServerCe. 4.0") a SqlCeProviderServices. instance vrací instanci typu Singleton služby SQL Compact. Zprostředkovatel EF</span><span class="sxs-lookup"><span data-stu-id="2b074-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="2b074-151">Co když není potřebný poskytovatel k dispozici?</span><span class="sxs-lookup"><span data-stu-id="2b074-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="2b074-152">Pokud je poskytovatel k dispozici pro předchozí verze EF, doporučujeme, abyste se obrátili na vlastníka poskytovatele a požádejte ho, aby vytvořil verzi EF6.</span><span class="sxs-lookup"><span data-stu-id="2b074-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="2b074-153">Měli byste zahrnout odkaz na [dokumentaci pro model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="2b074-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="2b074-154">Můžu poskytovatele napsat sami?</span><span class="sxs-lookup"><span data-stu-id="2b074-154">Can I write a provider myself?</span></span>

<span data-ttu-id="2b074-155">Je sice možné vytvořit poskytovatele EF sami, přestože by neměl být považován za triviální podnik.</span><span class="sxs-lookup"><span data-stu-id="2b074-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="2b074-156">Výše uvedený odkaz o modelu poskytovatele EF6 je dobrým místem, kde začít.</span><span class="sxs-lookup"><span data-stu-id="2b074-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="2b074-157">Může být také užitečné použít kód pro SQL Server a poskytovatele SQL CE, který je součástí [základu kódu open source v EF](https://github.com/aspnet/EntityFramework6) jako výchozí bod nebo pro referenci.</span><span class="sxs-lookup"><span data-stu-id="2b074-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="2b074-158">Všimněte si, že počínaje EF6 je poskytovatel EF méně těsně spojený s podkladovým poskytovatelem ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2b074-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="2b074-159">To usnadňuje psaní poskytovatele EF bez nutnosti psát nebo zabalit třídy ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2b074-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
