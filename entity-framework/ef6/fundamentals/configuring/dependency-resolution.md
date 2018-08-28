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
# <a name="dependency-resolution"></a>Řešení závislostí
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Spouští se s EF6, Entity Framework obsahuje mechanismus pro obecné účely pro získání implementace služeb, které jsou potřeba. To znamená když EF používá instanci některé rozhraní nebo základní požádá o zadání na konkrétní implementace rozhraní nebo základní třídy použít. Toho můžete dosáhnout pomocí rozhraní IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Metoda GetService je obvykle volána EF a zařizuje služba implementace IDbDependencyResolver EF nebo aplikace k dispozici. Při volání, typ argumentu je typ rozhraní nebo základní třídy požadované služby a klíče objektu je buď null, nebo objekt, který poskytuje kontextové informace o požadovanou službu.  

Tento článek neobsahuje všechny podrobnosti o tom, jak implementovat IDbDependencyResolver, ale místo toho funguje jako reference pro pro které EF volá GetService a sémantika klíčový objekt pro každou z těchto typů služeb (to znamená, typy rozhraní a základní třídy) volání. Tento dokument se zachová aktuální při přidání dalších služeb.  

## <a name="services-resolved"></a>Služby Vyřešeno  

Pokud není uvedeno jinak, žádné objekt vrácený musí být bezpečná pro vlákno, protože může sloužit jako singleton. V mnoha případech objekt se vrátil v takovém případě je objekt pro vytváření samotný objekt factory musí být bezpečná pro vlákno, ale objekt vrácený objekt pro vytváření nemusí být bezpečné pro vlákna, protože je požadována novou instanci z objekt factory pro každé použití.  

### <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: databáze inicializátor pro typ daný kontext  

**Klíč**: není používána a bude mít hodnotu null  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Verze zavedené**: EF6.0.0

**Objekt vrácený**: objekt pro vytváření vytvořit generátor SQL, který lze použít pro migraci a další akce, které způsobují databázi vytvořit, jako je vytvoření databáze s inicializátory databáze.  

**Klíč**: řetězec obsahující název výchozí poskytovatel ADO.NET určující typ databáze, pro který se vygeneruje SQL. Generátor SQL Server SQL je například vrátil pro klíč "System.Data.SqlClient".  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: The EF zprostředkovatele má být použit pro výchozí název daného zprostředkovatele  

**Klíč**: řetězec obsahující název výchozí poskytovatel ADO.NET určující typ databáze, pro kterou je potřeba zprostředkovatele. Zprostředkovatel SQL Server je třeba vrátil pro klíč "System.Data.SqlClient".  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: objekt factory připojení, který se použije při EF vytvoří připojení k databázi podle konvence. To znamená, když žádné připojení nebo připojovací řetězec je přidělena EF a žádný připojovací řetězec najdete v souboru app.config nebo web.config, pak tato služba se používá k vytvoření připojení podle konvence. Změna objekt factory připojení můžete povolit EF použít jiný typ databáze (například SQL Server Compact Edition) ve výchozím nastavení.  

**Klíč**: není používána a bude mít hodnotu null  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: služba, která můžete vygenerovat token manifestu zprostředkovatele připojení. Tato služba se obvykle používá dvěma způsoby. Nejprve ji je možné vyhnout Code First při vytváření modelu s připojením k databázi. Za druhé to je možné vynutit Code First pro vytvoření modelu pro konkrétní databázi verze – například k vynucení modelu pro SQL Server 2005, i v případě, že někdy se používá SQL Server 2008.  

**Doba života objektu**: Singleton – stejný objekt mohou být používané více časy a současně v různých vláknech  

**Klíč**: není používána a bude mít hodnotu null  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: služba, která může získat továrnu poskytovatele z dané připojení. V rozhraní .NET 4.5 je poskytovatel veřejně přístupná z připojení. Výchozí implementace této služby na rozhraní .NET 4 používá některé z heuristických metod k nalezení odpovídající zprostředkovatele. Pokud se to nepovede pak tuto službu novou implementaci lze registrovat zajistit odpovídající řešení.  

**Klíč**: není používána a bude mít hodnotu null  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: objekt factory, který vygeneruje klíč mezipaměti modelu pro daný kontext. Ve výchozím nastavení EF ukládá do mezipaměti jeden model podle typu DbContext na poskytovatele. Jinou implementaci této služby je možné přidat další informace, jako je například název schématu do klíče mezipaměti.  

**Klíč**: není používána a bude mít hodnotu null  

### <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: EF prostorových zprostředkovatele, který přidává podporu pro základní EF poskytovatele podle zeměpisného umístění a geometrie prostorové typy.  

**Klíč**: DbSptialServices se zobrazí výzva pro dvěma způsoby. První, specifickým pro zprostředkovatele prostorových služeb jsou požadovány pomocí objektu DbProviderInfo (obsahující invariantní vzhledem k názvu a manifestu token) jako klíč. Za druhé DbSpatialServices můžete požádat bez klíče. To se používá k překladu "globální prostorových poskytovateli" který se používá při vytváření samostatných DbGeography nebo DbGeometry typů.  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: objekt factory pro vytvoření služby, který umožňuje poskytovateli implementovat opakování nebo jiné chování při spuštění dotazů a příkazů na databázi. Pokud je k dispozici žádná implementace, pak EF jednoduše spustí příkazy a šířit všechny výjimky vyvolané. Tato služba se pro SQL Server používá k poskytování zásady opakování, který je obzvláště užitečná při spouštění cloudové databázové servery, jako je například SQL Azure.  

**Klíč**: ExecutionStrategyKey objekt, který obsahuje výchozí název zprostředkovatele a volitelně také název serveru, pro který se použije strategie provádění.  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, řetězec, System.Data.Entity.Migrations.History.HistoryContext\>  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: objekt factory, který umožňuje poskytovateli ke konfiguraci mapování HistoryContext k `__MigrationHistory` tabulky používané migrace EF. HistoryContext je první DbContext kódu a lze nakonfigurovat pomocí rozhraní fluent API pro běžné věci, jako je název tabulky a specifikace mapování sloupce změnit.  

**Klíč**: není používána a bude mít hodnotu null  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: zprostředkovatele ADO.NET pro výchozí název daného zprostředkovatele.  

**Klíč**: řetězec obsahující výchozí název zprostředkovatele ADO.NET  

>[!NOTE]
> Tato služba není obvykle přímo od změnila výchozí implementace používá normální registrace zprostředkovatele ADO.NET. Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: služba, která se používá k určení výchozí název zprostředkovatele pro daný typ DbProviderFactory. Výchozí implementace této služby používá registrace zprostředkovatele ADO.NET. To znamená, že pokud zprostředkovatele ADO.NET není zaregistrován běžným způsobem, protože probíhá DbProviderFactory řešení pomocí EF, pak bude také potřeba vyřešit tuto službu.  

**Klíč**: The DbProviderFactory instance, pro kterou výchozí název je povinné.  

>[!NOTE]
> Další podrobnosti týkající se poskytovatele služeb v EF6 najdete [modelu EF6 poskytovatele](~/ef6/fundamentals/providers/provider-model.md) části.  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: mezipaměť sestavení, které obsahují předem vygenerovaných zobrazení. Chcete, aby EF vědět, která sestavení obsahovat předem vygenerovaných zobrazení bez provádění jakékoli zjišťování se obvykle používá můžou nahradit aktuální soubor.  

**Klíč**: není používána a bude mít hodnotu null  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: služba používaná EF a pluralize množné číslo názvy. Ve výchozím nastavení se používá k službě anglické pluralizace.  

**Klíč**: není používána a bude mít hodnotu null  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Verze zavedené**: EF6.0.0

**Vrácených objektů**: žádné zachycovacích dotazů, které by měly být zaregistrovány při spuštění aplikace. Všimněte si, že tyto objekty jsou požadovány pomocí volání GetServices a budou všechny sběrače vrácený žádné překladače závislostí zaregistrovaný.

**Klíč**: není používána a bude mít hodnotu null.  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System.Data.Entity.DbContext, akce < řetězec\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Verze zavedené**: EF6.0.0  

**Objekt vrácený**: objekt factory, který se použije k vytvoření protokolu databáze formátovací modul, který bude používat, pokud kontext. Database.Log je hodnota nastavena na daném kontextu.  

**Klíč**: není používána a bude mít hodnotu null.  

### <a name="funcsystemdataentitydbcontext"></a>Func < System.Data.Entity.DbContext\>  

**Verze zavedené**: EF6.1.0  

**Objekt vrácený**: objekt factory, který se použije k vytvoření instance kontextu pro migrace kontextu nemá dostupný konstruktor bez parametrů.  

**Klíč**: typ objektu typu odvozené uvolněn objekt DbContext, pro kterou je potřeba objekt pro vytváření.  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Verze zavedené**: EF6.1.0  

**Objekt vrácený**: objekt factory, který se použije k vytvoření serializátory za účelem serializace silného typu vlastní poznámky tak, že je možné serializovat a desterilized do souboru XML pro použití v migrace Code First.  

**Klíč**: název poznámky, je serializován nebo deserializován.  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Verze zavedené**: EF6.1.0  

**Objekt vrácený**: objekt factory, který se použije k vytvoření obslužné rutiny pro transakce, tak, aby zvláštní zpracování lze použít v situacích, jako je zpracování selhání potvrzení.  

**Klíč**: ExecutionStrategyKey objekt, který obsahuje výchozí název zprostředkovatele a volitelně také název serveru, pro který se použije obslužnou rutinu transakce.  
