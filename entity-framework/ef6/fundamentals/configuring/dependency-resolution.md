---
title: Řešení závislostí - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 45681bb0cedecd502b1968b90b7f682d3257dd23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998159"
---
# <a name="dependency-resolution"></a><span data-ttu-id="ba216-102">Řešení závislostí</span><span class="sxs-lookup"><span data-stu-id="ba216-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="ba216-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ba216-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ba216-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="ba216-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="ba216-105">Spouští se s EF6, Entity Framework obsahuje mechanismus pro obecné účely pro získání implementace služeb, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="ba216-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="ba216-106">To znamená když EF používá instanci některé rozhraní nebo základní požádá o zadání na konkrétní implementace rozhraní nebo základní třídy použít.</span><span class="sxs-lookup"><span data-stu-id="ba216-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="ba216-107">Toho můžete dosáhnout pomocí rozhraní IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="ba216-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="ba216-108">Metoda GetService je obvykle volána EF a zařizuje služba implementace IDbDependencyResolver EF nebo aplikace k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ba216-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="ba216-109">Při volání, typ argumentu je typ rozhraní nebo základní třídy požadované služby a klíče objektu je buď null, nebo objekt, který poskytuje kontextové informace o požadovanou službu.</span><span class="sxs-lookup"><span data-stu-id="ba216-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="ba216-110">Tento článek neobsahuje všechny podrobnosti o tom, jak implementovat IDbDependencyResolver, ale místo toho funguje jako reference pro pro které EF volá GetService a sémantika klíčový objekt pro každou z těchto typů služeb (to znamená, typy rozhraní a základní třídy) volání.</span><span class="sxs-lookup"><span data-stu-id="ba216-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="ba216-111">Tento dokument se zachová aktuální při přidání dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="ba216-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="ba216-112">Služby Vyřešeno</span><span class="sxs-lookup"><span data-stu-id="ba216-112">Services resolved</span></span>  

<span data-ttu-id="ba216-113">Pokud není uvedeno jinak, žádné objekt vrácený musí být bezpečná pro vlákno, protože může sloužit jako singleton.</span><span class="sxs-lookup"><span data-stu-id="ba216-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="ba216-114">V mnoha případech objekt se vrátil v takovém případě je objekt pro vytváření samotný objekt factory musí být bezpečná pro vlákno, ale objekt vrácený objekt pro vytváření nemusí být bezpečné pro vlákna, protože je požadována novou instanci z objekt factory pro každé použití.</span><span class="sxs-lookup"><span data-stu-id="ba216-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="ba216-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="ba216-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="ba216-116">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-117">**Objekt vrácený**: databáze inicializátor pro typ daný kontext</span><span class="sxs-lookup"><span data-stu-id="ba216-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="ba216-118">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="ba216-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="ba216-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="ba216-120">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="ba216-121">**Objekt vrácený**: objekt pro vytváření vytvořit generátor SQL, který lze použít pro migraci a další akce, které způsobují databázi vytvořit, jako je vytvoření databáze s inicializátory databáze.</span><span class="sxs-lookup"><span data-stu-id="ba216-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="ba216-122">**Klíč**: řetězec obsahující název výchozí poskytovatel ADO.NET určující typ databáze, pro který se vygeneruje SQL.</span><span class="sxs-lookup"><span data-stu-id="ba216-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="ba216-123">Generátor SQL Server SQL je například vrátil pro klíč "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="ba216-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-124">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="ba216-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="ba216-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="ba216-126">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-127">**Objekt vrácený**: The EF zprostředkovatele má být použit pro výchozí název daného zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="ba216-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="ba216-128">**Klíč**: řetězec obsahující název výchozí poskytovatel ADO.NET určující typ databáze, pro kterou je potřeba zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ba216-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="ba216-129">Zprostředkovatel SQL Server je třeba vrátil pro klíč "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="ba216-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-130">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="ba216-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="ba216-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="ba216-132">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-133">**Objekt vrácený**: objekt factory připojení, který se použije při EF vytvoří připojení k databázi podle konvence.</span><span class="sxs-lookup"><span data-stu-id="ba216-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="ba216-134">To znamená, když žádné připojení nebo připojovací řetězec je přidělena EF a žádný připojovací řetězec najdete v souboru app.config nebo web.config, pak tato služba se používá k vytvoření připojení podle konvence.</span><span class="sxs-lookup"><span data-stu-id="ba216-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="ba216-135">Změna objekt factory připojení můžete povolit EF použít jiný typ databáze (například SQL Server Compact Edition) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba216-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="ba216-136">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-137">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="ba216-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="ba216-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="ba216-139">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-140">**Objekt vrácený**: služba, která můžete vygenerovat token manifestu zprostředkovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="ba216-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="ba216-141">Tato služba se obvykle používá dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="ba216-141">This service is typically used in two ways.</span></span> <span data-ttu-id="ba216-142">Nejprve ji je možné vyhnout Code First při vytváření modelu s připojením k databázi.</span><span class="sxs-lookup"><span data-stu-id="ba216-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="ba216-143">Za druhé to je možné vynutit Code First pro vytvoření modelu pro konkrétní databázi verze – například k vynucení modelu pro SQL Server 2005, i v případě, že někdy se používá SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="ba216-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="ba216-144">**Doba života objektu**: Singleton – stejný objekt mohou být používané více časy a současně v různých vláknech</span><span class="sxs-lookup"><span data-stu-id="ba216-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="ba216-145">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="ba216-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="ba216-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="ba216-147">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-148">**Objekt vrácený**: služba, která může získat továrnu poskytovatele z dané připojení.</span><span class="sxs-lookup"><span data-stu-id="ba216-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="ba216-149">V rozhraní .NET 4.5 je poskytovatel veřejně přístupná z připojení.</span><span class="sxs-lookup"><span data-stu-id="ba216-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="ba216-150">Výchozí implementace této služby na rozhraní .NET 4 používá některé z heuristických metod k nalezení odpovídající zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ba216-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="ba216-151">Pokud se to nepovede pak tuto službu novou implementaci lze registrovat zajistit odpovídající řešení.</span><span class="sxs-lookup"><span data-stu-id="ba216-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="ba216-152">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="ba216-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="ba216-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="ba216-154">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-155">**Objekt vrácený**: objekt factory, který vygeneruje klíč mezipaměti modelu pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="ba216-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="ba216-156">Ve výchozím nastavení EF ukládá do mezipaměti jeden model podle typu DbContext na poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="ba216-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="ba216-157">Jinou implementaci této služby je možné přidat další informace, jako je například název schématu do klíče mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ba216-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="ba216-158">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="ba216-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="ba216-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="ba216-160">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-161">**Objekt vrácený**: EF prostorových zprostředkovatele, který přidává podporu pro základní EF poskytovatele podle zeměpisného umístění a geometrie prostorové typy.</span><span class="sxs-lookup"><span data-stu-id="ba216-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="ba216-162">**Klíč**: DbSptialServices se zobrazí výzva pro dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="ba216-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="ba216-163">První, specifickým pro zprostředkovatele prostorových služeb jsou požadovány pomocí objektu DbProviderInfo (obsahující invariantní vzhledem k názvu a manifestu token) jako klíč.</span><span class="sxs-lookup"><span data-stu-id="ba216-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="ba216-164">Za druhé DbSpatialServices můžete požádat bez klíče.</span><span class="sxs-lookup"><span data-stu-id="ba216-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="ba216-165">To se používá k překladu "globální prostorových poskytovateli" který se používá při vytváření samostatných DbGeography nebo DbGeometry typů.</span><span class="sxs-lookup"><span data-stu-id="ba216-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-166">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="ba216-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="ba216-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="ba216-168">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-169">**Objekt vrácený**: objekt factory pro vytvoření služby, který umožňuje poskytovateli implementovat opakování nebo jiné chování při spuštění dotazů a příkazů na databázi.</span><span class="sxs-lookup"><span data-stu-id="ba216-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="ba216-170">Pokud je k dispozici žádná implementace, pak EF jednoduše spustí příkazy a šířit všechny výjimky vyvolané.</span><span class="sxs-lookup"><span data-stu-id="ba216-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="ba216-171">Tato služba se pro SQL Server používá k poskytování zásady opakování, který je obzvláště užitečná při spouštění cloudové databázové servery, jako je například SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="ba216-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="ba216-172">**Klíč**: ExecutionStrategyKey objekt, který obsahuje výchozí název zprostředkovatele a volitelně také název serveru, pro který se použije strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="ba216-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-173">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="ba216-174">Func < DbConnection, řetězec, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="ba216-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="ba216-175">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-176">**Objekt vrácený**: objekt factory, který umožňuje poskytovateli ke konfiguraci mapování HistoryContext k `__MigrationHistory` tabulky používané migrace EF.</span><span class="sxs-lookup"><span data-stu-id="ba216-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="ba216-177">HistoryContext je první DbContext kódu a lze nakonfigurovat pomocí rozhraní fluent API pro běžné věci, jako je název tabulky a specifikace mapování sloupce změnit.</span><span class="sxs-lookup"><span data-stu-id="ba216-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="ba216-178">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-179">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="ba216-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="ba216-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="ba216-181">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-182">**Objekt vrácený**: zprostředkovatele ADO.NET pro výchozí název daného zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ba216-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="ba216-183">**Klíč**: řetězec obsahující výchozí název zprostředkovatele ADO.NET</span><span class="sxs-lookup"><span data-stu-id="ba216-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-184">Tato služba není obvykle přímo od změnila výchozí implementace používá normální registrace zprostředkovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ba216-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="ba216-185">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="ba216-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="ba216-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="ba216-187">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-188">**Objekt vrácený**: služba, která se používá k určení výchozí název zprostředkovatele pro daný typ DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="ba216-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="ba216-189">Výchozí implementace této služby používá registrace zprostředkovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ba216-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="ba216-190">To znamená, že pokud zprostředkovatele ADO.NET není zaregistrován běžným způsobem, protože probíhá DbProviderFactory řešení pomocí EF, pak bude také potřeba vyřešit tuto službu.</span><span class="sxs-lookup"><span data-stu-id="ba216-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="ba216-191">**Klíč**: The DbProviderFactory instance, pro kterou výchozí název je povinné.</span><span class="sxs-lookup"><span data-stu-id="ba216-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="ba216-192">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="ba216-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="ba216-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="ba216-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="ba216-194">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-195">**Objekt vrácený**: mezipaměť sestavení, které obsahují předem vygenerovaných zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ba216-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="ba216-196">Chcete, aby EF vědět, která sestavení obsahovat předem vygenerovaných zobrazení bez provádění jakékoli zjišťování se obvykle používá můžou nahradit aktuální soubor.</span><span class="sxs-lookup"><span data-stu-id="ba216-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="ba216-197">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="ba216-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="ba216-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="ba216-199">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-200">**Objekt vrácený**: služba používaná EF a pluralize množné číslo názvy.</span><span class="sxs-lookup"><span data-stu-id="ba216-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="ba216-201">Ve výchozím nastavení se používá k službě anglické pluralizace.</span><span class="sxs-lookup"><span data-stu-id="ba216-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="ba216-202">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="ba216-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="ba216-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="ba216-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="ba216-204">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="ba216-205">**Vrácených objektů**: žádné zachycovacích dotazů, které by měly být zaregistrovány při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba216-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="ba216-206">Všimněte si, že tyto objekty jsou požadovány pomocí volání GetServices a budou všechny sběrače vrácený žádné překladače závislostí zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="ba216-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="ba216-207">**Klíč**: není používána a bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="ba216-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="ba216-208">Func < System.Data.Entity.DbContext, akce < řetězec\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="ba216-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="ba216-209">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="ba216-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="ba216-210">**Objekt vrácený**: objekt factory, který se použije k vytvoření protokolu databáze formátovací modul, který bude používat, pokud kontext. Database.Log je hodnota nastavena na daném kontextu.</span><span class="sxs-lookup"><span data-stu-id="ba216-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="ba216-211">**Klíč**: není používána a bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="ba216-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="ba216-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="ba216-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="ba216-213">**Verze zavedené**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="ba216-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="ba216-214">**Objekt vrácený**: objekt factory, který se použije k vytvoření instance kontextu pro migrace kontextu nemá dostupný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="ba216-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="ba216-215">**Klíč**: typ objektu typu odvozené uvolněn objekt DbContext, pro kterou je potřeba objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="ba216-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="ba216-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="ba216-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="ba216-217">**Verze zavedené**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="ba216-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="ba216-218">**Objekt vrácený**: objekt factory, který se použije k vytvoření serializátory za účelem serializace silného typu vlastní poznámky tak, že je možné serializovat a desterilized do souboru XML pro použití v migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="ba216-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="ba216-219">**Klíč**: název poznámky, je serializován nebo deserializován.</span><span class="sxs-lookup"><span data-stu-id="ba216-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="ba216-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="ba216-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="ba216-221">**Verze zavedené**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="ba216-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="ba216-222">**Objekt vrácený**: objekt factory, který se použije k vytvoření obslužné rutiny pro transakce, tak, aby zvláštní zpracování lze použít v situacích, jako je zpracování selhání potvrzení.</span><span class="sxs-lookup"><span data-stu-id="ba216-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="ba216-223">**Klíč**: ExecutionStrategyKey objekt, který obsahuje výchozí název zprostředkovatele a volitelně také název serveru, pro který se použije obslužnou rutinu transakce.</span><span class="sxs-lookup"><span data-stu-id="ba216-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
