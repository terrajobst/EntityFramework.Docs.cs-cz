---
title: Požadavky na výkon pro EF4, EF5 a EF6-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419445"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Požadavky na výkon pro EF 4, 5 a 6
Autorem David Obando, Eric Dettinger a ostatními

Publikováno: duben 2012

Poslední aktualizace: květen 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Úvod

Objektově-relační rozhraní mapování jsou pohodlným způsobem, jak poskytnout abstrakci pro přístup k datům v objektově orientované aplikaci. V případě aplikací .NET je Entity Framework doporučena technologie Microsoft pro/RM. V případě jakékoli abstrakce se může výkon stát problémem.

Tento dokument White Paper byl napsán tak, aby zobrazoval požadavky na výkon při vývoji aplikací pomocí Entity Framework, aby vývojáři poskytovali představu o Entity Frameworkch vnitřních algoritmech, které mohou ovlivnit výkon, a poskytnout tipy k vyšetřování a zlepšení výkonu ve svých aplikacích, které používají Entity Framework. K dispozici je řada dobrých témat o výkonu, které jsou již na webu k dispozici, a také jsme se na tyto prostředky snažili odkazovat.

Výkon je obtížné téma. Tento dokument White Paper je určený jako prostředek, který vám může usnadnit rozhodování související s výkonem pro vaše aplikace, které používají Entity Framework. Zahrnuli jsme několik testovacích metrik, které demonstrují výkon, ale tyto metriky nejsou určené jako absolutní indikátory výkonu, které se zobrazí ve vaší aplikaci.

Pro praktické účely tento dokument předpokládá, Entity Framework 4 je spuštěný v rozhraní .NET 4,0 a Entity Framework 5 a 6 se spouští pod .NET 4,5. Mnohé z vylepšení výkonu pro Entity Framework 5 se nacházejí v rámci základních komponent, které se dodávají s .NET 4,5.

Entity Framework 6 je verze mimo IP síť a nezávisí na Entity Frameworkch součástech, které se dodávají s .NET. Entity Framework 6 funguje na rozhraní .NET 4,0 i .NET 4,5 a může nabídnout velký výkon pro uživatele, kteří se neupgradovali z .NET 4,0, ale chtějí ve svých aplikacích využívat nejnovější Entity Framework bity. Pokud se tento dokument zmiňuje Entity Framework 6, odkazuje na nejnovější verzi, která je k dispozici v době psaní tohoto dokumentu: verze 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. studené a teplé provádění dotazů

Velmi poprvé se každý dotaz provede proti danému modelu, Entity Framework provede spoustu práce na pozadí pro načtení a ověření modelu. Často odkazujeme na tento první dotaz jako na studený dotaz.  Další dotazy na již načtený model jsou označovány jako "teplé" dotazy a jsou mnohem rychlejší.

Pojďme se podívat na podrobný pohled na čas strávený prováděním dotazu pomocí Entity Framework a zjistit, kde se vylepšit v Entity Framework 6.

**První spuštění dotazu – studený dotaz**

| Kódování uživatelských zápisů                                                                                     | Akce                    | Dopad na výkon EF4                                                                                                                                                                                                                                                                                                                                                                                                        | Dopad na výkon EF5                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Dopad na výkon EF6                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Vytvoření kontextu          | Střednědobé používání                                                                                                                                                                                                                                                                                                                                                                                                                        | Střednědobé používání                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Vytvoření výrazu dotazu | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                           | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Provádění dotazů LINQ      | -Načítání metadat: vysoké, ale v mezipaměti <br/> -Zobrazení generace: potenciálně velmi vysoké, ale v mezipaměti <br/> -Vyhodnocení parametrů: střední <br/> -Překlad dotazů: střední <br/> -Materializer generace: střední, ale v mezipaměti <br/> – Provádění databázového dotazu: potenciálně vysoké <br/> + Připojení. otevřít <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materializace objektu: střední <br/> -Vyhledávání identity: střední | -Načítání metadat: vysoké, ale v mezipaměti <br/> -Zobrazení generace: potenciálně velmi vysoké, ale v mezipaměti <br/> -Vyhodnocení parametrů: nízká <br/> – Překlad dotazů: střední, ale v mezipaměti <br/> -Materializer generace: střední, ale v mezipaměti <br/> – Provádění databázových dotazů: potenciálně vysoké (lepší dotazy v některých situacích) <br/> + Připojení. otevřít <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materializace objektu: střední <br/> -Vyhledávání identity: střední | -Načítání metadat: vysoké, ale v mezipaměti <br/> -View Generation: Medium, ale v mezipaměti <br/> -Vyhodnocení parametrů: nízká <br/> – Překlad dotazů: střední, ale v mezipaměti <br/> -Materializer generace: střední, ale v mezipaměti <br/> – Provádění databázových dotazů: potenciálně vysoké (lepší dotazy v některých situacích) <br/> + Připojení. otevřít <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materializace objektu: střední (rychlejší než EF5) <br/> -Vyhledávání identity: střední |
| `}`                                                                                                  | Připojení. Zavřít          | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                           | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Druhé provádění dotazů – Rychlý dotaz**

| Kódování uživatelských zápisů                                                                                     | Akce                    | Dopad na výkon EF4                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Dopad na výkon EF5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Dopad na výkon EF6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Vytvoření kontextu          | Střednědobé používání                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Střednědobé používání                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Vytvoření výrazu dotazu | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Provádění dotazů LINQ      | -Vyhledání metadat při ~~načítání~~ : ~~vysoká, ale~~ nízká úroveň mezipaměti <br/> -Vyhledávání ~~generace~~ zobrazení: ~~potenciálně velmi vysoké, ale málo v mezipaměti~~ <br/> -Vyhodnocení parametrů: střední <br/> – Vyhledávání ~~překladu~~ dotazů: střední <br/> – Vyhledávání ~~generace~~ materializer: ~~střední, ale nedostatek paměti~~ <br/> – Provádění databázového dotazu: potenciálně vysoké <br/> + Připojení. otevřít <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materializace objektu: střední <br/> -Vyhledávání identity: střední | -Vyhledání metadat při ~~načítání~~ : ~~vysoká, ale~~ nízká úroveň mezipaměti <br/> -Vyhledávání ~~generace~~ zobrazení: ~~potenciálně velmi vysoké, ale málo v mezipaměti~~ <br/> -Vyhodnocení parametrů: nízká <br/> – Vyhledávání ~~překladu~~ dotazů: ~~střední, ale málo v mezipaměti~~ <br/> – Vyhledávání ~~generace~~ materializer: ~~střední, ale nedostatek paměti~~ <br/> – Provádění databázových dotazů: potenciálně vysoké (lepší dotazy v některých situacích) <br/> + Připojení. otevřít <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materializace objektu: střední <br/> -Vyhledávání identity: střední | -Vyhledání metadat při ~~načítání~~ : ~~vysoká, ale~~ nízká úroveň mezipaměti <br/> -Vyhledávání ~~generace~~ zobrazení: ~~střední, ale nedostatek paměti~~ <br/> -Vyhodnocení parametrů: nízká <br/> – Vyhledávání ~~překladu~~ dotazů: ~~střední, ale málo v mezipaměti~~ <br/> – Vyhledávání ~~generace~~ materializer: ~~střední, ale nedostatek paměti~~ <br/> – Provádění databázových dotazů: potenciálně vysoké (lepší dotazy v některých situacích) <br/> + Připojení. otevřít <br/> + Command. ExecuteReader <br/> + DataReader. Read <br/> Materializace objektu: střední (rychlejší než EF5) <br/> -Vyhledávání identity: střední |
| `}`                                                                                                  | Připojení. Zavřít          | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Existuje několik způsobů, jak snížit náklady na výkon u studených i tepléch dotazů, a my se podíváme na tyto možnosti v následující části. Konkrétně se podíváme na to, jak snížit náklady na načítání modelů v studených dotazech pomocí předem vygenerovaných zobrazení, která by měla přispět ke zmírnění bolesti při vytváření zobrazení. V případě rychlých dotazů pokryjeme ukládání plánů dotazů do mezipaměti, žádné sledovací dotazy a různé možnosti spuštění dotazů.

### <a name="21-what-is-view-generation"></a>2,1 co je generování zobrazení?

Aby bylo možné pochopit, co je generování zobrazení, je nutné nejprve pochopit, co jsou zobrazení mapování. Zobrazení mapování jsou spustitelné reprezentace transformací určených v mapování pro každou sadu entit a přidružení. Interní tato zobrazení mapování přebírají tvar CQTs (kanonické stromy dotazů). Existují dva typy zobrazení mapování:

-   Zobrazení dotazů: představuje transformaci nutnou k přechodu ze schématu databáze do koncepčního modelu.
-   Zobrazení aktualizací: představuje transformaci nutnou k přechodu z koncepčního modelu do schématu databáze.

Mějte na paměti, že koncepční model se může od schématu databáze lišit různými způsoby. Například jedna jedna tabulka může být použita k uložení dat pro dva různé typy entit. Dědičnost a jiné než triviální mapování hrají roli ve složitosti zobrazení mapování.

Proces výpočtu těchto zobrazení na základě specifikace mapování je to, co voláme na generování zobrazení. Generování zobrazení může být provedeno dynamicky při načtení modelu nebo v čase sestavení pomocí "předem vygenerované zobrazení"; Druhá je serializovaná ve formě Entity SQLch příkazů do souboru jazyka C\# nebo VB.

Při generování zobrazení jsou také ověřeny. Z hlediska výkonu je velká většina generování zobrazení skutečně ověřením zobrazení, která zajistí, aby připojení mezi entitami dávala smysl a aby měla správnou mohutnost pro všechny podporované operace.

Když se spustí dotaz na sadu entit, dotaz se sloučí s odpovídajícím zobrazením dotazu a výsledek tohoto složení se spustí prostřednictvím kompilátoru plánu, aby se vytvořila reprezentace dotazu, kterou může záložní úložiště pochopit. Pro SQL Server konečný výsledek této kompilace bude příkaz SELECT jazyka T-SQL. Při prvním provedení aktualizace sady entit se zobrazení aktualizace spustí pomocí podobného procesu a převede je na příkazy DML pro cílovou databázi.

### <a name="22-factors-that-affect-view-generation-performance"></a>2,2 faktory ovlivňující výkon generování zobrazení

Výkon kroku generace (zobrazení) nezávisí jenom na velikosti modelu, ale také na způsobu propojení modelu. Pokud jsou dvě entity propojeny prostřednictvím řetězce dědičnosti nebo přidružení, označují se jako připojené. Podobně pokud jsou dvě tabulky propojeny pomocí cizího klíče, jsou propojeny. Když se zvýší počet propojených entit a tabulek ve vašich schématech, zvýší se náklady na zobrazení na generaci.

Algoritmus, který používáme k vygenerování a ověření zobrazení, je exponenciální v nejhorším případě, ale k vylepšení této hodnoty používáme několik optimalizací. Největší faktory, které zdají negativně ovlivnit výkon, jsou:

-   Velikost modelu odkazující na počet entit a množství přidružení mezi těmito entitami.
-   Složitosti modelu, konkrétně dědičnost zahrnující velký počet typů.
-   Použití nezávislých přidružení namísto přidružení cizího klíče.

U malých jednoduchých modelů může být cena dostatečně malá, aby se bother pomocí předem vygenerovaných zobrazení. Při zvýšení velikosti modelu a složitosti je k dispozici několik možností, jak snížit náklady na generování a ověřování v zobrazení.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2,3 použití předem vygenerovaných zobrazení ke snížení doby načítání modelu

Podrobné informace o tom, jak používat předem vygenerovaná zobrazení na Entity Framework 6, najdete v [Předgenerovaných zobrazeních mapování](~/ef6/fundamentals/performance/pre-generated-views.md) .

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 předem vygenerovaná zobrazení pomocí edice Entity Framework Power Tools Community

Pomocí [nástroje Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) můžete vygenerovat zobrazení modelu EDMX a Code First tak, že kliknete pravým tlačítkem myši na soubor třídy modelu a pomocí nabídky Entity Framework vyberete "generovat zobrazení". Edice Entity Framework Power Tools Community pracují pouze na kontextech odvozených od DbContext.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 použití předem vygenerovaných zobrazení s modelem vytvořeným pomocí EDMGen

EDMGen je nástroj, který se dodává s .NET a pracuje s Entity Framework 4 a 5, ale ne s Entity Framework 6. EDMGen umožňuje vygenerovat soubor modelu, vrstvu objektů a zobrazení z příkazového řádku. Jeden z výstupů bude soubor zobrazení ve vašem jazyce podle volby, VB nebo C\#. Toto je soubor kódu obsahující Entity SQL fragmentů pro každou sadu entit. Chcete-li povolit předem vygenerovaná zobrazení, stačí do projektu přidat soubor.

Pokud ručně provedete úpravy souborů schématu pro model, budete muset znovu vygenerovat soubor zobrazení. To můžete provést tak, že spustíte EDMGen pomocí příznaku **/Mode: ViewGeneration** .

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 Jak používat předem vygenerovaná zobrazení se souborem EDMX

Pomocí EDMGen můžete také vygenerovat zobrazení pro soubor EDMX – dříve odkazované téma MSDN popisuje, jak přidat událost před sestavením, aby to bylo možné, ale to je složité a v některých případech je to možné. Obecně je snazší použít šablonu T4 k vygenerování zobrazení, když je model v souboru EDMX.

Blog týmu ADO.NET obsahuje příspěvek, který popisuje, jak používat šablonu T4 pro generování zobrazení (\<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Tento příspěvek zahrnuje šablonu, kterou lze stáhnout a přidat do projektu. Šablona byla napsána pro první verzi Entity Framework, takže není zaručena práce s nejnovějšími verzemi Entity Framework. Můžete si však stáhnout aktuálnější sadu šablon pro generování zobrazení pro Entity Framework 4 a 5from galerii sady Visual Studio:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   \#jazyka C: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Pokud používáte Entity Framework 6, můžete z galerie sady Visual Studio získat šablony T4 pro generování zobrazení v \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2,4 snížení nákladů na generaci zobrazení

Použití předem vygenerovaných zobrazení přesouvá náklady na generaci zobrazení z načítání modelů (doba běhu) do doby návrhu. I když to zlepšuje výkon při spuštění za běhu, pořád při vývoji dojde k bolesti generace zobrazení. Existuje několik dalších zdvihů, které mohou snížit náklady na generaci zobrazení, a to jak v době kompilace, tak i v době běhu.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 použití přidružení cizího klíče k omezení nákladů na generaci zobrazení

Zjistili jsme několik případů, kdy při přepínání přidružení v modelu z nezávislých přidružení k přidružení cizího klíče došlo k výraznému zlepšení času stráveného při generování zobrazení.

K předvedení tohoto vylepšení jsme vygenerovali dvě verze modelu Navision pomocí EDMGen. *Poznámka: Popis modelu Navision naleznete v příloze C.* Model Navision je pro toto cvičení zajímavě z důvodu jeho velmi velkého množství entit a vztahů mezi nimi.

Jedna verze tohoto velmi velkého modelu byla generována s přidruženími cizích klíčů a druhá byla vygenerována s nezávislými přidruženími. Doba, po kterou trvalo generování zobrazení pro každý model, pak vyprší. Entity Framework 5 test použil metodu GenerateViews () z třídy EntityViewGenerator pro generování zobrazení, zatímco test Entity Framework 6 používal metodu GenerateViews () z třídy StorageMappingItemCollection. Z důvodu restrukturalizace kódu, ke které došlo v základu kódu Entity Framework 6.

Při použití Entity Framework 5 se generace zobrazení pro model s cizími klíči na testovacím počítači trvala 65 minut. Není známo, jak dlouho trvalo generování zobrazení pro model, který používal nezávislá přidružení. V našem testovacím prostředí jsme ponechali běžet za měsíc, než se počítač restartoval v našem testovacím prostředí, aby se nainstalovaly měsíční aktualizace.

Při použití Entity Framework 6 se ve stejném testovacím počítači trvalo 28 sekund ve generaci modelu s cizími klíči. Generování zobrazení modelu, který používá nezávislé přidružení, trvalo 58 sekund. Vylepšení Entity Framework 6 ve svém kódu generování zobrazení znamenají, že mnoho projektů nebude potřebovat předem vygenerovaná zobrazení k získání rychlejšího spuštění.

Je důležité přeznačit, že předběžně vygenerování zobrazení v Entity Framework 4 a 5 se dá provést pomocí EDMGen nebo Entity Framework nástrojů Power Tools. Pro Entity Framework 6 se generování zobrazení dá provést prostřednictvím Entity Framework nástrojů Power Tools nebo programově, jak je popsané v [Předgenerovaných zobrazeních mapování](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1, jak používat cizí klíče místo nezávislých přidružení

Při použití EDMGen nebo Entity Designer v aplikaci Visual Studio získáte FKs ve výchozím nastavení a pro přepínání mezi FKs a službou IAs stačí pouze jeden příznak CheckBox nebo příkazového řádku.

Pokud máte velký Code First model, bude použití nezávislých přidružení mít stejný účinek na generování zobrazení. Tomuto dopadu se můžete vyhnout zahrnutím vlastností cizích klíčů na třídy pro závislé objekty, i když někteří vývojáři to považují za znečišťující jejich objektový model. Další informace o tomto tématu najdete v \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Při použití      | Postup                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | Po přidání přidružení mezi dvěma entitami se ujistěte, že máte referenční omezení. Referenční omezení určují Entity Framework použití cizích klíčů namísto nezávislých přidružení. Další podrobnosti najdete \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Při použití EDMGen k vygenerování souborů z databáze se vaše cizí klíče budou dodržovat a přidají se do modelu jako takové. Další informace o různých možnostech, které zveřejňuje EDMGen, najdete v [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Informace o tom, jak zahrnout vlastnosti cizích klíčů do závislých objektů při použití Code First, najdete v části "konvence vztahu" v tématu [konvence Code First](~/ef6/modeling/code-first/conventions/built-in.md) .                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 přesunutí modelu do samostatného sestavení

Když je model zahrnut přímo do projektu aplikace a Vy vygenerujete zobrazení prostřednictvím události před sestavením nebo šablony T4, generování a ověřování se provedou, když se projekt znovu vytvoří, i když se model nezměnil. Pokud model přesunete do samostatného sestavení a odkazujete ho z projektu vaší aplikace, můžete provést další změny v aplikaci, aniž byste museli znovu sestavit projekt obsahující model.

*Poznámka:*  při přesunu modelu do samostatných sestavení Nezapomeňte zkopírovat připojovací řetězce pro model do konfiguračního souboru aplikace klientského projektu.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 zakázání ověřování modelu založeného na EDMX

Modely EDMX jsou ověřovány v době kompilace, a to i v případě, že model není změněn. Pokud váš model již byl ověřen, můžete potlačit ověřování v době kompilace nastavením vlastnosti "ověřit při sestavení" na hodnotu false v okně Vlastnosti. Když změníte mapování nebo model, můžete dočasně znovu povolit ověřování a ověřit provedené změny.

Zvýšení výkonu bylo provedeno v Entity Framework Designer pro Entity Framework 6 a náklady na ověření při sestavení jsou mnohem nižší než v předchozích verzích návrháře.

## <a name="3-caching-in-the-entity-framework"></a>3 ukládání do mezipaměti v Entity Framework

Entity Framework má následující formy integrovaného ukládání do mezipaměti:

1.  Ukládání objektů do mezipaměti – objekt ObjectStateManager integrovaný do instance ObjectContext udržuje v paměti objekty, které byly načteny pomocí této instance. Tato možnost je také známá jako mezipaměť první úrovně.
2.  Ukládání plánu dotazů do mezipaměti – opětovné použití příkazu pro vygenerované úložiště při spuštění dotazu více než jednou.
3.  Ukládání metadat do mezipaměti – sdílení metadat pro model napříč různými připojeními ke stejnému modelu.

Kromě mezipamětí, které jsou uvedené v EF, se k rozšiřování Entity Framework s mezipamětí pro výsledky načtené z databáze dá také použít zvláštní druh zprostředkovatele dat ADO.NET, který se označuje jako poskytovatel pro zabalení, a také označovaný jako ukládání do mezipaměti druhé úrovně.

### <a name="31-object-caching"></a>Mezipaměť objektu 3,1

Ve výchozím nastavení, když se entita vrátí do výsledků dotazu, těsně předtím, než EF materializuje, ObjectContext zkontroluje, jestli už entita se stejným klíčem načetla do svého objektu ObjectStateManager. Pokud již existuje entita se stejnými klíči, bude zahrnuta do výsledků dotazu. I když EF bude i nadále vystavovat dotaz na databázi, může toto chování obejít většinu nákladů na vyhodnocování entitu víckrát.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 získání entit z mezipaměti objektů pomocí hledání DbContext

Na rozdíl od regulárního dotazu, metoda Find v Negenerickými (rozhraní API zahrnutá poprvé v EF 4,1) provede hledání v paměti před tím, než se dotaz vystaví na databázi. Je důležité si uvědomit, že dvě různé instance ObjectContext budou mít dvě různé instance ObjectStateManager, což znamená, že mají oddělené mezipaměti objektů.

Při hledání se pomocí hodnoty primárního klíče pokusí najít entitu sledovanou kontextem. Pokud entita není v kontextu, bude dotaz proveden a vyhodnocován proti databázi a vrátí hodnotu null, pokud entita nebyla nalezena v kontextu nebo v databázi. Všimněte si, že funkce Find vrátí také entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.

Při použití Find se dá vzít v úvahu výkon. Volání této metody ve výchozím nastavení spustí ověřování mezipaměti objektů za účelem zjištění změn, které jsou stále nečekají na potvrzení databáze. Tento proces může být velmi nákladný, pokud je v mezipaměti objektů velký počet objektů nebo pokud je v grafu rozsáhlých objektů přidaných do mezipaměti objektů, ale může být také zakázán. V některých případech můžete vnímat v pořadí podle velikosti rozdílu v volání metody Find, pokud zakážete automatické rozpoznávání změn. Druhé pořadí velikostí je však vnímano, když je objekt ve skutečnosti v mezipaměti, a když je nutné načíst objekt z databáze. Tady je příklad grafu s měřeními provedenými pomocí některých mikrosrovnávacích testů vyjádřených v milisekundách s zatížením 5000 entit:

![Logaritmické měřítko .NET 4,5](~/ef6/media/net45logscale.png ".NET 4,5 – logaritmické měřítko")

Příklad hledání s automatickým rozpoznáním změn je zakázaný:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Co je potřeba vzít v úvahu při použití metody Find:

1.  Pokud objekt není v mezipaměti, výhody hledání jsou negaci, ale syntaxe je stále jednodušší než dotaz podle klíče.
2.  Pokud je povoleno automatické rozpoznávání změn, náklady metody Find mohou být v závislosti na složitosti modelu a množství entit v mezipaměti objektů zvětšeny o jedno pořadí nebo ještě více.

Mějte také na paměti, že funkce Find vrátí pouze hledanou entitu a nenačte automaticky přidružené entity, pokud ještě nejsou v mezipaměti objektů. Pokud potřebujete načíst přidružené entity, můžete použít dotaz podle klíče s Eager načítání. Další informace najdete v článku **8,1 opožděné načítání vs. Eager načítání**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>problémy s výkonem 3.1.2, pokud má mezipaměť objektů mnoho entit

Mezipaměť objektů pomáhá zvýšit celkovou rychlost odezvy Entity Framework. Pokud však mezipaměť objektů má nahrané velké množství entit, může to mít vliv na určité operace, jako je přidání, odebrání, vyhledání, zadání, SaveChanges a další. Konkrétně operace, které aktivují volání DetectChanges, budou mít negativně vliv na velmi velké mezipaměti objektů. DetectChanges synchronizuje graf objektů se správcem stavu objektu a jeho výkon se určí přímo podle velikosti grafu objektů. Další informace o DetectChanges najdete v tématu [sledování změn v entitách POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Při použití Entity Framework 6 můžou vývojáři volat AddRange a RemoveRange přímo na Negenerickými místo na iteraci v kolekci a volat metodu Add jednou na instanci. Výhodou použití metod rozsahu je, že náklady na DetectChanges jsou placeny pouze jednou pro celou sadu entit, a to na rozdíl od jednou pro každou přidanou entitu.

### <a name="32-query-plan-caching"></a>Mezipaměť plánu dotazů 3,2

Při prvním spuštění dotazu projde interním kompilátorem plánu, který převede koncepční dotaz do příkazu Store (například T-SQL, který se spustí při spuštění proti SQL Server).  Pokud je povoleno ukládání plánů dotazů do mezipaměti, při příštím spuštění dotazu se příkaz Store načte přímo z mezipaměti plánu dotazu ke spuštění, přičemž se vynechá kompilátor plánu.

Mezipaměť plánu dotazu je sdílena mezi instancemi ObjectContext v rámci stejné domény AppDomain. Nemusíte podržet instanci ObjectContext, abyste mohli těžit z ukládání do mezipaměti plánu dotazů.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 poznámky k ukládání plánu dotazů do mezipaměti

-   Mezipaměť plánu dotazu je sdílená pro všechny typy dotazů: Entity SQL, LINQ to Entities a CompiledQuery objekty.
-   Ve výchozím nastavení je ukládání do mezipaměti plánu dotazů povoleno pro Entity SQL dotazy, ať už prováděné prostřednictvím EntityCommand nebo prostřednictvím ObjectQuery. Ve výchozím nastavení je povolená taky pro LINQ to Entities dotazy v Entity Framework na platformě .NET 4,5 a v Entity Framework 6.
    -   Ukládání plánu dotazu do mezipaměti lze zakázat nastavením vlastnosti EnablePlanCaching (na EntityCommand nebo ObjectQuery) na false. Příklad:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   U parametrizovaných dotazů se při změně hodnoty parametru stále objeví dotaz uložený v mezipaměti. Ale změna omezujících vlastností parametru (například Size, Precision nebo Scale), bude mít za následek jinou položku v mezipaměti.
-   Při použití Entity SQL je řetězec dotazu součástí klíče. Pokud se dotaz změní na všechny, bude mít za následek jinou položku mezipaměti, i když dotazy budou funkčně ekvivalentní. To zahrnuje změny velikosti písmen nebo prázdných znaků.
-   Při použití LINQ je zpracován dotaz pro vygenerování části klíče. Změna výrazu LINQ proto vygeneruje jiný klíč.
-   Mohou platit další technická omezení; Další podrobnosti najdete v tématu o autokompilovaných dotazech.

#### <a name="322-cache-eviction-algorithm"></a>algoritmus vyřazení mezipaměti 3.2.2

Princip fungování interního algoritmu vám pomůže zjistit, kdy se má povolit nebo zakázat ukládání do mezipaměti plánu dotazů. Čistící algoritmus je následující:

1.  Jakmile mezipaměť obsahuje stanovený počet položek (800), spustíme časovač, který pravidelně (jednou za minutu) vypíše mezipaměť.
2.  Během úklidu mezipaměti se položky z mezipaměti odeberou na LFRU (nejméně často – nedávno používané). Při rozhodování o tom, které položky se mají vysunout, tento algoritmus vezme v úvahu jak počet přístupů, tak i stáří.
3.  Na konci každého úklidu mezipaměti mezipaměť opět obsahuje 800 záznamů.

Při určování, které položky se mají vyřadit, se všechny položky mezipaměti zpracovávají stejně. To znamená, že příkaz Store pro CompiledQuery má stejnou možnost vyřazení jako příkaz Store pro dotaz Entity SQL.

Všimněte si, že časovač vyřazení mezipaměti je v době, kdy jsou v mezipaměti dostupné entity 800, ale mezipaměť je Swept jenom 60 sekund po spuštění tohoto časovače. To znamená, že až 60 sekund může mezipaměť trvat poměrně velkou velikost.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 metriky testů, které demonstrují výkon mezipaměti plánu dotazů

Pro ukázku účinku ukládání do mezipaměti plánu dotazů na výkon vaší aplikace jsme provedli test, ve kterém jsme provedli několik dotazů Entity SQL pro model Navision. Popis modelu Navision a typy dotazů, které se provedly, najdete v příloze. V tomto testu nejdřív projdeme seznam dotazů a jednou se jednou provede jejich přidání do mezipaměti (Pokud je povolené ukládání do mezipaměti). Tento krok je nečasový. V dalším kroku se v hlavním vlákně po dobu více než 60 sekund uloží, aby bylo možné provést čištění mezipaměti. Nakonec projdeme seznam a za druhý čas pro spuštění dotazů uložených v mezipaměti. Kromě toho je mezipaměť plánu SQL Server vyprázdněna před provedením každé sady dotazů, aby časy, které získají přesně, odrážely výhody poskytnuté mezipamětí plánu dotazu.

##### <a name="3231-test-results"></a>3.2.3.1 Výsledky testů

| Test                                                                   | EF5 bez mezipaměti | EF5 v mezipaměti | EF6 bez mezipaměti | EF6 v mezipaměti |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Vytváření výčtu všech dotazů 18723                                          | 124          | 125,4      | 124,3        | 125,3      |
| Zamezení čištění (jenom prvních 800 dotazů, bez ohledu na složitost)  | 41,7         | 5.5        | 40.5         | 5.4        |
| Jenom dotazy AggregatingSubtotals (celkem 178 – zabrání se tak sweep) | 39,5         | 4,5        | 38,1         | 4.6        |

*Všechny časy v sekundách.*

Morální – při provádění spousty různých dotazů (například dynamicky se vytvořily dotazy), ukládání do mezipaměti neusnadní a výsledné vyprázdnění mezipaměti může uchovávat dotazy, které by mohly využít maximum z plánu do ukládání do mezipaměti v důsledku jejich použití.

Dotazy AggregatingSubtotals jsou nejsložitější z dotazů, které jsme testovali pomocí. Jak je očekáváno, složitější dotaz je, tím větší je přínos, který se zobrazí při ukládání plánu dotazů do mezipaměti.

Vzhledem k tomu, že CompiledQuery je ve skutečnosti dotaz LINQ s plánem uloženým do mezipaměti, porovnání CompiledQuery oproti ekvivalentnímu dotazu Entity SQL by mělo mít podobné výsledky. Ve skutečnosti platí, že pokud má aplikace spoustu dynamických Entity SQL dotazů, vyplňování mezipaměti v dotazech také efektivně způsobí, že CompiledQueries "dekompilovat", když jsou vyprázdněny z mezipaměti. V tomto scénáři můžete zlepšit výkon tím, že zakážete ukládání do mezipaměti v dynamických dotazech, abyste naCompiledQueriesi prioritu. Ještě lepší je, že by bylo nutné aplikaci přepsat tak, aby místo dynamických dotazů používala parametrizované dotazy.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3,3 použití CompiledQuery pro zlepšení výkonu pomocí dotazů LINQ

Naše testy označují, že použití CompiledQuery může přinést více než 7% u autokompilovaných dotazů LINQ; To znamená, že strávíte o 7% méně času spouštěním kódu z Entity Framework stacku; neznamená to, že vaše aplikace bude o 7% rychlejší. Obecně řečeno, náklady na psaní a udržování CompiledQuerych objektů v EF 5,0 nemusí být v porovnání s výhodami v hodnotě problému. Vaše kilometry se mohou lišit. tuto možnost můžete využít, pokud váš projekt vyžaduje nadbytečné vložení. Všimněte si, že CompiledQueries jsou kompatibilní pouze s modely odvozenými pomocí ObjectContext a nejsou kompatibilní s modely odvozenými DbContext.

Další informace o vytvoření a vyvolání CompiledQuery naleznete v tématu [kompilované dotazy (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Existují dva předpoklady, které je třeba provést při použití CompiledQuery, konkrétně požadavek na použití statických instancí a problémů, které mají s možností vytváření. Zde jsou podrobnější vysvětlení těchto dvou důležitých informací.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 použití statických instancí CompiledQuery

Vzhledem k tomu, že kompilování dotazu LINQ je časově náročný proces, nechci to udělat pokaždé, když potřebujeme načíst data z databáze. Instance CompiledQuery vám umožňují kompilovat jednou a běžet několikrát, ale musíte být opatrní a při každém pokusu znovu znovu použít stejnou instanci CompiledQuery, aniž byste ji museli kompilovat znovu a znovu používat. Použití statických členů k uložení instancí CompiledQuery bude nezbytné. jinak se vám neprojeví žádné výhody.

Předpokládejme například, že vaše stránka obsahuje následující tělo metody pro zpracování zobrazení produktů pro vybranou kategorii:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

V tomto případě vytvoříte novou instanci CompiledQuery průběžně při každém volání metody. Místo toho, aby se při načtení příkazu Store z mezipaměti plánu dotazů zobrazily výhody výkonu, CompiledQuery projde kompilátorem plánu pokaždé, když se vytvoří nová instance. Ve skutečnosti budete při každém volání metody znečištěni mezipaměť plánu dotazů novou položkou CompiledQuery.

Místo toho chcete vytvořit statickou instanci zkompilovaného dotazu, takže vyvoláte stejný kompilovaný dotaz pokaždé, když je metoda volána. Jedním z těchto způsobů je přidat instanci CompiledQuery jako člena kontextu objektu.  Pak můžete udělat něco trochu čistšího, když k CompiledQuery přistupujete prostřednictvím pomocné metody:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Tato pomocná metoda by se vyvolala takto:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>3.3.2 vytváření přes CompiledQuery

Možnost vytvářet v jakémkoli dotazu LINQ je mimořádně užitečná; k tomu stačí vyvolat metodu za rozhraním IQueryable, jako je *Skip ()* nebo *Count ()* . Nicméně v podstatě vrátí nový objekt IQueryable. I když není k dispozici nic, co by bylo možné zamezit vytváření přes CompiledQuery, způsobí to, že generování nového objektu IQueryable, který vyžaduje předání prostřednictvím kompilátoru plánu, bude zase.

Některé součásti využívají složené objekty IQueryable k povolení pokročilých funkcí. Například ASP. Objekt GridView v síti může být svázán s daty objektu IQueryable prostřednictvím vlastnosti SelectMethod. Prvek GridView potom vytvoří tento objekt IQueryable a umožní řazení a stránkování nad datovým modelem. Jak vidíte, použití CompiledQuery pro prvek GridView by nemuselo mít kompilovaný dotaz, ale vygenerovalo nový automaticky kompilovaný dotaz.

Jedním z míst, kde můžete běžet, je přidání progresivních filtrů do dotazu. Předpokládejme například, že byste měli stránku Customers (zákazníci) s několika rozevíracími seznamy pro volitelné filtry (například země a OrdersCount). Tyto filtry můžete vytvořit přes výsledky CompiledQuery IQueryable, ale pokud to uděláte, vytvoří se nový dotaz, který projde kompilátorem plánu pokaždé, když ho spustíte.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Chcete-li se této opakované kompilaci vyhnout, můžete přepsat CompiledQuery a vzít v úvahu možné filtry:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Který by se vyvolal v uživatelském rozhraní jako:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Kompromisy tady tvoří vygenerované úložiště, které bude mít vždycky filtry s kontrolními hodnotami null, ale u databázového serveru by se měly poměrně snadno optimalizovat:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>mezipaměť 3,4 metadat

Entity Framework také podporuje ukládání metadat do mezipaměti. To je v podstatě ukládání informací o typech a mapování typu na databázi mezi různými připojeními ke stejnému modelu. Mezipaměť metadat je jedinečná pro každou doménu AppDomain.

#### <a name="341-metadata-caching-algorithm"></a>algoritmus mezipaměti 3.4.1 metadata

1.  Informace metadat pro model jsou uloženy v ItemCollection pro každou EntityConnection.
    -   Jako Poznámka na straně, existují různé objekty ItemCollection pro různé části modelu. Například StoreItemCollections obsahuje informace o databázovém modelu. ObjectItemCollection obsahuje informace o datovém modelu; EdmItemCollection obsahuje informace o koncepčním modelu.

2.  Pokud dvě připojení používají stejný připojovací řetězec, budou sdílet stejnou instanci ItemCollection.
3.  Funkčně ekvivalentní, ale v textových i různých připojovacích řetězcích může dojít k různým mezipamětem metadat. Tokenizovat připojovací řetězce, takže jednoduše změnou pořadí tokenů by měla být sdílená metadata. Dva připojovací řetězce, které se zdají být funkčně stejné, ale nemusí být vyhodnoceny jako identické po tokenizace.
4.  ItemCollection se pravidelně kontroluje pro použití. Pokud se zjistí, že se pracovní prostor v poslední době nepoužil, bude pro vyčištění vyhodnocený jako při příštím čištění mezipaměti.
5.  Pouze vytvořením EntityConnection dojde k vytvoření mezipaměti metadat (i když kolekce položek v ní nebudou inicializovány, dokud nebude připojení otevřeno). Tento pracovní prostor zůstane v paměti, dokud algoritmus ukládání do mezipaměti nezjistí, že se nepoužívá.

Poradní tým pro zákazníky napsal Blogový příspěvek, který popisuje, že drží odkaz na ItemCollection, aby se předešlo "vyřazení" při použití velkých modelů: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 vztah mezi mezipamětí metadat a mezipamětí plánu dotazů

Instance mezipaměti plánu dotazu žije v ItemCollection typů úložiště MetadataWorkspace. To znamená, že příkazy úložiště uložené v mezipaměti budou použity pro dotazy proti libovolnému kontextu vytvořenému pomocí daného objektu MetadataWorkspace. Také to znamená, že pokud máte dva řetězce připojení, které se mírně liší a neodpovídají po tokenizací, budete mít různé instance mezipaměti plánu dotazů.

### <a name="35-results-caching"></a>3,5 výsledků do mezipaměti

Při ukládání výsledků do mezipaměti (označované také jako "ukládání do mezipaměti" druhé úrovně ") můžete uchovávat výsledky dotazů v místní mezipaměti. Při vystavování dotazu nejprve vidíte, zda jsou výsledky k dispozici v místním prostředí před dotazem na úložiště. I když mezipaměť výsledků není přímo podporovaná Entity Framework, je možné přidat mezipaměť druhé úrovně pomocí poskytovatele pro vybalení. Příkladem zalamování poskytovatele s mezipamětí druhé úrovně je [Entity Framework Alachisoft mezipaměť druhé úrovně na základě NCache](https://www.alachisoft.com/ncache/entity-framework.html).

Tato implementace ukládání do mezipaměti druhé úrovně je vložená funkce, která probíhá po vyhodnocení výrazu LINQ (a funcletized), a plán spuštění dotazu je vypočítán nebo načten z mezipaměti první úrovně. Mezipaměť druhé úrovně pak uloží pouze nezpracované výsledky databáze, takže kanál materializace se následně provede i poté.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 další odkazy na výsledky ukládání do mezipaměti u poskytovatele zabalení

-   Julie Lerman napsala "ukládání do mezipaměti druhé úrovně v Entity Framework a v článku věnovaném službě Windows Azure" na webu MSDN, který obsahuje informace o tom, jak aktualizovat poskytovatele obálky pro použití ukládání do mezipaměti Windows serveru AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Pokud pracujete s Entity Framework 5, blog týmu obsahuje příspěvek, který popisuje, jak získat věci běžící s poskytovatelem ukládání do mezipaměti pro Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Obsahuje taky šablonu T4, která vám usnadní automatizaci přidání ukládání do mezipaměti na druhé úrovni do vašeho projektu.

## <a name="4-autocompiled-queries"></a>4 autokompilované dotazy

Když je dotaz vydán pro databázi pomocí Entity Framework, musí projít řadou kroků před samotným vyhodnocování výsledků. jedním z těchto kroků je kompilace dotazů. U Entity SQL dotazů bylo známo, že mají dobrý výkon, když jsou automaticky ukládány do mezipaměti, takže druhý nebo třetí postup spuštění stejného dotazu může přeskočit plán kompilátoru a místo toho použít plán uložený v mezipaměti.

Entity Framework 5 zavádí také automatické ukládání do mezipaměti pro dotazy LINQ to Entities. V minulých edicích Entity Framework vytváření CompiledQuery pro urychlení výkonu byl běžný postup, protože by to vedlo k tomu, že vaše LINQ to Entities dotazování bude možné ukládat do mezipaměti. Vzhledem k tomu, že ukládání do mezipaměti je nyní provedeno automaticky bez použití CompiledQuery, zavoláme tuto funkci u autokompilovaných dotazů. Další informace o mezipaměti plánu dotazů a jejím mechanismu najdete v tématu ukládání plánu dotazů do mezipaměti.

Entity Framework detekuje, kdy je nutné znovu zkompilovat dotaz, a to tak, že je dotaz vyvolán, i když byl zkompilován před. Běžné podmínky, které způsobují překompilování dotazu, jsou:

-   Změna MergeOption přidruženého k vašemu dotazu. Dotaz uložený v mezipaměti se nepoužije, místo toho se spustí kompilátor plánování a nově vytvořený plán se uloží do mezipaměti.
-   Změna hodnoty předané možnosti ContextOptions. UseCSharpNullComparisonBehavior. Získáte stejný efekt jako změny MergeOption.

Další podmínky můžou zabránit vašemu dotazu v používání mezipaměti. Obvyklými příklady jsou:

-   Použití&gt;IEnumerable&lt;T Obsahuje&lt;&gt;(hodnota T).
-   Použití funkcí, které vytváří dotazy s konstantami.
-   Použití vlastností nemapovaného objektu.
-   Odkaz na dotaz na jiný dotaz, který vyžaduje překompilování.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4,1 použití rozhraní IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T)

Entity Framework neukládá do mezipaměti dotazy, které vyvolávají rozhraní IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(T hodnota) proti kolekci v paměti, protože hodnoty kolekce se považují za nestálé. Následující příklad dotazu nebude uložen do mezipaměti, takže bude vždy zpracován kompilátorem plánu:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Všimněte si, že velikost rozhraní IEnumerable, proti kterému obsahuje, určuje, jak rychle nebo jak pomalu je dotaz zkompilován. Při použití rozsáhlých kolekcí, jako je třeba ta, která je uvedená v předchozím příkladu, může dojít k výraznému snížení výkonu.

Entity Framework 6 obsahuje optimalizace způsobu, jakým rozhraní IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T) funguje při spuštění dotazů. Generovaný kód SQL je mnohem rychlejší, aby se vytvořil a čitelnější, ve většině případů se také rychleji spouští na serveru.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4,2 použití funkcí, které vytváří dotazy s konstantami

Operátory Skip (), přijmout (), Contains () a DefautIfEmpty () LINQ nevytváří dotazy SQL s parametry, ale místo toho předávají hodnoty jako konstanty. Z tohoto důvodu dotazy, které by jinak mohly být identické, ukončí mezipaměť plánu dotazů v zásobníku EF i databázovém serveru a nevrátí se, pokud se stejné konstanty nepoužijí při následném spuštění dotazu. Příklad:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

V tomto příkladu se pokaždé, když se tento dotaz spustí s jinou hodnotou pro ID, bude dotaz zkompilován do nového plánu.

Konkrétně věnujte pozornost použití akce přeskočit a provést při stránkování. V EF6 tyto metody mají přetížení lambda, které efektivně vede k opakovanému použití plánu dotazů v mezipaměti, protože EF může zachytit proměnné předané těmto metodám a jejich překlad na SQLparameters. To také pomáhá udržet čistič mezipaměti, protože jinak každý dotaz s jinou konstantou pro přeskočit a převzít by získal vlastní položku mezipaměti plánu dotazů.

Vezměte v úvahu následující kód, který je neoptimální, ale je určen pouze k tomu, aby exemplify tuto třídu dotazů:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Rychlejší verze stejného kódu by zahrnovala volání Skip s výrazem lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Druhý fragment kódu může být spuštěn až o 11% rychleji, protože stejný plán dotazu se používá při každém spuštění dotazu, který šetří čas procesoru a zabraňuje znečišťující mezipaměti dotazů. Vzhledem k tomu, že parametr, který se má přeskočit, se nachází v uzávěrce, takže kód může vypadat stejně jako teď:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4,3 použití vlastností nemapovaného objektu

Když dotaz použije vlastnosti nemapovaného typu objektu jako parametr, dotaz nebude uložen do mezipaměti. Příklad:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

V tomto příkladu Předpokládejme, že třída NonMappedType není součástí modelu entity. Tento dotaz lze snadno změnit na nepoužitý Nemapovaný typ a místo toho použít místní proměnnou jako parametr pro dotaz:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

V takovém případě se dotaz bude moci načíst do mezipaměti a bude využívat mezipaměť plánu dotazů.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4,4 propojení s dotazy, které vyžadují opětovné kompilování

Pokud máte druhý dotaz, který se spoléhá na dotaz, který se má znovu zkompilovat, bude v tomto příkladu znovu zkompilován celý druhý dotaz, který se shoduje s výše uvedeným příkladem. Tady je příklad, který ilustruje tento scénář:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

Příklad je obecný, ale ukazuje, jak propojení s firstQuery způsobuje, že secondQuery nebude možné získat do mezipaměti. Pokud firstQuery nebyl dotaz, který vyžaduje opětovné kompilování, pak secondQuery by byl uložen do mezipaměti.

## <a name="5-notracking-queries"></a>5 nesledovaných dotazů

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5,1 zakázání sledování změn pro snížení režie správy stavu

Pokud jste ve scénáři jen pro čtení a chcete se vyhnout režii načítání objektů do objektu ObjectStateManager, můžete vydávat dotazy "bez sledování".  Sledování změn je možné zakázat na úrovni dotazu.

Všimněte si, že když zakážete sledování změn, vypínáte mezipaměť objektů. Při dotazování na entitu nemůžeme vynechávat z objektu ObjectStateManager načtením dříve vyhodnocených výsledků dotazu. Pokud se opakovaně dotazuje na stejné entity v rámci stejného kontextu, může se stát, že se vám v důsledku povolení sledování změn zobrazuje výhoda výkonu.

Při dotazování pomocí rozhraní ObjectContext, ObjectQuery a ObjectSet instance si po nastavení zapamatuje MergeOption a dotazy, které jsou vytvořeny, zdědí efektivní MergeOption nadřazeného dotazu. Při použití DbContext je sledování možné zakázat voláním modifikátoru AsNoTracking () na Negenerickými.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 zakazování sledování změn pro dotaz při použití DbContext

Můžete přepnout režim dotazu na sledování pomocí zřetězení volání metody AsNoTracking () v dotazu. Na rozdíl od ObjectQuery třídy Negenerickými a DbQuery v rozhraní API DbContext nemají pro MergeOption proměnlivou vlastnost.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 zakázání sledování změn na úrovni dotazu pomocí objektu ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 zakázání sledování změn pro celou sadu entit pomocí objektu ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5,2 testovacích metrik, které demonstrují výhody výkonu sledování dotazů

V tomto testu se podíváme na náklady na naplnění objektu ObjectStateManager porovnáním sledování se sesledováním dotazů pro model Navision. Popis modelu Navision a typy dotazů, které se provedly, najdete v příloze. V tomto testu projdeme seznam dotazů a jednou ho spustíme. V případě, že se nesleduje dotazy a jednou s výchozí možností sloučení "AppendOnly", jsme spustili dvě variace testu. Každou variaci jsme spustili třikrát a vybereme střední hodnotu spuštění. Mezi testy vymažeme mezipaměť dotazů na SQL Server a zmenšete databázi tempdb spuštěním následujících příkazů:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Výsledky testů, medián přes 3 běhy:

|                        | BEZ SLEDOVÁNÍ – PRACOVNÍ SADA | BEZ SLEDOVÁNÍ – ČAS | JENOM PŘIPOJIT – PRACOVNÍ SADA | POUZE PŘIPOJENÍ – ČAS |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5 bude mít menší nároky na paměť na konci běhu než Entity Framework 6. Další paměť spotřebovaná pomocí Entity Framework 6 je výsledkem dalších struktur paměti a kódu, který umožňuje nové funkce a lepší výkon.

K dispozici je také jasný rozdíl v paměti při použití objektu ObjectStateManager. Entity Framework 5 zvýšila své nároky o 30% při udržení přehledu o všech entitách, které jsme z databáze vyhodnoceni. Entity Framework 6 zvýšila své nároky o 28% při jejich provádění.

V průběhu času Entity Framework 6 Entity Framework 5 v tomto testu velkým okrajem. Entity Framework 6 dokončil test přibližně o 16% času spotřebovaného Entity Framework 5. Kromě toho Entity Framework 5 při použití objektu ObjectStateManager trvat déle než 9% času. V porovnání Entity Framework 6 při použití objektu ObjectStateManager používá 3% více času.

## <a name="6-query-execution-options"></a>6 možností provádění dotazů

Entity Framework nabízí několik různých způsobů dotazování. Podíváme se na následující možnosti, porovnejte jednotlivé odborníky a nevýhody a prověřte jejich charakteristiky výkonu:

-   LINQ to Entities.
-   Žádná LINQ to Entities sledování.
-   Entity SQL přes ObjectQuery.
-   Entity SQL přes EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6,1 LINQ to Entities dotazů

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Výhody**

-   Vhodné pro CUD operace.
-   Plně vyhodnocené objekty.
-   Nejjednodušší pro zápis pomocí syntaxe integrované do programovacího jazyka.
-   Dobrý výkon.

**Nevýhody**

-   Některá technická omezení, jako například:
    -   Vzorce používající DefaultIfEmpty pro dotazy VNĚJŠÍho spojení mají za následek složitější dotazy než jednoduché příkazy VNĚJŠÍho spojení v Entity SQL.
    -   Stále nemůžete používat jako u obecného porovnávání vzorů.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6,2 žádné LINQ to Entities dotazy sledování

Když je kontext odvozený od objektu ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Když je kontext odvozený z DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Výhody**

-   Vylepšený výkon v pravidelných dotazech LINQ.
-   Plně vyhodnocené objekty.
-   Nejjednodušší pro zápis pomocí syntaxe integrované do programovacího jazyka.

**Nevýhody**

-   Není vhodné pro operace CUD.
-   Některá technická omezení, jako například:
    -   Vzorce používající DefaultIfEmpty pro dotazy VNĚJŠÍho spojení mají za následek složitější dotazy než jednoduché příkazy VNĚJŠÍho spojení v Entity SQL.
    -   Stále nemůžete používat jako u obecného porovnávání vzorů.

Všimněte si, že dotazy, které jsou skalární vlastnosti projektu, nejsou sledovány ani v případě, že není zadáno žádné sledování. Příklad:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Tento konkrétní dotaz explicitně neurčuje, že není sledován, ale vzhledem k tomu, že není vyhodnocování typ známý pro správce stavu objektu, materializovaná výsledek není sledován.

### <a name="63-entity-sql-over-an-objectquery"></a>6,3 Entity SQL ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Výhody**

-   Vhodné pro CUD operace.
-   Plně vyhodnocené objekty.
-   Podporuje ukládání plánu dotazů do mezipaměti.

**Nevýhody**

-   Zahrnuje textové řetězce dotazů, které jsou lépe náchylné k chybě uživatele, než je konstrukce dotazu integrována do jazyka.

### <a name="64-entity-sql-over-an-entity-command"></a>6,4 Entity SQL nad příkazem entity

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Výhody**

-   Podporuje ukládání plánů dotazů do mezipaměti v rozhraní .NET 4,0 (všechny ostatní typy dotazů v rozhraní .NET 4,5 podporují ukládání plánů do mezipaměti).

**Nevýhody**

-   Zahrnuje textové řetězce dotazů, které jsou lépe náchylné k chybě uživatele, než je konstrukce dotazu integrována do jazyka.
-   Není vhodné pro operace CUD.
-   Výsledky nejsou automaticky vyhodnoceny a musí být načteny z čtecího modulu dat.

### <a name="65-sqlquery-and-executestorequery"></a>6,5 SqlQuery a ExecuteStoreQuery

SqlQuery v databázi:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery na Negenerickými:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Výhody**

-   Všeobecně nejrychlejší výkon, protože kompilátor plánu je obejít.
-   Plně vyhodnocené objekty.
-   Vhodný pro CUD operace při použití z Negenerickými.

**Nevýhody**

-   Dotaz je text a náchylný k chybám.
-   Dotaz je vázaný na konkrétní back-end pomocí sémantiky úložiště namísto koncepční sémantiky.
-   Pokud je k dispozici dědičnost, dotaz handcrafted musí mít pro podmínky mapování pro požadovaný typ účet.

### <a name="66-compiledquery"></a>6,6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Výhody**

-   Poskytuje až 7% zlepšení výkonu oproti pravidelným dotazům LINQ.
-   Plně vyhodnocené objekty.
-   Vhodné pro CUD operace.

**Nevýhody**

-   Zvýšení složitosti a režie programování.
-   Zvýšení výkonu se při vytváření nad kompilovaným dotazem ztratí.
-   Některé dotazy LINQ nelze zapsat jako CompiledQuery – například projekce anonymních typů.

### <a name="67-performance-comparison-of-different-query-options"></a>6,7 porovnání výkonu různých možností dotazu

Jednoduché mikrosrovnávací testy, u kterých nebyl při vytváření kontextu kladen test. V kontrolovaném prostředí jsme naměřeni dotazování na 5000 časů pro sadu entit, které nejsou v mezipaměti. Tato čísla se budou považovat za upozornění: nereflektují se na skutečná čísla vytvořená aplikací, ale místo toho jsou velmi přesné měření toho, jak velká část rozdílu při dotazování je porovnávána. jablka na jablka s výjimkou nákladů na vytvoření nového kontextu.

| EF  | Test                                 | Čas (MS) | Memory (Paměť)   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Dotaz LINQ pro ObjectContext             | 2692      | 38277120 |
| EF5 | DbContext dotaz LINQ bez sledování     | 2818      | 41840640 |
| EF5 | Dotaz LINQ DbContext                 | 2930      | 41771008 |
| EF5 | Nesledovaný dotaz LINQ pro ObjectContext | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Dotaz LINQ pro ObjectContext             | 3074      | 45248512 |
| EF6 | DbContext dotaz LINQ bez sledování     | 3125      | 47575040 |
| EF6 | Dotaz LINQ DbContext                 | 3420      | 47652864 |
| EF6 | Nesledovaný dotaz LINQ pro ObjectContext | 3593      | 45260800 |

![EF5 Micro srovnávací testy, 5000 teplé iterace](~/ef6/media/ef5micro5000warm.png)

![EF6 Micro srovnávací testy, 5000 teplé iterace](~/ef6/media/ef6micro5000warm.png)

Mikrosrovnávací testy jsou velmi citlivé na malé změny v kódu. V tomto případě je rozdíl mezi náklady na Entity Framework 5 a Entity Framework 6 způsoben přidáním vylepšení [zachycení](~/ef6/fundamentals/logging-and-interception.md) a [transakcí](~/ef6/saving/transactions.md). Tato mikrosrovnávacích číslech však představují doplněnou vizi do velmi malé fragmenty toho, co Entity Framework. V reálných scénářích teplé dotazů by se při upgradu z Entity Framework 5 na Entity Framework 6 neměla zobrazovat regrese výkonu.

Pro porovnání reálného výkonu různých možností dotazu jsme vytvořili 5 samostatných variant testů, kde používáme jinou možnost dotazování pro výběr všech produktů, jejichž název kategorie je "nápoje". Každá iterace zahrnuje náklady na vytvoření kontextu a náklady na vyhodnocováníy všech vrácených entit. než se vybere součet 1000 časovanéch iterací, neuplynulý čas spuštění 10 iterací. Zobrazené výsledky jsou medián pořízený z 5 spuštění každého testu. Další informace naleznete v příloze B, která obsahuje kód pro test.

| EF  | Test                                        | Čas (MS) | Memory (Paměť)   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext – příkaz entity                | 621       | 39350272 |
| EF5 | DbContext dotaz SQL v databázi             | 825       | 37519360 |
| EF5 | Dotaz na úložiště ObjectContext                   | 878       | 39460864 |
| EF5 | Nesledovaný dotaz LINQ pro ObjectContext        | 969       | 38293504 |
| EF5 | Entita objektu ObjectContext SQL s použitím dotazu na objekt | 1089      | 38981632 |
| EF5 | Kompilovaný dotaz ObjectContext                | 1099      | 38682624 |
| EF5 | Dotaz LINQ pro ObjectContext                    | 1152      | 38178816 |
| EF5 | DbContext dotaz LINQ bez sledování            | 1208      | 41803776 |
| EF5 | DbContext dotaz SQL na Negenerickými                | 1414      | 37982208 |
| EF5 | Dotaz LINQ DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext – příkaz entity                | 480       | 47247360 |
| EF6 | Dotaz na úložiště ObjectContext                   | 493       | 46739456 |
| EF6 | DbContext dotaz SQL v databázi             | 614       | 41607168 |
| EF6 | Nesledovaný dotaz LINQ pro ObjectContext        | 684       | 46333952 |
| EF6 | Entita objektu ObjectContext SQL s použitím dotazu na objekt | 767       | 48865280 |
| EF6 | Kompilovaný dotaz ObjectContext                | 788       | 48467968 |
| EF6 | DbContext dotaz LINQ bez sledování            | 878       | 47554560 |
| EF6 | Dotaz LINQ pro ObjectContext                    | 953       | 47632384 |
| EF6 | DbContext dotaz SQL na Negenerickými                | 1023      | 41992192 |
| EF6 | Dotaz LINQ DbContext                        | 1290      | 47529984 |


![EF5 zahřívání dotaz 1000 – iterace](~/ef6/media/ef5warmquery1000.png)

![EF6 zahřívání dotaz 1000 – iterace](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> V případě úplnosti jsme zahrnuli variaci, kde spustíme Entity SQL dotaz na EntityCommand. Nicméně vzhledem k tomu, že výsledky nejsou pro takové dotazy materializované, porovnání není nutně v případě jablek. Test zahrnuje přibližnou aproximaci pro vyhodnocování, aby se pokus o porovnání povedl.

V tomto koncovém případě Entity Framework 6 Entity Framework 5 z důvodu zvýšení výkonu provedených na několika částech zásobníku, včetně mnohem světlejší inicializace DbContext a rychlejšího&gt; hledání metadat&lt;T.

## <a name="7-design-time-performance-considerations"></a>7 – požadavky na výkon při návrhu

### <a name="71-inheritance-strategies"></a>7,1 strategie dědičnosti

Dalším aspektem výkonu při použití Entity Framework je strategie dědičnosti, kterou používáte. Entity Framework podporuje 3 základní typy dědičnosti a jejich kombinace:

-   Tabulka na hierarchii (TPH) – kde každá sada dědičnosti mapuje na tabulku se sloupcem diskriminátoru, který označuje, který konkrétní typ v hierarchii je reprezentován na řádku.
-   Tabulka na typ (TPT) – kde každý typ má svou vlastní tabulku v databázi; podřízené tabulky definují pouze sloupce, které nadřazená tabulka neobsahuje.
-   Tabulka na třídu (TPC) – kde každý typ má svou vlastní úplnou tabulku v databázi; podřízené tabulky definují všechna jejich pole, včetně těch definovaných v nadřazených typech.

Pokud váš model používá dědění TPT, generované dotazy budou složitější než ty, které jsou vygenerovány jinými strategiemi dědičnosti, což může vést k delší době spuštění v úložišti.  Generování dotazů přes TPT model a vyhodnotit výsledných objektů bude obecně trvat déle.

V příspěvku na blogu MSDN Entity Framework příspěvek na blogu MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>se podívejte na téma požadavky na výkon při použití TPT (tabulka podle typu).

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 zamezení TPT v aplikacích Model First nebo Code First

Když vytvoříte model přes existující databázi, která má schéma TPT, nemáte mnoho možností. Ale při vytváření aplikace pomocí Model First nebo Code First byste se měli vyhnout dědičnosti TPT pro problémy s výkonem.

Při použití Model First v průvodci Entity Designer získáte TPT pro jakoukoliv dědičnost v modelu. Pokud chcete pomocí Model First přepnout na strategii dědičnosti TPH, můžete použít sadu Power Pack "Entity Designer generování databáze" dostupnou v galerii sady Visual Studio (\<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Při použití Code First ke konfiguraci mapování modelu s děděním bude EF standardně používat TPH, takže všechny entity v hierarchii dědičnosti budou namapovány na stejnou tabulku. Další podrobnosti najdete v části "mapování pomocí rozhraní Fluent API" v článku "Code First v Entity Framework 4.1" na webu MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)).

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7,2 upgrade z EF4 na zlepšení času generování modelu

Vylepšení algoritmu, který generuje vrstvu úložiště (SSDL) modelu, je k dispozici v Entity Framework 5 a 6 a jako aktualizace Entity Framework 4 při instalaci sady Visual Studio 2010 SP1. SQL Server Následující výsledky testu ukazují vylepšení při generování velmi velkého modelu, v tomto případě v modelu Navision. Další podrobnosti najdete v příloze C.

Model obsahuje sady entit 1005 a sady přidružení 4227.

| Konfigurace                              | Rozpis spotřebovaného času                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Generace SSDL: 2 hr 27 min. <br/> Generování mapování: 1 sekunda <br/> Generování CSDL: 1 sekunda <br/> Generování ObjectLayer: 1 sekunda <br/> Generování zobrazení: 2 h 14 min |
| Visual Studio 2010 SP1, Entity Framework 4 | Generace SSDL: 1 sekunda <br/> Generování mapování: 1 sekunda <br/> Generování CSDL: 1 sekunda <br/> Generování ObjectLayer: 1 sekunda <br/> Generování zobrazení: 1 hr 53 min.   |
| Visual Studio 2013 Entity Framework 5     | Generace SSDL: 1 sekunda <br/> Generování mapování: 1 sekunda <br/> Generování CSDL: 1 sekunda <br/> Generování ObjectLayer: 1 sekunda <br/> Generování zobrazení: 65 minut    |
| Visual Studio 2013, Entity Framework 6     | Generace SSDL: 1 sekunda <br/> Generování mapování: 1 sekunda <br/> Generování CSDL: 1 sekunda <br/> Generování ObjectLayer: 1 sekunda <br/> Generování zobrazení: 28 sekund   |


Je potřeba poznamenat, že při generování SSDL se zatížení téměř zcela stráví na SQL Server, zatímco vývojový počítač klienta čeká na nečinnost, než se výsledky vrátí ze serveru. Specializující by mělo toto vylepšení obzvlášť. Také je potřeba poznamenat, že v zásadě zobrazení generace probíhá celá cena za generování modelu.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7,3 rozdělení velkých modelů pomocí Database First a Model First

Když se zvětší velikost modelu, plocha návrháře bude nenáročná a bude obtížné ji používat. Model s více než 300 entitami obvykle považujeme za příliš velký, aby bylo možné efektivně používat návrháře. Následující Blogový příspěvek popisuje několik možností pro rozdělení velkých modelů: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Příspěvek byl napsaný pro první verzi Entity Framework, ale postup se pořád týká.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7,4 požadavky na výkon pomocí ovládacího prvku zdroje dat entity

Zjistili jsme případy s vícevláknovými testy výkonu a zátěže, kde výkon webové aplikace s použitím ovládacího prvku EntityDataSource výrazně zhoršuje. Základní příčinou je, že objekt EntityDataSource opakovaně volá MetadataWorkspace. LoadFromAssembly na sestavení, na která webová aplikace odkazuje, aby zjistila typy, které se mají použít jako entity.

Řešením je nastavit ContextTypeName objektu EntityDataSource na název typu odvozené třídy ObjectContext. Tím se vypne mechanismus, který kontroluje všechna odkazovaná sestavení pro typy entit.

Nastavení pole ContextTypeName také zabraňuje funkčnímu problému, kde EntityDataSource v rozhraní .NET 4,0 vyvolá výjimku ReflectionTypeLoadException, pokud nemůže načíst typ ze sestavení prostřednictvím reflexe. Tento problém byl vyřešen v rozhraní .NET 4,5.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7,5 entit POCO a proxy serverů pro sledování změn

Entity Framework umožňuje používat vlastní datové třídy spolu s datovým modelem, aniž by bylo nutné provádět jakékoli úpravy samotných datových tříd. To znamená, že s datovým modelem můžete použít "objekty CLR" ve starém Old "(POCO), jako například existující objekty domény. Tyto POCO datové třídy (označované také jako trvalá ignorování objektů), které jsou mapovány na entity, které jsou definovány v datovém modelu, podporují většinu stejného chování dotazu, vložení, aktualizace a odstranění jako typy entit, které jsou generovány nástroji model EDM (Entity Data Model).

Entity Framework může také vytvořit proxy třídy odvozené z vašich typů POCO, které se používají, pokud chcete povolit funkce, jako je opožděné načítání a automatické sledování změn v entitách POCO. Třídy POCO musí splňovat určité požadavky, aby Entity Framework mohl používat proxy, jak je popsáno zde: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Nepravděpodobné sledování proxy serverů upozorní správce stavu objektu pokaždé, když kterákoli z vlastností vašich entit má změnu hodnoty, takže Entity Framework ví skutečný stav entit po celou dobu. K tomu je potřeba přidat události oznámení do těla metod setter vašich vlastností a nechat správce stavu objektů takové události zpracovat. Všimněte si, že vytvoření entity proxy serveru bude obvykle dražší než vytváření neproxy entity POCO z důvodu přidané sady událostí vytvořených pomocí Entity Framework.

Pokud entita POCO nemá proxy server pro sledování změn, budou nalezeny změny porovnáním obsahu entit s kopií předchozího uloženého stavu. Toto hloubkové porovnání se změní na zdlouhavý proces, pokud máte ve svém kontextu mnoho entit nebo když vaše entity mají velmi velký objem vlastností, a to i v případě, že se žádná z nich od posledního porovnání nezměnila.

Shrnutí: při vytváření proxy serveru pro sledování změn platíte výkon, ale sledování změn vám pomůže zrychlit proces zjišťování změn, pokud mají vaše entity mnoho vlastností nebo pokud máte v modelu mnoho entit. U entit s malým počtem vlastností, kde množství entit neroste příliš mnoho, nemusí mít proxy servery pro sledování změn žádný přínos.

## <a name="8-loading-related-entities"></a>8\. načítání souvisejících entit

### <a name="81-lazy-loading-vs-eager-loading"></a>8,1 opožděné načítání vs. Eager načítání

Entity Framework nabízí několik různých způsobů, jak načíst entity, které souvisejí s cílovou entitou. Například při dotazování na produkty existují různé způsoby, jak budou související objednávky načteny do Správce stavu objektu. Z hlediska výkonu je největší otázkou, kterou je třeba vzít v úvahu při načítání souvisejících entit, použití opožděného načítání nebo Eager načítání.

Při použití Eager načítání jsou související entity načteny spolu s vaší cílovou sadou entit. Pomocí příkazu include v dotazu můžete určit, které související entity chcete přenést.

Při použití opožděného načítání bude počáteční dotaz do cílové sady entit přinese pouze. Kdykoli ale přistupujete k navigační vlastnosti, k načtení související entity se v úložišti vydá jiný dotaz.

Po načtení entity se všechny další dotazy pro entitu načtou přímo z správce stavu objektu bez ohledu na to, jestli používáte opožděné načítání nebo Eager načítání.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8,2 Jak zvolit mezi opožděným načítáním a Eager načítáním

Důležité je, abyste porozuměli rozdílu mezi opožděným načítáním a Eager načítáním, abyste mohli vytvořit správnou volbu pro vaši aplikaci. To vám pomůže vyhodnotit kompromisy mezi několika požadavky na databázi oproti jedné žádosti, která může obsahovat velkou datovou část. Může být vhodné použít Eager načítání v některých částech aplikace a opožděné načítání v jiných částech.

Jako příklad toho, co se děje v digestoři, Předpokládejme, že chcete zadat dotaz na zákazníky, kteří žijí v rámci Spojené království a jejich počet objednávek.

**Použití Eager načítání**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Použití opožděného načítání**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Při použití Eager načítání vydáte jediný dotaz, který vrátí všechny zákazníky a všechny objednávky. Příkaz Store vypadá takto:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Při použití opožděného načítání vydáte na začátku následující dotaz:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

A pokaždé, když přistupujete k vlastnosti navigace objednávky zákazníka, je na obchod vystavený jiný dotaz, jako je následující:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Další informace najdete v tématu [načítání souvisejících objektů](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 opožděné načítání vs. Eager načítání tahák listů

K dispozici není žádná taková věc jako jedna velikost, aby bylo možné vybrat Eager načítání versus opožděné načítání. Zkuste si nejdřív porozumět rozdílům mezi oběma strategiemi, abyste mohli dělat dobře kvalifikované rozhodnutí. Zvažte také, jestli váš kód odpovídá jakémukoli z následujících scénářů:

| Scénář                                                                    | Náš návrh                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Potřebujete získat přístup k mnoha vlastnostem navigace z načtených entit? | **Ne** – obě možnosti budou pravděpodobně provádět. Pokud ale datová část, kterou váš dotaz přináší, není příliš velká, může při použití Eager načítání docházet k výhodám výkonu, protože k vyhodnotití vašich objektů budete potřebovat méně síťových cyklů. <br/> <br/> **Ano** – Pokud potřebujete přístup k mnoha vlastnostem navigace z entit, provedete to tak, že v dotazu pomocí Eager načítání zadáte několik příkazů include. Mezi další entity, které zahrnete, se zobrazí větší část datové části, kterou váš dotaz vrátí. Po zahrnutí tří nebo více entit do dotazu zvažte přepnutí na opožděné načítání. |
| Víte přesně, jaká data budou potřebná v době běhu?                   | **Žádné** – opožděné načítání bude pro vás vhodnější. V opačném případě můžete ukončit dotazování na data, která nebudete potřebovat. <br/> <br/> **Ano** – Eager načítání je pravděpodobně nejlepším tipem; pomůže vám to rychleji načítat celé sady. Pokud Váš dotaz vyžaduje načtení velmi velkého množství dat a to je příliš pomalé, zkuste místo toho použít opožděné načtení.                                                                                                                                                                                                                                                       |
| Je váš kód spuštěný z vaší databáze? (zvýšené latence sítě)  | **Ne** – Pokud není latence sítě problémem, může použití opožděného načítání zjednodušit váš kód. Mějte na paměti, že se topologie vaší aplikace může změnit, takže nemusíte přebírat blízkost databáze pro udělení. <br/> <br/> **Ano** – Pokud se jedná o problém sítě, můžete se rozhodnout, co je pro váš scénář lepší. Obvykle se Eager načítání bude lepší, protože vyžaduje menší počet zpátečních cest.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>8.2.2 problémy s výkonem s několika Zahrnutími

Když uslyšíme otázky ohledně výkonu, které zahrnují problémy s dobou odezvy serveru, je zdrojem problému často dotazy s více příkazy include. I když jsou v dotazu i související entity výkonné, je důležité pochopit, co se děje v rámci pokrývání.

Trvá poměrně dlouhou dobu pro dotaz s více příkazy include, aby mohl projít interním kompilátorem plánu a vytvořit tak příkaz Store. Většina tohoto času stráví pokusem o optimalizaci výsledného dotazu. Příkaz vygenerované úložiště bude obsahovat vnější spojení nebo sjednocení pro každé zahrnutí v závislosti na mapování. Tyto dotazy budou zahrnovat do rozsáhlých propojených grafů z databáze v jediné datové části, což bude acerbate problémy s šířkou pásma, zejména v případě, že je v datové části hodně redundance (například když se k průchodu používá více úrovní zahrnutí). asociace v směru 1: n.

Můžete vyhledat případy, kdy dotazy vrací příliš velkou datovou část tím, že přistupují k podkladovým TSQLům pro dotaz pomocí ToTraceString a spustí se příkaz Store v SQL Server Management Studio pro zobrazení velikosti datové části. V takových případech se můžete pokusit snížit počet příkazů include v dotazu, abyste mohli jednoduše přenést data, která potřebujete. Nebo může být možné přerušit dotaz do menší posloupnosti poddotazů, například:

**Před přerušením dotazu:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Po přerušení dotazu:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

To bude fungovat jenom u sledovaných dotazů, protože využíváme možnosti, které musí kontext provádět rozlišení identity a opravy přidružení automaticky.

Stejně jako u opožděného načítání budou kompromisy více dotazy pro menší datové části. Můžete také použít projekce jednotlivých vlastností a explicitně vybrat pouze data, která potřebujete z každé entity, ale v tomto případě nebudete v tomto případě načítat entity a aktualizace se nepodporují.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 alternativní řešení pro získání opožděného načítání vlastností

Entity Framework aktuálně nepodporuje opožděné načítání skalárních nebo složitých vlastností. V případech, kdy máte tabulku, která obsahuje velký objekt, jako je například objekt BLOB, můžete použít rozdělení tabulky k oddělení velkých vlastností do samostatné entity. Předpokládejme například, že máte tabulku produktů, která obsahuje sloupec fotografie varbinary. Pokud nepotřebujete k této vlastnosti v dotazech často přístup, můžete použít rozdělování tabulky a přenést do nich pouze části entity, které běžně potřebujete. Entita představující fotografii produktu bude načtena pouze v případě, že ji výslovně budete potřebovat.

Dobrým prostředkem, který ukazuje, jak povolit rozdělování tabulky, je rozdělení tabulky Gil Fink na Blogový příspěvek Entity Framework: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 dalších otázek

### <a name="91-server-garbage-collection"></a>Uvolňování paměti serveru 9,1

Někteří uživatelé můžou zaznamenat spory o prostředky, které omezují paralelismuy, které očekává, pokud není systém uvolňování paměti správně nakonfigurovaný. Kdykoli EF použijete ve scénáři s více vlákny nebo v jakékoli aplikaci, která se podobá systému na straně serveru, ujistěte se, že je povoleno shromažďování paměti serveru. To se provádí prostřednictvím jednoduchého nastavení v konfiguračním souboru aplikace:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

To by mělo snížit spor vlákna a zvýšit vaši propustnost o až 30% v případě nasycených scénářů procesoru. Obecně platí, že byste měli vždy testovat, jak se vaše aplikace chová, pomocí klasického uvolňování paměti (což je lépe vyladěno pro scénáře uživatelského rozhraní a na straně klienta) a také pro uvolňování paměti serveru.

### <a name="92-autodetectchanges"></a>9,2 AutoDetectChanges

Jak bylo zmíněno dříve, Entity Framework může zobrazit problémy s výkonem, pokud má mezipaměť objektů mnoho entit. Některé operace, například přidat, odebrat, najít, vstup a SaveChanges, spouštějí volání DetectChanges, které mohou spotřebovat velké množství CPU na základě toho, jak velká je mezipaměť objektů. Důvodem je, že mezipaměť objektů a správce stavu objektu se pokusí zůstat v rámci každé operace provedené v kontextu jako synchronizované, aby byla vytvořená data zaručena správná v rámci nejrůznějších scénářů.

Obecně platí, že pro celou dobu života vaší aplikace je povoleno automatické zjišťování změn Entity Framework. Pokud je váš scénář negativně ovlivněn vysokým využitím procesoru a vaše profily označují, že příčinou je volání DetectChanges, zvažte dočasné vypnutí AutoDetectChanges v citlivé části kódu:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Před vypnutím AutoDetectChanges je dobré pochopit, že to může způsobit, že Entity Framework ztratí schopnost sledovat určité informace o změnách, které se na těchto entitách konají. Pokud je zpracování chybné, může to způsobit nekonzistenci dat ve vaší aplikaci. Pokud chcete získat další informace o vypnutí AutoDetectChanges, přečtěte si \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93-context-per-request"></a>kontext 9,3 na požadavek

Kontexty Entity Framework jsou určeny pro použití jako krátkodobé instance, aby bylo možné zajistit optimální prostředí výkonu. Očekává se, že kontexty by měly být krátké a zahozené a jako takové implementace jsou velmi odlehčené a znovu využívat metadata, kdykoli to bude možné. Ve webových scénářích je důležité mít na paměti, že je to v úmyslu, a nemají kontext po dobu delší než jeden požadavek. Podobně ve scénářích, které nejsou webové, by měl být kontext zahozen na základě vašeho porozumění různých úrovní ukládání do mezipaměti v Entity Framework. Obecně řečeno, jedna by neměla mít instanci kontextu v průběhu životního cyklu aplikace a také kontexty na vlákno a statické kontexty.

### <a name="94-database-null-semantics"></a>9,4 sémantika hodnoty null databáze

Entity Framework ve výchozím nastavení vygeneruje kód SQL, který má v jazyce C\# sémantiku porovnání s hodnotou null. Vezměte v úvahu následující příklad dotazu:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

V tomto příkladu porovnáváme počet proměnných s možnou hodnotou null v entitě, jako je ČísloDodavatele a JednotkováCena. Vygenerovaný SQL pro tento dotaz zobrazí dotaz, zda je hodnota parametru shodná s hodnotou sloupce, nebo pokud jsou parametry i hodnoty sloupce NULL. Tím se skryje způsob, jakým databázový server zpracovává hodnoty null, a poskytne konzistentní prostředí C\# null napříč různými dodavateli databáze. Na druhé straně generovaný kód je trochu konvoluce a nemusí být vhodný, pokud je míra porovnávání v příkazu WHERE dotazu vyšší než na velké číslo.

Jedním ze způsobů, jak řešit tuto situaci, je použití sémantiky s hodnotou null databáze. Všimněte si, že se může potenciálně chovat jinak, než je\# sémantika null, protože teď Entity Framework vygeneruje jednodušší SQL, které zveřejňuje způsob, jakým databázový stroj zpracovává hodnoty null. Sémantika s hodnotou null databáze může být aktivována pro každý kontext s jedním řádkem konfigurace s konfigurací kontextu:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Dotazy s malým až středním velikostí nebudou při použití sémantiky s hodnotou null databáze zobrazovat pozorně, ale rozdíl se projeví v dotazech s velkým počtem potenciálních porovnání s hodnotou null.

V příkladu dotazu výše byl rozdíl výkonu menší než 2% v mikrotestu běžícím v kontrolovaném prostředí.

### <a name="95-async"></a>9,5 Async

Entity Framework 6 zavádí podporu asynchronních operací při spuštění v rozhraní .NET 4,5 nebo novějším. U aplikací, u kterých se v/v v/v nepoužívá spor, bude výhodná použití asynchronních operací dotazů a ukládání. Pokud vaše aplikace neutrpěla kolize vstupu/výstupu, použití Async bude v optimálních případech spouštěno synchronně a vracet výsledek za stejné množství jako synchronní volání, nebo v nejhorším případě jednoduše odložit provádění na asynchronní úlohu a přidat další Tim e pro dokončení vašeho scénáře.

Informace o fungování asynchronního programování, které vám pomůžou při rozhodování o tom, jestli Async zvýší výkon vaší aplikace, najdete [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Další informace o použití asynchronních operací na Entity Framework najdete v tématu [asynchronní dotazování a ukládání](~/ef6/fundamentals/async.md
).

### <a name="96-ngen"></a>9,6 NGEN

Entity Framework 6 není součástí výchozí instalace rozhraní .NET Framework. V takovém případě Entity Framework sestavení nejsou ve výchozím nastavení NGEN, což znamená, že veškerý Entity Framework kód podléhá stejným JIT'ingým nákladům jako jakékoli jiné sestavení MSIL. To může v produkčním prostředí snížit prostředí F5 při vývoji a také studeném spuštění vaší aplikace. Aby se snížily náklady na procesor a paměť JIT'ing, doporučuje se, abyste v případě potřeby naEntity Framework image NGEN. Další informace o tom, jak zlepšit výkon při spuštění Entity Framework 6 s NGEN, najdete v tématu [zlepšení výkonu při spouštění pomocí Ngen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97-code-first-versus-edmx"></a>9,7 Code First oproti EDMX

Entity Framework důvody týkající se problému neshody mezi objektově orientovaným programováním a relačními databázemi pomocí reprezentace v paměti koncepčního modelu (objektů), schématu úložiště (databáze) a mapování mezi Druhá. Tato metadata se nazývají model EDM (Entity Data Model) nebo EDM pro krátké. Z tohoto modelu EDM Entity Framework odvozují zobrazení k převodu dat z objektů v paměti do databáze a zpět.

Když se používá Entity Framework se souborem EDMX, který formálně určuje koncepční model, schéma úložiště a mapování, pak fáze načítání modelu musí ověřit, jestli je EDM správný (například se ujistěte, že žádné mapování chybí), pak Vygenerujte zobrazení a pak ověřte zobrazení a nechte tato metadata připravená k použití. Pouze potom lze spustit dotaz nebo nová data uložit do úložiště dat.

Přístup k Code First je, ve svém srdce, propracovaného generátoru model EDM (Entity Data Model). Entity Framework musí z poskytnutého kódu vydávat EDM; provede tak analýzu tříd zapojených do modelu, použití konvencí a konfigurace modelu prostřednictvím rozhraní Fluent API. Po sestavení modelu EDM se Entity Framework v podstatě chová stejným způsobem, jako by byl v projektu přítomen soubor EDMX. Proto sestavíte model z Code First zvyšuje složitost, která překládá do pomalejšího času spuštění pro Entity Framework ve srovnání s podmnožinou EDMX. Náklady jsou zcela závislé na velikosti a složitosti modelu, který je sestaven.

Když zvolíte použití EDMX a Code First, je důležité znát, že flexibilita zavedená Code First zvyšuje náklady na sestavování modelu poprvé. Pokud vaše aplikace může vydržet náklady na toto první zatížení, obvykle Code First bude upřednostňovaným způsobem, jak jít.

## <a name="10-investigating-performance"></a>10 prošetření výkonu

### <a name="101-using-the-visual-studio-profiler"></a>10,1 použití profileru sady Visual Studio

Pokud máte problémy s výkonem Entity Framework, můžete použít Profiler, jako je ten, který je součástí sady Visual Studio, a zjistit, kde aplikace tráví svůj čas. Tento nástroj jsme použili k vygenerování výsečových grafů v tématu "zkoumání výkonu ADO.NET Entity Framework-Part 1" blogového příspěvku (\<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>), který ukazuje, kde Entity Framework stráví čas během studených a tepléch dotazů.

"Profilování Entity Framework pomocí programu Visual Studio 2010 profiler" na blogu, který je popsán v článku poradní tým pro data a modelování, zobrazuje reálný příklad použití profileru k prozkoumání problému s výkonem.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Tento příspěvek byl napsaný pro aplikaci pro Windows. Pokud potřebujete profilovat webovou aplikaci, nástroje Windows Performance Record (WPR) a Windows Performance Analyzer (WPA) můžou pracovat lépe než při práci ze sady Visual Studio. WPR a WPA jsou součástí sady Windows Performance Toolkit, která je součástí sady Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>Profilace aplikace/databáze 10,2

Nástroje, jako je profiler integrovaný do sady Visual Studio, vás informují o tom, kde vaše aplikace stráví čas.  K dispozici je jiný typ profileru, který provádí dynamickou analýzu běžící aplikace v produkčním prostředí nebo v předprodukčním prostředí v závislosti na potřebách a hledá běžné nástrah a anti-vzory přístupu k databázi.

K dispozici jsou dva komerčně dostupné profilery Entity Framework Profiler (\<http://efprof.com>) a ORMProfiler (\<http://ormprofiler.com>).

Pokud je vaše aplikace MVC aplikací pomocí Code First, můžete použít MiniProfileru StackExchange. Scott Hanselman popisuje tento nástroj ve svém blogu na adrese: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Další informace o profilování aktivity databáze vaší aplikace najdete v článku o katalogu MSDN Julie Lerman [s názvem aktivita databáze profilace v Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>protokolovací nástroj databáze 10,3

Pokud používáte Entity Framework 6, zvažte také použití integrované funkce protokolování. Vlastnost databáze kontextu může být pokyn k protokolování své aktivity prostřednictvím jednoduché konfigurace na jednom řádku:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

V tomto příkladu se databázová aktivita bude protokolovat do konzoly, ale vlastnost log se dá nakonfigurovat tak, aby volala jakýkoli&lt;řetězec&gt; delegáta.

Pokud chcete povolit protokolování databáze bez opětovné kompilace a používáte Entity Framework 6,1 nebo novější, můžete tak učinit přidáním zachytávací do souboru Web. config nebo App. config aplikace.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Další informace o tom, jak přidat protokolování bez opětovné kompilace, najdete v tématu \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>11. Dodatek

### <a name="111-a-test-environment"></a>11,1. testovací prostředí

Toto prostředí používá instalaci dvou počítačů s databází na samostatném počítači z klientské aplikace. Počítače jsou ve stejném stojanu, takže latence sítě je poměrně nízká, ale je realističtější než prostředí s jedním počítačem.

#### <a name="1111-app-server"></a>Server aplikace 11.1.1

##### <a name="11111-software-environment"></a>11.1.1.1 softwarové prostředí

-   Entity Framework 4 softwarové prostředí
    -   Název operačního systému: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate
    -   Visual Studio 2010 SP1 (pouze pro některá porovnání).
-   Entity Framework 5 a 6 softwarového prostředí
    -   Název operačního systému: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate

##### <a name="11112-hardware-environment"></a>11.1.1.2 hardwarové prostředí

-   Duální procesor: Intel (R) Xeon (R) CPU L5520 W3530 @ 2,27 GHz, 2261 Mhz8 GHz, 4 jádra, 84 logických procesorů.
-   2412 GB RamRAM.
-   jednotka 136 GB SCSI250GB SATA 7200 ot./min. povolenou/s se rozdělí do 4 oddílů.

#### <a name="1112-db-server"></a>Server 11.1.2 DB

##### <a name="11121-software-environment"></a>11.1.2.1 softwarové prostředí

-   Název operačního systému: Windows Server 2008 R 28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122-hardware-environment"></a>11.1.2.2 hardwarové prostředí

-   Jeden procesor: Intel (R) Xeon (R) CPU L5520 @ 2,27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 jádra, 8 logických procesorů.
-   824 GB RamRAM.
-   jednotka 465 GB ATA500GB SATA 7200 ot./min. 6 GB/s se rozdělí do 4 oddílů.

### <a name="112-b-query-performance-comparison-tests"></a>11,2 B. testy porovnání výkonu dotazů

Model Northwind byl použit k provedení těchto testů. Byla vygenerována z databáze pomocí návrháře Entity Framework. Pak následující kód byl použit pro porovnání výkonu možností spuštění dotazu:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>11,3 C. model Navision

Databáze Navision je velká databáze, která se používá k ukázce Microsoft Dynamics – NAV. Vygenerovaný koncepční model obsahuje sady entit 1005 a sady přidružení 4227. Model použitý v testu je "plochá" – do něj nebyla přidána žádná dědičnost.

#### <a name="1131-queries-used-for-navision-tests"></a>dotazy 11.3.1 použité pro testy v Navision

Seznam dotazy, který se používá s modelem Navision, obsahuje 3 kategorie dotazů Entity SQL:

##### <a name="11311-lookup"></a>11.3.1.1 vyhledávání

Jednoduchý vyhledávací dotaz bez agregací

-   Počet: 16232
-   Příklad:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

Normální dotaz BI s více agregacemi, ale žádné mezisoučty (jeden dotaz)

-   Počet: 2313
-   Příklad:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Kde MDF\_SessionLogin\_Time\_Max () je definován v modelu jako:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Dotaz BI s agregacemi a mezisoučty (přes sjednocení)

-   Počet: 178
-   Příklad:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
