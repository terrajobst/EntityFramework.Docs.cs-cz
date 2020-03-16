---
title: Odolnost připojení a logika opakování – EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402157"
---
# <a name="connection-resiliency-and-retry-logic"></a>Odolnost připojení a logika opakování
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Aplikace, které se připojují k databázovému serveru, byly vždycky zranitelné z důvodu selhání back-endu a nestability sítě. V prostředí založeném na síti LAN, které pracuje na vyhrazených databázových serverech, jsou ale tyto chyby vzácná natolik, že navíc není často nutná logika, která by tyto chyby zpracovala. S růstem cloudových databázových serverů, jako jsou Windows Azure SQL Database a připojení přes méně spolehlivé sítě, je teď častěji častější, než dojde k přerušením připojení. To může být způsobeno obrannou linií technikami, které používají cloudové databáze k zajištění spravedlivého poskytování služeb, jako je omezování připojení nebo nestabilitě sítě, která způsobuje přerušované časové limity a další přechodné chyby.  

Odolnost připojení označuje schopnost EF automaticky opakovat všechny příkazy, které selhaly kvůli přerušení připojení.  

## <a name="execution-strategies"></a>Strategie provádění  

Pokus o připojení se postará o implementaci rozhraní IDbExecutionStrategy. Implementace IDbExecutionStrategy budou zodpovědné za přijetí operace a, pokud dojde k výjimce, určí, jestli je opakování vhodné, a zkuste to znovu, pokud je. Existují čtyři strategie provádění dodávané s EF:  

1. **DefaultExecutionStrategy**: Tato strategie provádění neopakuje žádné operace, je výchozí pro databáze jiné než SQL Server.  
2. **DefaultSqlExecutionStrategy**: Jedná se o interní strategii provádění, která se používá ve výchozím nastavení. Tato strategie se vůbec neopakuje, ale zabalí všechny výjimky, které by mohly být přechodné, aby informovaly uživatele o tom, že by mohly chtít povolit odolnost připojení.  
3. **DbExecutionStrategy**: Tato třída je vhodná jako základní třída pro jiné strategie provádění, včetně vašich vlastních. Implementuje zásadu exponenciálního opakování, kde počáteční opakování proběhne s nulovým zpožděním a zpoždění se zvyšuje exponenciálně, dokud nebude dosaženo maximálního počtu opakování. Tato třída má abstraktní metodu ShouldRetryOn, která může být implementována v odvozených strategiích provádění, aby bylo možné řídit, které výjimky by se měly opakovat.  
4. **SqlAzureExecutionStrategy**: Tato strategie provádění dědí z DbExecutionStrategy a zopakuje pokus o výjimkách, u kterých je známo, že budou pravděpodobně přechodné při práci s Azure SQL Database.

> [!NOTE]
> Strategie provádění 2 a 4 jsou součástí poskytovatele SQL serveru, který je dodáván s EF, který je v sestavení EntityFramework. SqlServer a je navržen pro práci s SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Povolení strategie provádění  

Nejjednodušší způsob, jak říct EF, aby používal strategii provádění, je metoda SetExecutionStrategy třídy [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) :  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Tento kód oznamuje EF, aby při připojování k SQL Server používal rozhraní SqlAzureExecutionStrategy.  

## <a name="configuring-the-execution-strategy"></a>Konfigurace strategie provádění  

Konstruktor třídy SqlAzureExecutionStrategy může přijmout dva parametry, MaxRetryCount a MaxDelay. MaxRetry Count je maximální počet pokusů, kolikrát se strategie zopakuje. MaxDelay je interval TimeSpan, který představuje maximální zpoždění mezi opakovanými pokusy, které bude používat strategie provádění.  

Pokud chcete nastavit maximální počet opakovaných pokusů na hodnotu 1 a maximální zpoždění na 30 sekund, proveďte následující:  

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

SqlAzureExecutionStrategy se zopakuje okamžitě při prvním výskytu přechodného selhání, ale během každého opakování bude trvat déle, dokud nedosáhnete maximálního počtu opakování, nebo celkovým časem, který uplyne na maximálním zpoždění.  

Strategie provádění budou opakovat pouze omezený počet výjimek, které jsou obvykle přechodné, stále budete muset zpracovat další chyby a zachytit výjimku RetryLimitExceeded pro případ, že chyba není přechodná nebo je příliš dlouhá, aby ji bylo možné vyřešit. využít.  

Při použití strategie opakování pokusu je potřeba mít několik známých omezení:  

## <a name="streaming-queries-are-not-supported"></a>Dotazy streamování nejsou podporované.  

Ve výchozím nastavení EF6 a novější verze vyřadí výsledky dotazu do vyrovnávací paměti místo jejich streamování. Pokud chcete mít výsledky streamování výsledků, můžete pomocí metody AsStreaming změnit dotaz LINQ to Entities na streamování.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Streamování není podporováno, pokud je zaregistrována strategie spuštění opakování. Toto omezení existuje, protože připojení by mohlo vyřadit část způsobem prostřednictvím vrácených výsledků. V takovém případě musí EF znovu spustit celý dotaz, ale nemá žádný spolehlivý způsob, jak zjistit, které výsledky již byly vráceny (data se pravděpodobně od odeslání počátečního dotazu změnila, výsledky mohou být vráceny v jiném pořadí, výsledky nemusí mít jedinečný identifikátor). atd.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Uživatelem iniciované transakce nejsou podporované.  

Pokud jste nakonfigurovali strategii provádění, která má za následek opakování, existují určitá omezení týkající se použití transakcí.  

Ve výchozím nastavení bude EF provádět všechny aktualizace databáze v rámci transakce. Nemusíte nic dělat, abyste ho mohli povolit, EF to vždycky dělá automaticky.  

Například v následujícím kódu je kód SaveChanges automaticky proveden v rámci transakce. Pokud nebylo po vložení jedné z nových lokalit provedeno selhání metody SaveChanges, transakce by se vrátila zpět a v databázi se nepoužily žádné změny. Kontext je také ponechán ve stavu, který umožňuje, aby bylo volání metody SaveChanges znovu voláno pro opakované použití změn.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Pokud nepoužíváte strategii spuštění opakování, můžete v jedné transakci zabalit více operací. Například následující kód zalomí dvě volání metody SaveChanges v rámci jedné transakce. Pokud některé z obou operací selžou, nepoužije se žádná ze změn.  

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

Tato funkce není podporována, pokud používáte strategii opakovaného spuštění, protože EF neznáte žádné předchozí operace a postup opakování. Například pokud druhý příkaz SaveChanges neproběhne úspěšně, EF již neobsahuje požadované informace pro opakování prvního volání metody SaveChanges.  

### <a name="solution-manually-call-execution-strategy"></a>Řešení: ruční volání strategie provádění  

Řešením je ruční použití strategie spouštění a přidělení celé sady logiky, která se má spustit, aby se mohla opakovat vše, pokud jedna z operací neproběhne úspěšně. Pokud je spuštěná strategie spouštění odvozená z DbExecutionStrategy, pozastaví se implicitní prováděcí strategie používané v SaveChanges.  

Všimněte si, že všechny kontexty by měly být vytvořeny v rámci bloku kódu, který se má opakovat. Tím se zajistí, že pro každý pokus začneme s čistým stavem.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

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
```  
