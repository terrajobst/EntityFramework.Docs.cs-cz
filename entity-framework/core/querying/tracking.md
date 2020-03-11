---
title: Sledování vs. žádné dotazy sledování – EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417647"
---
# <a name="tracking-vs-no-tracking-queries"></a>Sledování vs. žádné dotazy sledování

Sledování chování řídí, pokud Entity Framework Core uchová informace o instanci entity ve sledování změn. Pokud je entita sledována, všechny změny zjištěné v entitě budou během `SaveChanges()`trvalé v databázi. EF Core budou také opravovat navigační vlastnosti mezi entitami ve výsledku sledovacího dotazu a entitami, které jsou v sledování změn.

> [!NOTE]
> [Typy entit bez klíčů](xref:core/modeling/keyless-entity-types) nejsou nikdy sledovány. Bez ohledu na to, kde tento článek uvádí typy entit, odkazuje na typy entit, které mají definován klíč.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.

## <a name="tracking-queries"></a>Sledování dotazů

Ve výchozím nastavení jsou sledovány dotazy, které vracejí typy entit. To znamená, že můžete provádět změny těchto instancí entit a nechat tyto změny trvale `SaveChanges()`. V následujícím příkladu bude v průběhu `SaveChanges()`zjištěna změna hodnocení blogů a jejich uchování do databáze.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Žádné dotazy pro sledování

Žádné sledovací dotazy nejsou užitečné, pokud jsou výsledky použity ve scénáři jen pro čtení. Spouští se rychleji, protože není potřeba nastavovat informace o sledování změn. Pokud nepotřebujete aktualizovat entity načtené z databáze, měli byste použít dotaz bez sledování. Jednotlivé dotazy můžete přepínat na možnost bez sledování.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Můžete také změnit výchozí chování při sledování na úrovni instance kontextu:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Překlad identity

Vzhledem k tomu, že sledovací dotaz používá sledování změn, EF Core provede překlad identity v dotazu sledování. Pokud je vyhodnocování entitou, EF Core vrátí stejnou instanci entity ze sledování změn, pokud již je sledována. Pokud výsledek obsahuje stejnou entitu víckrát, vrátí se stejná instance pro každý výskyt. Žádné dotazy pro sledování nepoužívají sledování změn a nedělají rozlišení identity. Takže se vrátí nová instance entity i v případě, že je stejná entita obsažena v výsledku víckrát. Toto chování se liší ve verzích před EF Core 3,0, viz [předchozí verze](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Sledování a vlastní projekce

I v případě, že typ výsledku dotazu není typ entity, EF Core budou stále sledovat typy entit obsažené ve výsledku ve výchozím nastavení. V následujícím dotazu, který vrací anonymní typ, budou sledovány instance `Blog` v sadě výsledků.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Pokud sada výsledků obsahuje typy entit, které pocházejí ze složení LINQ, EF Core je sledovat.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Pokud sada výsledků neobsahuje žádné typy entit, sledování se neprovádí. V následujícím dotazu vrátíme anonymní typ s některými hodnotami z entity (ale žádné instance skutečného typu entity). Z dotazu nejdou žádné sledované entity.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core podporuje testování klientů v projekci nejvyšší úrovně. Pokud EF Core materializuje instanci entity pro hodnocení klienta, bude sledována. Z toho důvodu, že předáváme `blog` entit `StandardizeURL`metodě klienta, EF Core bude sledovat i instance blogu.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core nesleduje instance entit bez klíčů obsažené ve výsledku. Ale EF Core sleduje všechny ostatní instance typů entit s klíčem podle pravidel uvedených výše.

Některá z výše uvedených pravidel fungovala jinak než EF Core 3,0. Další informace najdete v tématu [předchozí verze](#previous-versions).

## <a name="previous-versions"></a>Předchozí verze

Před verzí 3,0 byl EF Core několik rozdílů v tom, jak bylo sledování provedeno. Mezi významné rozdíly patří následující:

- Jak je vysvětleno na stránce [hodnocení klienta vs](xref:core/querying/client-eval) . EF Core podporované hodnocení klienta v jakékoli části dotazu před verzí 3,0. Vyhodnocení klienta způsobilo materializaci entit, které nebyly součástí výsledku. Proto EF Core analyzovat výsledek, aby bylo možné zjistit, co se má sledovat. Tento návrh má určité rozdíly, jak je znázorněno níže:
  - Vyhodnocení klienta v projekci, což způsobilo materializace, ale nevrátilo nevrácenou instanci materializované entity. Následující příklad nesledoval `blog` entit.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - V některých případech EF Core nesledovaly objekty, které jsou vycházející ze sestavení LINQ. Následující příklad nesledoval `Post`.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Vždy, když výsledky dotazu obsahují typy entit bez klíčů, byl celý dotaz proveden bez sledování. To znamená, že typy entit s klíči, které jsou ve výsledku, nebyly sledovány buď.
- EF Core se překlad identity v dotazu bez sledování. Používaly slabé odkazy, které udržují přehled o entitách, které již byly vráceny. Takže pokud sada výsledků dotazu obsahuje násobky vícekrát, získáte stejnou instanci pro každý výskyt. I když by předchozí výsledek se stejnou identitou byl mimo rozsah a byl uvolněný odpadkový systém, EF Core vrátil novou instanci.
