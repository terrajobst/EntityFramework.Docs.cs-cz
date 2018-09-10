---
title: Odolnost proti chybám a zkuste to znovu připojení logic - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: d7e58abfa17c5537cdc9b0068e7c2a3c2e390038
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250514"
---
# <a name="connection-resiliency-and-retry-logic"></a>Připojení logic odolnost proti chybám a zkuste to znovu
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Připojení k serveru pro databázi aplikace byly vždy snadno napadnutelný konce připojení z důvodu selhání back-end a nestability sítě. Ale v prostředí sítě LAN, na základě pracovat se službou vyhrazené databázové servery tyto chyby jsou dostatečně vzácné, že další logiky, která by tyto chyby se často nevyžaduje. S nástupem cloud na základě databázové servery, jako jsou Windows Azure SQL Database a připojení přes méně spolehlivé sítě, které je nyní běžné připojení konce řádků. To může být způsobeno obranné techniky, které cloudové databáze pomocí zajistit dostupnost služby, například omezování využití připojení, nebo na nestabilitu v síti způsobují přerušované vypršení časových limitů a jiné přechodné chyby.  

Odolnost připojení odkazuje na možnost pro EF, aby automaticky opakovala všechny příkazy, které selžou kvůli konce těchto připojení.  

## <a name="execution-strategies"></a>Strategie provádění  

Opakování připojení se postaral o implementaci rozhraní IDbExecutionStrategy. Implementace IDbExecutionStrategy bude zodpovídat za přijímání operace a pokud dojde k výjimce, určíte, pokud je opakování pokusu vhodné a to zkusíte znovu, pokud se jedná. Existují čtyři strategií provádění, které se dodávají s EF:  

1. **DefaultExecutionStrategy**: Tato strategie provádění neopakuje žádné operace, je výchozí nastavení pro databáze než sql server.  
2. **DefaultSqlExecutionStrategy**: Toto je strategie provádění vnitřní, který se používá ve výchozím nastavení. Tato strategie neopakuje vůbec, ale sbalíte žádné výjimky, které by mohly být přechodné informovat uživatele, kteří chtějí povolit odolnost připojení.  
3. **DbExecutionStrategy**: Tato třída je vhodná jako základní třída pro jiné strategie provádění, včetně vlastních vlastní značky. Implementuje zásady opakování exponenciálního, kde počáteční opakování se stane s nulovou zpoždění a zpoždění zvyšuje exponenciálně až do dosažení maximální počet opakování. Tato třída je abstraktní metody ShouldRetryOn, který se dá implementovat v odvozené spuštění strategie řízení výjimek, které je třeba opakovat.  
4. **SqlAzureExecutionStrategy**: Tato strategie provádění dědí z DbExecutionStrategy a bude opakovat při výjimkách, které jsou známé jako pravděpodobně přechodné při práci s Azure SQL Database.

> [!NOTE]
> Strategie provádění 2 a 4 jsou součástí zprostředkovatele Sql Server, který se dodává s EF, který se nachází v sestavení EntityFramework.SqlServer a jsou navrženy pro práci se serverem SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Povolení strategie provádění  

Nejjednodušší způsob, jak zjistit EF použití strategie provádění je s metodou SetExecutionStrategy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) třídy:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Tento kód říká EF SqlAzureExecutionStrategy používat při připojování k serveru SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Konfigurace strategie provádění  

Konstruktor SqlAzureExecutionStrategy může přijmout dva parametry, MaxRetryCount a MaxDelay. Počet MaxRetry je maximální počet opakování strategie. MaxDelay je časový interval představující zpoždění mezi opakovanými pokusy, které se používají strategie provádění.  

Chcete-li nastavit maximální počet pokusů o 1 a Maximální zpoždění 30 sekund by execue následující:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

SqlAzureExecutionStrategy bude opakovat okamžitě překročil poprvé, dojde k přechodnému selhání, ale bude nadále zpoždění mezi opakováními, dokud se buď maximální limit opakování nebo celkové doby narazí na maximální zpoždění.  

Strategie provádění bude opakovat jenom omezený počet výjimek, které jsou obvykle tansient, bude i nadále potřebujete zpracovávat jiné chyby, jakož i RetryLimitExceeded výjimku pro případ, kdy chyba není přechodná nebo trvá příliš dlouho řešení samotný.  

Existují některé známé omezení při použití strategie opakování spuštění:  

## <a name="streaming-queries-are-not-supported"></a>Streamování dotazů se nepodporují.  

Ve výchozím nastavení EF6 a novější verze bude ve vyrovnávací paměti výsledky dotazu a nikoli jejich streamování. Pokud chcete mít výsledky streamování můžete použít metodu AsStreaming změnit LINQ dotaz entity pro streamování.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Streamování se nepodporuje při registraci opakuje strategie provádění. Toto omezení existuje, protože připojení může vyřadit rozúčtují přes výsledky se vrací. V tomto případě je potřeba znovu spustit celý dotaz EF, ale nemá žádné spolehlivě zjistit, jaké výsledky již byly vráceny (dat mohl být změněn počáteční dotaz byla odeslána, výsledky mohou vrátit v jiném pořadí, výsledky nemusí být jedinečný identifikátor atd.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Uživatelem iniciované transakce nejsou podporovány.  

Pokud jste nakonfigurovali strategie provádění, jehož výsledkem opakovaných pokusů, narazíte na určitá omezení týkající použití transakcí.  

Ve výchozím nastavení provede EF žádné aktualizace databáze v rámci transakce. Nemusíte dělat nic, aby tuto možnost povolte, EF vždy to dělá automaticky.  

Například v následujícím kódu metoda SaveChanges se provádí automaticky v rámci transakce. Pokud SaveChanges po vložení mezi novou lokalitou a transakce bude vrácena zpět a žádné změny k databázi selhat. Kontext je také zbývá ve stavu umožňujícím uloží změny do volat pokus o použití změn.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Pokud nepoužíváte opakuje strategie provádění v rámci jedné transakce můžete zalomit více operací. Následující kód například zabalí dvě SaveChanges volání v rámci jedné transakce. Pokud se nezdaří libovolné části buď operaci pak žádná ze změn, jsou použity.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

To není podporováno při použití opakuje strategie provádění, protože není si vědom jakékoli předchozí operace a způsob opakování je EF. Například pokud se druhý SaveChanges nezdařilo pak EF už má požadované informace pro první volání SaveChanges zopakovat.  

### <a name="workaround-suspend-execution-strategy"></a>Alternativní řešení: Pozastavení strategie provádění  

Jedním z možných řešení je dočasně pozastavit opakuje strategie provádění pro část kódu, který potřebuje uživatel inicioval transakce. Nejjednodušší způsob, jak to provést, je přidání SuspendExecutionStrategy příznak, který do vašeho kódu na základě konfigurace třídy a změňte lambda strategie provádění pro vrácení výchozí strategie provádění (bez retying), když je příznak nastaven.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Všimněte si, že používáme CallContext pro ukládání hodnoty horní příznak. Poskytuje podobné funkce jako úložiště thread local, ale je bezpečné používat s asynchronní kód – včetně asynchronního dotazu a uložit s Entity Framework.  

Nyní jsme můžete pozastavení strategie provádění pro části kódu, který používá transakce iniciované uživatelem.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a>Alternativní řešení: Volání ručně strategie provádění  

Další možností je ručně pomocí strategie provádění a přiřaďte mu celá sada logiky pro spuštění, tak, že je všechno, co opakujte Pokud jedna operace selže. Stále potřebujeme k pozastavení strategie provádění - technikou uvedeno výše - tak, aby všechny kontexty použít uvnitř blok opakovatelného kódu nebude pokoušet o opakování.  

Všimněte si, že všechny kontexty by měla být vytvořena v rámci bloku kódu se na opakování pokusu. Tím se zajistí, že jsme začínají s do čistého stavu pro každou. Zkuste to znovu.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
