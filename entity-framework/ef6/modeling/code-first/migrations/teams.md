---
title: Migrace Code First v prostředích Team - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: 42f52e63fd6cfc1f02d6a721594f4a161eea9a7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997296"
---
# <a name="code-first-migrations-in-team-environments"></a>Migrace Code First v prostředích Team
> [!NOTE]
> Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře. Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Stáhněte si kávu, budete muset tento celý článek

Problémy v prostředích team jsou většinou kolem sloučení migrace po dvou vývojáři vygenerovaly migrace v jejich místní kódové základny. Kroky k vyřešení těchto jsou poměrně jednoduché, vyžadují mít důkladného porozumění fungování migrace. Prosím není právě přeskočte dopředu na konec – čas si přečíst celý článek k zajištění, že máte úspěšné.

## <a name="some-general-guidelines"></a>Některé obecné pokyny

Předtím, než se podíváme se podrobně na tom, jak spravovat sloučení migrace generovaných více vývojářů, tady jsou některé obecné pokyny k nastavení vám dá zajistit úspěšnost.

### <a name="each-team-member-should-have-a-local-development-database"></a>Každý člen týmu by měl mít místní vývoj databáze

Použití migrace  **\_ \_MigrationsHistory** tabulku pro ukládání, jaké migrace se použily k databázi. Pokud máte více vývojářů, generuje se při pokusu o cílit na stejné databáze různé migrace (a tedy sdílet  **\_ \_MigrationsHistory** tabulky) migrace bude velmi zmatení.

Samozřejmě pokud jste členy týmu, které nejsou generování migrace, neexistuje žádný problém s nimi sdílet centrální vývoje databáze.

### <a name="avoid-automatic-migrations"></a>Vyhněte se automatické migrace

Dolní řádek je, že automatické migrace zpočátku vypadat dobře v prostředích team, ale ve skutečnosti právě nefungují. Pokud budete chtít vědět proč, pokračujte ve čtení – Pokud ne, pak můžete přeskočit k další části.

Automatické migrace umožňuje mít schéma databáze aktualizovat tak, aby odpovídaly aktuálním modelu bez nutnosti Generovat soubory kódu (migrace založené na kódu). Automatické migrace by velmi dobře fungovat v prostředí team, pokud je pouze nikdy nepoužil a nikdy generována všechny migrace na kód. Problém je, že automatické migrace jsou omezené a nezpracovávají počet operací – přejmenuje sloupce nebo vlastností, přesun dat do jiné tabulky, atd. Pro zpracování scénářů skončíte generování migrace na kódu (a úpravy automaticky generovaný kód), které jsou ve smíšeném mezi změny, které jsou zpracovávány automatické migrace. Díky tomu téměř na možné sloučit změny, když dva vývojáři vrátit se změnami migrace.

## <a name="screencasts"></a>Screencasty

Pokud byste raději přehrát záznam dění na monitoru než v tomto článku, následující dvě videa zahrnují obsah jako v tomto článku.

### <a name="video-one-migrations---under-the-hood"></a>Video jeden: "Migrace - pod pokličkou"

[Tento záznam dění na monitoru](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) vysvětluje, jak migrace sleduje a používá informace o modelu ke zjištění změny modelu.

### <a name="video-two-migrations---team-environments"></a>Dvě videa: "Migrace - Týmová prostředí"

Stavíme na koncepty z předchozí video [tento záznam dění na monitoru](http://channel9.msdn.com/blogs/ef/migrations-team-environments) popisuje problémy, které vznikají v týmu prostředí a jak je vyřešit.

## <a name="understanding-how-migrations-works"></a>Pochopení, jak funguje migrace

Klíčem k úspěšné použití migrace v prostředí team je základní princip, jak migrace sleduje a informace o modelu využívá ke zjištění změny modelu.

### <a name="the-first-migration"></a>Při první migraci

Při první migraci přidáte do projektu, spustíte něco jako **migrace přidat první** v konzole Správce balíčků. Kroky na nejvyšší úrovni, které provádí tento příkaz jsou na obrázku níže.

![FirstMigration](~/ef6/media/firstmigration.png)

Aktuální model se počítá z uživatelského kódu (1). Požadované databázové objekty jsou pak vypočítá podle modelu se liší (2) – protože se jedná o první migraci modelu se liší jenom používá prázdný model pro porovnání. Požadované změny se předají generátor kódu sestavení kódu vyžaduje migraci (3), která se pak přidá do řešení sady Visual Studio (4).

Kromě skutečné migrace kódu, který je uložen v souboru hlavní kód generuje migrace také některé další použití modelu code-behind soubory. Tyto soubory jsou metadata, která používá migrace a nejsou něco, co byste měli upravovat. Některý z těchto souborů je soubor prostředku (RESX), který obsahuje snímek modelu v době, kdy byla vygenerována migrace. Uvidíte jak, to se používá v dalším kroku.

V tomto okamžiku by pravděpodobně spuštění **aktualizace databáze** použít změny v databázi, a pak přejděte o implementaci jiných oblastech vaší aplikace.

### <a name="subsequent-migrations"></a>Následné migrace

Později vrátit a provést nějaké změny do modelu – v našem příkladu přidáme **Url** vlastnost **blogu**. By potom vydat příkaz například **přidat migrace AddUrl** scaffold migrace použít odpovídající databáze změní. Kroky na nejvyšší úrovni, které provádí tento příkaz jsou na obrázku níže.

![SecondMigration](~/ef6/media/secondmigration.png)

Stejně jako předtím se vypočítá aktuální model z kódu (1). Nyní existují však existující migrace tak předchozí model se načte z nejnovější migrace (2). Tyto dva modely jsou diffed vyhledat změny databáze (3) a potom dokončí proces stejně jako předtím.

Tento stejný postup se používá pro všechny další migrace, které přidáte do projektu.

### <a name="why-bother-with-the-model-snapshot"></a>Proč zabývat snímku modelu?

Asi vás zajímá proč EF bothers klauzuli with snapshot modelu – Proč ne jenom pohled na databázi. Pokud ano, přečtěte si o. Pokud si nejste chtěli můžete tuto část přeskočit.

Existuje mnoho důvodů, které EF udržuje snímku modelu kolem:

-   To umožňuje databáze z modelu EF odchylek. Tyto změny provést přímo v databázi, nebo můžete změnit automaticky generovaný kód ve vaší migrace, aby se změny. Tady je několik příkladů to v praxi:
    -   Chcete přidat Inserted a aktualizované sloupce na jeden nebo více tabulek, ale nechcete zahrnout tyto sloupce do modelu EF. Pokud migrace podívali se na databázi průběžně, pokusí se odebrat tyto sloupce pokaždé, když se automaticky migrace. Pomocí modelu snímku, EF vždy jen rozpozná legitimní změny modelu.
    -   Chcete změnit text uložené procedury používané pro aktualizace zahrnout určité protokolování. Pokud migrace podívali se na tuto uloženou proceduru z databáze ji by průběžně zkuste a obnovit jej do definice, která očekává EF. Pomocí snímku modelu EF bude vždy jen generování uživatelského rozhraní kódu pro úpravu uložené procedury při změně tvaru podle postupu v modelu EF.
    -   Tyto stejné zásady platí i pro přidání dalších indexů, včetně další tabulky v databázi, mapování EF na zobrazení databáze, který je umístěný v tabulce, atd.
-   EF model obsahuje více než jen tvar databáze. Celý model umožňuje migrace podívat se na informace o vlastnostech a třídy v modelu a jak jsou mapovány pro tabulky a sloupce. Tyto informace umožňují migrace bude inteligentnější v kódu, který se vygeneruje uživatelské rozhraní. Například pokud změníte název sloupce, který se mapuje vlastnost migrace dokáže přejmenování tak zjistíte, že je stejná vlastnost – něco, co nelze provést, pokud máte pouze schéma databáze. 

## <a name="what-causes-issues-in-team-environments"></a>Co způsobuje problémy v prostředích team

Pracovní postup popsané v předchozí části funguje skvělé po jeden vývojář, pracující na aplikaci. Je také dobře funguje v prostředí team Pokud jste jediná osoba změn do modelu. V tomto scénáři můžete měnit model, generovat migrace a odesílat je na vaší správy zdrojového kódu. Ostatní vývojáři mohou synchronizace změn a spustit **aktualizace databáze** změny schématu použít.

Položky problémů umožňují zahájit vzniknou, když máte více vývojářů provádění změn modelu EF a odešlete do správy zdrojového kódu ve stejnou dobu. Co chybí EF je prvotřídní způsob, jak sloučit vaše místní migrace s migrace, které pro jiné vývojáře má odeslat do správy zdrojových kódů, protože poslední synchronizace.

## <a name="an-example-of-a-merge-conflict"></a>Příkladem konfliktu při slučování

První Podívejme se na konkrétní příkladem konfliktu sloučení. Na budeme pokračovat s příkladu, který jsme se podívali na dříve. Jako počáteční bod Pojďme předpokládají původní vývojáře byly vráceny změny z předchozí části. Budete sledujeme dva vývojářům jejich provádění změn kódu základní.

Sledujeme budete modelu EF a migrace do řadu změn. Pro výchozí bod jak vývojáři synchronizaci do úložiště správy zdrojového kódu, jak je znázorněno na následujícím obrázku.

![StartingPoint](~/ef6/media/startingpoint.png)

Pro vývojáře \#1 a pro vývojáře \#2 teď provádí některé změny v modelu EF v jejich místní kódu základní. Pro vývojáře \#1 přidá **hodnocení** vlastnost **blogu** – a generuje **AddRating** migrace změny se projeví do databáze. Pro vývojáře \#2 přidá **čtenáři** vlastnost **blogu** – a vygeneruje odpovídající **AddReaders** migrace. Spustit i vývojáře **aktualizace databáze**, abyste mohli aplikovat změny na svých místních databází a potom pokračovat ve vývoji aplikace.

> [!NOTE]
> Migrace mají předponu časové razítko, aby naše obrázek, který představuje AddReaders migrace z Developer \#2, po migraci AddRating pochází Developer \#1. Ať už pro vývojáře \#1 nebo \#2 vygenerovat první provede migraci žádný rozdíl na problémy, o práci v týmu nebo procesu pro sloučení jim, najdete v další části.

![LocalChanges](~/ef6/media/localchanges.png)

Je štěstí den pro vývojáře \#1 při jejich provádění předkládat své změny. Protože nikdo jiný se přihlásila vzhledem k tomu, že se synchronizují své úložiště, jsou pouze odesílat své změny bez provedení jakékoli sloučení.

![Odeslat](~/ef6/media/submit.png)

Nyní je čas pro vývojáře \#2 k odeslání. Nejsou tedy měli. Protože změny někoho jiného má odeslat, protože jsou synchronizované, se musí stáhnout změny a sloučení. Systém správy zdrojového kódu pravděpodobně bude moci automaticky sloučit změny na úrovni kódu, protože jsou velmi jednoduché. Stav pro vývojáře \#2 na místní úložiště po synchronizaci je znázorněn na následujícím obrázku. 

![O přijetí změn](~/ef6/media/pull.png)

V této fáze pro vývojáře \#2 můžete spustit **aktualizace databáze** který rozpozná nový **AddRating** migrace (která nebyla použita pro vývojáře \#2 databáze) a použijte ji. Nyní **hodnocení** sloupec se přidá do **blogy** tabulky a databáze je synchronizovaný s modelem.

Existuje však několik problémů:

1.  I když **aktualizace databáze** budou platit **AddRating** migrace také vyvolá upozornění: *nejde aktualizovat databázi tak, aby odpovídaly aktuálním modelu, protože existují čekající změny a je zakázáno automatické migrace...*
    Problém je, že model snímku uloženy poslední migrace (**AddReader**) chybí **hodnocení** vlastnost **blogu** (protože není součástí modelu při Migrace byla vygenerována). Kód nejprve zjistí, že model v posledních migrace neodpovídá aktuální model a vyvolá upozornění.
2.  Spuštění aplikace by mělo za následek InvalidOperationException oznamující, že "*model zálohování kontextu 'BloggingContext' byl změněn, protože byla vytvořena databáze. Vezměte v úvahu pomocí migrace Code First, a aktualizovat databázi..."*
    Problém je opět modelu snímku uloženy poslední migrace neodpovídá modelu.
3.  Nakonec by Očekáváme, že spuštění **přidat migrace** nyní vygeneruje prázdný migrace (protože nejsou žádné změny, které chcete použít pro databázi). Ale vzhledem k tomu, že migrace porovná aktuální model k jednomu z poslední migrace (které chybí **hodnocení** vlastnost) ji bude ve skutečnosti generování uživatelského rozhraní jiného **AddColumn** volání doplňku **Hodnocení** sloupce. Samozřejmě, tato migrace selže během **aktualizace databáze** protože **hodnocení** sloupec už existuje.

## <a name="resolving-the-merge-conflict"></a>Řešení konfliktu sloučení

Dobrou zprávou je, že není příliš obtížné řešit sloučení ručně – Pokud máte znalosti toho, jak funguje migrace. Pokud jste jste přeskočili v této části... je nám líto, ale je potřeba vrátit zpět a číst zbývající části článku nejprve!

Existují dvě možnosti, nejjednodušší je generování prázdné migrace, který má správnou aktuální model jako snímek. Druhou možností je aktualizovat snímek v posledních migrace na správný snímek modelu. Druhou možností je o něco složitější a nelze jej použít v každý scénář, ale je také přehlednější protože nezahrnuje přidávání dalších migrace.

### <a name="option-1-add-a-blank-merge-migration"></a>Možnost 1: Přidejte migraci prázdné "sloučení.

Při použití této možnosti se vygeneruje prázdný migrace výhradně pro účely zajištění nejnovější migrace správný model snímku v něm uložena.

Tuto možnost lze použít bez ohledu na to která vygenerovala poslední migrace. V příkladu jsme jste postupovali podle Developer \#2 se postará o sloučení a k nim došlo k vygenerování posledního migrace. Ale stejný postup můžete použít, pokud pro vývojáře \#1 generované poslední migrace. Postup platí také v případě více migrace se využívá řada – jste právě byla se díváme na dvě aby bylo možné zachovat jednoduché.

Následující postup je možné pro tento přístup, od doby, je dobré si uvědomit, že máte změny, které je třeba synchronizovat ze správy zdrojového kódu.

1.  Zajistěte, aby že byla zapsána změny čekající na vyřízení modelu v vašeho základu kódu místní migrace. Tento krok zajistí, že pokud jde o generování prázdné migrace dobu Nenechte si ujít všechny oprávněné změny.
2.  Synchronizace se správou zdrojového kódu.
3.  Spustit **aktualizace databáze** provádět žádné nové migrace, které ostatní vývojáři se změnami.
    **
    *Poznámka: *** Pokud neobdržíte žádné upozornění z příkazu Update-databáze a nebyly žádné nové migrace od jiných vývojářů a není nutné provádět žádné další sloučení.*
4.  Spustit **přidat migrace &lt;vyberte\_\_název&gt; – IgnoreChanges** (například **sloučit přidat migrace – IgnoreChanges**). To generuje migrace s všechna metadata (včetně snímek aktuální model), ale bude ignorovat jakékoli změny zjistí při porovnání aktuálního modelu na snímek v posledních migrace (to znamená získání prázdnou hodnotu **nahoru** a **Dolů** metoda).
5.  Pokračovat ve vývoji, nebo odešlete do správy zdrojového kódu (po spuštění jednotky samozřejmě testy).

Tady je stav pro vývojáře \#2 je místní kódové základny po použití tohoto přístupu.

![MergeMigration](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Možnost 2: Aktualizace modelu snímku v posledních migrace

Tato možnost je velmi podobný možnost 1, ale odebere prázdné migrace – protože jasné, kdo chce, aby se soubory zvláštní kód ve svých řešeních.

**Tento přístup je možné pouze v případě nejnovější migrace existuje pouze v místním kódové základny a dosud nebyla odeslána do správy zdrojových kódů (např. Pokud poslední migrace byla vygenerována koncipovaná sloučení)**. Úprava metadat migrace, které ostatní vývojáři mohou už jste použili k jejich vývoj databáze – nebo dokonce horší použitý k produkční databázi – může způsobit neočekávané vedlejší účinky. Během procesu budeme vrátit zpět poslední migrace v místní databázi a použijte ho znovu s aktualizovanými metadaty.

Poslední migrace je potřeba jenom ho v místním kódu základní neexistují žádná omezení číslo nebo pořadí migrace, které se pokračovat. Může existovat více migrací z více různých vývojářů a použít stejný postup – jste právě byla se díváme na dvě aby bylo možné zachovat jednoduché.

Následující postup je možné pro tento přístup, od doby, je dobré si uvědomit, že máte změny, které je třeba synchronizovat ze správy zdrojového kódu.

1.  Zajistěte, aby že byla zapsána změny čekající na vyřízení modelu v vašeho základu kódu místní migrace. Tento krok zajistí, že pokud jde o generování prázdné migrace dobu Nenechte si ujít všechny oprávněné změny.
2.  Synchronizace se správou zdrojového kódu.
3.  Spustit **aktualizace databáze** provádět žádné nové migrace, které ostatní vývojáři se změnami.
    **
    *Poznámka: *** Pokud neobdržíte žádné upozornění z příkazu Update-databáze a nebyly žádné nové migrace od jiných vývojářů a není nutné provádět žádné další sloučení.*
4.  Spustit **aktualizace databáze – TargetMigration &lt;druhý\_poslední\_migrace&gt;**  (v tomto příkladu jsme jste postupovali podle by to byl **aktualizace databáze – TargetMigration AddRating**). Tato role databáze zpět do stavu druhého naposledy migrace – efektivně "bez použití" poslední migrace z databáze.
    **
    *Poznámka: *** Tento krok je nutný k němu můžete bezpečně upravovat metadata migrace, protože metadata svazku uloží taky \_ \_MigrationsHistoryTable databáze. To je důvod, proč byste měli používat jenom tato možnost při poslední migrace pouze ve vaší místní základu kódu. Pokud poslední migrace použít jiné databáze by také muset je vrátit zpět a znovu použít posledních migrací pro aktualizaci metadat.* 
5.  Spustit **přidat migrace &lt;úplné\_název\_včetně\_časové razítko\_z\_poslední\_migrace** &gt; (v příkladu jste postupovali podle jsme to by měl vypadat **přidat migrace 201311062215252\_AddReaders**).
    **
    *Poznámka: *** bude muset zahrnovat časové razítko, aby migrace věděla, které chcete upravit existující migrace namísto generování nové.*
    Tím se aktualizují metadata pro poslední migraci tak, aby odpovídaly aktuálním modelu. Pokud se příkaz dokončí, ale to je přesně to co chcete, zobrazí se následující upozornění. "*Pouze návrháře kódu pro migraci" 201311062215252\_byla znovu vygenerované AddReaders'. Chcete-li znovu generování uživatelského rozhraní celé migrace, použijte parametr - Force. "*
6.  Spustit **aktualizace databáze** znovu použít nejnovější migrace s aktualizovanými metadaty.
7.  Pokračovat ve vývoji, nebo odešlete do správy zdrojového kódu (po spuštění jednotky samozřejmě testy).

Tady je stav pro vývojáře \#2 je místní kódové základny po použití tohoto přístupu.

![UpdatedMetadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Souhrn

Zde jsou některé běžné problémy při použití migrace Code First v prostředí team. Ale základní znalosti o fungování migrace a některé jednoduché přístupy k řešení konfliktů při sloučení usnadňují vyřešit tyto problémy.

Základní problém je nesprávná metadata uložená v nejnovější migrace. To způsobí, že Code First nesprávně zjistit, že se neshodují současného modelu a schématu databáze a generovat uživatelské rozhraní pro další migrace nesprávný kód. Tato situace je možné překonat generování prázdné migrace s modelem správný nebo aktualizace metadat nejnovější migrace.
