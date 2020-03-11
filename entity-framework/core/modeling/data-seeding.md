---
title: Osazení dat – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 5c056c600f696ad1443ddb7b8c95c4b0ead06d21
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417224"
---
# <a name="data-seeding"></a>Předvyplnění dat

Osazení dat je proces naplnění databáze s počáteční sadou dat.

Existuje několik způsobů, jak se dá dosáhnout v EF Core:

* Data počátečního modelu
* Ruční přizpůsobení migrace
* Vlastní inicializační logika

## <a name="model-seed-data"></a>Data počátečního modelu

> [!NOTE]
> Tato funkce je v EF Core 2,1 novinkou.

Na rozdíl od EF6, v EF Core lze data osazení přidružit k typu entity jako součást konfigurace modelu. Pak EF Core [migrace](xref:core/managing-schemas/migrations/index) může automaticky vypočítat, které operace vložení, aktualizace nebo odstranění se musí použít při upgradu databáze na novou verzi modelu.

> [!NOTE]
> Migrace zohledňuje pouze změny modelu při určování, jakou operaci je nutné provést k získání počátečních dat do požadovaného stavu. Proto se může stát, že dojde ke ztrátě všech změn dat provedených mimo migrace nebo dojde k chybě.

V takovém případě se v `OnModelCreating`nakonfigurují počáteční data pro `Blog`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Chcete-li přidat entity, které mají relaci, je nutné zadat hodnoty cizího klíče:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Pokud má typ entity ve stavu stín nějaké vlastnosti, můžete k poskytnutí těchto hodnot použít anonymní třídu:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Vlastní typy entit lze dosazením podobným způsobem:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Podívejte se na [úplný ukázkový projekt](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) pro další kontext.

Po přidání dat do modelu by se měly k použití změn použít [migrace](xref:core/managing-schemas/migrations/index) .

> [!TIP]
> Pokud v rámci automatizovaného nasazení potřebujete použít migrace, můžete [vytvořit skript SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , který může být před provedením zobrazený.

Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční data, například pro testovací databázi nebo při použití poskytovatele v paměti nebo jakékoli nerelační databáze. Všimněte si, že pokud databáze již existuje, `EnsureCreated()` nebude aktualizovat ani počáteční data v databázi. V případě relačních databází byste neměli volat `EnsureCreated()`, pokud plánujete používat migrace.

### <a name="limitations-of-model-seed-data"></a>Omezení dat osazení modelu

Tento typ počátečních dat je spravovaný migracemi a skript, který aktualizuje data, která jsou už v databázi, se musí vygenerovat bez připojení k databázi. Tato omezení představují některá omezení:

* Hodnota primárního klíče je nutné zadat i v případě, že je obvykle vygenerována databází. Použije se k detekci změn dat mezi migracemi.
* Dřív dosazený údaj se odebere, pokud se primární klíč změní jakýmkoli způsobem.

Proto je tato funkce nejužitečnější pro statická data, u kterých se neočekává Změna mimo migrace a která nezávisí na žádné jiné v databázi, například PSČ.

Pokud váš scénář obsahuje některý z následujících postupů, doporučuje se použít vlastní inicializační logiku popsanou v poslední části:

* Dočasná data pro testování
* Data, která závisí na stavu databáze
* Data, která vyžadují generování klíčových hodnot databáze, včetně entit, které jako identitu používají alternativní klíče
* Data, která vyžadují vlastní transformaci (která není zpracována [převody hodnot](xref:core/modeling/value-conversions)), jako například některé hodnoty hash hesla
* Data, která vyžadují volání do externího rozhraní API, například ASP.NET Core rolí identity a vytváření uživatelů

## <a name="manual-migration-customization"></a>Ruční přizpůsobení migrace

Při přidání migrace se změny dat zadaných pomocí `HasData` transformují na volání `InsertData()`, `UpdateData()`a `DeleteData()`. Jedním ze způsobů, jak obejít některá omezení `HasData`, je místo toho ručně přidat tato volání nebo [vlastní operace](xref:core/managing-schemas/migrations/operations) do migrace.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Vlastní inicializační logika

Jednoduchý a účinný způsob, jak provádět osazení dat, je použít [`DbContext.SaveChanges()`](xref:core/saving/index) před zahájením provádění hlavní aplikační logiky.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Kód pro osazení by neměl být součástí normálního spuštění aplikace, protože to může způsobovat problémy s souběžnou synchronizací, když je spuštěno více instancí, a také vyžaduje, aby aplikace měla oprávnění upravovat schéma databáze.

V závislosti na omezeních nasazení může být inicializační kód proveden různými způsoby:

* Místní spuštění inicializační aplikace
* Nasazení inicializační aplikace pomocí hlavní aplikace, vyvolání inicializační rutiny a zakázání nebo odebrání inicializační aplikace.

To se obvykle dá automatizovat pomocí [profilů publikování](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
