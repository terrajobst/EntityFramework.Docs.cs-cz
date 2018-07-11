---
title: Přenesení z EF6 do EF Core – ověření požadavků na
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949111"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="6b358-102">Před přenesení z EF6 do EF Core: ověření podle požadavků vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="6b358-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="6b358-103">Před zahájením procesu přenos je důležité, abyste ověřili, že EF Core splňuje požadavky na datový přístup pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b358-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="6b358-104">Chybějící funkce</span><span class="sxs-lookup"><span data-stu-id="6b358-104">Missing features</span></span>

<span data-ttu-id="6b358-105">Ujistěte se, že EF Core má všechny funkce, které je potřeba použít ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b358-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="6b358-106">Zobrazit [porovnání funkcí](../features.md) jak funkce nastavit v EF Core porovná s EF6 podrobné porovnání.</span><span class="sxs-lookup"><span data-stu-id="6b358-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="6b358-107">Pokud nebyly nalezeny všechny požadované funkce, ujistěte se, že může kompenzovat chybějící tyto funkce před přenesení do EF Core.</span><span class="sxs-lookup"><span data-stu-id="6b358-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="6b358-108">Změny chování</span><span class="sxs-lookup"><span data-stu-id="6b358-108">Behavior changes</span></span>

<span data-ttu-id="6b358-109">Toto je seznam není vyčerpávající některé změny v chování mezi EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="6b358-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="6b358-110">Je důležité udržovat v paměti jako port aplikace jako mění způsob, jak vaše aplikace chová, ale nezobrazí jako chyby při kompilaci po záměně na EF Core.</span><span class="sxs-lookup"><span data-stu-id="6b358-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="6b358-111">Graf a DbSet.Add/Attach chování</span><span class="sxs-lookup"><span data-stu-id="6b358-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="6b358-112">V EF6 volání `DbSet.Add()` na entity výsledkem rekurzivní hledání pro všechny entity odkazovat v jeho navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6b358-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="6b358-113">Žádné entity, které se nacházejí a nejsou již sledován pomocí funkce kontextu, jsou také označit jako přidali.</span><span class="sxs-lookup"><span data-stu-id="6b358-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="6b358-114">`DbSet.Attach()` se chová stejně, s výjimkou všechny entity jsou označeny jako beze změny.</span><span class="sxs-lookup"><span data-stu-id="6b358-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="6b358-115">**EF Core provádí podobné rekurzivní hledání, ale některé mírně odlišná pravidla.**</span><span class="sxs-lookup"><span data-stu-id="6b358-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="6b358-116">Kořenová entita je vždy v požadovaného stavu (přidat pro `DbSet.Add` a beze změny pro `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="6b358-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="6b358-117">**Pro entity, které byly nalezeny během rekurzivní hledání navigační vlastnosti:**</span><span class="sxs-lookup"><span data-stu-id="6b358-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="6b358-118">**Pokud je úložiště vygenerovat primární klíč entity**</span><span class="sxs-lookup"><span data-stu-id="6b358-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="6b358-119">Pokud primární klíč není nastavena na hodnotu, stav je nastaven na přidání.</span><span class="sxs-lookup"><span data-stu-id="6b358-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="6b358-120">Hodnota primárního klíče je považován za "Nenastaveno" Pokud je přiřazen výchozí hodnotu CLR pro tento typ vlastnosti (například `0` pro `int`, `null` pro `string`atd.).</span><span class="sxs-lookup"><span data-stu-id="6b358-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="6b358-121">Pokud primární klíč je nastavena na hodnotu, stav je nastaven na beze změny.</span><span class="sxs-lookup"><span data-stu-id="6b358-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="6b358-122">Pokud primární klíč není databáze, které jsou generovány, entita přejde do stejného stavu jako kořen.</span><span class="sxs-lookup"><span data-stu-id="6b358-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="6b358-123">Kód první inicializaci databáze</span><span class="sxs-lookup"><span data-stu-id="6b358-123">Code First database initialization</span></span>

<span data-ttu-id="6b358-124">**EF6 má významné množství magic, které provádí po výběru připojení k databázi a inicializaci databáze. Mezi tyto pravidla patří:**</span><span class="sxs-lookup"><span data-stu-id="6b358-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="6b358-125">Pokud žádná konfigurace se provádí, vybere EF6 databáze na SQL Express nebo LocalDb.</span><span class="sxs-lookup"><span data-stu-id="6b358-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="6b358-126">Pokud je řetězec připojení se stejným názvem jako kontext v aplikacích `App/Web.config` souboru, se použije toto připojení.</span><span class="sxs-lookup"><span data-stu-id="6b358-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="6b358-127">Pokud databáze buď neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="6b358-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="6b358-128">Pokud žádná z tabulek z modelu v databázi neexistuje, schéma pro aktuální model je přidána do databáze.</span><span class="sxs-lookup"><span data-stu-id="6b358-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="6b358-129">Pokud migrace jsou povolené, pak se používají k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="6b358-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="6b358-130">Pokud existuje databáze a EF6 už dřív vytvořil schématu, schéma se kontroluje u kompatibility aktuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="6b358-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="6b358-131">Pokud model byl změněn od vytvoření schématu je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="6b358-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="6b358-132">**EF Core neprovede žádné tento kouzla.**</span><span class="sxs-lookup"><span data-stu-id="6b358-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="6b358-133">Připojení k databázi musí být explicitně nakonfigurované v kódu.</span><span class="sxs-lookup"><span data-stu-id="6b358-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="6b358-134">Není inicializace provedena.</span><span class="sxs-lookup"><span data-stu-id="6b358-134">No initialization is performed.</span></span> <span data-ttu-id="6b358-135">Je nutné použít `DbContext.Database.Migrate()` použití migrace (nebo `DbContext.Database.EnsureCreated()` a `EnsureDeleted()` k vytvoření/odstranění databáze bez použití migrace).</span><span class="sxs-lookup"><span data-stu-id="6b358-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="6b358-136">První tabulka kódu zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="6b358-136">Code First table naming convention</span></span>

<span data-ttu-id="6b358-137">EF6 projde pluralizace služby, která vypočítá výchozí název tabulky, která entita je namapována na název třídy entity.</span><span class="sxs-lookup"><span data-stu-id="6b358-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="6b358-138">EF Core používá název `DbSet` vlastnost, která entity zpřístupněn v rámci odvozené.</span><span class="sxs-lookup"><span data-stu-id="6b358-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="6b358-139">Pokud nemá žádné entity `DbSet` vlastnost a potom název třídy se používá.</span><span class="sxs-lookup"><span data-stu-id="6b358-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
