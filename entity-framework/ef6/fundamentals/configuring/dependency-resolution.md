---
title: Řešení závislostí - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490958"
---
# <a name="dependency-resolution"></a><span data-ttu-id="90556-102">Řešení závislostí</span><span class="sxs-lookup"><span data-stu-id="90556-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="90556-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="90556-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="90556-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="90556-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="90556-105">Spouští se s EF6, Entity Framework obsahuje mechanismus pro obecné účely pro získání implementace služeb, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="90556-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="90556-106">To znamená když EF používá instanci některé rozhraní nebo základní požádá o zadání na konkrétní implementace rozhraní nebo základní třídy použít.</span><span class="sxs-lookup"><span data-stu-id="90556-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="90556-107">Toho můžete dosáhnout pomocí rozhraní IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="90556-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="90556-108">Metoda GetService je obvykle volána EF a zařizuje služba implementace IDbDependencyResolver EF nebo aplikace k dispozici.</span><span class="sxs-lookup"><span data-stu-id="90556-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="90556-109">Při volání, typ argumentu je typ rozhraní nebo základní třídy požadované služby a klíče objektu je buď null, nebo objekt, který poskytuje kontextové informace o požadovanou službu.</span><span class="sxs-lookup"><span data-stu-id="90556-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="90556-110">Pokud není uvedeno jinak, žádné objekt vrácený musí být bezpečná pro vlákno, protože může sloužit jako singleton.</span><span class="sxs-lookup"><span data-stu-id="90556-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="90556-111">V mnoha případech objekt se vrátil v takovém případě je objekt pro vytváření samotný objekt factory musí být bezpečná pro vlákno, ale objekt vrácený objekt pro vytváření nemusí být bezpečné pro vlákna, protože je požadována novou instanci z objekt factory pro každé použití.</span><span class="sxs-lookup"><span data-stu-id="90556-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="90556-112">Tento článek neobsahuje všechny podrobnosti o tom, jak implementovat IDbDependencyResolver, ale místo toho funguje jako reference pro pro které EF volá GetService a sémantika klíčový objekt pro každou z těchto typů služeb (to znamená, typy rozhraní a základní třídy) volání.</span><span class="sxs-lookup"><span data-stu-id="90556-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="90556-113">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="90556-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="90556-114">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-115">**Objekt vrácený**: databáze inicializátor pro typ daný kontext</span><span class="sxs-lookup"><span data-stu-id="90556-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="90556-116">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="90556-117">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="90556-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="90556-118">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="90556-119">**Objekt vrácený**: objekt pro vytváření vytvořit generátor SQL, který lze použít pro migraci a další akce, které způsobují databázi vytvořit, jako je vytvoření databáze s inicializátory databáze.</span><span class="sxs-lookup"><span data-stu-id="90556-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="90556-120">**Klíč**: řetězec obsahující název výchozí poskytovatel ADO.NET určující typ databáze, pro který se vygeneruje SQL.</span><span class="sxs-lookup"><span data-stu-id="90556-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="90556-121">Generátor SQL Server SQL je například vrátil pro klíč "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="90556-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-122">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="90556-123">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="90556-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="90556-124">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-125">**Objekt vrácený**: The EF zprostředkovatele má být použit pro výchozí název daného zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="90556-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="90556-126">**Klíč**: řetězec obsahující název výchozí poskytovatel ADO.NET určující typ databáze, pro kterou je potřeba zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="90556-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="90556-127">Zprostředkovatel SQL Server je třeba vrátil pro klíč "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="90556-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-128">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="90556-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="90556-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="90556-130">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-131">**Objekt vrácený**: objekt factory připojení, který se použije při EF vytvoří připojení k databázi podle konvence.</span><span class="sxs-lookup"><span data-stu-id="90556-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="90556-132">To znamená, když žádné připojení nebo připojovací řetězec je přidělena EF a žádný připojovací řetězec najdete v souboru app.config nebo web.config, pak tato služba se používá k vytvoření připojení podle konvence.</span><span class="sxs-lookup"><span data-stu-id="90556-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="90556-133">Změna objekt factory připojení můžete povolit EF použít jiný typ databáze (například SQL Server Compact Edition) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="90556-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="90556-134">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-135">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="90556-136">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="90556-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="90556-137">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-138">**Objekt vrácený**: služba, která můžete vygenerovat token manifestu zprostředkovatele připojení.</span><span class="sxs-lookup"><span data-stu-id="90556-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="90556-139">Tato služba se obvykle používá dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="90556-139">This service is typically used in two ways.</span></span> <span data-ttu-id="90556-140">Nejprve ji je možné vyhnout Code First při vytváření modelu s připojením k databázi.</span><span class="sxs-lookup"><span data-stu-id="90556-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="90556-141">Za druhé to je možné vynutit Code First pro vytvoření modelu pro konkrétní databázi verze – například k vynucení modelu pro SQL Server 2005, i v případě, že někdy se používá SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="90556-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="90556-142">**Doba života objektu**: Singleton – stejný objekt mohou být používané více časy a současně v různých vláknech</span><span class="sxs-lookup"><span data-stu-id="90556-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="90556-143">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="90556-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="90556-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="90556-145">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-146">**Objekt vrácený**: služba, která může získat továrnu poskytovatele z dané připojení.</span><span class="sxs-lookup"><span data-stu-id="90556-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="90556-147">V rozhraní .NET 4.5 je poskytovatel veřejně přístupná z připojení.</span><span class="sxs-lookup"><span data-stu-id="90556-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="90556-148">Výchozí implementace této služby na rozhraní .NET 4 používá některé z heuristických metod k nalezení odpovídající zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="90556-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="90556-149">Pokud se to nepovede pak tuto službu novou implementaci lze registrovat zajistit odpovídající řešení.</span><span class="sxs-lookup"><span data-stu-id="90556-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="90556-150">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="90556-151">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="90556-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="90556-152">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-153">**Objekt vrácený**: objekt factory, který vygeneruje klíč mezipaměti modelu pro daný kontext.</span><span class="sxs-lookup"><span data-stu-id="90556-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="90556-154">Ve výchozím nastavení EF ukládá do mezipaměti jeden model podle typu DbContext na poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="90556-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="90556-155">Jinou implementaci této služby je možné přidat další informace, jako je například název schématu do klíče mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="90556-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="90556-156">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="90556-157">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="90556-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="90556-158">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-159">**Objekt vrácený**: EF prostorových zprostředkovatele, který přidává podporu pro základní EF poskytovatele podle zeměpisného umístění a geometrie prostorové typy.</span><span class="sxs-lookup"><span data-stu-id="90556-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="90556-160">**Klíč**: DbSptialServices se zobrazí výzva pro dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="90556-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="90556-161">První, specifickým pro zprostředkovatele prostorových služeb jsou požadovány pomocí objektu DbProviderInfo (obsahující invariantní vzhledem k názvu a manifestu token) jako klíč.</span><span class="sxs-lookup"><span data-stu-id="90556-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="90556-162">Za druhé DbSpatialServices můžete požádat bez klíče.</span><span class="sxs-lookup"><span data-stu-id="90556-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="90556-163">To se používá k překladu "globální prostorových poskytovateli" který se používá při vytváření samostatných DbGeography nebo DbGeometry typů.</span><span class="sxs-lookup"><span data-stu-id="90556-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-164">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="90556-165">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="90556-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="90556-166">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-167">**Objekt vrácený**: objekt factory pro vytvoření služby, který umožňuje poskytovateli implementovat opakování nebo jiné chování při spuštění dotazů a příkazů na databázi.</span><span class="sxs-lookup"><span data-stu-id="90556-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="90556-168">Pokud je k dispozici žádná implementace, pak EF jednoduše spustí příkazy a šířit všechny výjimky vyvolané.</span><span class="sxs-lookup"><span data-stu-id="90556-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="90556-169">Tato služba se pro SQL Server používá k poskytování zásady opakování, který je obzvláště užitečná při spouštění cloudové databázové servery, jako je například SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="90556-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="90556-170">**Klíč**: ExecutionStrategyKey objekt, který obsahuje výchozí název zprostředkovatele a volitelně také název serveru, pro který se použije strategie provádění.</span><span class="sxs-lookup"><span data-stu-id="90556-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-171">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="90556-172">Func < DbConnection, řetězec, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="90556-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="90556-173">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-174">**Objekt vrácený**: objekt factory, který umožňuje poskytovateli ke konfiguraci mapování HistoryContext k `__MigrationHistory` tabulky používané migrace EF.</span><span class="sxs-lookup"><span data-stu-id="90556-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="90556-175">HistoryContext je první DbContext kódu a lze nakonfigurovat pomocí rozhraní fluent API pro běžné věci, jako je název tabulky a specifikace mapování sloupce změnit.</span><span class="sxs-lookup"><span data-stu-id="90556-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="90556-176">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-177">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="90556-178">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="90556-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="90556-179">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-180">**Objekt vrácený**: zprostředkovatele ADO.NET pro výchozí název daného zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="90556-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="90556-181">**Klíč**: řetězec obsahující výchozí název zprostředkovatele ADO.NET</span><span class="sxs-lookup"><span data-stu-id="90556-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-182">Tato služba není obvykle přímo od změnila výchozí implementace používá normální registrace zprostředkovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="90556-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="90556-183">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="90556-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="90556-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="90556-185">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-186">**Objekt vrácený**: služba, která se používá k určení výchozí název zprostředkovatele pro daný typ DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="90556-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="90556-187">Výchozí implementace této služby používá registrace zprostředkovatele ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="90556-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="90556-188">To znamená, že pokud zprostředkovatele ADO.NET není zaregistrován běžným způsobem, protože probíhá DbProviderFactory řešení pomocí EF, pak bude také potřeba vyřešit tuto službu.</span><span class="sxs-lookup"><span data-stu-id="90556-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="90556-189">**Klíč**: The DbProviderFactory instance, pro kterou výchozí název je povinné.</span><span class="sxs-lookup"><span data-stu-id="90556-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="90556-190">Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.</span><span class="sxs-lookup"><span data-stu-id="90556-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="90556-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="90556-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="90556-192">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-193">**Objekt vrácený**: mezipaměť sestavení, které obsahují předem vygenerovaných zobrazení.</span><span class="sxs-lookup"><span data-stu-id="90556-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="90556-194">Chcete, aby EF vědět, která sestavení obsahovat předem vygenerovaných zobrazení bez provádění jakékoli zjišťování se obvykle používá můžou nahradit aktuální soubor.</span><span class="sxs-lookup"><span data-stu-id="90556-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="90556-195">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="90556-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="90556-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="90556-197">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-198">**Objekt vrácený**: služba používaná EF a pluralize množné číslo názvy.</span><span class="sxs-lookup"><span data-stu-id="90556-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="90556-199">Ve výchozím nastavení se používá k službě anglické pluralizace.</span><span class="sxs-lookup"><span data-stu-id="90556-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="90556-200">**Klíč**: není používána a bude mít hodnotu null</span><span class="sxs-lookup"><span data-stu-id="90556-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="90556-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="90556-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="90556-202">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="90556-203">**Vrácených objektů**: žádné zachycovacích dotazů, které by měly být zaregistrovány při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="90556-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="90556-204">Všimněte si, že tyto objekty jsou požadovány pomocí volání GetServices a budou všechny sběrače vrácený žádné překladače závislostí zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="90556-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="90556-205">**Klíč**: není používána a bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="90556-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="90556-206">Func < System.Data.Entity.DbContext, akce < řetězec\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="90556-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="90556-207">**Verze zavedené**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="90556-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="90556-208">**Objekt vrácený**: objekt factory, který se použije k vytvoření protokolu databáze formátovací modul, který bude používat, pokud kontext. Database.Log je hodnota nastavena na daném kontextu.</span><span class="sxs-lookup"><span data-stu-id="90556-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="90556-209">**Klíč**: není používána a bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="90556-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="90556-210">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="90556-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="90556-211">**Verze zavedené**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="90556-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="90556-212">**Objekt vrácený**: objekt factory, který se použije k vytvoření instance kontextu pro migrace kontextu nemá dostupný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="90556-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="90556-213">**Klíč**: typ objektu typu odvozené uvolněn objekt DbContext, pro kterou je potřeba objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="90556-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="90556-214">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="90556-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="90556-215">**Verze zavedené**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="90556-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="90556-216">**Objekt vrácený**: objekt factory, který se použije k vytvoření serializátory za účelem serializace silného typu vlastní poznámky tak, že je možné serializovat a desterilized do souboru XML pro použití v migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="90556-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="90556-217">**Klíč**: název poznámky, je serializován nebo deserializován.</span><span class="sxs-lookup"><span data-stu-id="90556-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="90556-218">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="90556-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="90556-219">**Verze zavedené**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="90556-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="90556-220">**Objekt vrácený**: objekt factory, který se použije k vytvoření obslužné rutiny pro transakce, tak, aby zvláštní zpracování lze použít v situacích, jako je zpracování selhání potvrzení.</span><span class="sxs-lookup"><span data-stu-id="90556-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="90556-221">**Klíč**: ExecutionStrategyKey objekt, který obsahuje výchozí název zprostředkovatele a volitelně také název serveru, pro který se použije obslužnou rutinu transakce.</span><span class="sxs-lookup"><span data-stu-id="90556-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
