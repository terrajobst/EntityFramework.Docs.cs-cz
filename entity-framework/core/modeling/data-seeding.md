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
# <a name="data-seeding"></a><span data-ttu-id="33f8a-102">Předvyplnění dat</span><span class="sxs-lookup"><span data-stu-id="33f8a-102">Data Seeding</span></span>

<span data-ttu-id="33f8a-103">Osazení dat je proces naplnění databáze s počáteční sadou dat.</span><span class="sxs-lookup"><span data-stu-id="33f8a-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="33f8a-104">Existuje několik způsobů, jak se dá dosáhnout v EF Core:</span><span class="sxs-lookup"><span data-stu-id="33f8a-104">There are several ways this can be accomplished in EF Core:</span></span>

* <span data-ttu-id="33f8a-105">Data počátečního modelu</span><span class="sxs-lookup"><span data-stu-id="33f8a-105">Model seed data</span></span>
* <span data-ttu-id="33f8a-106">Ruční přizpůsobení migrace</span><span class="sxs-lookup"><span data-stu-id="33f8a-106">Manual migration customization</span></span>
* <span data-ttu-id="33f8a-107">Vlastní inicializační logika</span><span class="sxs-lookup"><span data-stu-id="33f8a-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="33f8a-108">Data počátečního modelu</span><span class="sxs-lookup"><span data-stu-id="33f8a-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="33f8a-109">Tato funkce je v EF Core 2,1 novinkou.</span><span class="sxs-lookup"><span data-stu-id="33f8a-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="33f8a-110">Na rozdíl od EF6, v EF Core lze data osazení přidružit k typu entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="33f8a-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="33f8a-111">Pak EF Core [migrace](xref:core/managing-schemas/migrations/index) může automaticky vypočítat, které operace vložení, aktualizace nebo odstranění se musí použít při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="33f8a-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="33f8a-112">Migrace zohledňuje pouze změny modelu při určování, jakou operaci je nutné provést k získání počátečních dat do požadovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="33f8a-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="33f8a-113">Proto se může stát, že dojde ke ztrátě všech změn dat provedených mimo migrace nebo dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="33f8a-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="33f8a-114">V takovém případě se v `OnModelCreating`nakonfigurují počáteční data pro `Blog`:</span><span class="sxs-lookup"><span data-stu-id="33f8a-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="33f8a-115">Chcete-li přidat entity, které mají relaci, je nutné zadat hodnoty cizího klíče:</span><span class="sxs-lookup"><span data-stu-id="33f8a-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="33f8a-116">Pokud má typ entity ve stavu stín nějaké vlastnosti, můžete k poskytnutí těchto hodnot použít anonymní třídu:</span><span class="sxs-lookup"><span data-stu-id="33f8a-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="33f8a-117">Vlastní typy entit lze dosazením podobným způsobem:</span><span class="sxs-lookup"><span data-stu-id="33f8a-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="33f8a-118">Podívejte se na [úplný ukázkový projekt](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) pro další kontext.</span><span class="sxs-lookup"><span data-stu-id="33f8a-118">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="33f8a-119">Po přidání dat do modelu by se měly k použití změn použít [migrace](xref:core/managing-schemas/migrations/index) .</span><span class="sxs-lookup"><span data-stu-id="33f8a-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="33f8a-120">Pokud v rámci automatizovaného nasazení potřebujete použít migrace, můžete [vytvořit skript SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , který může být před provedením zobrazený.</span><span class="sxs-lookup"><span data-stu-id="33f8a-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="33f8a-121">Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční data, například pro testovací databázi nebo při použití poskytovatele v paměti nebo jakékoli nerelační databáze.</span><span class="sxs-lookup"><span data-stu-id="33f8a-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="33f8a-122">Všimněte si, že pokud databáze již existuje, `EnsureCreated()` nebude aktualizovat ani počáteční data v databázi.</span><span class="sxs-lookup"><span data-stu-id="33f8a-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="33f8a-123">V případě relačních databází byste neměli volat `EnsureCreated()`, pokud plánujete používat migrace.</span><span class="sxs-lookup"><span data-stu-id="33f8a-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="33f8a-124">Omezení dat osazení modelu</span><span class="sxs-lookup"><span data-stu-id="33f8a-124">Limitations of model seed data</span></span>

<span data-ttu-id="33f8a-125">Tento typ počátečních dat je spravovaný migracemi a skript, který aktualizuje data, která jsou už v databázi, se musí vygenerovat bez připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="33f8a-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="33f8a-126">Tato omezení představují některá omezení:</span><span class="sxs-lookup"><span data-stu-id="33f8a-126">This imposes some restrictions:</span></span>

* <span data-ttu-id="33f8a-127">Hodnota primárního klíče je nutné zadat i v případě, že je obvykle vygenerována databází.</span><span class="sxs-lookup"><span data-stu-id="33f8a-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="33f8a-128">Použije se k detekci změn dat mezi migracemi.</span><span class="sxs-lookup"><span data-stu-id="33f8a-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="33f8a-129">Dřív dosazený údaj se odebere, pokud se primární klíč změní jakýmkoli způsobem.</span><span class="sxs-lookup"><span data-stu-id="33f8a-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="33f8a-130">Proto je tato funkce nejužitečnější pro statická data, u kterých se neočekává Změna mimo migrace a která nezávisí na žádné jiné v databázi, například PSČ.</span><span class="sxs-lookup"><span data-stu-id="33f8a-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="33f8a-131">Pokud váš scénář obsahuje některý z následujících postupů, doporučuje se použít vlastní inicializační logiku popsanou v poslední části:</span><span class="sxs-lookup"><span data-stu-id="33f8a-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>

* <span data-ttu-id="33f8a-132">Dočasná data pro testování</span><span class="sxs-lookup"><span data-stu-id="33f8a-132">Temporary data for testing</span></span>
* <span data-ttu-id="33f8a-133">Data, která závisí na stavu databáze</span><span class="sxs-lookup"><span data-stu-id="33f8a-133">Data that depends on database state</span></span>
* <span data-ttu-id="33f8a-134">Data, která vyžadují generování klíčových hodnot databáze, včetně entit, které jako identitu používají alternativní klíče</span><span class="sxs-lookup"><span data-stu-id="33f8a-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="33f8a-135">Data, která vyžadují vlastní transformaci (která není zpracována [převody hodnot](xref:core/modeling/value-conversions)), jako například některé hodnoty hash hesla</span><span class="sxs-lookup"><span data-stu-id="33f8a-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="33f8a-136">Data, která vyžadují volání do externího rozhraní API, například ASP.NET Core rolí identity a vytváření uživatelů</span><span class="sxs-lookup"><span data-stu-id="33f8a-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="33f8a-137">Ruční přizpůsobení migrace</span><span class="sxs-lookup"><span data-stu-id="33f8a-137">Manual migration customization</span></span>

<span data-ttu-id="33f8a-138">Při přidání migrace se změny dat zadaných pomocí `HasData` transformují na volání `InsertData()`, `UpdateData()`a `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="33f8a-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="33f8a-139">Jedním ze způsobů, jak obejít některá omezení `HasData`, je místo toho ručně přidat tato volání nebo [vlastní operace](xref:core/managing-schemas/migrations/operations) do migrace.</span><span class="sxs-lookup"><span data-stu-id="33f8a-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="33f8a-140">Vlastní inicializační logika</span><span class="sxs-lookup"><span data-stu-id="33f8a-140">Custom initialization logic</span></span>

<span data-ttu-id="33f8a-141">Jednoduchý a účinný způsob, jak provádět osazení dat, je použít [`DbContext.SaveChanges()`](xref:core/saving/index) před zahájením provádění hlavní aplikační logiky.</span><span class="sxs-lookup"><span data-stu-id="33f8a-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="33f8a-142">Kód pro osazení by neměl být součástí normálního spuštění aplikace, protože to může způsobovat problémy s souběžnou synchronizací, když je spuštěno více instancí, a také vyžaduje, aby aplikace měla oprávnění upravovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="33f8a-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="33f8a-143">V závislosti na omezeních nasazení může být inicializační kód proveden různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="33f8a-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>

* <span data-ttu-id="33f8a-144">Místní spuštění inicializační aplikace</span><span class="sxs-lookup"><span data-stu-id="33f8a-144">Running the initialization app locally</span></span>
* <span data-ttu-id="33f8a-145">Nasazení inicializační aplikace pomocí hlavní aplikace, vyvolání inicializační rutiny a zakázání nebo odebrání inicializační aplikace.</span><span class="sxs-lookup"><span data-stu-id="33f8a-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="33f8a-146">To se obvykle dá automatizovat pomocí [profilů publikování](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="33f8a-146">This can usually be automated by using [publish profiles](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
