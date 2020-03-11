---
title: Protokolování a zachycení databázových operací – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 35b0284a5ad8b2b732f074589bd458d243312575
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419476"
---
# <a name="logging-and-intercepting-database-operations"></a>Protokolování a zachycení databázových operací
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Počínaje Entity Framework 6, kdykoli Entity Framework odešle příkaz do databáze tento příkaz může zachytit kód aplikace. Nejčastěji se používá pro protokolování SQL, ale dá se použít i k úpravě nebo přerušení příkazu.  

Konkrétně EF zahrnuje:  

- Vlastnost protokolu pro kontext podobný kontextu DataContext. Přihlaste se LINQ to SQL  
- Mechanismus přizpůsobení obsahu a formátování výstupu odeslaného do protokolu  
- Stavební bloky nízké úrovně pro zachycení poskytující větší možnosti kontroly a flexibility  

## <a name="context-log-property"></a>Vlastnost log kontextu  

Vlastnost DbContext. Database. log lze nastavit na delegáta pro jakoukoliv metodu, která přebírá řetězec. Nejčastěji se používá s jakýmkoli modulem TextWriter nastavením na metodu zápisu tohoto modulu TextWriter. Všechny SQL generované aktuálním kontextem budou protokolovány do tohoto zapisovače. Například následující kód bude protokolovat SQL do konzoly:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Všimněte si, že kontext. Database. log je nastavená na Console. Write. To je všechno, co je potřeba k protokolování SQL do konzoly.  

Pojďme přidat nějaký jednoduchý kód pro dotaz, vložení/aktualizaci, aby se mohl zobrazit nějaký výstup:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Tím se vygeneruje následující výstup:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Všimněte si, že se jedná o výstup za předpokladu, že nějaká inicializace databáze již proběhla. Pokud inicializace databáze ještě neproběhla, měla by se zobrazit spousta dalších výstupů, na kterých se v rámci pokrývá všechny pracovní migrace za účelem kontroly nebo vytvoření nové databáze.)  

## <a name="what-gets-logged"></a>Co se protokoluje?  

Při nastavení vlastnosti log se zaprotokoluje následující:  

- SQL pro všechny různé druhy příkazů. Příklad:  
    - Dotazy, včetně normálních dotazů LINQ, dotazy eSQL a nezpracované dotazy z metod, jako je SqlQuery  
    - Vložení, aktualizace a odstranění vygenerované jako součást metody SaveChanges  
    - Vztah načítání dotazů, například generovaných opožděným načtením  
- Parametry  
- Bez ohledu na to, zda je příkaz prováděn asynchronně  
- Časové razítko, které signalizuje spuštění příkazu  
- Bez ohledu na to, jestli se příkaz úspěšně dokončil, selhala výjimka, nebo pro Async se zrušila operace.  
- Některé náznaky hodnoty výsledku  
- Přibližná doba, jakou trvalo spuštění příkazu. Všimněte si, že se jedná o čas odeslání příkazu k získání objektu výsledku zpět. Nezahrnuje čas pro čtení výsledků.  

Ve výše uvedeném příkladu se každý ze čtyř protokolovaných příkazů podívá:  

- Dotaz, který je výsledkem volání kontextu. Blogy. First  
    - Všimněte si, že metoda ToString pro získání SQL by pro tento dotaz nefungovala, protože "First" neposkytuje rozhraní IQueryable, na které by mohlo být voláno rozhraní ToString.  
- Dotaz vyplývají z opožděného načítání blogu. Fórech  
    - Všimněte si podrobností parametru pro hodnotu klíče, pro kterou probíhá opožděné načítání.  
    - Protokolují se pouze vlastnosti parametru, které jsou nastaveny na jiné než výchozí hodnoty. Například vlastnost Size se zobrazí pouze v případě, že je nenulová.  
- Dva příkazy, které jsou výsledkem SaveChangesAsync; jedna pro aktualizaci pro změnu názvu příspěvku, druhá pro vložení pro přidání nového příspěvku  
    - Všimněte si podrobností parametru pro vlastnosti FK a title.  
    - Všimněte si, že se tyto příkazy provádějí asynchronně.  

## <a name="logging-to-different-places"></a>Protokolování na různá místa  

Jak vidíte výše, protokolování do konzoly je velice snadné. Je také snadné protokolovat do paměti, souboru atd. pomocí různých druhů [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Pokud jste obeznámeni s LINQ to SQL, můžete si všimnout, že v části LINQ to SQL je vlastnost protokolu nastavena na skutečný objekt TextWriter (například Console. out), zatímco v EF je vlastnost log nastavena na metodu, která přijímá řetězec (například , Console. Write nebo Console. out. Write). Důvodem je oddělit EF od TextWriter tím, že přijmete libovolného delegáta, který může fungovat jako jímka pro řetězce. Představte si například, že už máte nějaké protokolovací rozhraní a definujete metodu protokolování, například:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

To může být zapojování do vlastnosti protokolu EF takto:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Protokolování výsledků  

Výchozí protokolovací nástroj protokoluje text příkazu (SQL), parametry a "spouští" řádek s časovým razítkem před odesláním příkazu do databáze. Po provedení příkazu se zaprotokoluje dokončený řádek obsahující uplynulý čas.  

Všimněte si, že pro asynchronní příkazy se "dokončený" řádek nezaznamenává do chvíle, kdy je asynchronní úkol dokončen, dojde k chybě nebo byl zrušen.  

Řádek dokončeno obsahuje různé informace v závislosti na typu příkazu a na tom, zda bylo spuštění úspěšné.  

### <a name="successful-execution"></a>Úspěšné provedení  

Pro příkazy, které se úspěšně dokončily, se výstup dokončí v x MS s výsledkem: následovaný nějakým náznakem toho, co byl výsledek. Pro příkazy, které vracejí data Reader, je výsledkem výsledná indikace typ vráceného typu [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) . Pro příkazy, které vrátí celočíselnou hodnotu, jako je například příkaz Update zobrazený výše zobrazeným výsledkem, je toto celé číslo.  

### <a name="failed-execution"></a>Neúspěšné provedení  

Pro příkazy, které selžou při vyvolání výjimky, výstup obsahuje zprávu z výjimky. Například použití SqlQuery k dotazování na tabulku, která existuje, bude mít za následek výstup protokolu podobný tomuto:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Zrušené provedení  

V případě asynchronních příkazů, kde je úloha zrušena, může být výsledkem chyba s výjimkou, protože se jedná o to, co nadřízený poskytovatel ADO.NET často provádí při pokusu o zrušení. Pokud k tomu nedojde a úloha se zruší čistě, bude výstup vypadat přibližně takto:  

```console
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Změna obsahu a formátování protokolu  

V části pokrývá vlastnost Database. log využívá objekt DatabaseLogFormatter. Tento objekt efektivně váže implementaci IDbCommandInterceptor (viz níže) k delegátovi, který přijímá řetězce a DbContext. To znamená, že metody v DatabaseLogFormatter jsou volány před a po provedení příkazů podle EF. Tyto metody DatabaseLogFormatter shromažďují a formátují výstup protokolu a odesílají je do delegáta.  

### <a name="customizing-databaselogformatter"></a>Přizpůsobení DatabaseLogFormatter  

Změna toho, co je protokolováno a jak je formátováno, lze dosáhnout vytvořením nové třídy, která je odvozena z DatabaseLogFormatter a potlačení metod podle potřeby. Nejběžnější metody přepsání:  

- LogCommand – přepište tuto změnu, chcete-li změnit způsob, jakým jsou protokolovány příkazy před jejich spuštěním. Ve výchozím nastavení LogCommand volá LogParameter pro každý parametr; místo toho se můžete rozhodnout v parametrech přepisu nebo pořizování jinak.  
- LogResult – přepište tuto změnu, chcete-li změnit způsob, jakým je výsledek spuštění příkazu zaznamenán.  
- LogParameter – toto přepište pro změnu formátování a obsahu protokolování parametrů.  

Předpokládejme například, že jsme chtěli protokolovat pouze jeden řádek před odesláním každého příkazu do databáze. Můžete to udělat se dvěma přepsáními:  

- Přepsat LogCommand pro formátování a zápis jednoho řádku SQL  
- Přepište LogResult, aby nedošlo k žádné akci.  

Kód by vypadal přibližně takto:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Do log output stačí zavolat metodu zápisu, která odešle výstup do konfigurovaného delegáta pro zápis.  

(Všimněte si, že tento kód provede odebrání konců řádků pouze jako příklad. Nebude nejspíš fungovat dobře pro zobrazení složitého SQL.)  

### <a name="setting-the-databaselogformatter"></a>Nastavení DatabaseLogFormatter  

Po vytvoření nové třídy DatabaseLogFormatter musí být zaregistrována v EF. To se provádí pomocí konfigurace založené na kódu. V kostce to znamená vytvořit novou třídu, která je odvozena z DbConfiguration ve stejném sestavení jako vaše třída DbContext a pak volat SetDatabaseLogFormatter v konstruktoru této nové třídy. Příklad:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Použití nového DatabaseLogFormatter  

Tento nový DatabaseLogFormatter se teď použije jako soubor Anytime Database. log. Takže spuštění kódu z části 1 teď má za následek následující výstup:  

```console
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Stavební bloky zachycení  

Zatím jsme si vyhledali, jak používat DbContext. Database. log k protokolování SQL generovaných EF. Tento kód je ale ve skutečnosti relativně tenká fasáda v některých stavebních blocích nízké úrovně pro obecnější zachycení.  

### <a name="interception-interfaces"></a>Rozhraní zachycení  

Kód zachycení je sestaven kolem konceptu rozhraní zachycení. Tato rozhraní dědí z IDbInterceptor a definují metody, které jsou volány, když EF provede určitou akci. Záměrem je, aby bylo možné zachytit jedno rozhraní na typ objektu. Například rozhraní IDbCommandInterceptor definuje metody, které jsou volány před EF, provede volání ExecuteNonQuery, ExecuteScalar, ExecuteReader a souvisejících metod. Podobně rozhraní definuje metody, které jsou volány při dokončení každé z těchto operací. Třída DatabaseLogFormatter, kterou jsme prohlédli výše, implementuje toto rozhraní k protokolování příkazů.  

### <a name="the-interception-context"></a>Kontext zachycení  

V případě metod, které jsou definovány v jakémkoli rozhraní zachytávací, je zřejmé, že každé volání je předané objektu typu DbInterceptionContext nebo nějaký typ odvozený z tohoto typu, jako je například DbCommandInterceptionContext\<\>. Tento objekt obsahuje kontextové informace o akci, kterou EF vezme. Například pokud je akce prováděna jménem DbContext, pak DbContext je součástí DbInterceptionContext. Podobně pro příkazy, které jsou spouštěny asynchronně, je příznak-Async nastaven na DbCommandInterceptionContext.  

### <a name="result-handling"></a>Zpracování výsledku  

Třída DbCommandInterceptionContext\<\> obsahuje vlastnosti nazvané výsledek, OriginalResult, výjimka a OriginalException. Tyto vlastnosti jsou nastaveny na hodnotu null/nula pro volání metod zachycení, které jsou volány před spuštěním operace – to znamená pro... Spouštění metod. Pokud je operace spuštěná a úspěšná, pak se výsledek a OriginalResult nastaví na výsledek operace. Tyto hodnoty lze následně pozorovat v metodách zachycení, které jsou volány po provedení operace – to znamená na... Spouštěné metody. Podobně platí, že pokud operace vyvolá, budou nastaveny vlastnosti Exception a OriginalException.  

#### <a name="suppressing-execution"></a>Potlačení provádění  

Pokud zachytává Vlastnost Result před provedením příkazu (v jednom z těchto... Po spuštění metod) se EF nepokusí spustit příkaz, ale místo toho použije sadu výsledků. Jinými slovy, zachytávací modul může potlačit provádění příkazu, ale měl by mít v kódu EF pokračovat, jako kdyby byl příkaz proveden.  

Příkladem toho, jak to lze použít, je dávkování příkazů, které bylo tradičně provedeno s poskytovatelem zabalení. Zachytávací příkaz by uložil příkaz pro pozdější spuštění jako dávku, ale měl by být "předstírat" na EF, že příkaz byl proveden jako normální. Všimněte si, že k implementaci dávkového zpracování vyžaduje více než tento postup, ale jedná se například o způsob, jakým je možné použít změnu výsledku zachycení.  

Spuštění lze také potlačit nastavením vlastnosti Exception v jednom z... Spouštění metod. To způsobuje, že EF pokračuje jako v případě, že provedení operace selhalo vyvoláním dané výjimky. To může samozřejmě způsobit chybu aplikace, ale může to být také přechodná výjimka nebo jiná výjimka, která je zpracována EF. To může být například použito v testovacích prostředích k otestování chování aplikace při neúspěšném spuštění příkazu.  

#### <a name="changing-the-result-after-execution"></a>Změna výsledku po spuštění  

Pokud zachytávací Vlastnost Result nastaví po provedení příkazu (v jedné z vlastností... Spouštěné metody), pak EF použije změněný výsledek místo výsledku, který byl skutečně vrácen z operace. Podobně, pokud zachytává událost nastaví vlastnost Exception po provedení příkazu, pak EF vyvolá výjimku set, jako kdyby byla výjimka vyvolána v operaci.  

Zachytávací objekt může také nastavit vlastnost Exception na hodnotu null, aby označovala, že by neměla být vyvolána žádná výjimka. To může být užitečné v případě, že provedení operace se nezdařilo, ale zachytávací, který chce EF pokračovat, jako kdyby operace proběhla úspěšně. To obvykle zahrnuje nastavení výsledku tak, aby měl EF při pokračování nějakou hodnotu výsledku.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult a OriginalException  

Po provedení operace v EF se nastaví buď vlastnosti Result a OriginalResult, pokud spuštění neselže, nebo pokud se spuštění nezdařilo, došlo k chybě s výjimkou.  

Vlastnosti OriginalResult a OriginalException jsou jen pro čtení a nastavují je jenom EF v rámci skutečného provedení operace. Tyto vlastnosti nelze nastavit pomocí zachycení. To znamená, že jakýkoliv zachytávací může rozlišovat výjimku nebo výsledek, který byl nastaven jiným zachytávací, na rozdíl od reálné výjimky nebo výsledek, ke kterému došlo při provedení operace.  

### <a name="registering-interceptors"></a>Registrace zachycení  

Jakmile je třída, která implementuje jedno nebo více rozhraní zachycení, vytvořena, může být zaregistrována s EF pomocí třídy DbInterception. Příklad:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Zachycení lze také registrovat na úrovni aplikačních domén pomocí mechanismu konfigurace založeného na kódu DbConfiguration.  

### <a name="example-logging-to-nlog"></a>Příklad: protokolování do NLog  

Pojďme to všechno dohromady do příkladu, který používá IDbCommandInterceptor a [nLOG](https://nlog-project.org/) k:  

- Zaprotokoluje upozornění pro všechny příkazy, které se provedly bez asynchronního zpracování.  
- Zaznamená chybu pro jakýkoli příkaz, který vyvolá při spuštění.  

Zde je třída, která provede protokolování, které by mělo být registrováno, jak je uvedeno výše:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Všimněte si, jak tento kód používá kontext zachycení ke zjištění, že se příkaz provádí neasynchronně a že při provádění příkazu se zjistila chyba.  
