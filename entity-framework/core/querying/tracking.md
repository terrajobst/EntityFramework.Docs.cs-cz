---
title: Sledování vs. Dotazy bez sledování – základní ef
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417647"
---
# <a name="tracking-vs-no-tracking-queries"></a>Sledování vs. dotazy bez sledování

Sledování chování řídí, pokud core entity framework uchová informace o instanci entity v jeho sledování změn. Pokud je entita sledována, budou všechny změny zjištěné `SaveChanges()`v entitě v databázi zachovány během . EF Core také opraví navigační vlastnosti mezi entitami ve výsledku sledování dotazu a entitami, které jsou v sledování změn.

> [!NOTE]
> [Bezklíčové typy entit](xref:core/modeling/keyless-entity-types) nejsou nikdy sledovány. Všude tam, kde tento článek zmiňuje typy entit, odkazuje na typy entit, které mají definovaný klíč.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.

## <a name="tracking-queries"></a>Sledování dotazů

Ve výchozím nastavení jsou dotazy, které vracejí typy entit, sledování. To znamená, že můžete provádět změny v těchto instancích entit a tyto změny zachovat . `SaveChanges()` V následujícím příkladu bude zjištěna změna hodnocení blogů a bude `SaveChanges()`zachována v databázi během .

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Dotazy bez sledování

Žádné sledování dotazy jsou užitečné při výsledky se používají ve scénáři jen pro čtení. Jejich spuštění je rychlejší, protože není nutné nastavovat informace o sledování změn. Pokud nepotřebujete aktualizovat entity načtené z databáze, měl by být použit dotaz bez sledování. Můžete vyměnit jednotlivé dotazy, které mají být bez sledování.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Můžete také změnit výchozí chování sledování na úrovni instance kontextu:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Rozlišení identity

Vzhledem k tomu, že sledovací dotaz používá sledování změn, EF Core provede rozlišení identity v dotazu sledování. Při materializaci entity EF Core vrátí stejnou instanci entity z nástroje pro sledování změn, pokud je již sledována. Pokud výsledek obsahuje stejnou entitu vícekrát, získáte zpět stejnou instanci pro každý výskyt. Žádné sledování dotazů nepoužívá sledování změn a nedělají rozlišení identity. Takže získáte zpět novou instanci entity i v případě, že je stejná entita obsažena ve výsledku vícekrát. Toto chování se lišilo ve verzích před EF Core 3.0, viz [předchozí verze](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Sledování a vlastní projekce

I v případě, že typ výsledku dotazu není typ entity, EF Core bude stále sledovat typy entit obsažené ve výsledku ve výchozím nastavení. V následujícím dotazu, který vrací anonymní typ, budou sledovány instance `Blog` v sadě výsledků.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Pokud sada výsledků obsahuje typy entit vycházející z kompozice LINQ, EF Core je bude sledovat.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Pokud sada výsledků neobsahuje žádné typy entit, neprovádí se žádné sledování. V následujícím dotazu vrátíme anonymní typ s některými hodnotami z entity (ale žádné instance skutečného typu entity). Z dotazu nejsou žádné sledované entity.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core podporuje hodnocení klienta v projekci nejvyšší úrovně. Pokud EF Core zhmotní instanci entity pro vyhodnocení klienta, bude sledována. Tady, protože jsme `blog` předávání entit do `StandardizeURL`klientské metody , EF Core bude sledovat blog instance příliš.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core nesleduje bezklíčové instance entit obsažené ve výsledku. Ale EF Core sleduje všechny ostatní instance typů entit s klíčem podle výše uvedených pravidel.

Některá z výše uvedených pravidel fungovala před EF Core 3.0 odlišně. Další informace naleznete v [předchozích verzích](#previous-versions).

## <a name="previous-versions"></a>Předchozí verze

Před verzí 3.0 ef core měl některé rozdíly v tom, jak bylo měření provedeno. Významné rozdíly jsou následující:

- Jak je vysvětleno na stránce [Vyhodnocení klienta vs serveru,](xref:core/querying/client-eval) EF Core podporoval ohodnocení klienta v libovolné části dotazu před verzí 3.0. Vyhodnocení klienta způsobilo zhmotnění entit, které nebyly součástí výsledku. Takže EF Core analyzoval výsledek, aby zjistil, co sledovat. Tento návrh měl určité rozdíly takto:
  - Vyhodnocení klienta v projekci, které způsobilo materializaci, ale nevrátilo zhmotněnou entitu, nebylo sledováno. Následující příklad nesledoval `blog` entity.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core nesledoval objekty přicházející z linq složení v určitých případech. Následující příklad nesledoval `Post`.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Kdykoli výsledky dotazu obsahovaly typy bezklíčových entit, celý dotaz byl proveden bez sledování. To znamená, že typy entit s klíči, které jsou výsledkem nebyly sledovány jeden.
- EF Core provedl rozlišení identity v dotazu bez sledování. Používá slabé odkazy ke sledování entit, které již byly vráceny. Takže pokud sada výsledků obsahovala násobek stejné entity, získali byste stejnou instanci pro každý výskyt. Ačkoli pokud předchozí výsledek se stejnou identitou přešel mimo rozsah a získal uvolňování paměti, EF Core vrátil novou instanci.
