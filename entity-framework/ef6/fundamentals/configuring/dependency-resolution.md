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
# <a name="dependency-resolution"></a>Řešení závislostí
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Počínaje verzí EF6 Entity Framework obsahuje obecný mechanizmus pro získání implementací služeb, které vyžaduje. To znamená, že když EF používá instanci některých rozhraní nebo základních tříd, požádá o konkrétní implementaci rozhraní nebo základní třídy, která se má použít. Toho lze dosáhnout použitím rozhraní IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Metoda GetService je obvykle volána nástrojem EF a je zpracována implementací IDbDependencyResolver poskytnutou buď v EF, nebo v aplikaci. Při volání je argumentem typu rozhraní nebo typ základní třídy požadované služby a objekt Key má buď hodnotu null, nebo objekt, který poskytuje kontextové informace o požadované službě.  

Pokud není uvedeno jinak, všechny vrácené objekty musí být bezpečné pro přístup z více vláken, protože je lze použít jako typ singleton. V mnoha případech je vrácen objekt factory, v takovém případě musí být samotný továrna bezpečná pro přístup z více vláken, ale objekt vrácený z továrny nemusí být bezpečný pro přístup z více vláken, protože z továrny je vyžádána nová instance pro každé použití.

Tento článek neobsahuje úplné podrobnosti o tom, jak implementovat IDbDependencyResolver, ale slouží jako odkaz na typy služeb (tj. rozhraní a typy základních tříd), pro které EF volá metodu GetService a sémantiku klíčového objektu pro každý z těchto volání.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System. data. entity. IDatabaseInitializer < TContext\>  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: inicializátor databáze pro daný typ kontextu  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System. data. entity. migrations. SQL. MigrationSqlGenerator\>  

**Představená verze**: EF 6.0.0

**Vrácen objekt**: továrna pro vytvoření generátoru SQL, který se dá použít pro migrace a další akce, které způsobují vytvoření databáze, jako je vytvoření databáze s Inicializátory databáze.  

**Key**: řetězec obsahující neutrální název poskytovatele ADO.NET určující typ databáze, pro kterou se bude generovat SQL. Například generátor SQL Server SQL se vrátí pro klíč "System. data. SqlClient".  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentitycorecommondbproviderservices"></a>System. data. entity. Core. Common. DbProviderServices  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: Zprostředkovatel EF, který má být použit pro daný neutrální název zprostředkovatele  

**Klíč**: řetězec obsahující neutrální název poskytovatele ADO.NET určující typ databáze, pro kterou je poskytovatel vyžadován. Například poskytovatel SQL Server je vrácen pro klíč "System. data. SqlClient".  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System. data. entity. Infrastructure. IDbConnectionFactory  

**Představená verze**: EF 6.0.0  

**Vrácený objekt: objekt**pro vytváření připojení, který se použije, když EF vytvoří připojení k databázi podle konvence. To znamená, že pokud se neudělí žádné připojení nebo připojovací řetězec k EF, a v souboru App. config nebo Web. config se nemůže najít připojovací řetězec, pak se tato služba používá k vytvoření připojení podle konvence. Změna objektu pro vytváření připojení může ve výchozím nastavení umožňovat, aby EF používal jiný typ databáze (například SQL Server Compact Edition).  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System. data. entity. Infrastructure. IManifestTokenService  

**Představená verze**: EF 6.0.0  

**Vrácený objekt**: služba, která může vygenerovat token manifestu zprostředkovatele z připojení. Tato služba se obvykle používá dvěma způsoby. Nejdřív se dá použít k tomu, abyste se vyhnuli Code First připojení k databázi při sestavování modelu. Za druhé, dá se použít k vynucení Code First vytvoření modelu pro konkrétní verzi databáze – například pro vynucení modelu SQL Server 2005 i v případě, že se někdy používá SQL Server 2008.  

**Doba života objektu**: singleton – stejný objekt může být použit několikrát a souběžně podle různých vláken.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System. data. entity. Infrastructure. IDbProviderFactoryService  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: služba, která může získat objekt pro vytváření poskytovatele z daného připojení. V rozhraní .NET 4,5 je poskytovatel veřejně přístupný z připojení. V rozhraní .NET 4 výchozí implementace této služby používá některé heuristické metody k nalezení odpovídajícího poskytovatele. V opačném případě je možné zaregistrovat novou implementaci této služby, aby poskytovala příslušné řešení.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System. data. entity. Infrastructure. IDbModelCacheKey\>  

**Představená verze**: EF 6.0.0  

**Vrácen objekt: objekt**pro vytváření, který vygeneruje klíč mezipaměti modelu pro daný kontext. EF ve výchozím nastavení ukládá do mezipaměti jeden model na DbContext typ na každého poskytovatele. Jinou implementaci této služby lze použít k přidání dalších informací, jako je například název schématu, do klíče mezipaměti.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="systemdataentityspatialdbspatialservices"></a>System. data. entity. prostor. DbSpatialServices  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: poskytovatel pro prostor EF, který přidává podporu základního poskytovatele EF pro geografické a geometrické prostorové typy.  

**Klíč**: DbSptialServices se vyzve dvěma způsoby. Nejprve jsou požadovány prostorové služby specifické pro poskytovatele pomocí objektu DbProviderInfo (který obsahuje neutrální název a token manifestu) jako klíč. Druhý DbSpatialServices může být požádán o bez klíče. Slouží k vyřešení "globálního poskytovatele prostorového přístupu", který se používá při vytváření samostatných typů DbGeography nebo DbGeometry.  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System. data. entity. Infrastructure. IDbExecutionStrategy\>  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: továrna pro vytvoření služby, která umožňuje zprostředkovateli implementovat opakování nebo jiné chování při spuštění dotazů a příkazů v databázi. Pokud není k dispozici žádná implementace, pak EF jednoduše provede příkazy a rozšíří všechny vyvolané výjimky. Pro SQL Server se tato služba používá k poskytnutí zásady opakování, která je obzvláště užitečná při spuštění na cloudových databázových serverech, jako je SQL Azure.  

**Key**: objekt ExecutionStrategyKey, který obsahuje neutrální název poskytovatele a volitelně název serveru, pro který se použije strategie provádění.  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, String, System. data. entity. migrations. History. HistoryContext\>  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: továrna, která umožňuje poskytovateli nakonfigurovat mapování HistoryContext na tabulku `__MigrationHistory`, kterou používají migrace EF. HistoryContext je Code First DbContext a dá se nakonfigurovat pomocí normálního rozhraní API Fluent, aby se změnily položky, jako je název tabulky a specifikace mapování sloupců.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdatacommondbproviderfactory"></a>System. data. Common. DbProviderFactory  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: poskytovatel ADO.NET, který má být použit pro daný neutrální název zprostředkovatele.  

**Klíč**: řetězec obsahující neutrální název poskytovatele ADO.NET  

>[!NOTE]
> Tato služba se obvykle nemění přímo, protože výchozí implementace používá normální registraci poskytovatele ADO.NET. Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System. data. entity. Infrastructure. IProviderInvariantName  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: služba, která slouží k určení neutrálního názvu poskytovatele pro daný typ DbProviderFactory. Výchozí implementace této služby používá registraci poskytovatele ADO.NET. To znamená, že pokud poskytovatel ADO.NET není zaregistrován v normálním způsobu, protože DbProviderFactory je vyřešen pomocí EF, bude také potřeba tuto službu vyřešit.  

**Key**: instance DbProviderFactory, pro kterou je vyžadován neutrální název.  

>[!NOTE]
> Další podrobnosti o službách souvisejících se zprostředkovatelem v EF6 najdete v části [model poskytovatele EF6](~/ef6/fundamentals/providers/provider-model.md) .  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System. data. entity. Core. Mapping. ViewGeneration. IViewAssemblyCache  

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: mezipaměť sestavení, která obsahují předem vygenerovaná zobrazení. Náhrada se obvykle používá k tomu, aby EF vědělo, která sestavení obsahují předem vygenerovaná zobrazení bez jakéhokoli zjištění.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System. data. entity. Infrastructure. plural. IPluralizationService

**Představená verze**: EF 6.0.0  

**Vrácen objekt**: služba, kterou používá EF k doplnit jednotné a množné číslo názvů. Ve výchozím nastavení se používá služba anglické plurality.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System. data. entity. Infrastructure. Intercept. IDbInterceptor  

**Představená verze**: EF 6.0.0

**Vrácené objekty**: jakékoli zachycení, které by mělo být registrováno při spuštění aplikace. Všimněte si, že tyto objekty jsou požadovány pomocí volání GetService a všechny zachycení vrácené všemi Překladači závislostí budou registrovány.

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System. data. entity. DbContext, Action < String\>, System. data. entity. Infrastructure. Intercept. DatabaseLogFormatter\>  

**Představená verze**: EF 6.0.0  

**Vrácen objekt: objekt**pro vytváření, který bude použit k vytvoření formátovacího modulu protokolu databáze, který bude použit v případě kontextu. Vlastnost Database. log je nastavena pro daný kontext.  

**Klíč**: nepoužívá se; bude mít hodnotu null.  

## <a name="funcsystemdataentitydbcontext"></a>Func < System. data. entity. DbContext\>  

**Představená verze**: EF 6.1.0  

**Vrácený objekt: objekt**pro vytváření, který se použije k vytvoření kontextových instancí pro migrace, když kontext nemá přístupný konstruktor bez parametrů.  

**Key**: typ objektu pro typ odvozeného DbContext, pro který je potřeba objekt pro vytváření.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System. data. entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\>  

**Představená verze**: EF 6.1.0  

**Vrácen objekt**: továrna, která bude použita k vytvoření serializátorů pro serializaci vlastních poznámek silně typovaného typu tak, aby mohly být serializovány a odsterilizovány do XML pro použití v migrace Code First.  

**Key**: název poznámky, která je serializována nebo deserializována.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System. data. entity. Infrastructure. TransactionHandler\>  

**Představená verze**: EF 6.1.0  

**Vrácen objekt**: továrna, která bude použita k vytváření obslužných rutin pro transakce, aby bylo možné použít speciální zpracování pro situace, jako je například zpracování selhání potvrzení.  

**Key**: objekt ExecutionStrategyKey, který obsahuje neutrální název poskytovatele a volitelně název serveru, pro který se použije obslužná rutina transakce.  
