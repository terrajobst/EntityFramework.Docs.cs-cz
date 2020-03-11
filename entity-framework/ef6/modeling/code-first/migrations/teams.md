---
title: Migrace Code First v týmových prostředích – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: b3c4c35d636caf4ddd251dd78e026587abc57d42
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418887"
---
# <a name="code-first-migrations-in-team-environments"></a>Migrace Code First v týmových prostředích
> [!NOTE]
> V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích. Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Navýšení kávy, je nutné si přečíst celý článek.

Problémy v týmovém prostředí jsou většinou sloučeny s migrací, když dva vývojáři vygenerovali migrace v jejich místní základu kódu. Postup řešení je poměrně jednoduchý, ale vyžaduje, abyste měli přehled o tom, jak migrace fungují. Nepřeskakujte se prosím na konec – Věnujte čas načtení celého článku, abyste měli jistotu, že budete úspěšně.

## <a name="some-general-guidelines"></a>Některé obecné pokyny

Předtím, než jsme DIGI, jak spravovat migrace vygenerované více vývojáři, tady jsou některé obecné pokyny pro nastavení úspěchu.

### <a name="each-team-member-should-have-a-local-development-database"></a>Každý člen týmu by měl mít místní vývojovou databázi.

Migrace používá tabulku **\_\_MigrationsHistory** k ukládání, které migrace byly použity v databázi. Pokud máte několik vývojářů, kteří při pokusu o zacílení na stejnou databázi nastavili různé migrace (a tak nasdíleli **\_\_tabulka MigrationsHistory** ), budou migrace značně matoucí.

Samozřejmě, pokud máte členy týmu, kteří negenerují migrace, neexistuje žádný problém, který by měl sdílet centrální vývojovou databázi.

### <a name="avoid-automatic-migrations"></a>Nepoužívejte automatické migrace

Dolním řádkem je, že automatické migrace zpočátku vypadají dobře v týmových prostředích, ale ve skutečnosti nefungují. Pokud chcete zjistit, proč, zachovat čtení – Pokud ne, můžete přejít k další části.

Automatické migrace umožňují aktualizaci schématu databáze tak, aby odpovídala aktuálnímu modelu, aniž by bylo nutné generovat soubory kódu (migrace založené na kódu). Automatické migrace by v prostředí týmu fungovaly velmi dobře, pokud jste je použili jenom někdy a nikdy negenerovali žádné migrace založené na kódu. Problémem je to, že automatické migrace jsou omezené a nezpracovávají počet operací – přejmenování vlastností nebo sloupců, přesun dat do jiné tabulky atd. Pro zpracování těchto scénářů je třeba provést generování migrace na základě kódu (a úpravou vygenerovaného kódu), které jsou smíchány mezi změnami, které jsou zpracovávány pomocí automatických migrací. Díky tomu bude téměř nemožné sloučit změny, když se dva vývojáři budou moci vrátit migrace.

## <a name="screencasts"></a>Screencasty

Pokud místo toho chcete sledovat záznam dění na záznamovém počítači, než je tento článek přečetl, následující dvě videa se týkají stejného obsahu jako tento článek.

### <a name="video-one-migrations---under-the-hood"></a>Video One: "migrace – pod digestoř"

[Tento záznam dění](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) popisuje, jak migrace sleduje a používá informace o modelu pro detekci změn modelu.

### <a name="video-two-migrations---team-environments"></a>Video dvě: migrace – Týmová prostředí

[Tento záznam dění](https://channel9.msdn.com/blogs/ef/migrations-team-environments) na základě konceptů z předchozího videa pokrývá problémy, které vznikají v týmovém prostředí, a jak je řešit.

## <a name="understanding-how-migrations-works"></a>Vysvětlení fungování migrací

Klíčem k úspěšnému použití migrace v týmovém prostředí je základní porozumění způsobu, jakým migrace sleduje a používá informace o modelu pro detekci změn modelu.

### <a name="the-first-migration"></a>První migrace

Když přidáte první migraci do projektu, spustíte něco jako **Přidání – migrace jako první** v konzole správce balíčků. Kroky vysoké úrovně, které tento příkaz provede, jsou na obrázku níže.

![První migrace](~/ef6/media/firstmigration.png)

Aktuální model se vypočítá z vašeho kódu (1). Požadované databázové objekty se pak vypočítávají pomocí modelu, který se liší (2) – protože se jedná o první migraci, model se liší jenom pomocí prázdného modelu pro porovnání. Požadované změny jsou předány generátoru kódu pro sestavení požadovaného kódu migrace (3), který je poté přidán do řešení aplikace Visual Studio (4).

Kromě samotného kódu migrace, který je uložen v souboru hlavního kódu, migrace také generuje další soubory kódu na pozadí. Tyto soubory jsou metadata, která jsou používána migracemi a nejsou něco, co byste měli upravovat. Jeden z těchto souborů je soubor prostředků (. resx), který obsahuje snímek modelu v době, kdy byla migrace vygenerována. V dalším kroku uvidíte, jak se to používá.

V tomto okamžiku by se vám pravděpodobně spouštěla **aktualizace-Database** , aby se změny projevily v databázi, a pak se podívejte na implementaci jiných oblastí aplikace.

### <a name="subsequent-migrations"></a>Následné migrace

Později se vrátíte zpátky a provedete změny modelu – v našem příkladu přidáme do **blogu**vlastnost **URL** . Pak budete mít příkaz, jako je například **Add-Migration AddUrl** , k vytvoření uživatelského rozhraní migrace k použití odpovídajících změn databáze. Kroky vysoké úrovně, které tento příkaz provede, jsou na obrázku níže.

![Druhá migrace](~/ef6/media/secondmigration.png)

Stejně jako v poslední době se aktuální model vypočítá z kódu (1). Tentokrát ale existují migrace, takže předchozí model se načte z poslední migrace (2). U těchto dvou modelů se nezměnily požadované změny databáze (3) a pak se proces dokončí jako dřív.

Stejný postup se používá pro všechny další migrace, které přidáte do projektu.

### <a name="why-bother-with-the-model-snapshot"></a>Proč bother pomocí snímku modelu?

Možná vás zajímá, proč EF oba s snímkem modelu – nestačí se podívat na databázi. Pokud ano, přečtěte si. Pokud vás zajímáte, můžete tuto část přeskočit.

Existuje mnoho důvodů, proč EF zachovává snímek modelu:

-   Umožňuje, aby se vaše databáze od modelu EF naunášena. Tyto změny lze provést přímo v databázi, nebo můžete změnit kód vygenerovaný v rámci migrace, aby se změny projevily. Tady je pár příkladů tohoto postupu v praxi:
    -   Chcete přidat vložené a aktualizované sloupce do jedné nebo více tabulek, ale nechcete tyto sloupce zahrnout do modelu EF. Pokud se migrace prohlédla v databázi, při každém vygenerování této migrace se neustále snaží tyto sloupce odstranit. Pomocí snímku modelu v EF se někdy jenom zjišťují legitimní změny modelu.
    -   Chcete změnit tělo úložné procedury, která se používá pro aktualizace k zahrnutí nějakého protokolování. Pokud se migrace prohlédla z databáze na tuto uloženou proceduru, bude se neustále zkoušet a obnoví se do definice, kterou EF očekává. Pomocí snímku modelu bude EF v případě, že změníte tvar procedury v modelu EF, pouze kód generování kódu pro změnu uložené procedury.
    -   Tyto stejné zásady platí pro přidání dalších indexů, včetně dalších tabulek v databázi, mapování EF na zobrazení databáze, které se nachází v tabulce atd.
-   Model EF obsahuje více než jen tvar databáze. Když máte celý model, umožníte migraci sledovat informace o vlastnostech a třídách v modelu a o tom, jak se mapují na sloupce a tabulky. Tyto informace umožňují, aby migrace byly inteligentnější v kódu, který generuje generátor IT. Pokud například změníte název sloupce, který vlastnost mapuje na migrace, může přejmenovat zjištění, že se jedná o stejnou vlastnost – něco, co nejde udělat, pokud máte jenom schéma databáze. 

## <a name="what-causes-issues-in-team-environments"></a>Co způsobuje problémy v týmových prostředích

Pracovní postup, který je popsaný v předchozí části, funguje skvěle při práci na aplikaci s jedním vývojářem. Pracuje také dobře v týmovém prostředí, pokud jste jedinou osobou, která provádí změny v modelu. V tomto scénáři můžete provádět změny modelu, generovat migrace a odesílat je do správy zdrojového kódu. Ostatní vývojáři mohou synchronizovat vaše změny a spustit **Update-Database** , aby byly změny schématu aplikovány.

Problémy se začnou vyskytnout, když máte více vývojářů, kteří provádějí změny v modelu EF a odesílají je do správy zdrojového kódu ve stejnou dobu. To, co chybí EF, je první třída způsob, jak sloučit místní migrace s migracemi, které od poslední synchronizace odeslal jiný vývojář do správy zdrojových kódů.

## <a name="an-example-of-a-merge-conflict"></a>Příklad konfliktu sloučení

Nejprve se podíváme na konkrétní příklad takového konfliktu sloučení. Budeme pokračovat s příkladem, který jsme si vyhledali dřív. Jako výchozí bod můžeme předpokládat, že změny z předchozí části vrátil původní vývojář. Sledujeme dva vývojáře při provádění změn v základu kódu.

Sledujeme model EF a migrace s tím, že se podíváme na několik změn. Pro výchozí bod se oba vývojáři synchronizovaly do úložiště správy zdrojového kódu, jak je znázorněno na následujícím obrázku.

![Počáteční bod](~/ef6/media/startingpoint.png)

Vývojář \#1 a vývojář \#2 teď provede některé změny modelu EF v rámci svého základu kódu. Developer \#1 přidá do **blogu** vlastnost **hodnocení** a vygeneruje migraci **AddRating** , která použije změny v databázi. Developer \#2 přidá vlastnost **čtenářů** do **blogu** – a vygeneruje odpovídající migraci **AddReaders** . Oba vývojáři spustí **příkaz Update-Database**, aby se změny projevily v místních databázích a pak pokračovali v vývoji aplikace.

> [!NOTE]
> Migrace mají předponu s časovým razítkem, takže naše grafika představuje, že migrace AddReaders od vývojáře \#2 pochází po migraci AddRating z Developer \#1. Bez ohledu na to, jestli vývojář \#1 nebo \#2 vytvoří migraci za prvé, neprovede žádný rozdíl na problémech práce v týmu nebo proces jejich sloučení, které se zobrazí v další části.

![Místní změny](~/ef6/media/localchanges.png)

Pro vývojáře \#1 se jedná o štěstíý den, protože se k tomu poprvé odesílají změny. Vzhledem k tomu, že nikdo jiný nebyl vrácen se změnami, protože synchronizoval své úložiště, může pouze odeslat své změny bez provedení sloučení.

![Odeslat](~/ef6/media/submit.png)

Teď je čas, kdy se má vývojář \#2 odeslat. Nejsou tak štěstíy. Vzhledem k tomu, že někdo jiný odeslal změny od synchronizace, bude muset stáhnout změny a sloučit je. Systém správy zdrojů bude pravděpodobně moci automaticky sloučit změny na úrovni kódu, protože jsou velmi jednoduché. Stav místního úložiště Developer \#2 po synchronizaci je znázorněný na následujícím obrázku. 

![O přijetí změn](~/ef6/media/pull.png)

V této fázi vývojář \#2 může spustit rutinu **Update-Database** , která detekuje novou migraci **AddRating** (nepoužila se pro databázi vývojáře \#2) a použije ji. Nyní se sloupec **hodnocení** přidá do tabulky **Blogy** a databáze je synchronizována s modelem.

Existuje několik problémů, i když:

1.  I když **aktualizace databáze** použije migraci **AddRating** , vyvolá taky upozornění: *databáze se nedá aktualizovat tak, aby odpovídala aktuálnímu modelu, protože existují nedokončené změny a Automatická migrace je zakázaná...*
    Problémem je, že snímek modelu uložený během poslední migrace (**AddReader**) postrádá vlastnost **hodnocení** na **blogu** (protože není součástí modelu při vygenerování migrace). Code First zjistí, že model v poslední migraci neodpovídá aktuálnímu modelu a vyvolá upozornění.
2.  Spuštění aplikace bude mít za následek, že*se od vytvoření databáze změnil model zálohování kontextu ' BloggingContext '. Zvažte použití Migrace Code First k aktualizaci databáze... "*
    Tento problém je opět stejný jako snímek modelu uložený během poslední migrace neodpovídá aktuálnímu modelu.
3.  Nakonec by se čekalo, že spuštění příkazu **Add-Migration** by nyní vygenerovalo prázdnou migraci (vzhledem k tomu, že v databázi nejsou žádné změny). Ale vzhledem k tomu, že migrace porovnává aktuální model od poslední migrace (ve které chybí vlastnost **hodnocení** ), bude ve skutečnosti vygenerované jiné volání **AddColumn** , které se přidá do sloupce **hodnocení** . Tato migrace samozřejmě během **aktualizace databáze** selže, protože sloupec **hodnocení** již existuje.

## <a name="resolving-the-merge-conflict"></a>Řešení konfliktu sloučení

Dobrá zpráva je, že není příliš těžko se zabývat sloučením ručně – Pokud jste obeznámeni s tím, jak migrace fungují. Takže pokud jste se přeskočili do této části... je nám líto, ale musíte se vrátit a přečíst si zbytek článku jako první!

K dispozici jsou dvě možnosti, nejjednodušší je vygenerovat prázdnou migraci, která má správný aktuální model jako snímek. Druhou možností je aktualizovat snímek v poslední migraci tak, aby měl správný snímek modelu. Druhá možnost je trochu obtížnější a nedá se použít v každém scénáři, ale je také čisticí, protože nezahrnuje přidání další migrace.

### <a name="option-1-add-a-blank-merge-migration"></a>Možnost 1: Přidání prázdné migrace ' sloučit '

V této možnosti vygenerujeme prázdnou migraci výhradně za účelem zajištění toho, aby poslední migrace měla uložený správný snímek modelu.

Tato možnost se dá použít bez ohledu na to, kdo poslední migraci vygeneroval. V příkladu, který jsme \#2, se postará o sloučení a při generování poslední migrace k nim došlo. Tyto stejné kroky je ale možné použít, pokud vývojář \#1 vygeneroval poslední migraci. Tento postup platí také v případě, že je zapojeno více migrací – právě jsme prohledali dvě, aby bylo snadné je zachovat.

Následující postup lze použít pro tento přístup, počínaje od okamžiku, kdy jste si uvědomili změny, které je třeba synchronizovat ze správy zdrojového kódu.

1.  Zajistěte, aby všechny změny modelu probíhajících změn v místní základu kódu byly zapsány do migrace. Tento krok zajistí, že při vygenerování prázdné migrace nepřijdete o žádné legitimní změny.
2.  Synchronizace se správou zdrojových kódů.
3.  Spusťte **příkaz Update-Database** a použijte přitom všechny nové migrace, které jiní vývojáři vrátili se změnami.
    **_Poznámka:_** *Pokud neobdržíte žádná upozornění z příkazu Update-Database, neexistují žádné nové migrace od jiných vývojářů a není nutné provádět žádné další sloučení.*
4.  Spusťte příkaz **Add-Migration &lt;vyberte\_\_název&gt; – IgnoreChanges** (například **sloučení migrace – IgnoreChanges**). Tím se vygeneruje migrace se všemi metadaty (včetně snímku aktuálního modelu), ale ignoruje všechny změny, které detekuje při porovnávání aktuálního modelu s snímkem během poslední migrace (což **znamená, že získáte prázdnou a** **nižší** metodu).
5.  Pokračujte v vývoji nebo odešlete do správy zdrojového kódu (po spuštění testování částí kurzu).

Tady je stav základu místního kódu pro vývojáře \#2 po použití tohoto přístupu.

![Sloučení migrace](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Možnost 2: aktualizace snímku modelu při poslední migraci

Tato možnost je velmi podobná možnosti 1, ale odstraní dodatečnou prázdnou migraci – protože Pojďme na ni dát další soubory kódu v jejich řešení.

**Tento přístup je vhodný pouze v případě, že poslední migrace existuje pouze v rámci místního kódu a ještě nebyla odeslána do správy zdrojových kódů (například pokud byla poslední migrace generována uživatelem, který provádí sloučení)** . Úprava metadat migrace, které jiní vývojáři již použili pro svou vývojovou databázi – nebo dokonce horšího použití na provozní databázi – můžou způsobit neočekávané vedlejší účinky. Během procesu vrátíme zpátky poslední migraci v naší místní databázi a znovu ji použijeme s aktualizovanými metadaty.

I když by poslední migrace měla být jenom v základu místního kódu, neexistují žádná omezení na počet nebo pořadí migrací, které to dostanou. Může se jednat o více migrací od různých vývojářů a stejný postup, jako když jsme se seznámili ještě dvakrát.

Následující postup lze použít pro tento přístup, počínaje od okamžiku, kdy jste si uvědomili změny, které je třeba synchronizovat ze správy zdrojového kódu.

1.  Zajistěte, aby všechny změny modelu probíhajících změn v místní základu kódu byly zapsány do migrace. Tento krok zajistí, že při vygenerování prázdné migrace nepřijdete o žádné legitimní změny.
2.  Proveďte synchronizaci se správou zdrojových kódů.
3.  Spusťte **příkaz Update-Database** a použijte přitom všechny nové migrace, které jiní vývojáři vrátili se změnami.
    **_Poznámka:_** *Pokud neobdržíte žádná upozornění z příkazu Update-Database, neexistují žádné nové migrace od jiných vývojářů a není nutné provádět žádné další sloučení.*
4.  Spusťte **rutinu Update-Database – TargetMigration &lt;druhé\_poslední\_&gt;migrace** (v příkladu jsme to udělali jako **Update-Database – TargetMigration AddRating**). Tím se databáze role vrátí do stavu druhé poslední migrace – efektivně nepoužívá poslední migraci z databáze.
    **_Poznámka:_** *Tento krok je nutný, aby bylo bezpečné upravit metadata migrace, protože metadata jsou také uložena v \_\_MigrationsHistoryTable databáze. Z tohoto důvodu byste měli tuto možnost používat jenom v případě, že se poslední migrace používá jenom v místním základu kódu. Pokud se poslední migrace použila u jiných databází, museli byste je také vrátit zpátky a znovu použít poslední migraci, aby se metadata aktualizovala.* 
5.  Spusťte příkaz **Add-migration &lt;full\_name\_včetně\_časového razítka\_\_poslední\_migrace**&gt; (v příkladu jsme to udělali jako rutina **add-Migration 201311062215252\_AddReaders**).
    **_Poznámka:_** *je potřeba zahrnout časové razítko, aby migrace věděly, že chcete upravit existující migraci, a ne vytvořit nové rozhraní.*
    Tím se aktualizují metadata poslední migrace tak, aby odpovídala aktuálnímu modelu. Až se příkaz dokončí, zobrazí se následující upozornění, které je přesně to, co chcete. *Jenom kód návrháře pro migraci 201311062215252\_AddReaders se znovu vygeneroval. K opětovnému vygenerování uživatelského rozhraní pro celou migraci použijte parametr-Force.*
6.  Spuštěním rutiny **Update-Database** znovu nainstalujte nejnovější migraci s aktualizovanými metadaty.
7.  Pokračujte v vývoji nebo odešlete do správy zdrojového kódu (po spuštění testování částí kurzu).

Tady je stav základu místního kódu pro vývojáře \#2 po použití tohoto přístupu.

![Aktualizovaná metadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Souhrn

Při použití Migrace Code First v týmovém prostředí dochází k problémům. Základní informace o tom, jak migrace fungují, a některé jednoduché přístupy k řešení konfliktů při sloučení usnadňují jejich překonání těchto problémů.

Základní problém je nesprávná metadata uložená v nejnovější migraci. To způsobí, že Code First nesprávně zjistit, jestli se aktuální model a schéma databáze neshodují, a v další migraci na nesprávný kód uživatelského rozhraní. Tato situace se dá překonat vygenerováním prázdné migrace se správným modelem nebo aktualizací metadat v rámci nejnovější migrace.
