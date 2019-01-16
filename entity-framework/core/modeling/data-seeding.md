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
# <a name="data-seeding"></a><span data-ttu-id="3df38-102">Předvyplnění dat</span><span class="sxs-lookup"><span data-stu-id="3df38-102">Data Seeding</span></span>

<span data-ttu-id="3df38-103">Předvyplnění dat je proces sestavování databáze s počáteční sadu data.</span><span class="sxs-lookup"><span data-stu-id="3df38-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="3df38-104">To lze provést v EF Core několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="3df38-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="3df38-105">Počáteční hodnota data modelu</span><span class="sxs-lookup"><span data-stu-id="3df38-105">Model seed data</span></span>
* <span data-ttu-id="3df38-106">Přizpůsobení ruční migraci</span><span class="sxs-lookup"><span data-stu-id="3df38-106">Manual migration customization</span></span>
* <span data-ttu-id="3df38-107">Logika vlastní inicializaci.</span><span class="sxs-lookup"><span data-stu-id="3df38-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="3df38-108">Počáteční hodnota data modelu</span><span class="sxs-lookup"><span data-stu-id="3df38-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="3df38-109">Tato funkce je nového v EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="3df38-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="3df38-110">Na rozdíl od v EF6 do EF Core předvyplnění dat může být přidružený typ entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="3df38-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="3df38-111">Potom EF Core [migrace](xref:core/managing-schemas/migrations/index) dokáže automaticky výpočetní co vložit, aktualizovat nebo odstranit operací nutné použít při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="3df38-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="3df38-112">Při určování, jaké operace se provádí, aby dostat počáteční data do požadovaného stavu, migrace uvažuje pouze změny modelu.</span><span class="sxs-lookup"><span data-stu-id="3df38-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="3df38-113">Všechny změny dat provést mimo migrace může tedy dojít ke ztrátě nebo může způsobit chybu.</span><span class="sxs-lookup"><span data-stu-id="3df38-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="3df38-114">Jako příklad, tím nakonfigurujete počáteční data `Blog` v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="3df38-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="3df38-115">Přidání entity, které mají hodnoty cizího klíče relace je potřeba zadat:</span><span class="sxs-lookup"><span data-stu-id="3df38-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="3df38-116">Typ entity má všechny vlastnosti v stínové stavu anonymní třída slouží k poskytování hodnot:</span><span class="sxs-lookup"><span data-stu-id="3df38-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="3df38-117">Vlastní entity, které typy lze nasadí podobným způsobem:</span><span class="sxs-lookup"><span data-stu-id="3df38-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="3df38-118">Zobrazit [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) pro širší kontext.</span><span class="sxs-lookup"><span data-stu-id="3df38-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="3df38-119">Po přidání dat do modelu, [migrace](xref:core/managing-schemas/migrations/index) by měla sloužit k použití změn.</span><span class="sxs-lookup"><span data-stu-id="3df38-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="3df38-120">Pokud potřebujete provést migrace jako součást automatizované nasazení můžete [vytvořit skript SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) , který lze zobrazit náhled před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="3df38-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="3df38-121">Alternativně můžete použít `context.Database.EnsureCreated()` k vytvoření nové databáze obsahující počáteční hodnoty dat, jako je například testovací databázi nebo při použití zprostředkovatele v paměti nebo jakékoli jiné relace databázi.</span><span class="sxs-lookup"><span data-stu-id="3df38-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="3df38-122">Všimněte si, že pokud databáze již existuje, `EnsureCreated()` ani aktualizuje schéma ani počáteční hodnoty dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="3df38-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="3df38-123">U relačních databází byste neměli volat `EnsureCreated()` Pokud budete chtít použít migrace.</span><span class="sxs-lookup"><span data-stu-id="3df38-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

### <a name="limitations-of-model-seed-data"></a><span data-ttu-id="3df38-124">Omezení modelování počáteční hodnoty dat</span><span class="sxs-lookup"><span data-stu-id="3df38-124">Limitations of model seed data</span></span>

<span data-ttu-id="3df38-125">Tento typ dat počáteční hodnoty spravuje migrace a skript, který chcete aktualizovat data, která je již v databázi je potřeba vygenerovat bez připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="3df38-125">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="3df38-126">To má určitá omezení:</span><span class="sxs-lookup"><span data-stu-id="3df38-126">This imposes some restrictions:</span></span>
* <span data-ttu-id="3df38-127">Hodnota primárního klíče musí být zadaná i v případě, že je obvykle generován databází.</span><span class="sxs-lookup"><span data-stu-id="3df38-127">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="3df38-128">Se použije ke zjištění změny dat mezi migrace.</span><span class="sxs-lookup"><span data-stu-id="3df38-128">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="3df38-129">Dříve dosazené odeberou data žádným způsobem při změně primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="3df38-129">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="3df38-130">Tato funkce je proto zvláště užitečná pro statická data, která se neočekává změna mimo migrace a nezávisí na nic jiného v databázi, například PSČ.</span><span class="sxs-lookup"><span data-stu-id="3df38-130">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="3df38-131">Pokud váš scénář zahrnuje některé z následujících akcí se doporučuje použít logiku vlastní inicializace je popsáno v předchozí části:</span><span class="sxs-lookup"><span data-stu-id="3df38-131">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="3df38-132">Dočasná data pro účely testování</span><span class="sxs-lookup"><span data-stu-id="3df38-132">Temporary data for testing</span></span>
* <span data-ttu-id="3df38-133">Data, která závisí na stav databáze</span><span class="sxs-lookup"><span data-stu-id="3df38-133">Data that depends on database state</span></span>
* <span data-ttu-id="3df38-134">Data, která vyžadují hodnoty klíče, který byl vygenerován nástrojem databáze, včetně entity, které používají alternativní klíče jako identita</span><span class="sxs-lookup"><span data-stu-id="3df38-134">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="3df38-135">Data, která vyžaduje vlastní transformace (, která není zpracována [hodnota převody](xref:core/modeling/value-conversions)), jako je například některé hashování hesel</span><span class="sxs-lookup"><span data-stu-id="3df38-135">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="3df38-136">Data, která vyžaduje volání externí rozhraní API, jako je například vytváření rolí a uživatelů v ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="3df38-136">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="3df38-137">Přizpůsobení ruční migraci</span><span class="sxs-lookup"><span data-stu-id="3df38-137">Manual migration customization</span></span>

<span data-ttu-id="3df38-138">Při migraci přidání změny dat zadaný `HasData` se transformuje na volání `InsertData()`, `UpdateData()`, a `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="3df38-138">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="3df38-139">Jeden ze způsobů, jak obejít některá omezení `HasData` je ručně přidat tato volání nebo [vlastní operace](xref:core/managing-schemas/migrations/operations) migrace místo.</span><span class="sxs-lookup"><span data-stu-id="3df38-139">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="3df38-140">Logika vlastní inicializaci.</span><span class="sxs-lookup"><span data-stu-id="3df38-140">Custom initialization logic</span></span>

<span data-ttu-id="3df38-141">Snadný a efektivní způsob, jak provádět zálohy dat je použití [ `DbContext.SaveChanges()` ](xref:core/saving/index) než hlavní aplikace logiky zahájí vykonávání.</span><span class="sxs-lookup"><span data-stu-id="3df38-141">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="3df38-142">Osazení kód by neměl být součást provedení běžná aplikace, protože to může způsobit problémy se souběžností, pokud běží více instancí a bude také vyžadovat aplikace máte oprávnění k úpravě databázového schématu.</span><span class="sxs-lookup"><span data-stu-id="3df38-142">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="3df38-143">V závislosti na omezení nasazení inicializační kód může provést různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="3df38-143">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="3df38-144">Místně spuštěná aplikace inicializace</span><span class="sxs-lookup"><span data-stu-id="3df38-144">Running the initialization app locally</span></span>
* <span data-ttu-id="3df38-145">Nasazení aplikace inicializace s hlavní aplikací, volání inicializační rutina a zakázání nebo odebrání inicializace aplikace.</span><span class="sxs-lookup"><span data-stu-id="3df38-145">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="3df38-146">To je obvykle možné automatizovat pomocí [publikační profily](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="3df38-146">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
