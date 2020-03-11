---
title: Model poskytovatele Entity Framework 6 – EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 8bda3f51e8934f2add862c30e60f1185f068c515
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419358"
---
# <a name="the-entity-framework-6-provider-model"></a>Model poskytovatele Entity Framework 6

Model poskytovatele Entity Framework povoluje použití Entity Framework s různými typy databázového serveru. Například jeden poskytovatel může být napájen do sítě, aby bylo možné použít EF pro Microsoft SQL Server, zatímco jiný poskytovatel může být připojen do, aby bylo možné použít EF v Microsoft SQL Server Compact Edition. Poskytovatelé pro EF6, o kterých jsme se dozvěděli, najdete na stránce [poskytovatelé Entity Framework](~/ef6/fundamentals/providers/index.md) .

Byly vyžadovány určité změny, jak EF spolupracuje s poskytovateli, aby bylo možné vydat EF v rámci licence Open Source. Tyto změny vyžadují nové sestavení zprostředkovatelů EF proti sestavením EF6 spolu s novými mechanismy pro registraci zprostředkovatele.

## <a name="rebuilding"></a>Nové sestavení

EF6 je nyní součástí základního kódu, který byl dřív součástí .NET Framework, jako sestavení mimo pásmo (OOB). Podrobnosti o tom, jak sestavovat aplikace pro EF6, najdete na stránce [aktualizace aplikací pro EF6](~/ef6/what-is-new/upgrading-to-ef6.md) . V těchto pokynech bude také potřeba znovu sestavit poskytovatele.

## <a name="provider-types-overview"></a>Přehled typů zprostředkovatelů

Zprostředkovatel EF je ve skutečnosti kolekce služeb specifických pro poskytovatele definovaných typy CLR, které tyto služby rozšířily (pro základní třídu) nebo implementující (pro rozhraní). Dvě z těchto služeb jsou zásadní a je nezbytné, aby EF fungovalo vůbec. Jiné jsou volitelné a je potřeba je implementovat jenom v případě, že je potřeba zadat konkrétní funkčnost a/nebo výchozí implementace těchto služeb nefungují pro konkrétní cílový databázový server.

## <a name="fundamental-provider-types"></a>Základní typy zprostředkovatelů

### <a name="dbproviderfactory"></a>DbProviderFactory

EF závisí na typu odvozeném z typu [System. data. Common. DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) pro provádění všech přístupů k databázi nízké úrovně. DbProviderFactory není ve skutečnosti součástí EF, ale je to třída v .NET Framework, která slouží vstupnímu bodu pro poskytovatele ADO.NET, který může být použit v EF, jiné O/RMs nebo přímo v aplikaci k získání instancí připojení, příkazů, parametrů a dalších ADO.NET abstrakcí v poskytovateli nezávislá. Další informace o DbProviderFactory najdete v [dokumentaci MSDN pro ADO.NET](https://msdn.microsoft.com/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

EF závisí na typu odvozeném od DbProviderServices pro poskytování dalších funkcí požadovaných v EF nad funkčnost, kterou už poskytuje poskytovatel ADO.NET. Ve starších verzích EF byla třída DbProviderServices součástí .NET Framework a byla nalezena v oboru názvů System. data. Common. Počínaje EF6 Tato třída je teď součástí EntityFramework. dll a je v oboru názvů System. data. entity. Core. Common.

Další podrobnosti o základních funkcích implementace DbProviderServices najdete na [webu MSDN](https://msdn.microsoft.com/library/ee789835.aspx). Upozorňujeme však, že od doby psaní těchto informací není pro EF6 aktualizováno, i když je většina konceptů stále platná. SQL Server a SQL Server Compact implementace DbProviderServices jsou také vráceny do [Open Source základu kódu](https://github.com/aspnet/EntityFramework6/) a mohou sloužit jako užitečné odkazy pro jiné implementace.

Ve starších verzích EF byla implementace DbProviderServices k použití získána přímo od poskytovatele ADO.NET. To bylo provedeno přetypováním DbProviderFactory na IServiceProvider a voláním metody GetService. Tím se úzce propojí poskytovatel EF s DbProviderFactory. Tento spojovací bod zablokoval EF z .NET Framework, a proto pro EF6 bylo odebráno toto těsné propojení a implementace DbProviderServices je nyní registrována přímo v konfiguračním souboru aplikace nebo v konfiguraci založené na kódu, jak je popsáno dále v části Další informace níže v části _registrace DbProviderServices_ .

## <a name="additional-services"></a>Další služby

Kromě základních služeb popsaných výše je k dispozici také mnoho dalších služeb používaných v rámci EF, které jsou buď vždy, nebo někdy specifické pro konkrétního poskytovatele. Výchozí implementace těchto služeb pro konkrétního zprostředkovatele může poskytovat implementace DbProviderServices. Aplikace mohou také přepsat implementace těchto služeb nebo poskytovat implementace, když typ DbProviderServices neposkytuje výchozí hodnotu. Tato informace je podrobněji popsána v části _řešení dalších služeb_ níže.

Níže jsou uvedené další typy služeb, které poskytovatel může přijímat k poskytovateli. Další podrobnosti o jednotlivých typech služeb najdete v dokumentaci k rozhraní API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Jedná se o volitelnou službu, která umožňuje zprostředkovateli implementovat opakované pokusy nebo jiné chování při spuštění dotazů a příkazů na databázi. Pokud není k dispozici žádná implementace, pak EF jednoduše provede příkazy a rozšíří všechny vyvolané výjimky. Pro SQL Server se tato služba používá k poskytnutí zásady opakování, která je obzvláště užitečná při spuštění na cloudových databázových serverech, jako je SQL Azure.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Toto je volitelná služba, která umožňuje poskytovateli vytvářet DbConnection objekty podle konvence, pokud je zadaný jenom název databáze. Všimněte si, že zatímco tuto službu je možné vyřešit pomocí implementace DbProviderServices, která je přítomna od EF 4,1 a lze ji také explicitně nastavit buď v konfiguračním souboru, nebo v kódu. Poskytovatel bude mít možnost tuto službu vyřešit jenom v případě, že je zaregistrovaná jako výchozí zprostředkovatel (viz _výchozí zprostředkovatel_ níže) a pokud výchozí objekt pro vytváření připojení není nastavený jinde.

### <a name="dbspatialservices"></a>DbSpatialServices

Toto jsou volitelné služby, které poskytovateli umožňují přidat podporu pro geografické a geometrické prostorové typy. Aby aplikace mohla používat EF s prostorovými typy, musí být dodána implementace této služby. DbSptialServices se vyzve dvěma způsoby. Nejprve jsou požadovány prostorové služby specifické pro poskytovatele pomocí objektu DbProviderInfo (který obsahuje neutrální název a token manifestu) jako klíč. Druhý DbSpatialServices může být požádán o bez klíče. Slouží k vyřešení "globálního poskytovatele prostorového přístupu", který se používá při vytváření samostatných typů DbGeography nebo DbGeometry.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Toto je volitelná služba, která umožňuje použití migrace EF pro generování SQL používaného při vytváření a úpravách schémat databáze Code First. Aby bylo možné podporovat migrace, je vyžadována implementace. Pokud je k dispozici implementace, bude použita také při vytváření databází pomocí inicializátorů databáze nebo metody Database. Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, String, HistoryContextFactory >

Toto je volitelná služba, která umožňuje poskytovateli nakonfigurovat mapování HistoryContext k tabulce `__MigrationHistory` používané migracemi EF. HistoryContext je Code First DbContext a dá se nakonfigurovat pomocí normálního rozhraní API Fluent, aby se změnily položky, jako je název tabulky a specifikace mapování sloupců. Výchozí implementace této služby vracené EF pro všechny poskytovatele můžou fungovat pro daný databázový server, pokud jsou všechna výchozí mapování tabulek a sloupců podporovaná zprostředkovatelem. V takovém případě poskytovatel nemusí poskytovat implementaci této služby.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Toto je volitelná služba pro získání správného DbProviderFactory z daného objektu DbConnection. Výchozí implementace této služby vracené EF pro všechny poskytovatele je určena pro práci pro všechny poskytovatele. Při spuštění v rozhraní .NET 4 ale DbProviderFactory není veřejně přístupný z jednoho, pokud jeho DbConnections. Proto EF používá některé heuristické metody k vyhledání shody s registrovanými zprostředkovateli. Je možné, že u některých zprostředkovatelů tyto heuristické služby selžou a v takových situacích by měl poskytovatel dodat novou implementaci.

## <a name="registering-dbproviderservices"></a>Registrace DbProviderServices

Implementaci DbProviderServices k použití lze zaregistrovat buď v konfiguračním souboru aplikace (App. config nebo Web. config), nebo pomocí konfigurace založené na kódu. V obou případech registrace používá jako klíč "neutrální název" poskytovatele. To umožňuje, aby bylo více poskytovatelů registrováno a použito v jediné aplikaci. Neutrální název použitý pro registrace EF je stejný jako neutrální název použitý pro registraci poskytovatele ADO.NET a připojovací řetězce. Například pro SQL Server je použit neutrální název System. data. SqlClient.

### <a name="config-file-registration"></a>Registrace konfiguračního souboru

Typ DbProviderServices, který se má použít, se zaregistruje jako element provider v seznamu zprostředkovatelů oddílu entityFramework konfiguračního souboru aplikace. Příklad:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

Řetězec _typu_ musí být kvalifikovaný název typu sestavení DbProviderServices implementace, která se má použít.

### <a name="code-based-registration"></a>Registrace na základě kódu

Počínaje poskytovatelem EF6 lze také zaregistrovat pomocí kódu. To umožňuje, aby byl zprostředkovatel EF použit bez jakýchkoli změn konfiguračního souboru aplikace. Pro použití konfigurace na základě kódu by aplikace měla vytvořit třídu DbConfiguration, jak je popsáno v [dokumentaci konfigurace na základě kódu](https://msdn.com/data/jj680699). Konstruktor třídy DbConfiguration by pak měl volat SetProviderServices a zaregistrovat poskytovatele EF. Příklad:

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

Jak je uvedeno výše v oddílu _Přehled typů zprostředkovatelů_ , lze také použít třídu DbProviderServices k vyřešení dalších služeb. To je možné, protože DbProviderServices implementuje IDbDependencyResolver a každý registrovaný typ DbProviderServices se přidá jako výchozí překladač. Mechanismus IDbDpendencyResolver je podrobněji popsán v tématu [řešení závislostí](~/ef6/fundamentals/configuring/dependency-resolution.md). Není ale nutné porozumět všem konceptům této specifikace, aby bylo možné vyřešit další služby ve zprostředkovateli.

Nejběžnější způsob, jak pro poskytovatele přeložit další služby, je volat DbProviderServices. AddDependencyResolver pro každou službu v konstruktoru třídy DbProviderServices. Například SqlProviderServices (Provider EF pro SQL Server) má podobný kód jako při inicializaci:

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

Tento konstruktor používá následující pomocné třídy:

*   SingletonDependencyResolver: poskytuje jednoduchý způsob, jak přeložit služby typu Singleton – to znamená služby, pro které se stejná instance vrací pokaždé, když je volána metoda GetService. Přechodné služby se často registrují jako továrna s jedním objektem, který se použije k vytvoření přechodných instancí na vyžádání.
*   ExecutionStrategyResolver: překladač určený k vrácení IExecutionStrategy implementací.

Místo používání DbProviderServices. AddDependencyResolver je také možné přepsat DbProviderServices. GetService a přímo vyřešit další služby. Tato metoda bude volána, když EF potřebuje službu definovanou určitým typem a v některých případech pro daný klíč. Metoda by měla vrátit službu, pokud ji může, nebo vrátit hodnotu null, aby se odhlásila od vrácení služby, a místo toho povolit jinou třídu k jejímu vyřešení. Například pro vyřešení výchozího objektu pro vytváření připojení může kód v GetService vypadat přibližně takto:

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

### <a name="registration-order"></a>Objednávka registrace

V případě, že je v konfiguračním souboru aplikace registrováno více implementací DbProviderServices, budou přidány jako sekundární překladače v pořadí, v jakém jsou uvedeny. Vzhledem k tomu, že překladače se vždy přidávají na začátek sekundárního řetězu překladu, znamená to, že poskytovatel na konci seznamu bude mít možnost vyřešit závislosti před ostatními. (To se může zdát, že je to pro vás málo intuitivní, ale dává smysl, pokud si představte převzetí každého poskytovatele ze seznamu a jeho skládání nad stávajícími poskytovateli.)

Toto pořadí většinou nezáleží na tom, že většina služeb poskytovatele je specifická pro poskytovatele a je nastavená jako neutrální název zprostředkovatele. Pro služby, které nejsou nastavené podle neutrálního názvu poskytovatele nebo nějakého jiného klíče specifického pro poskytovatele, se služba vyřeší v závislosti na tomto pořadí. Pokud není třeba explicitně nastavené jinak v jiném místě, bude výchozí továrna připojení od nejvyššího poskytovatele v řetězu.

## <a name="additional-config-file-registrations"></a>Další registrace konfiguračního souboru

Některé další služby poskytovatele popsané výše jsou možné explicitně zaregistrovat přímo v konfiguračním souboru aplikace. V případě, že se to provede, místo cokoli vrácené metodou GetService implementace DbProviderServices se použije registrace v konfiguračním souboru.

### <a name="registering-the-default-connection-factory"></a>Registrace výchozího objektu pro vytváření připojení

Počínaje EF5 balíček NuGet EntityFramework automaticky zaregistroval buď objekt pro vytváření připojení SQL Express, nebo objekt pro vytváření připojení LocalDb v konfiguračním souboru.

Příklad:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

_Typ_ je kvalifikovaný název typu sestavení pro výchozí objekt pro vytváření připojení, který musí implementovat IDbConnectionFactory.

Doporučuje se, aby balíček NuGet poskytovatele při instalaci nastavil výchozí objekt pro vytváření připojení. Níže najdete _balíčky NuGet pro poskytovatele_ .

## <a name="additional-ef6-provider-changes"></a>Další změny zprostředkovatele EF6

### <a name="spatial-provider-changes"></a>Změny prostorového poskytovatele

Poskytovatelé, kteří podporují prostorové typy, teď musí implementovat některé další metody pro třídy, které se odvozují z DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

K dispozici jsou také nové asynchronní verze stávajících metod, které jsou doporučeny pro přepsání jako výchozí implementace delegátů na synchronní metody, a proto neprovádět asynchronně:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Nativní podpora pro vyčíslitelné. obsahuje

EF6 zavádí nový typ výrazu DbInExpression, který byl přidán pro řešení problémů s výkonem kolem použití vyčíslitelné. obsahuje v dotazech LINQ. Třída DbProviderManifest má novou virtuální metodu SupportsInExpression, která je volána nástrojem EF k určení, zda zprostředkovatel zpracovává nový typ výrazu. Pro zajištění kompatibility s existujícími implementacemi poskytovatele vrátí metoda hodnotu false. Pro zvýšení výhod tohoto vylepšení může poskytovatel EF6 přidat kód pro zpracování DbInExpression a přepsání SupportsInExpression pro vrácení hodnoty true. Instance třídy DbInExpression může být vytvořena voláním metody DbExpressionBuilder.In. Instance DbInExpression se skládá z DbExpression, obvykle představuje sloupec tabulky, a seznam DbConstantExpression pro otestování shody.

## <a name="nuget-packages-for-providers"></a>Balíčky NuGet pro poskytovatele

Jedním ze způsobů, jak poskytovatele EF6 zpřístupnit, je uvolnit ho jako balíček NuGet. Použití balíčku NuGet má následující výhody:

*   K přidání registrace poskytovatele do konfiguračního souboru aplikace je snadné použít NuGet.
*   V konfiguračním souboru je možné provést další změny pro nastavení výchozího objektu pro vytváření připojení, aby připojení vydaná konvencí používala registrovaného poskytovatele.
*   NuGet zpracovává Přidání přesměrování vazby, aby měl poskytovatel EF6 i nadále fungovat i po uvolnění nového balíčku EF.

Příkladem je balíček EntityFramework. SqlServerCompact, který je součástí [kódu základu Open Source](https://github.com/aspnet/entityframework6). Tento balíček poskytuje dobrou šablonu pro vytváření balíčků NuGet pro poskytovatele EF.

### <a name="powershell-commands"></a>Příkazy PowerShellu

Po nainstalování balíčku NuGet EntityFramework zaregistruje modul prostředí PowerShell, který obsahuje dva příkazy, které jsou velmi užitečné pro balíčky zprostředkovatele:

*   Add-EFProvider přidá do konfiguračního souboru cílového projektu novou entitu pro poskytovatele a ověří, zda je na konci seznamu registrovaných zprostředkovatelů.
*   Add-EFDefaultConnectionFactory buď přidá nebo aktualizuje registraci defaultConnectionFactory v konfiguračním souboru cílového projektu.

Oba tyto příkazy postarou o přidání oddílu entityFramework do konfiguračního souboru a přidání kolekce Providers v případě potřeby.

Je určeno, že tyto příkazy budou volány ze skriptu NuGet Install. ps1. Například instalace. ps1 pro poskytovatele SQL Compact vypadá nějak takto:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Další informace o těchto příkazech lze získat pomocí příkazu Get-Help v okně konzoly Správce balíčků.

## <a name="wrapping-providers"></a>Poskytovatelé zabalení

Poskytovatel pro zabalení je poskytovatel EF nebo ADO.NET, který zabalí stávajícího poskytovatele, aby ho rozšířil s jinými funkcemi, jako je například profilace nebo trasování. Poskytovatelé zabalení mohou být registrováni běžným způsobem, ale je často pohodlnější nastavit poskytovatele zabalení za běhu tím, že zachytí řešení služeb souvisejících s poskytovatelem. K tomu lze použít statickou událost OnLockingConfiguration třídy DbConfiguration.

OnLockingConfiguration se volá poté, co EF určí, kde bude získána všechna konfigurace EF pro doménu aplikace, ale předtím, než bude uzamčena pro použití. Při spuštění aplikace (před použitím EF) by aplikace měla zaregistrovat obslužnou rutinu události pro tuto událost. (Zvažujeme přidání podpory pro registraci této obslužné rutiny v konfiguračním souboru, ale to ještě není podporováno.) Obslužná rutina události by pak měla zavolat ReplaceService pro každou službu, která musí být zabalená.  

Například pro zabalení IDbConnectionFactory a DbProviderService by obslužná rutina něco podobného, měla by být registrována:

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

Služba, která byla vyřešena a měla by nyní být zabalena spolu s klíčem, který byl použit k překladu služby, se předává obslužné rutině. Obslužná rutina pak může tuto službu zabalit a nahradit vrácenou službu obálkovou verzí.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Řešení DbProviderFactory pomocí EF

DbProviderFactory je jedním ze základních typů poskytovatele, které vyžaduje EF, jak je popsáno výše v části _Přehled typů zprostředkovatelů_ . Jak už bylo zmíněno, není to typ EF a registrace obvykle není součástí konfigurace EF, ale je to místo normální registrace poskytovatele ADO.NET v souboru Machine. config nebo v konfiguračním souboru aplikace.

Navzdory tomu, že tento EF stále používá normální mechanismus rozlišení závislosti při hledání DbProviderFactory, který se má použít. Výchozí překladač používá pro konfigurační soubory normální registraci ADO.NET, takže je to obvykle transparentní. Z důvodu normálního mechanismu rozlišení závislosti se ale používá, že IDbDependencyResolver lze použít k překladu DbProviderFactory i v případě, že nebyla provedena normální registrace ADO.NET.

Řešení DbProviderFactory tímto způsobem má několik dopadů:

*   Aplikace používající konfiguraci na základě kódu může přidat volání do své třídy DbConfiguration k registraci příslušného DbProviderFactory. To je užitečné hlavně u aplikací, které nechtějí (nebo nemůžou) používat jakoukoli konfiguraci založenou na souborech.
*   Služba může být zabalená nebo nahrazená pomocí ReplaceService, jak je popsáno výše v části _poskytovatelé vybalení_ .
*   Teoreticky by implementace DbProviderServices mohla vyřešit DbProviderFactory.

Důležité je vzít v vědomí, že při provádění kterékoli z těchto akcí budou mít vliv pouze na vyhledávání DbProviderFactory podle EF. Další kód, který není EF, může stále očekávat, že poskytovatel ADO.NET se má zaregistrovat v normálním způsobu, a pokud se registrace nenalezne, může selhat. Z tohoto důvodu je obvykle lepší zaregistrovat DbProviderFactory v normálním ADO.NETm způsobu.

### <a name="related-services"></a>Související služby

Pokud je k vyřešení DbProviderFactory použit EF, pak by měl také vyřešit služby IProviderInvariantName a IDbProviderFactoryResolver.

IProviderInvariantName je služba, která se používá k určení neutrálního názvu poskytovatele pro daný typ DbProviderFactory. Výchozí implementace této služby používá registraci poskytovatele ADO.NET. To znamená, že pokud poskytovatel ADO.NET není zaregistrován v normálním způsobu, protože DbProviderFactory je vyřešen pomocí EF, bude také potřeba tuto službu vyřešit. Všimněte si, že překladač pro tuto službu je automaticky přidán při použití metody DbConfiguration. SetProviderFactory.

Jak je popsáno v části _Přehled typů zprostředkovatelů_ , se IDbProviderFactoryResolver používá k získání správného DbProviderFactory z daného objektu DbConnection. Výchozí implementace této služby při spuštění v rozhraní .NET 4 používá registraci poskytovatele ADO.NET. To znamená, že pokud poskytovatel ADO.NET není zaregistrován v normálním způsobu, protože DbProviderFactory je vyřešen pomocí EF, bude také potřeba tuto službu vyřešit.
