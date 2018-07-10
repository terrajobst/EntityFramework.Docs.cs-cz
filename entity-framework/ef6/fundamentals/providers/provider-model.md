---
title: Zprostředkovatel modelu Entity Framework 6 - EF6
author: divega
ms.date: 2018-06-27
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
caps.latest.revision: 3
ms.openlocfilehash: 49b655bdbe1b256b7de517edec84945d1ee06f79
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912145"
---
# <a name="the-entity-framework-6-provider-model"></a>Zprostředkovatel modelu Entity Framework 6

Zprostředkovatel modelu Entity Framework umožňuje Entity Framework, který se má použít s různými typy databázového serveru. Například jeden zprostředkovatel může být zapojené do elektrické zásuvky umožňující EF, který se má použít pro Microsoft SQL Server, zatímco jiný zprostředkovatel může být připojeno do umožňující EF, který se má použít pro Microsoft SQL Server Compact Edition. Zprostředkovatelé pro EF6, která víme o můžete najít na [zprostředkovatele Entity Framework](~/ef6/fundamentals/providers/index.md) stránky.

Některé změny byly zapotřebí způsobu, jakým EF spolupracuje s poskytovateli umožňující EF se vydávají se na základě licence open source. Tyto změny vyžadují opětovné sestavení zprostředkovatelů EF proti sestavení EF6 spolu s novou mechanismy pro registraci poskytovatele.

## <a name="rebuilding"></a>Opětovné sestavení

S EF6 základní kód, který byl dřív součástí rozhraní .NET Framework se nyní dodává jako out-of-band (OOB) sestavení. Podrobnosti o tom, jak vytvářet aplikace využívající EF6 můžete najít na [aktualizace aplikací pro EF6](~/ef6/what-is-new/upgrading-to-ef6.md) stránky. Poskytovatelé budete muset znovu vytvořit pomocí následujících pokynů.

## <a name="provider-types-overview"></a>Přehled zprostředkovatele typů

Poskytovatele EF je ve skutečnosti kolekce specifickým pro zprostředkovatele služeb definované typy CLR, které tyto služby rozšířit z (pro základní třídu) nebo implementovat (pro rozhraní). Dva z těchto služeb jsou základní a nezbytné pro EF vůbec fungovat. Jiné jsou volitelné a pouze potřeba je implementovat, pokud konkrétní funkce jsou nezbytné a/nebo výchozí implementace těchto služeb nefunguje pro konkrétní databázový server, který se cílí.

### <a name="fundamental-provider-types"></a>Základní poskytovatel typů

#### <a name="dbproviderfactory"></a>DbProviderFactory

EF závisí na typu odvozeného z [System.Data.Common.DbProviderFactory](http://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) pro provádění veškerý přístup nízké úrovně databáze. DbProviderFactory není ve skutečnosti součástí EF, ale místo toho je třída v rozhraní .NET Framework, která slouží jako vstupní bod pro poskytovatele ADO.NET, který je možné pomocí EF, jiné vstupně/RMS. Přitom nezáleží, nebo přímo prostřednictvím aplikace k získání instance připojení, příkazy, parametry a jiné technologie ADO.NET abstrakcí ve zprostředkovateli způsobem, který nezohledňuje. Další informace o DbProviderFactory najdete v [dokumentaci MSDN pro technologii ADO.NET](http://msdn.microsoft.com/en-us/library/a6cd7c08.aspx).

#### <a name="dbproviderservices"></a>DbProviderServices

EF závisí na typu odvozeného z DbProviderServices pro zajištění další funkce vyžadované EF nad funkce už poskytované zprostředkovateli ADO.NET. Ve starších verzích EF třída DbProviderServices byla součástí rozhraní .NET Framework a byla nalezena v oboru názvů System.Data.Common. Počínaje EF6 Tato třída je nyní součástí EntityFramework.dll a je v oboru názvů System.Data.Entity.Core.Common.

Další informace o základních funkcích DbProviderServices implementace najdete na [MSDN](http://msdn.microsoft.com/en-us/library/ee789835.aspx). Mějte však na paměti, že k datu zápisu tyto informace není aktualizován pro EF6 i když většina koncepty jsou stále platné. Implementace systému SQL Server a SQL Server Compact DbProviderServices se také kontroluje do [základu kódu open source](https://gihtub.com/aspnet/EntityFramework6/) a může sloužit jako užitečné odkazy pro jiné implementace.

Ve starších verzích EF byl získán implementace DbProviderServices k použití přímo z poskytovatele ADO.NET. To bylo provedeno přetypování DbProviderFactory na IServiceProvider a voláním metody GetService. Tento zprostředkovatel EF těsně spjat s DbProviderFactory. Toto párování blokovat EF přesouvaných mimo rozhraní .NET Framework a proto pro EF6 tento určitou úzkou svázanost byla odebrána a implementace DbProviderServices je teď zaregistrovaný přímo v konfiguračním souboru aplikace nebo na úrovni kódu konfigurace, jak je popsáno podrobněji _registrace DbProviderServices_ níže v části.

### <a name="additional-services"></a>Další služby

Kromě základních služeb popsaných výše existují také mnoho dalších služeb používaných v EF které jsou vždy nebo někdy specifickým pro zprostředkovatele. Implementací DbProviderServices lze je zadat výchozí implementace specifickým pro zprostředkovatele z těchto služeb. Aplikace můžete také přepsat implementace těchto služeb nebo poskytnout implementace, pokud typ DbProviderServices neposkytuje výchozí. To je popsáno podrobněji _řešení dalšími službami_ níže v části.

Typy další služby, které mohou být zprostředkovatele, abyste zprostředkovatele jsou uvedeny níže. Další podrobnosti o každém z těchto typů služeb najdete v dokumentaci k rozhraní API.

#### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Toto je volitelnou službu, která umožňuje poskytovateli implementovat opakování nebo jiné chování při spuštění dotazů a příkazů na databázi. Pokud je k dispozici žádná implementace, pak EF jednoduše spustí příkazy a šířit všechny výjimky vyvolané. Tato služba se pro SQL Server používá k poskytování zásady opakování, který je obzvláště užitečná při spouštění cloudové databázové servery, jako je například SQL Azure.

#### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Toto je volitelnou službu, která umožňuje poskytovateli k vytváření objektů DbConnection podle konvence při pouze název databáze. Pamatujte, že zatímco tato služba dá vyřešit implementací DbProviderServices byla k dispozici od verze EF 4.1 a můžete také explicitně nastavit v konfiguračním souboru nebo v kódu. Zprostředkovatel se zobrazí pouze příležitost dobře se vyřešit tuto službu, pokud je registrován jako výchozího zprostředkovatele (naleznete v tématu _výchozího zprostředkovatele_ níže) a pokud nebyl nastaven výchozí objekt factory připojení, jinde.

#### <a name="dbspatialservices"></a>DbSpatialServices

Toto je volitelné služby, které umožňuje poskytovateli a přidat podporu pro typy prostorových zeměpisné oblasti a geometry. Implementace této služby je nutné zadat v pořadí pro aplikace pomocí EF prostorové typy. DbSptialServices se zobrazí výzva pro dvěma způsoby. První, specifickým pro zprostředkovatele prostorových služeb jsou požadovány pomocí objektu DbProviderInfo (obsahující invariantní vzhledem k názvu a manifestu token) jako klíč. Za druhé DbSpatialServices můžete požádat bez klíče. To se používá k překladu "globální prostorových poskytovatele", který se používá při vytváření samostatných DbGeography nebo DbGeometry typů.

#### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Toto je volitelnou službu, která umožňuje migraci EF, který bude používán pro generování SQL použít při vytváření a úpravy databázových schématech Code First. Implementace je nutné pro podporu migrace. Pokud je k dispozici implementace pak také použije při vytváření databáze pomocí databáze inicializátory nebo Database.Create metody.

#### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, řetězec, HistoryContextFactory >

Toto je volitelnou službu, která umožňuje poskytovateli ke konfiguraci mapování HistoryContext k `__MigrationHistory` tabulky používané migrace EF. HistoryContext je první DbContext kódu a lze nakonfigurovat pomocí rozhraní fluent API pro běžné věci, jako je název tabulky a specifikace mapování sloupce změnit. Výchozí implementace této služby vrátil EF pro všemi zprostředkovateli může fungovat pro danou databázi serveru, pokud všechny výchozí sloupce a tabulky mapování podporovaných tímto poskytovatelem. V takovém případě zprostředkovatele není nutné poskytnout implementaci této služby.

#### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Toto je volitelnou službu pro získání správné DbProviderFactory ze zadaný objekt DbConnection. Výchozí implementace této služby vrátil EF pro všech zprostředkovatelů by měla fungovat pro všechny poskytovatele. Ale při spuštění v rozhraní .NET 4, DbProviderFactory není veřejně přístupná z jednoho pokud jeho DbConnections. Proto EF používá některé z heuristických metod k hledání registrovaných zprostředkovatelů pro vyhledání shody. Je možné, že pro někteří poskytovatelé tyto heuristiky selže a v takových situacích by mělo nabízet poskytovateli novou implementaci.

## <a name="registering-dbproviderservices"></a>Registrace DbProviderServices

Implementace DbProviderServices použití lze zaregistrovat, buď v konfiguračním souboru (app.config nebo web.config) nebo pomocí konfigurace založená na kódu vaší aplikace. V obou případech používá registraci poskytovatele "výchozím názvem" jako klíč. To umožňuje více poskytovatelů zaregistrované a použít v jedné aplikaci. Neutrální název použitý pro EF registrace je stejný jako výchozí název použitý pro registraci a připojovací řetězce zprostředkovatele ADO.NET. Například pro SQL Server výchozí název se používá "System.Data.SqlClient".

### <a name="config-file-registration"></a>Registrace konfiguračního souboru

Typ DbProviderServices má být použit se zaregistruje jako prvek poskytovatele v seznamu poskytovatelů entityFramework oddílu konfiguračního souboru aplikace. Příklad:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

_Typ_ řetězec musí být název typu kvalifikovaného pro sestavení implementace DbProviderServices používat.

### <a name="code-based-registration"></a>Registrace na úrovni kódu

Počínaje EF6 poskytovatelé lze také zaregistrovat pomocí kódu. To umožňuje na EF poskytovatele, který se dá používat bez změny do konfiguračního souboru aplikace. Použití konfigurace založená na kód aplikace by měl vytvořit třídu DbConfiguration jak je popsáno v [dokumentaci založený na kódu konfigurační](http://msdn.com/data/jj680699). Konstruktor třídy DbConfiguration by měly volat pak SetProviderServices zaregistrujte poskytovatele EF. Příklad:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Řešení dalších služeb

Jak je uvedeno výše v _poskytovatele typů přehled_ části, DbProviderServices třídy lze použít také k vyřešení dalších služeb. To je možné, protože implementuje DbProviderServices IDbDependencyResolver a každý registrovaný typ DbProviderServices se přidá jako "výchozí překladač". Mechanismus IDbDpendencyResolver je popsána podrobněji [řešení závislostí](~/ef6/fundamentals/configuring/dependency-resolution.md). Není však nutné koncepce všechny v téhle specifikaci k vyřešení dalších služeb ve zprostředkovateli.

Nejběžnější způsob pro poskytovatelem a vyřešit dalších služeb je volat pro každou službu v konstruktoru třídy DbProviderServices DbProviderServices.AddDependencyResolver. Například SqlProviderServices (EF provider pro SQL Server) je podobný inicializaci kód:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Tento konstruktor používá následující tříd pomocných rutin:

*   SingletonDependencyResolver: poskytuje jednoduchý způsob, jak vyřešit deklarace služeb typu Singleton – to znamená, služby, pro které stejnou instanci je vrácena pokaždé, když, že je volat GetService. Přechodné služby jsou často registrován jako objekt pro vytváření jednotlivý prvek, který se použije pro vytvoření přechodné instancí na vyžádání.
*   ExecutionStrategyResolver: překladače specifické pro vrácení IExecutionStrategy implementace.

Namísto použití DbProviderServices.AddDependencyResolver je také možné přepsat DbProviderServices.GetService a dalších služeb vyřešit přímo. Tato metoda bude volána, když EF potřebuje služba definovány dobou určitého typu a v některých případech se pro zadaný klíč. Metoda by měla vrátit služby, pokud můžete, nebo vrátit hodnotu null se odhlásit vrácení službu a místo toho bylo jiné třídy k jeho vyřešení. Například chcete-li vyřešit výchozí objekt pro vytváření připojení kódu v GetService může vypadat přibližně takto:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Registrace pořadí

Když více implementací DbProviderServices zaregistrovaní v konfiguračním souboru aplikace budou přidány jako sekundární překladače v pořadí, ve kterém jsou uvedené. Protože překladače jsou vždy přidány na začátek řetězce sekundární překladače, to znamená, že poskytovatel na konci seznamu mít šanci vyřešit závislosti než ostatní. (To se může zdát trochu counter-intuitive zpočátku, ale dává smysl, pokud si představíte, přičemž každý poskytovatel ze seznamu a skládání nad existující poskytovatelé.)

Toto uspořádání obvykle nebude vadit, protože většina poskytovatele služeb jsou specifické pro zprostředkovatele a s klíči ve výchozí název zprostředkovatele. Ale pro služby, které nejsou označenými pomocí výchozí název zprostředkovatele nebo některé jiné specifickým pro zprostředkovatele klíč, který se vyřeší služby založené na tomto pořadí. Například pokud není explicitně nastavena jinak někde jinde, pak výchozí objekt pro vytváření připojení budou přicházet z nejvyšší zprostředkovatele v řetězci.

## <a name="additional-config-file-registrations"></a>Další konfigurační soubor registrace

Je možné explicitně zaregistrovat některé další poskytovatele služeb bylo popsáno výše přímo v konfiguračním souboru aplikace. Když to se provádí registraci v konfiguračním souboru se použijí místo nic vrácený metodou GetService DbProviderServices implementace.

### <a name="registering-the-default-connection-factory"></a>Registruje výchozí objekt pro vytváření připojení

Počínaje EF5 balíček EntityFramework NuGet automaticky registrovaný objekt factory připojení SQL Express nebo objekt factory LocalDb připojení v konfiguračním souboru.

Příklad:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_Typ_ je název typu kvalifikovaného pro sestavení pro objekt factory připojení výchozí musí implementovat IDbConnectionFactory.

Doporučuje se, že balíček NuGet zprostředkovatele nastavit tímto způsobem při instalaci výchozí objekt factory připojení. Zobrazit _balíčky NuGet pro poskytovatele_ níže.

## <a name="additional-ef6-provider-changes"></a>Další změny poskytovatele EF6

### <a name="spatial-provider-changes"></a>Prostorový poskytovatel změny

Poskytovatelé, které podporují prostorové typy nyní musí implementovat některé další metody u tříd odvozených z DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Existují také nové asynchronní verze stávající metody, které se doporučují jako výchozí implementace delegovat na synchronní metody a proto se neprovedou asynchronně přepsání:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Nativní podpora pro Enumerable.Contains

EF6 zavádí nový typ výrazu DbInExpression, který se přidal k řešení potíží s výkonem kolem užívání Enumerable.Contains v dotazech LINQ. Třída DbProviderManifest má novou virtuální metodou, SupportsInExpression, které je voláno rozhraním EF k určení, pokud zprostředkovatel zpracovává nový typ výrazu. Z důvodu kompatibility s existující implementace poskytovatele metoda vrátí hodnotu false. Toto vylepšení využít, poskytovatele EF6 přidat kód pro zpracování DbInExpression a přepsání SupportsInExpression vrátit hodnotu true. Instance DbInExpression lze vytvořit voláním metody DbExpressionBuilder.In. DbInExpression instance se skládá z DbExpression, obvykle představuje sloupci tabulky a seznam DbConstantExpression testování shody.

## <a name="nuget-packages-for-providers"></a>Balíčky NuGet pro poskytovatele

Jedním ze způsobů zpřístupnit poskytovatele EF6 je verze jako balíček NuGet. Pomocí balíčku NuGet má následující výhody:

*   Je snadno použitelný NuGet pro přidání registrace poskytovatele do konfiguračního souboru aplikace
*   Další změny, které můžete provést do konfiguračního souboru k nastavení výchozí objekt pro vytváření připojení tak, aby provedené při vytváření připojení budou používat registrovaného zprostředkovatele
*   Obslužné rutiny NuGet tak, aby zprostředkovatel EF6 by měly být nadále fungovat i po vydání nového balíčku EF přidání přesměrování vazeb

Příkladem je EntityFramework.SqlServerCompact balíček, který je součástí [opensourcových codebase](http://github.com/aspnet/entityframework6). Tento balíček poskytuje dobré šablony pro vytváření balíčků NuGet EF zprostředkovatele.

### <a name="powershell-commands"></a>Příkazy prostředí PowerShell

Při instalaci balíčku EntityFramework NuGet registruje modul Powershellu, který obsahuje dva příkazy, které jsou velmi užitečné pro zprostředkovatele balíčky:

*   Přidat EFProvider přidá novou entitu pro zprostředkovatele v konfiguračním souboru na cílový projekt a je zajištěno, že je na konci seznamu registrovaných zprostředkovatelů.
*   Přidat EFDefaultConnectionFactory přidá nebo aktualizuje defaultConnectionFactory registrace v konfiguračním souboru na cílový projekt.

Oba tyto příkazy se postará o přidání oddíl entityFramework do konfiguračního souboru a přidání kolekce poskytovatelů v případě potřeby.

Předpokládá se, že tyto příkazy volat z NuGet skript install.ps1. Například install.ps1 SQL Compact poskytovatele vypadá podobně jako tento:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Další informace o těchto příkazech získáte pomocí get-help v okně konzoly Správce balíčků.

## <a name="wrapping-providers"></a>Zabalení zprostředkovatelů

Zabalení poskytovatele je poskytovatele EF a/nebo ADO.NET, která obaluje existujícího poskytovatele rozšíření s další funkce, jako je například profilace, nebo trasování funkce. Obtékání poskytovatelé mohou být registrovány běžným způsobem, ale často je vhodné nastavení poskytovatele obtékání za běhu zachycením řešení související poskytovatele služeb. Statické události OnLockingConfiguration DbConfiguration třídy lze použít k tomu.

OnLockingConfiguration je volána po EF určil, kde budou všechny EF konfigurace domény aplikace získané, ale než se uzamkne pro použití. Při spuštění aplikace (před použitím EF) by měla aplikace zaregistrovat obslužnou rutinu události pro tuto událost. (Jsme zvažuje přidání podpory pro registraci této obslužné rutiny v konfiguračním souboru, ale ještě to není podporováno.) Proveďte volání ReplaceService pro každou službu, která musí být zabaleny by měla obslužná rutina události.  

Například při zabalení IDbConnectionFactory a DbProviderService, by měly být zaregistrovány obslužnou rutinu podobný následujícímu:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

Služby, který byl vyřešen a teď by měl být uzavřen společně s klíčem, který se použil při překladu služby jsou předány do obslužné rutiny. Obslužná rutina můžete zabalit tuto službu a nahraďte zabalené verze vrácený služby.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Řešení DbProviderFactory s EF

DbProviderFactory je jeden z typů základní zprostředkovatel vyžadované EF, jak je popsáno v _poskytovatele typů přehled_ výše uvedené části. Které jsou už bylo zmíněno, není typem EF a registrace není obvykle jako součást konfigurace EF, ale místo toho je normální registrace zprostředkovatele ADO.NET v souboru machine.config nebo v konfiguračním souboru aplikace.

Bez ohledu na toto EF dál používá svůj mechanismus rozlišení normální závislostí při hledání DbProviderFactory používat. Výchozí překladač používá normální registrace technologie ADO.NET v konfiguračních souborech a proto je obvykle transparentní. Z důvodu řešení normální závislostí se používá mechanismus, ale znamená, že IDbDependencyResolver lze vyřešit DbProviderFactory i v případě, že normální ADO.NET registraci nebylo provedeno.

Tímto způsobem řešení DbProviderFactory má vliv na několik:

*   Aplikace pomocí konfigurace založená na kódu můžete přidat volání ve své třídě DbConfiguration k registraci DbProviderFactory odpovídající. To je užitečné pro aplikace, které nechcete povolit (nebo nemůže) ujistěte se, použít všechny konfigurace souborové vůbec.
*   Službě mohou být zabaleny nebo nahradit pomocí ReplaceService, jak je popsáno v _obtékání poskytovatelé_ výše uvedené části
*   Implementace DbProviderServices teoreticky by se dala přeložit DbProviderFactory.

Důležité si uvědomit o tom, jak žádnou z těchto akcí je, že se ovlivní pouze vyhledávání DbProviderFactory pomocí EF. Jiný kód bez EF stále očekávaným poskytovatele ADO.NET k registraci běžným způsobem a může selhat, pokud registrace nebyla nalezena. Z tohoto důvodu je obvykle vhodnější k běžným způsobem ADO.NET registraci DbProviderFactory.

### <a name="related-services"></a>Související služby

Pokud EF se používá k překladu DbProviderFactory, pak by se měl také vyřešit IProviderInvariantName a IDbProviderFactoryResolver služby.

IProviderInvariantName je služba, která se používá k určení výchozí název zprostředkovatele pro daný typ DbProviderFactory. Výchozí implementace této služby používá registrace zprostředkovatele ADO.NET. To znamená, že pokud zprostředkovatele ADO.NET není zaregistrován běžným způsobem, protože probíhá DbProviderFactory řešení pomocí EF, pak bude také potřeba vyřešit tuto službu. Všimněte si, že se při použití metody DbConfiguration.SetProviderFactory překladač pro tuto službu automaticky přidá.

Jak je popsáno v _poskytovatele typů přehled_ výše uvedené části, IDbProviderFactoryResolver slouží k získání správné DbProviderFactory z daný objekt DbConnection. Výchozí implementace této služby při spuštění v rozhraní .NET 4 používá registrace zprostředkovatele ADO.NET. To znamená, že pokud zprostředkovatele ADO.NET není zaregistrován běžným způsobem, protože probíhá DbProviderFactory řešení pomocí EF, pak bude také potřeba vyřešit tuto službu.
