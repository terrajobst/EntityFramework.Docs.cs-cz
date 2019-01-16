---
title: Předvyplnění dat – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 1c450b142573368d043430f55a3144b6696a8691
ms.sourcegitcommit: b4a5ed177b86bf7f81602106dab6b4acc18dfc18
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/15/2019
ms.locfileid: "54316631"
---
# <a name="data-seeding"></a>Předvyplnění dat

Předvyplnění dat je proces sestavování databáze s počáteční sadu data.

To lze provést v EF Core několika způsoby:
* Počáteční hodnota data modelu
* Přizpůsobení ruční migraci
* Logika vlastní inicializaci.

## <a name="model-seed-data"></a>Počáteční hodnota data modelu

> [!NOTE]
> Tato funkce je nového v EF Core 2.1.

Na rozdíl od v EF6 do EF Core předvyplnění dat může být přidružený typ entity jako součást konfigurace modelu. Potom EF Core [migrace](xref:core/managing-schemas/migrations/index) dokáže automaticky výpočetní co vložit, aktualizovat nebo odstranit operací nutné použít při upgradu databáze na novou verzi modelu.

> [!NOTE]
> Při určování, jaké operace se provádí, aby dostat počáteční data do požadovaného stavu, migrace uvažuje pouze změny modelu. Všechny změny dat provést mimo migrace může tedy dojít ke ztrátě nebo může způsobit chybu.

Jako příklad, tím nakonfigurujete počáteční data `Blog` v `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Přidání entity, které mají hodnoty cizího klíče relace je potřeba zadat:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Typ entity má všechny vlastnosti v stínové stavu anonymní třída slouží k poskytování hodnot:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Vlastní entity, které typy lze nasadí podobným způsobem:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Zobrazit [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) pro širší kontext.

Po přidání dat do modelu, [migrace](xref:core/managing-schemas/migrations/index) by měla sloužit k použití změn.

> [!TIP]
> Pokud potřebujete provést migrace jako součást automatizované nasazení můžete [vytvořit skript SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , který lze zobrazit náhled před spuštěním.

Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční hodnoty dat, jako je například testovací databázi nebo při použití zprostředkovatele v paměti nebo jakékoli jiné relace databázi. Všimněte si, že pokud databáze již existuje, `EnsureCreated()` ani aktualizuje schéma ani počáteční hodnoty dat v databázi. U relačních databází byste neměli volat `EnsureCreated()` Pokud budete chtít použít migrace.

### <a name="limitations-of-model-seed-data"></a>Omezení modelování počáteční hodnoty dat

Tento typ dat počáteční hodnoty spravuje migrace a skript, který chcete aktualizovat data, která je již v databázi je potřeba vygenerovat bez připojení k databázi. To má určitá omezení:
* Hodnota primárního klíče musí být zadaná i v případě, že je obvykle generován databází. Se použije ke zjištění změny dat mezi migrace.
* Dříve dosazené odeberou data žádným způsobem při změně primárního klíče.

Tato funkce je proto zvláště užitečná pro statická data, která se neočekává změna mimo migrace a nezávisí na nic jiného v databázi, například PSČ.

Pokud váš scénář zahrnuje některé z následujících akcí se doporučuje použít logiku vlastní inicializace je popsáno v předchozí části:
* Dočasná data pro účely testování
* Data, která závisí na stav databáze
* Data, která vyžadují hodnoty klíče, který byl vygenerován nástrojem databáze, včetně entity, které používají alternativní klíče jako identita
* Data, která vyžaduje vlastní transformace (, která není zpracována [hodnota převody](xref:core/modeling/value-conversions)), jako je například některé hashování hesel
* Data, která vyžaduje volání externí rozhraní API, jako je například vytváření rolí a uživatelů v ASP.NET Core Identity

## <a name="manual-migration-customization"></a>Přizpůsobení ruční migraci

Při migraci přidání změny dat zadaný `HasData` se transformuje na volání `InsertData()`, `UpdateData()`, a `DeleteData()`. Jeden ze způsobů, jak obejít některá omezení `HasData` je ručně přidat tato volání nebo [vlastní operace](xref:core/managing-schemas/migrations/operations) migrace místo.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Logika vlastní inicializaci.

Snadný a efektivní způsob, jak provádět zálohy dat je použití [ `DbContext.SaveChanges()` ](xref:core/saving/index) než hlavní aplikace logiky zahájí vykonávání.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Osazení kód by neměl být součást provedení běžná aplikace, protože to může způsobit problémy se souběžností, pokud běží více instancí a bude také vyžadovat aplikace máte oprávnění k úpravě databázového schématu.

V závislosti na omezení nasazení inicializační kód může provést různými způsoby:
* Místně spuštěná aplikace inicializace
* Nasazení aplikace inicializace s hlavní aplikací, volání inicializační rutina a zakázání nebo odebrání inicializace aplikace.

To je obvykle možné automatizovat pomocí [publikační profily](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
