---
title: Migrace Code First s existující databází – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: eb7948eafb1322cabcf69b47bd5411f762fe8498
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182586"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migrace Code First s existující databází
> [!NOTE]
> **EF 4.3 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 4,1. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Tento článek popisuje použití Migrace Code First se stávající databází, kterou vytvořila aplikace Entity Framework.

> [!NOTE]
> V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích. Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="screencasts"></a>Screencasty

Pokud místo toho chcete sledovat záznam dění na záznamovém počítači, než je tento článek přečetl, následující dvě videa se týkají stejného obsahu jako tento článek.

### <a name="video-one-migrations---under-the-hood"></a>Video One: "migrace – pod digestoř"

[Tento záznam dění](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) popisuje, jak migrace sleduje a používá informace o modelu pro detekci změn modelu.

### <a name="video-two-migrations---existing-databases"></a>Video dvě: Migrace – existující databáze

[Tento záznam dění](https://channel9.msdn.com/blogs/ef/migrations-existing-databases) na základě konceptů z předchozího videa se zabývá tím, jak povolit a používat migrace pomocí existující databáze.

## <a name="step-1-create-a-model"></a>Krok 1: vytvoření modelu

Prvním krokem je vytvoření modelu Code First, který cílí na stávající databázi. Podrobné pokyny k tomu, jak to provést, najdete v tématu [Code First do existující databáze](~/ef6/modeling/code-first/workflows/existing-database.md) .

>[!NOTE]
> Před provedením změn v modelu, které by vyžadovaly změny ve schématu databáze, je důležité postupovat podle zbývajících kroků v tomto tématu. Následující kroky vyžadují, aby byl model synchronizovaný se schématem databáze.

## <a name="step-2-enable-migrations"></a>Krok 2: povolení migrací

Dalším krokem je povolení migrací. To můžete provést spuštěním příkazu **Povolit – migrace** v konzole správce balíčků.

Tento příkaz vytvoří ve vašem řešení složku s názvem migrace a v rámci ní vloží jednu třídu s názvem konfigurace. Konfigurační třída je místo, kde můžete nakonfigurovat migrace pro vaši aplikaci. Další informace najdete v tématu [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="step-3-add-an-initial-migration"></a>Krok 3: Přidání prvotní migrace

Po vytvoření a použití migrace do místní databáze můžete také tyto změny použít v jiných databázích. Například vaše místní databáze může být testovací databáze a nakonec můžete chtít použít změny také v provozní databázi a v jiných vývojářích testovacích databází. Pro tento krok jsou k dispozici dvě možnosti a ten, který byste měli vybrat, závisí na tom, jestli je schéma všech ostatních databází prázdné nebo aktuálně odpovídá schématu místní databáze.

-   **Možnost One: jako výchozí bod použijte existující schéma.** Tento postup byste měli použít, pokud budou další databáze, na které se migrace budou v budoucnu použity, mít stejné schéma jako vaše místní databáze v současné době. Můžete to například použít, pokud místní testovací databáze aktuálně odpovídá v1 vaší provozní databáze a později použijete tyto migrace k aktualizaci provozní databáze na v2.
-   **Možnost 2: jako výchozí bod použijte prázdnou databázi.** Tento postup byste měli použít, pokud budou další databáze, na které se migrace použijí, v budoucnu prázdné (nebo ještě neexistují). To můžete použít například v případě, že jste začali vyvíjet aplikaci pomocí testovací databáze, ale bez použití migrace a později budete chtít vytvořit provozní databázi od začátku.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Možnost One: jako výchozí bod použijte existující schéma.

Migrace Code First používá snímek modelu, který je uložený v nejnovější migraci, aby zjistil změny modelu (podrobné informace najdete v [migrace Code First v týmových prostředích](~/ef6/modeling/code-first/migrations/teams.md)). Vzhledem k tomu, že předpokládáme, že databáze již mají schéma aktuálního modelu, vytvoříme prázdnou migraci (No-OP), která má aktuální model jako snímek.

1.  Spusťte příkaz **Add-Migration InitialCreate-IgnoreChanges** v konzole správce balíčků. Tím se vytvoří prázdná migrace s aktuálním modelem jako snímek.
2.  Spusťte příkaz **Update-Database** v konzole správce balíčků. Tím se použije migrace InitialCreate na databázi. Vzhledem k tomu, že skutečná migrace neobsahuje žádné změny, jednoduše přidá řádek do tabulky \_\_MigrationsHistory, která indikuje, že tato migrace již byla použita.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Možnost 2: jako výchozí bod použijte prázdnou databázi

V tomto scénáři potřebujeme migrace, aby bylo možné vytvořit celou databázi úplně od začátku – včetně tabulek, které jsou již obsaženy v naší místní databázi. Chystáme se vygenerovat migraci InitialCreate, která obsahuje logiku pro vytvoření existujícího schématu. Naše existující databáze pak bude vypadat jako tato migrace již byla použita.

1.  Spusťte příkaz **Add-Migration InitialCreate** v konzole správce balíčků. Tím se vytvoří migrace pro vytvoření existujícího schématu.
2.  Odkomentujte veškerý kód v metodě up nově vytvořené migrace. To nám umožní použít migraci na místní databázi bez nutnosti znovu vytvořit všechny tabulky atd., které už existují.
3.  Spusťte příkaz **Update-Database** v konzole správce balíčků. Tím se použije migrace InitialCreate na databázi. Vzhledem k tomu, že skutečná migrace neobsahuje žádné změny (protože jsme je dočasně odhlásili), jednoduše přidá řádek do tabulky \_\_MigrationsHistory, která indikuje, že tato migrace již byla použita.
4.  Zruší komentář kódu v metodě up. To znamená, že když se tato migrace použije v budoucích databázích, vytvoří se v rámci migrace schéma, které už existovalo v místní databázi.

## <a name="things-to-be-aware-of"></a>Co je potřeba vědět o

K dispozici je několik věcí, které je potřeba vzít v úvahu při použití migrace na stávající databázi.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Výchozí/počítaný název nemusí odpovídat existujícímu schématu.

Migrace explicitně specifikuje názvy pro sloupce a tabulky, když dojde k jejich vygenerování. Existují však i další databázové objekty, které migrace při použití migrace vypočítá výchozí název. To zahrnuje indexy a omezení cizího klíče. Při cílení na stávající schéma nemusí tyto počítané názvy odpovídat, co ve vaší databázi skutečně existuje.

Tady je několik příkladů, kdy je potřeba mít na paměti tyto informace:

**Pokud jste použili možnost ' jedna: použijte existující schéma jako výchozí bod ' z kroku 3:**

-   Pokud budoucí změny v modelu vyžadují změnu nebo odstranění jednoho z databázových objektů, které jsou pojmenovány jinak, bude nutné změnit vygenerované migrace do systému zadáním správného názvu. Rozhraní API migrace mají volitelný parametr názvu, který vám to umožňuje.
    Například vaše existující schéma může mít tabulku post se sloupcem BlogId (cizí klíč), která má index s názvem IndexFk\_BlogId. Ve výchozím nastavení ale migrace by očekávala, že tento index má mít název IX\_BlogId. Pokud provedete změnu modelu, který má za následek vyřazení tohoto indexu, budete muset změnit vygenerované DropIndex volání a zadat název IndexFk\_BlogId.

**Pokud jste použili možnost 2: použít prázdnou databázi jako výchozí bod z kroku 3:**

-   Pokus o spuštění metody snížení počáteční migrace (tj. návrat k prázdné databázi) proti místní databázi může selhat, protože migrace se pokusí vyřadit indexy a omezení cizího klíče pomocí nesprávných názvů. To bude mít vliv jenom na vaši místní databázi, protože jiné databáze se vytvoří od začátku pomocí metody počáteční migrace.
    Chcete-li downgradovat stávající místní databázi do prázdného stavu, je nejjednodušší to provést ručně, a to tak, že databázi vyřadíte nebo vyřadíte všechny tabulky. Po počátečním downgradu se všechny objekty databáze znovu vytvoří s výchozími názvy, takže se tento problém neprojeví znovu.
-   Pokud budoucí změny v modelu vyžadují změnu nebo odstranění jednoho z databázových objektů, které jsou pojmenovány jinak, nebude tato akce fungovat na stávající místní databázi – protože názvy nebudou odpovídat výchozím hodnotám. Bude ale fungovat s databázemi vytvořenými od začátku, protože budou používat výchozí názvy zvolené migracemi.
    Tyto změny můžete provést buď ručně v místní existující databázi, nebo můžete zvážit, že se databáze znovu vytvoří od začátku – stejně jako na jiných počítačích.
-   Databáze vytvořené pomocí metody up počáteční migrace se můžou mírně lišit od místní databáze, protože se použijí počítané výchozí názvy pro indexy a omezení cizího klíče. Kromě toho může dokončit i další indexy, protože migrace vytvoří ve výchozím nastavení indexy pro sloupce cizího klíče – to ale nemusí být v původní místní databázi.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>V modelu nejsou reprezentovány všechny objekty databáze.

Objekty databáze, které nejsou součástí vašeho modelu, nebudou zajišťovat migrace. To může zahrnovat zobrazení, uložené procedury, oprávnění, tabulky, které nejsou součástí modelu, další indexy atd.

Tady je několik příkladů, kdy je potřeba mít na paměti tyto informace:

-   Bez ohledu na možnost, kterou jste zvolili v části "krok 3", pokud budoucí změny v modelu vyžadují změnu nebo odstranění těchto dalších objektů, nebudete při provádění těchto změn potřebovat migrace. Pokud například vyřadíte sloupec, který obsahuje další index, migrace nevědí, že se index vyřadí. Tento postup bude nutné přidat do vygenerované migrace ručně.
-   Pokud jste použili možnost 2: jako výchozí bod použít prázdnou databázi, tyto další objekty nebudou vytvořeny metodou up počáteční migrace.
    V případě potřeby můžete upravit metody nahoru a dolů a pořídit tyto další objekty. Pro objekty, které nejsou nativně podporované v rozhraní API pro migraci, jako jsou například zobrazení – můžete použít metodu [SQL](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) ke spuštění nezpracovaných SQL pro jejich vytvoření nebo přesunutí.
