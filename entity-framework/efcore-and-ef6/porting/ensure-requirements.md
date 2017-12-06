---
title: "Portování ze EF6 na jádro EF – ověří, jestli požadavky"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="027dd-102">Před Portování ze EF6 na jádro EF: ověření požadavků vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="027dd-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="027dd-103">Před zahájením procesu přenosem je potřeba ověřit, že základní EF splňuje požadavky na přístup pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="027dd-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="027dd-104">Chybějící funkce</span><span class="sxs-lookup"><span data-stu-id="027dd-104">Missing features</span></span>

<span data-ttu-id="027dd-105">Ujistěte se, že EF základní obsahuje všechny funkce, které budete muset použít v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="027dd-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="027dd-106">V tématu [porovnání funkcí](../features.md) podrobné porovnání jak funkci nastavena v základní EF porovnává EF6.</span><span class="sxs-lookup"><span data-stu-id="027dd-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="027dd-107">Pokud chybí všechny požadované funkce, ujistěte se, nedostatečná tyto funkce vám může kompenzovat před portování do EF základní.</span><span class="sxs-lookup"><span data-stu-id="027dd-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="027dd-108">Změny chování</span><span class="sxs-lookup"><span data-stu-id="027dd-108">Behavior changes</span></span>

<span data-ttu-id="027dd-109">Toto je seznam některé změny v chování mezi EF6 a EF základní doplňovat.</span><span class="sxs-lookup"><span data-stu-id="027dd-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="027dd-110">Je důležité udržovat v paměti jako portů aplikaci jako se můžou změnit způsob, jakým aplikace se chová, ale nebude zobrazují jako chyby při kompilaci po odkládací na EF jádro.</span><span class="sxs-lookup"><span data-stu-id="027dd-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="027dd-111">Chování DbSet.Add/Attach a graf</span><span class="sxs-lookup"><span data-stu-id="027dd-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="027dd-112">V EF6 volání `DbSet.Add()` o výsledcích entity v rekurzivní hledání pro všechny entity, kterou se odkazuje v jeho navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="027dd-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="027dd-113">Všechny entity, které se nacházejí a nejsou již sledován pomocí kontextu, jsou také označit jako přidat.</span><span class="sxs-lookup"><span data-stu-id="027dd-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="027dd-114">`DbSet.Attach()`se chová stejně, s výjimkou všechny entity jsou označeny jako beze změny.</span><span class="sxs-lookup"><span data-stu-id="027dd-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="027dd-115">**Základní EF provede podobné rekurzivní hledání, ale některé mírně odlišná pravidla.**</span><span class="sxs-lookup"><span data-stu-id="027dd-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="027dd-116">Entita kořenové je vždy v požadovaný stav (přidat pro `DbSet.Add` a nezměněné pro `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="027dd-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="027dd-117">**Pro entity, které se nacházejí během rekurzivní hledání navigační vlastnosti:**</span><span class="sxs-lookup"><span data-stu-id="027dd-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="027dd-118">**Pokud je primární klíč entity generovaný úložištěm**</span><span class="sxs-lookup"><span data-stu-id="027dd-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="027dd-119">Pokud primární klíč není nastavena na hodnotu, stav nastaven na přidaný.</span><span class="sxs-lookup"><span data-stu-id="027dd-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="027dd-120">Hodnotu primárního klíče považuje za "není nastavena" Pokud má přiřazenou CLR výchozí hodnota pro typ vlastnosti (tj. `0` pro `int`, `null` pro `string`atd.).</span><span class="sxs-lookup"><span data-stu-id="027dd-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="027dd-121">Pokud primární klíč je nastavena na hodnotu, stav nastaven beze změny.</span><span class="sxs-lookup"><span data-stu-id="027dd-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="027dd-122">Pokud není primární klíč generovaný databází, je entita umístit do stejného stavu jako kořen.</span><span class="sxs-lookup"><span data-stu-id="027dd-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="027dd-123">Kód první inicializaci databáze</span><span class="sxs-lookup"><span data-stu-id="027dd-123">Code First database initialization</span></span>

<span data-ttu-id="027dd-124">**EF6 má významné množství magic, které provádí kolem výběrem připojení k databázi a inicializace databáze. Mezi tato pravidla patří:**</span><span class="sxs-lookup"><span data-stu-id="027dd-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="027dd-125">Pokud žádná konfigurace se provádí, vybere EF6 databáze na SQL Express nebo LocalDb.</span><span class="sxs-lookup"><span data-stu-id="027dd-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="027dd-126">Pokud je řetězec připojení se stejným názvem jako kontext v aplikacích `App/Web.config` souboru, použije se toto připojení.</span><span class="sxs-lookup"><span data-stu-id="027dd-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="027dd-127">Pokud databáze neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="027dd-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="027dd-128">Pokud žádná z tabulek z modelu neexistují v databázi, schéma pro aktuální model se přidá do databáze.</span><span class="sxs-lookup"><span data-stu-id="027dd-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="027dd-129">Pokud jsou povolené migrace, pak se používají k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="027dd-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="027dd-130">Pokud existuje databáze a EF6 dříve vytvořili schéma, schéma se kontroluje k zajištění kompatibility se aktuální model.</span><span class="sxs-lookup"><span data-stu-id="027dd-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="027dd-131">Pokud model změnil od chvíle vytvoření schématu, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="027dd-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="027dd-132">**Základní EF neprovádí žádné z této magic.**</span><span class="sxs-lookup"><span data-stu-id="027dd-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="027dd-133">Připojení k databázi musí být explicitně nakonfigurovaný v kódu.</span><span class="sxs-lookup"><span data-stu-id="027dd-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="027dd-134">Neprobíhá žádná inicializace.</span><span class="sxs-lookup"><span data-stu-id="027dd-134">No initialization is performed.</span></span> <span data-ttu-id="027dd-135">Je nutné použít `DbContext.Database.Migrate()` použití migrace (nebo `DbContext.Database.EnsureCreated()` a `EnsureDeleted()` k vytvoření nebo odstranění databáze bez použití migrace).</span><span class="sxs-lookup"><span data-stu-id="027dd-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="027dd-136">První tabulka kód zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="027dd-136">Code First table naming convention</span></span>

<span data-ttu-id="027dd-137">EF6 spouští prostřednictvím pluralizační služby k výpočtu výchozí název tabulky, který entita je namapovaná na název třídy entity.</span><span class="sxs-lookup"><span data-stu-id="027dd-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="027dd-138">Základní EF používá název `DbSet` vlastnost, která entity je zpřístupněná na odvozené kontextu.</span><span class="sxs-lookup"><span data-stu-id="027dd-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="027dd-139">Pokud nemá entity `DbSet` vlastnost a potom název třídy se používá.</span><span class="sxs-lookup"><span data-stu-id="027dd-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
