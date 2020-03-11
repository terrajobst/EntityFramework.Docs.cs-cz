---
title: Rozlišení závislosti – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417860"
---
# <a name="dependency-resolution"></a><span data-ttu-id="76bbe-102">Řešení závislostí</span><span class="sxs-lookup"><span data-stu-id="76bbe-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="76bbe-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="76bbe-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="76bbe-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="76bbe-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="76bbe-105">Počínaje verzí EF6 Entity Framework obsahuje obecný mechanizmus pro získání implementací služeb, které vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="76bbe-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="76bbe-106">To znamená, že když EF používá instanci některých rozhraní nebo základních tříd, požádá o konkrétní implementaci rozhraní nebo základní třídy, která se má použít.</span><span class="sxs-lookup"><span data-stu-id="76bbe-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="76bbe-107">Toho lze dosáhnout použitím rozhraní IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="76bbe-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="76bbe-108">Metoda GetService je obvykle volána nástrojem EF a je zpracována implementací IDbDependencyResolver poskytnutou buď v EF, nebo v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76bbe-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="76bbe-109">Při volání je argumentem typu rozhraní nebo typ základní třídy požadované služby a objekt Key má buď hodnotu null, nebo objekt, který poskytuje kontextové informace o požadované službě.</span><span class="sxs-lookup"><span data-stu-id="76bbe-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="76bbe-110">Pokud není uvedeno jinak, všechny vrácené objekty musí být bezpečné pro přístup z více vláken, protože je lze použít jako typ singleton.</span><span class="sxs-lookup"><span data-stu-id="76bbe-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="76bbe-111">V mnoha případech je vrácen objekt factory, v takovém případě musí být samotný továrna bezpečná pro přístup z více vláken, ale objekt vrácený z továrny nemusí být bezpečný pro přístup z více vláken, protože z továrny je vyžádána nová instance pro každé použití.</span><span class="sxs-lookup"><span data-stu-id="76bbe-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="76bbe-112">Tento článek neobsahuje úplné podrobnosti o tom, jak implementovat IDbDependencyResolver, ale slouží jako odkaz na typy služeb (tj. rozhraní a typy základních tříd), pro které EF volá metodu GetService a sémantiku klíčového objektu pro každý z těchto volání.</span><span class="sxs-lookup"><span data-stu-id="76bbe-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="76bbe-113">System. data. entity. IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="76bbe-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="76bbe-114">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-115">**Vrácen objekt**: inicializátor databáze pro daný typ kontextu</span><span class="sxs-lookup"><span data-stu-id="76bbe-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="76bbe-116">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="76bbe-117">Func < System. data. entity. migrations. SQL. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="76bbe-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="76bbe-118">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="76bbe-119">**Vrácen objekt**: továrna pro vytvoření generátoru SQL, který se dá použít pro migrace a další akce, které způsobují vytvoření databáze, jako je vytvoření databáze s Inicializátory databáze.</span><span class="sxs-lookup"><span data-stu-id="76bbe-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="76bbe-120">**Key**: řetězec obsahující neutrální název poskytovatele ADO.NET určující typ databáze, pro kterou se bude generovat SQL.</span><span class="sxs-lookup"><span data-stu-id="76bbe-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="76bbe-121">Například generátor SQL Server SQL se vrátí pro klíč "System. data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="76bbe-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-122">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="76bbe-123">System. data. entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="76bbe-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="76bbe-124">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-125">**Vrácen objekt**: Zprostředkovatel EF, který má být použit pro daný neutrální název zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="76bbe-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="76bbe-126">**Klíč**: řetězec obsahující neutrální název poskytovatele ADO.NET určující typ databáze, pro kterou je poskytovatel vyžadován.</span><span class="sxs-lookup"><span data-stu-id="76bbe-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="76bbe-127">Například poskytovatel SQL Server je vrácen pro klíč "System. data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="76bbe-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-128">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="76bbe-129">System. data. entity. Infrastructure. IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="76bbe-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="76bbe-130">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-131">**Vrácený objekt: objekt**pro vytváření připojení, který se použije, když EF vytvoří připojení k databázi podle konvence.</span><span class="sxs-lookup"><span data-stu-id="76bbe-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="76bbe-132">To znamená, že pokud se neudělí žádné připojení nebo připojovací řetězec k EF, a v souboru App. config nebo Web. config se nemůže najít připojovací řetězec, pak se tato služba používá k vytvoření připojení podle konvence.</span><span class="sxs-lookup"><span data-stu-id="76bbe-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="76bbe-133">Změna objektu pro vytváření připojení může ve výchozím nastavení umožňovat, aby EF používal jiný typ databáze (například SQL Server Compact Edition).</span><span class="sxs-lookup"><span data-stu-id="76bbe-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="76bbe-134">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-135">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="76bbe-136">System. data. entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="76bbe-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="76bbe-137">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-138">**Vrácený objekt**: služba, která může vygenerovat token manifestu zprostředkovatele z připojení.</span><span class="sxs-lookup"><span data-stu-id="76bbe-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="76bbe-139">Tato služba se obvykle používá dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="76bbe-139">This service is typically used in two ways.</span></span> <span data-ttu-id="76bbe-140">Nejdřív se dá použít k tomu, abyste se vyhnuli Code First připojení k databázi při sestavování modelu.</span><span class="sxs-lookup"><span data-stu-id="76bbe-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="76bbe-141">Za druhé, dá se použít k vynucení Code First vytvoření modelu pro konkrétní verzi databáze – například pro vynucení modelu SQL Server 2005 i v případě, že se někdy používá SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="76bbe-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="76bbe-142">**Doba života objektu**: singleton – stejný objekt může být použit několikrát a souběžně podle různých vláken.</span><span class="sxs-lookup"><span data-stu-id="76bbe-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="76bbe-143">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="76bbe-144">System. data. entity. Infrastructure. IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="76bbe-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="76bbe-145">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-146">**Vrácen objekt**: služba, která může získat objekt pro vytváření poskytovatele z daného připojení.</span><span class="sxs-lookup"><span data-stu-id="76bbe-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="76bbe-147">V rozhraní .NET 4,5 je poskytovatel veřejně přístupný z připojení.</span><span class="sxs-lookup"><span data-stu-id="76bbe-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="76bbe-148">V rozhraní .NET 4 výchozí implementace této služby používá některé heuristické metody k nalezení odpovídajícího poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="76bbe-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="76bbe-149">V opačném případě je možné zaregistrovat novou implementaci této služby, aby poskytovala příslušné řešení.</span><span class="sxs-lookup"><span data-stu-id="76bbe-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="76bbe-150">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="76bbe-151">Func < DbContext, System. data. entity. Infrastructure. IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="76bbe-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="76bbe-152">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-153">**Vrácen objekt: objekt**pro vytváření, který vygeneruje klíč mezipaměti modelu pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="76bbe-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="76bbe-154">EF ve výchozím nastavení ukládá do mezipaměti jeden model na DbContext typ na každého poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="76bbe-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="76bbe-155">Jinou implementaci této služby lze použít k přidání dalších informací, jako je například název schématu, do klíče mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="76bbe-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="76bbe-156">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="76bbe-157">System. data. entity. prostor. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="76bbe-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="76bbe-158">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-159">**Vrácen objekt**: poskytovatel pro prostor EF, který přidává podporu základního poskytovatele EF pro geografické a geometrické prostorové typy.</span><span class="sxs-lookup"><span data-stu-id="76bbe-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="76bbe-160">**Klíč**: DbSptialServices se vyzve dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="76bbe-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="76bbe-161">Nejprve jsou požadovány prostorové služby specifické pro poskytovatele pomocí objektu DbProviderInfo (který obsahuje neutrální název a token manifestu) jako klíč.</span><span class="sxs-lookup"><span data-stu-id="76bbe-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="76bbe-162">Druhý DbSpatialServices může být požádán o bez klíče.</span><span class="sxs-lookup"><span data-stu-id="76bbe-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="76bbe-163">Slouží k vyřešení "globálního poskytovatele prostorového přístupu", který se používá při vytváření samostatných typů DbGeography nebo DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="76bbe-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-164">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="76bbe-165">Func < System. data. entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="76bbe-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="76bbe-166">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-167">**Vrácen objekt**: továrna pro vytvoření služby, která umožňuje zprostředkovateli implementovat opakování nebo jiné chování při spuštění dotazů a příkazů v databázi.</span><span class="sxs-lookup"><span data-stu-id="76bbe-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="76bbe-168">Pokud není k dispozici žádná implementace, pak EF jednoduše provede příkazy a rozšíří všechny vyvolané výjimky.</span><span class="sxs-lookup"><span data-stu-id="76bbe-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="76bbe-169">Pro SQL Server se tato služba používá k poskytnutí zásady opakování, která je obzvláště užitečná při spuštění na cloudových databázových serverech, jako je SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="76bbe-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="76bbe-170">**Key**: objekt ExecutionStrategyKey, který obsahuje neutrální název poskytovatele a volitelně název serveru, pro který se použije strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="76bbe-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-171">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="76bbe-172">Func < DbConnection, String, System. data. entity. migrations. History. HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="76bbe-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="76bbe-173">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-174">**Vrácen objekt**: továrna, která umožňuje poskytovateli nakonfigurovat mapování HistoryContext na tabulku `__MigrationHistory`, kterou používají migrace EF.</span><span class="sxs-lookup"><span data-stu-id="76bbe-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="76bbe-175">HistoryContext je Code First DbContext a dá se nakonfigurovat pomocí normálního rozhraní API Fluent, aby se změnily položky, jako je název tabulky a specifikace mapování sloupců.</span><span class="sxs-lookup"><span data-stu-id="76bbe-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="76bbe-176">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-177">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="76bbe-178">System. data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="76bbe-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="76bbe-179">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-180">**Vrácen objekt**: poskytovatel ADO.NET, který má být použit pro daný neutrální název zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="76bbe-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="76bbe-181">**Klíč**: řetězec obsahující neutrální název poskytovatele ADO.NET</span><span class="sxs-lookup"><span data-stu-id="76bbe-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-182">Tato služba se obvykle nemění přímo, protože výchozí implementace používá normální registraci poskytovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="76bbe-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="76bbe-183">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="76bbe-184">System. data. entity. Infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="76bbe-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="76bbe-185">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-186">**Vrácen objekt**: služba, která slouží k určení neutrálního názvu poskytovatele pro daný typ DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="76bbe-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="76bbe-187">Výchozí implementace této služby používá registraci poskytovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="76bbe-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="76bbe-188">To znamená, že pokud poskytovatel ADO.NET není zaregistrován v normálním způsobu, protože DbProviderFactory je vyřešen pomocí EF, bude také potřeba tuto službu vyřešit.</span><span class="sxs-lookup"><span data-stu-id="76bbe-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="76bbe-189">**Key**: instance DbProviderFactory, pro kterou je vyžadován neutrální název.</span><span class="sxs-lookup"><span data-stu-id="76bbe-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="76bbe-190">Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="76bbe-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="76bbe-191">System. data. entity. Core. Mapping. ViewGeneration. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="76bbe-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="76bbe-192">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-193">**Vrácen objekt**: mezipaměť sestavení, která obsahují předem vygenerovaná zobrazení.</span><span class="sxs-lookup"><span data-stu-id="76bbe-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="76bbe-194">Náhrada se obvykle používá k tomu, aby EF vědělo, která sestavení obsahují předem vygenerovaná zobrazení bez jakéhokoli zjištění.</span><span class="sxs-lookup"><span data-stu-id="76bbe-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="76bbe-195">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="76bbe-196">System. data. entity. Infrastructure. plural. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="76bbe-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="76bbe-197">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-198">**Vrácen objekt**: služba, kterou používá EF k doplnit jednotné a množné číslo názvů.</span><span class="sxs-lookup"><span data-stu-id="76bbe-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="76bbe-199">Ve výchozím nastavení se používá služba anglické plurality.</span><span class="sxs-lookup"><span data-stu-id="76bbe-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="76bbe-200">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="76bbe-201">System. data. entity. Infrastructure. Intercept. IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="76bbe-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="76bbe-202">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="76bbe-203">**Vrácené objekty**: jakékoli zachycení, které by mělo být registrováno při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="76bbe-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="76bbe-204">Všimněte si, že tyto objekty jsou požadovány pomocí volání GetService a všechny zachycení vrácené všemi Překladači závislostí budou registrovány.</span><span class="sxs-lookup"><span data-stu-id="76bbe-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="76bbe-205">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="76bbe-206">Func < System. data. entity. DbContext, Action < String\>, System. data. entity. Infrastructure. Intercept. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="76bbe-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="76bbe-207">**Představená verze**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="76bbe-208">**Vrácen objekt: objekt**pro vytváření, který bude použit k vytvoření formátovacího modulu protokolu databáze, který bude použit v případě kontextu. Vlastnost Database. log je nastavena pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="76bbe-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="76bbe-209">**Klíč**: nepoužívá se; bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="76bbe-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="76bbe-210">Func < System. data. entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="76bbe-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="76bbe-211">**Představená verze**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="76bbe-212">**Vrácený objekt: objekt**pro vytváření, který se použije k vytvoření kontextových instancí pro migrace, když kontext nemá přístupný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="76bbe-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="76bbe-213">**Key**: typ objektu pro typ odvozeného DbContext, pro který je potřeba objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="76bbe-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="76bbe-214">Func < System. data. entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="76bbe-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="76bbe-215">**Představená verze**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="76bbe-216">**Vrácen objekt**: továrna, která bude použita k vytvoření serializátorů pro serializaci vlastních poznámek silně typovaného typu tak, aby mohly být serializovány a odsterilizovány do XML pro použití v migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="76bbe-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="76bbe-217">**Key**: název poznámky, která je serializována nebo deserializována.</span><span class="sxs-lookup"><span data-stu-id="76bbe-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="76bbe-218">Func < System. data. entity. Infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="76bbe-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="76bbe-219">**Představená verze**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="76bbe-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="76bbe-220">**Vrácen objekt**: továrna, která bude použita k vytváření obslužných rutin pro transakce, aby bylo možné použít speciální zpracování pro situace, jako je například zpracování selhání potvrzení.</span><span class="sxs-lookup"><span data-stu-id="76bbe-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="76bbe-221">**Key**: objekt ExecutionStrategyKey, který obsahuje neutrální název poskytovatele a volitelně název serveru, pro který se použije obslužná rutina transakce.</span><span class="sxs-lookup"><span data-stu-id="76bbe-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
