---
title: Migrace Code First s existující databází - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: 06aabf3f57ca451f4d9cba469f6de40fd9aa8f23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998194"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migrace Code First s existující databáze
> [!NOTE]
> **EF4.3 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 4.1. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Tento článek se týká pomocí migrace Code First s existující databázi, který nebyl vytvořen poskytovatelem Entity Framework.

> [!NOTE]
> Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře. Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.

## <a name="screencasts"></a>Screencasty

Pokud byste raději přehrát záznam dění na monitoru než v tomto článku, následující dvě videa zahrnují obsah jako v tomto článku.

### <a name="video-one-migrations---under-the-hood"></a>Video jeden: "Migrace - pod pokličkou"

[Tento záznam dění na monitoru](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) vysvětluje, jak migrace sleduje a používá informace o modelu ke zjištění změny modelu.

### <a name="video-two-migrations---existing-databases"></a>Dvě videa: "Migrace – existující databáze"

Stavíme na koncepty z předchozí video [tento záznam dění na monitoru](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) dozvíte, jak povolit a používat migrace s existující databázi.

## <a name="step-1-create-a-model"></a>Krok 1: Vytvoření modelu

Prvním krokem bude vytvoření modelu Code First, zaměřuje na existující databázi. [Code First pro existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md) téma obsahuje podrobné informace o tom, jak to provést.

>[!NOTE]
> Je důležité, abyste před provedením jakýchkoli změn do modelu, které by vyžadovaly změny schématu databáze postupujte podle zbývajících kroků v tomto tématu. Následující kroky vyžadují modelu, který má být synchronizovaná se schématem databáze.

## <a name="step-2-enable-migrations"></a>Krok 2: Povolení migrace

Dalším krokem je povolení migrace. Můžete to provést spuštěním **povolení migrace** příkazu v konzole Správce balíčků.

Tento příkaz se vytvořte složku ve vašem řešení, volá se, migrace a vložit jednu třídu dovnitř označuje konfigurace. Třídu konfigurace je, kde konfigurujete migrace pro vaši aplikaci, můžete zjistit další informace se dozvíte v [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) tématu.

## <a name="step-3-add-an-initial-migration"></a>Krok 3: Přidání počáteční migraci

Po migrace byly vytvořeny a použity k místní databázi, můžete také použít změní na ostatní databáze. Například vaše místní databáze může být testovací databázi a nakonec můžete také změny se projeví na provozní databázi a/nebo jinými vývojáři testovací databáze. Existují dvě možnosti pro tento krok a ten, který by měl vybrat závisí, zda schéma případné další databáze, je prázdný nebo aktuálně odpovídá schématu místní databáze.

-   **Jedna možnost: Použijte stávající schéma jako výchozí bod.** Tento postup byste měli použít při další databáze, které migrace se použijí pro v budoucnu budou mít stejné schéma jako aktuálně má vaše místní databáze. To můžete použít například, pokud vaše místní testovací databáze aktuálně odpovídá v1 vaši produkční databázi a se později použijí tyto migrace aktualizovat vaši produkční databázi na v2.
-   **Možnost 2: Použití prázdnou databázi jako počáteční bod.** Tento postup byste měli použít, když ostatní databáze, které migrace se použijí k budoucímu jsou prázdné (nebo ještě neexistuje). To můžete použít například, pokud začít vyvíjet svoji aplikaci pomocí testovací databázi, ale bez použití migrace a budete později chtít vytvořit produkční databáze od začátku.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Jedna možnost: Používejte stávající schéma jako počáteční bod

Migrace Code First pomocí snímku model uložených v nejnovější migrace zjišťuje změny modelu (zjistíte podrobné informace najdete v [migrace Code First v prostředích Team](~/ef6/modeling/code-first/migrations/teams.md)). Vzhledem k tomu, že budeme předpokládat, že databáze již mít schéma aktuálního modelu, vygenerujeme migrace (no-op) do prázdné, která má aktuální model jako snímek.

1.  Spustit **přidat migrace InitialCreate – IgnoreChanges** příkazu v konzole Správce balíčků. Tím se vytvoří prázdný migrace současného modelu jako snímek.
2.  Spustit **aktualizace databáze** příkazu v konzole Správce balíčků. Tato operace provede určité InitialCreate migrace do databáze. Protože skutečné migrace neobsahuje žádné změny, bude jednoduše přidat řádek \_ \_MigrationsHistory tabulky určující, že se tato migrace už používá.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Možnost 2: Jako výchozí bod použijte prázdnou databázi

V tomto scénáři musíte migrace budete moci vytvořit celé databáze od začátku – včetně tabulek, které jsou již přítomny v místní databázi. My budeme generovat migrace do InitialCreate, který obsahuje logiku pro vytvoření existující schématu. Potom uděláme naší stávající databázi vypadají této migrace se už používá.

1.  Spustit **přidat migrace InitialCreate** příkazu v konzole Správce balíčků. Tím se vytvoří migrace k vytvoření existující schématu.
2.  Okomentujte veškerý kód v metodě nahoru nově vytvořený migrace. To vám umožní nám "" migrace do místní databáze bez pokusu o znovu vytvořit všechny tabulky atd., které již existují.
3.  Spustit **aktualizace databáze** příkazu v konzole Správce balíčků. Tato operace provede určité InitialCreate migrace do databáze. Od skutečné migrace neobsahuje změny (protože jsme dočasně opatřený komentáři je), se jednoduše přidejte řádek \_ \_MigrationsHistory tabulky určující, že se tato migrace už používá.
4.  Zrušit komentář kód v metodě nahoru. To znamená, že při použití této migrace u budoucích databází, schéma, které již existoval v místní databázi vytvořené pomocí migrace.

## <a name="things-to-be-aware-of"></a>Co je potřeba mít na paměti

Existuje několik věcí, které je potřeba mít na paměti při použití migrace na existující databázi.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Vypočítá/výchozí názvy nemusí odpovídat existujícím schématu

Migrace explicitně určuje názvy sloupců a tabulek, když ji vygeneruje uživatelské rozhraní migrace. Existují však ostatních databázových objektů, které migrace vypočítává výchozí název, při použití migrace. To zahrnuje indexy a omezení cizího klíče. Při cílení na stávajícím schématu, tyto počítané názvy nemusí odpovídat, co skutečně existuje v databázi.

Tady je několik příkladů, když je třeba tomu věnovat pozornost:

**Pokud jste použili "jedna možnost: jako výchozí bod použijte existující schéma" z kroku 3:**

-   Pokud budoucí změny v modelu nevyžaduje změnu nebo jeden z databázových objektů, které je pojmenované jinak vyřadit, musíte změnit automaticky generovaný migraci zadejte správný název. Migrace rozhraní API obsahovat nepovinný parametr název, který umožňuje udělat.
    Například existující schéma může obsahovat tabulku příspěvek se BlogId sloupec cizího klíče, který má index s názvem IndexFk\_BlogId. Ale ve výchozím nastavení migrace by uživatel očekával tento index s názvem IX\_BlogId. Pokud změníte model, který má za následek odstranění tohoto indexu, musíte změnit automaticky generovaný DropIndex volání k určení IndexFk\_BlogId název.

**Pokud jste použili "možnost 2: použití prázdnou databázi jako výchozí bod" z kroku 3:**

-   Chcete spustit metodu nižší počáteční migraci (který se vrátí k prázdnou databázi) na místní databázi může selhat, protože se pokusí migrace vyřaďte indexy a omezení cizího klíče pomocí správné názvy. Jen to bude mít vliv na vaše místní databáze od jiných databází se vytvoří úplně od začátku pomocí metody nahoru počáteční migraci.
    Pokud chcete přejít na nižší verzi vaší stávající místní databázi pro prázdný stav je nejjednodušší provést ručně, buď ve vyřazení databáze nebo vyřadit všechny tabulky. Po tomto počáteční přechod na starší verzi, které všechny objekty databáze se znovu vytvoří s výchozími názvy proto tento problém nebude k dispozici samotné znovu.
-   Pokud budoucí změny v modelu nevyžaduje změnu nebo jeden z databázových objektů, které je pojmenované jinak vyřadit, to nebude fungovat na vaší stávající místní databázi – protože názvy nebudou odpovídat výchozí hodnoty. Však bude fungovat s databázemi, které byly vytvořeny 'od začátku' vzhledem k tomu, že se mají používat výchozí názvy zvolí migrace.
    Může tyto změny provést ručně ve vaší stávající místní databázi, nebo zvažte, jestli by migrace znovu vytvořit databázi úplně od začátku – stejně jako ji na jiné počítače.
-   Vytvořené metodou nahoru počáteční migraci databáze se mohou poněkud lišit z místní databáze od počítaných výchozí názvy pro indexy a použije omezení cizího klíče. Můžete také skončit s další indexy jako migrace ve výchozím nastavení vytvoří indexy na sloupce cizích klíčů – to nemusí být případ v místní databázi.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Ne všechny databázové objekty jsou reprezentovány v modelu

Databázové objekty, které nejsou součástí modelu nesmí být zpracovány migrace. To může zahrnovat zobrazení, uložené procedury, oprávnění, tabulky, které nejsou součástí modelu, další indexy, atd.

Tady je několik příkladů, když je třeba tomu věnovat pozornost:

-   Bez ohledu na to, možnost jste vybrali v kroku 3 Pokud budoucí změny v modelu nevyžaduje změnu nebo vyřadit tyto další objekty migrace nebude vědět o provedení těchto změn. Například pokud je sloupec, který obsahuje další index ho vyřadit, migrace nebude vědět o drop index. Je potřeba ručně přidat vygenerované migrace.
-   Pokud jste použili "možnost 2: použijte prázdnou databázi jako výchozí bod", tyto další objekty se nevytvoří metodou nahoru počáteční migraci.
    Můžete upravit nahoru a dolů metody postará se o tyto další objekty, pokud si přejete. Pro objekty, které nativně nepodporují v rozhraní API migrace – například zobrazení – můžete použít [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) způsob spuštění nezpracovaná SQL k vytvoření/drop je.
