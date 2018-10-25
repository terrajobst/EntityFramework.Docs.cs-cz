---
title: Entity Framework poskytovatelé - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: 0cfe6c30d6533b0207f78a031d342847a7f68632
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022220"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="b9d8b-102">Zprostředkovatelé Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b9d8b-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="b9d8b-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b9d8b-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b9d8b-105">Rozhraní Entity Framework je nyní vyvíjených v licenci open source a EF6 a nad nebude odeslaná jako součást rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="b9d8b-106">To má mnoho výhod, ale také vyžaduje, že EF poskytovatelé znovu sestavit na EF6 sestavení.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="b9d8b-107">To znamená, že EF poskytovatelů pro EF5 a pod nebudou fungovat s EF6, dokud se po opětovném sestavení.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="b9d8b-108">Poskytovatelů, kteří jsou k dispozici pro EF6?</span><span class="sxs-lookup"><span data-stu-id="b9d8b-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="b9d8b-109">Víme o poskytovatele, který po opětovném sestavení pro EF6 patří:</span><span class="sxs-lookup"><span data-stu-id="b9d8b-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="b9d8b-110">**Zprostředkovatel Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="b9d8b-111">Od [Entity Framework otevřete základu zdrojového kódu](http://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-111">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="b9d8b-112">Dodán jako součást [balíček NuGet objektu EntityFramework](http://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-112">Shipped as part of the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="b9d8b-113">**Zprostředkovatel Microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="b9d8b-114">Od [Entity Framework otevřete základu zdrojového kódu](http://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-114">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="b9d8b-115">Součástí [balíček EntityFramework.SqlServerCompact NuGet](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="b9d8b-116">**Devart dotConnect zprostředkovatelů dat.**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-116">**Devart dotConnect Data Providers**</span></span>](http://www.devart.com/dotconnect/)
    *   <span data-ttu-id="b9d8b-117">Existují poskytovatele třetí strany z [Devart](http://www.devart.com/) pro širokou škálu databází včetně Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 a SQL Server</span><span class="sxs-lookup"><span data-stu-id="b9d8b-117">There are third-party providers from [Devart](http://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="b9d8b-118">**Poskytovatelé softwaru CData**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-118">**CData Software providers**</span></span>](http://www.cdata.com/ado/)
    *   <span data-ttu-id="b9d8b-119">Existují poskytovatele třetí strany z [CData softwaru](http://www.cdata.com/ado/) pro širokou škálu úložišť dat, včetně Salesforce, Azure Table Storage, MySql a mnoho dalších</span><span class="sxs-lookup"><span data-stu-id="b9d8b-119">There are third-party providers from [CData Software](http://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="b9d8b-120">**Firebird poskytovatele**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="b9d8b-121">K dispozici jako [balíček NuGet](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-121">Available as a [NuGet Package](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span></span>
*   <span data-ttu-id="b9d8b-122">**Vizuální Fox Pro zprostředkovatele**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="b9d8b-123">K dispozici jako [balíček NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="b9d8b-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-124">**MySQL**</span></span>
    *   [<span data-ttu-id="b9d8b-125">MySQL Connector/Net</span><span class="sxs-lookup"><span data-stu-id="b9d8b-125">MySQL Connector/Net</span></span>](http://dev.mysql.com/downloads/connector/net/)
*   <span data-ttu-id="b9d8b-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="b9d8b-127">Je k dispozici jako Npgsql [balíček NuGet](http://www.nuget.org/packages/Npgsql.EF6/)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-127">Npgsql is available as a [NuGet package](http://www.nuget.org/packages/Npgsql.EF6/)</span></span>
*   <span data-ttu-id="b9d8b-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="b9d8b-128">**Oracle**</span></span>
    *   <span data-ttu-id="b9d8b-129">Je k dispozici jako ODP.NET [balíček NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="b9d8b-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="b9d8b-130">Všimněte si, že zařazení v tomto seznamu nenaznačuje, že úroveň funkce nebo podporu pro daného zprostředkovatele, pouze to, že sestavení pro EF6 byl zpřístupněn.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="b9d8b-131">Registrace zprostředkovatele EF</span><span class="sxs-lookup"><span data-stu-id="b9d8b-131">Registering EF providers</span></span>

<span data-ttu-id="b9d8b-132">Od zprostředkovatele Entity Framework 6 EF lze registrovat pomocí konfigurace buď založený na kódu nebo v konfiguračním souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="b9d8b-133">Registrace konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="b9d8b-133">Config file registration</span></span>

<span data-ttu-id="b9d8b-134">Registrace poskytovatele EF v souboru app.config nebo web.config má následující formát:</span><span class="sxs-lookup"><span data-stu-id="b9d8b-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="b9d8b-135">Všimněte si, že často poskytovateli EF při instalaci z NuGet, pak balíček NuGet automaticky přidá tuto registraci do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="b9d8b-136">Pokud balíček NuGet nainstalovat do projektu, který není spouštěný projekt pro vaši aplikaci, budete muset zkopírujte registrace do konfiguračního souboru pro spouštěný projekt.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="b9d8b-137">"InvariantName" v tomto registraci je stejný neutrální název používaný k identifikaci poskytovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="b9d8b-138">To lze nalézt jako atribut "Výchozí" v registraci DbProviderFactories a jako atribut "providerName" v registraci řetězec připojení.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="b9d8b-139">Výchozí název položky používat také být součástí dokumentace pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="b9d8b-140">Příklady invariantní názvů, které jsou "System.Data.SqlClient" pro SQL Server a "System.Data.SqlServerCe.4.0" pro SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="b9d8b-141">"Type" v tomto registraci je název kvalifikovaný pro sestavení typu poskytovatele, který je odvozen od "System.Data.Entity.Core.Common.DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="b9d8b-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="b9d8b-142">Řetězec, který má použít pro SQL Compact je například "System.Data.Entity.SqlServerCompact.SqlCeProviderServices EntityFramework.SqlServerCompact".</span><span class="sxs-lookup"><span data-stu-id="b9d8b-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="b9d8b-143">Typ použití zde by měl být součástí dokumentace pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="b9d8b-144">Registrace na úrovni kódu</span><span class="sxs-lookup"><span data-stu-id="b9d8b-144">Code-based registration</span></span>

<span data-ttu-id="b9d8b-145">Od verze Entity Framework 6 celou aplikaci konfigurace pro EF můžete zadat v kódu.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="b9d8b-146">Úplné podrobnosti najdete v tématu  _[Entity Framework Code-Based konfigurace](https://msdn.microsoft.com/data/jj680699)_.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="b9d8b-147">Obvyklým způsobem zaregistrovat poskytovatele EF pomocí konfigurace založená na kódu je vytvořte novou třídu, která je odvozena z System.Data.Entity.DbConfiguration a jeho následné uložení do stejného sestavení jako vaší třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="b9d8b-148">Vaše třída DbConfiguration by pak zaregistrujte poskytovatele 've svém konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="b9d8b-149">Například k registraci SQL Compact poskytovatele DbConfiguration třídy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b9d8b-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="b9d8b-150">V tomto kódu "SqlCeProviderServices.ProviderInvariantName" je v zájmu usnadnění pro SQL Server Compact výchozí název zprostředkovatele řetězec ("System.Data.SqlServerCe.4.0") a SqlCeProviderServices.Instance vrátí instanci typu singleton SQL Compact EF zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="b9d8b-151">Co když se poskytovatel budu potřebovat není k dispozici?</span><span class="sxs-lookup"><span data-stu-id="b9d8b-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="b9d8b-152">Pokud se k dispozici v předchozích verzích sady EF, pak doporučujeme obraťte se na vlastníka poskytovatele a požádejte ho, chcete-li vytvořit ve verzi EF6.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="b9d8b-153">Měli byste zahrnout odkaz na [dokumentaci podle modelu poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="b9d8b-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="b9d8b-154">Můžete mi psát zprostředkovatele vlastní?</span><span class="sxs-lookup"><span data-stu-id="b9d8b-154">Can I write a provider myself?</span></span>

<span data-ttu-id="b9d8b-155">Je určitě možné vytvořit poskytovatele EF, sami sebe i když by neměly být zahrnuté triviální podniku.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="b9d8b-156">Výše uvedený odkaz o podle modelu poskytovatele EF6 je dobrým začátkem.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="b9d8b-157">Vám může být také vhodné k použití tohoto kódu pro zprostředkovatele SQL Server a SQL CE součástí [EF opensourcových codebase](https://github.com/aspnet/EntityFramework6) jako výchozí bod nebo odkaz.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="b9d8b-158">Všimněte si, že počínaje EF6 poskytovateli EF je menší těsně spjat s podkladového zprostředkovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="b9d8b-159">Díky tomu je snazší psát poskytovatele EF aniž byste museli napsat nebo zabalovat třídy rozhraní ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="b9d8b-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
