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
# <a name="entity-framework-6-providers"></a><span data-ttu-id="04e86-102">Zprostředkovatelé rozhraní entity 6</span><span class="sxs-lookup"><span data-stu-id="04e86-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="04e86-103">**EF6 Pouze dále** – funkce, rozhraní API atd.</span><span class="sxs-lookup"><span data-stu-id="04e86-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="04e86-104">Pokud používáte starší verzi, některé nebo všechny informace se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="04e86-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="04e86-105">Entity Framework je nyní vyvíjen pod open source licence a EF6 a výše nebudou dodávány jako součást rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="04e86-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="04e86-106">To má mnoho výhod, ale také vyžaduje, aby zprostředkovatelé EF znovu sestavit proti sestavení EF6.</span><span class="sxs-lookup"><span data-stu-id="04e86-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="04e86-107">To znamená, že zprostředkovatelé EF pro EF5 a nižší nebudou pracovat s EF6, dokud nebudou znovu sestaveny.</span><span class="sxs-lookup"><span data-stu-id="04e86-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="04e86-108">Kteří zprostředkovatelé jsou k dispozici pro EF6?</span><span class="sxs-lookup"><span data-stu-id="04e86-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="04e86-109">Poskytovatelé jsme si vědomi, že byly přestavěny pro EF6 patří:</span><span class="sxs-lookup"><span data-stu-id="04e86-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="04e86-110">**Zprostředkovatel serveru Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="04e86-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="04e86-111">Vytvořeno z [open source kódu entity Framework](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="04e86-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="04e86-112">Dodáno jako součást [balíčku EntityFramework NuGet](https://nuget.org/packages/EntityFramework)</span><span class="sxs-lookup"><span data-stu-id="04e86-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="04e86-113">**Zprostředkovatel microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="04e86-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="04e86-114">Vytvořeno z [open source kódu entity Framework](https://github.com/aspnet/EntityFramework6)</span><span class="sxs-lookup"><span data-stu-id="04e86-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="04e86-115">Dodáno v [balíčku EntityFramework.SqlServerCompact NuGet](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span><span class="sxs-lookup"><span data-stu-id="04e86-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="04e86-116">**Zprostředkovatelé dat Devart dotConnect**</span><span class="sxs-lookup"><span data-stu-id="04e86-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="04e86-117">Existují externí poskytovatelé od [Společnosti Devart](https://www.devart.com/) pro různé databáze včetně Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 a SQL Server</span><span class="sxs-lookup"><span data-stu-id="04e86-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="04e86-118">**Poskytovatelé softwaru CData**</span><span class="sxs-lookup"><span data-stu-id="04e86-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="04e86-119">Existují externí poskytovatelé z [CData Software](https://www.cdata.com/ado/) pro různá úložiště dat, včetně Salesforce, Azure Table Storage, MySql a mnoho dalších</span><span class="sxs-lookup"><span data-stu-id="04e86-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="04e86-120">**Firebird poskytovatel**</span><span class="sxs-lookup"><span data-stu-id="04e86-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="04e86-121">K dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/)</span><span class="sxs-lookup"><span data-stu-id="04e86-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="04e86-122">**Poskytovatel visual fox pro**</span><span class="sxs-lookup"><span data-stu-id="04e86-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="04e86-123">K dispozici jako [balíček NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span><span class="sxs-lookup"><span data-stu-id="04e86-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="04e86-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="04e86-124">**MySQL**</span></span>
    *   [<span data-ttu-id="04e86-125">MySQL Konektor/NET pro entity frameworku</span><span class="sxs-lookup"><span data-stu-id="04e86-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="04e86-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="04e86-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="04e86-127">Npgsql je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span><span class="sxs-lookup"><span data-stu-id="04e86-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="04e86-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="04e86-128">**Oracle**</span></span>
    *   <span data-ttu-id="04e86-129">ODP.NET je k dispozici jako [balíček NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span><span class="sxs-lookup"><span data-stu-id="04e86-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="04e86-130">Všimněte si, že zahrnutí v tomto seznamu neznamená úroveň funkčnosti nebo podpory pro daného zprostředkovatele, pouze, že sestavení pro EF6 byla k dispozici.</span><span class="sxs-lookup"><span data-stu-id="04e86-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="04e86-131">Registrace zprostředkovatelů EF</span><span class="sxs-lookup"><span data-stu-id="04e86-131">Registering EF providers</span></span>

<span data-ttu-id="04e86-132">Počínaje entity Framework 6 zprostředkovatelé EF lze zaregistrovat pomocí konfigurace založené na kódu nebo v konfiguračním souboru aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e86-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="04e86-133">Registrace konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="04e86-133">Config file registration</span></span>

<span data-ttu-id="04e86-134">Registrace zprostředkovatele EF v souboru app.config nebo web.config má následující formát:</span><span class="sxs-lookup"><span data-stu-id="04e86-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="04e86-135">Všimněte si, že často pokud je poskytovatel EF nainstalován z NuGet, pak balíček NuGet automaticky přidá tuto registraci do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="04e86-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="04e86-136">Pokud nainstalujete balíček NuGet do projektu, který není projekt spuštění pro vaši aplikaci, pak možná budete muset zkopírovat registraci do konfiguračního souboru pro projekt spuštění.</span><span class="sxs-lookup"><span data-stu-id="04e86-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="04e86-137">"InvariantName" v této registraci je stejný invariantní název používaný k identifikaci zprostředkovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="04e86-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="04e86-138">To lze nalézt jako atribut "invariant" v registraci DbProviderFactories a jako atribut "providerName" v registraci připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="04e86-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="04e86-139">Název invariant, který chcete použít, by měl být také zahrnut v dokumentaci pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="04e86-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="04e86-140">Příklady invariantních názvů jsou "System.Data.SqlClient" pro SQL Server a "System.Data.SqlServerCe.4.0" pro SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="04e86-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="04e86-141">"Typ" v této registraci je název typu zprostředkovatele s kvalifikací sestavení, který je odvozen z "System.Data.Entity.Core.Common.DbProviderServices".</span><span class="sxs-lookup"><span data-stu-id="04e86-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="04e86-142">Například řetězec, který se má použít pro SQL Compact, je "System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact".</span><span class="sxs-lookup"><span data-stu-id="04e86-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="04e86-143">Typ, který se má použít zde by měly být zahrnuty v dokumentaci pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="04e86-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="04e86-144">Registrace založená na kódu</span><span class="sxs-lookup"><span data-stu-id="04e86-144">Code-based registration</span></span>

<span data-ttu-id="04e86-145">Počínaje entity Framework 6 konfigurace pro celou aplikaci EF lze zadat v kódu.</span><span class="sxs-lookup"><span data-stu-id="04e86-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="04e86-146">Podrobné informace naleznete _[v tématu Konfigurace založená na kódu entity frameworku](https://msdn.microsoft.com/data/jj680699)_.</span><span class="sxs-lookup"><span data-stu-id="04e86-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="04e86-147">Normální způsob registrace zprostředkovatele EF pomocí konfigurace založené na kódu je vytvořit novou třídu, která je odvozena od System.Data.Entity.DbConfiguration a umístit ji do stejného sestavení jako vaše dbcontext třídy.</span><span class="sxs-lookup"><span data-stu-id="04e86-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="04e86-148">Vaše DbConfiguration třída by pak zaregistrovat zprostředkovatele v jeho konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="04e86-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="04e86-149">Chcete-li například zaregistrovat zprostředkovatele SQL Compact, třída DbConfiguration vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="04e86-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="04e86-150">V tomto kódu "SqlCeProviderServices.ProviderInvariantName" je pohodlí pro SQL Server Compact kompaktní název řetězce ("System.Data.SqlServerCe.4.0") a SqlCeProviderServices.Instance vrátí instanci singleton poskytovatele SQL Compact.</span><span class="sxs-lookup"><span data-stu-id="04e86-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="04e86-151">Co když poskytovatel, který potřebuji, není k dispozici?</span><span class="sxs-lookup"><span data-stu-id="04e86-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="04e86-152">Pokud je poskytovatel k dispozici pro předchozí verze EF, doporučujeme kontaktovat vlastníka zprostředkovatele a požádat je o vytvoření verze EF6.</span><span class="sxs-lookup"><span data-stu-id="04e86-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="04e86-153">Měli byste zahrnout odkaz na [dokumentaci pro model zprostředkovatele EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="04e86-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="04e86-154">Mohu napsat poskytovatel sám?</span><span class="sxs-lookup"><span data-stu-id="04e86-154">Can I write a provider myself?</span></span>

<span data-ttu-id="04e86-155">Je jistě možné vytvořit poskytovatele EF sami, i když by neměl být považován za triviální podnik.</span><span class="sxs-lookup"><span data-stu-id="04e86-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="04e86-156">Výše uvedený odkaz o modelu zprostředkovatele EF6 je dobrým místem pro začátek.</span><span class="sxs-lookup"><span data-stu-id="04e86-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="04e86-157">Může také být užitečné použít kód pro SQL Server a SQL CE zprostředkovatele zahrnuté v [EF open source codebase](https://github.com/aspnet/EntityFramework6) jako výchozí bod nebo pro referenci.</span><span class="sxs-lookup"><span data-stu-id="04e86-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="04e86-158">Všimněte si, že počínaje EF6 zprostředkovatele ef je méně úzce spojena s poskytovatelem základní ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="04e86-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="04e86-159">To usnadňuje zápis zprostředkovatele EF bez nutnosti zápisu nebo zabalení ADO.NET třídy.</span><span class="sxs-lookup"><span data-stu-id="04e86-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
