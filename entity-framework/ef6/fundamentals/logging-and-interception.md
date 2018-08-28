---
title: Protokolování a zachycení databázových operací - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 2e16502abf54be3f3b2f63fe69d2605ef13dea27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994632"
---
# <a name="logging-and-intercepting-database-operations"></a>Protokolování a zachycení databázové operace
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Od verze Entity Framework 6, kdykoliv Entity Framework odešle příkaz do databáze tohoto příkazu může být zachycen kódu aplikace. To se většinou používá při protokolování SQL, ale můžete také použít ke změně nebo zrušení příkazu.  

Konkrétně EF zahrnuje:  

- Vlastnosti protokolu pro daný kontext podobný DataContext.Log v technologii LINQ to SQL  
- Mechanismus pro přizpůsobení obsahu a formátování výstupu odesílány do protokolu  
- Nízké úrovně stavební bloky pro zachycení poskytuje větší ovládací prvek/flexibilitu  

## <a name="context-log-property"></a>Vlastnost Context protokolu  

Vlastnost DbContext.Database.Log můžete nastavit na delegáta pro libovolnou metodu, která přebírá řetězec. Nejčastěji se používá s jakékoli TextWriter nastavením na metody "Zápisu" Tento TextWriter. Všechny SQL generovaných aktuálním kontextu se budou protokolovat do tohoto zapisovače. Například následující kód se budou protokolovat SQL do konzoly:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Všimněte si, že tento kontext. Database.Log nastavená na Console.Write. To je vše, co je potřeba pro SQL přihlášení ke konzole.  

### <a name="example-output"></a>Příklad výstupu  

Přidejme jednoduchého kódu query/insert nebo update, jsme viděli některé výstup:  

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

(Všimněte si, že se jedná o výstup, za předpokladu, že již došlo k inicializaci žádné databáze. Pokud inicializaci databáze nebyly již došlo k pak bude mnohem více výstup zobrazuje všechnu práci migrace provede na pozadí vyhledat nebo vytvořit novou databázi.)  

### <a name="what-gets-logged"></a>Co získá přihlášení?  

Když je vlastnost protokolu nastavena všechny tyto se protokolovat:  

- SQL pro všechny různé druhy příkazy. Příklad:  
    - Dotazy, včetně běžných dotazů LINQ, eSQL dotazy a nezpracované dotazy z metod, jako je například SqlQuery  
    - Vložení, aktualizace a odstranění generována jako součást SaveChanges  
    - Relace načítání dotazů, jako jsou protokoly generované systémem opožděné načtení  
- Parametry  
- Určuje, jestli tento příkaz se provádí asynchronně  
- Časové razítko určující, kdy příkaz zahájilo se spuštění  
- Jestli se nedokončil úspěšně, příkaz se nezdařil, vyvoláním výjimky nebo pro asynchronní, byla zrušena.  
- Některá označení výsledná hodnota  
- Přibližná množství času, které trvalo provedení příkazu. Všimněte si, že se jedná o času v odesílání příkazu k získání objektu výsledek zpět. Doba čtení výsledků neobsahuje.  

Hledání na výše uvedeném příkladu výstupu, každý ze čtyř příkazů protokoluje jsou:  

- Dotaz vyplývající z volání kontextu. Blogs.First  
    - Všimněte si, že Metoda ToString získání SQL nebude mít pracoval pro tento dotaz od "First" neposkytuje položku IQueryable, na kterém se mohl nazývat ToString  
- Dotaz vyplývající z blogu opožděné načtení. Příspěvky  
    - Všimněte si, že se děje podrobnosti parametr pro hodnotu klíče, pro které opožděné načtení  
    - Pouze vlastnosti parametru, které jsou nastaveny na jiné než výchozí hodnoty jsou protokolovány. Například vlastnost velikosti se zobrazí pouze pokud je nulová.  
- Dva příkazy vyplývající z SaveChangesAsync; jeden pro aktualizace, chcete-li změnit název post, druhá pro vložení pro přidání nového příspěvku  
    - Všimněte si, že parametr podrobné vlastnosti cizího klíče a funkce  
    - Všimněte si, že tyto příkazy se spouštějí asynchronně  

### <a name="logging-to-different-places"></a>Protokolování na různých místech  

Jak je znázorněno výše protokolování do konzoly máte velmi snadný. Se dá taky snadno protokolovat do paměti, souborů, atd. pomocí různých typů z [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Pokud jste se seznámili s jazykem LINQ to SQL pravděpodobně zjistíte, že v technologii LINQ to SQL protokolu vlastnost nastavena na skutečné TextWriter objekt (například Console.Out) při v EF protokolu vlastnost nastavena na metodu, která přijímá řetězec (například Console.Write nebo Console.Out.Write). Důvodem je oddělit EF z TextWriter přijetím jakýkoli delegát, který může fungovat jako jímka pro řetězce. Představte si například, že už máte některé protokolovacího rozhraní a definuje metodu protokolování takto:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

To může být připojili k vlastnosti EF protokolu následujícím způsobem:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

### <a name="result-logging"></a>Výsledek protokolování  

Protokolovací nástroj výchozí text příkazu (SQL), parametry a protokoly "Zpracování" řádku s časovým razítkem před odesláním příkazu do databáze. "Dokončených" řádek obsahující uplynulý čas je zaznamenané následující provedení příkazu.  

Všimněte si, že pro asynchronní příkazy "dokončených" řádku není přihlášen, dokud se asynchronní úlohy ve skutečnosti se dokončí, selže nebo je zrušena.  

"Dokončených" řádek obsahuje různé informace v závislosti na typu příkazu a určuje, jestli spuštění proběhlo úspěšně.  

#### <a name="successful-execution"></a>Úspěšná spuštění  

Příkazy, které úspěšně dokončit výstup je "dokončeno v x ms s výsledkem:" následované některé indikace toho, co bylo výsledek. Pro příkazy, které vracejí výsledek čtecí modul dat je označení typu [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) vrátila. Pro příkazy, které vrátí celočíselnou hodnotu, jako je například aktualizace je uvedenému výše ukazuje výsledek tohoto celé číslo.  

#### <a name="failed-execution"></a>Neúspěšné provedení  

Výstup obsahuje příkazy, které selhání vyvoláním výjimky, zprávu výjimky. Například pomocí dotazu na tabulku, která neexistuje SqlQuery se v protokolu výstupu výsledků vypadat přibližně takto:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

#### <a name="canceled-execution"></a>Zrušená spuštění  

Pro asynchronní příkazy, kde je tato úloha nezruší výsledek může být chyba s výjimkou, protože se jedná podkladového zprostředkovatele ADO.NET často význam při pokusu o zrušení. Pokud rovnosti a úloha se zruší čistě pak výstup bude vypadat přibližně takto:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Změna obsah protokolu a formátování  

### <a name="databaselogformatter"></a>DatabaseLogFormatter  

Pod pokličkou Database.Log vlastnost díky využívání DatabaseLogFormatter objektu. Tento objekt efektivně váže IDbCommandInterceptor implementaci (viz níže) na delegáta, který přijímá řetězce a DbContext. To znamená, že v DatabaseLogFormatter metody jsou volány před a po spuštění příkazů EF. Tyto metody DatabaseLogFormatter shromažďovat a formátování výstupu protokolu a odeslat do delegáta.  

### <a name="customizing-databaselogformatter"></a>Přizpůsobení DatabaseLogFormatter  

Změna co a jak je formátovaný můžete dosáhnout tím, že vytvoříte novou třídu, která je odvozena z DatabaseLogFormatter a přepíše metody podle potřeby. Nejběžnější metody přepsání jsou následující:  

- LogCommand – změnit, změnit, jak jsou protokolovány příkazy předtím, než se spustí. Ve výchozím nastavení LogCommand volá LogParameter pro každý parametr; můžete provést totéž v přepsání nebo zpracování parametrů jinak místo.  
- LogResult – změnit, změnit, jak se do protokolu zapíše výsledek spuštění příkazu.  
- LogParameter – změnit, změňte parametr protokolování obsahu a formátování.  

Předpokládejme například, že jsme chtěli přihlásit jen jeden řádek před odesláním každý příkaz v databázi. To můžete udělat s dva přepisy:  

- Přepsat LogCommand formátování a napsat jediný řádek SQL  
- Přepište LogResult neprovede žádnou akci.  

Kód by vypadat přibližně takto:

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

Protokolovat výstup jednoduše volání zápisu metodu, která bude odesílat výstup delegáta nakonfigurované zápisu.  

(Všimněte si, že tento kód provede zjednodušenou odebrání konců jenom jako příklad. Nebude pravděpodobně fungovat dobře pro zobrazení komplexní SQL.)  

### <a name="setting-the-databaselogformatter"></a>Nastavení DatabaseLogFormatter  

Po jeho vytvoření nové třídy DatabaseLogFormatter musí být registrována pomocí EF. To se provádí pomocí konfigurace založená na kódu. Řečeno v kostce to znamená, že vytváří novou třídu, která je odvozena z DbConfiguration ve stejném sestavení jako vaší třídy DbContext a následným voláním SetDatabaseLogFormatter v konstruktoru tato nová třída. Příklad:  

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

### <a name="using-the-new-databaselogformatter"></a>Pomocí nové DatabaseLogFormatter  

Tento nový DatabaseLogFormatter budou se teď dá kdykoliv Database.Log nastavena. Ano spouštění kódu z část 1 nyní způsobí následující výstup:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Stavební bloky zachycení  

Zatím jsme se podívat na použití k protokolování SQL generovaných EF DbContext.Database.Log. Ale tento kód je ve skutečnosti poměrně dynamického zajišťování průčelí přes některé nízké úrovně stavební bloky pro obecnější zachycení.  

### <a name="interception-interfaces"></a>Zachycení rozhraní  

Zachycení kódu byla vybudována okolo koncept zachycení rozhraní. Tato rozhraní se dědí z IDbInterceptor a definovat metody, které jsou volány při EF provede určitou akci. Cílem je mít jedno rozhraní na typ objektu, které jsou zachyceny. Například IDbCommandInterceptor rozhraní definuje metody, které jsou volány před EF zavolá metodu ExecuteNonQuery, ExecuteScalar, ExecuteReader a s ní související metody. Obdobně rozhraní definuje metody, které jsou volány při dokončení každé z těchto operací. DatabaseLogFormatter třídu, která jsme se podívali na výše implementuje toto rozhraní k protokolování příkazy.  

### <a name="the-interception-context"></a>Zachycení kontextu  

Hledání na metody definované na žádném z rozhraní zachycování je zřejmé, že všechna volání je daný objekt typu DbInterceptionContext nebo typ odvozený od to například DbCommandInterceptionContext\<\>. Tento objekt obsahuje kontextové informace o akci, která EF trvá. Například pokud se právě používá akce jménem DbContext, pak uvolněn objekt DbContext je součástí DbInterceptionContext. Podobně pro příkazy, které se spouštějí asynchronně, isasync: příznak nastaven na DbCommandInterceptionContext.  

### <a name="result-handling"></a>Výsledek zpracování  

DbCommandInterceptionContext\< \> třída obsahuje vlastnosti s názvem výsledek, OriginalResult, výjimky a původní výjimka. Tyto vlastnosti jsou nastaveny na hodnotu null nebo nulu pro volání metod zachycení, které jsou volány předtím, než se operace provede – to znamená pro... Provádění metody. Pokud operace se spustí a proběhne úspěšně, potom výsledek a OriginalResult jsou nastavenou na výsledek operace. Tyto hodnoty pak můžete pozorovat zachycení metody, které jsou volány po provedení operace – to znamená na... Provedený metody. Podobně pokud vyvolá operaci, pak výjimku a původní výjimka budou nastaveny vlastnosti.  

#### <a name="suppressing-execution"></a>Potlačení spuštění  

Pokud zachycování předtím, než se spustí příkaz nastaví vlastnost výsledek (v jednom z... Provádění metody) pak EF nepokusí se provést příkaz ve skutečnosti, ale budou místo toho použijte sadu výsledků dotazu. Jinými slovy lze sběrač potlačit provedení příkazu ale mít EF pokračovat jako příkazu se provedl.  

Příkaz dávkové zpracování, která proběhla tradičně s zabalení poskytovatele je příklad, jak to může být používána. Sběrač by uložit pro pozdější provedení příkazu v dávce, ale by "předstírají, že" EF, který provedl příkazu jako za normálních okolností. Všimněte si, že vyžaduje více než toto implementovat dávkové zpracování, ale toto je příklad použití Změna výsledku zachycení může být.  

Spuštění lze potlačit také tak, že nastavíte vlastnost výjimky v jednom z... Provádění metody. To způsobí, že EF pokračovat jako v případě provádění operace se nezdařila vyvoláním výjimka. Samozřejmě to může způsobit aplikace ohlašování chyb, ale mohou být také s přechodnou výjimkou nebo jinou výjimku, kterou provádí služba EF. Například to může použít v testovacích prostředích k testování chování aplikace při spuštění příkazu se nezdaří.  

#### <a name="changing-the-result-after-execution"></a>Změna výsledku po spuštění  

Pokud zachycování nastaví vlastnost výsledek po provedení příkazu (v jednom z... Spuštění metody) pak EF bude používat změněný výsledek místo výsledku, který se ve skutečnosti vrácená z operace. Podobně pokud se zachycování nastaví vlastnost výjimky po provedení příkazu, pak EF vyvolá výjimku sady jako by operace měla vyvolána výjimka.  

Zachycování můžete také nastavit vlastnost výjimku null k označení, že by měl být vyvolána žádná výjimka. To může být užitečné, pokud se nepodařilo spustit operaci, ale sběrač si přeje EF, abyste mohli pokračovat, jako by se operace dokončila. Obvykle to také zahrnuje nastavení výsledku tak, aby EF má některá z hodnot výsledek pro práci s jako pokračuje.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult a původní výjimka  

Po EF po provedení operace nastaví vlastnosti výsledek a OriginalResult, pokud byla úspěšná spuštění nebo vlastnosti výjimky a původní výjimka Pokud spuštění selhalo s výjimkou.  

OriginalResult a původní výjimka vlastnosti jsou jen pro čtení a jsou nastaveny jenom pomocí EF po skutečně provedení operace. Tyto vlastnosti nejde nastavit zachycovacích dotazů. To znamená, že všechny zachycování možné rozlišit výjimku nebo výsledek, který je nastavená podle jiných zachycování na rozdíl od skutečné výjimky nebo výsledek, ke které došlo při operaci spustil.  

### <a name="registering-interceptors"></a>Registrace zachycovacích dotazů  

Po vytvoření třídy, která implementuje jednu nebo více rozhraní zachycení lze registrovat pomocí EF horizontálních oddílů pomocí třídy DbInterception. Příklad:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Sběrače lze také zaregistrovat na úrovni domény aplikace pomocí mechanismu DbConfiguration konfigurace založená na kódu.  

### <a name="example-logging-to-nlog"></a>Příklad: Protokolování NLog  

Pojďme dohromady to vše na příklad, že při použití IDbCommandInterceptor a [NLog](http://nlog-project.org/) na:  

- Upozornění pro jakýkoli příkaz, který je spuštěn mimo asynchronně  
- Zaznamenat chybu pro jakýkoli příkaz, který vyvolá při spuštění  

Tady je třída, která provádí protokolování, které by měly být zaregistrovány, jak je uvedeno výše:  

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

Všimněte si, jak tento kód používá kontext zachycení ke zjištění, kdy příkaz se zpracovává jiné asynchronní a chcete zjistit, když došlo k chybě při spuštění příkazu.  
