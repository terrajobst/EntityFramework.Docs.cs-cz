---
title: Specifikace CSDL – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182601"
---
# <a name="csdl-specification"></a><span data-ttu-id="d5ee8-102">Specifikace CSDL</span><span class="sxs-lookup"><span data-stu-id="d5ee8-102">CSDL Specification</span></span>
<span data-ttu-id="d5ee8-103">Jazyk CSDL (konceptuální schéma Definition Language) je jazyk založený na jazyce XML, který popisuje entity, vztahy a funkce tvořící koncepční model aplikace řízené daty.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="d5ee8-104">Tento koncepční model lze použít Entity Framework nebo WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="d5ee8-105">Metadata popsaná pomocí CSDL používá Entity Framework k mapování entit a vztahů, které jsou definovány v koncepčním modelu na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="d5ee8-106">Další informace najdete v tématu Specifikace [SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) a [specifikace MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="d5ee8-107">CSDL je Entity Framework implementace model EDM (Entity Data Model).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="d5ee8-108">V Entity Framework aplikaci jsou metadata konceptuálního modelu načtena ze souboru. csdl (napsaného v CSDL) do instance System. data. Metadata. Edm. EdmItemCollection a je přístupná pomocí metod v System. data. Metadata. Edm. MetadataWorkspace – třída</span><span class="sxs-lookup"><span data-stu-id="d5ee8-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="d5ee8-109">Entity Framework používá metadata konceptuálního modelu k překladu dotazů na koncepční model na příkazy specifické pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="d5ee8-110">Návrhář EF ukládá informace o koncepčním modelu do souboru. edmx v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="d5ee8-111">V okamžiku sestavení používá Návrhář EF informace v souboru. edmx k vytvoření souboru. csdl, který je vyžadován Entity Framework za běhu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="d5ee8-112">Verze CSDL jsou odlišeny obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="d5ee8-113">Verze CSDL</span><span class="sxs-lookup"><span data-stu-id="d5ee8-113">CSDL Version</span></span> | <span data-ttu-id="d5ee8-114">Obor názvů XML</span><span class="sxs-lookup"><span data-stu-id="d5ee8-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="d5ee8-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="d5ee8-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="d5ee8-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="d5ee8-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="d5ee8-117">CSDL V3</span><span class="sxs-lookup"><span data-stu-id="d5ee8-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="d5ee8-118">Association – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-118">Association Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-119">Element **Association** definuje vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="d5ee8-120">Přidružení musí určovat typy entit, které jsou součástí relace, a možný počet typů entit na každém konci relace, který se označuje jako násobnost.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="d5ee8-121">Násobnost elementu end přidružení může mít hodnotu jedna (1), 0 nebo 1 (0.. 1) nebo mnoho (\*).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="d5ee8-122">Tyto informace jsou zadány ve dvou podřízených elementech end.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="d5ee8-123">Instance typu entity na jednom konci přidružení lze zpřístupnit prostřednictvím vlastností navigace nebo cizích klíčů, pokud jsou vystaveny na typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="d5ee8-124">V aplikaci instance přidružení představuje konkrétní přidružení mezi instancemi typů entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="d5ee8-125">Instance přidružení jsou logicky seskupeny v sadě přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="d5ee8-126">Element **Association** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-127">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-128">End (přesně 2 elementy)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="d5ee8-129">Elementu ReferentialConstraint (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-130">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-131">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-131">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-132">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Association** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="d5ee8-133">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-133">Attribute Name</span></span> | <span data-ttu-id="d5ee8-134">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-134">Is Required</span></span> | <span data-ttu-id="d5ee8-135">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="d5ee8-136">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-136">**Name**</span></span>       | <span data-ttu-id="d5ee8-137">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-137">Yes</span></span>         | <span data-ttu-id="d5ee8-138">Název přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-139">Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="d5ee8-140">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-141">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-142">Example</span></span>

<span data-ttu-id="d5ee8-143">Následující příklad ukazuje element **Association** definující přidružení **CustomerOrders** v případě, že cizí klíče nebyly zveřejněny na typech entit **zákazníka** a **objednávky** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="d5ee8-144">Hodnoty **násobnosti** pro každý **konec** přidružení označují, že mnoho **objednávek** může být přidruženo k **zákazníkovi**, ale k **objednávce**je možné přidružit pouze jednoho **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="d5ee8-145">Kromě toho element **IsDeleted** označuje, že všechny **objednávky** , které souvisejí s konkrétním **zákazníkem** a byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="d5ee8-146">Následující příklad ukazuje element **Association** definující přidružení **CustomerOrders** , pokud byly cizí klíče vystaveny na typech entit **Zákazník** a **objednávka** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="d5ee8-147">Při zpřístupnění cizích klíčů je vztah mezi entitami spravován pomocí elementu **elementu ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="d5ee8-148">Odpovídající element AssociationSetMapping není nutný k mapování tohoto přidružení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="d5ee8-149">AssociationSet – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-150">Element **AssociationSet** v jazyce CSDL (konceptuální schéma Definition Language) je logický kontejner pro instance přidružení stejného typu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="d5ee8-151">Sada přidružení poskytuje definici pro seskupení instancí přidružení, aby bylo možné je namapovat na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="d5ee8-152">Element **AssociationSet** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-153">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-154">End (vyžaduje se přesně dva prvky)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="d5ee8-155">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="d5ee8-156">Atribut **Association** určuje typ přidružení, které sada přidružení obsahuje.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="d5ee8-157">Sady entit, které tvoří konce sady přidružení, jsou zadány pomocí přesně dvou podřízených elementů **End** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-158">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-158">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-159">Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="d5ee8-160">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-160">Attribute Name</span></span>  | <span data-ttu-id="d5ee8-161">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-161">Is Required</span></span> | <span data-ttu-id="d5ee8-162">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-163">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-163">**Name**</span></span>        | <span data-ttu-id="d5ee8-164">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-164">Yes</span></span>         | <span data-ttu-id="d5ee8-165">Název sady entit</span><span class="sxs-lookup"><span data-stu-id="d5ee8-165">The name of the entity set.</span></span> <span data-ttu-id="d5ee8-166">Hodnota atributu **Name** nemůže být stejná jako hodnota atributu **Association** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="d5ee8-167">**Přidružení**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-167">**Association**</span></span> | <span data-ttu-id="d5ee8-168">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-168">Yes</span></span>         | <span data-ttu-id="d5ee8-169">Plně kvalifikovaný název asociace, který sada přidružení obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="d5ee8-170">Přidružení musí být ve stejném oboru názvů jako sada přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-171">Pro element **AssociationSet** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="d5ee8-172">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-173">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-174">Example</span></span>

<span data-ttu-id="d5ee8-175">Následující příklad ukazuje element **EntityContainer** se dvěma elementy **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="d5ee8-176">CollectionType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-177">Element **CollectionType** v jazyce CSDL (konceptuální schéma Definition Language) určuje, že parametr funkce nebo návratový typ funkce je kolekce.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="d5ee8-178">Element **CollectionType** může být podřízeným elementem parametru nebo elementem ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="d5ee8-179">Typ kolekce lze zadat buď pomocí atributu **Type** , nebo jednoho z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="d5ee8-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-180">**CollectionType**</span></span>
-   <span data-ttu-id="d5ee8-181">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-181">ReferenceType</span></span>
-   <span data-ttu-id="d5ee8-182">RowType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-182">RowType</span></span>
-   <span data-ttu-id="d5ee8-183">Odkaz</span><span class="sxs-lookup"><span data-stu-id="d5ee8-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-184">Model nebude ověřen, pokud je zadán typ kolekce s atributem **typu** i s podřízeným prvkem.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-185">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-185">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-186">Následující tabulka popisuje atributy, které mohou být aplikovány na element **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="d5ee8-187">Všimněte si, že atributy **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**a **kolace** platí pouze pro kolekce **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="d5ee8-188">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-188">Attribute Name</span></span>                                                          | <span data-ttu-id="d5ee8-189">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-189">Is Required</span></span> | <span data-ttu-id="d5ee8-190">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-191">**Type**</span></span>                                                                | <span data-ttu-id="d5ee8-192">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-192">No</span></span>          | <span data-ttu-id="d5ee8-193">Typ kolekce.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="d5ee8-194">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-194">**Nullable**</span></span>                                                            | <span data-ttu-id="d5ee8-195">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-195">No</span></span>          | <span data-ttu-id="d5ee8-196">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="d5ee8-197">> Ve službě CSDL V1 musí mít vlastnost komplexního typu `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="d5ee8-198">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="d5ee8-199">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-199">No</span></span>          | <span data-ttu-id="d5ee8-200">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="d5ee8-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="d5ee8-202">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-202">No</span></span>          | <span data-ttu-id="d5ee8-203">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="d5ee8-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="d5ee8-205">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-205">No</span></span>          | <span data-ttu-id="d5ee8-206">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="d5ee8-207">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-207">**Precision**</span></span>                                                           | <span data-ttu-id="d5ee8-208">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-208">No</span></span>          | <span data-ttu-id="d5ee8-209">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="d5ee8-210">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-210">**Scale**</span></span>                                                               | <span data-ttu-id="d5ee8-211">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-211">No</span></span>          | <span data-ttu-id="d5ee8-212">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="d5ee8-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-213">**SRID**</span></span>                                                                | <span data-ttu-id="d5ee8-214">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-214">No</span></span>          | <span data-ttu-id="d5ee8-215">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="d5ee8-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d5ee8-216">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="d5ee8-217">   Další informace najdete v tématech [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="d5ee8-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-218">**Unicode**</span></span>                                                             | <span data-ttu-id="d5ee8-219">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-219">No</span></span>          | <span data-ttu-id="d5ee8-220">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="d5ee8-221">**Velké**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-221">**Collation**</span></span>                                                           | <span data-ttu-id="d5ee8-222">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-222">No</span></span>          | <span data-ttu-id="d5ee8-223">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-224">Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="d5ee8-225">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-226">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-227">Example</span></span>

<span data-ttu-id="d5ee8-228">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci typů entit **Person** (jak je určeno atributem **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="d5ee8-229">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="d5ee8-230">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce přijímá jako parametr kolekce typů entit **oddělení** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="d5ee8-231">ComplexType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-232">Element **complexType** definuje strukturu dat složenou z vlastností **EdmSimpleType** nebo jiných komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="d5ee8-233">  Komplexní typ může být vlastnost typu entity nebo jiného komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="d5ee8-234">Komplexní typ je podobný typu entity v tom, že komplexní typ definuje data.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="d5ee8-235">Existují však některé klíčové rozdíly mezi komplexními typy a typy entit:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="d5ee8-236">Komplexní typy nemají identity (nebo klíče), a proto nemohou existovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="d5ee8-237">Komplexní typy mohou existovat pouze jako vlastnosti typů entit nebo jiných komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="d5ee8-238">Komplexní typy se nemůžou účastnit přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="d5ee8-239">Ani konec přidružení může být komplexní typ, a proto nelze definovat vlastnosti navigace pro komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="d5ee8-240">Vlastnost komplexního typu nemůže mít hodnotu null, ačkoliv skalární vlastnosti komplexního typu mohou být nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="d5ee8-241">Element **complexType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-242">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-243">Vlastnost (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="d5ee8-244">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="d5ee8-245">Následující tabulka popisuje atributy, které lze použít pro element **complexType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="d5ee8-246">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="d5ee8-247">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-247">Is Required</span></span> | <span data-ttu-id="d5ee8-248">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-249">Name</span><span class="sxs-lookup"><span data-stu-id="d5ee8-249">Name</span></span>                                                                                                           | <span data-ttu-id="d5ee8-250">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-250">Yes</span></span>         | <span data-ttu-id="d5ee8-251">Název komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-251">The name of the complex type.</span></span> <span data-ttu-id="d5ee8-252">Název komplexního typu nemůže být stejný jako název jiného komplexního typu, typu entity nebo asociace, který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="d5ee8-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="d5ee8-254">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-254">No</span></span>          | <span data-ttu-id="d5ee8-255">Název jiného komplexního typu, který je základním typem komplexního typu, který je definován.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="d5ee8-256">> Tento atribut nelze použít v CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="d5ee8-257">Dědičnost pro komplexní typy není v této verzi podporována.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="d5ee8-258">Abstraktní</span><span class="sxs-lookup"><span data-stu-id="d5ee8-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="d5ee8-259">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-259">No</span></span>          | <span data-ttu-id="d5ee8-260">**True** nebo **false** (výchozí hodnota) v závislosti na tom, zda je komplexní typ abstraktní typ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="d5ee8-261">> Tento atribut nelze použít v CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="d5ee8-262">Komplexní typy v této verzi nemohou být abstraktní typy.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-263">Pro element **complexType** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="d5ee8-264">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-265">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-266">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-266">Example</span></span>

<span data-ttu-id="d5ee8-267">Následující příklad ukazuje komplexní typ a **adresu**s vlastností **EdmSimpleType** **StreetAddress**, **City**, **StateOrProvince**, **Country**a **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="d5ee8-268">Chcete-li definovat **adresu** komplexního typu (výše) jako vlastnost typu entity, je nutné deklarovat typ vlastnosti v definici typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="d5ee8-269">Následující příklad ukazuje vlastnost **adresa** jako komplexní typ pro typ entity (**Vydavatel**):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="d5ee8-270">DefiningExpression – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-271">Element **DefiningExpression** v jazyce CSDL (konceptuální schéma Definition Language) obsahuje výraz Entity SQL, který definuje funkci v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="d5ee8-272">Pro účely ověřování může element **DefiningExpression** obsahovat libovolný obsah.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="d5ee8-273">Nicméně Entity Framework vyvolá výjimku za běhu, pokud element **DefiningExpression** neobsahuje platný Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-274">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-274">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-275">Pro element **DefiningExpression** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="d5ee8-276">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-277">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-278">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-278">Example</span></span>

<span data-ttu-id="d5ee8-279">Následující příklad používá element **DefiningExpression** k definování funkce, která vrátí počet roků od publikování knihy.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="d5ee8-280">Obsah elementu **DefiningExpression** je napsaný v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="d5ee8-281">Závislý element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-282">**Závislý** element v jazyce CSDL (konceptuální schéma Definition Language) je podřízeným elementem elementu elementu ReferentialConstraint a definuje závislý konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="d5ee8-283">Element **elementu ReferentialConstraint** definuje funkčnost, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="d5ee8-284">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="d5ee8-285">Odkazovaný typ entity se nazývá *hlavní konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="d5ee8-286">Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="d5ee8-287">Prvky **PropertyRef** slouží k určení, které klíče odkazují na hlavní element end.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="d5ee8-288">**Závislý** element může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-289">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="d5ee8-290">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-291">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-291">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-292">Následující tabulka popisuje atributy, které mohou být aplikovány na **závislý** element.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="d5ee8-293">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-293">Attribute Name</span></span> | <span data-ttu-id="d5ee8-294">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-294">Is Required</span></span> | <span data-ttu-id="d5ee8-295">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-296">**Role**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-296">**Role**</span></span>       | <span data-ttu-id="d5ee8-297">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-297">Yes</span></span>         | <span data-ttu-id="d5ee8-298">Název typu entity na závislém konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-299">Pro **závislý** element lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="d5ee8-300">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-301">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-302">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-302">Example</span></span>

<span data-ttu-id="d5ee8-303">Následující příklad ukazuje element **elementu ReferentialConstraint** , který se používá jako součást definice asociace **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="d5ee8-304">Vlastnost **PublisherId** typu entity **Book** vytváří závislý konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="d5ee8-305">Element dokumentace (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-306">Element **dokumentace** v jazyce CSDL (konceptuální schéma Definition Language) lze použít k poskytnutí informací o objektu, který je definován v nadřazeném elementu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="d5ee8-307">V souboru. edmx, pokud je prvek **dokumentace** podřízenosti prvku, který se zobrazí jako objekt na návrhové ploše návrháře EF (například entita, asociace nebo vlastnost), obsah elementu **dokumentace** se zobrazí v Okno **vlastností** sady Visual Studio pro daný objekt.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="d5ee8-308">Prvek **dokumentace** může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-309">**Souhrn**: Stručný popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="d5ee8-310">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-310">(zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-311">**Longdescription**: Rozsáhlý popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="d5ee8-312">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-312">(zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-313">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="d5ee8-313">Annotation elements.</span></span> <span data-ttu-id="d5ee8-314">(nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-315">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-315">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-316">Pro prvek **dokumentace** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="d5ee8-317">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-318">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-319">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-319">Example</span></span>

<span data-ttu-id="d5ee8-320">Následující příklad ukazuje prvek **dokumentace** jako podřízený prvek elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="d5ee8-321">Pokud následující fragment kódu byl v obsahu CSDL souboru. edmx, obsah **souhrnu** a elementů **Longdescription** se zobrazí v okně **vlastnosti** sady Visual Studio, když kliknete na typ entity `Customer`.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="d5ee8-322">End – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-322">End Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-323">**Koncovým** elementem v jazyce CSDL (koncepční schéma Definition Language) může být podřízeným prvkem elementu Association nebo elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="d5ee8-324">V každém případě je role elementu **End** odlišná a příslušné atributy se liší.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="d5ee8-325">Ukončit element jako podřízený elementu přidružení</span><span class="sxs-lookup"><span data-stu-id="d5ee8-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="d5ee8-326">Element **End** (jako podřízený prvek elementu **Association** ) identifikuje typ entity na jednom konci přidružení a počet instancí typu entity, které mohou existovat na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="d5ee8-327">Zakončení přidružení jsou definována jako součást přidružení; přidružení musí mít přesně dvě zakončení přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="d5ee8-328">Instance typu entity na jednom konci přidružení lze zpřístupnit prostřednictvím vlastností navigace nebo cizích klíčů, pokud jsou vystaveny na typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="d5ee8-329">Element **End** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-330">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-331">Při odstranění (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-332">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-333">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-333">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-334">Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízenou položkou elementu **Association** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="d5ee8-335">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-335">Attribute Name</span></span>   | <span data-ttu-id="d5ee8-336">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-336">Is Required</span></span> | <span data-ttu-id="d5ee8-337">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-338">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-338">**Type**</span></span>         | <span data-ttu-id="d5ee8-339">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-339">Yes</span></span>         | <span data-ttu-id="d5ee8-340">Název typu entity na jednom konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="d5ee8-341">**Role**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-341">**Role**</span></span>         | <span data-ttu-id="d5ee8-342">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-342">No</span></span>          | <span data-ttu-id="d5ee8-343">Název zakončení přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-343">A name for the association end.</span></span> <span data-ttu-id="d5ee8-344">Pokud není zadaný žádný název, použije se název typu entity na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="d5ee8-345">**Násobnost**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-345">**Multiplicity**</span></span> | <span data-ttu-id="d5ee8-346">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-346">Yes</span></span>         | <span data-ttu-id="d5ee8-347">**1**, **0.. 1**nebo **\*** v závislosti na počtu instancí typu entity, které mohou být na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="d5ee8-348">**1** znamená, že na konci přidružení existuje přesně jedna instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="d5ee8-349">**0.. 1** znamená, že na konci přidružení existují žádné nebo jedna instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="d5ee8-350">**\*** označuje, že na konci přidružení existují žádné instance typu entity s hodnotou nula, jedna nebo více.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-351">Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="d5ee8-352">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-353">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d5ee8-354">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-354">Example</span></span>

<span data-ttu-id="d5ee8-355">Následující příklad ukazuje element **Association** , který definuje přidružení **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="d5ee8-356">Hodnoty **násobnosti** pro každý **konec** přidružení označují, že mnoho **objednávek** může být přidruženo k **zákazníkovi**, ale k **objednávce**je možné přidružit pouze jednoho **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="d5ee8-357">Kromě toho element **IsDeleted** označuje, že všechny **objednávky** , které se vztahují k určitému **zákazníkovi** a které byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="d5ee8-358">Ukončit element jako podřízený element elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="d5ee8-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="d5ee8-359">Element **End** Určuje jeden konec sady přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="d5ee8-360">Element **AssociationSet** musí obsahovat dva elementy **End** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="d5ee8-361">Informace obsažené v elementu **End** se používají v mapování sady přidružení na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="d5ee8-362">Element **End** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-363">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-364">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-365">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="d5ee8-366">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-367">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-367">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-368">Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízeným elementem elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="d5ee8-369">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-369">Attribute Name</span></span> | <span data-ttu-id="d5ee8-370">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-370">Is Required</span></span> | <span data-ttu-id="d5ee8-371">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-372">**Sada**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-372">**EntitySet**</span></span>  | <span data-ttu-id="d5ee8-373">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-373">Yes</span></span>         | <span data-ttu-id="d5ee8-374">Název elementu **EntitySet** , který definuje jeden element end nadřazeného elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="d5ee8-375">Element **EntitySet** musí být definován ve stejném kontejneru entity jako nadřazený element **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="d5ee8-376">**Role**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-376">**Role**</span></span>       | <span data-ttu-id="d5ee8-377">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-377">No</span></span>          | <span data-ttu-id="d5ee8-378">Název zakončení sady přidružení</span><span class="sxs-lookup"><span data-stu-id="d5ee8-378">The name of the association set end.</span></span> <span data-ttu-id="d5ee8-379">Pokud se atribut **role** nepoužívá, název zakončení sady přidružení bude název sady entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-380">Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="d5ee8-381">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-382">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d5ee8-383">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-383">Example</span></span>

<span data-ttu-id="d5ee8-384">Následující příklad ukazuje element **EntityContainer** se dvěma elementy **AssociationSet** , z nichž každý má dva elementy **End** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="d5ee8-385">EntityContainer – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-386">Element **EntityContainer** v jazyce CSDL (konceptuální schéma Definition Language) je logický kontejner pro sady entit, sady přidružení a importy funkcí.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="d5ee8-387">Kontejner entity koncepčního modelu se mapuje na kontejner entit modelu úložiště prostřednictvím elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="d5ee8-388">Kontejner entity modelu úložiště popisuje strukturu databáze: sady entit popisují tabulky, sady přidružení popisují omezení cizího klíče a importy funkcí popisují uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="d5ee8-389">Element **EntityContainer** může mít nula nebo jeden prvek dokumentace.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="d5ee8-390">Pokud je přítomen prvek **dokumentace** , musí předcházet všechny elementy **EntitySet**, **AssociationSet**a **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="d5ee8-391">Element **EntityContainer** může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-392">Sada</span><span class="sxs-lookup"><span data-stu-id="d5ee8-392">EntitySet</span></span>
-   <span data-ttu-id="d5ee8-393">Vlastností</span><span class="sxs-lookup"><span data-stu-id="d5ee8-393">AssociationSet</span></span>
-   <span data-ttu-id="d5ee8-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="d5ee8-394">FunctionImport</span></span>
-   <span data-ttu-id="d5ee8-395">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="d5ee8-395">Annotation elements</span></span>

<span data-ttu-id="d5ee8-396">Element **EntityContainer** můžete roztáhnout tak, aby zahrnoval obsah jiného **elementu EntityContainer** , který je ve stejném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="d5ee8-397">Chcete-li zahrnout obsah jiného **elementu EntityContainer**, nastavte v odkazujícím elementu **EntityContainer** hodnotu atributu **extends** na název elementu **EntityContainer** , který chcete zahrnout.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="d5ee8-398">Všechny podřízené elementy vloženého elementu **EntityContainer** budou považovány za podřízené prvky odkazujícího elementu **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-399">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-399">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-400">Následující tabulka popisuje atributy, které lze použít pro element **using** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="d5ee8-401">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-401">Attribute Name</span></span> | <span data-ttu-id="d5ee8-402">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-402">Is Required</span></span> | <span data-ttu-id="d5ee8-403">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="d5ee8-404">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-404">**Name**</span></span>       | <span data-ttu-id="d5ee8-405">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-405">Yes</span></span>         | <span data-ttu-id="d5ee8-406">Název kontejneru entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="d5ee8-407">**Nachází**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-407">**Extends**</span></span>    | <span data-ttu-id="d5ee8-408">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-408">No</span></span>          | <span data-ttu-id="d5ee8-409">Název jiného kontejneru entity v rámci stejného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-410">Pro element **EntityContainer** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="d5ee8-411">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-412">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-413">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-413">Example</span></span>

<span data-ttu-id="d5ee8-414">Následující příklad ukazuje element **EntityContainer** , který definuje tři sady entit a dvě sady přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="d5ee8-415">EntitySet – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-416">Element **EntitySet** v jazyce koncepční definice schématu je logický kontejner pro instance typu entity a instance libovolného typu, který je odvozen z daného typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="d5ee8-417">Vztah mezi typem entity a sadou entit je podobný relaci mezi řádkem a tabulkou v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="d5ee8-418">Podobně jako u řádku typ entity definuje sadu souvisejících dat, podobně jako tabulka, sada entit obsahuje instance této definice.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="d5ee8-419">Sada entit poskytuje konstrukci pro seskupování instancí typů entit tak, aby mohly být mapovány na související datové struktury ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="d5ee8-420">Je možné definovat více než jednu sadu entit pro konkrétní typ entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-421">Návrhář EF nepodporuje koncepční modely, které obsahují více sad entit na typ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="d5ee8-422">Element **EntitySet** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-423">Element dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-424">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-425">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-425">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-426">Následující tabulka popisuje atributy, které lze použít pro element **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="d5ee8-427">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-427">Attribute Name</span></span> | <span data-ttu-id="d5ee8-428">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-428">Is Required</span></span> | <span data-ttu-id="d5ee8-429">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-430">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-430">**Name**</span></span>       | <span data-ttu-id="d5ee8-431">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-431">Yes</span></span>         | <span data-ttu-id="d5ee8-432">Název sady entit</span><span class="sxs-lookup"><span data-stu-id="d5ee8-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="d5ee8-433">**Objektu**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-433">**EntityType**</span></span> | <span data-ttu-id="d5ee8-434">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-434">Yes</span></span>         | <span data-ttu-id="d5ee8-435">Plně kvalifikovaný název typu entity, pro který sada entit obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-436">Pro element **EntitySet** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="d5ee8-437">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-438">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-439">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-439">Example</span></span>

<span data-ttu-id="d5ee8-440">Následující příklad ukazuje element **EntityContainer** se třemi elementy **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="d5ee8-441">Je možné definovat několik sad entit na typ (MEST).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="d5ee8-442">Následující příklad definuje kontejner entit se dvěma sadami entit pro typ entity **Book** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="d5ee8-443">EntityType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-444">Element **EntityType** reprezentuje strukturu konceptu nejvyšší úrovně, jako je například zákazník nebo objednávka, v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="d5ee8-445">Typ entity je šablona pro instance typů entit v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="d5ee8-446">Každá šablona obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="d5ee8-447">Jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-447">A unique name.</span></span> <span data-ttu-id="d5ee8-448">(Povinné.)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-448">(Required.)</span></span>
-   <span data-ttu-id="d5ee8-449">Klíč entity, který je definován jednou nebo více vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="d5ee8-450">(Povinné.)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-450">(Required.)</span></span>
-   <span data-ttu-id="d5ee8-451">Vlastnosti pro obsahující data</span><span class="sxs-lookup"><span data-stu-id="d5ee8-451">Properties for containing data.</span></span> <span data-ttu-id="d5ee8-452">(Volitelné.)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-452">(Optional.)</span></span>
-   <span data-ttu-id="d5ee8-453">Navigační vlastnosti, které umožňují navigaci od jednoho konce přidružení k druhému konci.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="d5ee8-454">(Volitelné.)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-454">(Optional.)</span></span>

<span data-ttu-id="d5ee8-455">V aplikaci instance typu entity představuje konkrétní objekt (například konkrétního zákazníka nebo objednávky).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="d5ee8-456">Každá instance typu entity musí mít v sadě entit jedinečný klíč entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="d5ee8-457">Dvě instance typu entity jsou považovány za rovné pouze v případě, že jsou stejného typu a hodnoty jejich klíčů entit jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="d5ee8-458">Element **EntityType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-459">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-460">Klíč (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-461">Vlastnost (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="d5ee8-462">NavigationProperty (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="d5ee8-463">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-464">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-464">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-465">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="d5ee8-466">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="d5ee8-467">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-467">Is Required</span></span> | <span data-ttu-id="d5ee8-468">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-469">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="d5ee8-470">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-470">Yes</span></span>         | <span data-ttu-id="d5ee8-471">Název typu entity</span><span class="sxs-lookup"><span data-stu-id="d5ee8-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="d5ee8-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="d5ee8-473">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-473">No</span></span>          | <span data-ttu-id="d5ee8-474">Název jiného typu entity, který je základním typem entity typu, který je definován.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="d5ee8-475">**Výtah**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="d5ee8-476">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-476">No</span></span>          | <span data-ttu-id="d5ee8-477">**Hodnota true** nebo **false**v závislosti na tom, zda je typ entity abstraktní typ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="d5ee8-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="d5ee8-479">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-479">No</span></span>          | <span data-ttu-id="d5ee8-480">**True** nebo **false** v závislosti na tom, zda je typ entity typ otevřené entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="d5ee8-481">> Atributu **OpenType** platí pouze pro typy entit, které jsou definovány v koncepčních modelech, které jsou používány s Data Services ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-482">Pro element **EntityType** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="d5ee8-483">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-484">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-485">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-485">Example</span></span>

<span data-ttu-id="d5ee8-486">Následující příklad ukazuje element **EntityType** se třemi prvky **vlastností** a dvěma elementy **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="d5ee8-487">EnumType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-488">Element **enumType** reprezentuje Výčtový typ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="d5ee8-489">Element **enumType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-490">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-491">Člen (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="d5ee8-492">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-493">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-493">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-494">Následující tabulka popisuje atributy, které mohou být aplikovány na element **enumType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="d5ee8-495">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-495">Attribute Name</span></span>     | <span data-ttu-id="d5ee8-496">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-496">Is Required</span></span> | <span data-ttu-id="d5ee8-497">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-498">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-498">**Name**</span></span>           | <span data-ttu-id="d5ee8-499">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-499">Yes</span></span>         | <span data-ttu-id="d5ee8-500">Název typu entity</span><span class="sxs-lookup"><span data-stu-id="d5ee8-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="d5ee8-501">**Příznak**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-501">**IsFlags**</span></span>        | <span data-ttu-id="d5ee8-502">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-502">No</span></span>          | <span data-ttu-id="d5ee8-503">**Hodnota true** nebo **false**v závislosti na tom, zda lze typ výčtu použít jako sadu příznaků.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="d5ee8-504">Výchozí hodnota je **false.** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="d5ee8-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-505">**UnderlyingType**</span></span> | <span data-ttu-id="d5ee8-506">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-506">No</span></span>          | <span data-ttu-id="d5ee8-507">**EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** nebo **EDM. SByte** definující rozsah hodnot typu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="d5ee8-508">  Výchozí podkladový typ prvků výčtu je **EDM. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-509">Pro element **enumType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="d5ee8-510">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-511">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-512">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-512">Example</span></span>

<span data-ttu-id="d5ee8-513">Následující příklad ukazuje element **enumType** se třemi prvky **členů** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="d5ee8-514">Function – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-514">Function Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-515">Element **Function** v jazyce CSDL (konceptuální schéma Definition Language) se používá k definování nebo deklaraci funkcí v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="d5ee8-516">Funkce je definována pomocí elementu DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="d5ee8-517">Element **Function** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-518">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-519">Parametr (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="d5ee8-520">DefiningExpression (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-521">ReturnType (Function) (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-522">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="d5ee8-523">Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** (Function), nebo atributu **ReturnType** (viz níže), ale nikoli obou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="d5ee8-524">Možné návratové typy jsou jakékoli EdmSimpleType, typ entity, komplexní typ, typ řádku nebo typ ref (nebo kolekce jednoho z těchto typů).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-525">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-525">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-526">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Function** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="d5ee8-527">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-527">Attribute Name</span></span> | <span data-ttu-id="d5ee8-528">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-528">Is Required</span></span> | <span data-ttu-id="d5ee8-529">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="d5ee8-530">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-530">**Name**</span></span>       | <span data-ttu-id="d5ee8-531">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-531">Yes</span></span>         | <span data-ttu-id="d5ee8-532">Název funkce</span><span class="sxs-lookup"><span data-stu-id="d5ee8-532">The name of the function.</span></span>          |
| <span data-ttu-id="d5ee8-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-533">**ReturnType**</span></span> | <span data-ttu-id="d5ee8-534">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-534">No</span></span>          | <span data-ttu-id="d5ee8-535">Typ vrácený funkcí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-536">Pro element **Function** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="d5ee8-537">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-538">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-539">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-539">Example</span></span>

<span data-ttu-id="d5ee8-540">Následující příklad používá element **Function** k definování funkce, která vrátí počet roků od přijetí instruktora.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="d5ee8-541">FunctionImport – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-542">Element **FunctionImport** v koncepčním definičním jazyce (CSDL) představuje funkci, která je definována ve zdroji dat, ale je k dispozici pro objekty prostřednictvím koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="d5ee8-543">Například prvek funkce v modelu úložiště lze použít k reprezentaci uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="d5ee8-544">Element **FunctionImport** v koncepčním modelu představuje odpovídající funkci v aplikaci Entity Framework a je mapována na funkci modelu úložiště pomocí elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="d5ee8-545">Když je funkce volána v aplikaci, odpovídající uložená procedura je spuštěna v databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="d5ee8-546">Element **FunctionImport** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-547">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-548">Parametr (povolený nula nebo více elementů)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-549">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-550">ReturnType (FunctionImport) (povolené nula nebo více elementů)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="d5ee8-551">Jeden prvek **parametru** by měl být definován pro každý parametr, který funkce přijímá.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="d5ee8-552">Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** (FunctionImport), nebo atributu **ReturnType** (viz níže), ale nikoli obou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="d5ee8-553">Hodnota návratového typu musí být kolekce elementů EdmSimpleType, EntityType nebo ComplexType.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-554">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-554">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-555">Následující tabulka popisuje atributy, které lze použít pro element **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="d5ee8-556">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-556">Attribute Name</span></span>   | <span data-ttu-id="d5ee8-557">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-557">Is Required</span></span> | <span data-ttu-id="d5ee8-558">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-559">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-559">**Name**</span></span>         | <span data-ttu-id="d5ee8-560">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-560">Yes</span></span>         | <span data-ttu-id="d5ee8-561">Název importované funkce</span><span class="sxs-lookup"><span data-stu-id="d5ee8-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="d5ee8-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-562">**ReturnType**</span></span>   | <span data-ttu-id="d5ee8-563">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-563">No</span></span>          | <span data-ttu-id="d5ee8-564">Typ, který funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-564">The type that the function returns.</span></span> <span data-ttu-id="d5ee8-565">Nepoužívejte tento atribut, pokud funkce nevrátí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="d5ee8-566">V opačném případě musí být hodnotou kolekce ComplexType, EntityType nebo EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="d5ee8-567">**Sada**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-567">**EntitySet**</span></span>    | <span data-ttu-id="d5ee8-568">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-568">No</span></span>          | <span data-ttu-id="d5ee8-569">Pokud funkce vrátí kolekci typů entit, hodnota **objektu EntitySet** musí být sada entit, do které kolekce patří.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="d5ee8-570">V opačném případě nesmí být použit atribut **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="d5ee8-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-571">**IsComposable**</span></span> | <span data-ttu-id="d5ee8-572">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-572">No</span></span>          | <span data-ttu-id="d5ee8-573">Pokud je hodnota nastavena na true, funkce je vytvořena (funkce vracející tabulku) a lze ji použít v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="d5ee8-574">  Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-575">Pro element **FunctionImport** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="d5ee8-576">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-577">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-578">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-578">Example</span></span>

<span data-ttu-id="d5ee8-579">Následující příklad ukazuje element **FunctionImport** , který přijímá jeden parametr a vrací kolekci typů entit:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="d5ee8-580">Key – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-580">Key Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-581">**Klíčový** prvek je podřízený prvek elementu EntityType a definuje *klíč entity* (vlastnost nebo sadu vlastností typu entity, který určuje identitu).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="d5ee8-582">Vlastnosti, které tvoří klíč entity, se volí v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="d5ee8-583">Hodnoty vlastností klíče entity musí jedinečně určovat instanci typu entity v sadě entit za běhu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="d5ee8-584">Pro zajištění jedinečnosti instancí v sadě entit by se měla zvolit vlastnosti, které tvoří klíč entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="d5ee8-585">**Klíčový** prvek definuje klíč entity odkazem na jednu nebo více vlastností typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="d5ee8-586">**Klíčový** prvek může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="d5ee8-587">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="d5ee8-588">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-589">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-589">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-590">Pro **klíčový** element lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="d5ee8-591">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-592">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-593">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-593">Example</span></span>

<span data-ttu-id="d5ee8-594">Následující příklad definuje typ entity s názvem **Book**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="d5ee8-595">Klíč entity je definován pomocí odkazu na vlastnost **ISBN** typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="d5ee8-596">Vlastnost **ISBN** je dobrou volbou pro klíč entity, protože mezinárodní číslo knihy (ISBN) jednoznačně identifikuje knihu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="d5ee8-597">Následující příklad ukazuje typ entity (**Autor**), který má klíč entity, který se skládá ze dvou vlastností, **názvů** a **adres**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="d5ee8-598">Použití **názvu** a **adresy** pro klíč entity je rozumnou volbou, protože dva autoři se stejným názvem pravděpodobně nebudou v provozu na stejné adrese.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="d5ee8-599">Tato volba pro klíč entity ale v sadě entit naprosto nezaručuje jedinečné klíče entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="d5ee8-600">V tomto případě se doporučuje přidat vlastnost, jako je například **AuthorID**, která se dá použít k jednoznačné identifikaci autora.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="d5ee8-601">Element member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-601">Member Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-602">**Členský** prvek je podřízeným prvkem prvku enumType a definuje člena výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-603">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-603">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-604">Následující tabulka popisuje atributy, které lze použít pro element **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="d5ee8-605">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-605">Attribute Name</span></span> | <span data-ttu-id="d5ee8-606">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-606">Is Required</span></span> | <span data-ttu-id="d5ee8-607">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-608">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-608">**Name**</span></span>       | <span data-ttu-id="d5ee8-609">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-609">Yes</span></span>         | <span data-ttu-id="d5ee8-610">Název člena.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="d5ee8-611">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-611">**Value**</span></span>      | <span data-ttu-id="d5ee8-612">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-612">No</span></span>          | <span data-ttu-id="d5ee8-613">Hodnota člena.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-613">The value of the member.</span></span> <span data-ttu-id="d5ee8-614">Ve výchozím nastavení má první člen hodnotu 0 a hodnota každého úspěšného enumerátoru se zvýší o 1.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="d5ee8-615">Může existovat více členů se stejnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-616">Pro element **FunctionImport** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="d5ee8-617">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-618">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-619">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-619">Example</span></span>

<span data-ttu-id="d5ee8-620">Následující příklad ukazuje element **enumType** se třemi prvky **členů** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="d5ee8-621">NavigationProperty – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-622">Element **NavigationProperty** definuje vlastnost navigace, která poskytuje odkaz na druhý konec přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="d5ee8-623">Na rozdíl od vlastností, které jsou definovány pomocí elementu Property, navigační vlastnosti nedefinují tvar a vlastnosti dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="d5ee8-624">Poskytují způsob, jak procházet přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="d5ee8-625">Všimněte si, že navigační vlastnosti jsou volitelné na obou typech entit na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="d5ee8-626">Definujete-li vlastnost navigace na jednom typu entity na konci přidružení, nemusíte definovat vlastnost navigace na typu entity na druhém konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="d5ee8-627">Datový typ vrácený navigační vlastností je určen násobností jeho vzdáleného zakončení přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="d5ee8-628">Předpokládejme například, že vlastnost navigace **OrdersNavProp**existuje v typu entity **zákazníka** a prochází přidružení 1:1 mezi **zákazníkem** a **objednávkou**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="d5ee8-629">Vzhledem k tomu, že vzdálené zakončení přidružení pro navigační vlastnost má násobnost mnoho (\*), je jeho datovým typem kolekce ( **pořadí**).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="d5ee8-630">Podobně platí, že pokud v typu entity **objednávky** existuje navigační vlastnost **CustomerNavProp**, její datový typ by byl **zákazníkem** , protože násobnost vzdáleného elementu end je jedna (1).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="d5ee8-631">Element **NavigationProperty** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-632">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-633">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-634">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-634">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-635">Následující tabulka popisuje atributy, které mohou být aplikovány na element **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="d5ee8-636">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-636">Attribute Name</span></span>   | <span data-ttu-id="d5ee8-637">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-637">Is Required</span></span> | <span data-ttu-id="d5ee8-638">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-639">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-639">**Name**</span></span>         | <span data-ttu-id="d5ee8-640">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-640">Yes</span></span>         | <span data-ttu-id="d5ee8-641">Název navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="d5ee8-642">**Vztah**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-642">**Relationship**</span></span> | <span data-ttu-id="d5ee8-643">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-643">Yes</span></span>         | <span data-ttu-id="d5ee8-644">Název asociace, který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="d5ee8-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-645">**ToRole**</span></span>       | <span data-ttu-id="d5ee8-646">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-646">Yes</span></span>         | <span data-ttu-id="d5ee8-647">Konec přidružení, na kterém končí navigace.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="d5ee8-648">Hodnota atributu **ToRole** musí být stejná jako hodnota jednoho z atributů **role** definované v jednom z elementů Association (definovaných v elementu AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="d5ee8-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-649">**FromRole**</span></span>     | <span data-ttu-id="d5ee8-650">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-650">Yes</span></span>         | <span data-ttu-id="d5ee8-651">Konec přidružení, ze kterého začíná navigace.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="d5ee8-652">Hodnota atributu **FromRole** musí být stejná jako hodnota jednoho z atributů **role** definované v jednom z elementů Association (definovaných v elementu AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-653">Pro element **NavigationProperty** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="d5ee8-654">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-655">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-656">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-656">Example</span></span>

<span data-ttu-id="d5ee8-657">Následující příklad definuje typ entity (**Book**) se dvěma navigačními vlastnostmi (**PublishedBy** a **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="d5ee8-658">IsDeleted – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-659">Element **IsDeleted** v koncepčním definičním jazyce (CSDL) definuje chování, které je připojeno k přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="d5ee8-660">Je-li atribut **Action** nastaven na hodnotu **Cascade** na jednom konci přidružení, budou související typy entit na druhém konci přidružení odstraněny při odstranění typu entity v prvním elementu end.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="d5ee8-661">Pokud je přidružení mezi dvěma typy entit primárním vztahem klíče k primárnímu klíči, pak se načtený závislý objekt odstraní při odstranění objektu zabezpečení na druhém konci přidružení bez ohledu na specifikaci při **odstranění** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="d5ee8-662">Element **IsDeleted** má vliv pouze na chování aplikace za běhu. nemá vliv na chování ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="d5ee8-663">Chování definované ve zdroji dat by mělo být stejné jako chování definované v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="d5ee8-664">Element **IsDeleted** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-665">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-666">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-667">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-667">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-668">Následující tabulka popisuje atributy, které mohou být aplikovány na element **IsDeleted** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="d5ee8-669">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-669">Attribute Name</span></span> | <span data-ttu-id="d5ee8-670">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-670">Is Required</span></span> | <span data-ttu-id="d5ee8-671">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-672">**Akce**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-672">**Action**</span></span>     | <span data-ttu-id="d5ee8-673">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-673">Yes</span></span>         | <span data-ttu-id="d5ee8-674">**Cascade** nebo **none**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-674">**Cascade** or **None**.</span></span> <span data-ttu-id="d5ee8-675">V případě, že se **kaskády**odstraní, odstraní se závislé typy entit při odstranění typu objektu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="d5ee8-676">Pokud **žádný**není, typy závislých entit nebudou odstraněny při odstranění typu hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-677">Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="d5ee8-678">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-679">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-680">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-680">Example</span></span>

<span data-ttu-id="d5ee8-681">Následující příklad ukazuje element **Association** , který definuje přidružení **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="d5ee8-682">Element **IsDeleted** označuje, že všechny **objednávky** , které se vztahují ke konkrétnímu **zákazníkovi** a které byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="d5ee8-683">Parameter – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-684">Element **Parameter** v koncepčním definičním jazyce (CSDL) může být podřízeným elementem FunctionImport nebo prvkem funkce.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="d5ee8-685">Element FunctionImport – aplikace</span><span class="sxs-lookup"><span data-stu-id="d5ee8-685">FunctionImport Element Application</span></span>

<span data-ttu-id="d5ee8-686">Element **parametru** (jako podřízený prvek elementu **FunctionImport** ) slouží k definování vstupních a výstupních parametrů pro importy funkce, které jsou deklarovány v CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="d5ee8-687">Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-688">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-689">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-690">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-690">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-691">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="d5ee8-692">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-692">Attribute Name</span></span> | <span data-ttu-id="d5ee8-693">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-693">Is Required</span></span> | <span data-ttu-id="d5ee8-694">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-695">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-695">**Name**</span></span>       | <span data-ttu-id="d5ee8-696">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-696">Yes</span></span>         | <span data-ttu-id="d5ee8-697">Název parametru</span><span class="sxs-lookup"><span data-stu-id="d5ee8-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="d5ee8-698">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-698">**Type**</span></span>       | <span data-ttu-id="d5ee8-699">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-699">Yes</span></span>         | <span data-ttu-id="d5ee8-700">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-700">The parameter type.</span></span> <span data-ttu-id="d5ee8-701">Hodnota musí být **EDMSimpleType** nebo komplexní typ, který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="d5ee8-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-702">**Mode**</span></span>       | <span data-ttu-id="d5ee8-703">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-703">No</span></span>          | <span data-ttu-id="d5ee8-704">**In**, **out**nebo **InOut** v závislosti na tom, zda je parametr vstupní, výstupní nebo vstupní/výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="d5ee8-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-705">**MaxLength**</span></span>  | <span data-ttu-id="d5ee8-706">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-706">No</span></span>          | <span data-ttu-id="d5ee8-707">Maximální povolená délka parametru.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="d5ee8-708">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-708">**Precision**</span></span>  | <span data-ttu-id="d5ee8-709">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-709">No</span></span>          | <span data-ttu-id="d5ee8-710">Přesnost parametru.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="d5ee8-711">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-711">**Scale**</span></span>      | <span data-ttu-id="d5ee8-712">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-712">No</span></span>          | <span data-ttu-id="d5ee8-713">Měřítko parametru.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="d5ee8-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-714">**SRID**</span></span>       | <span data-ttu-id="d5ee8-715">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-715">No</span></span>          | <span data-ttu-id="d5ee8-716">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="d5ee8-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d5ee8-717">Platné pouze pro parametry prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="d5ee8-718">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-719">Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="d5ee8-720">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-721">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d5ee8-722">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-722">Example</span></span>

<span data-ttu-id="d5ee8-723">Následující příklad ukazuje element **FunctionImport** s jedním podřízeným elementem **parametru** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="d5ee8-724">Funkce přijímá jeden vstupní parametr a vrací kolekci typů entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="d5ee8-725">Aplikace prvku funkce</span><span class="sxs-lookup"><span data-stu-id="d5ee8-725">Function Element Application</span></span>

<span data-ttu-id="d5ee8-726">Element **Parameter** (jako podřízený prvek **funkce** ) definuje parametry pro funkce, které jsou definovány nebo deklarovány v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="d5ee8-727">Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-728">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="d5ee8-729">CollectionType (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="d5ee8-730">Hodnota ReferenceType (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="d5ee8-731">RowType (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-732">Pouze jeden z prvků typu **CollectionType**, **hodnota ReferenceType**nebo **RowType** může být podřízeným prvkem elementu **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="d5ee8-733">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-734">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="d5ee8-735">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-736">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-736">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-737">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="d5ee8-738">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-738">Attribute Name</span></span>   | <span data-ttu-id="d5ee8-739">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-739">Is Required</span></span> | <span data-ttu-id="d5ee8-740">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-741">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-741">**Name**</span></span>         | <span data-ttu-id="d5ee8-742">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-742">Yes</span></span>         | <span data-ttu-id="d5ee8-743">Název parametru</span><span class="sxs-lookup"><span data-stu-id="d5ee8-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="d5ee8-744">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-744">**Type**</span></span>         | <span data-ttu-id="d5ee8-745">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-745">No</span></span>          | <span data-ttu-id="d5ee8-746">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-746">The parameter type.</span></span> <span data-ttu-id="d5ee8-747">Parametr může být libovolný z následujících typů (nebo kolekcí těchto typů):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="d5ee8-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="d5ee8-749">entity type</span><span class="sxs-lookup"><span data-stu-id="d5ee8-749">entity type</span></span> <br/> <span data-ttu-id="d5ee8-750">complex type</span><span class="sxs-lookup"><span data-stu-id="d5ee8-750">complex type</span></span> <br/> <span data-ttu-id="d5ee8-751">typ řádku</span><span class="sxs-lookup"><span data-stu-id="d5ee8-751">row type</span></span> <br/> <span data-ttu-id="d5ee8-752">odkazový typ</span><span class="sxs-lookup"><span data-stu-id="d5ee8-752">reference type</span></span>                             |
| <span data-ttu-id="d5ee8-753">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-753">**Nullable**</span></span>     | <span data-ttu-id="d5ee8-754">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-754">No</span></span>          | <span data-ttu-id="d5ee8-755">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu **null** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="d5ee8-756">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-756">**DefaultValue**</span></span> | <span data-ttu-id="d5ee8-757">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-757">No</span></span>          | <span data-ttu-id="d5ee8-758">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d5ee8-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-759">**MaxLength**</span></span>    | <span data-ttu-id="d5ee8-760">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-760">No</span></span>          | <span data-ttu-id="d5ee8-761">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d5ee8-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-762">**FixedLength**</span></span>  | <span data-ttu-id="d5ee8-763">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-763">No</span></span>          | <span data-ttu-id="d5ee8-764">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d5ee8-765">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-765">**Precision**</span></span>    | <span data-ttu-id="d5ee8-766">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-766">No</span></span>          | <span data-ttu-id="d5ee8-767">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d5ee8-768">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-768">**Scale**</span></span>        | <span data-ttu-id="d5ee8-769">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-769">No</span></span>          | <span data-ttu-id="d5ee8-770">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d5ee8-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-771">**SRID**</span></span>         | <span data-ttu-id="d5ee8-772">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-772">No</span></span>          | <span data-ttu-id="d5ee8-773">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="d5ee8-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d5ee8-774">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d5ee8-775">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d5ee8-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-776">**Unicode**</span></span>      | <span data-ttu-id="d5ee8-777">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-777">No</span></span>          | <span data-ttu-id="d5ee8-778">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d5ee8-779">**Velké**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-779">**Collation**</span></span>    | <span data-ttu-id="d5ee8-780">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-780">No</span></span>          | <span data-ttu-id="d5ee8-781">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-782">Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="d5ee8-783">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-784">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d5ee8-785">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-785">Example</span></span>

<span data-ttu-id="d5ee8-786">Následující příklad ukazuje prvek **funkce** , který používá jeden podřízený element **Parameter** k definování parametru funkce.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="d5ee8-787">Element zabezpečení (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-788">Element **Principal** v jazyce CSDL (konceptuální schéma Definition Language) je podřízeným prvkem elementu elementu ReferentialConstraint, který definuje hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="d5ee8-789">Element **elementu ReferentialConstraint** definuje funkčnost, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="d5ee8-790">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="d5ee8-791">Odkazovaný typ entity se nazývá *hlavní konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="d5ee8-792">Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="d5ee8-793">Prvky **PropertyRef** slouží k určení, na které klíče se odkazuje závislý konec.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="d5ee8-794">Element **Principal** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-795">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="d5ee8-796">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-797">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-797">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-798">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Principal** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="d5ee8-799">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-799">Attribute Name</span></span> | <span data-ttu-id="d5ee8-800">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-800">Is Required</span></span> | <span data-ttu-id="d5ee8-801">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-802">**Role**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-802">**Role**</span></span>       | <span data-ttu-id="d5ee8-803">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-803">Yes</span></span>         | <span data-ttu-id="d5ee8-804">Název typu entity na hlavním konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-805">U elementu **Principal** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="d5ee8-806">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-807">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-808">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-808">Example</span></span>

<span data-ttu-id="d5ee8-809">Následující příklad ukazuje element **elementu ReferentialConstraint** , který je součástí definice přidružení **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="d5ee8-810">Vlastnost **ID** typu entity **vydavatele** vytvoří hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="d5ee8-811">Property – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-811">Property Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-812">Element **Property** v jazyce CSDL (koncepční schéma Definition Language) může být podřízeným elementem EntityType, elementem complexType nebo elementu RowType.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="d5ee8-813">EntityType a ComplexType – aplikace elementu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="d5ee8-814">Prvky **vlastností** (jako podřízené prvky **EntityType** nebo **complexType** ) definují tvar a vlastnosti dat, které instance typu entity nebo instance komplexního typu budou obsahovat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="d5ee8-815">Vlastnosti v koncepčním modelu jsou analogické k vlastnostem, které jsou definovány ve třídě.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="d5ee8-816">Stejně jako vlastnosti třídy definují tvar třídy a přenášejí informace o objektech, vlastnosti v koncepčním modelu definují tvar typu entity a přenášejí informace o instancích typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="d5ee8-817">Element **Property** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-818">Element dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-819">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="d5ee8-820">Následující omezující vlastnosti lze použít na prvek **vlastnosti** : **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **COLLATE**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="d5ee8-821">Omezující vlastnosti jsou atributy XML, které poskytují informace o tom, jak jsou hodnoty vlastností uloženy v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-822">Omezující vlastnosti lze použít pouze pro vlastnosti typu **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-823">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-823">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-824">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="d5ee8-825">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-825">Attribute Name</span></span>                                                         | <span data-ttu-id="d5ee8-826">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-826">Is Required</span></span> | <span data-ttu-id="d5ee8-827">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-828">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-828">**Name**</span></span>                                                               | <span data-ttu-id="d5ee8-829">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-829">Yes</span></span>         | <span data-ttu-id="d5ee8-830">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="d5ee8-831">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-831">**Type**</span></span>                                                               | <span data-ttu-id="d5ee8-832">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-832">Yes</span></span>         | <span data-ttu-id="d5ee8-833">Typ hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-833">The type of the property value.</span></span> <span data-ttu-id="d5ee8-834">Typ hodnoty vlastnosti musí být **EDMSimpleType** nebo komplexní typ (určený plně kvalifikovaným názvem), který je v rozsahu modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="d5ee8-835">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-835">**Nullable**</span></span>                                                           | <span data-ttu-id="d5ee8-836">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-836">No</span></span>          | <span data-ttu-id="d5ee8-837">**True** (výchozí hodnota) nebo <strong>false</strong> v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="d5ee8-838">> V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="d5ee8-839">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="d5ee8-840">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-840">No</span></span>          | <span data-ttu-id="d5ee8-841">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d5ee8-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="d5ee8-843">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-843">No</span></span>          | <span data-ttu-id="d5ee8-844">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d5ee8-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="d5ee8-846">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-846">No</span></span>          | <span data-ttu-id="d5ee8-847">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d5ee8-848">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-848">**Precision**</span></span>                                                          | <span data-ttu-id="d5ee8-849">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-849">No</span></span>          | <span data-ttu-id="d5ee8-850">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d5ee8-851">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-851">**Scale**</span></span>                                                              | <span data-ttu-id="d5ee8-852">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-852">No</span></span>          | <span data-ttu-id="d5ee8-853">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d5ee8-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-854">**SRID**</span></span>                                                               | <span data-ttu-id="d5ee8-855">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-855">No</span></span>          | <span data-ttu-id="d5ee8-856">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="d5ee8-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d5ee8-857">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d5ee8-858">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d5ee8-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-859">**Unicode**</span></span>                                                            | <span data-ttu-id="d5ee8-860">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-860">No</span></span>          | <span data-ttu-id="d5ee8-861">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d5ee8-862">**Velké**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-862">**Collation**</span></span>                                                          | <span data-ttu-id="d5ee8-863">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-863">No</span></span>          | <span data-ttu-id="d5ee8-864">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="d5ee8-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="d5ee8-866">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-866">No</span></span>          | <span data-ttu-id="d5ee8-867">**Žádná** (výchozí hodnota) nebo **pevná**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="d5ee8-868">Pokud je hodnota nastavena na **pevná**, bude hodnota vlastnosti použita při kontrole optimistického řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-869">Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="d5ee8-870">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-871">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d5ee8-872">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-872">Example</span></span>

<span data-ttu-id="d5ee8-873">Následující příklad ukazuje element **EntityType** se třemi prvky **vlastností** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="d5ee8-874">Následující příklad ukazuje element **complexType** s pěti prvky **vlastností** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="d5ee8-875">Aplikace prvku RowType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-875">RowType Element Application</span></span>

<span data-ttu-id="d5ee8-876">Prvky **vlastností** (jako podřízené objekty elementu **RowType** ) definují tvar a charakteristiky dat, která lze předat nebo vrátit z modelu definované funkce.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="d5ee8-877">Element **Property** může mít přesně jeden z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="d5ee8-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-878">CollectionType</span></span>
-   <span data-ttu-id="d5ee8-879">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-879">ReferenceType</span></span>
-   <span data-ttu-id="d5ee8-880">RowType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-880">RowType</span></span>

<span data-ttu-id="d5ee8-881">Element **Property** může mít libovolný počet podřízených elementů poznámky.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-882">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-883">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-883">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-884">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="d5ee8-885">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-885">Attribute Name</span></span>                                                     | <span data-ttu-id="d5ee8-886">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-886">Is Required</span></span> | <span data-ttu-id="d5ee8-887">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-888">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-888">**Name**</span></span>                                                           | <span data-ttu-id="d5ee8-889">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-889">Yes</span></span>         | <span data-ttu-id="d5ee8-890">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="d5ee8-891">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-891">**Type**</span></span>                                                           | <span data-ttu-id="d5ee8-892">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-892">Yes</span></span>         | <span data-ttu-id="d5ee8-893">Typ hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="d5ee8-894">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-894">**Nullable**</span></span>                                                       | <span data-ttu-id="d5ee8-895">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-895">No</span></span>          | <span data-ttu-id="d5ee8-896">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="d5ee8-897">> V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="d5ee8-898">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="d5ee8-899">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-899">No</span></span>          | <span data-ttu-id="d5ee8-900">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d5ee8-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="d5ee8-902">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-902">No</span></span>          | <span data-ttu-id="d5ee8-903">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d5ee8-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="d5ee8-905">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-905">No</span></span>          | <span data-ttu-id="d5ee8-906">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d5ee8-907">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-907">**Precision**</span></span>                                                      | <span data-ttu-id="d5ee8-908">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-908">No</span></span>          | <span data-ttu-id="d5ee8-909">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d5ee8-910">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-910">**Scale**</span></span>                                                          | <span data-ttu-id="d5ee8-911">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-911">No</span></span>          | <span data-ttu-id="d5ee8-912">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d5ee8-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-913">**SRID**</span></span>                                                           | <span data-ttu-id="d5ee8-914">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-914">No</span></span>          | <span data-ttu-id="d5ee8-915">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="d5ee8-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d5ee8-916">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d5ee8-917">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d5ee8-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-918">**Unicode**</span></span>                                                        | <span data-ttu-id="d5ee8-919">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-919">No</span></span>          | <span data-ttu-id="d5ee8-920">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d5ee8-921">**Velké**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-921">**Collation**</span></span>                                                      | <span data-ttu-id="d5ee8-922">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-922">No</span></span>          | <span data-ttu-id="d5ee8-923">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-924">Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="d5ee8-925">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-926">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="d5ee8-927">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-927">Example</span></span>

<span data-ttu-id="d5ee8-928">Následující příklad ukazuje prvky **vlastností** používané k definování tvaru návratového typu funkce definované modelem.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="d5ee8-929">PropertyRef – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-930">Element **PropertyRef** v jazyce CSDL (konceptuální schéma Definition Language) odkazuje na vlastnost typu entity, která označuje, že vlastnost provede jednu z následujících rolí:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="d5ee8-931">Část klíče entity (vlastnost nebo sada vlastností typu entity, která určuje identitu).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="d5ee8-932">Jeden nebo více elementů **PropertyRef** lze použít k definování klíče entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="d5ee8-933">Závislý nebo hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="d5ee8-934">Element **PropertyRef** může mít jako podřízené elementy pouze prvky poznámky (nula nebo více).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-935">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-936">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-936">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-937">Následující tabulka popisuje atributy, které mohou být aplikovány na element **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="d5ee8-938">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-938">Attribute Name</span></span> | <span data-ttu-id="d5ee8-939">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-939">Is Required</span></span> | <span data-ttu-id="d5ee8-940">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="d5ee8-941">**Název**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-941">**Name**</span></span>       | <span data-ttu-id="d5ee8-942">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-942">Yes</span></span>         | <span data-ttu-id="d5ee8-943">Název odkazované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-944">Pro element **PropertyRef** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="d5ee8-945">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-946">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-947">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-947">Example</span></span>

<span data-ttu-id="d5ee8-948">Následující příklad definuje typ entity (**Book**).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="d5ee8-949">Klíč entity je definován pomocí odkazu na vlastnost **ISBN** typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="d5ee8-950">V následujícím příkladu jsou použity dva prvky **PropertyRef** k označení toho, že dvě vlastnosti (**ID** a **PublisherId**) jsou objekty zabezpečení a závislé na konci referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="d5ee8-951">Hodnota ReferenceType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-952">Element **hodnota ReferenceType** v jazyce CSDL (konceptuální schéma Definition Language) určuje odkaz na typ entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="d5ee8-953">Element **hodnota ReferenceType** může být podřízeným prvkem následujících prvků:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="d5ee8-954">ReturnType (Function)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="d5ee8-955">Parametr</span><span class="sxs-lookup"><span data-stu-id="d5ee8-955">Parameter</span></span>
-   <span data-ttu-id="d5ee8-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-956">CollectionType</span></span>

<span data-ttu-id="d5ee8-957">Element **hodnota ReferenceType** se používá při definování parametru nebo návratového typu pro funkci.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="d5ee8-958">Element **hodnota ReferenceType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-959">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-960">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-961">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-961">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-962">Následující tabulka popisuje atributy, které mohou být aplikovány na element **hodnota ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="d5ee8-963">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-963">Attribute Name</span></span> | <span data-ttu-id="d5ee8-964">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-964">Is Required</span></span> | <span data-ttu-id="d5ee8-965">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="d5ee8-966">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-966">**Type**</span></span>       | <span data-ttu-id="d5ee8-967">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-967">Yes</span></span>         | <span data-ttu-id="d5ee8-968">Název odkazovaného typu entity</span><span class="sxs-lookup"><span data-stu-id="d5ee8-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-969">Pro element **hodnota ReferenceType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="d5ee8-970">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-971">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-972">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-972">Example</span></span>

<span data-ttu-id="d5ee8-973">Následující příklad ukazuje element **hodnota ReferenceType** , který se používá jako podřízený prvek **parametru** v rámci funkce definované modelem, která přijímá odkaz na typ entity **Person** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="d5ee8-974">Následující příklad ukazuje element **hodnota ReferenceType** , který se používá jako podřízený prvek **ReturnType** (Function) ve funkci definovaného modelu, která vrací odkaz na typ entity **Person** :</span><span class="sxs-lookup"><span data-stu-id="d5ee8-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="d5ee8-975">Elementu ReferentialConstraint – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-976">Element **elementu ReferentialConstraint** v jazyce CSDL (konceptuální schéma Definition Language) definuje funkce, které jsou podobné omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="d5ee8-977">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="d5ee8-978">Odkazovaný typ entity se nazývá *hlavní konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="d5ee8-979">Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="d5ee8-980">Pokud cizí klíč, který je vystaven na jednom typu entity, odkazuje na vlastnost v jiném typu entity, element **elementu ReferentialConstraint** definuje přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="d5ee8-981">Vzhledem k tomu, že element **elementu ReferentialConstraint** poskytuje informace o tom, jak se vztahují dva typy entit, není v jazyce MSL (Mapping Specification Language) nutný žádný odpovídající element AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="d5ee8-982">Přidružení mezi dvěma typy entit, které nemají vystavené cizími klíči, musí mít odpovídající element **AssociationSetMapping** , aby bylo možné mapovat informace o přidružení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="d5ee8-983">Pokud cizí klíč není vystavený pro typ entity, element **elementu ReferentialConstraint** může definovat jenom primární omezení klíče na primární klíč mezi typem entity a jiným typem entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="d5ee8-984">Element **elementu ReferentialConstraint** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-985">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-986">Objekt zabezpečení (právě jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="d5ee8-987">Závislý (právě jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="d5ee8-988">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-989">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-989">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-990">Element **elementu ReferentialConstraint** může mít libovolný počet atributů anotace (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="d5ee8-991">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-992">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-993">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-993">Example</span></span>

<span data-ttu-id="d5ee8-994">Následující příklad ukazuje element **elementu ReferentialConstraint** , který se používá jako součást definice asociace **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="d5ee8-995">ReturnType – element (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-996">Element **ReturnType** (Function) v jazyce CSDL (konceptuální schéma Definition Language) určuje návratový typ pro funkci, která je definována v elementu Function.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="d5ee8-997">Návratový typ funkce lze také zadat s atributem **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="d5ee8-998">Návratové typy mohou být libovolné **EdmSimpleType**, typ entity, komplexní typ, typ řádku, typ odkazu nebo kolekce jednoho z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="d5ee8-999">Návratový typ funkce lze zadat buď pomocí atributu **Type** elementu **ReturnType** (Function), nebo s jedním z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="d5ee8-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1000">CollectionType</span></span>
-   <span data-ttu-id="d5ee8-1001">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1001">ReferenceType</span></span>
-   <span data-ttu-id="d5ee8-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-1003">Model nebude ověřen, pokud zadáte návratový typ funkce s atributem **Type** elementu **ReturnType** (Function) a jedním z podřízených elementů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-1004">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1004">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-1005">Následující tabulka popisuje atributy, které mohou být aplikovány na element **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="d5ee8-1006">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1006">Attribute Name</span></span> | <span data-ttu-id="d5ee8-1007">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1007">Is Required</span></span> | <span data-ttu-id="d5ee8-1008">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="d5ee8-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1009">**ReturnType**</span></span> | <span data-ttu-id="d5ee8-1010">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1010">No</span></span>          | <span data-ttu-id="d5ee8-1011">Typ vrácený funkcí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-1012">Pro element **ReturnType** (Function) lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="d5ee8-1013">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1014">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-1015">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1015">Example</span></span>

<span data-ttu-id="d5ee8-1016">Následující příklad používá element **Function** k definování funkce, která vrátí počet roků, po který se kniha tiskne.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="d5ee8-1017">Všimněte si, že návratový typ je určen atributem **typu** elementu **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="d5ee8-1018">ReturnType (FunctionImport) – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-1019">Element **ReturnType** (FunctionImport) v jazyce CSDL (konceptuální schéma Definition Language) určuje návratový typ pro funkci, která je definována v elementu FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="d5ee8-1020">Návratový typ funkce lze také zadat s atributem **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="d5ee8-1021">Návratové typy mohou být libovolné kolekce typu entity, komplexního typu nebo **EdmSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="d5ee8-1022">Návratový typ funkce je zadán s atributem **Type** elementu **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-1023">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1023">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-1024">Následující tabulka popisuje atributy, které lze použít na element **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="d5ee8-1025">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1025">Attribute Name</span></span> | <span data-ttu-id="d5ee8-1026">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1026">Is Required</span></span> | <span data-ttu-id="d5ee8-1027">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-1028">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1028">**Type**</span></span>       | <span data-ttu-id="d5ee8-1029">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1029">No</span></span>          | <span data-ttu-id="d5ee8-1030">Typ, který funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1030">The type that the function returns.</span></span> <span data-ttu-id="d5ee8-1031">Hodnotou musí být kolekce ComplexType, EntityType nebo EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="d5ee8-1032">**Sada**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1032">**EntitySet**</span></span>  | <span data-ttu-id="d5ee8-1033">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1033">No</span></span>          | <span data-ttu-id="d5ee8-1034">Pokud funkce vrátí kolekci typů entit, hodnota **objektu EntitySet** musí být sada entit, do které kolekce patří.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="d5ee8-1035">V opačném případě nesmí být použit atribut **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-1036">Pro element **ReturnType** (FunctionImport) lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="d5ee8-1037">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1038">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-1039">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1039">Example</span></span>

<span data-ttu-id="d5ee8-1040">V následujícím příkladu je použita funkce **FunctionImport** , která vrací knihy a vydavatele.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="d5ee8-1041">Všimněte si, že funkce vrací dvě sady výsledků, a proto jsou zadány dva prvky **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="d5ee8-1042">RowType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-1043">Element **RowType** v koncepčním definičním jazyce (CSDL) definuje nepojmenované struktury jako parametr nebo návratový typ pro funkci definovanou v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="d5ee8-1044">Element **RowType** může být podřízeným prvkem následujících prvků:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="d5ee8-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1045">CollectionType</span></span>
-   <span data-ttu-id="d5ee8-1046">Parametr</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1046">Parameter</span></span>
-   <span data-ttu-id="d5ee8-1047">ReturnType (Function)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1047">ReturnType (Function)</span></span>

<span data-ttu-id="d5ee8-1048">Element **RowType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-1049">Vlastnost (jedna nebo více)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1049">Property (one or more)</span></span>
-   <span data-ttu-id="d5ee8-1050">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-1051">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1051">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-1052">Pro element **RowType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="d5ee8-1053">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1054">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-1055">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1055">Example</span></span>

<span data-ttu-id="d5ee8-1056">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="d5ee8-1057">Schema – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-1058">Element **Schema** je kořenovým prvkem definice koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="d5ee8-1059">Obsahuje definice pro objekty, funkce a kontejnery, které tvoří koncepční model.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="d5ee8-1060">Element **Schema** může obsahovat nula nebo více z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="d5ee8-1061">Použití</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1061">Using</span></span>
-   <span data-ttu-id="d5ee8-1062">Kontejneru</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1062">EntityContainer</span></span>
-   <span data-ttu-id="d5ee8-1063">Typ entity</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1063">EntityType</span></span>
-   <span data-ttu-id="d5ee8-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1064">EnumType</span></span>
-   <span data-ttu-id="d5ee8-1065">Řídí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1065">Association</span></span>
-   <span data-ttu-id="d5ee8-1066">Elementu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1066">ComplexType</span></span>
-   <span data-ttu-id="d5ee8-1067">Funkce</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1067">Function</span></span>

<span data-ttu-id="d5ee8-1068">Prvek **schématu** může obsahovat nula nebo jeden prvek poznámky.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-1069">Element **funkce** a prvky poznámky jsou povoleny pouze v CSDL v2 a novějším.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="d5ee8-1070">Element **Schema** používá atribut **Namespace** k definování oboru názvů pro typ entity, komplexní typ a objekty přidružení v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="d5ee8-1071">V rámci oboru názvů nemohou mít žádné dva objekty stejný název.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="d5ee8-1072">Obory názvů mohou zahrnovat více prvků **schématu** a více souborů. csdl.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="d5ee8-1073">Obor názvů koncepčního modelu je jiný než obor názvů XML elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="d5ee8-1074">Obor názvů koncepčního modelu (jak je definováno atributem **Namespace** ) je logický kontejner pro typy entit, komplexní typy a typy přidružení.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="d5ee8-1075">Obor názvů XML (uvedený atributem **xmlns** ) elementu **Schema** je výchozí obor názvů pro podřízené elementy a atributy elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="d5ee8-1076">Obory názvů XML ve formátu https://schemas.microsoft.com/ado/YYYY/MM/edm (kde RRRR a MM představují rok a měsíc) jsou vyhrazené pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1077">Vlastní elementy a atributy nemůžou být v oborech názvů, které mají tento formulář.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-1078">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1078">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-1079">Následující tabulka popisuje atributy, které lze použít pro element **Schema** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="d5ee8-1080">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1080">Attribute Name</span></span> | <span data-ttu-id="d5ee8-1081">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1081">Is Required</span></span> | <span data-ttu-id="d5ee8-1082">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1083">**Namespace**</span></span>  | <span data-ttu-id="d5ee8-1084">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1084">Yes</span></span>         | <span data-ttu-id="d5ee8-1085">Obor názvů koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="d5ee8-1086">Hodnota atributu **Namespace** slouží k vytvoření plně kvalifikovaného názvu typu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="d5ee8-1087">Pokud je například **EntityType** s názvem *Zákazník* v jednoduchém oboru názvů. example. model, pak plně kvalifikovaný název **objektu EntityType** je SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="d5ee8-1088">Následující řetězce nelze použít jako hodnotu pro atribut **Namespace** : **System**, **přechodný**nebo **EDM**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="d5ee8-1089">Hodnota atributu **Namespace** nemůže být stejná jako hodnota atributu **Namespace** v elementu schématu SSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="d5ee8-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1090">**Alias**</span></span>      | <span data-ttu-id="d5ee8-1091">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1091">No</span></span>          | <span data-ttu-id="d5ee8-1092">Identifikátor použitý místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="d5ee8-1093">Pokud je například **EntityType** s názvem *Zákazník* v jednoduchém oboru názvů. example. model a hodnota atributu **alias** je *model*, pak můžete použít model. Customer jako plně kvalifikovaný název **objektu EntityType.**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-1094">Pro element **Schema** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="d5ee8-1095">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1096">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-1097">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1097">Example</span></span>

<span data-ttu-id="d5ee8-1098">Následující příklad ukazuje element **schématu** , který obsahuje element **EntityContainer** , dva elementy **EntityType** a jeden element **Association** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="d5ee8-1099">TypeRef – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-1100">Element **TypeRef** v jazyce CSDL (konceptuální schéma Definition Language) poskytuje odkaz na existující pojmenovaný typ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="d5ee8-1101">Element **TypeRef** může být podřízeným elementem CollectionType, který se používá k určení toho, že funkce má kolekci jako parametr nebo návratový typ.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="d5ee8-1102">Element **TypeRef** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="d5ee8-1103">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="d5ee8-1104">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-1105">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1105">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-1106">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="d5ee8-1107">Všimněte si, že atributy **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**a **kolace** se vztahují pouze na **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="d5ee8-1108">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="d5ee8-1109">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1109">Is Required</span></span> | <span data-ttu-id="d5ee8-1110">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-1111">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1111">**Type**</span></span>                                                           | <span data-ttu-id="d5ee8-1112">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1112">No</span></span>          | <span data-ttu-id="d5ee8-1113">Název odkazovaného typu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="d5ee8-1114">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="d5ee8-1115">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1115">No</span></span>          | <span data-ttu-id="d5ee8-1116">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="d5ee8-1117">> V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="d5ee8-1118">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="d5ee8-1119">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1119">No</span></span>          | <span data-ttu-id="d5ee8-1120">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="d5ee8-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="d5ee8-1122">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1122">No</span></span>          | <span data-ttu-id="d5ee8-1123">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="d5ee8-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="d5ee8-1125">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1125">No</span></span>          | <span data-ttu-id="d5ee8-1126">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="d5ee8-1127">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1127">**Precision**</span></span>                                                      | <span data-ttu-id="d5ee8-1128">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1128">No</span></span>          | <span data-ttu-id="d5ee8-1129">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="d5ee8-1130">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1130">**Scale**</span></span>                                                          | <span data-ttu-id="d5ee8-1131">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1131">No</span></span>          | <span data-ttu-id="d5ee8-1132">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="d5ee8-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1133">**SRID**</span></span>                                                           | <span data-ttu-id="d5ee8-1134">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1134">No</span></span>          | <span data-ttu-id="d5ee8-1135">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="d5ee8-1136">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="d5ee8-1137">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="d5ee8-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="d5ee8-1139">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1139">No</span></span>          | <span data-ttu-id="d5ee8-1140">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="d5ee8-1141">**Velké**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1141">**Collation**</span></span>                                                      | <span data-ttu-id="d5ee8-1142">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1142">No</span></span>          | <span data-ttu-id="d5ee8-1143">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-1144">Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="d5ee8-1145">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1146">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-1147">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1147">Example</span></span>

<span data-ttu-id="d5ee8-1148">Následující příklad ukazuje funkci definovanou modelem, která používá prvek **TypeRef** (jako podřízený prvek typu **CollectionType** ) k určení, že funkce přijímá kolekci typů entit **oddělení** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="d5ee8-1149">Using – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="d5ee8-1150">Element **using** v jazyce CSDL (konceptuální schéma Definition Language) importuje obsah koncepčního modelu, který existuje v jiném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="d5ee8-1151">Nastavením hodnoty atributu **Namespace** můžete odkazovat na typy entit, komplexní typy a typy přidružení, které jsou definovány v jiném koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="d5ee8-1152">Více než jeden element **using** může být podřízeným elementu **schématu** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-1153">Element **using** v CSDL nefunguje úplně stejně jako příkaz **using** v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="d5ee8-1154">Importováním oboru názvů pomocí příkazu **using** v programovacím jazyce neovlivníte objekty v původním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="d5ee8-1155">V CSDL může importovaný obor názvů obsahovat typ entity, který je odvozen z typu entity v původním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="d5ee8-1156">To může mít vliv na sady entit deklarované v původním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="d5ee8-1157">Element **using** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="d5ee8-1158">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="d5ee8-1159">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="d5ee8-1160">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1160">Applicable Attributes</span></span>

<span data-ttu-id="d5ee8-1161">Následující tabulka popisuje atributy, které lze použít pro element **using** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="d5ee8-1162">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1162">Attribute Name</span></span> | <span data-ttu-id="d5ee8-1163">Je povinné</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1163">Is Required</span></span> | <span data-ttu-id="d5ee8-1164">Value</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1165">**Namespace**</span></span>  | <span data-ttu-id="d5ee8-1166">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1166">Yes</span></span>         | <span data-ttu-id="d5ee8-1167">Název importovaného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="d5ee8-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1168">**Alias**</span></span>      | <span data-ttu-id="d5ee8-1169">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1169">Yes</span></span>         | <span data-ttu-id="d5ee8-1170">Identifikátor použitý místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="d5ee8-1171">I když je tento atribut vyžadován, není vyžadováno, aby byl použit místo názvu oboru názvů k získání názvů objektů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="d5ee8-1172">Pro element **using** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="d5ee8-1173">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="d5ee8-1174">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="d5ee8-1175">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1175">Example</span></span>

<span data-ttu-id="d5ee8-1176">Následující příklad ukazuje **použití** prvku, který je použit pro import oboru názvů, který je definován jinde.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="d5ee8-1177">Všimněte si, že obor názvů pro zobrazený element **schématu** je `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="d5ee8-1178">Vlastnost `Address` na**EntityType** `Publisher` je komplexní typ, který je definován v oboru názvů `ExtendedBooksModel` (importované pomocí elementu **using** ).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="d5ee8-1179">Atributy poznámek (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="d5ee8-1180">Atributy poznámek v nástroji CSDL (konceptuální schéma Definition Language) jsou vlastní atributy XML v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="d5ee8-1181">Kromě toho, že mají platnou strukturu XML, musí mít následující hodnoty true atributů Anotace:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="d5ee8-1182">Atributy poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="d5ee8-1183">U daného elementu CSDL lze použít více než jeden atribut poznámky.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="d5ee8-1184">Plně kvalifikované názvy všech dvou atributů poznámek nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="d5ee8-1185">Atributy poznámek lze použít k poskytnutí dalších metadat o prvcích v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="d5ee8-1186">K metadatům obsaženým v prvcích poznámky lze za běhu přistup použít třídy v oboru názvů System. data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-1187">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1187">Example</span></span>

<span data-ttu-id="d5ee8-1188">Následující příklad ukazuje element **EntityType** s atributem poznámky (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="d5ee8-1189">Příklad také ukazuje prvek poznámky aplikovaný na prvek typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="d5ee8-1190">Následující kód načte metadata v atributu anotace a zapíše je do konzoly:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="d5ee8-1191">Výše uvedený kód předpokládá, že soubor `School.csdl` je ve výstupním adresáři projektu a že jste do projektu přidali následující příkazy `Imports` a `Using`:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="d5ee8-1192">Prvky poznámek (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="d5ee8-1193">Prvky poznámek v jazyce CSDL (konceptuální schéma Definition Language) jsou vlastní prvky XML v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="d5ee8-1194">Kromě toho, že mají platnou strukturu XML, musí mít následující hodnoty true pro prvky poznámky:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="d5ee8-1195">Prvky poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="d5ee8-1196">Více než jeden element poznámky může být podřízeným prvku daného elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="d5ee8-1197">Plně kvalifikované názvy všech dvou prvků poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="d5ee8-1198">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích daného elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="d5ee8-1199">Prvky poznámky lze použít k poskytnutí dalších metadat o prvcích v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="d5ee8-1200">Počínaje verzí .NET Framework 4 je k metadatům obsaženým v prvcích poznámky možné přistupovat za běhu pomocí tříd v oboru názvů System. data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-1201">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1201">Example</span></span>

<span data-ttu-id="d5ee8-1202">Následující příklad ukazuje element **EntityType** s elementem Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="d5ee8-1203">Příklad také ukazuje atribut anotace aplikovaný na prvek typu entity.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="d5ee8-1204">Následující kód načte metadata v prvku anotace a zapíše jej do konzoly:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="d5ee8-1205">Výše uvedený kód předpokládá, že soubor School. csdl je ve výstupním adresáři projektu a že jste do projektu přidali následující příkazy `Imports` a `Using`:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="d5ee8-1206">Typy konceptuálních modelů (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="d5ee8-1207">Model CSDL (konceptuální schéma Definition Language) podporuje sadu abstraktních primitivních datových typů s názvem **EDMSimpleTypes**, které definují vlastnosti v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="d5ee8-1208">**EDMSimpleTypes** jsou proxy pro primitivní datové typy, které jsou podporovány v úložišti nebo hostitelském prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="d5ee8-1209">Následující tabulka uvádí primitivní datové typy, které podporuje CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="d5ee8-1210">Tabulka obsahuje také seznam omezujících vlastností, které lze použít na jednotlivé **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="d5ee8-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="d5ee8-1212">Popis</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1212">Description</span></span>                                                | <span data-ttu-id="d5ee8-1213">Použitelné omezující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="d5ee8-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="d5ee8-1215">Obsahuje binární data.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="d5ee8-1216">MaxLength, FixedLength, Nullable, default</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="d5ee8-1217">**EDM. Boolean**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="d5ee8-1218">Obsahuje hodnotu **true** nebo **false**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="d5ee8-1219">Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="d5ee8-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="d5ee8-1221">Obsahuje 8bitové celočíselnou hodnotu bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="d5ee8-1222">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="d5ee8-1224">Představuje datum a čas.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="d5ee8-1225">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1226">**EDM. DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="d5ee8-1227">Obsahuje datum a čas jako posun v minutách od času GMT.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="d5ee8-1228">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="d5ee8-1230">Obsahuje číselnou hodnotu s pevnou přesností a škálováním.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="d5ee8-1231">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1232">**EDM. Double**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="d5ee8-1233">Obsahuje číslo s plovoucí desetinnou čárkou s přesností na 15 číslic.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="d5ee8-1234">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="d5ee8-1236">Obsahuje číslo s plovoucí desetinnou čárkou s přesností na 7 číslic.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="d5ee8-1237">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="d5ee8-1239">Obsahuje jedinečný identifikátor o velikosti 16 bajtů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="d5ee8-1240">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="d5ee8-1242">Obsahuje 16bitová celočíselnou hodnotu se znaménkem.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="d5ee8-1243">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1244">**EDM. Int32**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="d5ee8-1245">Obsahuje podepsaná 32 celočíselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="d5ee8-1246">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1247">**EDM. Int64**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="d5ee8-1248">Obsahuje podepsaná 64 celočíselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="d5ee8-1249">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="d5ee8-1251">Obsahuje 8bitové celočíselnou hodnotu se znaménkem.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="d5ee8-1252">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1253">**Edm.String**</span></span>                   | <span data-ttu-id="d5ee8-1254">Obsahuje znaková data.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1254">Contains character data.</span></span>                                   | <span data-ttu-id="d5ee8-1255">Unicode, FixedLength, MaxLength, kolace, přesnost, Nullable, default</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="d5ee8-1256">**EDM. time**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="d5ee8-1257">Obsahuje denní dobu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="d5ee8-1258">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="d5ee8-1259">**EDM. geografie**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="d5ee8-1260">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1261">**EDM. GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="d5ee8-1262">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="d5ee8-1264">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="d5ee8-1266">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="d5ee8-1268">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="d5ee8-1270">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="d5ee8-1272">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1273">**EDM. geografie –Collection**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="d5ee8-1274">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="d5ee8-1276">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="d5ee8-1278">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="d5ee8-1280">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="d5ee8-1282">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="d5ee8-1284">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="d5ee8-1286">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="d5ee8-1288">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="d5ee8-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="d5ee8-1290">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="d5ee8-1291">Omezující vlastnosti (CSDL)</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1291">Facets (CSDL)</span></span>

<span data-ttu-id="d5ee8-1292">Charakteristiky v jazyce CSDL (konceptuální schéma Definition Language) reprezentují omezení vlastností typů entit a komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="d5ee8-1293">Omezující vlastnosti se zobrazí jako atributy XML u následujících elementů CSDL:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="d5ee8-1294">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1294">Property</span></span>
-   <span data-ttu-id="d5ee8-1295">Odkaz</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1295">TypeRef</span></span>
-   <span data-ttu-id="d5ee8-1296">Parametr</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1296">Parameter</span></span>

<span data-ttu-id="d5ee8-1297">Následující tabulka obsahuje popis omezujících vlastností, které jsou podporovány v CSDL.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="d5ee8-1298">Všechny omezující vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1298">All facets are optional.</span></span> <span data-ttu-id="d5ee8-1299">Některé omezující vlastnosti, které jsou uvedeny níže, jsou používány Entity Framework při generování databáze z koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ee8-1300">Informace o datových typech v koncepčním modelu naleznete v tématu typy konceptuálních modelů (CSDL).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="d5ee8-1301">Omezující</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1301">Facet</span></span>               | <span data-ttu-id="d5ee8-1302">Popis</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="d5ee8-1303">Platná pro</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="d5ee8-1304">Používá se pro generování databáze.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1304">Used for the database generation</span></span> | <span data-ttu-id="d5ee8-1305">Používáno modulem runtime</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="d5ee8-1306">**Velké**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1306">**Collation**</span></span>       | <span data-ttu-id="d5ee8-1307">Určuje pořadí kompletování (nebo řazení sekvence), které se má použít při provádění operací porovnání a řazení u hodnot vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="d5ee8-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d5ee8-1309">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1309">Yes</span></span>                              | <span data-ttu-id="d5ee8-1310">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1310">No</span></span>                  |
| <span data-ttu-id="d5ee8-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="d5ee8-1312">Označuje, že hodnota vlastnosti by měla být použita pro kontroly optimistického řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="d5ee8-1313">Všechny vlastnosti **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="d5ee8-1314">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1314">No</span></span>                               | <span data-ttu-id="d5ee8-1315">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1315">Yes</span></span>                 |
| <span data-ttu-id="d5ee8-1316">**Výchozí**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1316">**Default**</span></span>         | <span data-ttu-id="d5ee8-1317">Určuje výchozí hodnotu vlastnosti, pokud není při vytváření instance zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="d5ee8-1318">Všechny vlastnosti **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="d5ee8-1319">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1319">Yes</span></span>                              | <span data-ttu-id="d5ee8-1320">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1320">Yes</span></span>                 |
| <span data-ttu-id="d5ee8-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1321">**FixedLength**</span></span>     | <span data-ttu-id="d5ee8-1322">Určuje, zda může být délka hodnoty vlastnosti odlišná.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="d5ee8-1323">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d5ee8-1324">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1324">Yes</span></span>                              | <span data-ttu-id="d5ee8-1325">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1325">No</span></span>                  |
| <span data-ttu-id="d5ee8-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1326">**MaxLength**</span></span>       | <span data-ttu-id="d5ee8-1327">Určuje maximální délku hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="d5ee8-1328">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d5ee8-1329">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1329">Yes</span></span>                              | <span data-ttu-id="d5ee8-1330">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1330">No</span></span>                  |
| <span data-ttu-id="d5ee8-1331">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1331">**Nullable**</span></span>        | <span data-ttu-id="d5ee8-1332">Určuje, zda vlastnost může mít hodnotu **null** .</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="d5ee8-1333">Všechny vlastnosti **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="d5ee8-1334">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1334">Yes</span></span>                              | <span data-ttu-id="d5ee8-1335">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1335">Yes</span></span>                 |
| <span data-ttu-id="d5ee8-1336">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1336">**Precision**</span></span>       | <span data-ttu-id="d5ee8-1337">Pro vlastnosti typu **Decimal**určuje počet číslic, které může mít hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="d5ee8-1338">Pro vlastnosti typu **Time**, **DateTime**a **DateTimeOffset**určuje počet číslic pro zlomkovou část hodnoty vlastnosti v sekundách.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="d5ee8-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **EDM. time**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="d5ee8-1340">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1340">Yes</span></span>                              | <span data-ttu-id="d5ee8-1341">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1341">No</span></span>                  |
| <span data-ttu-id="d5ee8-1342">**Kapacity**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1342">**Scale**</span></span>           | <span data-ttu-id="d5ee8-1343">Určuje počet číslic vpravo od desetinné čárky pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="d5ee8-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="d5ee8-1345">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1345">Yes</span></span>                              | <span data-ttu-id="d5ee8-1346">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1346">No</span></span>                  |
| <span data-ttu-id="d5ee8-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1347">**SRID**</span></span>            | <span data-ttu-id="d5ee8-1348">Určuje ID referenčního systému prostorového systému.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="d5ee8-1349">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="d5ee8-1350">**EDM. geografie, Edm. GeographyPoint, Edm. GeographyLineString, Edm. GeographyPolygon, Edm. GeographyMultiPoint, Edm. GeographyMultiLineString, Edm. GeographyMultiPolygon, Edm. geografie, Edm. Geometry, Edm. GeometryPoint, EDM. GeometryLineString, Edm. GeometryPolygon, Edm. GeometryMultiPoint, Edm. GeometryMultiLineString, Edm. GeometryMultiPolygon, Edm. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="d5ee8-1351">Ne</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1351">No</span></span>                               | <span data-ttu-id="d5ee8-1352">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1352">Yes</span></span>                 |
| <span data-ttu-id="d5ee8-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1353">**Unicode**</span></span>         | <span data-ttu-id="d5ee8-1354">Určuje, zda je hodnota vlastnosti uložena jako Unicode.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="d5ee8-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="d5ee8-1356">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1356">Yes</span></span>                              | <span data-ttu-id="d5ee8-1357">Ano</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="d5ee8-1358">Při generování databáze z koncepčního modelu Průvodce generováním databáze rozpozná hodnotu atributu **StoreGeneratedPattern** elementu **Property** , pokud je v následujícím oboru názvů: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="d5ee8-1359">Podporované hodnoty atributu jsou **identity** a **počítané**.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="d5ee8-1360">Hodnota **identity** vytvoří sloupec databáze s hodnotou identity, která je vygenerována v databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="d5ee8-1361">Hodnota **počítaná** vygeneruje sloupec s hodnotou, která je vypočítána v databázi.</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="d5ee8-1362">Příklad</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1362">Example</span></span>

<span data-ttu-id="d5ee8-1363">Následující příklad ukazuje charakteristiky použité pro vlastnosti typu entity:</span><span class="sxs-lookup"><span data-stu-id="d5ee8-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
