---
title: Přenesení z EF6 do EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 42e40ce769a67a987883027e1807ec7eaeb4ad7a
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198025"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="9c9cd-102">Přenesení z EF6 do EF Core</span><span class="sxs-lookup"><span data-stu-id="9c9cd-102">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="9c9cd-103">Z důvodu zásadních změn v EF Core nedoporučujeme migraci z EF6 na EF Core, pokud pro takovou změnu nemáte dobrý důvod.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-103">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="9c9cd-104">Měli byste zobrazit přesun z EF6, aby se místo upgradu EF Core jako port.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-104">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c9cd-105">Než zahájíte proces přenosu, je důležité ověřit, že EF Core splňuje požadavky na přístup k datům pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-105">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="9c9cd-106">Chybějící funkce</span><span class="sxs-lookup"><span data-stu-id="9c9cd-106">Missing features</span></span>

<span data-ttu-id="9c9cd-107">Ujistěte se, že EF Core obsahuje všechny funkce, které v aplikaci potřebujete použít.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-107">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="9c9cd-108">Podrobné porovnání toho, jak je sada funkcí v EF Core porovnána s EF6, naleznete v tématu [porovnání funkcí](xref:efcore-and-ef6/index) .</span><span class="sxs-lookup"><span data-stu-id="9c9cd-108">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="9c9cd-109">Pokud některé z požadovaných funkcí chybí, zajistěte, abyste před přenosem na EF Core mohli kompenzovat nedostatek těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-109">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="9c9cd-110">Změny chování</span><span class="sxs-lookup"><span data-stu-id="9c9cd-110">Behavior changes</span></span>

<span data-ttu-id="9c9cd-111">Toto je nevyčerpávající seznam některých změn v chování mezi EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-111">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="9c9cd-112">Je důležité mít na paměti, že vaše aplikace může měnit způsob, jakým se aplikace chová, ale po odchodu do EF Core se nezobrazí jako chyby kompilace.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-112">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="9c9cd-113">Negenerickými. Přidání/připojení a chování grafu</span><span class="sxs-lookup"><span data-stu-id="9c9cd-113">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="9c9cd-114">V EF6 volání `DbSet.Add()` na entitu má za následek rekurzivní vyhledávání všech entit, na které se odkazuje ve vlastnostech navigace.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-114">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="9c9cd-115">Všechny nalezené entity a již nejsou sledovány kontextem, jsou také označeny jako přidané.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-115">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="9c9cd-116">`DbSet.Attach()`se chová stejně, s výjimkou všech entit jsou označeny jako beze změny.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-116">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="9c9cd-117">**EF Core provádí podobné rekurzivní vyhledávání, ale s trochu odlišnými pravidly.**</span><span class="sxs-lookup"><span data-stu-id="9c9cd-117">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="9c9cd-118">Kořenová entita je vždy ve vyžádaném stavu (přidáno `DbSet.Add` pro a `DbSet.Attach`beze změny).</span><span class="sxs-lookup"><span data-stu-id="9c9cd-118">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="9c9cd-119">**Pro entity, které byly nalezeny během rekurzivního prohledávání vlastností navigace:**</span><span class="sxs-lookup"><span data-stu-id="9c9cd-119">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="9c9cd-120">**Pokud je primární klíč entity vygenerovaný jako úložiště**</span><span class="sxs-lookup"><span data-stu-id="9c9cd-120">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="9c9cd-121">Pokud primární klíč není nastaven na hodnotu, stav je nastaveno na přidáno.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-121">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="9c9cd-122">Hodnota primárního klíče se považuje za nenastavenou, pokud je přiřazena výchozí hodnota CLR pro daný typ vlastnosti ( `0` například pro `int`, `null` pro `string`atd.).</span><span class="sxs-lookup"><span data-stu-id="9c9cd-122">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="9c9cd-123">Pokud je primární klíč nastaven na hodnotu, stav je nastaven na nezměněný.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-123">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="9c9cd-124">Pokud primární klíč není vygenerovaný databází, entita se umístí do stejného stavu jako kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-124">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="9c9cd-125">Inicializace databáze Code First</span><span class="sxs-lookup"><span data-stu-id="9c9cd-125">Code First database initialization</span></span>

<span data-ttu-id="9c9cd-126">**EF6 má velký výkon, který vychází z výběru připojení databáze a inicializace databáze. Mezi tato pravidla patří:**</span><span class="sxs-lookup"><span data-stu-id="9c9cd-126">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="9c9cd-127">Pokud se neprovede žádná konfigurace, EF6 vybere databázi na SQL Express nebo LocalDb.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-127">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="9c9cd-128">Pokud je připojovací řetězec se stejným názvem jako kontext v souboru aplikace `App/Web.config` , bude použito toto připojení.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-128">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="9c9cd-129">Pokud databáze neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-129">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="9c9cd-130">Pokud v databázi neexistuje žádná z tabulek z modelu, je schéma pro aktuální model přidáno do databáze.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-130">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="9c9cd-131">Pokud jsou povolené migrace, použijí se k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-131">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="9c9cd-132">Pokud databáze existuje a EF6 dříve vytvořila schéma, je u schématu kontrolována kompatibilita s aktuálním modelem.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-132">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="9c9cd-133">Výjimka je vyvolána, pokud se model od vytvoření schématu změnil.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-133">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="9c9cd-134">**EF Core neprovádí žádné z těchto Magic.**</span><span class="sxs-lookup"><span data-stu-id="9c9cd-134">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="9c9cd-135">Připojení k databázi musí být explicitně nakonfigurované v kódu.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-135">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="9c9cd-136">Není provedena žádná inicializace.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-136">No initialization is performed.</span></span> <span data-ttu-id="9c9cd-137">K použití migrace `DbContext.Database.Migrate()` ( `DbContext.Database.EnsureCreated()` `EnsureDeleted()` nebo k vytvoření/odstranění databáze bez použití migrace) musíte použít.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-137">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="9c9cd-138">Code First zásady vytváření názvů tabulek</span><span class="sxs-lookup"><span data-stu-id="9c9cd-138">Code First table naming convention</span></span>

<span data-ttu-id="9c9cd-139">EF6 spustí název třídy entity prostřednictvím služby pro pojmenování a vypočítá výchozí název tabulky, na kterou je entita namapována.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-139">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="9c9cd-140">EF Core používá název `DbSet` vlastnosti, ke které je entita vystavena v odvozeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-140">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="9c9cd-141">Pokud entita `DbSet` neobsahuje vlastnost, použije se název třídy.</span><span class="sxs-lookup"><span data-stu-id="9c9cd-141">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
