---
title: Faktory ovlivňující výkon u EF4 EF5 a EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: c87c1412cb23abf232663d7e4f44eef5f7818ea2
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022386"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Faktory ovlivňující výkon u EF 6, 4 a 5
David Obando, Eric Dettinger a další

Publikováno: Duben 2012

Poslední aktualizace: květen 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Úvod

Pohodlný způsob, jak poskytuje abstrakci pro přístup k datům v objektově orientované aplikace jsou objektově-relační mapování rozhraní. Pro aplikace .NET doporučujeme od Microsoftu O/RM je Entity Framework. Pomocí libovolné abstrakce, výkon se může stát žádný problém.

Tento dokument White Paper byla zapsána do zobrazit důležité informace o výkonu, při vývoji aplikací pomocí Entity Frameworku, vývojáři získat představu o Entity Framework vnitřní algoritmy, které mohou ovlivnit výkon a poskytuje tipy pro šetření a zlepšení výkonu ve svých aplikacích, které používají rozhraní Entity Framework. Existuje mnoho témat dobrý výkon již k dispozici na webu a také Snažili jsme směřující k těmto prostředkům, kde je to možné.

Výkon je složité téma. Tento dokument White Paper má jako zdroj pomoci provedete výkonu související s rozhodnutí, která pro vaše aplikace, které používají rozhraní Entity Framework. Uvádíme některé metriky testů k předvedení výkon, ale tyto metriky nejsou určeny jako absolutní indikátory výkonu, které se zobrazí ve vaší aplikaci.

Praktické důvodů tento dokument předpokládá Entity Framework 4 je spuštěn v rámci rozhraní .NET 4.0 a Entity Framework 5 a 6 jsou spuštěny v rozhraní .NET 4.5. Řada vylepšení výkonu pro Entity Framework 5 jsou uložené v základních komponent, které se dodávají s rozhraním .NET 4.5.

Entity Framework 6 je out of band verze a nezávisí na komponenty Entity Framework, které se dodávají s .NET. Entity Framework 6 pracovat na rozhraní .NET 4.0 a 4.5 rozhraní .NET a můžou nabízet velká výhoda všem uživatelům, kteří nejsou z rozhraní .NET 4.0, ale má nejnovější součásti Entity Framework ve svých aplikacích. Když tento dokument uvádí Entity Framework 6, odkazuje na nejnovější verzi k dispozici v době psaní tohoto textu: verze 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Studená vs. Provádění dotazu teplé

Při prvním provedené jakéhokoli dotazu pro daný model Entity Framework nemá spoustu práce na pozadí k načtení a ověření modelu. Často označujeme tento první dotaz jako dotaz "studenými".  Další dotazy vůči už načtený modelu jsou označovány jako "teplé" dotazy a je mnohem rychlejší.

Pojďme trvat souhrnný přehled trvají delší dobu při provádění dotazu používá nástroj Entity Framework a v tématu, kde se věci zlepšení v Entity Framework 6.

**První spuštění dotazu – studenou dotazu**

| Uživatel zapíše kód                                                                                     | Akce                    | EF4 Dopad na výkon                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Dopad na výkon                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Dopad na výkon                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Vytvoření kontextu          | Střední                                                                                                                                                                                                                                                                                                                                                                                                                        | Střední                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Vytvoření výrazu dotazu | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                           | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Provádění dotazů LINQ      | -Načítání metadat: Vysoká, ale v mezipaměti <br/> – Zobrazení generování: potenciálně velmi vysoké, ale v mezipaměti <br/> -Parametr hodnocení: střední <br/> -Dotazování překlad: střední <br/> -Generování materializer: střední ale v mezipaměti <br/> -Databáze provádění dotazů: potenciálně velkému <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializace objektů: střední <br/> -Vyhledávání identity: střední | -Načítání metadat: Vysoká, ale v mezipaměti <br/> – Zobrazení generování: potenciálně velmi vysoké, ale v mezipaměti <br/> -Parametr hodnocení: Nízká <br/> -Dotazování překlad: střední ale v mezipaměti <br/> -Generování materializer: střední ale v mezipaměti <br/> -Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializace objektů: střední <br/> -Vyhledávání identity: střední | -Načítání metadat: Vysoká, ale v mezipaměti <br/> – Zobrazení generování: střední ale v mezipaměti <br/> -Parametr hodnocení: Nízká <br/> -Dotazování překlad: střední ale v mezipaměti <br/> -Generování materializer: střední ale v mezipaměti <br/> -Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializace objektů: střední (rychlejší než EF5) <br/> -Vyhledávání identity: střední |
| `}`                                                                                                  | Connection.Close          | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                           | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Spuštění druhého dotazu – teplé dotazu**

| Uživatel zapíše kód                                                                                     | Akce                    | EF4 Dopad na výkon                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Dopad na výkon                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Dopad na výkon                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Vytvoření kontextu          | Střední                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Střední                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Vytvoření výrazu dotazu | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Provádění dotazů LINQ      | -Metadat ~~načítání~~ vyhledávání: ~~vysoká, ale v mezipaměti~~ nízká <br/> – Zobrazení ~~generování~~ vyhledávání: ~~potenciálně velmi vysoké, ale v mezipaměti~~ nízká <br/> -Parametr hodnocení: střední <br/> -Dotazování ~~překlad~~ vyhledávání: střední <br/> -Materializer ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká <br/> -Databáze provádění dotazů: potenciálně velkému <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializace objektů: střední <br/> -Vyhledávání identity: střední | -Metadat ~~načítání~~ vyhledávání: ~~vysoká, ale v mezipaměti~~ nízká <br/> – Zobrazení ~~generování~~ vyhledávání: ~~potenciálně velmi vysoké, ale v mezipaměti~~ nízká <br/> -Parametr hodnocení: Nízká <br/> -Dotazování ~~překlad~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká <br/> -Materializer ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká <br/> -Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializace objektů: střední <br/> -Vyhledávání identity: střední | -Metadat ~~načítání~~ vyhledávání: ~~vysoká, ale v mezipaměti~~ nízká <br/> – Zobrazení ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká <br/> -Parametr hodnocení: Nízká <br/> -Dotazování ~~překlad~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká <br/> -Materializer ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká <br/> -Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materializace objektů: střední (rychlejší než EF5) <br/> -Vyhledávání identity: střední |
| `}`                                                                                                  | Connection.Close          | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Nízká                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Existuje několik způsobů, jak snížit náklady na výkon dotazů, studené a horké a provedeme najdete v těchto v následující části. Konkrétně zaměříme na snížíte náklady na načítání v studenou dotazy pomocí předem vygenerovaných zobrazení, které by měly pomoci zmírnit výkonu důsledně došlo během generování zobrazení modelu. Za dotazy na horké budeme zabývat ukládání do mezipaměti plánu dotazu, žádné dotazy sledování a možnosti spuštění různých dotazů.

### <a name="21-what-is-view-generation"></a>2.1 co je generování zobrazení?

Chcete-li pochopit, jaké zobrazení generování není, musíte nejprve rozumí tomu, co jsou "Zobrazení mapování". Zobrazení mapování jsou spustitelné vyjádření transformací zadán v mapování pro každou sadu entit a přidružení. Interně tato zobrazení mapování mít tvar CQTs (dotaz kanonické stromy). Existují dva typy zobrazení mapování:

-   Zobrazení dotazu: představují transformace potřeba přejít ze schématu databáze do koncepčního modelu.
-   Aktualizovat zobrazení: představují transformace potřeba přejít na schéma databáze v konceptuálním modelu.

Mějte na paměti, že konceptuálního modelu se mohou lišit od schématu databáze různými způsoby. Například jeden jedné tabulky dají zneužít k ukládání dat pro dva typy různých entit. Dědičnost a nejsou v netriviálních mapování hrají roli složitosti zobrazení mapování.

Proces výpočetních tato zobrazení podle specifikace mapování je, čemu říkáme generování zobrazení. Generování zobrazení můžete buď probíhat dynamicky při načtení modelu, nebo v okamžiku sestavení s použitím "předem vygenerovaných zobrazení"; se serializují ve formuláři Entity SQL příkazy a C\# nebo soubor jazyka Visual Basic.

Při generování zobrazení, se také ověří. Většinu náklady na generování zobrazení z hlediska výkonu je ve skutečnosti ověření zobrazení, která zajistí, že připojení mezi entitami dávat smysl a mít správné kardinalitu pro všechny podporované operace.

Při spuštění dotazu za sadu entit dotaz je v kombinaci s odpovídající zobrazení dotazu a výsledek tohoto sestavení je projít plán kompilátor vytvoří reprezentace dotaz, který umožní pochopit záložního úložiště. Konečný výsledek tohoto kompilace pro SQL Server, bude příkaz T-SQL SELECT. Při prvním provádí aktualizace přes sadu entit, zobrazení aktualizace je spuštěn prostřednictvím podobným jeho transformaci na příkazy DML pro cílovou databázi.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 faktory, které ovlivňují výkon generování zobrazení

Výkon zobrazení generování krok nejen závisí na velikosti vašeho modelu, ale také na způsob propojených je model. Pokud dvě entity, které jsou připojené přes řetěz dědičnosti nebo přidružení, se označují za připojené. Podobně pokud dvě tabulky jsou připojené přes cizího klíče, jsou připojeni. Jak zvýšit počet připojených entit a tabulek v vaše schémat, generování zobrazení náklady narůstají.

Algoritmus, který jsme použili pro generování a ověření zobrazení je exponenciální v nejhorším případě ale některé optimalizace používáme ke zlepšování to. Zdá se, že negativně ovlivnit výkon největší faktory jsou:

-   Model velikost odkazující na počet entit a množství přidružení mezi těmito entitami.
-   Model složitost, konkrétně dědičnosti zahrnujících velký počet typů.
-   Místo nezávislé přidružení cizího klíče asociace.

Pro malé, jednoduchých modelů může být dostatečně malá, aby nebyl nepokoušejte pomocí předem vygenerovaných zobrazení náklady. Model velikosti a složitosti zvýšit, máte několik možností, které jsou k dispozici, abyste snížili náklady na generování zobrazení a ověření.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 použití Pre-Generated zobrazení modelu snížit dobu načítání

Podrobné informace o tom, jak použít předem vygenerovaných zobrazení v Entity Framework 6 [Pre-Generated mapování zobrazení](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 předem vygenerovaných zobrazení pomocí Entity Framework Power Tools Community Edition

Můžete použít [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) ke generování zobrazení modelů EDMX a Code First pravým tlačítkem myši na soubor třídy modelu a pomocí Entity Framework nabídce vyberte "Generování zobrazení". Entity Framework Power Tools Community Edition fungují jenom s kontexty odvozené DbContext.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 jak pomocí předem vygenerovaných zobrazení s modelem vytvořené EDMGen

EDMGen je nástroj, který se dodává s využitím .NET a funguje s Entity Framework 4 a 5, ale ne s Entity Framework 6. EDMGen umožňuje generovat soubor modelu, objektové vrstvě a zobrazení z příkazového řádku. Jedna z výstupů se bude zobrazení souboru v jazyce podle vlastní volby, VB nebo C\#. Toto je soubor kódu obsahující fragmenty Entity SQL pro každou sadu entit. Pokud chcete povolit předem vygenerovaných zobrazení, jednoduše zahrnout soubor ve vašem projektu.

Pokud provádíte ruční úpravy soubory schémat pro model, musíte znovu vygenerovat zobrazení souboru. Můžete to provést spuštěním EDMGen s **/mode:ViewGeneration** příznak.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 jak Pre-Generated zobrazení pomocí souboru EDMX

EDMGen můžete také použít ke generování zobrazení souboru EDMX – dříve odkazovaných MSDN téma popisuje postup přidání události před sestavením k tomu –, ale to je složitá a existují případy, kdy není možné. Je obvykle jednodušší použít šablonu T4 pro generování zobrazení, když je model v souboru edmx.

Blog týmu ADO.NET má příspěvek, který popisuje, jak použít šablonu T4 pro generování zobrazení ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Tento příspěvek obsahuje šablonu, která je možné stáhnout a přidat do projektu. Šablony bylo napsáno pro první verzi Entity Frameworku, takže není zaručena pro práci s nejnovější verzí Entity Framework. Však si můžete stáhnout aktuální sadu zobrazení generování šablon pro Entity Framework 4 a 5from Galerie sady Visual Studio:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Pokud používáte Entity Framework 6 můžete získat zobrazení T4 generování šablon z Galerie sady Visual Studio na \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 snížíte náklady na generování zobrazení

Náklady na generování zobrazení z modelu načítají (čas spuštění) pomocí předem vygenerovaných zobrazení přesune do doby návrhu. Když to zlepšuje výkon při spouštění v době běhu, bude stále přetrvávají bolest generování zobrazení při vývoji. Existuje několik dalších triky, které můžou pomoct snížit náklady na generování zobrazení, jak v době kompilace a spuštění čas.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 pomocí přidružení cizího klíče, abyste snížili náklady na generování zobrazení

Jsme viděli počet případů, kdy přepínání přidružení v modelu z nezávislé přidružení cizího klíče asociace výrazné vylepšení doby trvání generování zobrazení.

Abychom si předvedli toto vylepšení, jsme dvě verze modelu Navision vygenerovaného s využitím EDMGen. *Poznámka: seeappendix Cfor popis Navision modelu.* Navision model je zajímavé pro toto cvičení z důvodu jeho velmi velké množství entit a vztahů mezi nimi.

Jedna verze tohoto modelu velmi velké se vygeneroval s přidružení cizího klíče a druhá se vygeneroval s nezávislé přidružení. Potom skončila, jak dlouho trvalo generování zobrazení pro každý model. Entity Framework5 test použil metodu GenerateViews() ze třídy EntityViewGenerator ke generování zobrazení, i když Entity Framework 6 test použít metodu GenerateViews() ze třídy objekt StorageMappingItemCollection. To z důvodu kód restrukturalizaci, ke které došlo v základu kódu Entity Framework 6.

Pomocí Entity Framework 5, generování zobrazení pro model s cizí klíče trvalo 65 minut na testovacím počítači. Není známo jak dlouho by trvalo generování zobrazení pro model, který používá nezávislé přidružení. Ponechali jsme test spuštěného víc než měsíc předtím, než počítač byl restartován v našem testovacím prostředí instalace měsíčních aktualizací.

Pomocí Entity Framework 6, trvalo generování zobrazení pro model s cizí klíče 28 sekundách ve stejném počítači testovacího prostředí. Generování zobrazení pro model, který používá nezávislé přidružení trvalo 58 sekund. Vylepšení Hotovo a Entity Framework 6 na jeho zobrazení generování kódu znamená, že mnoho projektů, nebude nutné předem vygenerovaných zobrazení zajistit kratší časy spouštění.

Je důležité na příspěvek, který předem generování zobrazení v Entity Framework 4 a 5, můžete provést s EDMGen nebo Entity Framework Power Tools. Pro zobrazení Entity Framework 6 generování to provést prostřednictvím Entity Framework Power Tools nebo programově, jak je popsáno v [zobrazení mapování Pre-Generated](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 jak nahrazujícím nezávislé přidružení cizího klíče

Pokud používáte EDMGen nebo návrháři entit v sadě Visual Studio, dostanete FKs ve výchozím nastavení a trvá jen jednoho zaškrtávacího políčka nebo příkazového řádku příznak přepínat mezi FKs a služby ověřování v Internetu.

Pokud máte velké model Code First pomocí nezávislé přidružení bude mít stejný účinek na generování zobrazení. I když někteří vývojáři bude předpokládat, že to být zahlcení jejich objektového modelu, můžete tomuto důsledku vyhnout zahrnutím vlastnosti cizího klíče u tříd pro vaše závislé objekty. Můžete najít další informace o této skutečnosti do \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Při použití      | Postup                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| V návrháři entit | Po přidání přidružení mezi dvěma entitami, ujistěte se, že máte referenční omezení. Referenční omezení řekněte Entity Framework nahrazujícím nezávislé přidružení cizího klíče. Další podrobnosti najdete \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Při použití EDMGen ke generování souborů z databáze, cizí klíče budou dodržovat i a přidá do modelu jako takové. Další informace o různých možnostech, které jsou vystavené EDMGen [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Kód nejprve      | Najdete v části "Konvence vztah" [první konvence kódu](~/ef6/modeling/code-first/conventions/built-in.md) informace o tom, jak při použití Code First obsahovat vlastnosti cizího klíče na závislé objekty.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 Přesun do samostatných sestavení modelu

Když váš model je přímo součástí vaší aplikace a generovat zobrazení prostřednictvím události před sestavením nebo šablony T4, generování zobrazení a ověření proběhne pokaždé, když se je znovu sestavit projekt, i když je model nebyl změněn. Pokud přesunout do samostatných sestavení modelu a na něj odkazovat z vaší aplikace, lze provádět jiné změny do vaší aplikace bez nutnosti znovu sestavit projekt, který obsahuje model.

*Poznámka:*  při přesunu modelu do samostatných sestavení, nezapomeňte si zkopírovat připojovací řetězce pro model do konfiguračního souboru aplikace projektu klienta.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 zakážete ověřování modelu bázi edmx

Modely EDMX se ověřují v době kompilace, i když je model beze změny. Pokud model je už potvrzená, můžete potlačit ověření v době kompilace tak, že nastavíte vlastnost "Ověřit při sestavení" v okně Vlastnosti na hodnotu false. Při změně mapování nebo modelu, můžete dočasně znovu povolit ověření zkontrolujte provedené změny.

Všimněte si, že byla provedena vylepšení výkonu Entity Framework Designer pro Entity Framework 6 a náklady "ověřit při sestavení" jsou mnohem nižší než v předchozích verzích návrháře.

## <a name="3-caching-in-the-entity-framework"></a>3 ukládání do mezipaměti v Entity Framework

Entity Framework má následující formy integrované ukládání do mezipaměti:

1.  Objekt, ukládání do mezipaměti – objektu ObjectStateManager integrované do ObjectContext instance sleduje v paměti objektů, které byly načteny pomocí této instance. Toto je také označovaný jako první úrovně mezipaměti.
2.  Mezipaměti plánu dotazu – opětovné použití příkaz generované úložiště, pokud je dotaz proveden více než jednou.
3.  Metadata ukládání do mezipaměti – sdílení metadata pro model v různých připojení do stejného modelu.

Kromě mezipamětí, které EF poskytuje hned po spuštění zvláštní druh zprostředkovatele dat ADO.NET, označované jako zprostředkovatele zabalení lze také rozšířit Entity Framework s mezipamětí pro výsledky získané z databáze, označované také jako ukládání do mezipaměti druhé úrovně.

### <a name="31-object-caching"></a>3.1 objektu ukládání do mezipaměti

Ve výchozím nastavení když entity se vrátí ve výsledcích dotazu, těsně před plánovaným začátkem EF bude realizována, objekt ObjectContext zkontroluje Pokud entity se stejným klíčem již byla načtena do jeho objektu ObjectStateManager. Pokud už entity se stejnými klíči EF jej zahrnout do výsledků dotazu. I když EF stále vydá dotaz na databázi, můžete toto chování obcházet velkou část náklady materializaci entita více než jednou.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 získání entity z mezipaměti objekt DbContext hledání

Na rozdíl od pravidelných dotaz metodu najít v DbSet (zahrnutá v EF 4.1 poprvé rozhraní API) provede vyhledávání v paměti před i zadání dotazu na databázi. Je důležité si uvědomit, že dvě různé instance ObjectContext bude mít dva různé instance objektu ObjectStateManager, což znamená, že mají samostatný objekt mezipaměti.

Hledání používá hodnotu primárního klíče pokusí se najít entitu sledován pomocí funkce kontextu. Entitu se nenachází v kontextu pak dotaz provést, který se vyhodnotí na databázi a vrátí se hodnota null, pokud entita nebyl nalezen v kontextu nebo v databázi. Všimněte si, že hledání vrací entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.

Existuje posouzení výkonu, jež mají být provedeny při hledání. Volání této metody ve výchozím nastavení spustí ověření mezipaměti objektů zjišťovat změny, které jsou stále čeká na potvrzení změn do databáze. Tento proces může být velmi náročná, pokud existuje velký počet objektů v mezipaměti objektů nebo velkého objektu grafu se přidávají do mezipaměti objektů, ale můžete také zakázat. V některých případech může přes řád velké rozdíly v volání najít metodu, pokud zakážete automatické zjišťování vnímat změny. Ještě druhý řádově bude považován, když ve skutečnosti je objekt v mezipaměti a když má objekt, který se má načíst z databáze. Tady je ukázka grafu s měření pomocí některé z našich microbenchmarks vyjádřené v milisekundách, s načtením 5000 entit:

![Hodnota na logaritmické stupnici .NET 4.5](~/ef6/media/net45logscale.png ".NET 4.5 – logaritmické měřítko")

Příklad najít změnami auto-detect zakázané:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Co je nutné zvážit při použití metody najít je:

1.  Pokud objekt není v mezipaměti jsou negovat výhody hledání, ale syntaxe je ještě jednodušší než dotaz podle klíče.
2.  Pokud automatické zjištění změn je povoleno náklady na metodu Find může zvýšit jeden řádově, nebo ještě větší v závislosti na složitosti modelu a množství entit ve vaší mezipaměti objektů.

Také Uvědomte si, že najít pouze vrátí entitu, kterou jste hledali a neodstraní automaticky načte jeho přidružených entit Pokud nejsou již v mezipaměti objektů. Pokud potřebujete získat související entity, můžete pomocí klíče se nemůžou dočkat, až načítání dotazu. Další informace najdete v části **8.1 opožděné načtení vs. Předběžné načítání**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 Pokud objekt mezipaměti má mnoho entit problémů s výkonem

Objekt mezipaměti pomáhá zvýšit celkovou rychlost reakce Entity Framework. Pokud objekt mezipaměti má velmi velký objem entity načíst že ji může mít vliv na určité operace, třeba přidat, odebrat, najít položku, SaveChanges a další. Konkrétně se operace, které aktivují volání metoda DetectChanges negativně ovlivní mezipaměti velmi velké objekty. Metoda DetectChanges synchronizuje grafu objektu s objekt state manager a jeho výkonu se určuje podle velikosti objektu grafu přímo. Další informace o metoda DetectChanges najdete v tématu [sledování změn v entity objektů POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Pokud používáte Entity Framework 6, jsou vývojáři schopni volání AddRange a RemoveRange přímo na DbSet, namísto iterace v kolekci a přidat volání jednou za instanci. Výhodou použití rozsahu metody je, že náklady na metoda DetectChanges pouze platí jednou pro celou sadu entit, na rozdíl od jednou za každé přidání entity.

### <a name="32-query-plan-caching"></a>3.2 dotazu do mezipaměti plánu

Prvním je dotaz proveden, prochází přes vnitřní plán kompilátor převede koncepční dotaz na příkaz úložiště (třeba T-SQL, který se spustí při spuštění SQL serveru).  Pokud je povoleno ukládání do mezipaměti plánu dotazu, při příštím dotaz je proveden úložišti je příkaz načíst přímo z mezipaměti plánu dotazu pro provádění bez použití kompilátoru plánu.

Mezipaměti plánu dotazu je sdílen mezi instance ObjectContext v rámci téže třídy AppDomain. Není nutné pro udržení instance ObjectContext, abyste využili výhod ukládání do mezipaměti plánu dotazu.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 několik poznámek o dotazu plánování ukládání do mezipaměti

-   Mezipaměti plánu dotazu jsou sdílena pro všechny typy dotazů: Entity SQL, technologii LINQ to Entities a CompiledQuery objekty.
-   Ve výchozím nastavení ukládání do mezipaměti plánu dotazu je povoleno pro Entity SQL dotazy, zda provést prostřednictvím EntityCommand nebo ObjectQuery. Je také ve výchozím nastavení zapnutá pro LINQ dotazy entity v Entity Framework v rozhraní .NET 4.5 a Entity Framework 6
    -   Ukládání do mezipaměti plánu dotazu je možné zakázat nastavením vlastnosti EnablePlanCaching (na EntityCommand nebo ObjectQuery) na hodnotu false. Příklad:
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
-   Pro parametrizované dotazy se změna hodnoty parametru stále dostanou dotazu z mezipaměti. Ale změna omezujících vlastností parametrů (například velikost, přesnost nebo škálování) se dostanou jinou položku v mezipaměti.
-   Pokud používáte Entity SQL, řetězec dotazu je součástí klíče. Změna dotazu vůbec způsobí jiný mezipaměť, i v případě, dotazy jsou funkčně ekvivalentní. To zahrnuje změny v malých a velkých písmen nebo prázdný znak.
-   Při použití LINQ dotaz zpracování k vygenerování součástí klíče. Nahradit výraz LINQ proto vygeneruje za jiný klíč.
-   Další technická omezení můžou vztahovat; Další podrobnosti najdete v Autocompiled dotazy.

#### <a name="322-cache-eviction-algorithm"></a>3.2.2 mezipaměti vyřazení algoritmus

Ke zjištění, jak funguje interním algoritmu a vám pomůžou přijít na to, když chcete povolit nebo zakázat dotazu ukládání do mezipaměti plánu. Algoritmus vyčištění vypadá takto:

1.  Jakmile mezipaměti obsahuje stanovený počet položek (800), začneme časovač, pravidelně (jednou za minutu) přesune do mezipaměti.
2.  Během změny mezipaměti položky jsou odebrány z mezipaměti LFRU (nejméně často – nedávno použité) základ. Tento algoritmus zohledňuje počet průchodů a stáří při rozhodování o tom, které položky jsou vysunout.
3.  Na konci každé vyčištění mezipaměti se mezipaměť znovu obsahuje 800 položky.

Všechny položky mezipaměti se zachází stejně při určení položek k vyřazení. To znamená, že příkaz úložiště pro CompiledQuery má stejnou šanci vyřazení jako příkaz úložiště dotazu Entity SQL.

Všimněte si, že se spustí se časovač vyřazení mezipaměti v, když nejsou 800 entity v mezipaměti, ale mezipaměti je pouze, jež jsou 60 sekund, po spuštění tohoto časovače. To znamená, že až na 60 sekund mezipaměti může zvětšit poměrně velká.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 testování metriky demonstrace plán dotazu, ukládání do mezipaměti výkonu

Abychom si předvedli efekt plán dotazu do mezipaměti na výkon vaší aplikace, jsme provedli test kde jsme spouštěli počet dotazů Entity SQL proti Navision modelu. Viz dodatek popis modelu Navision a typy dotazů, které byly spuštěny. V tomto testu jsme první iteraci v rámci seznamu dotazů a po spuštění každé z nich přidat do mezipaměti (Pokud je povoleno ukládání do mezipaměti). Tento krok je untimed. V dalším kroku můžeme přejít do režimu spánku hlavního vlákna pro více než 60 sekund mezipaměti cílit na konkrétní uskutečnit; Nakonec jsme iterovat přes seznam 2. čas ke spuštění dotazy uložené v mezipaměti. Kromě toho má SQL Server mezipaměti plánu vyprázdní před provedením každého sadu dotazů tak, aby kolikrát získáme přesně odrážet výhodu Dal mezipaměti plánu dotazu.

##### <a name="3231-test-results"></a>3.2.3.1 výsledky testů

| Test                                                                   | EF5 žádná mezipaměť | EF5 do mezipaměti | EF6 žádná mezipaměť | EF6 do mezipaměti |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Výčet všech 18723 dotazů                                          | 124          | 125.4      | 124.3        | 125.3      |
| Jak se vyhnout oblouku (pouze první 800 dotazy, bez ohledu na složitost)  | 41.7         | 5.5        | 40.5         | 5.4        |
| Právě AggregatingSubtotals dotazy (178 celkem – nemusí tedy přenášet úklidu) | 39.5         | 4.5        | 38.1         | 4.6        |

*Celou dobu v sekundách.*

Osobnostní - při provádění mnoho různých dotazů (například dynamicky vytvořené dotazy), ukládání do mezipaměti nepomůže a výsledné vyprazdňování mezipaměti můžete ponechat dotazy, které je výhodná maximum z jejího použití ukládání do mezipaměti plánu.

Dotazy AggregatingSubtotals jsou nejsložitější dotazů, které jsme testovali s. Podle očekávání, tím složitější je dotaz další výhody, zobrazí se do mezipaměti plánu dotazu.

Protože CompiledQuery je ve skutečnosti dotaz LINQ s svůj plán v mezipaměti, porovnání CompiledQuery oproti ekvivalentní Entity SQL dotazu by měl mít podobné výsledky. Ve skutečnosti Pokud aplikace obsahuje velké množství dynamických dotazů Entity SQL, naplnění mezipaměti s dotazy také fakticky způsobí CompiledQueries "dekompilovat", když budou zapsány z mezipaměti. V tomto scénáři se dá vylepšit výkon tím, že zakážete, ukládání do mezipaměti na dynamické dotazy k určení priority CompiledQueries. Ještě lepší je samozřejmě, bude pro přepsání aplikace pro použití parametrizovaných dotazů místo dynamických dotazů.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 pomocí CompiledQuery ke zlepšení výkonu s dotazy LINQ

Naše testy zjistí, že pomocí CompiledQuery přinést výhody 7 % přes autocompiled LINQ dotazy; To znamená, že strávíte 7 % méně času provádění kódu v sadě Entity Framework; neznamená to, že vaše aplikace bude rychlejší % 7. Obecně řečeno náklady na psaní a údržbě CompiledQuery objekty v EF 5.0 nemusí být vhodné potíže s ve srovnání s výhody. Vaše vzdálenost mohou lišit, tak výkon tuto možnost, pokud váš projekt vyžaduje další nasdílení změn. Všimněte si, že CompiledQueries jsou pouze kompatibilní s odvozenému objektu ObjectContext modely a není kompatibilní s modely odvozené DbContext.

Další informace o vytváření a vyvolávání CompiledQuery najdete v tématu [zkompilován dotazy (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Existují dvě okolnosti, které je třeba provést při použití CompiledQuery, a to nutnost používat statické instance a potíží, že budou mít s skládání. Následuje podrobné vysvětlení těchto dvou důležitých informací.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 použít statické instance CompiledQuery

Protože je kompilování dotazu LINQ časově náročný proces, nechceme to pokaždé, když budeme potřebovat načíst data z databáze. Instance CompiledQuery umožňují jednou kompilace a spuštění více než jednou, ale musí být opatrní a nákupem znovu použít stejnou instanci CompiledQuery pokaždé, když místo kompilaci znovu a znovu. Použití statických členů k ukládání instancí CompiledQuery nezbytná.; jinak se nezobrazí žádnou výhodu.

Předpokládejme například, že vaše stránka obsahuje následující tělo metody pro zpracování zobrazení produkty pro vybrané kategorie:

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

V tomto případě vytvoříte novou instanci CompiledQuery průběžně pokaždé, když je volána metoda. Místo toho přinese zlepšení výkonu načtením příkaz úložiště z mezipaměti plánu dotazu, abyste CompiledQuery projdou kompilátoru plán pokaždé, když je vytvořena nová instance. Ve skutečnosti je bude možné zahlcení vaší mezipaměti plánu dotazu s novým záznamem CompiledQuery pokaždé, když je volána metoda.

Místo toho chcete vytvořit instanci statické v kompilovaném dotazu, takže pokaždé, když je volána metoda vyvoláváte stejný zkompilovaný dotaz. Jeden ze způsobů, jak to tedy přidáním CompiledQuery instance jako člen objektu kontextu.  Potom můžete provést akce trochu čistější díky přístupu CompiledQuery s využitím pomocnou metodu:

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

Tuto metodu helper by spustit následujícím způsobem:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>3.3.2 sestavování přes CompiledQuery

Je velmi užitečné; možnost compose přes jakýkoli dotaz LINQ k tomuto účelu můžete jednoduše vyvolat metodu po položka IQueryable, jako *Skip()* nebo *Count()*. Však to tedy v podstatě vrátí nový objekt IQueryable. Není nutné nic zastavit technicky z sestavování přes CompiledQuery, díky tomu budou generování nového IQueryable objektu, který vyžaduje procházející kompilátoru plán znovu.

Některé součásti se využívají složené IQueryable objekty umožňují pokročilé funkce. Například ASP. NET prvku GridView může být vázán na data IQueryable objektu prostřednictvím vlastnosti metoda SelectMethod. Přes tento objekt IQueryable umožňuje řazení a stránkování datovém modelu se pak compose prvku GridView. Jak vidíte, pomocí prvku GridView CompiledQuery by přístupů kompilovaném dotazu ale vygeneruje nový dotaz autocompiled.

Zákaznický poradní tým to popisuje v jejich "Potenciální výkon problémy s zkompilován LINQ dotaz znovu zkompiluje" blogovém příspěvku: <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Jedno místo, kde můžete narazit to je při přidávání progresivní filtry dotazu. Předpokládejme například, že byste měli stránku zákazníky s několika rozevíracích seznamech pro volitelné filtry (například země a OrdersCount). Tyto filtry můžete vytvářet přes výsledky IQueryable CompiledQuery, ale to uděláte, dojde v nový dotaz projít kompilátoru plán pokaždé, když ji spustíte.

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

 Abyste předešli této opakované kompilace, je možné přepsat CompiledQuery možné filtry vzít v úvahu:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Který by vyvolání v uživatelském rozhraní, jako jsou:

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

 Kompromis tady je příkaz generované úložiště bude mít vždy filtry se kontroly hodnoty null, ale tady by měly být pro server databáze pro optimalizaci poměrně jednoduchý:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 ukládání do mezipaměti metadat

Entity Framework také podporuje ukládání do mezipaměti metadat. To je v podstatě ukládání do mezipaměti informace o typu a informace o mapování typu databáze po různých připojení do stejného modelu. Metadata mezipaměti je jedinečná na doménu aplikace.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 ukládání do mezipaměti metadat algoritmus

1.  Informace metadat modelu je uložen v objektu ItemCollection pro každý EntityConnection.
    -   Jako poznámka na okraj existují různé objekty ItemCollection pro různé součásti modelu. Například StoreItemCollections obsahuje informace o modelu databáze; ObjectItemCollection obsahuje informace o modelu dat Kolekci EdmItemCollection obsahuje informace o konceptuálního modelu.

2.  Pokud dvě připojení používat stejný připojovací řetězec, budou sdílet stejnou instanci ItemCollection.
3.  Funkčně ekvivalentní, ale pomocí textu jiné připojovací řetězce může vést k rozdílná metadata mezipaměti. Připojovací řetězce, není to jednoduše změnou pořadí tokeny by měl být sdílená metadata tokenizovat. Ale dva připojovací řetězce, které vypadá to, že funkčně stejný nemusí být vyhodnoceny jako shodné Tokenizace.
4.  Element ItemCollection je pravidelně kontroluje pro použití. Pokud je zjištěno, že pracovní prostor nepřistupovalo nedávno, budou označeny pro vyčištění na další vyčištění mezipaměti.
5.  Vytváření pouze EntityConnection způsobí, že mezipaměti metadat, který se má vytvořit (v případě, že kolekce položek v něm nelze inicializovat dokud je otevřeno připojení). Tento pracovní prostor zůstane v paměti, dokud nebude algoritmu ukládání do mezipaměti určuje, že ho nepoužívá "".

Zákaznický poradní tým zapsala blogový příspěvek popisuje uchovávající odkaz na objektu ItemCollection předejdete "vyřazení" při použití velkých modelů: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 vztah mezi ukládání do mezipaměti metadat a plán dotazu ukládání do mezipaměti

Instance mezipaměti plánu dotazu nachází v objektu MetadataWorkspace ItemCollection typů úložiště. To znamená, že příkazy v mezipaměti úložiště se použije pro dotazy na jakýkoli kontext vytvořen pomocí daného objektu MetadataWorkspace. Také znamená, že pokud máte dva řetězce připojení, které se mírně liší a po tokenizaci se neshodují, budete mít jiný dotaz, naplánujte instancí mezipaměti.

### <a name="35-results-caching"></a>3.5 výsledky ukládání do mezipaměti

S výsledky ukládání do mezipaměti (označované také jako "druhé úrovně ukládání do mezipaměti") zachovat výsledky dotazů v místní mezipaměti. Po zadání dotazu, nejprve zobrazí, pokud jsou k dispozici místně před dotaz proti úložišti výsledky. Když výsledky ukládání do mezipaměti není přímo podporován rozhraním Entity Framework, je možné přidat druhou úroveň mezipaměti s použitím zprostředkovatele zabalení. Příklad zabalení zprostředkovatele s mezipamětí druhé úrovně se od Alachisoft [Entity Framework druhou úroveň mezipaměti podle NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Tato implementace druhé úrovně, ukládání do mezipaměti je vložená funkce, která přebírá místo po vyhodnocení výrazu LINQ (a funcletized) a plán provádění dotazu je vypočítáván nebo načíst z mezipaměti první úrovně. Druhé úrovně mezipaměti pak uloží pouze výsledky nezpracovaná databáze, tak kanálu materializace stále provede později.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 Další informace o ukládání do mezipaměti s tímto poskytovatelem zabalení výsledky

-   Julie Lerman zapsala článku MSDN "Druhé úrovně ukládání do mezipaměti v Entity Framework a Windows Azure", obsahující aktualizace poskytovatele zabalení Ukázka použití ukládání do mezipaměti systému Windows Server AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Pokud pracujete s Entity Framework 5, na blogu týmu má příspěvek, který popisuje, jak pracovat s poskytovateli ukládání do mezipaměti pro Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Zahrnuje také šablona T4 pro automatizaci přidání úrovně 2. ukládání do mezipaměti do projektu.

## <a name="4-autocompiled-queries"></a>4 Autocompiled dotazy

Vystavení dotaz na databázi pomocí Entity Frameworku ho musí projít řadou kroků před skutečně materializaci výsledky; jeden takový krokem je kompilace dotazu. Dotazy SQL entity používalo mít dobrý výkon, se automaticky uloží do mezipaměti, aby druhý nebo třetí čas spuštění stejný dotaz může přeskočit kompilátoru plánu a místo toho použijte plánů v mezipaměti.

Entity Framework 5 zavedená, automatické ukládání do mezipaměti pro funkci LINQ na entity dotazy a také. V posledních verzích sady Entity Framework vytváření CompiledQuery urychlit byl výkon běžnou praxí, jak to s žádným LINQ dotazu entity možné ukládat do mezipaměti. Protože ukládání do mezipaměti se teď provádí automaticky bez použití CompiledQuery, budeme volat tuto funkci "autocompiled dotazy". Další informace o mezipaměti plánu dotazu a jeho mechanics najdete v článku ukládání do mezipaměti plánu dotazu.

Entity Framework rozpozná, pokud dotaz vyžaduje překompilovat, a o to postará při vyvolání dotaz i v případě, kdyby byl zkompilován před. Běžné podmínky, které způsobí dotaz překompilovat jsou:

-   Změna MergeOption přidružené k vašemu dotazu. V mezipaměti dotaz nebude použit, místo toho kompilátor plán znovu spustí a nově vytvořený plán získá uložili do mezipaměti.
-   Změna hodnoty ContextOptions.UseCSharpNullComparisonBehavior. Budete mít stejný účinek jako v případě změny MergeOption.

Další podmínky zabránit použití mezipaměti váš dotaz. Obvyklými příklady jsou:

-   Použití rozhraní IEnumerable&lt;T&gt;. Obsahuje&lt;&gt;(hodnota T).
-   Pomocí funkcí, které vytvářejí dotazy s konstanty.
-   Pomocí vlastností objektu není namapována.
-   Propojení dotaz k jinému dotazu, který vyžaduje třeba znovu zkompilovat.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 pomocí IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T)

Entity Framework neukládá do mezipaměti dotazů, které vyvolají IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T) proti kolekci v paměti, protože hodnoty kolekce se považují za volatile. Následující příklad dotazu nebudou zapisována do mezipaměti, abyste ho vždy se zpracuje kompilátorem plánu:

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

Mějte na paměti, která se spustí velikost IEnumerable, proti které obsahuje Určuje, jak rychle nebo pomalu jak je zkompilován dotaz. Výkon může být negativně výrazně při používání rozsáhlých kolekcí, jako je uvedeno v předchozím příkladu.

Entity Framework 6 obsahuje Optimalizace způsobu, jakým IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T) funguje při provádění dotazů. Kód SQL, který je generován se vytvoří mnohem rychleji a lépe čitelný, a ve většině případů se také provede rychleji na serveru.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 pomocí funkcí, které vytvářejí dotazy pomocí konstant

Operátory Skip(), Take(), metoda Contains() a DefautIfEmpty() LINQ záměrně neprodukují příkazy jazyka SQL s parametry, ale místo toho umístit hodnot předaných jako konstanty. Z tohoto důvodu dotazy, které jinak může být identické skončí zahlcení dotaz plánování mezipaměti, v zásobníku EF a na serveru databáze a získat reutilized není Pokud stejnou konstanty se používají v provádění dalších dotazů. Příklad:

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

V tomto příkladu se pokaždé, když tento dotaz provádí s jinou hodnotou pro id dotazu se zkompiluje do nového plánu.

V konkrétní věnujte pozornost tomu použití Skip a Take, při stránkování. EF6 mít tyto metody přetížení lambda, umožňující efektivní plán dotazu z mezipaměti opakovaně použitelné protože EF můžete zachytit proměnné předána tyto metody a překládat je SQLparameters. Navíc pomáhají zachovat mezipaměti přehlednější, protože každý dotaz s jinou konstantou Skip a Take, jinak by zobrazí jeho vlastní položka mezipaměti plánu dotazu.

Vezměte v úvahu následující kód, který je neoptimální ale slouží pouze k exemplify Tato třída dotazy:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Rychlejší verzi Tento stejný kód by vyžadovalo volání přeskočit pomocí výrazu lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Druhý fragment kódu může spustit až 11 % rychlejší, protože stejný plán dotazu, který se používá při každém spuštění dotazu, což šetří čas procesoru a zabraňuje zahlcení mezipaměti dotazů. Navíc vzhledem k tomu, že parametr Skip je v uzávěru kód může také vypadat takto nyní:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 pomocí vlastností objektu bez mapované

Když dotaz používá vlastnosti typu objektu není mapován jako parametr a dotaz nebude získat uložené v mezipaměti. Příklad:

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

V tomto příkladu se předpokládá, že třída NonMappedType není součástí modelu Entity. Tento dotaz lze snadno změnit nelze použít typ není namapována a místo toho použít místní proměnné jako parametr dotazu:

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

Dotaz v tomto případě bude moct získat do mezipaměti a budou těžit z mezipaměti plánu dotazu.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 odkazování na dotazy, které vyžadují opětovné kompilaci

V následujícím příkladu stejný jako výše Pokud máte druhý dotaz, který závisí na dotaz, který je potřeba překompilovat, celý druhého dotazu se také překompilují. Tady je příklad pro ilustraci tohoto scénáře:

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

V příkladu je obecný, ale ukazuje, jak je propojení firstQuery příčinou secondQuery nebude možné získat uložené v mezipaměti. Pokud firstQuery nebyla dotaz, který vyžaduje opětovné kompilaci, pak secondQuery by mít se zapíší do mezipaměti.

## <a name="5-notracking-queries"></a>5 NoTracking dotazy

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 zakázání ke snížení režie na správu stavu sledování změn

Pokud se v případě jen pro čtení a režie načítání objektů do objektu ObjectStateManager určitě nepřejete, můžete posílat dotazy "No sledování".  Sledování změn je možné zakázat na úrovni dotazů.

Nezapomeňte, že tím, že zakážete sledování vám změn jsou účinně vypnutí mezipaměti objektů. Když odešlete dotaz pro entitu, jsme nelze přeskočit materializace díky přebírání výsledky dříve materializovaného dotazu z objektu ObjectStateManager. Pokud jsou opakovaně dotazování na stejné entity ve stejném kontextu, může se ve skutečnosti zobrazit výkonu těžit z povolení řešení change tracking.

Při dotazování pomocí objektu ObjectContext, instance ObjectQuery a ObjectSet si budou pamatovat MergeOption po nastavení a dotazy, které se skládají na nich zdědí efektivní MergeOption nadřazený dotaz. Při použití DbContext, je možné zakázat sledování získaný pomocí funkce modifikátor AsNoTracking() DbSet.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 zakázání sledování změn pro dotaz při použití DbContext

Přepnout režim dotazu na NoTracking pomocí zřetězení volání metody AsNoTracking() v dotazu. Na rozdíl od ObjectQuery nemají DbSet a DbQuery třídy v rozhraní API pro DbContext měnitelné vlastnosti pro MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 zakázání řešení change tracking na úrovni dotazů pomocí ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 zakázání sledování změn pro entitu celá sada s použitím objektu ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 ukázka výhody výkonu dotazů NoTracking metriky testu

V tomto testu podíváme za cenu porovnáním sledování NoTracking dotazy pro Navision model vyplnění objektu ObjectStateManager. Viz dodatek popis modelu Navision a typy dotazů, které byly spuštěny. V tomto testu jsme iteraci v rámci seznamu dotazů a každý z nich provedena pouze jednou. Spustili jsme dvě varianty testu, jednou s dotazy NoTracking a jednou s možností sloučení výchozí pouze "Přidat". Jsme spustili každou změnu 3krát a trvat střední hodnoty spuštění. Mezi testy jsme vymazat mezipaměť dotazu na SQL serveru a zmenšit databázi tempdb spuštěním následujících příkazů:

1.  PŘÍKAZ DBCC DROPCLEANBUFFERS
2.  PŘÍKAZ DBCC FREEPROCCACHE
3.  Příkaz DBCC SHRINKDATABASE (databáze tempdb, 0)

Výsledky, střední více než 3 spuštění testů:

|                        | ŽÁDNÉ SLEDOVÁNÍ – PRACOVNÍ SADA | ŽÁDNÉ SLEDOVÁNÍ – ČAS | PŘIPOJIT POUZE – PRACOVNÍ SADA | PŘIPOJIT POUZE – ČAS |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5 bude mít menší nároky na paměť na konci spuštění než Entity Framework 6. Další paměti používané Entity Framework 6 je výsledkem další paměť struktur a kód, který povolení nových funkcí a vyšší výkon.

Je také vymazat rozdíl v nároky na paměť při použití objektu ObjectStateManager. Entity Framework 5 vyšší nároky na jeho místo o 30 % při udržování přehledu o všechny entity, které jsme materializovaného z databáze. Když to tak uděláte Entity Framework 6 zvýšit své nároky na místo o 28 %.

Z hlediska času Entity Framework 6 lepší výkon než Entity Framework 5 v tomto testu pomocí velký okraj. Entity Framework 6 dokončit test v zhruba 16 % času používané Entity Framework 5. Kromě toho Entity Framework 5 % 9, další nějakou dobu trvá dokončení při použití objektu ObjectStateManager. Ve srovnání s Entity Framework 6 používá více času při použití objektu ObjectStateManager % 3.

## <a name="6-query-execution-options"></a>6 možnosti spuštění dotazu

Entity Framework nabízí několik různých způsobů, jak dotaz. Použijeme podívejte se na následující možnosti, porovnat výhody a nevýhody jednotlivých a zkontrolovat jejich výkonnostní charakteristice:

-   Technologie LINQ to Entities.
-   Žádné sledování LINQ to Entities.
-   Entita SQL přes ObjectQuery.
-   Entita SQL přes EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6.1 dotazech LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**V oblasti IT**

-   Vhodný pro operace vytvoření.
-   Plně materializovaného objekty.
-   Nejjednodušší jak psát pomocí syntaxe integrovaná v programovacím jazyce.
-   Dobrý výkon.

**Nevýhody**

-   Určitá technická omezení, jako například:
    -   Využitím DefaultIfEmpty OUTER JOIN dotazů za následek složitější dotazy než jednoduché příkazy OUTER JOIN v Entity SQL.
    -   Stále nelze použít s obecné porovnávání vzorů.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6.2 žádné sledování LINQ dotazy na entity

Když kontextu odvozuje ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Když kontextu odvozuje DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**V oblasti IT**

-   Vylepšený výkon než regulární dotazů LINQ.
-   Plně materializovaného objekty.
-   Nejjednodušší jak psát pomocí syntaxe integrovaná v programovacím jazyce.

**Nevýhody**

-   Není vhodný pro operace vytvoření.
-   Určitá technická omezení, jako například:
    -   Využitím DefaultIfEmpty OUTER JOIN dotazů za následek složitější dotazy než jednoduché příkazy OUTER JOIN v Entity SQL.
    -   Stále nelze použít s obecné porovnávání vzorů.

Všimněte si, že i když není zadaný NoTracking nebudou pro účely dotazů, které Skalární vlastnosti projektu. Příklad:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Tento konkrétní dotaz nemá určenou explicitně se NoTracking, ale vzhledem k tomu, že není materializaci typ, který je známo, že správce stavu objektu pak výsledku materializovaného není sledována.

### <a name="63-entity-sql-over-an-objectquery"></a>6.3 entity SQL přes ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**V oblasti IT**

-   Vhodný pro operace vytvoření.
-   Plně materializovaného objekty.
-   Podporuje dotazování, ukládání do mezipaměti plánu.

**Nevýhody**

-   Zahrnuje dotazu textové řetězce, které jsou více náchylná k chybám uživatelů než konstrukce dotazů integrované do jazyka.

### <a name="64-entity-sql-over-an-entity-command"></a>6.4 entity SQL přes příkaz Entity

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

**V oblasti IT**

-   Podporuje dotazování, ukládání do mezipaměti plánu v rozhraní .NET 4.0 (ukládání do mezipaměti plánu se podporuje další typy dotazů v rozhraní .NET 4.5).

**Nevýhody**

-   Zahrnuje dotazu textové řetězce, které jsou více náchylná k chybám uživatelů než konstrukce dotazů integrované do jazyka.
-   Není vhodný pro operace vytvoření.
-   Výsledky nejsou automaticky vyhodnocena a musí být četlo čtecí modul dat.

### <a name="65-sqlquery-and-executestorequery"></a>6.5 SqlQuery a ExecuteStoreQuery

SqlQuery v databázi:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery na DbSet:

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

**V oblasti IT**

-   Obecně nejrychlejší výkon od plánu kompilátoru přeskočí.
-   Plně materializovaného objekty.
-   Vhodný pro operace vytvoření při použití z DbSet.

**Nevýhody**

-   Dotaz je textové a náchylné k chybám.
-   Dotaz se váže na konkrétní back-endu s využitím úložiště sémantikou místo koncepční sémantiku.
-   Při dědičnost je k dispozici, je potřeba účet pro mapování podmínky pro požadovaný typ rukodìlných dotazu.

### <a name="66-compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**V oblasti IT**

-   Poskytuje až zlepšení výkonu 7 % prostřednictvím pravidelné dotazů LINQ.
-   Plně materializovaného objekty.
-   Vhodný pro operace vytvoření.

**Nevýhody**

-   Zvýšení složitosti a programování režijní náklady.
-   Zlepšení výkonu je ztracena při psaní nad kompilovaném dotazu.
-   Některé dotazy LINQ nelze zapsat jako CompiledQuery – například projekcí anonymních typů.

### <a name="67-performance-comparison-of-different-query-options"></a>6.7 porovnání výkonu možností jiný dotaz

Byly jednoduché microbenchmarks, kde se vytvoření kontextu vypršel časový limit zařazení do testu. Jsme měří dotazování 5000 časy pro sadu entit bez mezipaměti v řízeném prostředí. Tato čísla jsou mají být provedeny s upozorněním: neodrážejí aktuální počet vytvářených aplikace, ale místo toho jsou jak velká část rozdíly ve výkonnosti se, pokud jsou porovnány různé možnosti dotazování velmi přesné měření jablka na apples, s výjimkou náklady na vytvoření nový kontext.

| EF  | Test                                 | Doba (ms) | Paměť   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | Dotaz Linq ObjectContext             | 2692      | 38277120 |
| EF5 | Žádné sledování dotazu DbContext Linq     | 2818      | 41840640 |
| EF5 | Dotaz Linq DbContext                 | 2930      | 41771008 |
| EF5 | Objekt ObjectContext Linq dotaz bez sledování | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | Dotaz Linq ObjectContext             | 3074      | 45248512 |
| EF6 | Žádné sledování dotazu DbContext Linq     | 3125      | 47575040 |
| EF6 | Dotaz Linq DbContext                 | 3420      | 47652864 |
| EF6 | Objekt ObjectContext Linq dotaz bez sledování | 3593      | 45260800 |

![EF5 micro srovnávací testy, 5000 teplé iterací](~/ef6/media/ef5micro5000warm.png)

![EF6 micro srovnávací testy, 5000 teplé iterací](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks jsou velmi citlivé na malých změn v kódu. V tomto případě rozdíl mezi náklady na Entity Framework 5 a Entity Framework 6 jsou z důvodu přidání [zachycení](~/ef6/fundamentals/logging-and-interception.md) a [transakční vylepšení](~/ef6/saving/transactions.md). Tato čísla microbenchmarks jsou však zesilovací vizi do velmi malý fragment toho, co dělá rozhraní Entity Framework. Reálné scénáře teplé dotazů by se neměly zobrazovat regrese výkonu při upgradu z Entity Framework 5 na Entity Framework 6.

Porovnat výkon skutečné možností jiný dotaz, vytvořili jsme 5 variace samostatný test, kde používáme možnost jiný dotaz, vyberte všechny produkty, jejichž název kategorie je "Nápoje". Každá iterace zahrnuje náklady na vytvoření kontextu a náklady na materializaci všechny vrácené entity. Před provedením součtu vypršel časový limit 1000 iterací jsou spuštěny untimed 10 iterací. Střední spustit z 5 spuštění každého testu jsou výsledky zobrazeny. Další informace najdete v tématu dodatku B, který obsahuje kód pro test.

| EF  | Test                                        | Doba (ms) | Paměť   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Příkaz ObjectContext Entity                | 621       | 39350272 |
| EF5 | Kontext databáze. dotaz Sql na databázi             | 825       | 37519360 |
| EF5 | Query Store ObjectContext                   | 878       | 39460864 |
| EF5 | Objekt ObjectContext Linq dotaz bez sledování        | 969       | 38293504 |
| EF5 | Pomocí dotazu objektu ObjectContext Entity Sql | 1089      | 38981632 |
| EF5 | Zkompilovaný dotaz ObjectContext                | 1099      | 38682624 |
| EF5 | Dotaz Linq ObjectContext                    | 1152      | 38178816 |
| EF5 | Žádné sledování dotazu DbContext Linq            | 1208      | 41803776 |
| EF5 | Kontext databáze. dotaz Sql na DbSet                | 1414      | 37982208 |
| EF5 | Dotaz Linq DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Příkaz ObjectContext Entity                | 480       | 47247360 |
| EF6 | Query Store ObjectContext                   | 493       | 46739456 |
| EF6 | Kontext databáze. dotaz Sql na databázi             | 614       | 41607168 |
| EF6 | Objekt ObjectContext Linq dotaz bez sledování        | 684       | 46333952 |
| EF6 | Pomocí dotazu objektu ObjectContext Entity Sql | 767       | 48865280 |
| EF6 | Zkompilovaný dotaz ObjectContext                | 788       | 48467968 |
| EF6 | Žádné sledování dotazu DbContext Linq            | 878       | 47554560 |
| EF6 | Dotaz Linq ObjectContext                    | 953       | 47632384 |
| EF6 | Kontext databáze. dotaz Sql na DbSet                | 1023      | 41992192 |
| EF6 | Dotaz Linq DbContext                        | . 1290      | 47529984 |


![EF5 teplé dotazu 1000 iterací](~/ef6/media/ef5warmquery1000.png)

![EF6 teplé dotazu 1000 iterací](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Pro úplnost jsme zahrnuli variace, kde jsme na EntityCommand spuštění dotazu Entity SQL. Ale protože výsledky nejsou vyhodnocena pro takové dotazy, porovnání, není nutně jablka jablka. Test zahrnuje aproximace pro materializaci vyzkoušet srovnávání spravedlivější.

V tomto případě začátku do konce, Entity Framework 6 lepší výkon než Entity Framework 5 z důvodu vylepšení výkonu na několik částí zásobníku, včetně mnohem míň DbContext inicializace a rychlejší MetadataCollection&lt;T&gt; vyhledávání.

## <a name="7-design-time-performance-considerations"></a>Faktory ovlivňující výkon čas návrh 7

### <a name="71-inheritance-strategies"></a>7.1 strategie dědičnosti

Dalším aspektem výkon při použití rozhraní Entity Framework je strategie dědičnosti, které používáte. Entity Framework podporuje 3 základní typy dědičnosti a jejich kombinace:

-   V řádku je reprezentované tabulky na hierarchii (TPH) – kde každý dědičnosti nastavit mapování na tabulku s sloupec diskriminátoru, který označuje, který konkrétní typ v hierarchii.
-   Tabulky podle typu (TPT) – kam každý typ má své vlastní tabulky v databázi. podřízené tabulky definovat pouze sloupce, které neobsahuje nadřazené tabulky.
-   Tabulky podle třídy (TPC) – kam každý typ má své vlastní celou tabulku v databázi. podřízené tabulky definovat všechny jejich polí, včetně těch, které definovaný v nadřazené typy.

Používá-li model dědičnosti TPT, dotazy, které se generují bude složitější než ty, které jsou generovány s dalšími strategiemi dědičnosti, což může způsobit na delší dobu provádění ve storu.  Obecně bude trvat déle, vygenerujte dotazy TPT modelu a materializovat výsledných objektech.

"Důležité informace o výkonu při použití dědičnosti TPT (tabulka na jeden typ) v Entity Framework" najdete v příspěvku blogu MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 vyhnout TPT v aplikacích první Model nebo Code First

Při vytváření modelu přes existující databázi, která má schéma TPT nemáte celou řadu možností. Ale při vytváření aplikace pomocí modelu první nebo Code First, měli byste se vyhnout TPT dědičnost dopadům na výkon.

Při použití modelu první v Průvodci návrháře entit, zobrazí se TPT jakékoli dědičnosti ve vašem modelu. Pokud chcete přepnout na strategii TPH dědičnosti s první Model, můžete použít "Entity návrháře databáze generování Power Pack" k dispozici z Galerie sady Visual Studio ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Při použití Code First pro konfiguraci mapování modelu s dědičnosti, EF použije TPH ve výchozím nastavení, proto všechny entity v hierarchii dědičnosti budou zmapována do stejné tabulky. V části "Mapování s rozhraní Fluent API" z "Kód první v entitě Framework4.1" článek v časopise MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) pro další podrobnosti.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 upgrade z EF4 ke zlepšení generování modelu čas

SQL Server – konkrétní zlepšení algoritmu, který generuje vrstvy úložiště (SSDL) modelu je k dispozici v Entity Framework 5 a 6 a jako aktualizace Entity Framework 4, při instalaci sady Visual Studio 2010 SP1. Následující výsledky testů ukazují zlepšení při vytváření velmi velkých objemů modelu, v tomto případě Navision modelu. Další podrobnosti naleznete v tématu dodatku C.

Model obsahuje sady 1005 entit a sad 4227 přidružení.

| Konfigurace                              | Rozpis uplynulý čas                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Generování souborů SSDL: 2 hr 27 min <br/> Generování mapování: 1 sekunda <br/> CSDL generace: 1 sekunda <br/> ObjectLayer generace: 1 sekunda <br/> Generování zobrazení: 2 h 14 min |
| Visual Studio 2010 SP1, Entity Framework 4 | Generování souborů SSDL: 1 sekunda <br/> Generování mapování: 1 sekunda <br/> CSDL generace: 1 sekunda <br/> ObjectLayer generace: 1 sekunda <br/> Generování zobrazení: 1 hr 53 min.   |
| Visual Studio 2013, rozhraní Entity Framework 5     | Generování souborů SSDL: 1 sekunda <br/> Generování mapování: 1 sekunda <br/> CSDL generace: 1 sekunda <br/> ObjectLayer generace: 1 sekunda <br/> Generování zobrazení: 65 minut    |
| Visual Studio 2013, rozhraní Entity Framework 6     | Generování souborů SSDL: 1 sekunda <br/> Generování mapování: 1 sekunda <br/> CSDL generace: 1 sekunda <br/> ObjectLayer generace: 1 sekunda <br/> Generování zobrazení: 28 sekundách.   |


Je vhodné poznamenat, že při generování souborů SSDL, zatížení je téměř zcela strávená na serveru SQL Server čeká na klientském počítači. vývojové nečinnosti výsledků k téhle akci vrátit ze serveru. Specializující by měl ocení zejména toto vylepšení. Je také vhodné poznamenat, že v podstatě náklady na generování modelu probíhá generování zobrazení nyní.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7.3 nejprve rozdělení velkých modelů s databází a Model First

Jak se zvyšuje velikost modelu, na plochu návrháře stane nevypadala a obtížně použitelný. Obvykle považujeme modelu s více než 300 entit příliš velkou efektivní použití návrháře. V následujícím příspěvku blogu popisuje několik možností pro rozdělení velkých modelů: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Příspěvek byl zapsán pro první verzi Entity Frameworku, ale postup se vztahuje.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7.4 důležité informace o výkonu s Entity Data Source Control

Zaznamenali jsme případy vícevláknové výkonnostních a zátěžových testů ve kterém výkon webové aplikace pomocí ovládacího prvku EntityDataSource deteriorates výrazně. Příčinou je, že třídu EntityDataSource platí MetadataWorkspace.LoadFromAssembly opakovaně volá na sestavení odkazuje webové aplikace ke zjištění typy použité jako entity.

Řešením je nastavit ContextTypeName třídu EntityDataSource platí na název typu odvozené třídy objektu ObjectContext. Tím dojde k vypnutí mechanismus, který prohledá všechna odkazovaná sestavení pro typy entit.

Nastavení pole ContextTypeName zabrání také o funkční problém, ve kterém třídu EntityDataSource platí v rozhraní .NET 4.0 výjimce ReflectionTypeLoadException byla vyvolána při provádění jej nelze načíst typ z sestavení prostřednictvím reflexe. Tento problém byl vyřešen v rozhraní .NET 4.5.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7.5 POCO entity a change tracking proxy servery

Entity Framework umožňuje používat vlastní datové třídy společně s datový model bez provedení změn na datové třídy sami. To znamená, že můžete použít "prostý staré" CLR objektů POCO, jako je například existujících objektů domény s datovým modelem. Tyto POCO datových tříd (označované také jako ignorujících objekty), které jsou mapovány na subjekty, které jsou definovány v datovém modelu, podporují většinu stejného dotazu, vložit, aktualizovat a odstranit chování jako typy entit, které jsou generovány pomocí nástroje modelu Entity Data Model.

Entity Framework můžete také vytvořit proxy třídy odvozené od POCO typy, které se používají, když chcete povolit funkce, jako je automatické sledování změn pro POCO entity a opožděné načítání. Třídy POCO musí splňovat určité požadavky umožňující Entity Framework pro použití proxy, jak je popsáno zde: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Možnost sledování proxy bude informovat správce stavu objektu pokaždé, když vlastnosti vaší entity má svou hodnotu mění, takže Entity Framework neustále ví skutečného stavu entity. To se provádí přidáním oznámení událostí do těla metody setter vlastnosti, a s object Manageru stav zpracování těchto událostí. Všimněte si, že vytváření proxy entity se obvykle být dražší než vytvoříte entitu POCO bez proxy serveru z důvodu přidání sadu událostí, které jsou vytvořené Entity Framework.

Entita POCO nemá proxy sledování změn, změny se nacházejí porovnáním obsah entity kopii předchozímu uloženému stavu. Podrobné porovnání se stanou časově náročný proces Pokud máte mnoho entit ve vaší místní, nebo když vaše entity obsahují velmi velké množství vlastností, i v případě, že žádná z nich změněn od posledního porovnání konal úplně.

Stručně řečeno: zaplatíte výkonu při vytváření proxy sledování změn, ale řešení change tracking vám pomůže urychlit proces zjišťování změn při entity mají mnoho vlastností nebo pokud máte mnoho entit v modelu. U entit s malý počet vlastností, kde velikost entity není růst příliš mnoho proxy sledování změn nemusí být mnoho výhod.

## <a name="8-loading-related-entities"></a>8 načtení souvisejících entit

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 opožděné načtení vs. Předběžné načítání

Entity Framework nabízí několik různých způsobů načítání entit, které se vztahují na cílovou entitu. Například při dotazování u produktů, existují různé způsoby, že související objednávky se načtou do Správce stavu objektu. Z hlediska výkonu bude největší dotaz a vezměte v úvahu při načítání související entity, jestli se má použít opožděné načtení nebo nemůžou dočkat, až načítání.

Při použití nemůžou dočkat, až načítání, související entity načtou spolu s cílovou sadou entit. K označení, která související entity, které chcete připojit použijete příkaz Include v dotazu.

Při použití opožděné načtení, připojí počátečního dotazu pouze v cílové sady entit. Ale kdykoli budete přistupovat k vlastnosti navigace, jiný dotaz je vydaný pro úložiště se načíst související entity.

Po načtení entity, žádné další dotazy entity se načtení přímo ze Správce stavu objektu, jestli používáte opožděné načtení nebo předběžné načítání.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 návodu k výběru mezi opožděné načtení a nemůžou dočkat, až načítání

Důležité je, když rozumíte rozdílu mezi opožděné načtení a nemůžou dočkat, až načítání, tak, aby měli správnou volbu pro vaši aplikaci. To vám pomůže vyhodnotit kompromis mezi více požadavků na databázi a jeden požadavek, který může obsahovat velké datové části. Může být vhodné je použít v ostatních částech nemůžou dočkat, až načítání v některé části vaší aplikace a opožděné načtení.

Jako příklad toho, co se děje pod pokličkou Předpokládejme, že chcete zadat dotaz pro zákazníky, kteří žijí v Spojeném království a počtu jejich pořadí.

**Pomocí předběžné načítání**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Pomocí opožděné načtení**

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

Pokud používáte předběžné načítání, budete vydávat jeden dotaz, který vrátí všechny zákazníky a všechny objednávky. Příkaz úložiště vypadá takto:

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

Při použití opožděné načtení, budete nejdřív vydejte následující dotaz:

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

A pokaždé, když přistupujete navigační vlastnost objednávky zákazníka úložišti uživatelů, kteří je vydán jiný dotaz podobný tomuto:

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

Další informace najdete v tématu [načítání související objekty](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 opožděné načtení a nemůžou dočkat, až načítání tahák

Není žádná taková věc, kterou jako univerzální výběrem předběžné načítání a opožděné načtení. Zkuste nejdřív znát rozdíly mezi obě strategie tedy můžete také přijímat podložená rozhodnutí; Zvažte také, pokud váš kód odpovídá podmínkám ke kterékoli z následujících scénářů:

| Scénář                                                                    | Náš návrh                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Je potřeba pro přístup k mnoha navigační vlastnosti z načtených entity? | **Ne** -pravděpodobné bude, že obě možnosti. Nicméně pokud datová část, kterou přináší váš dotaz není příliš velký, že může docházet přinese zlepšení výkonu pomocí předběžné načítání, jak tomu budete potřebovat méně síti, zpomalí sloučit objekty. <br/> <br/> **Ano** – Pokud je potřeba přístup k mnoha navigační vlastnosti z entity, provedli byste, že pomocí několika #include v dotazu se nemůžou dočkat, až načítání. Zahrnout další entity, že čím větší datovou část dotaz vrátí. Jakmile zahrnete tři nebo více entit do vašeho dotazu, zvažte možnost opožděné načtení. |
| Víte, jaká data přesně bude potřeba v době běhu?                   | **Ne** – opožděné načtení bude lepší za vás. Jinak můžete skončit dotazování na data, že nebudete potřebovat. <br/> <br/> **Ano** – nemůžou dočkat, až načítání je pravděpodobně nejlepším řešením; pomůže to zrychlení načítání celé sady. Pokud váš dotaz vyžaduje načítají velké množství dat a toto řešení je příliš pomalé, zkuste opožděné načtení místo.                                                                                                                                                                                                                                                       |
| Váš kód spouští daleko od databáze? (latence sítě)  | **Ne** – Pokud latence sítě není problém, pomocí opožděné načtení může zjednodušit kód. Mějte na paměti, že topologii vaší aplikace se může změnit postupně nevyřídí databáze blízkosti samozřejmost tedy. <br/> <br/> **Ano** – Pokud síť není problém, pouze se můžete rozhodnout, co lépe vyhovuje vašemu scénáři. Předběžné načítání obvykle bude lepší, protože vyžaduje menší počet zpátečních cest.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>8.2.2 aspekty výkonu s více zahrnuje

Slyšeli jsme, že otázky výkonu, které zahrnují problémech čas odpovědi serveru, příčinu problému při často dotazy s více příkazy Include. Zatímco včetně souvisejících entit v dotazu je efektivní, je důležité pochopit, co se děje na pozadí.

Trvá poměrně dlouho pro dotaz s více příkazy Include tak, projděte si naše interní plán kompilátor vytvoří příkaz úložiště. Většina této doby se věnovalo snaze optimalizovat výsledný dotaz. Příkaz generované úložiště bude obsahovat Outer Join nebo sjednocení pro každé zahrnutí, v závislosti na vaší mapování. Dotazy tímto způsobem bude přenést velkých grafů připojených z databáze v jedné datové části, která bude acerbate jakékoli potíže se šířkou pásma, zejména v případě, že dochází k mnoha redundance v datové části (například pokud několik úrovní zahrnout se používá k procházení přidružení ve směru 1 n).

Můžete zkontrolovat pro případy, kde dotazů jako nadměrně velké datové části vrací tak přístup k podkladové TSQL pro dotaz s použitím ToTraceString a spouští se příkaz úložiště v SQL Server Management Studio zobrazíte velikost datové části. V takovém případě můžete zkusit snížit počet vložených příkazů v dotazu jenom umožňuje přinést si data, která potřebujete. Nebo je možné rozdělit svůj dotaz na menší posloupnost poddotazy, například:

**Před dopadem na dřívější kód dotazu:**

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

**Po rozdělení dotazu:**

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

Bude to fungovat jenom u sledovaných dotazy, jak provádíme použijte možnost kontextu má provádět překlad IP adres a přidružení oprava identity.

Stejně jako u opožděné načtení, bude kompromis více dotazů pro menší datové části. Projekce jednotlivé vlastnosti můžete použít také explicitně vybrat pouze potřebná data z jednotlivých entit, ale můžete nesmí být načítání entit v tomto případě a aktualizace nebudou podporovány.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 alternativní řešení Chcete-li získat opožděné načtení vlastností

Entity Framework v současné době nepodporuje opožděné načtení skalární nebo komplexní vlastností. Ale v případech, kdy máte tabulku, která zahrnuje velkých objektů, jako je například objekt BLOB, můžete rozdělení tabulky rozčlenit velké vlastnosti do samostatné entity. Předpokládejme například, že máte tabulku produktů, která zahrnuje sloupce varbinary fotografii. Pokud nepotřebujete často se k této vlastnosti v dotazech, můžete použít pro entity, která je obvykle třeba jen ty části rozdělení tabulky. Entita, která představuje fotografie produktu se načtou jenom když ho potřebujete explicitně.

Dobrý prostředek, který ukazuje, jak povolit rozdělení tabulky je Gil Fink "Tabulky rozdělení v Entity Framework" blogový příspěvek: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 další důležité informace

### <a name="91-server-garbage-collection"></a>9.1 uvolnění paměti serveru

Někteří uživatelé setkat sporu prostředků, která omezuje paralelismu, které jsou se očekává při uvolňování paměti není správně nakonfigurována. Pokaždé, když EF se používá ve scénáři s více vlákny, nebo v jakékoli aplikaci, která vypadá podobně jako na straně serveru systému, ujistěte se, že chcete povolit uvolnění paměti serveru. To se provádí prostřednictvím jednoduché nastavení v konfiguračním souboru aplikace:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

To by měla snížit vaše spor vlákna a zvýšit propustnost až o 30 % ve scénářích procesoru přeplněný. Obecně řečeno byste měli vždy otestovat chování aplikací pomocí klasické kolekce uvolnění paměti (která je vyladěná lépe pro scénáře na straně uživatelského rozhraní a klient) a také kolekce volnění paměti serveru.

### <a name="92-autodetectchanges"></a>9.2 AutoDetectChanges

Jak už bylo zmíněno dříve, Entity Framework může zobrazit problémy s výkonem při mezipaměti objektů má mnoho entit. Některé operace, jako je například přidat, odebrat, hledání, vstupu a SaveChanges, aktivovat volání metoda DetectChanges, které může využívat velké procento využití procesoru podle jak velký stal mezipaměti objektů. Důvodem je, že mezipaměti objektů a zkuste správce stavu objekt zůstat jako synchronizují nejvíce na každou operace prováděné na kontext, tak, aby vyprodukované dat je záruku správnosti v rámci široké škály scénářů.

Obecně je vhodné ponechat Entity Framework automaticky změnit detekce povolené pro celou dobu životnosti vaší aplikace. Pokud váš scénář bude negativně ovlivněna podle vysoké využití procesoru a profily znamenat, že nadměrné spotřeby volání metoda DetectChanges, zvažte dočasné vypnutí AutoDetectChanges v citlivých část kódu:

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

Před vypnutím AutoDetectChanges, je vhodné pochopit, že to může vést ke ztrátě schopnosti ke sledování určité informace o změnách, které budou probíhat na entity Entity Framework. Pokud nesprávně zpracována, tato akce může způsobit nekonzistenci dat ve vaší aplikaci. Další informace o vypnutí AutoDetectChanges najdete v článku \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93-context-per-request"></a>9.3 kontext každý požadavek

Kontext Entity Framework jsou určené pro použití jako krátkodobé a jednorázové instance, aby bylo možné poskytovat optimální výkon prostředí. Kontexty očekává krátkodobé žít a zahodí a proto je implementovaná odlehčení a reutilize metadat, kdykoli je to možné. Ve web scénářích je potřeba to mějte na paměti a není nutné kontext pro více než jedné žádosti. Podobně v mimo web scénářích kontextu měly být zahozeny podle pochopíte různé úrovně ukládání do mezipaměti v Entity Framework. Obecně řečeno jeden by měl nepoužívejte instance kontextu po celou dobu životnosti aplikace, stejně jako kontexty na vlákno a statické kontexty.

### <a name="94-database-null-semantics"></a>9.4 sémantika s hodnotou null databáze

Entity Framework ve výchozím nastavení vygeneruje kód SQL, který má C\# sémantiku porovnání s hodnotou null. Vezměte v úvahu následující příklad dotazu:

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

V tomto příkladu jsme řadu s možnou hodnotou Null proměnné s možnou hodnotou NULL vlastnosti entity, jako je například KódDodavatele a UnitPrice srovnání. Vygenerovaný SQL pro tento dotaz se zeptá, pokud hodnota parametru je stejná jako hodnota sloupce, nebo pokud parametr a hodnoty ve sloupcích mají hodnotu null. Tím se skrýt tak, jak databázový server zpracovává hodnoty Null a bude poskytovat konzistentní C\# null prostředí mezi dodavateli jinou databázi. Na druhé straně generovaný kód je poněkud složitými a nemusí fungovat dobře v případě množství porovnávání v where příkazu dotazu se hodně zvětšuje.

Jedním ze způsobů řešení této situace je pomocí databáze sémantika s hodnotou null. Všimněte si, že to může potenciálně chovat jinak c\# null sémantiku od teď Entity Frameworku vygeneruje jednodušší SQL, který poskytuje způsob, jak databázový stroj zpracovává hodnoty null. Sémantika s hodnotou null databáze může být aktivovaná za kontextu s jedním řádkem jediné konfiguraci proti kontextu konfigurace:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Malé a střední velikosti dotazy nezobrazí zlepšení postřehnutelné výkonu při použití databáze sémantika s hodnotou null, ale rozdíl bude snadněji postřehnutelné na dotazy s velkým množstvím možných porovnávání s hodnotou null.

Ve výše uvedené vzorový dotaz byl rozdíly ve výkonnosti méně než 2 % microbenchmark spuštěné v řízeném prostředí.

### <a name="95-async"></a>9.5 asynchronní

Entity Framework 6 zavedena podpora asynchronní operace při spuštění v rozhraní .NET 4.5 nebo novější. Ve většině případů aplikací, které obsahují vstupně-výstupní operace týkající se sporů využívat na maximum pomocí asynchronního dotazu, který se operace uložení. Pokud vaše aplikace nezpůsobuje žádné kolize vstupně-výstupní operace, použijte asynchronní, v nejlepší případech běžely synchronně a vrátí výsledek ve stejnou dobu jako synchronní volání nebo v nejhorším případě, jednoduše odložit provádění asynchronní úloha a přidat další tim elektronické pro dokončení vašeho scénáře.

Informace o tom, jak asynchronní programovací práce, který vám pomůže rozhodování o tom, pokud asynchronní zlepší výkon vaší aplikace navštívíte [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Další informace týkající se použití asynchronních operací v Entity Framework naleznete v tématu [asynchronního dotazu a uložit](~/ef6/fundamentals/async.md
).

### <a name="96-ngen"></a>9.6 NGEN

Entity Framework 6 nepochází ve výchozí instalaci rozhraní .NET framework. V důsledku toho sestavení rozhraní Entity Framework nejsou že Ngen by ve výchozím nastavení, což znamená, že veškerý kód Entity Framework se řídí stejnou náklady JIT'ing jako jakékoli jiné sestavení jazyka MSIL. To může snížit F5 zkušenosti při vývoji a také úplné spuštění vaší aplikace v produkčním prostředí. Aby bylo možné snížit náklady na využití procesoru a paměti JIT'ing se doporučuje NGEN Entity Framework Image podle potřeby. Další informace o tom, jak zlepšit výkon při spuštění nástroje Entity Framework 6 pomocí technologie NGEN najdete v tématu [zlepšuje výkon při spouštění pomocí technologie NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97-code-first-versus-edmx"></a>9.7 kódu nejprve oproti EDMX

Entity Framework důvodů o problému vzniklé vzájemné napětí Neshoda mezi objektově orientované programování a relačními databázemi tím, že reprezentaci v paměti koncepčního modelu (objekty), schéma úložiště (databáze) a mapování mezi dvě. Tato metadata je volána modelu Entity Data Model nebo EDM pro krátké. Z tohoto modelu EDM Entity Framework bude odvozovat zobrazení umožňujícím zpětnou transformaci dat z objektů v paměti do databáze a zpět.

Pokud Entity Framework je použit společně s souboru EDMX, který formálně určuje konceptuální model, schéma úložiště a mapování, pak fázi načítání modelu se musí ověřit správnost modelu EDM (například, ujistěte se, že nebyly nalezeny žádné mapování), pak Generovat zobrazení, ověření zobrazení a mít tato metadata, která je připravená k použití. Pouze spustit pak může dotaz nebo nová data uložit do úložiště dat.

Přístupu Code First je svou podstatou sofistikované generátor modelu Entity Data Model. Entity Framework je pro vytvoření modelu EDM z poskytnutého kódu; dělá to tak analýza zahrnutých v modelu s použitím konvence a konfigurace modelu přes rozhraní Fluent API třídy. Po sestavení modelu EDM Entity Framework v podstatě se chová stejně jako by měl soubor EDMX bylo k dispozici v projektu. Díky tomu se sestavení modelu z Code First přidá další složitosti, který překládá do pomalejší čas spuštění pro Entity Framework ve srovnání s tím, že EDMX. Náklady jsou zcela závisí na velikosti a složitosti modelu, který má být sestaven.

Pokud se rozhodnete použít EDMX a Code First, je důležité vědět, že pružnosti ji Code First zvyšuje náklady na sestavení modelu poprvé. Pokud vaše aplikace dokázal náklady na toto první zatížení obvykle Code First budou preferovaný způsob, jak přejít.

## <a name="10-investigating-performance"></a>10 zkoumání výkonu

### <a name="101-using-the-visual-studio-profiler"></a>10.1 pomocí Profiler sady Visual Studio

Pokud máte problémy s výkonem s použitím rozhraní Entity Framework, můžete zobrazit, kde aplikace spotřebuje své doby profiler stejný, jako je integrované do sady Visual Studio. Toto je nástroj, který jsme použili k vygenerování výsečové grafy v blogovém příspěvku "Zkoumání výkonu technologie ADO.NET Entity Framework – část 1" ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) , které uvádí, kde Entity Framework stráví času během studené a horké dotazy.

Blogový příspěvek "Profilace Entity Framework pomocí Visual Studio 2010 Profiler" napsal Data a modelování zákaznického poradního týmu ukazuje příklad reálného světa jak používají profiler k prozkoumat problémy s výkonem.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Tento příspěvek napsaný pro aplikace systému windows. Pokud chcete profilovat webové aplikace mohou nástroje požadavku webové Windows Performance Recorder (části) a Windows Performance Analyzer (WPA) fungují lépe než pracovní ze sady Visual Studio. Požadavku webové části a WPA jsou součástí Windows Performance Toolkit, který je součástí sady Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 profilace aplikace a databáze

Nástroje, jako je integrované do sady Visual Studio profiler zjistit, kde aplikace spotřebuje čas.  Je k dispozici jiný typ profileru, který provádí dynamické analýze aplikace běžící v produkčním prostředí nebo předprodukčním prostředí, v závislosti na požadavcích a hledá běžné nástrahy a antimodely přístup k databázi.

Jsou dva komerčně dostupný profilery Profiler Entity Framework ( \<http://efprof.com>) a ORMProfiler ( \<http://ormprofiler.com>).

Pokud vaše aplikace je aplikace MVC pomocí Code First, můžete použít MiniProfiler od StackExchange. Scott Hanselman popisuje tento nástroj na svém blogu na: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Další informace o aktivitě databáze dané aplikace, najdete v článku MSDN Magazine Julie Lerman s názvem profilace [profilace aktivity databáze v Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 protokolovací nástroj databáze

Pokud používáte Entity Framework 6 také zvážit použití vestavěné protokolování. Protokolování jeho činnosti prostřednictvím jednoduché konfigurace jednořádkové může být nastavena vlastnost databáze kontextu:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

V tomto příkladu se budou protokolovat činnosti databáze do konzoly, ale vlastnost Log lze nastavit volat žádnou akci&lt;řetězec&gt; delegovat.

Pokud chcete povolit protokolování do databáze bez opětovné kompilace a vy používáte Entity Framework 6.1 nebo novější, můžete tak učinit tak, že přidáte zachycování v souboru web.config nebo app.config aplikace.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Další informace o tom, jak přidat protokolování bez opětovné kompilace přejít na \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>Dodatek 11

### <a name="111-a-test-environment"></a>11.1 A. testovacího prostředí

Toto prostředí používá počítač 2. nastavení se databáze na samostatném počítači z klientské aplikace. Počítače jsou ve stejné stojanu, tak je latence sítě, relativně nízký, ale než jeden počítač prostředí víc odpovídají realitě.

#### <a name="1111-app-server"></a>11.1.1 aplikačního serveru

##### <a name="11111-software-environment"></a>11.1.1.1 softwarovém prostředí

-   Entity Framework 4 softwarovém prostředí
    -   Název operačního systému: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (pouze pro několik porovnání).
-   Entity Framework 5 a 6 softwarovém prostředí
    -   Název operačního systému: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112-hardware-environment"></a>11.1.1.2 hardwarového prostředí

-   Dvoujádrový procesor: Intel(R) Xeon(R) CPU L5520 W3530 @ 2,27 GHz,. 2261 Mhz8 GHz, 4 jader, 84 logických procesorů.
-   RamRAM 2412 GB.
-   136 GB SCSI250GB SATA 7200 ot. / min / 3GB/s disku rozdělit do 4 oddíly.

#### <a name="1112-db-server"></a>11.1.2 Databázového serveru

##### <a name="11121-software-environment"></a>11.1.2.1 softwarovém prostředí

-   Název operačního systému: Windows Server 2008 R28.1 Enterprise s aktualizací SP1.
-   SQL Server 2008 R22012.

##### <a name="11122-hardware-environment"></a>11.1.2.2 hardwarového prostředí

-   Jeden procesor: Intel(R) Xeon(R) CPU L5520 @ 2,27 GHz,. 2261 MhzES-1620 0 @ 3.60 GHz, 4 jader, 8 logických procesorů.
-   RamRAM 824 GB.
-   465 GB ATA500GB SATA 7200 ot. / min 6GB/s disku rozdělit do 4 oddíly.

### <a name="112-b-query-performance-comparison-tests"></a>11.2 testy porovnání výkonu dotazů B.

Northwind model byl použit ke spuštění těchto testů. Byla vygenerována z databáze pomocí Entity Framework designer. Následující kód se použije k porovnání výkonu provádění dotazu:

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

### <a name="113-c-navision-model"></a>Model Navision 11,3 C.

Navision databáze je velké databáze, použít pro ukázkové aplikace Microsoft Dynamics – NAV. Vygenerovaný konceptuální model obsahuje sady 1005 entit a sad 4227 přidružení. Model použitý v testu je "ploché" – žádné dědičnosti se přidala do něj.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 dotazy použité pro Navision testy

Seznam dotazů použít s modelem Navision obsahuje 3 kategorie jazyka Entity SQL:

##### <a name="11311-lookup"></a>11.3.1.1 vyhledávání

Jednoduché vyhledávací dotaz s žádné agregace

-   Počet: 16232
-   Příklad:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

Normální BI dotaz s více agregací, ale žádné mezisoučty (jeden dotaz)

-   Počet: 2313
-   Příklad:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Kde MDF\_SessionLogin\_čas\_Max() je definován v modelu jako:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Dotaz BI s agregace a souhrny (prostřednictvím sjednocení všechny)

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
