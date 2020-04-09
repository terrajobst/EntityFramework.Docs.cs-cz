---
title: Přenos z EF6 na EF Core – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416950"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="abfae-102">Přenesení z EF6 do EF Core</span><span class="sxs-lookup"><span data-stu-id="abfae-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="abfae-103">Vzhledem k zásadní změny v EF Core nedoporučujeme pokoušet se přesunout ef6 aplikace EFCore, pokud máte pádný důvod provést změnu.</span><span class="sxs-lookup"><span data-stu-id="abfae-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="abfae-104">Měli byste zobrazit přechod z EF6 na EF Core jako port, nikoli upgrade.</span><span class="sxs-lookup"><span data-stu-id="abfae-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abfae-105">Před zahájením procesu přenosu je důležité ověřit, zda EF Core splňuje požadavky na přístup k datům pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="abfae-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="abfae-106">Chybějící funkce</span><span class="sxs-lookup"><span data-stu-id="abfae-106">Missing features</span></span>

<span data-ttu-id="abfae-107">Ujistěte se, že EF Core má všechny funkce, které potřebujete použít ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="abfae-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="abfae-108">Podrobné porovnání vlastností sady funkcí v EF Core s ef6 najdete v tématu [Porovnání funkcí.](xref:efcore-and-ef6/index)</span><span class="sxs-lookup"><span data-stu-id="abfae-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="abfae-109">Pokud některé požadované funkce chybí, ujistěte se, že můžete kompenzovat nedostatek těchto funkcí před přenesením na EF Core.</span><span class="sxs-lookup"><span data-stu-id="abfae-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="abfae-110">Změny chování</span><span class="sxs-lookup"><span data-stu-id="abfae-110">Behavior changes</span></span>

<span data-ttu-id="abfae-111">Toto je nevyčerpávající seznam některých změn v chování mezi EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="abfae-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="abfae-112">Je důležité mít na paměti, jako port aplikace, protože mohou změnit způsob, jakým se vaše aplikace chová, ale nezobrazí se jako chyby kompilace po přepnutí na EF Core.</span><span class="sxs-lookup"><span data-stu-id="abfae-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="abfae-113">DbSet.Add/Attach a chování grafu</span><span class="sxs-lookup"><span data-stu-id="abfae-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="abfae-114">V EF6 `DbSet.Add()` volání entity má za následek rekurzivní hledání pro všechny entity odkazované v jeho navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="abfae-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="abfae-115">Všechny entity, které jsou nalezeny a nejsou již sledovány kontextem, jsou také označeny jako přidané.</span><span class="sxs-lookup"><span data-stu-id="abfae-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="abfae-116">`DbSet.Attach()`chová se stejně, s výjimkou všech entit jsou označeny jako nezměněné.</span><span class="sxs-lookup"><span data-stu-id="abfae-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="abfae-117">**EF Core provádí podobné rekurzivní vyhledávání, ale s některými mírně odlišnými pravidly.**</span><span class="sxs-lookup"><span data-stu-id="abfae-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="abfae-118">Kořenová entita je vždy v `DbSet.Add` požadovaném `DbSet.Attach`stavu (přidána pro a nezměněná pro ).</span><span class="sxs-lookup"><span data-stu-id="abfae-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="abfae-119">**Pro entity, které se nacházejí během rekurzivního hledání navigačních vlastností:**</span><span class="sxs-lookup"><span data-stu-id="abfae-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="abfae-120">**Pokud je vygenerován primární klíč entity**</span><span class="sxs-lookup"><span data-stu-id="abfae-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="abfae-121">Pokud primární klíč není nastaven na hodnotu, stav je nastaven na přidán.</span><span class="sxs-lookup"><span data-stu-id="abfae-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="abfae-122">`0` Hodnota primárního klíče je považována za "není nastavena", pokud je přiřazena `int`výchozí `null` `string`hodnota CLR pro typ vlastnosti (například pro , for , atd.).</span><span class="sxs-lookup"><span data-stu-id="abfae-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="abfae-123">Pokud je primární klíč nastaven na hodnotu, stav je nastaven na nezměněné.</span><span class="sxs-lookup"><span data-stu-id="abfae-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="abfae-124">Pokud primární klíč není generován databáze, entita je umístěn ve stejném stavu jako kořen.</span><span class="sxs-lookup"><span data-stu-id="abfae-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="abfae-125">První inicializace první databáze kódu</span><span class="sxs-lookup"><span data-stu-id="abfae-125">Code First database initialization</span></span>

<span data-ttu-id="abfae-126">**EF6 má značné množství magie provádí kolem výběru připojení k databázi a inicializace databáze. Některá z těchto pravidel zahrnují:**</span><span class="sxs-lookup"><span data-stu-id="abfae-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="abfae-127">Pokud není provedena žádná konfigurace, EF6 vybere databázi na SQL Express nebo LocalDb.</span><span class="sxs-lookup"><span data-stu-id="abfae-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="abfae-128">Pokud je v souboru aplikace `App/Web.config` připojovací řetězec se stejným názvem jako kontext, bude použito toto připojení.</span><span class="sxs-lookup"><span data-stu-id="abfae-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="abfae-129">Pokud databáze neexistuje, je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="abfae-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="abfae-130">Pokud žádná z tabulek z modelu existují v databázi, schéma pro aktuální model je přidán do databáze.</span><span class="sxs-lookup"><span data-stu-id="abfae-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="abfae-131">Pokud jsou povoleny migrace, pak se používají k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="abfae-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="abfae-132">Pokud databáze existuje a EF6 dříve vytvořil schéma, pak je schéma zkontrolováno kompatibilitu s aktuálním modelem.</span><span class="sxs-lookup"><span data-stu-id="abfae-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="abfae-133">Výjimka je vyvolána, pokud se model změnil od vytvoření schématu.</span><span class="sxs-lookup"><span data-stu-id="abfae-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="abfae-134">**EF Core neprovádí žádné z této magie.**</span><span class="sxs-lookup"><span data-stu-id="abfae-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="abfae-135">Připojení databáze musí být explicitně nakonfigurováno v kódu.</span><span class="sxs-lookup"><span data-stu-id="abfae-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="abfae-136">Není provedena žádná inicializace.</span><span class="sxs-lookup"><span data-stu-id="abfae-136">No initialization is performed.</span></span> <span data-ttu-id="abfae-137">Je nutné `DbContext.Database.Migrate()` použít migrace `DbContext.Database.EnsureCreated()` (nebo `EnsureDeleted()` vytvořit nebo odstranit databázi bez použití migrace).</span><span class="sxs-lookup"><span data-stu-id="abfae-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="abfae-138">Konvence pojmenování první tabulky kódu</span><span class="sxs-lookup"><span data-stu-id="abfae-138">Code First table naming convention</span></span>

<span data-ttu-id="abfae-139">EF6 spustí název třídy entity prostřednictvím služby pluralizace k výpočtu výchozího názvu tabulky, na který je entita mapována.</span><span class="sxs-lookup"><span data-stu-id="abfae-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="abfae-140">EF Core používá název `DbSet` vlastnosti, která je vystavena v na odvozeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="abfae-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="abfae-141">Pokud entita nemá `DbSet` vlastnost, použije se název třídy.</span><span class="sxs-lookup"><span data-stu-id="abfae-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
