---
title: Entity Framework glosář - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4e42e5870879524b814cecdc361e688d36f0180f
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131390"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="b85e0-102">Entity Framework – glosář</span><span class="sxs-lookup"><span data-stu-id="b85e0-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="b85e0-103">Kód nejprve</span><span class="sxs-lookup"><span data-stu-id="b85e0-103">Code First</span></span>
<span data-ttu-id="b85e0-104">Vytvoření modelu Entity Framework pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="b85e0-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="b85e0-105">Model můžete cílit na existující databázi nebo novou databázi.</span><span class="sxs-lookup"><span data-stu-id="b85e0-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="b85e0-106">Kontext</span><span class="sxs-lookup"><span data-stu-id="b85e0-106">Context</span></span>
<span data-ttu-id="b85e0-107">Třída, která představuje relaci s databází, umožňuje dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="b85e0-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="b85e0-108">Kontext je odvozena z třídy DbContext nebo objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="b85e0-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="b85e0-109">Konvence (Code First)</span><span class="sxs-lookup"><span data-stu-id="b85e0-109">Convention (Code First)</span></span>
<span data-ttu-id="b85e0-110">Pravidlo, které používá Entity Framework k odvození tvar model z vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="b85e0-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="b85e0-111">První databáze</span><span class="sxs-lookup"><span data-stu-id="b85e0-111">Database First</span></span>
<span data-ttu-id="b85e0-112">Vytvoření modelu Entity Framework, pomocí EF designeru, zaměřuje na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="b85e0-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="b85e0-113">Předběžné načítání</span><span class="sxs-lookup"><span data-stu-id="b85e0-113">Eager loading</span></span>
<span data-ttu-id="b85e0-114">Vzor načítání souvisejících dat, kde dotaz pro jeden typ entity se také načtou související entity jako součást dotazu.</span><span class="sxs-lookup"><span data-stu-id="b85e0-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="b85e0-115">EF designeru</span><span class="sxs-lookup"><span data-stu-id="b85e0-115">EF Designer</span></span>
<span data-ttu-id="b85e0-116">Vizuálního návrháře v sadě Visual Studio, která umožňuje vytvoření modelu Entity Framework pomocí polí a čáry.</span><span class="sxs-lookup"><span data-stu-id="b85e0-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="b85e0-117">Entity</span><span class="sxs-lookup"><span data-stu-id="b85e0-117">Entity</span></span>
<span data-ttu-id="b85e0-118">Třída nebo objekt, který představuje data aplikací, jako jsou zákazníci, výrobky a objednávky.</span><span class="sxs-lookup"><span data-stu-id="b85e0-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="b85e0-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="b85e0-119">Entity Data Model</span></span>
<span data-ttu-id="b85e0-120">Model, který popisuje entit a vztahů mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="b85e0-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="b85e0-121">EF používá k popisu konceptuální model, proti kterému EDM vývojářské programy.</span><span class="sxs-lookup"><span data-stu-id="b85e0-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="b85e0-122">EDM je založena na modelu relace Entity zavedené zotavení po havárii. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="b85e0-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="b85e0-123">EDM byla původně vyvinuta primárním cílem stávají common data model napříč sada pro vývojáře a server technologií od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="b85e0-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="b85e0-124">EDM slouží také jako součást protokolu OData.</span><span class="sxs-lookup"><span data-stu-id="b85e0-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="b85e0-125">explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="b85e0-125">Explicit loading</span></span>
<span data-ttu-id="b85e0-126">Vzor načítání souvisejících dat, kde jsou načteny související objekty voláním rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b85e0-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b85e0-127">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="b85e0-127">Fluent API</span></span>
<span data-ttu-id="b85e0-128">Rozhraní API, které lze použít ke konfiguraci modelu Code First.</span><span class="sxs-lookup"><span data-stu-id="b85e0-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="b85e0-129">Přidružení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="b85e0-129">Foreign key association</span></span>
<span data-ttu-id="b85e0-130">Přidružení mezi entitami, kde je vlastnost, která představuje klíč, cizí součástí třídy entity závislé.</span><span class="sxs-lookup"><span data-stu-id="b85e0-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="b85e0-131">Vlastnost ID kategorie obsahuje například produktu.</span><span class="sxs-lookup"><span data-stu-id="b85e0-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="b85e0-132">Určení vztahu</span><span class="sxs-lookup"><span data-stu-id="b85e0-132">Identifying relationship</span></span>
<span data-ttu-id="b85e0-133">Vztah, kde primární klíč entity, objektu zabezpečení je součástí primárního klíče entity závislé.</span><span class="sxs-lookup"><span data-stu-id="b85e0-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="b85e0-134">V tento druh vztahu závislé entity nemůže existovat bez instančního objektu entity.</span><span class="sxs-lookup"><span data-stu-id="b85e0-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="b85e0-135">Nezávislé přidružení</span><span class="sxs-lookup"><span data-stu-id="b85e0-135">Independent association</span></span>
<span data-ttu-id="b85e0-136">Přidružení mezi entitami tam, kde není žádná vlastnost představující cizí klíč ve třídě závislé entity.</span><span class="sxs-lookup"><span data-stu-id="b85e0-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="b85e0-137">Například třída produkt obsahuje vztah ke kategorii, ale žádná vlastnost ID kategorie.</span><span class="sxs-lookup"><span data-stu-id="b85e0-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="b85e0-138">Entity Framework sleduje stav přidružení bez ohledu na jejich stav entit na koncích dva přidružení.</span><span class="sxs-lookup"><span data-stu-id="b85e0-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="b85e0-139">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="b85e0-139">Lazy loading</span></span>
<span data-ttu-id="b85e0-140">Vzor načítání souvisejících dat, kde načteny související objekty jsou automaticky při přístupu k vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="b85e0-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="b85e0-141">První model</span><span class="sxs-lookup"><span data-stu-id="b85e0-141">Model First</span></span>
<span data-ttu-id="b85e0-142">Vytvoření modelu Entity Framework, pomocí EF designeru, který potom slouží k vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="b85e0-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="b85e0-143">Navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="b85e0-143">Navigation property</span></span>
<span data-ttu-id="b85e0-144">Vlastnosti entity, která odkazuje na jinou entitou.</span><span class="sxs-lookup"><span data-stu-id="b85e0-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="b85e0-145">Například produkt obsahuje vlastnost navigace kategorie a kategorie obsahuje produkty navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b85e0-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="b85e0-146">POCO</span><span class="sxs-lookup"><span data-stu-id="b85e0-146">POCO</span></span>
<span data-ttu-id="b85e0-147">Zkratka pro objekt CLR prostý staré.</span><span class="sxs-lookup"><span data-stu-id="b85e0-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="b85e0-148">Jednoduché uživatelské třídu, která nemá žádné závislosti pomocí libovolné architektury.</span><span class="sxs-lookup"><span data-stu-id="b85e0-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="b85e0-149">V rámci EF, třídu entity, která není odvozena od EntityObject, implementuje všechna rozhraní nebo představuje libovolné atributy definované v EF.</span><span class="sxs-lookup"><span data-stu-id="b85e0-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="b85e0-150">Takové entity třídy, které jsou oddělené od rozhraní trvalost se také označují jako "trvalost ignorant".</span><span class="sxs-lookup"><span data-stu-id="b85e0-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="b85e0-151">Inverzní relace</span><span class="sxs-lookup"><span data-stu-id="b85e0-151">Relationship inverse</span></span>
<span data-ttu-id="b85e0-152">V opačném směru relace, například produktu. Kategorie a kategorie. Produkt.</span><span class="sxs-lookup"><span data-stu-id="b85e0-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="b85e0-153">Vlastní sledování entity</span><span class="sxs-lookup"><span data-stu-id="b85e0-153">Self-tracking entity</span></span>
<span data-ttu-id="b85e0-154">Entity vytvořené ze šablony generování kódu, který pomáhá s N-Vrstvý vývoj.</span><span class="sxs-lookup"><span data-stu-id="b85e0-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="b85e0-155">Tabulky na konkrétní typ (TPC)</span><span class="sxs-lookup"><span data-stu-id="b85e0-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="b85e0-156">Metoda mapování dědičnosti, kde každý neabstraktního typu v hierarchii je mapována do samostatné tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="b85e0-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="b85e0-157">Za hierarchii tabulky (TPH)</span><span class="sxs-lookup"><span data-stu-id="b85e0-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="b85e0-158">Metoda mapování dědičnosti, kde jsou všechny typy v hierarchii namapovány na stejnou tabulku v databázi.</span><span class="sxs-lookup"><span data-stu-id="b85e0-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="b85e0-159">Diskriminátor sloupců, které se používá k určení, jaký typ každý řádek je přidružený.</span><span class="sxs-lookup"><span data-stu-id="b85e0-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="b85e0-160">Za typ tabulky (TPT)</span><span class="sxs-lookup"><span data-stu-id="b85e0-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="b85e0-161">Metoda mapování dědičnosti, kde běžné vlastnosti všech typů v hierarchii jsou namapovány na stejnou tabulku v databázi, ale vlastnosti jedinečné pro jednotlivé typy jsou mapovány do samostatné tabulky.</span><span class="sxs-lookup"><span data-stu-id="b85e0-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="b85e0-162">Typ zjišťování</span><span class="sxs-lookup"><span data-stu-id="b85e0-162">Type discovery</span></span>
<span data-ttu-id="b85e0-163">Proces identifikace typů, které by měla být součástí modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b85e0-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
