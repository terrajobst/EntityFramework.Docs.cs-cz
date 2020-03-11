---
title: Specifikace CSDL – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418775"
---
# <a name="csdl-specification"></a><span data-ttu-id="2e16d-102">Specifikace CSDL</span><span class="sxs-lookup"><span data-stu-id="2e16d-102">CSDL Specification</span></span>
<span data-ttu-id="2e16d-103">Jazyk CSDL (konceptuální schéma Definition Language) je jazyk založený na jazyce XML, který popisuje entity, vztahy a funkce tvořící koncepční model aplikace řízené daty.</span><span class="sxs-lookup"><span data-stu-id="2e16d-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="2e16d-104">Tento koncepční model lze použít Entity Framework nebo WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="2e16d-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="2e16d-105">Metadata popsaná pomocí CSDL používá Entity Framework k mapování entit a vztahů, které jsou definovány v koncepčním modelu na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="2e16d-106">Další informace najdete v tématu Specifikace [SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) a [specifikace MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="2e16d-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="2e16d-107">CSDL je Entity Framework implementace model EDM (Entity Data Model).</span><span class="sxs-lookup"><span data-stu-id="2e16d-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="2e16d-108">V Entity Framework aplikaci jsou metadata konceptuálního modelu načtena ze souboru. csdl (napsaného v CSDL) do instance System. data. Metadata. Edm. EdmItemCollection a je přístupná pomocí metod v System. data. Metadata. Edm. MetadataWorkspace – třída</span><span class="sxs-lookup"><span data-stu-id="2e16d-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="2e16d-109">Entity Framework používá metadata konceptuálního modelu k překladu dotazů na koncepční model na příkazy specifické pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="2e16d-110">Návrhář EF ukládá informace o koncepčním modelu do souboru. edmx v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="2e16d-111">V okamžiku sestavení používá Návrhář EF informace v souboru. edmx k vytvoření souboru. csdl, který je vyžadován Entity Framework za běhu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="2e16d-112">Verze CSDL jsou odlišeny obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="2e16d-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="2e16d-113">Verze CSDL</span><span class="sxs-lookup"><span data-stu-id="2e16d-113">CSDL Version</span></span> | <span data-ttu-id="2e16d-114">Obor názvů XML</span><span class="sxs-lookup"><span data-stu-id="2e16d-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="2e16d-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="2e16d-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="2e16d-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="2e16d-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="2e16d-117">CSDL V3</span><span class="sxs-lookup"><span data-stu-id="2e16d-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="2e16d-118">Association – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-118">Association Element (CSDL)</span></span>

<span data-ttu-id="2e16d-119">Element **Association** definuje vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="2e16d-120">Přidružení musí určovat typy entit, které jsou součástí relace, a možný počet typů entit na každém konci relace, který se označuje jako násobnost.</span><span class="sxs-lookup"><span data-stu-id="2e16d-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="2e16d-121">Násobnost elementu end přidružení může mít hodnotu jedna (1), 0 nebo 1 (0.. 1) nebo mnoho (\*).</span><span class="sxs-lookup"><span data-stu-id="2e16d-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="2e16d-122">Tyto informace jsou zadány ve dvou podřízených elementech end.</span><span class="sxs-lookup"><span data-stu-id="2e16d-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="2e16d-123">Instance typu entity na jednom konci přidružení lze zpřístupnit prostřednictvím vlastností navigace nebo cizích klíčů, pokud jsou vystaveny na typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="2e16d-124">V aplikaci instance přidružení představuje konkrétní přidružení mezi instancemi typů entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="2e16d-125">Instance přidružení jsou logicky seskupeny v sadě přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="2e16d-126">Element **Association** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-127">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-128">End (přesně 2 elementy)</span><span class="sxs-lookup"><span data-stu-id="2e16d-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="2e16d-129">Elementu ReferentialConstraint (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-130">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-131">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-131">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-132">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Association** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="2e16d-133">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-133">Attribute Name</span></span> | <span data-ttu-id="2e16d-134">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-134">Is Required</span></span> | <span data-ttu-id="2e16d-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="2e16d-136">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-136">**Name**</span></span>       | <span data-ttu-id="2e16d-137">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-137">Yes</span></span>         | <span data-ttu-id="2e16d-138">Název přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-139">Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="2e16d-140">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-141">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-142">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-142">Example</span></span>

<span data-ttu-id="2e16d-143">Následující příklad ukazuje element **Association** definující přidružení **CustomerOrders** v případě, že cizí klíče nebyly zveřejněny na typech entit **zákazníka** a **objednávky** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="2e16d-144">Hodnoty **násobnosti** pro každý **konec** přidružení označují, že mnoho **objednávek** může být přidruženo k **zákazníkovi**, ale k **objednávce**je možné přidružit pouze jednoho **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="2e16d-145">Kromě toho element **IsDeleted** označuje, že všechny **objednávky** , které souvisejí s konkrétním **zákazníkem** a byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="2e16d-146">Následující příklad ukazuje element **Association** definující přidružení **CustomerOrders** , pokud byly cizí klíče vystaveny na typech entit **Zákazník** a **objednávka** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="2e16d-147">Při zpřístupnění cizích klíčů je vztah mezi entitami spravován pomocí elementu **elementu ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="2e16d-148">Odpovídající element AssociationSetMapping není nutný k mapování tohoto přidružení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="2e16d-149">AssociationSet – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="2e16d-150">Element **AssociationSet** v jazyce CSDL (konceptuální schéma Definition Language) je logický kontejner pro instance přidružení stejného typu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="2e16d-151">Sada přidružení poskytuje definici pro seskupení instancí přidružení, aby bylo možné je namapovat na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="2e16d-152">Element **AssociationSet** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-153">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="2e16d-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="2e16d-154">End (vyžaduje se přesně dva prvky)</span><span class="sxs-lookup"><span data-stu-id="2e16d-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="2e16d-155">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="2e16d-156">Atribut **Association** určuje typ přidružení, které sada přidružení obsahuje.</span><span class="sxs-lookup"><span data-stu-id="2e16d-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="2e16d-157">Sady entit, které tvoří konce sady přidružení, jsou zadány pomocí přesně dvou podřízených elementů **End** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-158">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-158">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-159">Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="2e16d-160">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-160">Attribute Name</span></span>  | <span data-ttu-id="2e16d-161">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-161">Is Required</span></span> | <span data-ttu-id="2e16d-162">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-163">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-163">**Name**</span></span>        | <span data-ttu-id="2e16d-164">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-164">Yes</span></span>         | <span data-ttu-id="2e16d-165">Název sady entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-165">The name of the entity set.</span></span> <span data-ttu-id="2e16d-166">Hodnota atributu **Name** nemůže být stejná jako hodnota atributu **Association** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="2e16d-167">**Řídí**</span><span class="sxs-lookup"><span data-stu-id="2e16d-167">**Association**</span></span> | <span data-ttu-id="2e16d-168">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-168">Yes</span></span>         | <span data-ttu-id="2e16d-169">Plně kvalifikovaný název asociace, který sada přidružení obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="2e16d-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="2e16d-170">Přidružení musí být ve stejném oboru názvů jako sada přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-171">Pro element **AssociationSet** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="2e16d-172">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-173">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-174">Example</span></span>

<span data-ttu-id="2e16d-175">Následující příklad ukazuje element **EntityContainer** se dvěma elementy **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="2e16d-176">CollectionType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="2e16d-177">Element **CollectionType** v jazyce CSDL (konceptuální schéma Definition Language) určuje, že parametr funkce nebo návratový typ funkce je kolekce.</span><span class="sxs-lookup"><span data-stu-id="2e16d-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="2e16d-178">Element **CollectionType** může být podřízeným elementem parametru nebo elementem ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="2e16d-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="2e16d-179">Typ kolekce lze zadat buď pomocí atributu **Type** , nebo jednoho z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="2e16d-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="2e16d-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-180">**CollectionType**</span></span>
-   <span data-ttu-id="2e16d-181">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="2e16d-181">ReferenceType</span></span>
-   <span data-ttu-id="2e16d-182">RowType</span><span class="sxs-lookup"><span data-stu-id="2e16d-182">RowType</span></span>
-   <span data-ttu-id="2e16d-183">Odkaz</span><span class="sxs-lookup"><span data-stu-id="2e16d-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-184">Model nebude ověřen, pokud je zadán typ kolekce s atributem **typu** i s podřízeným prvkem.</span><span class="sxs-lookup"><span data-stu-id="2e16d-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-185">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-185">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-186">Následující tabulka popisuje atributy, které mohou být aplikovány na element **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="2e16d-187">Všimněte si, že atributy **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**a **kolace** platí pouze pro kolekce **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="2e16d-188">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-188">Attribute Name</span></span>                                                          | <span data-ttu-id="2e16d-189">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-189">Is Required</span></span> | <span data-ttu-id="2e16d-190">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-191">**Type**</span></span>                                                                | <span data-ttu-id="2e16d-192">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-192">No</span></span>          | <span data-ttu-id="2e16d-193">Typ kolekce</span><span class="sxs-lookup"><span data-stu-id="2e16d-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="2e16d-194">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="2e16d-194">**Nullable**</span></span>                                                            | <span data-ttu-id="2e16d-195">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-195">No</span></span>          | <span data-ttu-id="2e16d-196">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2e16d-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="2e16d-197">> V CSDL V1 musí mít vlastnost komplexního typu `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="2e16d-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="2e16d-198">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="2e16d-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="2e16d-199">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-199">No</span></span>          | <span data-ttu-id="2e16d-200">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="2e16d-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="2e16d-202">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-202">No</span></span>          | <span data-ttu-id="2e16d-203">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="2e16d-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="2e16d-205">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-205">No</span></span>          | <span data-ttu-id="2e16d-206">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="2e16d-207">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-207">**Precision**</span></span>                                                           | <span data-ttu-id="2e16d-208">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-208">No</span></span>          | <span data-ttu-id="2e16d-209">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="2e16d-210">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-210">**Scale**</span></span>                                                               | <span data-ttu-id="2e16d-211">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-211">No</span></span>          | <span data-ttu-id="2e16d-212">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="2e16d-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-213">**SRID**</span></span>                                                                | <span data-ttu-id="2e16d-214">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-214">No</span></span>          | <span data-ttu-id="2e16d-215">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="2e16d-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2e16d-216">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="2e16d-217">   Další informace najdete v tématech [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) .</span><span class="sxs-lookup"><span data-stu-id="2e16d-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="2e16d-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-218">**Unicode**</span></span>                                                             | <span data-ttu-id="2e16d-219">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-219">No</span></span>          | <span data-ttu-id="2e16d-220">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="2e16d-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="2e16d-221">**Velké**</span><span class="sxs-lookup"><span data-stu-id="2e16d-221">**Collation**</span></span>                                                           | <span data-ttu-id="2e16d-222">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-222">No</span></span>          | <span data-ttu-id="2e16d-223">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="2e16d-224">Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="2e16d-225">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-226">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-227">Example</span></span>

<span data-ttu-id="2e16d-228">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci typů entit **Person** (jak je určeno atributem **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="2e16d-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="2e16d-229">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="2e16d-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="2e16d-230">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce přijímá jako parametr kolekce typů entit **oddělení** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="2e16d-231">ComplexType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="2e16d-232">Element **complexType** definuje strukturu dat složenou z vlastností **EdmSimpleType** nebo jiných komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="2e16d-233">  Komplexní typ může být vlastnost typu entity nebo jiného komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="2e16d-234">Komplexní typ je podobný typu entity v tom, že komplexní typ definuje data.</span><span class="sxs-lookup"><span data-stu-id="2e16d-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="2e16d-235">Existují však některé klíčové rozdíly mezi komplexními typy a typy entit:</span><span class="sxs-lookup"><span data-stu-id="2e16d-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="2e16d-236">Komplexní typy nemají identity (nebo klíče), a proto nemohou existovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="2e16d-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="2e16d-237">Komplexní typy mohou existovat pouze jako vlastnosti typů entit nebo jiných komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="2e16d-238">Komplexní typy se nemůžou účastnit přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="2e16d-239">Ani konec přidružení může být komplexní typ, a proto nelze definovat vlastnosti navigace pro komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="2e16d-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="2e16d-240">Vlastnost komplexního typu nemůže mít hodnotu null, ačkoliv skalární vlastnosti komplexního typu mohou být nastaveny na hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2e16d-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="2e16d-241">Element **complexType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-242">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-243">Vlastnost (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="2e16d-244">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="2e16d-245">Následující tabulka popisuje atributy, které lze použít pro element **complexType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="2e16d-246">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="2e16d-247">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-247">Is Required</span></span> | <span data-ttu-id="2e16d-248">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-249">Název</span><span class="sxs-lookup"><span data-stu-id="2e16d-249">Name</span></span>                                                                                                           | <span data-ttu-id="2e16d-250">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-250">Yes</span></span>         | <span data-ttu-id="2e16d-251">Název komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-251">The name of the complex type.</span></span> <span data-ttu-id="2e16d-252">Název komplexního typu nemůže být stejný jako název jiného komplexního typu, typu entity nebo asociace, který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="2e16d-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="2e16d-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="2e16d-254">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-254">No</span></span>          | <span data-ttu-id="2e16d-255">Název jiného komplexního typu, který je základním typem komplexního typu, který je definován.</span><span class="sxs-lookup"><span data-stu-id="2e16d-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="2e16d-256">> Tento atribut nelze použít v CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="2e16d-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="2e16d-257">Dědičnost pro komplexní typy není v této verzi podporována.</span><span class="sxs-lookup"><span data-stu-id="2e16d-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="2e16d-258">Shrnutí</span><span class="sxs-lookup"><span data-stu-id="2e16d-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="2e16d-259">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-259">No</span></span>          | <span data-ttu-id="2e16d-260">**True** nebo **false** (výchozí hodnota) v závislosti na tom, zda je komplexní typ abstraktní typ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="2e16d-261">> Tento atribut nelze použít v CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="2e16d-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="2e16d-262">Komplexní typy v této verzi nemohou být abstraktní typy.</span><span class="sxs-lookup"><span data-stu-id="2e16d-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="2e16d-263">Pro element **complexType** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="2e16d-264">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-265">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-266">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-266">Example</span></span>

<span data-ttu-id="2e16d-267">Následující příklad ukazuje komplexní typ a **adresu**s vlastností **EdmSimpleType** **StreetAddress**, **City**, **StateOrProvince**, **Country**a **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="2e16d-268">Chcete-li definovat **adresu** komplexního typu (výše) jako vlastnost typu entity, je nutné deklarovat typ vlastnosti v definici typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="2e16d-269">Následující příklad ukazuje vlastnost **adresa** jako komplexní typ pro typ entity (**Vydavatel**):</span><span class="sxs-lookup"><span data-stu-id="2e16d-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="2e16d-270">DefiningExpression – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="2e16d-271">Element **DefiningExpression** v jazyce CSDL (konceptuální schéma Definition Language) obsahuje výraz Entity SQL, který definuje funkci v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="2e16d-272">Pro účely ověřování může element **DefiningExpression** obsahovat libovolný obsah.</span><span class="sxs-lookup"><span data-stu-id="2e16d-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="2e16d-273">Nicméně Entity Framework vyvolá výjimku za běhu, pokud element **DefiningExpression** neobsahuje platný Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-274">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-274">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-275">Pro element **DefiningExpression** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="2e16d-276">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-277">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-278">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-278">Example</span></span>

<span data-ttu-id="2e16d-279">Následující příklad používá element **DefiningExpression** k definování funkce, která vrátí počet roků od publikování knihy.</span><span class="sxs-lookup"><span data-stu-id="2e16d-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="2e16d-280">Obsah elementu **DefiningExpression** je napsaný v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="2e16d-281">Závislý element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="2e16d-282">**Závislý** element v jazyce CSDL (konceptuální schéma Definition Language) je podřízeným elementem elementu elementu ReferentialConstraint a definuje závislý konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="2e16d-283">Element **elementu ReferentialConstraint** definuje funkčnost, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="2e16d-284">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="2e16d-285">Odkazovaný typ entity se nazývá *hlavní konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="2e16d-286">Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="2e16d-287">Prvky **PropertyRef** slouží k určení, které klíče odkazují na hlavní element end.</span><span class="sxs-lookup"><span data-stu-id="2e16d-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="2e16d-288">**Závislý** element může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-289">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="2e16d-290">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-291">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-291">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-292">Následující tabulka popisuje atributy, které mohou být aplikovány na **závislý** element.</span><span class="sxs-lookup"><span data-stu-id="2e16d-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="2e16d-293">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-293">Attribute Name</span></span> | <span data-ttu-id="2e16d-294">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-294">Is Required</span></span> | <span data-ttu-id="2e16d-295">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="2e16d-296">**Role**</span><span class="sxs-lookup"><span data-stu-id="2e16d-296">**Role**</span></span>       | <span data-ttu-id="2e16d-297">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-297">Yes</span></span>         | <span data-ttu-id="2e16d-298">Název typu entity na závislém konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-299">Pro **závislý** element lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="2e16d-300">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-301">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-302">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-302">Example</span></span>

<span data-ttu-id="2e16d-303">Následující příklad ukazuje element **elementu ReferentialConstraint** , který se používá jako součást definice asociace **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="2e16d-304">Vlastnost **PublisherId** typu entity **Book** vytváří závislý konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="2e16d-305">Element dokumentace (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="2e16d-306">Element **dokumentace** v jazyce CSDL (konceptuální schéma Definition Language) lze použít k poskytnutí informací o objektu, který je definován v nadřazeném elementu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="2e16d-307">V souboru EDMX, pokud je prvek **dokumentace** podřízenosti prvku, který se zobrazuje jako objekt na návrhové ploše návrháře EF (například entita, asociace nebo vlastnost), obsah elementu **dokumentace** se zobrazí v okně **vlastností** sady Visual Studio pro daný objekt.</span><span class="sxs-lookup"><span data-stu-id="2e16d-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="2e16d-308">Prvek **dokumentace** může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-309">**Summary**: stručný popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="2e16d-310">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-310">(zero or one element)</span></span>
-   <span data-ttu-id="2e16d-311">**Longdescription**: rozsáhlý popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="2e16d-312">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-312">(zero or one element)</span></span>
-   <span data-ttu-id="2e16d-313">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="2e16d-313">Annotation elements.</span></span> <span data-ttu-id="2e16d-314">(nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-315">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-315">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-316">Pro prvek **dokumentace** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="2e16d-317">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-318">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-319">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-319">Example</span></span>

<span data-ttu-id="2e16d-320">Následující příklad ukazuje prvek **dokumentace** jako podřízený prvek elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="2e16d-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="2e16d-321">Pokud následující fragment kódu byl v obsahu CSDL souboru. edmx, obsah **souhrnu** a elementů **Longdescription** se zobrazí v okně **vlastnosti** sady Visual Studio, když kliknete na typ entity `Customer`.</span><span class="sxs-lookup"><span data-stu-id="2e16d-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="2e16d-322">End – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-322">End Element (CSDL)</span></span>

<span data-ttu-id="2e16d-323">**Koncovým** elementem v jazyce CSDL (koncepční schéma Definition Language) může být podřízeným prvkem elementu Association nebo elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="2e16d-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="2e16d-324">V každém případě je role elementu **End** odlišná a příslušné atributy se liší.</span><span class="sxs-lookup"><span data-stu-id="2e16d-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="2e16d-325">Ukončit element jako podřízený elementu přidružení</span><span class="sxs-lookup"><span data-stu-id="2e16d-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="2e16d-326">Element **End** (jako podřízený prvek elementu **Association** ) identifikuje typ entity na jednom konci přidružení a počet instancí typu entity, které mohou existovat na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="2e16d-327">Zakončení přidružení jsou definována jako součást přidružení; přidružení musí mít přesně dvě zakončení přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="2e16d-328">Instance typu entity na jednom konci přidružení lze zpřístupnit prostřednictvím vlastností navigace nebo cizích klíčů, pokud jsou vystaveny na typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="2e16d-329">Element **End** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-330">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-331">Při odstranění (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-332">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-333">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-333">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-334">Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízenou položkou elementu **Association** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="2e16d-335">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-335">Attribute Name</span></span>   | <span data-ttu-id="2e16d-336">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-336">Is Required</span></span> | <span data-ttu-id="2e16d-337">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-338">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-338">**Type**</span></span>         | <span data-ttu-id="2e16d-339">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-339">Yes</span></span>         | <span data-ttu-id="2e16d-340">Název typu entity na jednom konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="2e16d-341">**Role**</span><span class="sxs-lookup"><span data-stu-id="2e16d-341">**Role**</span></span>         | <span data-ttu-id="2e16d-342">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-342">No</span></span>          | <span data-ttu-id="2e16d-343">Název zakončení přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-343">A name for the association end.</span></span> <span data-ttu-id="2e16d-344">Pokud není zadaný žádný název, použije se název typu entity na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="2e16d-345">**Násobnost**</span><span class="sxs-lookup"><span data-stu-id="2e16d-345">**Multiplicity**</span></span> | <span data-ttu-id="2e16d-346">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-346">Yes</span></span>         | <span data-ttu-id="2e16d-347">**1**, **0.. 1**nebo **\*** v závislosti na počtu instancí typu entity, které mohou být na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="2e16d-348">**1** znamená, že na konci přidružení existuje přesně jedna instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="2e16d-349">**0.. 1** znamená, že na konci přidružení existují žádné nebo jedna instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="2e16d-350">**\*** označuje, že na konci přidružení existují žádné instance typu entity nula, jedna nebo více.</span><span class="sxs-lookup"><span data-stu-id="2e16d-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-351">Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="2e16d-352">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-353">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="2e16d-354">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-354">Example</span></span>

<span data-ttu-id="2e16d-355">Následující příklad ukazuje element **Association** , který definuje přidružení **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="2e16d-356">Hodnoty **násobnosti** pro každý **konec** přidružení označují, že mnoho **objednávek** může být přidruženo k **zákazníkovi**, ale k **objednávce**je možné přidružit pouze jednoho **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="2e16d-357">Kromě toho element **IsDeleted** označuje, že všechny **objednávky** , které se vztahují k určitému **zákazníkovi** a které byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="2e16d-358">Ukončit element jako podřízený element elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="2e16d-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="2e16d-359">Element **End** Určuje jeden konec sady přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="2e16d-360">Element **AssociationSet** musí obsahovat dva elementy **End** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="2e16d-361">Informace obsažené v elementu **End** se používají v mapování sady přidružení na zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="2e16d-362">Element **End** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-363">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-364">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-365">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích.</span><span class="sxs-lookup"><span data-stu-id="2e16d-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="2e16d-366">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="2e16d-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-367">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-367">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-368">Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízeným elementem elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="2e16d-369">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-369">Attribute Name</span></span> | <span data-ttu-id="2e16d-370">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-370">Is Required</span></span> | <span data-ttu-id="2e16d-371">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-372">**Sada**</span><span class="sxs-lookup"><span data-stu-id="2e16d-372">**EntitySet**</span></span>  | <span data-ttu-id="2e16d-373">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-373">Yes</span></span>         | <span data-ttu-id="2e16d-374">Název elementu **EntitySet** , který definuje jeden element end nadřazeného elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="2e16d-375">Element **EntitySet** musí být definován ve stejném kontejneru entity jako nadřazený element **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="2e16d-376">**Role**</span><span class="sxs-lookup"><span data-stu-id="2e16d-376">**Role**</span></span>       | <span data-ttu-id="2e16d-377">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-377">No</span></span>          | <span data-ttu-id="2e16d-378">Název zakončení sady přidružení</span><span class="sxs-lookup"><span data-stu-id="2e16d-378">The name of the association set end.</span></span> <span data-ttu-id="2e16d-379">Pokud se atribut **role** nepoužívá, název zakončení sady přidružení bude název sady entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="2e16d-380">Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="2e16d-381">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-382">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="2e16d-383">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-383">Example</span></span>

<span data-ttu-id="2e16d-384">Následující příklad ukazuje element **EntityContainer** se dvěma elementy **AssociationSet** , z nichž každý má dva elementy **End** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="2e16d-385">EntityContainer – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="2e16d-386">Element **EntityContainer** v jazyce CSDL (konceptuální schéma Definition Language) je logický kontejner pro sady entit, sady přidružení a importy funkcí.</span><span class="sxs-lookup"><span data-stu-id="2e16d-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="2e16d-387">Kontejner entity koncepčního modelu se mapuje na kontejner entit modelu úložiště prostřednictvím elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="2e16d-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="2e16d-388">Kontejner entity modelu úložiště popisuje strukturu databáze: sady entit popisují tabulky, sady přidružení popisují omezení cizího klíče a importy funkcí popisují uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="2e16d-389">Element **EntityContainer** může mít nula nebo jeden prvek dokumentace.</span><span class="sxs-lookup"><span data-stu-id="2e16d-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="2e16d-390">Pokud je přítomen prvek **dokumentace** , musí předcházet všechny elementy **EntitySet**, **AssociationSet**a **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="2e16d-391">Element **EntityContainer** může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-392">Sada</span><span class="sxs-lookup"><span data-stu-id="2e16d-392">EntitySet</span></span>
-   <span data-ttu-id="2e16d-393">Vlastností</span><span class="sxs-lookup"><span data-stu-id="2e16d-393">AssociationSet</span></span>
-   <span data-ttu-id="2e16d-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="2e16d-394">FunctionImport</span></span>
-   <span data-ttu-id="2e16d-395">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="2e16d-395">Annotation elements</span></span>

<span data-ttu-id="2e16d-396">Element **EntityContainer** můžete roztáhnout tak, aby zahrnoval obsah jiného **elementu EntityContainer** , který je ve stejném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="2e16d-397">Chcete-li zahrnout obsah jiného **elementu EntityContainer**, nastavte v odkazujícím elementu **EntityContainer** hodnotu atributu **extends** na název elementu **EntityContainer** , který chcete zahrnout.</span><span class="sxs-lookup"><span data-stu-id="2e16d-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="2e16d-398">Všechny podřízené elementy vloženého elementu **EntityContainer** budou považovány za podřízené prvky odkazujícího elementu **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-399">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-399">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-400">Následující tabulka popisuje atributy, které lze použít pro element **using** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="2e16d-401">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-401">Attribute Name</span></span> | <span data-ttu-id="2e16d-402">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-402">Is Required</span></span> | <span data-ttu-id="2e16d-403">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="2e16d-404">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-404">**Name**</span></span>       | <span data-ttu-id="2e16d-405">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-405">Yes</span></span>         | <span data-ttu-id="2e16d-406">Název kontejneru entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="2e16d-407">**Nachází**</span><span class="sxs-lookup"><span data-stu-id="2e16d-407">**Extends**</span></span>    | <span data-ttu-id="2e16d-408">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-408">No</span></span>          | <span data-ttu-id="2e16d-409">Název jiného kontejneru entity v rámci stejného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-410">Pro element **EntityContainer** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="2e16d-411">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-412">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-413">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-413">Example</span></span>

<span data-ttu-id="2e16d-414">Následující příklad ukazuje element **EntityContainer** , který definuje tři sady entit a dvě sady přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="2e16d-415">EntitySet – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="2e16d-416">Element **EntitySet** v jazyce koncepční definice schématu je logický kontejner pro instance typu entity a instance libovolného typu, který je odvozen z daného typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="2e16d-417">Vztah mezi typem entity a sadou entit je podobný relaci mezi řádkem a tabulkou v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="2e16d-418">Podobně jako u řádku typ entity definuje sadu souvisejících dat, podobně jako tabulka, sada entit obsahuje instance této definice.</span><span class="sxs-lookup"><span data-stu-id="2e16d-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="2e16d-419">Sada entit poskytuje konstrukci pro seskupování instancí typů entit tak, aby mohly být mapovány na související datové struktury ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="2e16d-420">Je možné definovat více než jednu sadu entit pro konkrétní typ entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-421">Návrhář EF nepodporuje koncepční modely, které obsahují více sad entit na typ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="2e16d-422">Element **EntitySet** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-423">Element dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="2e16d-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="2e16d-424">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-425">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-425">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-426">Následující tabulka popisuje atributy, které lze použít pro element **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="2e16d-427">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-427">Attribute Name</span></span> | <span data-ttu-id="2e16d-428">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-428">Is Required</span></span> | <span data-ttu-id="2e16d-429">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-430">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-430">**Name**</span></span>       | <span data-ttu-id="2e16d-431">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-431">Yes</span></span>         | <span data-ttu-id="2e16d-432">Název sady entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="2e16d-433">**Objektu**</span><span class="sxs-lookup"><span data-stu-id="2e16d-433">**EntityType**</span></span> | <span data-ttu-id="2e16d-434">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-434">Yes</span></span>         | <span data-ttu-id="2e16d-435">Plně kvalifikovaný název typu entity, pro který sada entit obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="2e16d-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-436">Pro element **EntitySet** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="2e16d-437">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-438">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-439">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-439">Example</span></span>

<span data-ttu-id="2e16d-440">Následující příklad ukazuje element **EntityContainer** se třemi elementy **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="2e16d-441">Je možné definovat několik sad entit na typ (MEST).</span><span class="sxs-lookup"><span data-stu-id="2e16d-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="2e16d-442">Následující příklad definuje kontejner entit se dvěma sadami entit pro typ entity **Book** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="2e16d-443">EntityType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="2e16d-444">Element **EntityType** reprezentuje strukturu konceptu nejvyšší úrovně, jako je například zákazník nebo objednávka, v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="2e16d-445">Typ entity je šablona pro instance typů entit v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e16d-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="2e16d-446">Každá šablona obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="2e16d-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="2e16d-447">Jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="2e16d-447">A unique name.</span></span> <span data-ttu-id="2e16d-448">(Povinné.)</span><span class="sxs-lookup"><span data-stu-id="2e16d-448">(Required.)</span></span>
-   <span data-ttu-id="2e16d-449">Klíč entity, který je definován jednou nebo více vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="2e16d-450">(Povinné.)</span><span class="sxs-lookup"><span data-stu-id="2e16d-450">(Required.)</span></span>
-   <span data-ttu-id="2e16d-451">Vlastnosti pro obsahující data</span><span class="sxs-lookup"><span data-stu-id="2e16d-451">Properties for containing data.</span></span> <span data-ttu-id="2e16d-452">(Volitelné.)</span><span class="sxs-lookup"><span data-stu-id="2e16d-452">(Optional.)</span></span>
-   <span data-ttu-id="2e16d-453">Navigační vlastnosti, které umožňují navigaci od jednoho konce přidružení k druhému konci.</span><span class="sxs-lookup"><span data-stu-id="2e16d-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="2e16d-454">(Volitelné.)</span><span class="sxs-lookup"><span data-stu-id="2e16d-454">(Optional.)</span></span>

<span data-ttu-id="2e16d-455">V aplikaci instance typu entity představuje konkrétní objekt (například konkrétního zákazníka nebo objednávky).</span><span class="sxs-lookup"><span data-stu-id="2e16d-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="2e16d-456">Každá instance typu entity musí mít v sadě entit jedinečný klíč entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="2e16d-457">Dvě instance typu entity jsou považovány za rovné pouze v případě, že jsou stejného typu a hodnoty jejich klíčů entit jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="2e16d-458">Element **EntityType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-459">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-460">Klíč (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-461">Vlastnost (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="2e16d-462">NavigationProperty (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="2e16d-463">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-464">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-464">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-465">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="2e16d-466">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="2e16d-467">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-467">Is Required</span></span> | <span data-ttu-id="2e16d-468">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-469">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="2e16d-470">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-470">Yes</span></span>         | <span data-ttu-id="2e16d-471">Název typu entity</span><span class="sxs-lookup"><span data-stu-id="2e16d-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="2e16d-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="2e16d-473">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-473">No</span></span>          | <span data-ttu-id="2e16d-474">Název jiného typu entity, který je základním typem entity typu, který je definován.</span><span class="sxs-lookup"><span data-stu-id="2e16d-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="2e16d-475">**Výtah**</span><span class="sxs-lookup"><span data-stu-id="2e16d-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="2e16d-476">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-476">No</span></span>          | <span data-ttu-id="2e16d-477">**Hodnota true** nebo **false**v závislosti na tom, zda je typ entity abstraktní typ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="2e16d-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="2e16d-479">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-479">No</span></span>          | <span data-ttu-id="2e16d-480">**True** nebo **false** v závislosti na tom, zda je typ entity typ otevřené entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="2e16d-481">> Atributu **OpenType** platí pouze pro typy entit, které jsou definovány v koncepčních modelech, které jsou používány s Data Services ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="2e16d-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="2e16d-482">Pro element **EntityType** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="2e16d-483">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-484">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-485">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-485">Example</span></span>

<span data-ttu-id="2e16d-486">Následující příklad ukazuje element **EntityType** se třemi prvky **vlastností** a dvěma elementy **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="2e16d-487">EnumType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="2e16d-488">Element **enumType** reprezentuje Výčtový typ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="2e16d-489">Element **enumType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-490">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-491">Člen (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="2e16d-492">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-493">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-493">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-494">Následující tabulka popisuje atributy, které mohou být aplikovány na element **enumType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="2e16d-495">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-495">Attribute Name</span></span>     | <span data-ttu-id="2e16d-496">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-496">Is Required</span></span> | <span data-ttu-id="2e16d-497">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-498">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-498">**Name**</span></span>           | <span data-ttu-id="2e16d-499">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-499">Yes</span></span>         | <span data-ttu-id="2e16d-500">Název typu entity</span><span class="sxs-lookup"><span data-stu-id="2e16d-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="2e16d-501">**Příznak**</span><span class="sxs-lookup"><span data-stu-id="2e16d-501">**IsFlags**</span></span>        | <span data-ttu-id="2e16d-502">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-502">No</span></span>          | <span data-ttu-id="2e16d-503">**Hodnota true** nebo **false**v závislosti na tom, zda lze typ výčtu použít jako sadu příznaků.</span><span class="sxs-lookup"><span data-stu-id="2e16d-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="2e16d-504">Výchozí hodnota je **false.** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="2e16d-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-505">**UnderlyingType**</span></span> | <span data-ttu-id="2e16d-506">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-506">No</span></span>          | <span data-ttu-id="2e16d-507">**EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** nebo **EDM. SByte** definující rozsah hodnot typu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="2e16d-508">  Výchozí podkladový typ prvků výčtu je **EDM. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-509">Pro element **enumType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="2e16d-510">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-511">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-512">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-512">Example</span></span>

<span data-ttu-id="2e16d-513">Následující příklad ukazuje element **enumType** se třemi prvky **členů** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="2e16d-514">Function – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-514">Function Element (CSDL)</span></span>

<span data-ttu-id="2e16d-515">Element **Function** v jazyce CSDL (konceptuální schéma Definition Language) se používá k definování nebo deklaraci funkcí v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="2e16d-516">Funkce je definována pomocí elementu DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="2e16d-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="2e16d-517">Element **Function** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-518">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-519">Parametr (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="2e16d-520">DefiningExpression (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-521">ReturnType (Function) (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-522">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="2e16d-523">Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** (Function), nebo atributu **ReturnType** (viz níže), ale nikoli obou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="2e16d-524">Možné návratové typy jsou jakékoli EdmSimpleType, typ entity, komplexní typ, typ řádku nebo typ ref (nebo kolekce jednoho z těchto typů).</span><span class="sxs-lookup"><span data-stu-id="2e16d-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-525">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-525">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-526">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Function** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="2e16d-527">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-527">Attribute Name</span></span> | <span data-ttu-id="2e16d-528">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-528">Is Required</span></span> | <span data-ttu-id="2e16d-529">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="2e16d-530">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-530">**Name**</span></span>       | <span data-ttu-id="2e16d-531">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-531">Yes</span></span>         | <span data-ttu-id="2e16d-532">Název funkce</span><span class="sxs-lookup"><span data-stu-id="2e16d-532">The name of the function.</span></span>          |
| <span data-ttu-id="2e16d-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-533">**ReturnType**</span></span> | <span data-ttu-id="2e16d-534">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-534">No</span></span>          | <span data-ttu-id="2e16d-535">Typ vrácený funkcí</span><span class="sxs-lookup"><span data-stu-id="2e16d-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-536">Pro element **Function** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="2e16d-537">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-538">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-539">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-539">Example</span></span>

<span data-ttu-id="2e16d-540">Následující příklad používá element **Function** k definování funkce, která vrátí počet roků od přijetí instruktora.</span><span class="sxs-lookup"><span data-stu-id="2e16d-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="2e16d-541">FunctionImport – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="2e16d-542">Element **FunctionImport** v koncepčním definičním jazyce (CSDL) představuje funkci, která je definována ve zdroji dat, ale je k dispozici pro objekty prostřednictvím koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="2e16d-543">Například prvek funkce v modelu úložiště lze použít k reprezentaci uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="2e16d-544">Element **FunctionImport** v koncepčním modelu představuje odpovídající funkci v aplikaci Entity Framework a je mapována na funkci modelu úložiště pomocí elementu FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="2e16d-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="2e16d-545">Když je funkce volána v aplikaci, odpovídající uložená procedura je spuštěna v databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="2e16d-546">Element **FunctionImport** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-547">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="2e16d-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="2e16d-548">Parametr (povolený nula nebo více elementů)</span><span class="sxs-lookup"><span data-stu-id="2e16d-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="2e16d-549">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="2e16d-550">ReturnType (FunctionImport) (povolené nula nebo více elementů)</span><span class="sxs-lookup"><span data-stu-id="2e16d-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="2e16d-551">Jeden prvek **parametru** by měl být definován pro každý parametr, který funkce přijímá.</span><span class="sxs-lookup"><span data-stu-id="2e16d-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="2e16d-552">Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** (FunctionImport), nebo atributu **ReturnType** (viz níže), ale nikoli obou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="2e16d-553">Hodnota návratového typu musí být kolekce elementů EdmSimpleType, EntityType nebo ComplexType.</span><span class="sxs-lookup"><span data-stu-id="2e16d-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-554">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-554">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-555">Následující tabulka popisuje atributy, které lze použít pro element **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="2e16d-556">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-556">Attribute Name</span></span>   | <span data-ttu-id="2e16d-557">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-557">Is Required</span></span> | <span data-ttu-id="2e16d-558">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-559">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-559">**Name**</span></span>         | <span data-ttu-id="2e16d-560">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-560">Yes</span></span>         | <span data-ttu-id="2e16d-561">Název importované funkce</span><span class="sxs-lookup"><span data-stu-id="2e16d-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="2e16d-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-562">**ReturnType**</span></span>   | <span data-ttu-id="2e16d-563">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-563">No</span></span>          | <span data-ttu-id="2e16d-564">Typ, který funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="2e16d-564">The type that the function returns.</span></span> <span data-ttu-id="2e16d-565">Nepoužívejte tento atribut, pokud funkce nevrátí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="2e16d-566">V opačném případě musí být hodnotou kolekce ComplexType, EntityType nebo EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="2e16d-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="2e16d-567">**Sada**</span><span class="sxs-lookup"><span data-stu-id="2e16d-567">**EntitySet**</span></span>    | <span data-ttu-id="2e16d-568">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-568">No</span></span>          | <span data-ttu-id="2e16d-569">Pokud funkce vrátí kolekci typů entit, hodnota **objektu EntitySet** musí být sada entit, do které kolekce patří.</span><span class="sxs-lookup"><span data-stu-id="2e16d-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="2e16d-570">V opačném případě nesmí být použit atribut **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="2e16d-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="2e16d-571">**IsComposable**</span></span> | <span data-ttu-id="2e16d-572">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-572">No</span></span>          | <span data-ttu-id="2e16d-573">Pokud je hodnota nastavena na true, funkce je vytvořena (funkce vracející tabulku) a lze ji použít v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="2e16d-574">  Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="2e16d-575">Pro element **FunctionImport** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="2e16d-576">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-577">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-578">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-578">Example</span></span>

<span data-ttu-id="2e16d-579">Následující příklad ukazuje element **FunctionImport** , který přijímá jeden parametr a vrací kolekci typů entit:</span><span class="sxs-lookup"><span data-stu-id="2e16d-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="2e16d-580">Key – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-580">Key Element (CSDL)</span></span>

<span data-ttu-id="2e16d-581">**Klíčový** prvek je podřízený prvek elementu EntityType a definuje *klíč entity* (vlastnost nebo sadu vlastností typu entity, který určuje identitu).</span><span class="sxs-lookup"><span data-stu-id="2e16d-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="2e16d-582">Vlastnosti, které tvoří klíč entity, se volí v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="2e16d-583">Hodnoty vlastností klíče entity musí jedinečně určovat instanci typu entity v sadě entit za běhu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="2e16d-584">Pro zajištění jedinečnosti instancí v sadě entit by se měla zvolit vlastnosti, které tvoří klíč entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="2e16d-585">**Klíčový** prvek definuje klíč entity odkazem na jednu nebo více vlastností typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="2e16d-586">**Klíčový** prvek může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="2e16d-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="2e16d-587">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="2e16d-588">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-589">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-589">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-590">Pro **klíčový** element lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="2e16d-591">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-592">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-593">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-593">Example</span></span>

<span data-ttu-id="2e16d-594">Následující příklad definuje typ entity s názvem **Book**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="2e16d-595">Klíč entity je definován pomocí odkazu na vlastnost **ISBN** typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="2e16d-596">Vlastnost **ISBN** je dobrou volbou pro klíč entity, protože mezinárodní číslo knihy (ISBN) jednoznačně identifikuje knihu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="2e16d-597">Následující příklad ukazuje typ entity (**Autor**), který má klíč entity, který se skládá ze dvou vlastností, **názvů** a **adres**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="2e16d-598">Použití **názvu** a **adresy** pro klíč entity je rozumnou volbou, protože dva autoři se stejným názvem pravděpodobně nebudou v provozu na stejné adrese.</span><span class="sxs-lookup"><span data-stu-id="2e16d-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="2e16d-599">Tato volba pro klíč entity ale v sadě entit naprosto nezaručuje jedinečné klíče entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="2e16d-600">V tomto případě se doporučuje přidat vlastnost, jako je například **AuthorID**, která se dá použít k jednoznačné identifikaci autora.</span><span class="sxs-lookup"><span data-stu-id="2e16d-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="2e16d-601">Element member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-601">Member Element (CSDL)</span></span>

<span data-ttu-id="2e16d-602">**Členský** prvek je podřízeným prvkem prvku enumType a definuje člena výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-603">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-603">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-604">Následující tabulka popisuje atributy, které lze použít pro element **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="2e16d-605">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-605">Attribute Name</span></span> | <span data-ttu-id="2e16d-606">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-606">Is Required</span></span> | <span data-ttu-id="2e16d-607">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-608">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-608">**Name**</span></span>       | <span data-ttu-id="2e16d-609">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-609">Yes</span></span>         | <span data-ttu-id="2e16d-610">Název členu</span><span class="sxs-lookup"><span data-stu-id="2e16d-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="2e16d-611">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="2e16d-611">**Value**</span></span>      | <span data-ttu-id="2e16d-612">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-612">No</span></span>          | <span data-ttu-id="2e16d-613">Hodnota člena</span><span class="sxs-lookup"><span data-stu-id="2e16d-613">The value of the member.</span></span> <span data-ttu-id="2e16d-614">Ve výchozím nastavení má první člen hodnotu 0 a hodnota každého úspěšného enumerátoru se zvýší o 1.</span><span class="sxs-lookup"><span data-stu-id="2e16d-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="2e16d-615">Může existovat více členů se stejnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2e16d-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-616">Pro element **FunctionImport** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="2e16d-617">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-618">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-619">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-619">Example</span></span>

<span data-ttu-id="2e16d-620">Následující příklad ukazuje element **enumType** se třemi prvky **členů** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="2e16d-621">NavigationProperty – Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="2e16d-622">Element **NavigationProperty** definuje vlastnost navigace, která poskytuje odkaz na druhý konec přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="2e16d-623">Na rozdíl od vlastností, které jsou definovány pomocí elementu Property, navigační vlastnosti nedefinují tvar a vlastnosti dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="2e16d-624">Poskytují způsob, jak procházet přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="2e16d-625">Všimněte si, že navigační vlastnosti jsou volitelné na obou typech entit na konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="2e16d-626">Definujete-li vlastnost navigace na jednom typu entity na konci přidružení, nemusíte definovat vlastnost navigace na typu entity na druhém konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="2e16d-627">Datový typ vrácený navigační vlastností je určen násobností jeho vzdáleného zakončení přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="2e16d-628">Předpokládejme například, že vlastnost navigace **OrdersNavProp**existuje v typu entity **zákazníka** a prochází přidružení 1:1 mezi **zákazníkem** a **objednávkou**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="2e16d-629">Vzhledem k tomu, že vzdálené zakončení přidružení pro navigační vlastnost má mnoho (\*), je jeho datovým typem kolekce ( **pořadí**).</span><span class="sxs-lookup"><span data-stu-id="2e16d-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="2e16d-630">Podobně platí, že pokud v typu entity **objednávky** existuje navigační vlastnost **CustomerNavProp**, její datový typ by byl **zákazníkem** , protože násobnost vzdáleného elementu end je jedna (1).</span><span class="sxs-lookup"><span data-stu-id="2e16d-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="2e16d-631">Element **NavigationProperty** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-632">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-633">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-634">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-634">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-635">Následující tabulka popisuje atributy, které mohou být aplikovány na element **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="2e16d-636">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-636">Attribute Name</span></span>   | <span data-ttu-id="2e16d-637">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-637">Is Required</span></span> | <span data-ttu-id="2e16d-638">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-639">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-639">**Name**</span></span>         | <span data-ttu-id="2e16d-640">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-640">Yes</span></span>         | <span data-ttu-id="2e16d-641">Název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="2e16d-642">**Vztah**</span><span class="sxs-lookup"><span data-stu-id="2e16d-642">**Relationship**</span></span> | <span data-ttu-id="2e16d-643">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-643">Yes</span></span>         | <span data-ttu-id="2e16d-644">Název asociace, který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="2e16d-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="2e16d-645">**ToRole**</span></span>       | <span data-ttu-id="2e16d-646">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-646">Yes</span></span>         | <span data-ttu-id="2e16d-647">Konec přidružení, na kterém končí navigace.</span><span class="sxs-lookup"><span data-stu-id="2e16d-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="2e16d-648">Hodnota atributu **ToRole** musí být stejná jako hodnota jednoho z atributů **role** definované v jednom z elementů Association (definovaných v elementu AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="2e16d-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="2e16d-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="2e16d-649">**FromRole**</span></span>     | <span data-ttu-id="2e16d-650">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-650">Yes</span></span>         | <span data-ttu-id="2e16d-651">Konec přidružení, ze kterého začíná navigace.</span><span class="sxs-lookup"><span data-stu-id="2e16d-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="2e16d-652">Hodnota atributu **FromRole** musí být stejná jako hodnota jednoho z atributů **role** definované v jednom z elementů Association (definovaných v elementu AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="2e16d-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-653">Pro element **NavigationProperty** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="2e16d-654">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-655">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-656">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-656">Example</span></span>

<span data-ttu-id="2e16d-657">Následující příklad definuje typ entity (**Book**) se dvěma navigačními vlastnostmi (**PublishedBy** a **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="2e16d-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="2e16d-658">IsDeleted – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="2e16d-659">Element **IsDeleted** v koncepčním definičním jazyce (CSDL) definuje chování, které je připojeno k přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="2e16d-660">Je-li atribut **Action** nastaven na hodnotu **Cascade** na jednom konci přidružení, budou související typy entit na druhém konci přidružení odstraněny při odstranění typu entity v prvním elementu end.</span><span class="sxs-lookup"><span data-stu-id="2e16d-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="2e16d-661">Pokud je přidružení mezi dvěma typy entit primárním vztahem klíče k primárnímu klíči, pak se načtený závislý objekt odstraní při odstranění objektu zabezpečení na druhém konci přidružení bez ohledu na specifikaci při **odstranění** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="2e16d-662">Element **IsDeleted** má vliv pouze na chování aplikace za běhu. nemá vliv na chování ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="2e16d-663">Chování definované ve zdroji dat by mělo být stejné jako chování definované v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e16d-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="2e16d-664">Element **IsDeleted** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-665">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-666">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-667">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-667">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-668">Následující tabulka popisuje atributy, které mohou být aplikovány na element **IsDeleted** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="2e16d-669">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-669">Attribute Name</span></span> | <span data-ttu-id="2e16d-670">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-670">Is Required</span></span> | <span data-ttu-id="2e16d-671">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-672">**Akce**</span><span class="sxs-lookup"><span data-stu-id="2e16d-672">**Action**</span></span>     | <span data-ttu-id="2e16d-673">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-673">Yes</span></span>         | <span data-ttu-id="2e16d-674">**Cascade** nebo **none**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-674">**Cascade** or **None**.</span></span> <span data-ttu-id="2e16d-675">V případě, že se **kaskády**odstraní, odstraní se závislé typy entit při odstranění typu objektu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="2e16d-676">Pokud **žádný**není, typy závislých entit nebudou odstraněny při odstranění typu hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-677">Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="2e16d-678">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-679">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-680">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-680">Example</span></span>

<span data-ttu-id="2e16d-681">Následující příklad ukazuje element **Association** , který definuje přidružení **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="2e16d-682">Element **IsDeleted** označuje, že všechny **objednávky** , které se vztahují ke konkrétnímu **zákazníkovi** a které byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="2e16d-683">Parameter – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="2e16d-684">Element **Parameter** v koncepčním definičním jazyce (CSDL) může být podřízeným elementem FunctionImport nebo prvkem funkce.</span><span class="sxs-lookup"><span data-stu-id="2e16d-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="2e16d-685">Element FunctionImport – aplikace</span><span class="sxs-lookup"><span data-stu-id="2e16d-685">FunctionImport Element Application</span></span>

<span data-ttu-id="2e16d-686">Element **parametru** (jako podřízený prvek elementu **FunctionImport** ) slouží k definování vstupních a výstupních parametrů pro importy funkce, které jsou deklarovány v CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="2e16d-687">Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-688">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="2e16d-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="2e16d-689">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-690">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-690">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-691">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="2e16d-692">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-692">Attribute Name</span></span> | <span data-ttu-id="2e16d-693">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-693">Is Required</span></span> | <span data-ttu-id="2e16d-694">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-695">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-695">**Name**</span></span>       | <span data-ttu-id="2e16d-696">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-696">Yes</span></span>         | <span data-ttu-id="2e16d-697">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2e16d-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="2e16d-698">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-698">**Type**</span></span>       | <span data-ttu-id="2e16d-699">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-699">Yes</span></span>         | <span data-ttu-id="2e16d-700">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="2e16d-700">The parameter type.</span></span> <span data-ttu-id="2e16d-701">Hodnota musí být **EDMSimpleType** nebo komplexní typ, který je v rámci oboru modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="2e16d-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-702">**Mode**</span></span>       | <span data-ttu-id="2e16d-703">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-703">No</span></span>          | <span data-ttu-id="2e16d-704">**In**, **out**nebo **InOut** v závislosti na tom, zda je parametr vstupní, výstupní nebo vstupní/výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="2e16d-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="2e16d-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-705">**MaxLength**</span></span>  | <span data-ttu-id="2e16d-706">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-706">No</span></span>          | <span data-ttu-id="2e16d-707">Maximální povolená délka parametru.</span><span class="sxs-lookup"><span data-stu-id="2e16d-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="2e16d-708">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-708">**Precision**</span></span>  | <span data-ttu-id="2e16d-709">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-709">No</span></span>          | <span data-ttu-id="2e16d-710">Přesnost parametru.</span><span class="sxs-lookup"><span data-stu-id="2e16d-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="2e16d-711">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-711">**Scale**</span></span>      | <span data-ttu-id="2e16d-712">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-712">No</span></span>          | <span data-ttu-id="2e16d-713">Měřítko parametru.</span><span class="sxs-lookup"><span data-stu-id="2e16d-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="2e16d-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-714">**SRID**</span></span>       | <span data-ttu-id="2e16d-715">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-715">No</span></span>          | <span data-ttu-id="2e16d-716">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="2e16d-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2e16d-717">Platné pouze pro parametry prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="2e16d-718">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e16d-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-719">Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="2e16d-720">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-721">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="2e16d-722">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-722">Example</span></span>

<span data-ttu-id="2e16d-723">Následující příklad ukazuje element **FunctionImport** s jedním podřízeným elementem **parametru** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="2e16d-724">Funkce přijímá jeden vstupní parametr a vrací kolekci typů entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="2e16d-725">Aplikace prvku funkce</span><span class="sxs-lookup"><span data-stu-id="2e16d-725">Function Element Application</span></span>

<span data-ttu-id="2e16d-726">Element **Parameter** (jako podřízený prvek **funkce** ) definuje parametry pro funkce, které jsou definovány nebo deklarovány v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="2e16d-727">Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-728">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="2e16d-729">CollectionType (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="2e16d-730">Hodnota ReferenceType (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="2e16d-731">RowType (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-732">Pouze jeden z prvků typu **CollectionType**, **hodnota ReferenceType**nebo **RowType** může být podřízeným prvkem elementu **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="2e16d-733">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-734">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích.</span><span class="sxs-lookup"><span data-stu-id="2e16d-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="2e16d-735">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="2e16d-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-736">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-736">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-737">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="2e16d-738">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-738">Attribute Name</span></span>   | <span data-ttu-id="2e16d-739">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-739">Is Required</span></span> | <span data-ttu-id="2e16d-740">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-741">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-741">**Name**</span></span>         | <span data-ttu-id="2e16d-742">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-742">Yes</span></span>         | <span data-ttu-id="2e16d-743">Název parametru</span><span class="sxs-lookup"><span data-stu-id="2e16d-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="2e16d-744">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-744">**Type**</span></span>         | <span data-ttu-id="2e16d-745">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-745">No</span></span>          | <span data-ttu-id="2e16d-746">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="2e16d-746">The parameter type.</span></span> <span data-ttu-id="2e16d-747">Parametr může být libovolný z následujících typů (nebo kolekcí těchto typů):</span><span class="sxs-lookup"><span data-stu-id="2e16d-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="2e16d-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="2e16d-749">entity type</span><span class="sxs-lookup"><span data-stu-id="2e16d-749">entity type</span></span> <br/> <span data-ttu-id="2e16d-750">complex type</span><span class="sxs-lookup"><span data-stu-id="2e16d-750">complex type</span></span> <br/> <span data-ttu-id="2e16d-751">Typ řádku</span><span class="sxs-lookup"><span data-stu-id="2e16d-751">row type</span></span> <br/> <span data-ttu-id="2e16d-752">odkazový typ</span><span class="sxs-lookup"><span data-stu-id="2e16d-752">reference type</span></span>                             |
| <span data-ttu-id="2e16d-753">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="2e16d-753">**Nullable**</span></span>     | <span data-ttu-id="2e16d-754">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-754">No</span></span>          | <span data-ttu-id="2e16d-755">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu **null** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="2e16d-756">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="2e16d-756">**DefaultValue**</span></span> | <span data-ttu-id="2e16d-757">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-757">No</span></span>          | <span data-ttu-id="2e16d-758">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="2e16d-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-759">**MaxLength**</span></span>    | <span data-ttu-id="2e16d-760">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-760">No</span></span>          | <span data-ttu-id="2e16d-761">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="2e16d-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-762">**FixedLength**</span></span>  | <span data-ttu-id="2e16d-763">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-763">No</span></span>          | <span data-ttu-id="2e16d-764">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="2e16d-765">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-765">**Precision**</span></span>    | <span data-ttu-id="2e16d-766">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-766">No</span></span>          | <span data-ttu-id="2e16d-767">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="2e16d-768">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-768">**Scale**</span></span>        | <span data-ttu-id="2e16d-769">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-769">No</span></span>          | <span data-ttu-id="2e16d-770">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="2e16d-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-771">**SRID**</span></span>         | <span data-ttu-id="2e16d-772">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-772">No</span></span>          | <span data-ttu-id="2e16d-773">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="2e16d-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2e16d-774">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="2e16d-775">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e16d-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="2e16d-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-776">**Unicode**</span></span>      | <span data-ttu-id="2e16d-777">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-777">No</span></span>          | <span data-ttu-id="2e16d-778">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="2e16d-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="2e16d-779">**Velké**</span><span class="sxs-lookup"><span data-stu-id="2e16d-779">**Collation**</span></span>    | <span data-ttu-id="2e16d-780">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-780">No</span></span>          | <span data-ttu-id="2e16d-781">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="2e16d-782">Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="2e16d-783">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-784">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="2e16d-785">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-785">Example</span></span>

<span data-ttu-id="2e16d-786">Následující příklad ukazuje prvek **funkce** , který používá jeden podřízený element **Parameter** k definování parametru funkce.</span><span class="sxs-lookup"><span data-stu-id="2e16d-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="2e16d-787">Element zabezpečení (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="2e16d-788">Element **Principal** v jazyce CSDL (konceptuální schéma Definition Language) je podřízeným prvkem elementu elementu ReferentialConstraint, který definuje hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="2e16d-789">Element **elementu ReferentialConstraint** definuje funkčnost, která je podobná omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="2e16d-790">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="2e16d-791">Odkazovaný typ entity se nazývá *hlavní konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="2e16d-792">Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="2e16d-793">Prvky **PropertyRef** slouží k určení, na které klíče se odkazuje závislý konec.</span><span class="sxs-lookup"><span data-stu-id="2e16d-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="2e16d-794">Element **Principal** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-795">PropertyRef (jeden nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="2e16d-796">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-797">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-797">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-798">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Principal** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="2e16d-799">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-799">Attribute Name</span></span> | <span data-ttu-id="2e16d-800">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-800">Is Required</span></span> | <span data-ttu-id="2e16d-801">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="2e16d-802">**Role**</span><span class="sxs-lookup"><span data-stu-id="2e16d-802">**Role**</span></span>       | <span data-ttu-id="2e16d-803">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-803">Yes</span></span>         | <span data-ttu-id="2e16d-804">Název typu entity na hlavním konci přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-805">U elementu **Principal** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="2e16d-806">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-807">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-808">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-808">Example</span></span>

<span data-ttu-id="2e16d-809">Následující příklad ukazuje element **elementu ReferentialConstraint** , který je součástí definice přidružení **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="2e16d-810">Vlastnost **ID** typu entity **vydavatele** vytvoří hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="2e16d-811">Property – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-811">Property Element (CSDL)</span></span>

<span data-ttu-id="2e16d-812">Element **Property** v jazyce CSDL (koncepční schéma Definition Language) může být podřízeným elementem EntityType, elementem complexType nebo elementu RowType.</span><span class="sxs-lookup"><span data-stu-id="2e16d-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="2e16d-813">EntityType a ComplexType – aplikace elementu</span><span class="sxs-lookup"><span data-stu-id="2e16d-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="2e16d-814">Prvky **vlastností** (jako podřízené prvky **EntityType** nebo **complexType** ) definují tvar a vlastnosti dat, které instance typu entity nebo instance komplexního typu budou obsahovat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="2e16d-815">Vlastnosti v koncepčním modelu jsou analogické k vlastnostem, které jsou definovány ve třídě.</span><span class="sxs-lookup"><span data-stu-id="2e16d-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="2e16d-816">Stejně jako vlastnosti třídy definují tvar třídy a přenášejí informace o objektech, vlastnosti v koncepčním modelu definují tvar typu entity a přenášejí informace o instancích typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="2e16d-817">Element **Property** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-818">Element dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="2e16d-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="2e16d-819">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="2e16d-820">Následující omezující vlastnosti lze použít na prvek **vlastnosti** : **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **COLLATE**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="2e16d-821">Omezující vlastnosti jsou atributy XML, které poskytují informace o tom, jak jsou hodnoty vlastností uloženy v úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-822">Omezující vlastnosti lze použít pouze pro vlastnosti typu **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-823">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-823">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-824">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="2e16d-825">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-825">Attribute Name</span></span>                                                         | <span data-ttu-id="2e16d-826">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-826">Is Required</span></span> | <span data-ttu-id="2e16d-827">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-828">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-828">**Name**</span></span>                                                               | <span data-ttu-id="2e16d-829">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-829">Yes</span></span>         | <span data-ttu-id="2e16d-830">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="2e16d-831">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-831">**Type**</span></span>                                                               | <span data-ttu-id="2e16d-832">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-832">Yes</span></span>         | <span data-ttu-id="2e16d-833">Typ hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-833">The type of the property value.</span></span> <span data-ttu-id="2e16d-834">Typ hodnoty vlastnosti musí být **EDMSimpleType** nebo komplexní typ (určený plně kvalifikovaným názvem), který je v rozsahu modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="2e16d-835">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="2e16d-835">**Nullable**</span></span>                                                           | <span data-ttu-id="2e16d-836">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-836">No</span></span>          | <span data-ttu-id="2e16d-837">**True** (výchozí hodnota) nebo <strong>false</strong> v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2e16d-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="2e16d-838">> V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="2e16d-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="2e16d-839">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="2e16d-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="2e16d-840">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-840">No</span></span>          | <span data-ttu-id="2e16d-841">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="2e16d-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="2e16d-843">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-843">No</span></span>          | <span data-ttu-id="2e16d-844">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="2e16d-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="2e16d-846">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-846">No</span></span>          | <span data-ttu-id="2e16d-847">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="2e16d-848">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-848">**Precision**</span></span>                                                          | <span data-ttu-id="2e16d-849">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-849">No</span></span>          | <span data-ttu-id="2e16d-850">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="2e16d-851">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-851">**Scale**</span></span>                                                              | <span data-ttu-id="2e16d-852">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-852">No</span></span>          | <span data-ttu-id="2e16d-853">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="2e16d-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-854">**SRID**</span></span>                                                               | <span data-ttu-id="2e16d-855">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-855">No</span></span>          | <span data-ttu-id="2e16d-856">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="2e16d-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2e16d-857">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="2e16d-858">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e16d-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="2e16d-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-859">**Unicode**</span></span>                                                            | <span data-ttu-id="2e16d-860">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-860">No</span></span>          | <span data-ttu-id="2e16d-861">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="2e16d-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="2e16d-862">**Velké**</span><span class="sxs-lookup"><span data-stu-id="2e16d-862">**Collation**</span></span>                                                          | <span data-ttu-id="2e16d-863">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-863">No</span></span>          | <span data-ttu-id="2e16d-864">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="2e16d-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="2e16d-866">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-866">No</span></span>          | <span data-ttu-id="2e16d-867">**Žádná** (výchozí hodnota) nebo **pevná**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="2e16d-868">Pokud je hodnota nastavena na **pevná**, bude hodnota vlastnosti použita při kontrole optimistického řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="2e16d-869">Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="2e16d-870">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-871">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="2e16d-872">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-872">Example</span></span>

<span data-ttu-id="2e16d-873">Následující příklad ukazuje element **EntityType** se třemi prvky **vlastností** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="2e16d-874">Následující příklad ukazuje element **complexType** s pěti prvky **vlastností** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="2e16d-875">Aplikace prvku RowType</span><span class="sxs-lookup"><span data-stu-id="2e16d-875">RowType Element Application</span></span>

<span data-ttu-id="2e16d-876">Prvky **vlastností** (jako podřízené objekty elementu **RowType** ) definují tvar a charakteristiky dat, která lze předat nebo vrátit z modelu definované funkce.</span><span class="sxs-lookup"><span data-stu-id="2e16d-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="2e16d-877">Element **Property** může mít přesně jeden z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="2e16d-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="2e16d-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="2e16d-878">CollectionType</span></span>
-   <span data-ttu-id="2e16d-879">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="2e16d-879">ReferenceType</span></span>
-   <span data-ttu-id="2e16d-880">RowType</span><span class="sxs-lookup"><span data-stu-id="2e16d-880">RowType</span></span>

<span data-ttu-id="2e16d-881">Element **Property** může mít libovolný počet podřízených elementů poznámky.</span><span class="sxs-lookup"><span data-stu-id="2e16d-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-882">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="2e16d-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-883">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-883">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-884">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="2e16d-885">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-885">Attribute Name</span></span>                                                     | <span data-ttu-id="2e16d-886">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-886">Is Required</span></span> | <span data-ttu-id="2e16d-887">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-888">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-888">**Name**</span></span>                                                           | <span data-ttu-id="2e16d-889">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-889">Yes</span></span>         | <span data-ttu-id="2e16d-890">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="2e16d-891">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-891">**Type**</span></span>                                                           | <span data-ttu-id="2e16d-892">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-892">Yes</span></span>         | <span data-ttu-id="2e16d-893">Typ hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="2e16d-894">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="2e16d-894">**Nullable**</span></span>                                                       | <span data-ttu-id="2e16d-895">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-895">No</span></span>          | <span data-ttu-id="2e16d-896">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2e16d-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="2e16d-897">> V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="2e16d-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="2e16d-898">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="2e16d-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="2e16d-899">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-899">No</span></span>          | <span data-ttu-id="2e16d-900">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="2e16d-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="2e16d-902">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-902">No</span></span>          | <span data-ttu-id="2e16d-903">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="2e16d-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="2e16d-905">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-905">No</span></span>          | <span data-ttu-id="2e16d-906">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="2e16d-907">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-907">**Precision**</span></span>                                                      | <span data-ttu-id="2e16d-908">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-908">No</span></span>          | <span data-ttu-id="2e16d-909">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="2e16d-910">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-910">**Scale**</span></span>                                                          | <span data-ttu-id="2e16d-911">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-911">No</span></span>          | <span data-ttu-id="2e16d-912">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="2e16d-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-913">**SRID**</span></span>                                                           | <span data-ttu-id="2e16d-914">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-914">No</span></span>          | <span data-ttu-id="2e16d-915">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="2e16d-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2e16d-916">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="2e16d-917">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e16d-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="2e16d-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-918">**Unicode**</span></span>                                                        | <span data-ttu-id="2e16d-919">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-919">No</span></span>          | <span data-ttu-id="2e16d-920">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="2e16d-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="2e16d-921">**Velké**</span><span class="sxs-lookup"><span data-stu-id="2e16d-921">**Collation**</span></span>                                                      | <span data-ttu-id="2e16d-922">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-922">No</span></span>          | <span data-ttu-id="2e16d-923">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="2e16d-924">Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="2e16d-925">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-926">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="2e16d-927">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-927">Example</span></span>

<span data-ttu-id="2e16d-928">Následující příklad ukazuje prvky **vlastností** používané k definování tvaru návratového typu funkce definované modelem.</span><span class="sxs-lookup"><span data-stu-id="2e16d-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="2e16d-929">PropertyRef – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="2e16d-930">Element **PropertyRef** v jazyce CSDL (konceptuální schéma Definition Language) odkazuje na vlastnost typu entity, která označuje, že vlastnost provede jednu z následujících rolí:</span><span class="sxs-lookup"><span data-stu-id="2e16d-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="2e16d-931">Část klíče entity (vlastnost nebo sada vlastností typu entity, která určuje identitu).</span><span class="sxs-lookup"><span data-stu-id="2e16d-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="2e16d-932">Jeden nebo více elementů **PropertyRef** lze použít k definování klíče entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="2e16d-933">Závislý nebo hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="2e16d-934">Element **PropertyRef** může mít jako podřízené elementy pouze prvky poznámky (nula nebo více).</span><span class="sxs-lookup"><span data-stu-id="2e16d-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-935">Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.</span><span class="sxs-lookup"><span data-stu-id="2e16d-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-936">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-936">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-937">Následující tabulka popisuje atributy, které mohou být aplikovány na element **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="2e16d-938">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-938">Attribute Name</span></span> | <span data-ttu-id="2e16d-939">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-939">Is Required</span></span> | <span data-ttu-id="2e16d-940">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="2e16d-941">**Název**</span><span class="sxs-lookup"><span data-stu-id="2e16d-941">**Name**</span></span>       | <span data-ttu-id="2e16d-942">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-942">Yes</span></span>         | <span data-ttu-id="2e16d-943">Název odkazované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-944">Pro element **PropertyRef** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="2e16d-945">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-946">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-947">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-947">Example</span></span>

<span data-ttu-id="2e16d-948">Následující příklad definuje typ entity (**Book**).</span><span class="sxs-lookup"><span data-stu-id="2e16d-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="2e16d-949">Klíč entity je definován pomocí odkazu na vlastnost **ISBN** typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="2e16d-950">V následujícím příkladu jsou použity dva prvky **PropertyRef** k označení toho, že dvě vlastnosti (**ID** a **PublisherId**) jsou objekty zabezpečení a závislé na konci referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="2e16d-951">Hodnota ReferenceType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="2e16d-952">Element **hodnota ReferenceType** v jazyce CSDL (konceptuální schéma Definition Language) určuje odkaz na typ entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="2e16d-953">Element **hodnota ReferenceType** může být podřízeným prvkem následujících prvků:</span><span class="sxs-lookup"><span data-stu-id="2e16d-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="2e16d-954">ReturnType (Function)</span><span class="sxs-lookup"><span data-stu-id="2e16d-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="2e16d-955">Parametr</span><span class="sxs-lookup"><span data-stu-id="2e16d-955">Parameter</span></span>
-   <span data-ttu-id="2e16d-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="2e16d-956">CollectionType</span></span>

<span data-ttu-id="2e16d-957">Element **hodnota ReferenceType** se používá při definování parametru nebo návratového typu pro funkci.</span><span class="sxs-lookup"><span data-stu-id="2e16d-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="2e16d-958">Element **hodnota ReferenceType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-959">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-960">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-961">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-961">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-962">Následující tabulka popisuje atributy, které mohou být aplikovány na element **hodnota ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="2e16d-963">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-963">Attribute Name</span></span> | <span data-ttu-id="2e16d-964">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-964">Is Required</span></span> | <span data-ttu-id="2e16d-965">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="2e16d-966">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-966">**Type**</span></span>       | <span data-ttu-id="2e16d-967">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-967">Yes</span></span>         | <span data-ttu-id="2e16d-968">Název odkazovaného typu entity</span><span class="sxs-lookup"><span data-stu-id="2e16d-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-969">Pro element **hodnota ReferenceType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="2e16d-970">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-971">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-972">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-972">Example</span></span>

<span data-ttu-id="2e16d-973">Následující příklad ukazuje element **hodnota ReferenceType** , který se používá jako podřízený prvek **parametru** v rámci funkce definované modelem, která přijímá odkaz na typ entity **Person** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="2e16d-974">Následující příklad ukazuje element **hodnota ReferenceType** , který se používá jako podřízený prvek **ReturnType** (Function) ve funkci definovaného modelu, která vrací odkaz na typ entity **Person** :</span><span class="sxs-lookup"><span data-stu-id="2e16d-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="2e16d-975">Elementu ReferentialConstraint – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="2e16d-976">Element **elementu ReferentialConstraint** v jazyce CSDL (konceptuální schéma Definition Language) definuje funkce, které jsou podobné omezení referenční integrity v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="2e16d-977">Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="2e16d-978">Odkazovaný typ entity se nazývá *hlavní konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="2e16d-979">Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="2e16d-980">Pokud cizí klíč, který je vystaven na jednom typu entity, odkazuje na vlastnost v jiném typu entity, element **elementu ReferentialConstraint** definuje přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="2e16d-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="2e16d-981">Vzhledem k tomu, že element **elementu ReferentialConstraint** poskytuje informace o tom, jak se vztahují dva typy entit, není v jazyce MSL (Mapping Specification Language) nutný žádný odpovídající element AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="2e16d-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="2e16d-982">Přidružení mezi dvěma typy entit, které nemají vystavené cizími klíči, musí mít odpovídající element **AssociationSetMapping** , aby bylo možné mapovat informace o přidružení ke zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="2e16d-983">Pokud cizí klíč není vystavený pro typ entity, element **elementu ReferentialConstraint** může definovat jenom primární omezení klíče na primární klíč mezi typem entity a jiným typem entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="2e16d-984">Element **elementu ReferentialConstraint** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-985">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-986">Objekt zabezpečení (právě jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="2e16d-987">Závislý (právě jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="2e16d-988">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-989">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-989">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-990">Element **elementu ReferentialConstraint** může mít libovolný počet atributů anotace (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="2e16d-991">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-992">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-993">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-993">Example</span></span>

<span data-ttu-id="2e16d-994">Následující příklad ukazuje element **elementu ReferentialConstraint** , který se používá jako součást definice asociace **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="2e16d-995">ReturnType – element (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="2e16d-996">Element **ReturnType** (Function) v jazyce CSDL (konceptuální schéma Definition Language) určuje návratový typ pro funkci, která je definována v elementu Function.</span><span class="sxs-lookup"><span data-stu-id="2e16d-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="2e16d-997">Návratový typ funkce lze také zadat s atributem **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="2e16d-998">Návratové typy mohou být libovolné **EdmSimpleType**, typ entity, komplexní typ, typ řádku, typ odkazu nebo kolekce jednoho z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="2e16d-999">Návratový typ funkce lze zadat buď pomocí atributu **Type** elementu **ReturnType** (Function), nebo s jedním z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="2e16d-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="2e16d-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="2e16d-1000">CollectionType</span></span>
-   <span data-ttu-id="2e16d-1001">Hodnota ReferenceType</span><span class="sxs-lookup"><span data-stu-id="2e16d-1001">ReferenceType</span></span>
-   <span data-ttu-id="2e16d-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="2e16d-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-1003">Model nebude ověřen, pokud zadáte návratový typ funkce s atributem **Type** elementu **ReturnType** (Function) a jedním z podřízených elementů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-1004">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-1004">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-1005">Následující tabulka popisuje atributy, které mohou být aplikovány na element **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="2e16d-1006">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1006">Attribute Name</span></span> | <span data-ttu-id="2e16d-1007">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-1007">Is Required</span></span> | <span data-ttu-id="2e16d-1008">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="2e16d-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1009">**ReturnType**</span></span> | <span data-ttu-id="2e16d-1010">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1010">No</span></span>          | <span data-ttu-id="2e16d-1011">Typ vrácený funkcí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-1012">Pro element **ReturnType** (Function) lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="2e16d-1013">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1014">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-1015">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1015">Example</span></span>

<span data-ttu-id="2e16d-1016">Následující příklad používá element **Function** k definování funkce, která vrátí počet roků, po který se kniha tiskne.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="2e16d-1017">Všimněte si, že návratový typ je určen atributem **typu** elementu **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="2e16d-1018">ReturnType (FunctionImport) – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="2e16d-1019">Element **ReturnType** (FunctionImport) v jazyce CSDL (konceptuální schéma Definition Language) určuje návratový typ pro funkci, která je definována v elementu FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="2e16d-1020">Návratový typ funkce lze také zadat s atributem **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="2e16d-1021">Návratové typy mohou být libovolné kolekce typu entity, komplexního typu nebo **EdmSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="2e16d-1022">Návratový typ funkce je zadán s atributem **Type** elementu **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-1023">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-1023">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-1024">Následující tabulka popisuje atributy, které lze použít na element **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="2e16d-1025">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1025">Attribute Name</span></span> | <span data-ttu-id="2e16d-1026">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-1026">Is Required</span></span> | <span data-ttu-id="2e16d-1027">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-1028">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1028">**Type**</span></span>       | <span data-ttu-id="2e16d-1029">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1029">No</span></span>          | <span data-ttu-id="2e16d-1030">Typ, který funkce vrátí.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1030">The type that the function returns.</span></span> <span data-ttu-id="2e16d-1031">Hodnotou musí být kolekce ComplexType, EntityType nebo EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="2e16d-1032">**Sada**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1032">**EntitySet**</span></span>  | <span data-ttu-id="2e16d-1033">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1033">No</span></span>          | <span data-ttu-id="2e16d-1034">Pokud funkce vrátí kolekci typů entit, hodnota **objektu EntitySet** musí být sada entit, do které kolekce patří.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="2e16d-1035">V opačném případě nesmí být použit atribut **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-1036">Pro element **ReturnType** (FunctionImport) lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="2e16d-1037">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1038">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-1039">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1039">Example</span></span>

<span data-ttu-id="2e16d-1040">V následujícím příkladu je použita funkce **FunctionImport** , která vrací knihy a vydavatele.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="2e16d-1041">Všimněte si, že funkce vrací dvě sady výsledků, a proto jsou zadány dva prvky **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="2e16d-1042">RowType – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="2e16d-1043">Element **RowType** v koncepčním definičním jazyce (CSDL) definuje nepojmenované struktury jako parametr nebo návratový typ pro funkci definovanou v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="2e16d-1044">Element **RowType** může být podřízeným prvkem následujících prvků:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="2e16d-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="2e16d-1045">CollectionType</span></span>
-   <span data-ttu-id="2e16d-1046">Parametr</span><span class="sxs-lookup"><span data-stu-id="2e16d-1046">Parameter</span></span>
-   <span data-ttu-id="2e16d-1047">ReturnType (Function)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1047">ReturnType (Function)</span></span>

<span data-ttu-id="2e16d-1048">Element **RowType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-1049">Vlastnost (jedna nebo více)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1049">Property (one or more)</span></span>
-   <span data-ttu-id="2e16d-1050">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-1051">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-1051">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-1052">Pro element **RowType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="2e16d-1053">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1054">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-1055">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1055">Example</span></span>

<span data-ttu-id="2e16d-1056">Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="2e16d-1057">Schema – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="2e16d-1058">Element **Schema** je kořenovým prvkem definice koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="2e16d-1059">Obsahuje definice pro objekty, funkce a kontejnery, které tvoří koncepční model.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="2e16d-1060">Element **Schema** může obsahovat nula nebo více z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="2e16d-1061">Použití</span><span class="sxs-lookup"><span data-stu-id="2e16d-1061">Using</span></span>
-   <span data-ttu-id="2e16d-1062">Kontejneru</span><span class="sxs-lookup"><span data-stu-id="2e16d-1062">EntityContainer</span></span>
-   <span data-ttu-id="2e16d-1063">Typ entity</span><span class="sxs-lookup"><span data-stu-id="2e16d-1063">EntityType</span></span>
-   <span data-ttu-id="2e16d-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="2e16d-1064">EnumType</span></span>
-   <span data-ttu-id="2e16d-1065">Řídí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1065">Association</span></span>
-   <span data-ttu-id="2e16d-1066">Elementu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1066">ComplexType</span></span>
-   <span data-ttu-id="2e16d-1067">Funkce</span><span class="sxs-lookup"><span data-stu-id="2e16d-1067">Function</span></span>

<span data-ttu-id="2e16d-1068">Prvek **schématu** může obsahovat nula nebo jeden prvek poznámky.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-1069">Element **funkce** a prvky poznámky jsou povoleny pouze v CSDL v2 a novějším.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="2e16d-1070">Element **Schema** používá atribut **Namespace** k definování oboru názvů pro typ entity, komplexní typ a objekty přidružení v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="2e16d-1071">V rámci oboru názvů nemohou mít žádné dva objekty stejný název.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="2e16d-1072">Obory názvů mohou zahrnovat více prvků **schématu** a více souborů. csdl.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="2e16d-1073">Obor názvů koncepčního modelu je jiný než obor názvů XML elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="2e16d-1074">Obor názvů koncepčního modelu (jak je definováno atributem **Namespace** ) je logický kontejner pro typy entit, komplexní typy a typy přidružení.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="2e16d-1075">Obor názvů XML (uvedený atributem **xmlns** ) elementu **Schema** je výchozí obor názvů pro podřízené elementy a atributy elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="2e16d-1076">Obory názvů XML ve formátu https://schemas.microsoft.com/ado/YYYY/MM/edm (kde RRRR a MM představují rok a měsíc) jsou vyhrazené pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1077">Vlastní elementy a atributy nemůžou být v oborech názvů, které mají tento formulář.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-1078">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-1078">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-1079">Následující tabulka popisuje atributy, které lze použít pro element **Schema** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="2e16d-1080">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1080">Attribute Name</span></span> | <span data-ttu-id="2e16d-1081">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-1081">Is Required</span></span> | <span data-ttu-id="2e16d-1082">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-1083">**Hosting**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1083">**Namespace**</span></span>  | <span data-ttu-id="2e16d-1084">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1084">Yes</span></span>         | <span data-ttu-id="2e16d-1085">Obor názvů koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="2e16d-1086">Hodnota atributu **Namespace** slouží k vytvoření plně kvalifikovaného názvu typu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="2e16d-1087">Pokud je například **EntityType** s názvem *Zákazník* v jednoduchém oboru názvů. example. model, pak plně kvalifikovaný název **objektu EntityType** je SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="2e16d-1088">Následující řetězce nelze použít jako hodnotu pro atribut **Namespace** : **System**, **přechodný**nebo **EDM**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="2e16d-1089">Hodnota atributu **Namespace** nemůže být stejná jako hodnota atributu **Namespace** v elementu schématu SSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="2e16d-1090">**Zástupný**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1090">**Alias**</span></span>      | <span data-ttu-id="2e16d-1091">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1091">No</span></span>          | <span data-ttu-id="2e16d-1092">Identifikátor použitý místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="2e16d-1093">Pokud je například **EntityType** s názvem *Zákazník* v jednoduchém oboru názvů. example. model a hodnota atributu **alias** je *model*, pak můžete použít model. Customer jako plně kvalifikovaný název **objektu EntityType.**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="2e16d-1094">Pro element **Schema** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="2e16d-1095">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1096">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-1097">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1097">Example</span></span>

<span data-ttu-id="2e16d-1098">Následující příklad ukazuje element **schématu** , který obsahuje element **EntityContainer** , dva elementy **EntityType** a jeden element **Association** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="2e16d-1099">TypeRef – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="2e16d-1100">Element **TypeRef** v jazyce CSDL (konceptuální schéma Definition Language) poskytuje odkaz na existující pojmenovaný typ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="2e16d-1101">Element **TypeRef** může být podřízeným elementem CollectionType, který se používá k určení toho, že funkce má kolekci jako parametr nebo návratový typ.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="2e16d-1102">Element **TypeRef** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="2e16d-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2e16d-1103">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2e16d-1104">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-1105">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-1105">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-1106">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="2e16d-1107">Všimněte si, že atributy **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**a **kolace** se vztahují pouze na **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="2e16d-1108">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="2e16d-1109">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-1109">Is Required</span></span> | <span data-ttu-id="2e16d-1110">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-1111">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1111">**Type**</span></span>                                                           | <span data-ttu-id="2e16d-1112">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1112">No</span></span>          | <span data-ttu-id="2e16d-1113">Název odkazovaného typu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="2e16d-1114">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="2e16d-1115">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1115">No</span></span>          | <span data-ttu-id="2e16d-1116">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="2e16d-1117">> V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="2e16d-1118">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="2e16d-1119">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1119">No</span></span>          | <span data-ttu-id="2e16d-1120">Výchozí hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="2e16d-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="2e16d-1122">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1122">No</span></span>          | <span data-ttu-id="2e16d-1123">Maximální délka hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="2e16d-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="2e16d-1125">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1125">No</span></span>          | <span data-ttu-id="2e16d-1126">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="2e16d-1127">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1127">**Precision**</span></span>                                                      | <span data-ttu-id="2e16d-1128">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1128">No</span></span>          | <span data-ttu-id="2e16d-1129">Přesnost hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="2e16d-1130">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1130">**Scale**</span></span>                                                          | <span data-ttu-id="2e16d-1131">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1131">No</span></span>          | <span data-ttu-id="2e16d-1132">Měřítko hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="2e16d-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1133">**SRID**</span></span>                                                           | <span data-ttu-id="2e16d-1134">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1134">No</span></span>          | <span data-ttu-id="2e16d-1135">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="2e16d-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2e16d-1136">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="2e16d-1137">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="2e16d-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="2e16d-1139">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1139">No</span></span>          | <span data-ttu-id="2e16d-1140">**True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="2e16d-1141">**Velké**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1141">**Collation**</span></span>                                                      | <span data-ttu-id="2e16d-1142">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1142">No</span></span>          | <span data-ttu-id="2e16d-1143">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="2e16d-1144">Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="2e16d-1145">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1146">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-1147">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1147">Example</span></span>

<span data-ttu-id="2e16d-1148">Následující příklad ukazuje funkci definovanou modelem, která používá prvek **TypeRef** (jako podřízený prvek typu **CollectionType** ) k určení, že funkce přijímá kolekci typů entit **oddělení** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="2e16d-1149">Using – element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="2e16d-1150">Element **using** v jazyce CSDL (konceptuální schéma Definition Language) importuje obsah koncepčního modelu, který existuje v jiném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="2e16d-1151">Nastavením hodnoty atributu **Namespace** můžete odkazovat na typy entit, komplexní typy a typy přidružení, které jsou definovány v jiném koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="2e16d-1152">Více než jeden element **using** může být podřízeným elementu **schématu** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-1153">Element **using** v CSDL nefunguje úplně stejně jako příkaz **using** v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="2e16d-1154">Importováním oboru názvů pomocí příkazu **using** v programovacím jazyce neovlivníte objekty v původním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="2e16d-1155">V CSDL může importovaný obor názvů obsahovat typ entity, který je odvozen z typu entity v původním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="2e16d-1156">To může mít vliv na sady entit deklarované v původním oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="2e16d-1157">Element **using** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="2e16d-1158">Dokumentace (povolené nula nebo jeden prvek)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="2e16d-1159">Prvky poznámky (povoleny nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2e16d-1160">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="2e16d-1160">Applicable Attributes</span></span>

<span data-ttu-id="2e16d-1161">Následující tabulka popisuje atributy, které lze použít pro element **using** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="2e16d-1162">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2e16d-1162">Attribute Name</span></span> | <span data-ttu-id="2e16d-1163">Je povinné</span><span class="sxs-lookup"><span data-stu-id="2e16d-1163">Is Required</span></span> | <span data-ttu-id="2e16d-1164">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2e16d-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-1165">**Hosting**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1165">**Namespace**</span></span>  | <span data-ttu-id="2e16d-1166">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1166">Yes</span></span>         | <span data-ttu-id="2e16d-1167">Název importovaného oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="2e16d-1168">**Zástupný**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1168">**Alias**</span></span>      | <span data-ttu-id="2e16d-1169">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1169">Yes</span></span>         | <span data-ttu-id="2e16d-1170">Identifikátor použitý místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="2e16d-1171">I když je tento atribut vyžadován, není vyžadováno, aby byl použit místo názvu oboru názvů k získání názvů objektů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="2e16d-1172">Pro element **using** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="2e16d-1173">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2e16d-1174">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="2e16d-1175">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1175">Example</span></span>

<span data-ttu-id="2e16d-1176">Následující příklad ukazuje **použití** prvku, který je použit pro import oboru názvů, který je definován jinde.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="2e16d-1177">Všimněte si, že obor názvů pro zobrazený element **schématu** je `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="2e16d-1178">Vlastnost `Address` na `Publisher`**EntityType** je komplexní typ, který je definován v oboru názvů `ExtendedBooksModel` (importované pomocí elementu **using** ).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="2e16d-1179">Atributy poznámek (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="2e16d-1180">Atributy poznámek v nástroji CSDL (konceptuální schéma Definition Language) jsou vlastní atributy XML v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="2e16d-1181">Kromě toho, že mají platnou strukturu XML, musí mít následující hodnoty true atributů Anotace:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="2e16d-1182">Atributy poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="2e16d-1183">U daného elementu CSDL lze použít více než jeden atribut poznámky.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="2e16d-1184">Plně kvalifikované názvy všech dvou atributů poznámek nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="2e16d-1185">Atributy poznámek lze použít k poskytnutí dalších metadat o prvcích v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="2e16d-1186">K metadatům obsaženým v prvcích poznámky lze za běhu přistup použít třídy v oboru názvů System. data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-1187">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1187">Example</span></span>

<span data-ttu-id="2e16d-1188">Následující příklad ukazuje element **EntityType** s atributem poznámky (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="2e16d-1189">Příklad také ukazuje prvek poznámky aplikovaný na prvek typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="2e16d-1190">Následující kód načte metadata v atributu anotace a zapíše je do konzoly:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="2e16d-1191">Výše uvedený kód předpokládá, že soubor `School.csdl` je ve výstupním adresáři projektu a že jste přidali následující `Imports` a `Using` příkazy do projektu:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="2e16d-1192">Prvky poznámek (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="2e16d-1193">Prvky poznámek v jazyce CSDL (konceptuální schéma Definition Language) jsou vlastní prvky XML v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="2e16d-1194">Kromě toho, že mají platnou strukturu XML, musí mít následující hodnoty true pro prvky poznámky:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="2e16d-1195">Prvky poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="2e16d-1196">Více než jeden element poznámky může být podřízeným prvku daného elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="2e16d-1197">Plně kvalifikované názvy všech dvou prvků poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="2e16d-1198">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích daného elementu CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="2e16d-1199">Prvky poznámky lze použít k poskytnutí dalších metadat o prvcích v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="2e16d-1200">Počínaje verzí .NET Framework 4 je k metadatům obsaženým v prvcích poznámky možné přistupovat za běhu pomocí tříd v oboru názvů System. data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-1201">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1201">Example</span></span>

<span data-ttu-id="2e16d-1202">Následující příklad ukazuje element **EntityType** s elementem Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="2e16d-1203">Příklad také ukazuje atribut anotace aplikovaný na prvek typu entity.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="2e16d-1204">Následující kód načte metadata v prvku anotace a zapíše jej do konzoly:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="2e16d-1205">Výše uvedený kód předpokládá, že soubor School. csdl je ve výstupním adresáři projektu a že jste přidali následující `Imports` a `Using` příkazů do projektu:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="2e16d-1206">Typy konceptuálních modelů (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="2e16d-1207">Model CSDL (konceptuální schéma Definition Language) podporuje sadu abstraktních primitivních datových typů s názvem **EDMSimpleTypes**, které definují vlastnosti v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="2e16d-1208">**EDMSimpleTypes** jsou proxy pro primitivní datové typy, které jsou podporovány v úložišti nebo hostitelském prostředí.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="2e16d-1209">Následující tabulka uvádí primitivní datové typy, které podporuje CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="2e16d-1210">Tabulka obsahuje také seznam omezujících vlastností, které lze použít na jednotlivé **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="2e16d-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="2e16d-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="2e16d-1212">Popis</span><span class="sxs-lookup"><span data-stu-id="2e16d-1212">Description</span></span>                                                | <span data-ttu-id="2e16d-1213">Použitelné omezující vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2e16d-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="2e16d-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="2e16d-1215">Obsahuje binární data.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="2e16d-1216">MaxLength, FixedLength, Nullable, default</span><span class="sxs-lookup"><span data-stu-id="2e16d-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="2e16d-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="2e16d-1218">Obsahuje hodnotu **true** nebo **false**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="2e16d-1219">Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="2e16d-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="2e16d-1221">Obsahuje 8bitové celočíselnou hodnotu bez znaménka.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="2e16d-1222">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="2e16d-1224">Představuje datum a čas.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="2e16d-1225">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="2e16d-1227">Obsahuje datum a čas jako posun v minutách od času GMT.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="2e16d-1228">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="2e16d-1230">Obsahuje číselnou hodnotu s pevnou přesností a škálováním.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="2e16d-1231">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="2e16d-1233">Obsahuje číslo s plovoucí desetinnou čárkou s přesností na 15 číslic.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="2e16d-1234">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="2e16d-1236">Obsahuje číslo s plovoucí desetinnou čárkou s přesností na 7 číslic.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="2e16d-1237">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="2e16d-1239">Obsahuje jedinečný identifikátor o velikosti 16 bajtů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="2e16d-1240">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="2e16d-1242">Obsahuje 16bitová celočíselnou hodnotu se znaménkem.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="2e16d-1243">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="2e16d-1245">Obsahuje podepsaná 32 celočíselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="2e16d-1246">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="2e16d-1248">Obsahuje podepsaná 64 celočíselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="2e16d-1249">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="2e16d-1251">Obsahuje 8bitové celočíselnou hodnotu se znaménkem.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="2e16d-1252">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1253">**Edm.String**</span></span>                   | <span data-ttu-id="2e16d-1254">Obsahuje znaková data.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1254">Contains character data.</span></span>                                   | <span data-ttu-id="2e16d-1255">Unicode, FixedLength, MaxLength, kolace, přesnost, Nullable, default</span><span class="sxs-lookup"><span data-stu-id="2e16d-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="2e16d-1256">**EDM. time**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="2e16d-1257">Obsahuje denní dobu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="2e16d-1258">Přesnost, Nullable, výchozí</span><span class="sxs-lookup"><span data-stu-id="2e16d-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="2e16d-1259">**EDM. geografie**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="2e16d-1260">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="2e16d-1262">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="2e16d-1264">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="2e16d-1266">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="2e16d-1268">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="2e16d-1270">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="2e16d-1272">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1273">**EDM. geografie –Collection**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="2e16d-1274">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="2e16d-1276">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="2e16d-1278">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="2e16d-1280">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="2e16d-1282">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="2e16d-1284">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="2e16d-1286">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="2e16d-1288">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="2e16d-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="2e16d-1290">Nullable, Default, SRID</span><span class="sxs-lookup"><span data-stu-id="2e16d-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="2e16d-1291">Omezující vlastnosti (CSDL)</span><span class="sxs-lookup"><span data-stu-id="2e16d-1291">Facets (CSDL)</span></span>

<span data-ttu-id="2e16d-1292">Charakteristiky v jazyce CSDL (konceptuální schéma Definition Language) reprezentují omezení vlastností typů entit a komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="2e16d-1293">Omezující vlastnosti se zobrazí jako atributy XML u následujících elementů CSDL:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="2e16d-1294">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2e16d-1294">Property</span></span>
-   <span data-ttu-id="2e16d-1295">Odkaz</span><span class="sxs-lookup"><span data-stu-id="2e16d-1295">TypeRef</span></span>
-   <span data-ttu-id="2e16d-1296">Parametr</span><span class="sxs-lookup"><span data-stu-id="2e16d-1296">Parameter</span></span>

<span data-ttu-id="2e16d-1297">Následující tabulka obsahuje popis omezujících vlastností, které jsou podporovány v CSDL.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="2e16d-1298">Všechny omezující vlastnosti jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1298">All facets are optional.</span></span> <span data-ttu-id="2e16d-1299">Některé omezující vlastnosti, které jsou uvedeny níže, jsou používány Entity Framework při generování databáze z koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="2e16d-1300">Informace o datových typech v koncepčním modelu naleznete v tématu typy konceptuálních modelů (CSDL).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="2e16d-1301">Omezující</span><span class="sxs-lookup"><span data-stu-id="2e16d-1301">Facet</span></span>               | <span data-ttu-id="2e16d-1302">Popis</span><span class="sxs-lookup"><span data-stu-id="2e16d-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="2e16d-1303">Platí pro</span><span class="sxs-lookup"><span data-stu-id="2e16d-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="2e16d-1304">Používá se pro generování databáze.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1304">Used for the database generation</span></span> | <span data-ttu-id="2e16d-1305">Používáno modulem runtime</span><span class="sxs-lookup"><span data-stu-id="2e16d-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="2e16d-1306">**Velké**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1306">**Collation**</span></span>       | <span data-ttu-id="2e16d-1307">Určuje pořadí kompletování (nebo řazení sekvence), které se má použít při provádění operací porovnání a řazení u hodnot vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="2e16d-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="2e16d-1309">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1309">Yes</span></span>                              | <span data-ttu-id="2e16d-1310">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1310">No</span></span>                  |
| <span data-ttu-id="2e16d-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="2e16d-1312">Označuje, že hodnota vlastnosti by měla být použita pro kontroly optimistického řízení souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="2e16d-1313">Všechny vlastnosti **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="2e16d-1314">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1314">No</span></span>                               | <span data-ttu-id="2e16d-1315">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1315">Yes</span></span>                 |
| <span data-ttu-id="2e16d-1316">**Výchozí**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1316">**Default**</span></span>         | <span data-ttu-id="2e16d-1317">Určuje výchozí hodnotu vlastnosti, pokud není při vytváření instance zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="2e16d-1318">Všechny vlastnosti **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="2e16d-1319">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1319">Yes</span></span>                              | <span data-ttu-id="2e16d-1320">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1320">Yes</span></span>                 |
| <span data-ttu-id="2e16d-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1321">**FixedLength**</span></span>     | <span data-ttu-id="2e16d-1322">Určuje, zda může být délka hodnoty vlastnosti odlišná.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="2e16d-1323">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="2e16d-1324">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1324">Yes</span></span>                              | <span data-ttu-id="2e16d-1325">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1325">No</span></span>                  |
| <span data-ttu-id="2e16d-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1326">**MaxLength**</span></span>       | <span data-ttu-id="2e16d-1327">Určuje maximální délku hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="2e16d-1328">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="2e16d-1329">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1329">Yes</span></span>                              | <span data-ttu-id="2e16d-1330">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1330">No</span></span>                  |
| <span data-ttu-id="2e16d-1331">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1331">**Nullable**</span></span>        | <span data-ttu-id="2e16d-1332">Určuje, zda vlastnost může mít hodnotu **null** .</span><span class="sxs-lookup"><span data-stu-id="2e16d-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="2e16d-1333">Všechny vlastnosti **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="2e16d-1334">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1334">Yes</span></span>                              | <span data-ttu-id="2e16d-1335">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1335">Yes</span></span>                 |
| <span data-ttu-id="2e16d-1336">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1336">**Precision**</span></span>       | <span data-ttu-id="2e16d-1337">Pro vlastnosti typu **Decimal**určuje počet číslic, které může mít hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="2e16d-1338">Pro vlastnosti typu **Time**, **DateTime**a **DateTimeOffset**určuje počet číslic pro zlomkovou část hodnoty vlastnosti v sekundách.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="2e16d-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **EDM. time**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="2e16d-1340">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1340">Yes</span></span>                              | <span data-ttu-id="2e16d-1341">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1341">No</span></span>                  |
| <span data-ttu-id="2e16d-1342">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1342">**Scale**</span></span>           | <span data-ttu-id="2e16d-1343">Určuje počet číslic vpravo od desetinné čárky pro hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="2e16d-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="2e16d-1345">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1345">Yes</span></span>                              | <span data-ttu-id="2e16d-1346">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1346">No</span></span>                  |
| <span data-ttu-id="2e16d-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1347">**SRID**</span></span>            | <span data-ttu-id="2e16d-1348">Určuje ID referenčního systému prostorového systému.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="2e16d-1349">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e16d-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="2e16d-1350">**EDM. geografie, Edm. GeographyPoint, Edm. GeographyLineString, Edm. GeographyPolygon, Edm. GeographyMultiPoint, Edm. GeographyMultiLineString, Edm. GeographyMultiPolygon, Edm. geografie, Edm. Geometry, Edm. GeometryPoint, EDM. GeometryLineString, Edm. GeometryPolygon, Edm. GeometryMultiPoint, Edm. GeometryMultiLineString, Edm. GeometryMultiPolygon, Edm. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="2e16d-1351">Ne</span><span class="sxs-lookup"><span data-stu-id="2e16d-1351">No</span></span>                               | <span data-ttu-id="2e16d-1352">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1352">Yes</span></span>                 |
| <span data-ttu-id="2e16d-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1353">**Unicode**</span></span>         | <span data-ttu-id="2e16d-1354">Určuje, zda je hodnota vlastnosti uložena jako Unicode.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="2e16d-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="2e16d-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="2e16d-1356">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1356">Yes</span></span>                              | <span data-ttu-id="2e16d-1357">Ano</span><span class="sxs-lookup"><span data-stu-id="2e16d-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="2e16d-1358">Při generování databáze z koncepčního modelu Průvodce generováním databáze rozpozná hodnotu atributu **StoreGeneratedPattern** elementu **Property** , pokud je v následujícím oboru názvů: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="2e16d-1359">Podporované hodnoty atributu jsou **identity** a **počítané**.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="2e16d-1360">Hodnota **identity** vytvoří sloupec databáze s hodnotou identity, která je vygenerována v databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="2e16d-1361">Hodnota **počítaná** vygeneruje sloupec s hodnotou, která je vypočítána v databázi.</span><span class="sxs-lookup"><span data-stu-id="2e16d-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="2e16d-1362">Příklad</span><span class="sxs-lookup"><span data-stu-id="2e16d-1362">Example</span></span>

<span data-ttu-id="2e16d-1363">Následující příklad ukazuje charakteristiky použité pro vlastnosti typu entity:</span><span class="sxs-lookup"><span data-stu-id="2e16d-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
