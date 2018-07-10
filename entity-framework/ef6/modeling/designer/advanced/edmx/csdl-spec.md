---
title: Specifikace CSDL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
caps.latest.revision: 3
ms.openlocfilehash: 0ece73a19fe7ea244905bccb728ab2a104c5179f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912862"
---
# <a name="csdl-specification"></a><span data-ttu-id="39601-102">Specifikace CSDL</span><span class="sxs-lookup"><span data-stu-id="39601-102">CSDL Specification</span></span>
<span data-ttu-id="39601-103">Konceptuální schéma definici jazyka (CSDL) je jazyk založený na formátu XML, který popisuje entity, vztahy a funkce, které tvoří konceptuálního modelu aplikace řízené daty.</span><span class="sxs-lookup"><span data-stu-id="39601-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="39601-104">Entity Framework nebo WCF Data Services je možné tento koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="39601-105">Metadata, která je popsána pomocí CSDL používá Entity Framework pro mapování entit a vztahů, které jsou definované v konceptuálním modelu na zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="39601-106">Další informace najdete v tématu [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) a [specifikace MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="39601-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="39601-107">Soubor CSDL je implementace Entity Framework modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="39601-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="39601-108">V aplikaci Entity Framework koncepčního modelu metadat je načteno ze souboru .csdl (napsaného v CSDL) do instance System.Data.Metadata.Edm.EdmItemCollection a je přístupný pomocí metod v Třída System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="39601-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="39601-109">Entity Framework používá metadata koncepčního modelu dotazy na koncepční model příkazů specifických pro zdroj dat převodu.</span><span class="sxs-lookup"><span data-stu-id="39601-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="39601-110">EF designeru ukládá informace o konceptuální model v souboru .edmx v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="39601-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="39601-111">V okamžiku sestavení Návrhář EF používá informace v souboru .edmx vytvořit soubor .csdl, který je potřeba pro Entity Framework za běhu.</span><span class="sxs-lookup"><span data-stu-id="39601-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="39601-112">Verze CSDL jsou rozlišené pomocí obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="39601-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="39601-113">Verze CSDL</span><span class="sxs-lookup"><span data-stu-id="39601-113">CSDL Version</span></span> | <span data-ttu-id="39601-114">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="39601-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="39601-115">Soubor CSDL v1</span><span class="sxs-lookup"><span data-stu-id="39601-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="39601-116">Soubor CSDL v2</span><span class="sxs-lookup"><span data-stu-id="39601-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="39601-117">Soubor CSDL v3</span><span class="sxs-lookup"><span data-stu-id="39601-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="39601-118">Element Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-118">Association Element (CSDL)</span></span>

<span data-ttu-id="39601-119">**Přidružení** element definuje vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="39601-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="39601-120">Asociace musíte zadat typy entit, které jsou zahrnuty v relaci a možný počet typů entit na každém konci vztahu, který je označován jako násobnost.</span><span class="sxs-lookup"><span data-stu-id="39601-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="39601-121">Násobnost zakončení přidružení může mít hodnotu jedna (1), žádný nebo jeden (0..1) nebo mnoho (\*).</span><span class="sxs-lookup"><span data-stu-id="39601-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="39601-122">Tato informace zadána v dva podřízené elementy End.</span><span class="sxs-lookup"><span data-stu-id="39601-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="39601-123">Instance typu entity na jednom konci asociace je přístupný prostřednictvím vlastnosti navigace nebo cizí klíče, pokud jsou zveřejněné na typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="39601-124">V aplikaci, která představuje instance přidružení konkrétní přidružení mezi instancemi typů entit.</span><span class="sxs-lookup"><span data-stu-id="39601-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="39601-125">Přidružení instance jsou logicky rozdělena do nastavení přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="39601-126">**Přidružení** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-127">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-128">End (přesně 2 prvky)</span><span class="sxs-lookup"><span data-stu-id="39601-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="39601-129">Elementu ReferentialConstraint (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="39601-130">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-131">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-131">Applicable Attributes</span></span>

<span data-ttu-id="39601-132">Následující tabulka popisuje atributy, které mohou být použity **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="39601-133">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-133">Attribute Name</span></span> | <span data-ttu-id="39601-134">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-134">Is Required</span></span> | <span data-ttu-id="39601-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="39601-136">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-136">**Name**</span></span>       | <span data-ttu-id="39601-137">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-137">Yes</span></span>         | <span data-ttu-id="39601-138">Název přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-139">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="39601-140">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-141">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-142">Example</span></span>

<span data-ttu-id="39601-143">Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení při cizí klíče dosud byl zpřístupněn na **zákazníka** a  **Pořadí** typy entit.</span><span class="sxs-lookup"><span data-stu-id="39601-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="39601-144">**Násobnost** hodnoty pro každou **koncový** přidružení označuje tolik **objednávky** lze přidružit **zákazníka**, ale pouze jeden **zákazníka** je možné přidružit **pořadí**.</span><span class="sxs-lookup"><span data-stu-id="39601-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="39601-145">Kromě toho **elementy OnDelete** element znamená, že všechny **objednávky** , která se vztahují ke konkrétní **zákazníka** a byla načtena do objektu ObjectContext se odstraní. Pokud **zákazníka** se odstraní.</span><span class="sxs-lookup"><span data-stu-id="39601-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="39601-146">Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení při cizí klíče mají byl zpřístupněn na **zákazníka** a  **Pořadí** typy entit.</span><span class="sxs-lookup"><span data-stu-id="39601-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="39601-147">S vystavený cizí klíče, je vztah mezi entitami spravované pomocí **elementu ReferentialConstraint** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="39601-148">Odpovídající prvek AssociationSetMapping není nutné mapovat tato přidružení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="39601-149">Element AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="39601-150">**AssociationSet** element v Konceptuální schéma definici jazyka (CSDL) je logický kontejner pro instance přiřazení stejného typu.</span><span class="sxs-lookup"><span data-stu-id="39601-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="39601-151">Skupinu přidružení obsahuje definici pro seskupení přidružení instance tak, aby se lze mapovat na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="39601-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="39601-152">**AssociationSet** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-153">Dokumentace ke službě (žádný nebo jeden prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="39601-154">End (vyžaduje právě dva elementy)</span><span class="sxs-lookup"><span data-stu-id="39601-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="39601-155">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="39601-156">**Přidružení** atribut určuje typ přidružení, které obsahuje sadu přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="39601-157">Sady entit, které tvoří konce sady přidružení se zadaným přesně dva podřízené **End** elementy.</span><span class="sxs-lookup"><span data-stu-id="39601-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-158">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-158">Applicable Attributes</span></span>

<span data-ttu-id="39601-159">Následující tabulka popisuje atributy, které mohou být použity **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="39601-160">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-160">Attribute Name</span></span>  | <span data-ttu-id="39601-161">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-161">Is Required</span></span> | <span data-ttu-id="39601-162">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-163">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-163">**Name**</span></span>        | <span data-ttu-id="39601-164">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-164">Yes</span></span>         | <span data-ttu-id="39601-165">Název sady entit.</span><span class="sxs-lookup"><span data-stu-id="39601-165">The name of the entity set.</span></span> <span data-ttu-id="39601-166">Hodnota **název** atribut nemůže být stejná jako hodnota **přidružení** atribut.</span><span class="sxs-lookup"><span data-stu-id="39601-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="39601-167">**Přidružení**</span><span class="sxs-lookup"><span data-stu-id="39601-167">**Association**</span></span> | <span data-ttu-id="39601-168">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-168">Yes</span></span>         | <span data-ttu-id="39601-169">Plně kvalifikovaný název přidružení, na kterou přidružení nastavení obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="39601-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="39601-170">Přidružení musí být ve stejném oboru názvů jako sada přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-171">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="39601-172">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-173">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-174">Example</span></span>

<span data-ttu-id="39601-175">Následující příklad ukazuje **EntityContainer** element se dvěma **AssociationSet** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="39601-176">Element CollectionType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="39601-177">**CollectionType** element v Konceptuální schéma definici jazyka (CSDL) určuje, že parametr funkce nebo funkce, návratový typ je kolekce.</span><span class="sxs-lookup"><span data-stu-id="39601-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="39601-178">**CollectionType** element může být podřízený element parametru nebo element ReturnType (funkce).</span><span class="sxs-lookup"><span data-stu-id="39601-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="39601-179">Typ kolekce se dá nastavit pomocí **typ** atribut nebo jednu z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="39601-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="39601-180">**Objekt CollectionType**</span><span class="sxs-lookup"><span data-stu-id="39601-180">**CollectionType**</span></span>
-   <span data-ttu-id="39601-181">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="39601-181">ReferenceType</span></span>
-   <span data-ttu-id="39601-182">RowType</span><span class="sxs-lookup"><span data-stu-id="39601-182">RowType</span></span>
-   <span data-ttu-id="39601-183">Odkaz TypeRef</span><span class="sxs-lookup"><span data-stu-id="39601-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="39601-184">Model neověří, pokud je zadán typ kolekce s oběma **typ** atribut a podřízený element.</span><span class="sxs-lookup"><span data-stu-id="39601-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="39601-185">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-185">Applicable Attributes</span></span>

<span data-ttu-id="39601-186">Následující tabulka popisuje atributy, které mohou být použity **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="39601-187">Všimněte si, že **DefaultValue**, **MaxLength**, **FixedLength**, **přesnost**, **škálování**,  **Kódování Unicode**, a **kolace** atributy platí pouze pro kolekce **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="39601-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="39601-188">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-188">Attribute Name</span></span>                                                          | <span data-ttu-id="39601-189">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-189">Is Required</span></span> | <span data-ttu-id="39601-190">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-191">**Type**</span></span>                                                                | <span data-ttu-id="39601-192">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-192">No</span></span>          | <span data-ttu-id="39601-193">Typ kolekce.</span><span class="sxs-lookup"><span data-stu-id="39601-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="39601-194">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="39601-194">**Nullable**</span></span>                                                            | <span data-ttu-id="39601-195">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-195">No</span></span>          | <span data-ttu-id="39601-196">**Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39601-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="39601-197">> V CSDL v1, musí mít vlastnost komplexní typ `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="39601-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="39601-198">**Výchozí hodnota**</span><span class="sxs-lookup"><span data-stu-id="39601-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="39601-199">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-199">No</span></span>          | <span data-ttu-id="39601-200">Výchozí hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="39601-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="39601-202">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-202">No</span></span>          | <span data-ttu-id="39601-203">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="39601-204">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="39601-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="39601-205">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-205">No</span></span>          | <span data-ttu-id="39601-206">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.</span><span class="sxs-lookup"><span data-stu-id="39601-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="39601-207">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-207">**Precision**</span></span>                                                           | <span data-ttu-id="39601-208">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-208">No</span></span>          | <span data-ttu-id="39601-209">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="39601-210">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-210">**Scale**</span></span>                                                               | <span data-ttu-id="39601-211">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-211">No</span></span>          | <span data-ttu-id="39601-212">Měřítko pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="39601-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-213">**SRID**</span></span>                                                                | <span data-ttu-id="39601-214">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-214">No</span></span>          | <span data-ttu-id="39601-215">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="39601-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="39601-216">Platí pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="39601-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="39601-217">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="39601-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="39601-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="39601-218">**Unicode**</span></span>                                                             | <span data-ttu-id="39601-219">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-219">No</span></span>          | <span data-ttu-id="39601-220">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.</span><span class="sxs-lookup"><span data-stu-id="39601-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="39601-221">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="39601-221">**Collation**</span></span>                                                           | <span data-ttu-id="39601-222">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-222">No</span></span>          | <span data-ttu-id="39601-223">Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="39601-224">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="39601-225">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-226">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-227">Example</span></span>

<span data-ttu-id="39601-228">Následující příklad ukazuje funkce definované v modelu, která, která používá **CollectionType** elementu k určení, že funkce vrátí kolekci **osoba** typy entit (jako zadaný **ElementType** atributu).</span><span class="sxs-lookup"><span data-stu-id="39601-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="39601-229">Následující příklad ukazuje funkce definované v modelu, která používá **CollectionType** elementu k určení, že funkce vrátí kolekce řádků (jak je uvedeno v **RowType** element).</span><span class="sxs-lookup"><span data-stu-id="39601-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="39601-230">Následující příklad ukazuje funkce definované v modelu, která používá **CollectionType** elementu k určení, že funkce přijímá jako parametr kolekci **oddělení** typy entit.</span><span class="sxs-lookup"><span data-stu-id="39601-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="39601-231">Element ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="39601-232">A **ComplexType** element definuje datová struktura tvořený **EdmSimpleType** vlastnosti nebo jiné komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="39601-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="39601-233">Komplexní typ může být vlastnost typu entity nebo jiného komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="39601-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="39601-234">Komplexní typ je podobný typ entity, komplexní typ definuje data.</span><span class="sxs-lookup"><span data-stu-id="39601-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="39601-235">Existují však některé hlavní rozdíly mezi typy entit a komplexní typy:</span><span class="sxs-lookup"><span data-stu-id="39601-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="39601-236">Komplexní typy nemají identit (nebo klíče) a proto nemůže existovat nezávisle na sobě.</span><span class="sxs-lookup"><span data-stu-id="39601-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="39601-237">Komplexní typy může existovat pouze jako typy entit nebo jiné komplexní typy vlastností.</span><span class="sxs-lookup"><span data-stu-id="39601-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="39601-238">Komplexní typy se nemůže podílet na přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="39601-239">Ani end přidružení může být komplexní typ a proto nejde definovat navigační vlastnosti pro komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="39601-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="39601-240">Vlastnost komplexní typ nemůže mít hodnotu null, i když Skalární vlastnosti složitého typu každý je možné nastavit na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39601-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="39601-241">A **ComplexType** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-242">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-243">Vlastnosti (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="39601-244">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="39601-245">Následující tabulka popisuje atributy, které mohou být použity **ComplexType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="39601-246">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="39601-247">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-247">Is Required</span></span> | <span data-ttu-id="39601-248">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-249">Název</span><span class="sxs-lookup"><span data-stu-id="39601-249">Name</span></span>                                                                                                           | <span data-ttu-id="39601-250">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-250">Yes</span></span>         | <span data-ttu-id="39601-251">Název komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="39601-251">The name of the complex type.</span></span> <span data-ttu-id="39601-252">Název komplexního typu nemůže být stejný jako název jiného komplexní typ, typ entity nebo přidružení, které je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="39601-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="39601-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="39601-254">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-254">No</span></span>          | <span data-ttu-id="39601-255">Název jiného komplexní typ, který je základním typem komplexní typ, který se zrovna definuje.</span><span class="sxs-lookup"><span data-stu-id="39601-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="39601-256">> Tento atribut není platných CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="39601-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="39601-257">V této verzi nepodporuje dědičnosti pro komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="39601-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="39601-258">Abstraktní</span><span class="sxs-lookup"><span data-stu-id="39601-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="39601-259">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-259">No</span></span>          | <span data-ttu-id="39601-260">**Hodnota TRUE** nebo **False** (výchozí hodnota) v závislosti na tom, zda komplexní typ je typ abstraktní.</span><span class="sxs-lookup"><span data-stu-id="39601-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="39601-261">> Tento atribut není platných CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="39601-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="39601-262">Komplexní typy v této verzi nemohou být abstraktní typy.</span><span class="sxs-lookup"><span data-stu-id="39601-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="39601-263">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ComplexType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="39601-264">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-265">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-266">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-266">Example</span></span>

<span data-ttu-id="39601-267">Následující příklad ukazuje komplexní typ, **adresu**, se **EdmSimpleType** vlastnosti **StreetAddress**, **Město**,  **StátNeboKraj**, **země**, a **PSČ**.</span><span class="sxs-lookup"><span data-stu-id="39601-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="39601-268">Chcete-li definovat komplexní typ **adresu** (výše) jako vlastnost typu entity, je třeba deklarovat typ vlastnosti v definici typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="39601-269">Následující příklad ukazuje **adresu** vlastnost jako komplexní typ na typ entity (**vydavatele**):</span><span class="sxs-lookup"><span data-stu-id="39601-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="39601-270">Element DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="39601-271">**DefiningExpression** element v Konceptuální schéma definici jazyka (CSDL) obsahuje Entity SQL výraz, který definuje funkci v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="39601-272">Pro účely ověření **DefiningExpression** element může obsahovat libovolný obsah.</span><span class="sxs-lookup"><span data-stu-id="39601-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="39601-273">Nicméně, Entity Framework vyvolá výjimku za běhu Pokud **DefiningExpression** element neobsahuje platný Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="39601-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="39601-274">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-274">Applicable Attributes</span></span>

<span data-ttu-id="39601-275">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **DefiningExpression** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="39601-276">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-277">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="39601-278">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-278">Example</span></span>

<span data-ttu-id="39601-279">Následující příklad používá **DefiningExpression** prvek, který chcete definovat funkci, která vrátí počet roků, protože knize publikoval.</span><span class="sxs-lookup"><span data-stu-id="39601-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="39601-280">Obsah **DefiningExpression** element je napsána v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="39601-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="39601-281">Závislé – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="39601-282">**Závislé** element v Konceptuální schéma definici jazyka (CSDL) je podřízeným prvkem prvku elementu ReferentialConstraint a definuje závislé konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="39601-283">A **elementu ReferentialConstraint** element definuje funkci, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="39601-284">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč z jiné tabulky lze vlastnost (nebo vlastnosti) typu entity odkazují na klíč entity, která jiný typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="39601-285">Je volána typ entity, na který odkazuje *hlavní koncový* omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="39601-286">Typ entity, která odkazuje na konci instančního objektu je volána *závislé end* omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="39601-287">**PropertyRef** elementy se používají k určení klíče, které odkazují na konci instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="39601-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="39601-288">**Závislé** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-289">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="39601-290">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-291">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-291">Applicable Attributes</span></span>

<span data-ttu-id="39601-292">Následující tabulka popisuje atributy, které mohou být použity **závislé** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="39601-293">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-293">Attribute Name</span></span> | <span data-ttu-id="39601-294">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-294">Is Required</span></span> | <span data-ttu-id="39601-295">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="39601-296">**Role**</span><span class="sxs-lookup"><span data-stu-id="39601-296">**Role**</span></span>       | <span data-ttu-id="39601-297">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-297">Yes</span></span>         | <span data-ttu-id="39601-298">Název typu entity na závislé straně asociace.</span><span class="sxs-lookup"><span data-stu-id="39601-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-299">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **závislé** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="39601-300">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-301">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-302">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-302">Example</span></span>

<span data-ttu-id="39601-303">Následující příklad ukazuje **elementu ReferentialConstraint** element se používá jako součást definice **PublishedBy** přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="39601-304">**PublisherId** vlastnost **knihy** typ entity tvoří závislé konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="39601-305">Element Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="39601-306">**Dokumentaci** element v Konceptuální schéma definici jazyka (CSDL) slouží k zadání informací o objektu, který je definován v nadřazeném elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="39601-307">V souboru .edmx při **dokumentaci** element je podřízeným prvkem elementu, který se zobrazí jako objekt na návrhové ploše návrháře EF (například entit, přidružení nebo vlastnost), obsah **dokumentace**  prvek se zobrazí v sadě Visual Studio **vlastnosti** okna pro objekt.</span><span class="sxs-lookup"><span data-stu-id="39601-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="39601-308">**Dokumentaci** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-309">**Souhrn**: stručný popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="39601-310">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-310">(zero or one element)</span></span>
-   <span data-ttu-id="39601-311">**LongDescription**: rozsáhlý popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="39601-312">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-312">(zero or one element)</span></span>
-   <span data-ttu-id="39601-313">Elementů poznámky.</span><span class="sxs-lookup"><span data-stu-id="39601-313">Annotation elements.</span></span> <span data-ttu-id="39601-314">(nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-315">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-315">Applicable Attributes</span></span>

<span data-ttu-id="39601-316">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **dokumentaci** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="39601-317">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-318">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="39601-319">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-319">Example</span></span>

<span data-ttu-id="39601-320">Následující příklad ukazuje **dokumentaci** element jako podřízený prvek elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="39601-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="39601-321">Následující fragment kódu byl v CSDL obsah edmx soubor, obsah **Souhrn** a **LongDescription** prvky objevuje v aplikaci Visual Studio **vlastnosti** okno po kliknutí na `Customer` typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="39601-322">Element end (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-322">End Element (CSDL)</span></span>

<span data-ttu-id="39601-323">**End** element v Konceptuální schéma definici jazyka (CSDL) může být podřízeným prvkem elementu Association elementu nebo elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="39601-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="39601-324">V každém případě role **End** element se liší a příslušné atributy se liší.</span><span class="sxs-lookup"><span data-stu-id="39601-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="39601-325">Koncový Element jako podřízený Element přidružení</span><span class="sxs-lookup"><span data-stu-id="39601-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="39601-326">**End** – element (jako podřízený objekt **přidružení** element) identifikuje typ entity na jednom konci asociace a počtu instancí typu entity, které mohou existovat za tímto účelem přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="39601-327">Zakončení jsou definované jako součást přidružení; přidružení musí mít přesně dva elementy přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="39601-328">Instance typu entity na jednom konci asociace lze přistupovat prostřednictvím vlastnosti navigace nebo cizí klíče, pokud jsou zveřejněné na typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="39601-329">**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-330">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-331">Elementy OnDelete (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="39601-332">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="39601-333">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-333">Applicable Attributes</span></span>

<span data-ttu-id="39601-334">Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="39601-335">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-335">Attribute Name</span></span>   | <span data-ttu-id="39601-336">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-336">Is Required</span></span> | <span data-ttu-id="39601-337">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-338">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-338">**Type**</span></span>         | <span data-ttu-id="39601-339">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-339">Yes</span></span>         | <span data-ttu-id="39601-340">Název typu entity na jednom konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="39601-341">**Role**</span><span class="sxs-lookup"><span data-stu-id="39601-341">**Role**</span></span>         | <span data-ttu-id="39601-342">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-342">No</span></span>          | <span data-ttu-id="39601-343">Název přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-343">A name for the association end.</span></span> <span data-ttu-id="39601-344">Pokud není název zadán, použije se název typu entity na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="39601-345">**Násobnost**</span><span class="sxs-lookup"><span data-stu-id="39601-345">**Multiplicity**</span></span> | <span data-ttu-id="39601-346">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-346">Yes</span></span>         | <span data-ttu-id="39601-347">**1**, **0..1**, nebo **\*** v závislosti na počtu instancí typu entity, které mohou být na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="39601-348">**1** znamená tuto instanci typu přesně jedna entita existuje na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="39601-349">**0..1** udává, že existuje žádnou nebo jednou instancí typu entity na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="39601-350">**\*** Udává, že existuje žádného, jednoho nebo více instancí typu entity na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-351">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="39601-352">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-353">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="39601-354">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-354">Example</span></span>

<span data-ttu-id="39601-355">Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="39601-356">**Násobnost** hodnoty pro každou **koncový** přidružení označuje tolik **objednávky** lze přidružit **zákazníka**, ale pouze jeden **zákazníka** je možné přidružit **pořadí**.</span><span class="sxs-lookup"><span data-stu-id="39601-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="39601-357">Kromě toho **elementy OnDelete** element znamená, že všechny **objednávky** , která se vztahují ke konkrétní **zákazníka** a, která byla načtena do objektu ObjectContext bude odstraněné if **zákazníka** se odstraní.</span><span class="sxs-lookup"><span data-stu-id="39601-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="39601-358">Koncový Element jako podřízený AssociationSet Element</span><span class="sxs-lookup"><span data-stu-id="39601-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="39601-359">**End** jednom konci sady přidružení určuje element.</span><span class="sxs-lookup"><span data-stu-id="39601-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="39601-360">**AssociationSet** element musí obsahovat dvě **End** elementy.</span><span class="sxs-lookup"><span data-stu-id="39601-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="39601-361">Informace obsažené v **End** element je použít v mapování sada ke zdroji dat přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="39601-362">**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-363">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-364">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="39601-365">Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="39601-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="39601-366">Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="39601-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="39601-367">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-367">Applicable Attributes</span></span>

<span data-ttu-id="39601-368">Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="39601-369">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-369">Attribute Name</span></span> | <span data-ttu-id="39601-370">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-370">Is Required</span></span> | <span data-ttu-id="39601-371">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-372">**Objekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="39601-372">**EntitySet**</span></span>  | <span data-ttu-id="39601-373">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-373">Yes</span></span>         | <span data-ttu-id="39601-374">Název **objektu EntitySet** element, který definuje jeden konec nadřazené **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="39601-375">**Objektu EntitySet** elementu musí být definován ve stejném kontejneru jako nadřazená entita **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="39601-376">**Role**</span><span class="sxs-lookup"><span data-stu-id="39601-376">**Role**</span></span>       | <span data-ttu-id="39601-377">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-377">No</span></span>          | <span data-ttu-id="39601-378">Název přidružení, nastavit konec.</span><span class="sxs-lookup"><span data-stu-id="39601-378">The name of the association set end.</span></span> <span data-ttu-id="39601-379">Pokud **Role** atribut není použit, název elementu end přidružení sady bude název sady entit.</span><span class="sxs-lookup"><span data-stu-id="39601-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="39601-380">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="39601-381">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-382">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="39601-383">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-383">Example</span></span>

<span data-ttu-id="39601-384">Následující příklad ukazuje **EntityContainer** element se dvěma **AssociationSet** prvky, každého se dvěma **koncové** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="39601-385">Element EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="39601-386">**EntityContainer** prvek Konceptuální schéma definici jazyka (CSDL) je logický kontejner sady entit, sad přidružení nebo importované funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="39601-387">Kontejner entit koncepčního modelu mapuje na kontejner úložiště modelu entity pomocí elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="39601-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="39601-388">Struktura databáze popisuje kontejner úložiště modelu entity: sady entit popisují tabulky sad přidružení popisují omezení cizího klíče a importované funkce popisují uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="39601-389">**EntityContainer** element může mít nejvýše jedno dokumentace prvků.</span><span class="sxs-lookup"><span data-stu-id="39601-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="39601-390">Pokud **dokumentaci** element je k dispozici, musí být všechny **objektu EntitySet**, **AssociationSet**, a **element FunctionImport** elementy.</span><span class="sxs-lookup"><span data-stu-id="39601-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="39601-391">**EntityContainer** prvek může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-392">Objekt EntitySet</span><span class="sxs-lookup"><span data-stu-id="39601-392">EntitySet</span></span>
-   <span data-ttu-id="39601-393">Element AssociationSet</span><span class="sxs-lookup"><span data-stu-id="39601-393">AssociationSet</span></span>
-   <span data-ttu-id="39601-394">Element FunctionImport</span><span class="sxs-lookup"><span data-stu-id="39601-394">FunctionImport</span></span>
-   <span data-ttu-id="39601-395">Elementů poznámky</span><span class="sxs-lookup"><span data-stu-id="39601-395">Annotation elements</span></span>

<span data-ttu-id="39601-396">Můžete rozšířit **EntityContainer** prvek, který chcete zahrnout obsah jiné **EntityContainer** , který je v rámci stejného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="39601-397">Zahrnout obsah jiné **EntityContainer**, v referenčních **EntityContainer** elementu, nastavte hodnotu **rozšiřuje** atribut name  **EntityContainer** element, který chcete zahrnout.</span><span class="sxs-lookup"><span data-stu-id="39601-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="39601-398">Všechny podřízené prvky prvku zahrnutou **EntityContainer** elementu, bude zacházeno jako podřízené prvky prvku odkazujícího **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-399">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-399">Applicable Attributes</span></span>

<span data-ttu-id="39601-400">Následující tabulka popisuje atributy, které mohou být použity **použití** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="39601-401">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-401">Attribute Name</span></span> | <span data-ttu-id="39601-402">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-402">Is Required</span></span> | <span data-ttu-id="39601-403">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="39601-404">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-404">**Name**</span></span>       | <span data-ttu-id="39601-405">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-405">Yes</span></span>         | <span data-ttu-id="39601-406">Název kontejneru entity.</span><span class="sxs-lookup"><span data-stu-id="39601-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="39601-407">**Rozšiřuje**</span><span class="sxs-lookup"><span data-stu-id="39601-407">**Extends**</span></span>    | <span data-ttu-id="39601-408">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-408">No</span></span>          | <span data-ttu-id="39601-409">Název jiný kontejner entity v rámci stejného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-410">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="39601-411">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-412">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-413">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-413">Example</span></span>

<span data-ttu-id="39601-414">Následující příklad ukazuje **EntityContainer** element, který definuje tři sad entit a sad přidružení dvě.</span><span class="sxs-lookup"><span data-stu-id="39601-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="39601-415">Element EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="39601-416">**Objektu EntitySet** element v Konceptuální schéma definici jazyka je logický kontejner pro instance typu entity a instance typu, který je odvozený z tohoto typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="39601-417">Vztah mezi typem entity a sady entit je obdobou vztah mezi řádek a tabulky v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="39601-418">Stejně jako řádek typ entity, který definuje množinu souvisejících dat a jako tabulka, obsahuje sadu entit instance definice.</span><span class="sxs-lookup"><span data-stu-id="39601-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="39601-419">Sadu entit poskytuje konstrukci pro tak, aby může být namapována na související datové struktury ve zdroji dat seskupení instancí typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="39601-420">Může být definován více než jednu sadu entit pro typ konkrétní entity.</span><span class="sxs-lookup"><span data-stu-id="39601-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-421">EF designeru nepodporuje koncepčními modely, které obsahují více sad entit podle typu.</span><span class="sxs-lookup"><span data-stu-id="39601-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="39601-422">**Objektu EntitySet** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-423">Element Documentation (žádný nebo jeden prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="39601-424">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-425">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-425">Applicable Attributes</span></span>

<span data-ttu-id="39601-426">Následující tabulka popisuje atributy, které mohou být použity **objektu EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="39601-427">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-427">Attribute Name</span></span> | <span data-ttu-id="39601-428">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-428">Is Required</span></span> | <span data-ttu-id="39601-429">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-430">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-430">**Name**</span></span>       | <span data-ttu-id="39601-431">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-431">Yes</span></span>         | <span data-ttu-id="39601-432">Název sady entit.</span><span class="sxs-lookup"><span data-stu-id="39601-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="39601-433">**Typ entity**</span><span class="sxs-lookup"><span data-stu-id="39601-433">**EntityType**</span></span> | <span data-ttu-id="39601-434">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-434">Yes</span></span>         | <span data-ttu-id="39601-435">Plně kvalifikovaný název typu entity, pro kterou sada entit obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="39601-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-436">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **objektu EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="39601-437">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-438">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-439">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-439">Example</span></span>

<span data-ttu-id="39601-440">Následující příklad ukazuje **EntityContainer** element s tři **objektu EntitySet** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="39601-441">Je možné definovat více sad entit podle typu (MEST).</span><span class="sxs-lookup"><span data-stu-id="39601-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="39601-442">Následující příklad definuje kontejner služby entity se dvěma sadami entity pro **knihy** typ entity:</span><span class="sxs-lookup"><span data-stu-id="39601-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="39601-443">Element EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="39601-444">**EntityType** element reprezentuje strukturu nejvyšší úrovně konceptů, jako je například zákazník nebo pořadí, v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="39601-445">Typ entity je šablona pro instance typů entit v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="39601-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="39601-446">Každá šablona obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="39601-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="39601-447">Jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="39601-447">A unique name.</span></span> <span data-ttu-id="39601-448">(Povinné).</span><span class="sxs-lookup"><span data-stu-id="39601-448">(Required.)</span></span>
-   <span data-ttu-id="39601-449">Klíč entity, který je definován jeden nebo více vlastností.</span><span class="sxs-lookup"><span data-stu-id="39601-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="39601-450">(Povinné).</span><span class="sxs-lookup"><span data-stu-id="39601-450">(Required.)</span></span>
-   <span data-ttu-id="39601-451">Vlastnosti obsahující data.</span><span class="sxs-lookup"><span data-stu-id="39601-451">Properties for containing data.</span></span> <span data-ttu-id="39601-452">(Volitelné).</span><span class="sxs-lookup"><span data-stu-id="39601-452">(Optional.)</span></span>
-   <span data-ttu-id="39601-453">Navigační vlastnosti, které umožňují navigace z jednoho konce asociace na druhém konci.</span><span class="sxs-lookup"><span data-stu-id="39601-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="39601-454">(Volitelné).</span><span class="sxs-lookup"><span data-stu-id="39601-454">(Optional.)</span></span>

<span data-ttu-id="39601-455">V aplikaci, která představuje instance typu entity na konkrétní objekt (například konkrétního zákazníka nebo pořadí).</span><span class="sxs-lookup"><span data-stu-id="39601-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="39601-456">Každá instance typu entity musí mít jedinečné entity klíč v rámci sady entit.</span><span class="sxs-lookup"><span data-stu-id="39601-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="39601-457">Dvě instance typu entity se považovat za rovné pouze v případě, že jsou stejného typu a hodnoty jejich klíče entity jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="39601-458">**EntityType** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-459">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-460">Klíč (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="39601-461">Vlastnosti (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="39601-462">Objekt NavigationProperty (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="39601-463">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-464">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-464">Applicable Attributes</span></span>

<span data-ttu-id="39601-465">Následující tabulka popisuje atributy, které mohou být použity **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="39601-466">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="39601-467">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-467">Is Required</span></span> | <span data-ttu-id="39601-468">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-469">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="39601-470">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-470">Yes</span></span>         | <span data-ttu-id="39601-471">Název typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="39601-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="39601-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="39601-473">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-473">No</span></span>          | <span data-ttu-id="39601-474">Název jiného typu entity, která je základní typ, který definuje typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="39601-475">**Abstraktní**</span><span class="sxs-lookup"><span data-stu-id="39601-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="39601-476">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-476">No</span></span>          | <span data-ttu-id="39601-477">**Hodnota TRUE** nebo **False**, v závislosti na tom, jestli je typ entity abstraktní typ.</span><span class="sxs-lookup"><span data-stu-id="39601-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="39601-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="39601-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="39601-479">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-479">No</span></span>          | <span data-ttu-id="39601-480">**Hodnota TRUE** nebo **False** v závislosti na tom, zda typ entity je typu open entity.</span><span class="sxs-lookup"><span data-stu-id="39601-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="39601-481">> Tím **OpenType** atribut platí pouze pro typy entit, které jsou definovány v konceptuálních modelů, které se používají s využitím ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="39601-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="39601-482">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="39601-483">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-484">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-485">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-485">Example</span></span>

<span data-ttu-id="39601-486">Následující příklad ukazuje **EntityType** element s tři **vlastnost** elementy a dva **NavigationProperty** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="39601-487">Element EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="39601-488">**EnumType** představuje Výčtový typ elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="39601-489">**EnumType** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-490">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-491">Člen (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="39601-492">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-493">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-493">Applicable Attributes</span></span>

<span data-ttu-id="39601-494">Následující tabulka popisuje atributy, které mohou být použity **EnumType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="39601-495">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-495">Attribute Name</span></span>     | <span data-ttu-id="39601-496">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-496">Is Required</span></span> | <span data-ttu-id="39601-497">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-498">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-498">**Name**</span></span>           | <span data-ttu-id="39601-499">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-499">Yes</span></span>         | <span data-ttu-id="39601-500">Název typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="39601-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="39601-501">**IsFlags**</span></span>        | <span data-ttu-id="39601-502">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-502">No</span></span>          | <span data-ttu-id="39601-503">**Hodnota TRUE** nebo **False**, v závislosti na tom, zda typ výčtu může sloužit jako sada příznaků.</span><span class="sxs-lookup"><span data-stu-id="39601-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="39601-504">Výchozí hodnota je **hodnotu False.**.</span><span class="sxs-lookup"><span data-stu-id="39601-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="39601-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="39601-505">**UnderlyingType**</span></span> | <span data-ttu-id="39601-506">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-506">No</span></span>          | <span data-ttu-id="39601-507">**Edm.Byte**, **Edm.Int16**, **typem Edm.Int32**, **Edm.Int64** nebo **Edm.SByte** definování rozsahu hodnot typu.</span><span class="sxs-lookup"><span data-stu-id="39601-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="39601-508">Výchozí základní typ výčtu prvků je **typem Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="39601-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-509">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EnumType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="39601-510">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-511">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-512">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-512">Example</span></span>

<span data-ttu-id="39601-513">Následující příklad ukazuje **EnumType** element s tři **člen** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="39601-514">Element Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-514">Function Element (CSDL)</span></span>

<span data-ttu-id="39601-515">**Funkce** element v Konceptuální schéma definici jazyka (CSDL) se používá k definování nebo deklarace funkcí v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="39601-516">Funkce je definován pomocí DefiningExpression elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="39601-517">A **funkce** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-518">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-519">Parametr (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="39601-520">DefiningExpression (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="39601-521">Vlastnost ReturnType (Function) (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="39601-522">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="39601-523">Návratová typ pro funkci musí být určen buď **ReturnType** elementu (Function) nebo **ReturnType** atribut (viz níže), ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="39601-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="39601-524">Je to možné návratové typy jsou všechny EdmSimpleType, entity typu, komplexní typ, typ řádku, nebo Typ ref (nebo kolekce jednoho z těchto typů).</span><span class="sxs-lookup"><span data-stu-id="39601-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-525">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-525">Applicable Attributes</span></span>

<span data-ttu-id="39601-526">Následující tabulka popisuje atributy, které mohou být použity **funkce** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="39601-527">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-527">Attribute Name</span></span> | <span data-ttu-id="39601-528">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-528">Is Required</span></span> | <span data-ttu-id="39601-529">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="39601-530">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-530">**Name**</span></span>       | <span data-ttu-id="39601-531">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-531">Yes</span></span>         | <span data-ttu-id="39601-532">Název funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-532">The name of the function.</span></span>          |
| <span data-ttu-id="39601-533">**Vlastnost ReturnType**</span><span class="sxs-lookup"><span data-stu-id="39601-533">**ReturnType**</span></span> | <span data-ttu-id="39601-534">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-534">No</span></span>          | <span data-ttu-id="39601-535">Typ vrácené funkcí.</span><span class="sxs-lookup"><span data-stu-id="39601-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-536">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **funkce** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="39601-537">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-538">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-539">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-539">Example</span></span>

<span data-ttu-id="39601-540">Následující příklad používá **funkce** prvek, který chcete definovat funkci, která vrátí počet roků, protože byl přijat instruktorem.</span><span class="sxs-lookup"><span data-stu-id="39601-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="39601-541">Element FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="39601-542">**Element FunctionImport** prvek v Konceptuální schéma definici jazyka (CSDL) představuje funkci, která je definovaná ve zdroji dat, ale k dispozici pro objekty prostřednictvím konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="39601-543">Například funkce elementu v modelu úložiště slouží k reprezentaci uloženou proceduru v databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="39601-544">A **element FunctionImport** element v konceptuálním modelu představuje odpovídající funkce v aplikaci Entity Framework a je namapována na funkci úložiště modelu s použitím elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="39601-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="39601-545">Při volání funkce v aplikaci, spustit je odpovídající uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="39601-546">**Element FunctionImport** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-547">Dokumentace ke službě (žádný nebo jeden prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="39601-548">Parametr (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="39601-549">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="39601-550">Vlastnost ReturnType (element FunctionImport) (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="39601-551">Jeden **parametr** element by měl být definován pro každý parametr, který přijímá funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="39601-552">Návratová typ pro funkci musí být určen buď **ReturnType** – element (element FunctionImport) nebo **ReturnType** atribut (viz níže), ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="39601-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="39601-553">Hodnota návratový typ musí být kolekce EdmSimpleType, EntityType nebo ComplexType.</span><span class="sxs-lookup"><span data-stu-id="39601-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-554">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-554">Applicable Attributes</span></span>

<span data-ttu-id="39601-555">Následující tabulka popisuje atributy, které mohou být použity **element FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="39601-556">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-556">Attribute Name</span></span>   | <span data-ttu-id="39601-557">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-557">Is Required</span></span> | <span data-ttu-id="39601-558">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-559">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-559">**Name**</span></span>         | <span data-ttu-id="39601-560">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-560">Yes</span></span>         | <span data-ttu-id="39601-561">Název importované funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="39601-562">**Vlastnost ReturnType**</span><span class="sxs-lookup"><span data-stu-id="39601-562">**ReturnType**</span></span>   | <span data-ttu-id="39601-563">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-563">No</span></span>          | <span data-ttu-id="39601-564">Typ, který funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="39601-564">The type that the function returns.</span></span> <span data-ttu-id="39601-565">Tento atribut nepoužívejte, pokud funkce nevrací hodnotu.</span><span class="sxs-lookup"><span data-stu-id="39601-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="39601-566">V opačném případě hodnota musí být kolekce ComplexType, EntityType nebo EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="39601-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="39601-567">**Objekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="39601-567">**EntitySet**</span></span>    | <span data-ttu-id="39601-568">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-568">No</span></span>          | <span data-ttu-id="39601-569">Pokud funkce vrátí kolekci entit typy, hodnota **objektu EntitySet** musí být sada entit, ke které patří do kolekce.</span><span class="sxs-lookup"><span data-stu-id="39601-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="39601-570">V opačném případě **objektu EntitySet** atribut nesmí být použit.</span><span class="sxs-lookup"><span data-stu-id="39601-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="39601-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="39601-571">**IsComposable**</span></span> | <span data-ttu-id="39601-572">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-572">No</span></span>          | <span data-ttu-id="39601-573">Pokud je hodnota nastavena na hodnotu true, funkce je složení (funkce vracející tabulku) a lze použít v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="39601-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="39601-574">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="39601-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="39601-575">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **element FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="39601-576">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-577">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-578">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-578">Example</span></span>

<span data-ttu-id="39601-579">Následující příklad ukazuje **element FunctionImport** element, který přijímá jeden parametr a vrátí kolekci typů entit:</span><span class="sxs-lookup"><span data-stu-id="39601-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="39601-580">Klíčovým prvkem (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-580">Key Element (CSDL)</span></span>

<span data-ttu-id="39601-581">**Klíč** element je podřízeným prvkem elementu EntityType a definuje *klíč entity* (vlastnost nebo sadu vlastností typu entity, které určují identity).</span><span class="sxs-lookup"><span data-stu-id="39601-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="39601-582">Vlastnosti, které tvoří klíč entity jsou vybrán v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="39601-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="39601-583">Hodnoty vlastnosti klíče entity musí jednoznačně identifikovat instanci entity typu v rámci entity nastavit v době běhu.</span><span class="sxs-lookup"><span data-stu-id="39601-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="39601-584">Vlastnosti, které tvoří klíč entity, je třeba zvolit pro zajištění jedinečnosti instancí v sadu entit.</span><span class="sxs-lookup"><span data-stu-id="39601-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="39601-585">**Klíč** element definuje klíč entity pomocí odkazu na jeden nebo více vlastností typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="39601-586">**Klíč** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="39601-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="39601-587">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="39601-588">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-589">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-589">Applicable Attributes</span></span>

<span data-ttu-id="39601-590">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **klíč** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="39601-591">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-592">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="39601-593">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-593">Example</span></span>

<span data-ttu-id="39601-594">Následující příklad definuje typ entity s názvem **knihy**.</span><span class="sxs-lookup"><span data-stu-id="39601-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="39601-595">Klíč entity je definována odkazováním **ISBN** vlastnost typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="39601-596">**ISBN** vlastnost je dobrou volbou pro klíč entity, protože mezinárodní čísla pro standardní knihy (ISBN) jednoznačně identifikuje knihy.</span><span class="sxs-lookup"><span data-stu-id="39601-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="39601-597">Následující příklad ukazuje typ entity (**Autor**), který má klíč entity, která se skládá ze dvou vlastností **název** a **adresu**.</span><span class="sxs-lookup"><span data-stu-id="39601-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="39601-598">Pomocí **název** a **adresu** entity klíč je vhodná volba, protože je nepravděpodobné, že za provozu na stejné adrese dvě autoři se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="39601-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="39601-599">Nicméně tato volba pro klíč entity naprosto nezaručuje klíče jedinečné entity v sadě entit.</span><span class="sxs-lookup"><span data-stu-id="39601-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="39601-600">Přidání vlastnosti, jako například **AuthorId**, který může použít k jednoznačné identifikaci Autor by v tomto případě doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="39601-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="39601-601">Člen – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-601">Member Element (CSDL)</span></span>

<span data-ttu-id="39601-602">**Člen** element je podřízeným prvkem elementu EnumType a definuje člen výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="39601-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-603">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-603">Applicable Attributes</span></span>

<span data-ttu-id="39601-604">Následující tabulka popisuje atributy, které mohou být použity **element FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="39601-605">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-605">Attribute Name</span></span> | <span data-ttu-id="39601-606">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-606">Is Required</span></span> | <span data-ttu-id="39601-607">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-608">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-608">**Name**</span></span>       | <span data-ttu-id="39601-609">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-609">Yes</span></span>         | <span data-ttu-id="39601-610">Název člena.</span><span class="sxs-lookup"><span data-stu-id="39601-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="39601-611">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="39601-611">**Value**</span></span>      | <span data-ttu-id="39601-612">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-612">No</span></span>          | <span data-ttu-id="39601-613">Hodnota člena.</span><span class="sxs-lookup"><span data-stu-id="39601-613">The value of the member.</span></span> <span data-ttu-id="39601-614">Ve výchozím nastavení první člen má hodnotu 0 a hodnotu každé po sobě jdoucích čítač se zvyšuje o 1.</span><span class="sxs-lookup"><span data-stu-id="39601-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="39601-615">Může existovat více členů se stejnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="39601-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-616">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **element FunctionImport** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="39601-617">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-618">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-619">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-619">Example</span></span>

<span data-ttu-id="39601-620">Následující příklad ukazuje **EnumType** element s tři **člen** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="39601-621">Element NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="39601-622">A **NavigationProperty** element definuje vlastnost navigace, který poskytuje odkaz na druhém konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="39601-623">Na rozdíl od vlastnosti definované s elementem vlastností navigačních vlastností nedefinují tvar a vlastnosti dat.</span><span class="sxs-lookup"><span data-stu-id="39601-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="39601-624">Poskytují způsob, jak procházet přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="39601-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="39601-625">Všimněte si, že jsou volitelné na oba typy entit na konci asociace navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="39601-626">Pokud definujete navigační vlastnost jednoho typu entity na konci asociace, není nutné definovat vlastnost navigace typu entity na druhém konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="39601-627">Datový typ vracený navigační vlastnost se určuje podle násobnost ukončení vzdálené přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="39601-628">Předpokládejme například, vlastnost navigace, **OrdersNavProp**, existuje na **zákazníka** typu entity a přejde na více přidružení mezi **zákazníka** a  **Pořadí**.</span><span class="sxs-lookup"><span data-stu-id="39601-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="39601-629">Protože ukončení vzdálené přidružení pro navigační vlastnost má násobnost mnoho (\*), jeho datový typ je kolekce (z **pořadí**).</span><span class="sxs-lookup"><span data-stu-id="39601-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="39601-630">Podobně, pokud vlastnost navigace, **CustomerNavProp**, existuje na **pořadí** typ entity, jeho datový typ by **zákazníka** protože má násobnost vzdáleným koncem jedna (1).</span><span class="sxs-lookup"><span data-stu-id="39601-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="39601-631">A **NavigationProperty** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-632">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-633">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-634">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-634">Applicable Attributes</span></span>

<span data-ttu-id="39601-635">Následující tabulka popisuje atributy, které mohou být použity **NavigationProperty** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="39601-636">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-636">Attribute Name</span></span>   | <span data-ttu-id="39601-637">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-637">Is Required</span></span> | <span data-ttu-id="39601-638">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-639">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-639">**Name**</span></span>         | <span data-ttu-id="39601-640">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-640">Yes</span></span>         | <span data-ttu-id="39601-641">Název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="39601-642">**Relace**</span><span class="sxs-lookup"><span data-stu-id="39601-642">**Relationship**</span></span> | <span data-ttu-id="39601-643">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-643">Yes</span></span>         | <span data-ttu-id="39601-644">Název přidružení, které je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="39601-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="39601-645">**ToRole**</span></span>       | <span data-ttu-id="39601-646">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-646">Yes</span></span>         | <span data-ttu-id="39601-647">End přidružení, na které koncích navigace.</span><span class="sxs-lookup"><span data-stu-id="39601-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="39601-648">Hodnota **ToRole** atribut musí být stejná jako hodnota některého **Role** atributy definovaného u jednoho z zakončení (definovaný v elementu AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="39601-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="39601-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="39601-649">**FromRole**</span></span>     | <span data-ttu-id="39601-650">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-650">Yes</span></span>         | <span data-ttu-id="39601-651">Konec přidružení, ze kterého se spustí navigace.</span><span class="sxs-lookup"><span data-stu-id="39601-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="39601-652">Hodnota **FromRole** atribut musí být stejná jako hodnota některého **Role** atributy definovaného u jednoho z zakončení (definovaný v elementu AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="39601-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-653">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **NavigationProperty** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="39601-654">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-655">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-656">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-656">Example</span></span>

<span data-ttu-id="39601-657">Následující příklad definuje typ entity (**knihy**) s dvěma navigační vlastnosti (**PublishedBy** a **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="39601-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="39601-658">Elementy OnDelete – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="39601-659">**Elementy OnDelete** prvek v Konceptuální schéma definici jazyka (CSDL) definuje chování, která je propojená s přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="39601-660">Pokud **akce** atribut je nastaven na **Cascade** na jednom konci asociace související s typy entity na druhém konci přidružení se odstraní při odstranění typu entity na konci prvního.</span><span class="sxs-lookup"><span data-stu-id="39601-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="39601-661">Pokud je primární klíč primárního klíče vztah přidružení mezi dvěma typy entity a pak načíst závislý objekt se odstraní při odstranění objektu na druhém konci přidružení, bez ohledu na to **elementy OnDelete** specifikace.</span><span class="sxs-lookup"><span data-stu-id="39601-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="39601-662">**Elementy OnDelete** element má vliv pouze chování modulu runtime aplikace; nemá vliv na chování ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="39601-663">Chování definované ve zdroji dat by měl být stejný jako chování definované v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="39601-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="39601-664">**Elementy OnDelete** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-665">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-666">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-667">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-667">Applicable Attributes</span></span>

<span data-ttu-id="39601-668">Následující tabulka popisuje atributy, které mohou být použity **elementy OnDelete** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="39601-669">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-669">Attribute Name</span></span> | <span data-ttu-id="39601-670">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-670">Is Required</span></span> | <span data-ttu-id="39601-671">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-672">**Akce**</span><span class="sxs-lookup"><span data-stu-id="39601-672">**Action**</span></span>     | <span data-ttu-id="39601-673">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-673">Yes</span></span>         | <span data-ttu-id="39601-674">**Kaskádové** nebo **žádný**.</span><span class="sxs-lookup"><span data-stu-id="39601-674">**Cascade** or **None**.</span></span> <span data-ttu-id="39601-675">Pokud **Cascade**, typy závislé entit se odstraní při odstranění typ objektu zabezpečení entity.</span><span class="sxs-lookup"><span data-stu-id="39601-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="39601-676">Pokud **žádný**, typy závislé entit se neodstraní při odstraňování instančního objektu entity typu.</span><span class="sxs-lookup"><span data-stu-id="39601-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-677">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="39601-678">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-679">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-680">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-680">Example</span></span>

<span data-ttu-id="39601-681">Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="39601-682">**Elementy OnDelete** element znamená, že všechny **objednávky** , která se vztahují ke konkrétní **zákazníka** a byly načtené do objektu ObjectContext se odstraní při  **Zákazník** se odstraní.</span><span class="sxs-lookup"><span data-stu-id="39601-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="39601-683">Parameter – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="39601-684">**Parametr** element v Konceptuální schéma definici jazyka (CSDL) může být podřízený FunctionImport element nebo elementu Function.</span><span class="sxs-lookup"><span data-stu-id="39601-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="39601-685">FunctionImport Element aplikace</span><span class="sxs-lookup"><span data-stu-id="39601-685">FunctionImport Element Application</span></span>

<span data-ttu-id="39601-686">A **parametr** – element (jako podřízený objekt **element FunctionImport** element) se používá k definování vstupních a výstupních parametrů pro importované funkce, které jsou deklarovány v CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="39601-687">**Parametr** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-688">Dokumentace ke službě (žádný nebo jeden prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="39601-689">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="39601-690">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-690">Applicable Attributes</span></span>

<span data-ttu-id="39601-691">Následující tabulka popisuje atributy, které mohou být použity **parametr** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="39601-692">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-692">Attribute Name</span></span> | <span data-ttu-id="39601-693">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-693">Is Required</span></span> | <span data-ttu-id="39601-694">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-695">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-695">**Name**</span></span>       | <span data-ttu-id="39601-696">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-696">Yes</span></span>         | <span data-ttu-id="39601-697">Název parametru</span><span class="sxs-lookup"><span data-stu-id="39601-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="39601-698">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-698">**Type**</span></span>       | <span data-ttu-id="39601-699">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-699">Yes</span></span>         | <span data-ttu-id="39601-700">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="39601-700">The parameter type.</span></span> <span data-ttu-id="39601-701">Hodnota musí být **EDMSimpleType** nebo komplexní typ, který spadá do rozsahu modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="39601-702">**Režim**</span><span class="sxs-lookup"><span data-stu-id="39601-702">**Mode**</span></span>       | <span data-ttu-id="39601-703">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-703">No</span></span>          | <span data-ttu-id="39601-704">**V**, **si**, nebo **InOut** v závislosti na tom, zda je parametr vstup, výstup nebo vstupně výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="39601-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="39601-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-705">**MaxLength**</span></span>  | <span data-ttu-id="39601-706">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-706">No</span></span>          | <span data-ttu-id="39601-707">Maximální povolená délka parametru.</span><span class="sxs-lookup"><span data-stu-id="39601-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="39601-708">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-708">**Precision**</span></span>  | <span data-ttu-id="39601-709">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-709">No</span></span>          | <span data-ttu-id="39601-710">Přesnost parametru.</span><span class="sxs-lookup"><span data-stu-id="39601-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="39601-711">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-711">**Scale**</span></span>      | <span data-ttu-id="39601-712">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-712">No</span></span>          | <span data-ttu-id="39601-713">Měřítko parametru.</span><span class="sxs-lookup"><span data-stu-id="39601-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="39601-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-714">**SRID**</span></span>       | <span data-ttu-id="39601-715">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-715">No</span></span>          | <span data-ttu-id="39601-716">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="39601-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="39601-717">Platí jenom pro parametry prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="39601-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="39601-718">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="39601-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-719">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **parametr** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="39601-720">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-721">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="39601-722">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-722">Example</span></span>

<span data-ttu-id="39601-723">Následující příklad ukazuje **element FunctionImport** element s jedním **parametr** podřízený element.</span><span class="sxs-lookup"><span data-stu-id="39601-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="39601-724">Funkce přijímá jeden vstupní parametr a vrátí kolekci typů entit.</span><span class="sxs-lookup"><span data-stu-id="39601-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="39601-725">Aplikaci prvku – funkce</span><span class="sxs-lookup"><span data-stu-id="39601-725">Function Element Application</span></span>

<span data-ttu-id="39601-726">A **parametr** – element (jako podřízený objekt **funkce** element) definuje parametry pro funkce, které jsou definována nebo deklarována v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="39601-727">**Parametr** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-728">Dokumentace ke službě (žádný nebo jeden prvky)</span><span class="sxs-lookup"><span data-stu-id="39601-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="39601-729">Objekt CollectionType (žádný nebo jeden prvky)</span><span class="sxs-lookup"><span data-stu-id="39601-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="39601-730">Hodnota ReferenceType (žádný nebo jeden prvky)</span><span class="sxs-lookup"><span data-stu-id="39601-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="39601-731">RowType (žádný nebo jeden prvky)</span><span class="sxs-lookup"><span data-stu-id="39601-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="39601-732">Pouze jeden z **CollectionType**, **ReferenceType**, nebo **RowType** prvků může být podřízený prvek **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="39601-733">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="39601-734">Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="39601-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="39601-735">Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="39601-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="39601-736">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-736">Applicable Attributes</span></span>

<span data-ttu-id="39601-737">Následující tabulka popisuje atributy, které mohou být použity **parametr** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="39601-738">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-738">Attribute Name</span></span>   | <span data-ttu-id="39601-739">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-739">Is Required</span></span> | <span data-ttu-id="39601-740">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-741">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-741">**Name**</span></span>         | <span data-ttu-id="39601-742">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-742">Yes</span></span>         | <span data-ttu-id="39601-743">Název parametru</span><span class="sxs-lookup"><span data-stu-id="39601-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="39601-744">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-744">**Type**</span></span>         | <span data-ttu-id="39601-745">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-745">No</span></span>          | <span data-ttu-id="39601-746">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="39601-746">The parameter type.</span></span> <span data-ttu-id="39601-747">Parametr může být některý z následujících typů (nebo kolekcí z těchto typů):</span><span class="sxs-lookup"><span data-stu-id="39601-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="39601-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="39601-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="39601-749">Typ entity</span><span class="sxs-lookup"><span data-stu-id="39601-749">entity type</span></span> <br/> <span data-ttu-id="39601-750">komplexní typ</span><span class="sxs-lookup"><span data-stu-id="39601-750">complex type</span></span> <br/> <span data-ttu-id="39601-751">Typ řádku</span><span class="sxs-lookup"><span data-stu-id="39601-751">row type</span></span> <br/> <span data-ttu-id="39601-752">odkazový typ</span><span class="sxs-lookup"><span data-stu-id="39601-752">reference type</span></span>                             |
| <span data-ttu-id="39601-753">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="39601-753">**Nullable**</span></span>     | <span data-ttu-id="39601-754">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-754">No</span></span>          | <span data-ttu-id="39601-755">**Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít **null** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="39601-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="39601-756">**Výchozí hodnota**</span><span class="sxs-lookup"><span data-stu-id="39601-756">**DefaultValue**</span></span> | <span data-ttu-id="39601-757">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-757">No</span></span>          | <span data-ttu-id="39601-758">Výchozí hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="39601-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-759">**MaxLength**</span></span>    | <span data-ttu-id="39601-760">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-760">No</span></span>          | <span data-ttu-id="39601-761">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="39601-762">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="39601-762">**FixedLength**</span></span>  | <span data-ttu-id="39601-763">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-763">No</span></span>          | <span data-ttu-id="39601-764">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.</span><span class="sxs-lookup"><span data-stu-id="39601-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="39601-765">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-765">**Precision**</span></span>    | <span data-ttu-id="39601-766">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-766">No</span></span>          | <span data-ttu-id="39601-767">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="39601-768">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-768">**Scale**</span></span>        | <span data-ttu-id="39601-769">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-769">No</span></span>          | <span data-ttu-id="39601-770">Měřítko pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="39601-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-771">**SRID**</span></span>         | <span data-ttu-id="39601-772">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-772">No</span></span>          | <span data-ttu-id="39601-773">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="39601-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="39601-774">Platí pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="39601-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="39601-775">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="39601-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="39601-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="39601-776">**Unicode**</span></span>      | <span data-ttu-id="39601-777">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-777">No</span></span>          | <span data-ttu-id="39601-778">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.</span><span class="sxs-lookup"><span data-stu-id="39601-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="39601-779">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="39601-779">**Collation**</span></span>    | <span data-ttu-id="39601-780">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-780">No</span></span>          | <span data-ttu-id="39601-781">Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="39601-782">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **parametr** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="39601-783">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-784">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="39601-785">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-785">Example</span></span>

<span data-ttu-id="39601-786">Následující příklad ukazuje **funkce** element, který používá jednu **parametr** podřízený element definovat parametr funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="39601-787">Instanční objekt – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="39601-788">**Hlavní** element v Konceptuální schéma definici jazyka (CSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="39601-789">A **elementu ReferentialConstraint** element definuje funkci, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="39601-790">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč z jiné tabulky lze vlastnost (nebo vlastnosti) typu entity odkazují na klíč entity, která jiný typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="39601-791">Je volána typ entity, na který odkazuje *hlavní koncový* omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="39601-792">Typ entity, která odkazuje na konci instančního objektu je volána *závislé end* omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="39601-793">**PropertyRef** elementy se používají k určení klíče, které je odkazováno dle závislé end.</span><span class="sxs-lookup"><span data-stu-id="39601-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="39601-794">**Hlavní** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-795">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="39601-796">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-797">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-797">Applicable Attributes</span></span>

<span data-ttu-id="39601-798">Následující tabulka popisuje atributy, které mohou být použity **hlavní** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="39601-799">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-799">Attribute Name</span></span> | <span data-ttu-id="39601-800">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-800">Is Required</span></span> | <span data-ttu-id="39601-801">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="39601-802">**Role**</span><span class="sxs-lookup"><span data-stu-id="39601-802">**Role**</span></span>       | <span data-ttu-id="39601-803">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-803">Yes</span></span>         | <span data-ttu-id="39601-804">Název typu entity na hlavní konec asociace.</span><span class="sxs-lookup"><span data-stu-id="39601-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-805">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **hlavní** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="39601-806">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-807">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-808">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-808">Example</span></span>

<span data-ttu-id="39601-809">Následující příklad ukazuje **elementu ReferentialConstraint** element, který je součástí definice **PublishedBy** přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="39601-810">**Id** vlastnost **vydavatele** typ entity tvoří hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="39601-811">Property – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-811">Property Element (CSDL)</span></span>

<span data-ttu-id="39601-812">**Vlastnost** element v Konceptuální schéma definici jazyka (CSDL) může být podřízený EntityType element, ComplexType element nebo RowType element.</span><span class="sxs-lookup"><span data-stu-id="39601-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="39601-813">Třída EntityType a ComplexType Element aplikací</span><span class="sxs-lookup"><span data-stu-id="39601-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="39601-814">**Vlastnost** prvky (jako podřízené objekty **EntityType** nebo **ComplexType** elementy) definovat tvar a vlastnosti dat, která bude obsahovat instance typu entity nebo komplexní typ instance .</span><span class="sxs-lookup"><span data-stu-id="39601-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="39601-815">Vlastnosti v konceptuálním modelu jsou podobná vlastnosti, které jsou definovány ve třídě.</span><span class="sxs-lookup"><span data-stu-id="39601-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="39601-816">Stejným způsobem, že vlastnosti třídy definovat tvar třídy a budou mít informace o objektech vlastnosti v konceptuálním modelu definovat tvar typu entity a nesou informaci o instancí typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="39601-817">**Vlastnost** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-818">Element Documentation (žádný nebo jeden prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="39601-819">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="39601-820">Lze použít následující omezující vlastnosti **vlastnost** element: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **přesnost**, **škálování**, **Unicode**, **kolace**,  **Režim ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="39601-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="39601-821">Omezující vlastnosti jsou atributy ve formátu XML, které obsahují informace o způsobu uložení hodnoty vlastnosti dat v úložišti.</span><span class="sxs-lookup"><span data-stu-id="39601-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-822">Charakteristiky může používat jedině pro vlastnosti typu **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="39601-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="39601-823">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-823">Applicable Attributes</span></span>

<span data-ttu-id="39601-824">Následující tabulka popisuje atributy, které mohou být použity **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="39601-825">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-825">Attribute Name</span></span>                                                         | <span data-ttu-id="39601-826">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-826">Is Required</span></span> | <span data-ttu-id="39601-827">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-828">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-828">**Name**</span></span>                                                               | <span data-ttu-id="39601-829">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-829">Yes</span></span>         | <span data-ttu-id="39601-830">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="39601-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="39601-831">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-831">**Type**</span></span>                                                               | <span data-ttu-id="39601-832">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-832">Yes</span></span>         | <span data-ttu-id="39601-833">Typ hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-833">The type of the property value.</span></span> <span data-ttu-id="39601-834">Musí být typ hodnoty vlastnosti **EDMSimpleType** nebo komplexní typ (označená plně kvalifikovaný název), který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="39601-835">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="39601-835">**Nullable**</span></span>                                                           | <span data-ttu-id="39601-836">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-836">No</span></span>          | <span data-ttu-id="39601-837">**Hodnota TRUE** (výchozí hodnota) nebo <strong>False</strong> v závislosti na tom, zda tato vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39601-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="39601-838">> CSDL v1 komplexní typ vlastnosti musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="39601-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="39601-839">**Výchozí hodnota**</span><span class="sxs-lookup"><span data-stu-id="39601-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="39601-840">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-840">No</span></span>          | <span data-ttu-id="39601-841">Výchozí hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="39601-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="39601-843">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-843">No</span></span>          | <span data-ttu-id="39601-844">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="39601-845">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="39601-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="39601-846">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-846">No</span></span>          | <span data-ttu-id="39601-847">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.</span><span class="sxs-lookup"><span data-stu-id="39601-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="39601-848">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-848">**Precision**</span></span>                                                          | <span data-ttu-id="39601-849">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-849">No</span></span>          | <span data-ttu-id="39601-850">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="39601-851">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-851">**Scale**</span></span>                                                              | <span data-ttu-id="39601-852">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-852">No</span></span>          | <span data-ttu-id="39601-853">Měřítko pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="39601-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-854">**SRID**</span></span>                                                               | <span data-ttu-id="39601-855">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-855">No</span></span>          | <span data-ttu-id="39601-856">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="39601-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="39601-857">Platí pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="39601-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="39601-858">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="39601-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="39601-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="39601-859">**Unicode**</span></span>                                                            | <span data-ttu-id="39601-860">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-860">No</span></span>          | <span data-ttu-id="39601-861">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.</span><span class="sxs-lookup"><span data-stu-id="39601-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="39601-862">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="39601-862">**Collation**</span></span>                                                          | <span data-ttu-id="39601-863">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-863">No</span></span>          | <span data-ttu-id="39601-864">Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="39601-865">**Režim ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="39601-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="39601-866">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-866">No</span></span>          | <span data-ttu-id="39601-867">**Žádný** (výchozí hodnota) nebo **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="39601-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="39601-868">Pokud je hodnota nastavena na **Fixed**, bude použita hodnota vlastnosti v kontroly optimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="39601-869">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="39601-870">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-871">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="39601-872">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-872">Example</span></span>

<span data-ttu-id="39601-873">Následující příklad ukazuje **EntityType** element s tři **vlastnost** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="39601-874">Následující příklad ukazuje **ComplexType** element s pěti **vlastnost** prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="39601-875">RowType Element aplikace</span><span class="sxs-lookup"><span data-stu-id="39601-875">RowType Element Application</span></span>

<span data-ttu-id="39601-876">**Vlastnost** prvky (jako podřízených prvků **RowType** element) definovat tvar a vlastnosti dat, které mohou být předány nebo vrácena z funkce modelově definovaných.</span><span class="sxs-lookup"><span data-stu-id="39601-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="39601-877">**Vlastnost** prvek může mít nastavený právě jeden z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="39601-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="39601-878">Objekt CollectionType</span><span class="sxs-lookup"><span data-stu-id="39601-878">CollectionType</span></span>
-   <span data-ttu-id="39601-879">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="39601-879">ReferenceType</span></span>
-   <span data-ttu-id="39601-880">RowType</span><span class="sxs-lookup"><span data-stu-id="39601-880">RowType</span></span>

<span data-ttu-id="39601-881">**Vlastnost** prvek může mít libovolný počet podřízených elementů poznámky.</span><span class="sxs-lookup"><span data-stu-id="39601-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-882">Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="39601-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="39601-883">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-883">Applicable Attributes</span></span>

<span data-ttu-id="39601-884">Následující tabulka popisuje atributy, které mohou být použity **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="39601-885">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-885">Attribute Name</span></span>                                                     | <span data-ttu-id="39601-886">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-886">Is Required</span></span> | <span data-ttu-id="39601-887">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-888">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-888">**Name**</span></span>                                                           | <span data-ttu-id="39601-889">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-889">Yes</span></span>         | <span data-ttu-id="39601-890">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="39601-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="39601-891">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-891">**Type**</span></span>                                                           | <span data-ttu-id="39601-892">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-892">Yes</span></span>         | <span data-ttu-id="39601-893">Typ hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="39601-894">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="39601-894">**Nullable**</span></span>                                                       | <span data-ttu-id="39601-895">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-895">No</span></span>          | <span data-ttu-id="39601-896">**Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39601-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="39601-897">> CSDL v1 komplexní typ vlastnosti musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="39601-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="39601-898">**Výchozí hodnota**</span><span class="sxs-lookup"><span data-stu-id="39601-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="39601-899">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-899">No</span></span>          | <span data-ttu-id="39601-900">Výchozí hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="39601-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="39601-902">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-902">No</span></span>          | <span data-ttu-id="39601-903">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="39601-904">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="39601-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="39601-905">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-905">No</span></span>          | <span data-ttu-id="39601-906">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.</span><span class="sxs-lookup"><span data-stu-id="39601-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="39601-907">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-907">**Precision**</span></span>                                                      | <span data-ttu-id="39601-908">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-908">No</span></span>          | <span data-ttu-id="39601-909">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="39601-910">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-910">**Scale**</span></span>                                                          | <span data-ttu-id="39601-911">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-911">No</span></span>          | <span data-ttu-id="39601-912">Měřítko pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="39601-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-913">**SRID**</span></span>                                                           | <span data-ttu-id="39601-914">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-914">No</span></span>          | <span data-ttu-id="39601-915">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="39601-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="39601-916">Platí pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="39601-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="39601-917">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="39601-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="39601-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="39601-918">**Unicode**</span></span>                                                        | <span data-ttu-id="39601-919">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-919">No</span></span>          | <span data-ttu-id="39601-920">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.</span><span class="sxs-lookup"><span data-stu-id="39601-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="39601-921">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="39601-921">**Collation**</span></span>                                                      | <span data-ttu-id="39601-922">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-922">No</span></span>          | <span data-ttu-id="39601-923">Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="39601-924">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="39601-925">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-926">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="39601-927">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-927">Example</span></span>

<span data-ttu-id="39601-928">Následující příklad ukazuje **vlastnost** prvků, které slouží k definování tvar návratový typ modelu uživatelsky definované funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="39601-929">Element PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="39601-930">**PropertyRef** element v Konceptuální schéma definici jazyka (CSDL) odkazuje na vlastnost typu entity k označení, že vlastnost bude provádět jednu z následujících rolí:</span><span class="sxs-lookup"><span data-stu-id="39601-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="39601-931">Část klíče entity (vlastnost nebo sadu vlastností typu entity, které určují identity).</span><span class="sxs-lookup"><span data-stu-id="39601-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="39601-932">Jeden nebo více **PropertyRef** prvky můžete použít k definování klíč entity.</span><span class="sxs-lookup"><span data-stu-id="39601-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="39601-933">Závislé nebo hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="39601-934">**PropertyRef** element může obsahovat pouze prvky poznámky (nula nebo více) jako podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="39601-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-935">Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="39601-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="39601-936">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-936">Applicable Attributes</span></span>

<span data-ttu-id="39601-937">Následující tabulka popisuje atributy, které mohou být použity **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="39601-938">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-938">Attribute Name</span></span> | <span data-ttu-id="39601-939">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-939">Is Required</span></span> | <span data-ttu-id="39601-940">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="39601-941">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="39601-941">**Name**</span></span>       | <span data-ttu-id="39601-942">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-942">Yes</span></span>         | <span data-ttu-id="39601-943">Název odkazované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-944">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="39601-945">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-946">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-947">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-947">Example</span></span>

<span data-ttu-id="39601-948">Následující příklad definuje typ entity (**knihy**).</span><span class="sxs-lookup"><span data-stu-id="39601-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="39601-949">Klíč entity je definována odkazováním **ISBN** vlastnost typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="39601-950">V následujícím příkladu dvě **PropertyRef** elementy se používají k označení tohoto dvě vlastnosti (**Id** a **PublisherId**) jsou konců objektem zabezpečení a závislými referenční omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="39601-951">Element ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="39601-952">**ReferenceType** element v Konceptuální schéma definici jazyka (CSDL) určuje odkaz na typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="39601-953">**ReferenceType** element může být podřízená následující prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="39601-954">Vlastnost ReturnType (Function)</span><span class="sxs-lookup"><span data-stu-id="39601-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="39601-955">Parametr</span><span class="sxs-lookup"><span data-stu-id="39601-955">Parameter</span></span>
-   <span data-ttu-id="39601-956">Objekt CollectionType</span><span class="sxs-lookup"><span data-stu-id="39601-956">CollectionType</span></span>

<span data-ttu-id="39601-957">**ReferenceType** element se používá při definování parametr nebo návratový typ pro funkci.</span><span class="sxs-lookup"><span data-stu-id="39601-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="39601-958">A **ReferenceType** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-959">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-960">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-961">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-961">Applicable Attributes</span></span>

<span data-ttu-id="39601-962">Následující tabulka popisuje atributy, které mohou být použity **ReferenceType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="39601-963">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-963">Attribute Name</span></span> | <span data-ttu-id="39601-964">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-964">Is Required</span></span> | <span data-ttu-id="39601-965">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="39601-966">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-966">**Type**</span></span>       | <span data-ttu-id="39601-967">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-967">Yes</span></span>         | <span data-ttu-id="39601-968">Název typu entity, na kterou se odkazuje.</span><span class="sxs-lookup"><span data-stu-id="39601-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-969">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReferenceType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="39601-970">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-971">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-972">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-972">Example</span></span>

<span data-ttu-id="39601-973">Následující příklad ukazuje **ReferenceType** používá jako podřízený element **parametr** elementu v modelu definován funkci, která přijímá odkaz na **osoba** entity Typ:</span><span class="sxs-lookup"><span data-stu-id="39601-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="39601-974">Následující příklad ukazuje **ReferenceType** používá jako podřízený element **ReturnType** – element (funkce) v modelu definován funkci, která vrátí odkaz na **osoba**typ entity:</span><span class="sxs-lookup"><span data-stu-id="39601-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="39601-975">Element elementu ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="39601-976">A **elementu ReferentialConstraint** prvek v Konceptuální schéma definici jazyka (CSDL) definuje funkci, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="39601-977">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč z jiné tabulky lze vlastnost (nebo vlastnosti) typu entity odkazují na klíč entity, která jiný typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="39601-978">Je volána typ entity, na který odkazuje *hlavní koncový* omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="39601-979">Typ entity, která odkazuje na konci instančního objektu je volána *závislé end* omezení.</span><span class="sxs-lookup"><span data-stu-id="39601-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="39601-980">Pokud cizí klíč, který je zveřejněný na jednu entitu typu odkazuje na vlastnost na jiný typ entity, **elementu ReferentialConstraint** element definuje přidružení mezi tyto dva typy entity.</span><span class="sxs-lookup"><span data-stu-id="39601-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="39601-981">Vzhledem k tomu, **elementu ReferentialConstraint** element poskytuje informace o tom, jak dva typy entit souvisejí, bez odpovídajícího prvku AssociationSetMapping je nezbytné v mapování specification language (MSL).</span><span class="sxs-lookup"><span data-stu-id="39601-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="39601-982">Přidružení mezi dvěma typy entit, které nemají cizí klíče, které jsou vystaveny musí mít odpovídající **AssociationSetMapping** prvku abyste mohli mapovat na zdroj dat informací o přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="39601-983">Pokud cizí klíč není dostupná na typ entity, **elementu ReferentialConstraint** elementu lze definovat pouze klíč primárního omezení primárního klíče mezi typu entity a jiný typ entity.</span><span class="sxs-lookup"><span data-stu-id="39601-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="39601-984">A **elementu ReferentialConstraint** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-985">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-986">Objekt zabezpečení (právě jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="39601-987">Závislé (právě jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="39601-988">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-989">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-989">Applicable Attributes</span></span>

<span data-ttu-id="39601-990">**Elementu ReferentialConstraint** prvek může mít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="39601-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="39601-991">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-992">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="39601-993">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-993">Example</span></span>

<span data-ttu-id="39601-994">Následující příklad ukazuje **elementu ReferentialConstraint** element se používá jako součást definice **PublishedBy** přidružení.</span><span class="sxs-lookup"><span data-stu-id="39601-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="39601-995">Element ReturnType (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="39601-996">**ReturnType** – element (funkce) v Konceptuální schéma definici jazyka (CSDL) určuje návratový typ pro funkci, která je definována v elementu Function.</span><span class="sxs-lookup"><span data-stu-id="39601-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="39601-997">Funkce, návratový typ je také možné zadat při **ReturnType** atribut.</span><span class="sxs-lookup"><span data-stu-id="39601-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="39601-998">Vrátí typy mohou být některé **EdmSimpleType**, typ entity, komplexní typ, typ řádku, typ odkazu nebo kolekce jednoho z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="39601-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="39601-999">Návratový typ funkce se dá nastavit některým **typ** atribut **ReturnType** – element (funkce), nebo některou z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="39601-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="39601-1000">Objekt CollectionType</span><span class="sxs-lookup"><span data-stu-id="39601-1000">CollectionType</span></span>
-   <span data-ttu-id="39601-1001">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="39601-1001">ReferenceType</span></span>
-   <span data-ttu-id="39601-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="39601-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="39601-1003">Model neověří při zadání funkce, návratový typ se službami **typ** atribut **ReturnType** elementu (funkce) a jeden z podřízených prvků.</span><span class="sxs-lookup"><span data-stu-id="39601-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="39601-1004">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-1004">Applicable Attributes</span></span>

<span data-ttu-id="39601-1005">Následující tabulka popisuje atributy, které mohou být použity **ReturnType** – element (funkce).</span><span class="sxs-lookup"><span data-stu-id="39601-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="39601-1006">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-1006">Attribute Name</span></span> | <span data-ttu-id="39601-1007">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-1007">Is Required</span></span> | <span data-ttu-id="39601-1008">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="39601-1009">**Vlastnost ReturnType**</span><span class="sxs-lookup"><span data-stu-id="39601-1009">**ReturnType**</span></span> | <span data-ttu-id="39601-1010">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1010">No</span></span>          | <span data-ttu-id="39601-1011">Typ vrácené funkcí.</span><span class="sxs-lookup"><span data-stu-id="39601-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-1012">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReturnType** – element (funkce).</span><span class="sxs-lookup"><span data-stu-id="39601-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="39601-1013">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-1014">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-1015">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1015">Example</span></span>

<span data-ttu-id="39601-1016">V následujícím příkladu **funkce** prvek, který chcete definovat funkci, která vrátí počet roků knihy se při tisku.</span><span class="sxs-lookup"><span data-stu-id="39601-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="39601-1017">Všimněte si, že je návratový typ určený **typ** atribut **ReturnType** – element (funkce).</span><span class="sxs-lookup"><span data-stu-id="39601-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="39601-1018">Vlastnost ReturnType (element FunctionImport) – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="39601-1019">**ReturnType** – element (element FunctionImport) v Konceptuální schéma definici jazyka (CSDL) určuje návratový typ pro funkci, která je definována v elementu FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="39601-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="39601-1020">Funkce, návratový typ je také možné zadat při **ReturnType** atribut.</span><span class="sxs-lookup"><span data-stu-id="39601-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="39601-1021">Vrátí typy mohou být jakoukoli kolekci typu entity, komplexní typ, nebo **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="39601-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="39601-1022">Návratový typ funkce se zadaným **typ** atribut **ReturnType** – element (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="39601-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-1023">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-1023">Applicable Attributes</span></span>

<span data-ttu-id="39601-1024">Následující tabulka popisuje atributy, které mohou být použity **ReturnType** – element (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="39601-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="39601-1025">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-1025">Attribute Name</span></span> | <span data-ttu-id="39601-1026">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-1026">Is Required</span></span> | <span data-ttu-id="39601-1027">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-1028">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-1028">**Type**</span></span>       | <span data-ttu-id="39601-1029">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1029">No</span></span>          | <span data-ttu-id="39601-1030">Typ, který funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="39601-1030">The type that the function returns.</span></span> <span data-ttu-id="39601-1031">Hodnota musí být kolekce ComplexType, EntityType nebo EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="39601-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="39601-1032">**Objekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="39601-1032">**EntitySet**</span></span>  | <span data-ttu-id="39601-1033">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1033">No</span></span>          | <span data-ttu-id="39601-1034">Pokud funkce vrátí kolekci entit typy, hodnota **objektu EntitySet** musí být sada entit, ke které patří do kolekce.</span><span class="sxs-lookup"><span data-stu-id="39601-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="39601-1035">V opačném případě **objektu EntitySet** atribut nesmí být použit.</span><span class="sxs-lookup"><span data-stu-id="39601-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-1036">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReturnType** – element (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="39601-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="39601-1037">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-1038">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-1039">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1039">Example</span></span>

<span data-ttu-id="39601-1040">Následující příklad používá **element FunctionImport** , která vrací knihy a vydavatelích.</span><span class="sxs-lookup"><span data-stu-id="39601-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="39601-1041">Všimněte si, že funkce vrátí dvou sad výsledků a tedy dvě **ReturnType** zadaných elementů (element FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="39601-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="39601-1042">Element RowType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="39601-1043">A **RowType** prvek v Konceptuální schéma definici jazyka (CSDL) definuje nepojmenovanou strukturu jako parametr nebo návratový typ funkce definované v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="39601-1044">A **RowType** element může být podřízená následující prvky:</span><span class="sxs-lookup"><span data-stu-id="39601-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="39601-1045">Objekt CollectionType</span><span class="sxs-lookup"><span data-stu-id="39601-1045">CollectionType</span></span>
-   <span data-ttu-id="39601-1046">Parametr</span><span class="sxs-lookup"><span data-stu-id="39601-1046">Parameter</span></span>
-   <span data-ttu-id="39601-1047">Vlastnost ReturnType (Function)</span><span class="sxs-lookup"><span data-stu-id="39601-1047">ReturnType (Function)</span></span>

<span data-ttu-id="39601-1048">A **RowType** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-1049">Vlastnosti (jeden nebo více)</span><span class="sxs-lookup"><span data-stu-id="39601-1049">Property (one or more)</span></span>
-   <span data-ttu-id="39601-1050">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="39601-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-1051">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-1051">Applicable Attributes</span></span>

<span data-ttu-id="39601-1052">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **RowType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="39601-1053">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-1054">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="39601-1055">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1055">Example</span></span>

<span data-ttu-id="39601-1056">Následující příklad ukazuje funkce definované v modelu, která používá **CollectionType** elementu k určení, že funkce vrátí kolekce řádků (jak je uvedeno v **RowType** element).</span><span class="sxs-lookup"><span data-stu-id="39601-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="39601-1057">Element schématu (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="39601-1058">**Schématu** prvek je kořenovým elementem definici konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="39601-1059">Obsahuje definice pro objekty, funkce a kontejnerů, které tvoří konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="39601-1060">**Schématu** element může obsahovat nula nebo více z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="39601-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="39601-1061">Použití</span><span class="sxs-lookup"><span data-stu-id="39601-1061">Using</span></span>
-   <span data-ttu-id="39601-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="39601-1062">EntityContainer</span></span>
-   <span data-ttu-id="39601-1063">Typ entity</span><span class="sxs-lookup"><span data-stu-id="39601-1063">EntityType</span></span>
-   <span data-ttu-id="39601-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="39601-1064">EnumType</span></span>
-   <span data-ttu-id="39601-1065">Přidružení</span><span class="sxs-lookup"><span data-stu-id="39601-1065">Association</span></span>
-   <span data-ttu-id="39601-1066">Typ ComplexType</span><span class="sxs-lookup"><span data-stu-id="39601-1066">ComplexType</span></span>
-   <span data-ttu-id="39601-1067">Funkce</span><span class="sxs-lookup"><span data-stu-id="39601-1067">Function</span></span>

<span data-ttu-id="39601-1068">A **schématu** element může obsahovat nejvýše jedno elementů poznámky.</span><span class="sxs-lookup"><span data-stu-id="39601-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-1069">**Funkce** elementu a elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="39601-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="39601-1070">**Schématu** prvek používá **Namespace** atribut pro definování oboru názvů pro typ entity, komplexní typ a přidružení objekty v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="39601-1071">V rámci oboru názvů může mít žádné dva objekty se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="39601-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="39601-1072">Obory názvů může zahrnovat více **schématu** prvků a více souborů .csdl.</span><span class="sxs-lookup"><span data-stu-id="39601-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="39601-1073">Obor názvů koncepčního modelu se liší od oboru názvů XML **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="39601-1074">Obor názvů koncepčního modelu (podle definice **Namespace** atribut) je logický kontejner pro typy entit a komplexní typy, přidružení typů.</span><span class="sxs-lookup"><span data-stu-id="39601-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="39601-1075">Obor názvů XML (indikován **xmlns** atribut) z **schématu** element je výchozí obor názvů pro podřízené prvky a atributy **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="39601-1076">Obory názvů XML formuláře http://schemas.microsoft.com/ado/YYYY/MM/edm (kde RRRR a MM představují roku a měsíce v uvedeném pořadí) jsou vyhrazené pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="39601-1077">Vlastní elementy a atributy nemohou být v oborech názvů, které mají tento formulář.</span><span class="sxs-lookup"><span data-stu-id="39601-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-1078">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-1078">Applicable Attributes</span></span>

<span data-ttu-id="39601-1079">Následující tabulka popisuje atributy lze použít **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="39601-1080">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-1080">Attribute Name</span></span> | <span data-ttu-id="39601-1081">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-1081">Is Required</span></span> | <span data-ttu-id="39601-1082">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="39601-1083">**Namespace**</span></span>  | <span data-ttu-id="39601-1084">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1084">Yes</span></span>         | <span data-ttu-id="39601-1085">Obor názvů konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="39601-1086">Hodnota **Namespace** atribut se používá k vytvoření plně kvalifikovaný název typu.</span><span class="sxs-lookup"><span data-stu-id="39601-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="39601-1087">Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů Simple.Example.Model pak plně kvalifikovaný název **EntityType** je SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="39601-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="39601-1088">Následující řetězce nelze použít jako hodnotu **Namespace** atribut: **systému**, **přechodné**, nebo **Edm**.</span><span class="sxs-lookup"><span data-stu-id="39601-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="39601-1089">Hodnota **Namespace** atribut nemůže být stejná jako hodnota pro **Namespace** atribut v prvek schématu SSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="39601-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="39601-1090">**Alias**</span></span>      | <span data-ttu-id="39601-1091">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1091">No</span></span>          | <span data-ttu-id="39601-1092">Identifikátor, použijí se místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="39601-1093">Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů Simple.Example.Model a hodnoty **Alias** atribut je *modelu*, můžete použít Model.Customer jako plně kvalifikovaný název **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="39601-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="39601-1094">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="39601-1095">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-1096">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-1097">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1097">Example</span></span>

<span data-ttu-id="39601-1098">Následující příklad ukazuje **schématu** element, který obsahuje **EntityContainer** elementu, dvěma **EntityType** elementy a jeden **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="39601-1099">Element TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="39601-1100">**TypeRef** element v Konceptuální schéma definici jazyka (CSDL) poskytuje odkaz na existující s názvem typu.</span><span class="sxs-lookup"><span data-stu-id="39601-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="39601-1101">**TypeRef** element může být podřízený element CollectionType, který se používá k určení, že má kolekce jako parametr nebo návratový typ funkce.</span><span class="sxs-lookup"><span data-stu-id="39601-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="39601-1102">A **TypeRef** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="39601-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="39601-1103">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="39601-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="39601-1104">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="39601-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-1105">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-1105">Applicable Attributes</span></span>

<span data-ttu-id="39601-1106">Následující tabulka popisuje atributy, které mohou být použity **TypeRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="39601-1107">Všimněte si, že **DefaultValue**, **MaxLength**, **FixedLength**, **přesnost**, **škálování**,  **Kódování Unicode**, a **kolace** atributy se vztahují jenom **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="39601-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="39601-1108">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="39601-1109">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-1109">Is Required</span></span> | <span data-ttu-id="39601-1110">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-1111">**Typ**</span><span class="sxs-lookup"><span data-stu-id="39601-1111">**Type**</span></span>                                                           | <span data-ttu-id="39601-1112">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1112">No</span></span>          | <span data-ttu-id="39601-1113">Název typu, který se odkazuje.</span><span class="sxs-lookup"><span data-stu-id="39601-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="39601-1114">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="39601-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="39601-1115">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1115">No</span></span>          | <span data-ttu-id="39601-1116">**Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="39601-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="39601-1117">> CSDL v1 komplexní typ vlastnosti musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="39601-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="39601-1118">**Výchozí hodnota**</span><span class="sxs-lookup"><span data-stu-id="39601-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="39601-1119">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1119">No</span></span>          | <span data-ttu-id="39601-1120">Výchozí hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="39601-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="39601-1122">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1122">No</span></span>          | <span data-ttu-id="39601-1123">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="39601-1124">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="39601-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="39601-1125">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1125">No</span></span>          | <span data-ttu-id="39601-1126">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.</span><span class="sxs-lookup"><span data-stu-id="39601-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="39601-1127">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-1127">**Precision**</span></span>                                                      | <span data-ttu-id="39601-1128">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1128">No</span></span>          | <span data-ttu-id="39601-1129">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="39601-1130">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-1130">**Scale**</span></span>                                                          | <span data-ttu-id="39601-1131">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1131">No</span></span>          | <span data-ttu-id="39601-1132">Měřítko pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="39601-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-1133">**SRID**</span></span>                                                           | <span data-ttu-id="39601-1134">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1134">No</span></span>          | <span data-ttu-id="39601-1135">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="39601-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="39601-1136">Platí pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="39601-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="39601-1137">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="39601-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="39601-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="39601-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="39601-1139">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1139">No</span></span>          | <span data-ttu-id="39601-1140">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.</span><span class="sxs-lookup"><span data-stu-id="39601-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="39601-1141">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="39601-1141">**Collation**</span></span>                                                      | <span data-ttu-id="39601-1142">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1142">No</span></span>          | <span data-ttu-id="39601-1143">Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="39601-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="39601-1144">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="39601-1145">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-1146">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-1147">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1147">Example</span></span>

<span data-ttu-id="39601-1148">Následující příklad ukazuje funkce definované v modelu, která používá **TypeRef** – element (jako podřízený objekt **CollectionType** element) k určení, že funkce přijímá kolekce  **Oddělení** typy entit.</span><span class="sxs-lookup"><span data-stu-id="39601-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="39601-1149">Pomocí elementu (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="39601-1150">**Použití** element v Konceptuální schéma definici jazyka (CSDL) Importuje obsah Koncepční model, který existuje v jiné oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="39601-1151">Nastavením hodnoty **Namespace** atributu, mohou odkazovat na typy entit, komplexní typy a typy přidružení, které jsou definovány v jiném koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="39601-1152">Více než jeden **použití** element může být podřízený **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-1153">**Použití** nebude fungovat stejně jako prvek v CSDL **pomocí** příkaz v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="39601-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="39601-1154">Importováním oboru názvů s **pomocí** příkaz v programovacím jazyce, nemají vliv objekty v oboru názvů původní.</span><span class="sxs-lookup"><span data-stu-id="39601-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="39601-1155">V CSDL importovaným oborem názvů může obsahovat typ entity, který je odvozen z typu entity v původní oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="39601-1156">To může ovlivnit sady entit deklarované v původní obor názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="39601-1157">**Použití** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="39601-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="39601-1158">Dokumentace ke službě (žádný nebo jeden prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="39601-1159">Elementů poznámky (nula nebo více prvků povolený)</span><span class="sxs-lookup"><span data-stu-id="39601-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="39601-1160">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="39601-1160">Applicable Attributes</span></span>

<span data-ttu-id="39601-1161">Následující tabulka popisuje atributy lze použít **použití** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="39601-1162">Název atributu</span><span class="sxs-lookup"><span data-stu-id="39601-1162">Attribute Name</span></span> | <span data-ttu-id="39601-1163">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="39601-1163">Is Required</span></span> | <span data-ttu-id="39601-1164">Hodnota</span><span class="sxs-lookup"><span data-stu-id="39601-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="39601-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="39601-1165">**Namespace**</span></span>  | <span data-ttu-id="39601-1166">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1166">Yes</span></span>         | <span data-ttu-id="39601-1167">Importované oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="39601-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="39601-1168">**Alias**</span></span>      | <span data-ttu-id="39601-1169">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1169">Yes</span></span>         | <span data-ttu-id="39601-1170">Identifikátor, použijí se místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="39601-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="39601-1171">I když tento atribut je vyžadován, není nutné název oboru názvů se být zastoupen kvalifikovat názvy objektů.</span><span class="sxs-lookup"><span data-stu-id="39601-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="39601-1172">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **použití** elementu.</span><span class="sxs-lookup"><span data-stu-id="39601-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="39601-1173">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="39601-1174">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="39601-1175">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1175">Example</span></span>

<span data-ttu-id="39601-1176">Následující příklad ukazuje, **použití** element se používá pro import oboru názvů, který je definovaný jinde.</span><span class="sxs-lookup"><span data-stu-id="39601-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="39601-1177">Všimněte si, že je obor názvů pro **schématu** uvedeného prvku je `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="39601-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="39601-1178">`Address` Vlastnost `Publisher` **EntityType** je komplexní typ, který je definován v `ExtendedBooksModel` obor názvů (importovat s **použití** element).</span><span class="sxs-lookup"><span data-stu-id="39601-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="39601-1179">Atributy poznámek (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="39601-1180">Atributy poznámky v Konceptuální schéma definici jazyka (CSDL) jsou vlastní atributy XML v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="39601-1181">Kromě s platnou strukturu XML, musí být splněné anotace atributů následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="39601-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="39601-1182">Atributy poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="39601-1183">Více než jeden atribut poznámky se můžou vztahovat na daný prvek CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="39601-1184">Plně kvalifikovaných názvů atributů dvě poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="39601-1185">Anotace atributy je možné poskytnout další metadata o prvky v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="39601-1186">Metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="39601-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="39601-1187">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1187">Example</span></span>

<span data-ttu-id="39601-1188">Následující příklad ukazuje **EntityType** element s atributem poznámky (**Atribut CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="39601-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="39601-1189">Příklad také ukazuje element poznámky na prvek typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="39601-1190">Následující kód načte metadata v atributu poznámky a zapíše jej do konzoly:</span><span class="sxs-lookup"><span data-stu-id="39601-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="39601-1191">Výše uvedený kód předpokládá, že `School.csdl` soubor v adresáři výstupu projektu a že jste přidali následující `Imports` a `Using` příkazy do projektu:</span><span class="sxs-lookup"><span data-stu-id="39601-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="39601-1192">Elementů poznámky (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="39601-1193">Elementů poznámky v Konceptuální schéma definici jazyka (CSDL) jsou vlastní elementy XML v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="39601-1194">Kromě s platnou strukturu XML, musí být elementů poznámky platí následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="39601-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="39601-1195">Elementů poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="39601-1196">Více než jeden element anotace může být podřízeným daného prvku CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="39601-1197">Plně kvalifikovaných názvů všech elementů dvě poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="39601-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="39601-1198">Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky daného prvku CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="39601-1199">Elementů poznámky je možné poskytnout další metadata o prvky v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="39601-1200">Od verze rozhraní .NET Framework verze 4, metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="39601-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="39601-1201">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1201">Example</span></span>

<span data-ttu-id="39601-1202">Následující příklad ukazuje **EntityType** prvek s elementem poznámky (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="39601-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="39601-1203">V příkladu také zobrazit poznámky atribut na prvek typu entity.</span><span class="sxs-lookup"><span data-stu-id="39601-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="39601-1204">Následující kód načte metadata v elementu poznámky a zapíše jej do konzoly:</span><span class="sxs-lookup"><span data-stu-id="39601-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="39601-1205">Výše uvedený kód předpokládá, že je School.csdl soubor v adresáři výstupu projektu a že jste přidali následující `Imports` a `Using` příkazy do projektu:</span><span class="sxs-lookup"><span data-stu-id="39601-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="39601-1206">Typy konceptuálních modelů (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="39601-1207">Konceptuální schéma definici jazyka (CSDL) podporuje sadu abstraktní primitivní datové typy, volá **EDMSimpleTypes**, které definují vlastnosti v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="39601-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="39601-1208">**EDMSimpleTypes** jsou proxy servery pro primitivní datové typy, které jsou podporovány ve službě storage nebo hostitelského prostředí.</span><span class="sxs-lookup"><span data-stu-id="39601-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="39601-1209">Následující tabulka uvádí primitivní datové typy, které jsou podporovány CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="39601-1210">V tabulce jsou uvedeny také omezující vlastnosti, které lze použít u každého **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="39601-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="39601-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="39601-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="39601-1212">Popis</span><span class="sxs-lookup"><span data-stu-id="39601-1212">Description</span></span>                                                | <span data-ttu-id="39601-1213">Použít omezující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="39601-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="39601-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="39601-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="39601-1215">Obsahuje binární data.</span><span class="sxs-lookup"><span data-stu-id="39601-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="39601-1216">MaxLength, hodnoty, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="39601-1217">**Typem Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="39601-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="39601-1218">Obsahuje hodnotu **true** nebo **false**.</span><span class="sxs-lookup"><span data-stu-id="39601-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="39601-1219">S povolenou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="39601-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="39601-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="39601-1221">Obsahuje hodnotu bez znaménka 8bitové celé číslo.</span><span class="sxs-lookup"><span data-stu-id="39601-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="39601-1222">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="39601-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="39601-1224">Představuje datum a čas.</span><span class="sxs-lookup"><span data-stu-id="39601-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="39601-1225">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="39601-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="39601-1227">Obsahuje data a času jako posun během několika minut od GMT.</span><span class="sxs-lookup"><span data-stu-id="39601-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="39601-1228">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="39601-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="39601-1230">Obsahuje číselná hodnota s pevnou hodnot precision a scale.</span><span class="sxs-lookup"><span data-stu-id="39601-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="39601-1231">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="39601-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="39601-1233">Obsahuje plovoucí desetinnou čárkou s přesností na 15 číslic čísla</span><span class="sxs-lookup"><span data-stu-id="39601-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="39601-1234">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="39601-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="39601-1236">Obsahuje bod s plovoucí desetinnou čárkou s přesností 7 číslic čísla.</span><span class="sxs-lookup"><span data-stu-id="39601-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="39601-1237">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="39601-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="39601-1239">Obsahuje jedinečný identifikátor 16 bajtů.</span><span class="sxs-lookup"><span data-stu-id="39601-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="39601-1240">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="39601-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="39601-1242">Obsahuje hodnotu se znaménkem 16 bitů.</span><span class="sxs-lookup"><span data-stu-id="39601-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="39601-1243">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1244">**Typem Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="39601-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="39601-1245">Obsahuje hodnotu se znaménkem 32-bit.</span><span class="sxs-lookup"><span data-stu-id="39601-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="39601-1246">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="39601-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="39601-1248">Obsahuje hodnotu se znaménkem 64-bit.</span><span class="sxs-lookup"><span data-stu-id="39601-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="39601-1249">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="39601-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="39601-1251">Obsahuje hodnotu se znaménkem 8 bitů.</span><span class="sxs-lookup"><span data-stu-id="39601-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="39601-1252">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="39601-1253">**Edm.String**</span></span>                   | <span data-ttu-id="39601-1254">Obsahuje znaková data.</span><span class="sxs-lookup"><span data-stu-id="39601-1254">Contains character data.</span></span>                                   | <span data-ttu-id="39601-1255">Výchozí kódování Unicode, hodnoty, MaxLength, kolace, přesností, s možnou hodnotou Null,</span><span class="sxs-lookup"><span data-stu-id="39601-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="39601-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="39601-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="39601-1257">Obsahuje denní dobu.</span><span class="sxs-lookup"><span data-stu-id="39601-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="39601-1258">Přesnost, s možnou hodnotou Null, výchozí</span><span class="sxs-lookup"><span data-stu-id="39601-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="39601-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="39601-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="39601-1260">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="39601-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="39601-1262">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="39601-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="39601-1264">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="39601-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="39601-1266">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="39601-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="39601-1268">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="39601-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="39601-1270">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="39601-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="39601-1272">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="39601-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="39601-1274">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="39601-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="39601-1276">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="39601-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="39601-1278">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="39601-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="39601-1280">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="39601-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="39601-1282">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="39601-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="39601-1284">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="39601-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="39601-1286">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="39601-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="39601-1288">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="39601-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="39601-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="39601-1290">S povolenou hodnotou Null, výchozí, SRID</span><span class="sxs-lookup"><span data-stu-id="39601-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="39601-1291">Omezující vlastnosti (CSDL)</span><span class="sxs-lookup"><span data-stu-id="39601-1291">Facets (CSDL)</span></span>

<span data-ttu-id="39601-1292">Omezující vlastnosti v Konceptuální schéma definici jazyka (CSDL) představují omezení na vlastnosti typů entit a komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="39601-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="39601-1293">Omezující vlastnosti zobrazí jako atributy ve formátu XML na následujících prvcích CSDL:</span><span class="sxs-lookup"><span data-stu-id="39601-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="39601-1294">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="39601-1294">Property</span></span>
-   <span data-ttu-id="39601-1295">Odkaz TypeRef</span><span class="sxs-lookup"><span data-stu-id="39601-1295">TypeRef</span></span>
-   <span data-ttu-id="39601-1296">Parametr</span><span class="sxs-lookup"><span data-stu-id="39601-1296">Parameter</span></span>

<span data-ttu-id="39601-1297">Následující tabulka popisuje omezující vlastnosti, které jsou podporovány v CSDL.</span><span class="sxs-lookup"><span data-stu-id="39601-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="39601-1298">Všechny omezující vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="39601-1298">All facets are optional.</span></span> <span data-ttu-id="39601-1299">Některé omezujících vlastností uvedených níže používá Entity Framework při generování databáze z Koncepční model.</span><span class="sxs-lookup"><span data-stu-id="39601-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="39601-1300">Informace o datových typech v konceptuálním modelu najdete v tématu typy koncepční Model (CSDL).</span><span class="sxs-lookup"><span data-stu-id="39601-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="39601-1301">Omezující vlastnost</span><span class="sxs-lookup"><span data-stu-id="39601-1301">Facet</span></span>               | <span data-ttu-id="39601-1302">Popis</span><span class="sxs-lookup"><span data-stu-id="39601-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="39601-1303">Platí pro</span><span class="sxs-lookup"><span data-stu-id="39601-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="39601-1304">Používá pro generování databáze</span><span class="sxs-lookup"><span data-stu-id="39601-1304">Used for the database generation</span></span> | <span data-ttu-id="39601-1305">Použít modul runtime</span><span class="sxs-lookup"><span data-stu-id="39601-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="39601-1306">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="39601-1306">**Collation**</span></span>       | <span data-ttu-id="39601-1307">Určuje pořadí řazení (nebo pořadí řazení) pro použití při provádění porovnání a řazení operací na hodnotách vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="39601-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="39601-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="39601-1309">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1309">Yes</span></span>                              | <span data-ttu-id="39601-1310">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1310">No</span></span>                  |
| <span data-ttu-id="39601-1311">**Režim ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="39601-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="39601-1312">Označuje, že hodnota vlastnosti má být použita pro kontroly optimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="39601-1313">Všechny **EDMSimpleType** vlastnosti</span><span class="sxs-lookup"><span data-stu-id="39601-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="39601-1314">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1314">No</span></span>                               | <span data-ttu-id="39601-1315">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1315">Yes</span></span>                 |
| <span data-ttu-id="39601-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="39601-1316">**Default**</span></span>         | <span data-ttu-id="39601-1317">Určuje výchozí hodnotu vlastnosti, pokud není zadána žádná hodnota, při vytváření instance.</span><span class="sxs-lookup"><span data-stu-id="39601-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="39601-1318">Všechny **EDMSimpleType** vlastnosti</span><span class="sxs-lookup"><span data-stu-id="39601-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="39601-1319">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1319">Yes</span></span>                              | <span data-ttu-id="39601-1320">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1320">Yes</span></span>                 |
| <span data-ttu-id="39601-1321">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="39601-1321">**FixedLength**</span></span>     | <span data-ttu-id="39601-1322">Určuje, zda se může lišit délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="39601-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="39601-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="39601-1324">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1324">Yes</span></span>                              | <span data-ttu-id="39601-1325">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1325">No</span></span>                  |
| <span data-ttu-id="39601-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="39601-1326">**MaxLength**</span></span>       | <span data-ttu-id="39601-1327">Určuje maximální délku hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="39601-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="39601-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="39601-1329">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1329">Yes</span></span>                              | <span data-ttu-id="39601-1330">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1330">No</span></span>                  |
| <span data-ttu-id="39601-1331">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="39601-1331">**Nullable**</span></span>        | <span data-ttu-id="39601-1332">Určuje, zda tato vlastnost může mít **null** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="39601-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="39601-1333">Všechny **EDMSimpleType** vlastnosti</span><span class="sxs-lookup"><span data-stu-id="39601-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="39601-1334">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1334">Yes</span></span>                              | <span data-ttu-id="39601-1335">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1335">Yes</span></span>                 |
| <span data-ttu-id="39601-1336">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="39601-1336">**Precision**</span></span>       | <span data-ttu-id="39601-1337">Pro vlastnosti typu **desítkové**, určuje počet číslic, může mít hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="39601-1338">Pro vlastnosti typu **čas**, **data a času**, a **DateTimeOffset**, určuje počet číslic za desetinnou čárkou sady sekund hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="39601-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="39601-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="39601-1340">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1340">Yes</span></span>                              | <span data-ttu-id="39601-1341">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1341">No</span></span>                  |
| <span data-ttu-id="39601-1342">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="39601-1342">**Scale**</span></span>           | <span data-ttu-id="39601-1343">Určuje počet číslic vpravo od desetinné čárky pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="39601-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="39601-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="39601-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="39601-1345">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1345">Yes</span></span>                              | <span data-ttu-id="39601-1346">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1346">No</span></span>                  |
| <span data-ttu-id="39601-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="39601-1347">**SRID**</span></span>            | <span data-ttu-id="39601-1348">Určuje ID prostorových systému odkaz na systém.</span><span class="sxs-lookup"><span data-stu-id="39601-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="39601-1349">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="39601-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="39601-1350">**Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="39601-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="39601-1351">Ne</span><span class="sxs-lookup"><span data-stu-id="39601-1351">No</span></span>                               | <span data-ttu-id="39601-1352">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1352">Yes</span></span>                 |
| <span data-ttu-id="39601-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="39601-1353">**Unicode**</span></span>         | <span data-ttu-id="39601-1354">Označuje, zda hodnota vlastnosti je uložen jako kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="39601-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="39601-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="39601-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="39601-1356">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1356">Yes</span></span>                              | <span data-ttu-id="39601-1357">Ano</span><span class="sxs-lookup"><span data-stu-id="39601-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="39601-1358">Při generování z Koncepční model databáze, databáze průvodce generovat rozpozná hodnoty **výčet StoreGeneratedPattern** atribut na **vlastnost** elementu, jestliže je v následujícím obor názvů: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="39601-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="39601-1359">Podporované hodnoty pro atribut **Identity** a **vypočítané**.</span><span class="sxs-lookup"><span data-stu-id="39601-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="39601-1360">Hodnota **Identity** vytvoří sloupec databáze hodnotu identity, který je vygenerován v databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="39601-1361">Hodnota **vypočítané** vytvoří sloupec hodnot, které je vypočítán v databázi.</span><span class="sxs-lookup"><span data-stu-id="39601-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="39601-1362">Příklad</span><span class="sxs-lookup"><span data-stu-id="39601-1362">Example</span></span>

<span data-ttu-id="39601-1363">Následující příklad ukazuje použitý pro vlastnosti typu entity omezující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="39601-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
