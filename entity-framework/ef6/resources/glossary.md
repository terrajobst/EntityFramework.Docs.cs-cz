---
title: Glosář Entity Framework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656148"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="e9410-102">Glosář Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e9410-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="e9410-103">Code First</span><span class="sxs-lookup"><span data-stu-id="e9410-103">Code First</span></span>
<span data-ttu-id="e9410-104">Vytvoření modelu Entity Framework pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="e9410-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="e9410-105">Model může cílit na existující databázi nebo novou databázi.</span><span class="sxs-lookup"><span data-stu-id="e9410-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="e9410-106">Souvislost</span><span class="sxs-lookup"><span data-stu-id="e9410-106">Context</span></span>
<span data-ttu-id="e9410-107">Třída, která představuje relaci s databází a umožňuje dotazování a ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="e9410-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="e9410-108">Kontext je odvozen z třídy DbContext nebo ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="e9410-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="e9410-109">Konvence (Code First)</span><span class="sxs-lookup"><span data-stu-id="e9410-109">Convention (Code First)</span></span>
<span data-ttu-id="e9410-110">Pravidlo, které Entity Framework používá k odvození tvaru vašeho modelu z vašich tříd.</span><span class="sxs-lookup"><span data-stu-id="e9410-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="e9410-111">Database First</span><span class="sxs-lookup"><span data-stu-id="e9410-111">Database First</span></span>
<span data-ttu-id="e9410-112">Vytvoření modelu Entity Framework pomocí návrháře EF, který cílí na stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="e9410-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="e9410-113">Eager načítání</span><span class="sxs-lookup"><span data-stu-id="e9410-113">Eager loading</span></span>
<span data-ttu-id="e9410-114">Vzor načítání souvisejících dat, kde dotaz na jeden typ entity také načte související entity jako součást dotazu.</span><span class="sxs-lookup"><span data-stu-id="e9410-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="e9410-115">Návrhář EF</span><span class="sxs-lookup"><span data-stu-id="e9410-115">EF Designer</span></span>
<span data-ttu-id="e9410-116">Vizuální Návrhář v aplikaci Visual Studio, který umožňuje vytvořit Entity Framework model pomocí polí a čar.</span><span class="sxs-lookup"><span data-stu-id="e9410-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="e9410-117">Entity</span><span class="sxs-lookup"><span data-stu-id="e9410-117">Entity</span></span>
<span data-ttu-id="e9410-118">Třída nebo objekt reprezentující data aplikace, jako jsou například zákazníci, produkty a objednávky.</span><span class="sxs-lookup"><span data-stu-id="e9410-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="e9410-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="e9410-119">Entity Data Model</span></span>
<span data-ttu-id="e9410-120">Model, který popisuje entity a vztahy mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="e9410-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="e9410-121">EF používá model EDM k popisu koncepčního modelu, proti kterému vývojářské programy.</span><span class="sxs-lookup"><span data-stu-id="e9410-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="e9410-122">Model EDM sestaví na modelu vztahů mezi entitami, který zavádí Dr. Petra Chen.</span><span class="sxs-lookup"><span data-stu-id="e9410-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="e9410-123">Model EDM byl původně vyvinut s primárním cílem, který se stane běžným datovým modelem v rámci sady technologií pro vývojáře a server od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="e9410-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="e9410-124">Model EDM se používá také jako součást protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="e9410-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="e9410-125">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="e9410-125">Explicit loading</span></span>
<span data-ttu-id="e9410-126">Vzor načítání souvisejících dat v případě, že související objekty jsou načteny voláním rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e9410-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e9410-127">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="e9410-127">Fluent API</span></span>
<span data-ttu-id="e9410-128">Rozhraní API, které lze použít ke konfiguraci Code Firstho modelu.</span><span class="sxs-lookup"><span data-stu-id="e9410-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="e9410-129">Přidružení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="e9410-129">Foreign key association</span></span>
<span data-ttu-id="e9410-130">Přidružení mezi entitami, kde vlastnost reprezentující cizí klíč je součástí třídy závislé entity.</span><span class="sxs-lookup"><span data-stu-id="e9410-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="e9410-131">Například produkt obsahuje vlastnost KódKategorie.</span><span class="sxs-lookup"><span data-stu-id="e9410-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="e9410-132">Identifikace vztahu</span><span class="sxs-lookup"><span data-stu-id="e9410-132">Identifying relationship</span></span>
<span data-ttu-id="e9410-133">Vztah, ve kterém je primární klíč hlavní entity součástí primárního klíče závislé entity.</span><span class="sxs-lookup"><span data-stu-id="e9410-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="e9410-134">V tomto typu relace nemůže závislá entita existovat bez hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="e9410-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="e9410-135">Nezávislé přidružení</span><span class="sxs-lookup"><span data-stu-id="e9410-135">Independent association</span></span>
<span data-ttu-id="e9410-136">Přidružení mezi entitami, kde neexistuje žádná vlastnost reprezentující cizí klíč ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="e9410-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="e9410-137">Například třída produktu obsahuje relaci ke kategorii, ale žádnou vlastnost KódKategorie.</span><span class="sxs-lookup"><span data-stu-id="e9410-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="e9410-138">Entity Framework sleduje stav přidružení nezávisle na stavu entit na obou koncích přidružení.</span><span class="sxs-lookup"><span data-stu-id="e9410-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="e9410-139">Opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="e9410-139">Lazy loading</span></span>
<span data-ttu-id="e9410-140">Vzor načítání souvisejících dat, kde jsou související objekty automaticky načteny, když je k dispozici navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e9410-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="e9410-141">Model First</span><span class="sxs-lookup"><span data-stu-id="e9410-141">Model First</span></span>
<span data-ttu-id="e9410-142">Vytvoření modelu Entity Framework pomocí návrháře EF, který je pak použit k vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="e9410-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="e9410-143">Navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="e9410-143">Navigation property</span></span>
<span data-ttu-id="e9410-144">Vlastnost entity, která odkazuje na jinou entitu.</span><span class="sxs-lookup"><span data-stu-id="e9410-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="e9410-145">Například produkt obsahuje navigační vlastnost kategorie a kategorie obsahuje navigační vlastnost Products.</span><span class="sxs-lookup"><span data-stu-id="e9410-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="e9410-146">POCO</span><span class="sxs-lookup"><span data-stu-id="e9410-146">POCO</span></span>
<span data-ttu-id="e9410-147">Zkratka pro objekt CLR v prostém starém formátu.</span><span class="sxs-lookup"><span data-stu-id="e9410-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="e9410-148">Jednoduchá třída uživatele, která nemá žádné závislosti s žádným rozhraním.</span><span class="sxs-lookup"><span data-stu-id="e9410-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="e9410-149">V kontextu EF Třída entity, která není odvozena od objektů EntityObject, implementuje jakákoli rozhraní nebo přenáší žádné atributy definované v EF.</span><span class="sxs-lookup"><span data-stu-id="e9410-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="e9410-150">Takové třídy entit, které jsou odděleny od rozhraní trvalosti, jsou také označovány jako "trvalé ignorování".</span><span class="sxs-lookup"><span data-stu-id="e9410-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="e9410-151">Inverzní relace</span><span class="sxs-lookup"><span data-stu-id="e9410-151">Relationship inverse</span></span>
<span data-ttu-id="e9410-152">Opačný konec relace, například produkt. Kategorie a kategorie. Produktu.</span><span class="sxs-lookup"><span data-stu-id="e9410-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="e9410-153">Entita pro sledování sebe</span><span class="sxs-lookup"><span data-stu-id="e9410-153">Self-tracking entity</span></span>
<span data-ttu-id="e9410-154">Entita vytvořená z šablony pro generování kódu, která pomáhá s vývojem na N-vrstvách.</span><span class="sxs-lookup"><span data-stu-id="e9410-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="e9410-155">Pro konkrétní typ tabulky (TPC)</span><span class="sxs-lookup"><span data-stu-id="e9410-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="e9410-156">Metoda mapování dědičnosti, kde jsou jednotlivé neabstraktní typy v hierarchii namapovány na samostatnou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="e9410-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="e9410-157">Tabulka na hierarchii (TPH)</span><span class="sxs-lookup"><span data-stu-id="e9410-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="e9410-158">Metoda mapování dědičnosti, kde jsou všechny typy v hierarchii namapovány na stejnou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="e9410-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="e9410-159">Ke zjištění, ke kterému typu jsou přidruženy jednotlivé řádky, se používají sloupce diskriminátorů.</span><span class="sxs-lookup"><span data-stu-id="e9410-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="e9410-160">Tabulka podle typu (TPT)</span><span class="sxs-lookup"><span data-stu-id="e9410-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="e9410-161">Metoda mapování dědičnosti, kde jsou společné vlastnosti všech typů v hierarchii namapovány na stejnou tabulku v databázi, ale vlastnosti jedinečné pro každý typ jsou namapovány na samostatnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="e9410-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="e9410-162">Zjišťování typů</span><span class="sxs-lookup"><span data-stu-id="e9410-162">Type discovery</span></span>
<span data-ttu-id="e9410-163">Proces určení typů, které by měly být součástí modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e9410-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
