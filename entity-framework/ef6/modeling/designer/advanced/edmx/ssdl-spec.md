---
title: SSDL – specifikace – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418723"
---
# <a name="ssdl-specification"></a><span data-ttu-id="69809-102">Specifikace SSDL</span><span class="sxs-lookup"><span data-stu-id="69809-102">SSDL Specification</span></span>
<span data-ttu-id="69809-103">Soubor SSDL (Schema Definition Language) je jazyk založený na jazyce XML, který popisuje model úložiště aplikace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="69809-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="69809-104">V Entity Framework aplikaci se metadata modelu úložiště načítají ze souboru. ssdl (napsaného ve verzi SSDL) do instance System. data. Metadata. Edm. StoreItemCollection a jsou přístupná pomocí metod v System. data. Metadata. Edm. MetadataWorkspace – třída</span><span class="sxs-lookup"><span data-stu-id="69809-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="69809-105">Entity Framework používá metadata modelu úložiště k překladu dotazů na koncepční model pro ukládání specifických příkazů.</span><span class="sxs-lookup"><span data-stu-id="69809-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="69809-106">Entity Framework Designer (EF Designer) ukládá informace o modelu úložiště v souboru EDMX v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="69809-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="69809-107">V okamžiku sestavení Entity Designer používá informace v souboru. edmx k vytvoření souboru. ssdl, který je vyžadován Entity Framework za běhu.</span><span class="sxs-lookup"><span data-stu-id="69809-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="69809-108">Verze SSDL jsou odlišeny obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="69809-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="69809-109">Verze SSDL</span><span class="sxs-lookup"><span data-stu-id="69809-109">SSDL Version</span></span> | <span data-ttu-id="69809-110">Obor názvů XML</span><span class="sxs-lookup"><span data-stu-id="69809-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="69809-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="69809-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="69809-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="69809-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="69809-113">SSDL V3</span><span class="sxs-lookup"><span data-stu-id="69809-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="69809-114">Element Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-114">Association Element (SSDL)</span></span>

<span data-ttu-id="69809-115">Element **Association** ve službě Store Schema Definition Language (SSDL) Určuje sloupce tabulky, které se účastní omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="69809-116">Dva povinné podřízené elementy end určují tabulky na konci přidružení a násobnost na každém konci.</span><span class="sxs-lookup"><span data-stu-id="69809-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="69809-117">Volitelný element elementu ReferentialConstraint určuje hlavní a závislé konce přidružení a také zúčastněné sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="69809-118">Pokud není přítomen žádný element **elementu ReferentialConstraint** , je nutné použít element AssociationSetMapping k určení mapování sloupce pro přidružení.</span><span class="sxs-lookup"><span data-stu-id="69809-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="69809-119">Element **Association** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-120">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-121">End (přesně dva)</span><span class="sxs-lookup"><span data-stu-id="69809-121">End (exactly two)</span></span>
-   <span data-ttu-id="69809-122">Elementu ReferentialConstraint (nula nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="69809-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="69809-123">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-124">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-124">Applicable Attributes</span></span>

<span data-ttu-id="69809-125">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Association** .</span><span class="sxs-lookup"><span data-stu-id="69809-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="69809-126">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-126">Attribute Name</span></span> | <span data-ttu-id="69809-127">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-127">Is Required</span></span> | <span data-ttu-id="69809-128">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="69809-129">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-129">**Name**</span></span>       | <span data-ttu-id="69809-130">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-130">Yes</span></span>         | <span data-ttu-id="69809-131">Název odpovídajícího omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-132">Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="69809-133">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-134">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-135">Example</span></span>

<span data-ttu-id="69809-136">Následující příklad ukazuje element **Association** , který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_FK** :</span><span class="sxs-lookup"><span data-stu-id="69809-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="69809-137">AssociationSet – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="69809-138">Element **AssociationSet** ve službě Store Schema Definition Language (SSDL) představuje omezení cizího klíče mezi dvěma tabulkami v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="69809-139">Sloupce tabulky, které jsou součástí omezení cizího klíče, jsou zadány v elementu Association.</span><span class="sxs-lookup"><span data-stu-id="69809-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="69809-140">Element **Association** , který odpovídá danému elementu **AssociationSet** , je určen v atributu **Association** elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="69809-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="69809-141">Sady přidružení SSDL jsou namapovány na sady přidružení CSDL pomocí elementu AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="69809-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="69809-142">Pokud je však přidružení CSDL pro danou sadu přidružení CSDL definováno pomocí elementu elementu ReferentialConstraint, není nutný žádný odpovídající prvek **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="69809-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="69809-143">V tomto případě, pokud je přítomen element **AssociationSetMapping** , mapování, které definuje, bude přepsáno prvkem **elementu ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="69809-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="69809-144">Element **AssociationSet** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-145">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-146">End (nula nebo 2)</span><span class="sxs-lookup"><span data-stu-id="69809-146">End (zero or two)</span></span>
-   <span data-ttu-id="69809-147">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-148">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-148">Applicable Attributes</span></span>

<span data-ttu-id="69809-149">Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="69809-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="69809-150">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-150">Attribute Name</span></span>  | <span data-ttu-id="69809-151">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-151">Is Required</span></span> | <span data-ttu-id="69809-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-153">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-153">**Name**</span></span>        | <span data-ttu-id="69809-154">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-154">Yes</span></span>         | <span data-ttu-id="69809-155">Název omezení cizího klíče, který představuje sada přidružení.</span><span class="sxs-lookup"><span data-stu-id="69809-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="69809-156">**Řídí**</span><span class="sxs-lookup"><span data-stu-id="69809-156">**Association**</span></span> | <span data-ttu-id="69809-157">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-157">Yes</span></span>         | <span data-ttu-id="69809-158">Název asociace, který definuje sloupce, které jsou součástí omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-159">Pro element **AssociationSet** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="69809-160">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-161">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-162">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-162">Example</span></span>

<span data-ttu-id="69809-163">Následující příklad ukazuje element **AssociationSet** , který představuje omezení `FK_CustomerOrders` cizího klíče v podkladové databázi:</span><span class="sxs-lookup"><span data-stu-id="69809-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="69809-164">CollectionType – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="69809-165">Element **CollectionType** ve službě Store Schema Definition Language (SSDL) určuje, že návratový typ funkce je kolekce.</span><span class="sxs-lookup"><span data-stu-id="69809-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="69809-166">Element **CollectionType** je podřízeným prvkem atributu ReturnType.</span><span class="sxs-lookup"><span data-stu-id="69809-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="69809-167">Typ kolekce je určen pomocí podřízeného prvku RowType:</span><span class="sxs-lookup"><span data-stu-id="69809-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="69809-168">Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="69809-169">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-170">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-171">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-171">Example</span></span>

<span data-ttu-id="69809-172">Následující příklad ukazuje funkci, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků.</span><span class="sxs-lookup"><span data-stu-id="69809-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="69809-173">CommandText – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="69809-174">Element **CommandText** ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku funkce, který umožňuje definovat příkaz SQL, který je spuštěn v databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="69809-175">Element **CommandText** umožňuje přidat funkce, které jsou podobné uložené proceduře v databázi, ale v modelu úložiště definujte element **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="69809-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="69809-176">Element **CommandText** nemůže mít podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="69809-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="69809-177">Tělo elementu **CommandText** musí být platným příkazem SQL pro podkladovou databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="69809-178">Pro element **CommandText** nelze použít žádné atributy.</span><span class="sxs-lookup"><span data-stu-id="69809-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="69809-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-179">Example</span></span>

<span data-ttu-id="69809-180">Následující příklad ukazuje element **Function** s podřízeným elementem **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="69809-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="69809-181">Vystavte funkci **UpdateProductInOrder** jako metodu objektu ObjectContext importem do koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="69809-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="69809-182">DefiningQuery – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="69809-183">Element **DefiningQuery** v úložišti SSDL (Schema Definition Language) umožňuje spustit příkaz SQL přímo v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="69809-184">Element **DefiningQuery** se běžně používá jako zobrazení databáze, ale zobrazení je definované v modelu úložiště místo databáze.</span><span class="sxs-lookup"><span data-stu-id="69809-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="69809-185">Zobrazení definované v elementu **DefiningQuery** lze namapovat na typ entity v koncepčním modelu prostřednictvím elementu EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="69809-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="69809-186">Tato mapování jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="69809-186">These mappings are read-only.</span></span>  

<span data-ttu-id="69809-187">Následující syntaxe SSDL ukazuje deklaraci objektu **EntitySet** následovaný elementem **DefiningQuery** , který obsahuje dotaz použitý k načtení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="69809-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="69809-188">Uložené procedury v Entity Framework můžete použít k povolení scénářů pro čtení i zápis v zobrazeních.</span><span class="sxs-lookup"><span data-stu-id="69809-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="69809-189"> Můžete použít buď zobrazení zdroje dat, nebo zobrazení Entity SQL jako základní tabulku pro načítání dat a pro zpracování změn v uložených procedurách.</span><span class="sxs-lookup"><span data-stu-id="69809-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="69809-190">Element **DefiningQuery** můžete použít k cíli Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="69809-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="69809-191">I když SQL Server Compact 3,5 nepodporuje uložené procedury, můžete implementovat podobné funkce pomocí elementu **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="69809-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="69809-192">Další místo, kde může být užitečné, je vytvoření uložených procedur k překonání neshody mezi datovými typy použitými v programovacím jazyce a zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="69809-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="69809-193">Můžete napsat **DefiningQuery** , který přebírá určitou sadu parametrů, a pak volá uloženou proceduru s jinou sadou parametrů, například uloženou proceduru, která odstraňuje data.</span><span class="sxs-lookup"><span data-stu-id="69809-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="69809-194">Závislý element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="69809-195">**Závislý** element ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje závislý konec omezení cizího klíče (označuje se také jako referenční omezení).</span><span class="sxs-lookup"><span data-stu-id="69809-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="69809-196">**Závislý** element určuje sloupec (nebo sloupce) v tabulce, která odkazuje na sloupec primárního klíče (nebo sloupce).</span><span class="sxs-lookup"><span data-stu-id="69809-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="69809-197">Prvky **PropertyRef** určují, na které sloupce se odkazuje.</span><span class="sxs-lookup"><span data-stu-id="69809-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="69809-198">Hlavní prvek určuje sloupce primárního klíče, na které jsou odkazovány sloupce, které jsou zadány v **závislém** elementu.</span><span class="sxs-lookup"><span data-stu-id="69809-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="69809-199">**Závislý** element může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-200">PropertyRef (jedna nebo víc)</span><span class="sxs-lookup"><span data-stu-id="69809-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="69809-201">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-202">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-202">Applicable Attributes</span></span>

<span data-ttu-id="69809-203">Následující tabulka popisuje atributy, které mohou být aplikovány na **závislý** element.</span><span class="sxs-lookup"><span data-stu-id="69809-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="69809-204">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-204">Attribute Name</span></span> | <span data-ttu-id="69809-205">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-205">Is Required</span></span> | <span data-ttu-id="69809-206">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-207">**Role**</span><span class="sxs-lookup"><span data-stu-id="69809-207">**Role**</span></span>       | <span data-ttu-id="69809-208">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-208">Yes</span></span>         | <span data-ttu-id="69809-209">Stejná hodnota jako atribut **role** (Pokud se používá) odpovídajícího elementu end; jinak název tabulky, která obsahuje odkazující sloupec.</span><span class="sxs-lookup"><span data-stu-id="69809-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-210">Pro **závislý** element lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="69809-211">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="69809-212">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-213">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-213">Example</span></span>

<span data-ttu-id="69809-214">Následující příklad ukazuje element Association, který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="69809-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="69809-215">**Závislý** element určuje sloupec **KódZákazníka** tabulky **Order** jako závislý konec omezení.</span><span class="sxs-lookup"><span data-stu-id="69809-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="69809-216">Element dokumentace (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="69809-217">Element **dokumentace** v úložišti SSDL (Schema Definition Language) je možné použít k poskytnutí informací o objektu, který je definovaný v nadřazeném elementu.</span><span class="sxs-lookup"><span data-stu-id="69809-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="69809-218">Prvek **dokumentace** může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-219">**Summary**: stručný popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="69809-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="69809-220">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-220">(zero or one element)</span></span>
-   <span data-ttu-id="69809-221">**Longdescription**: rozsáhlý popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="69809-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="69809-222">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-223">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-223">Applicable Attributes</span></span>

<span data-ttu-id="69809-224">Pro prvek **dokumentace** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="69809-225">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="69809-226">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-227">Example</span></span>

<span data-ttu-id="69809-228">Následující příklad ukazuje prvek **dokumentace** jako podřízený prvek elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="69809-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="69809-229">End – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-229">End Element (SSDL)</span></span>

<span data-ttu-id="69809-230">Element **End** ve službě Store Schema Definition Language (SSDL) Určuje tabulku a počet řádků na jednom konci omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="69809-231">Element **End** může být podřízeným prvkem elementu Association nebo elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="69809-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="69809-232">V každém případě jsou možné podřízené prvky a příslušné atributy odlišné.</span><span class="sxs-lookup"><span data-stu-id="69809-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="69809-233">Ukončit element jako podřízený elementu přidružení</span><span class="sxs-lookup"><span data-stu-id="69809-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="69809-234">Element **End** (jako podřízený prvek elementu **Association** ) Určuje tabulku a počet řádků na konci omezení cizího klíče s atributy **typu** a **násobnosti** .</span><span class="sxs-lookup"><span data-stu-id="69809-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="69809-235">Konce omezení cizího klíče jsou definovány jako součást přidružení SSDL; přidružení SSDL musí mít přesně dva konce.</span><span class="sxs-lookup"><span data-stu-id="69809-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="69809-236">Element **End** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-237">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="69809-238">Při odstranění (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="69809-239">Prvky poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="69809-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="69809-240">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-240">Applicable Attributes</span></span>

<span data-ttu-id="69809-241">Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízenou položkou elementu **Association** .</span><span class="sxs-lookup"><span data-stu-id="69809-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="69809-242">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-242">Attribute Name</span></span>   | <span data-ttu-id="69809-243">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-243">Is Required</span></span> | <span data-ttu-id="69809-244">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-245">**Typ**</span><span class="sxs-lookup"><span data-stu-id="69809-245">**Type**</span></span>         | <span data-ttu-id="69809-246">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-246">Yes</span></span>         | <span data-ttu-id="69809-247">Plně kvalifikovaný název sady entit SSDL, která je na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="69809-248">**Role**</span><span class="sxs-lookup"><span data-stu-id="69809-248">**Role**</span></span>         | <span data-ttu-id="69809-249">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-249">No</span></span>          | <span data-ttu-id="69809-250">Hodnota atributu **role** v objektu zabezpečení nebo závislém elementu odpovídajícího prvku elementu ReferentialConstraint (Pokud se používá).</span><span class="sxs-lookup"><span data-stu-id="69809-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="69809-251">**Násobnost**</span><span class="sxs-lookup"><span data-stu-id="69809-251">**Multiplicity**</span></span> | <span data-ttu-id="69809-252">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-252">Yes</span></span>         | <span data-ttu-id="69809-253">**1**, **0.. 1**nebo **\*** v závislosti na počtu řádků, které mohou být na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="69809-254">**1** znamená, že v konci omezení cizího klíče existuje přesně jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="69809-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="69809-255">**0.. 1** znamená, že v konci omezení cizího klíče existuje žádný nebo jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="69809-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="69809-256">**\*** označuje, že v konci omezení cizího klíče existuje nula, jeden nebo více řádků.</span><span class="sxs-lookup"><span data-stu-id="69809-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-257">Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="69809-258">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="69809-259">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="69809-260">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-260">Example</span></span>

<span data-ttu-id="69809-261">Následující příklad ukazuje element **Association** , který definuje omezení cizího klíče **CustomerOrders\_** .</span><span class="sxs-lookup"><span data-stu-id="69809-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="69809-262">Hodnoty **násobnosti** zadané u každého elementu **End** označují, že mnoho řádků v tabulce **Orders** je možné přidružit k řádku v tabulce **Customers** , ale k řádku v tabulce **Orders** může být přidružen pouze jeden řádek v tabulce **Customers** .</span><span class="sxs-lookup"><span data-stu-id="69809-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="69809-263">Kromě toho element **IsDeleted** označuje, že všechny řádky v tabulce **Orders** , které odkazují na konkrétní řádek v tabulce **Customers** , se odstraní, pokud se řádek v tabulce **Customers (zákazníci** ) odstraní.</span><span class="sxs-lookup"><span data-stu-id="69809-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="69809-264">Ukončit element jako podřízený element elementu AssociationSet</span><span class="sxs-lookup"><span data-stu-id="69809-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="69809-265">Element **End** (jako podřízený prvek elementu **AssociationSet** ) Určuje tabulku na jednom konci omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="69809-266">Element **End** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-267">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-268">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="69809-269">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-269">Applicable Attributes</span></span>

<span data-ttu-id="69809-270">Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízeným elementem elementu **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="69809-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="69809-271">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-271">Attribute Name</span></span> | <span data-ttu-id="69809-272">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-272">Is Required</span></span> | <span data-ttu-id="69809-273">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-274">**Sada**</span><span class="sxs-lookup"><span data-stu-id="69809-274">**EntitySet**</span></span>  | <span data-ttu-id="69809-275">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-275">Yes</span></span>         | <span data-ttu-id="69809-276">Název sady entit SSDL, která je na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="69809-277">**Role**</span><span class="sxs-lookup"><span data-stu-id="69809-277">**Role**</span></span>       | <span data-ttu-id="69809-278">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-278">No</span></span>          | <span data-ttu-id="69809-279">Hodnota jednoho z atributů **role** zadaná u jednoho elementu **End** odpovídajícího elementu Association.</span><span class="sxs-lookup"><span data-stu-id="69809-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-280">Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="69809-281">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="69809-282">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="69809-283">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-283">Example</span></span>

<span data-ttu-id="69809-284">Následující příklad ukazuje element **EntityContainer** s elementem **AssociationSet** se dvěma elementy **End** :</span><span class="sxs-lookup"><span data-stu-id="69809-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="69809-285">EntityContainer – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="69809-286">Element **EntityContainer** v rámci služby Store Schema Definition Language (SSDL) popisuje strukturu podkladového zdroje dat v aplikaci Entity Framework: sady entit SSDL (definované v elementech EntitySet) reprezentují tabulky v databázi, typy entit SSDL (definované v elementech EntityType) reprezentují v databázi omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="69809-287">Kontejner entit modelu úložiště se mapuje na kontejner entit koncepčního modelu prostřednictvím elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="69809-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="69809-288">Element **EntityContainer** může mít nula nebo jeden prvek dokumentace.</span><span class="sxs-lookup"><span data-stu-id="69809-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="69809-289">Pokud je přítomen prvek **dokumentace** , musí předcházet všem ostatním podřízeným elementům.</span><span class="sxs-lookup"><span data-stu-id="69809-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="69809-290">Element **EntityContainer** může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-291">Sada</span><span class="sxs-lookup"><span data-stu-id="69809-291">EntitySet</span></span>
-   <span data-ttu-id="69809-292">Vlastností</span><span class="sxs-lookup"><span data-stu-id="69809-292">AssociationSet</span></span>
-   <span data-ttu-id="69809-293">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="69809-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-294">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-294">Applicable Attributes</span></span>

<span data-ttu-id="69809-295">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="69809-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="69809-296">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-296">Attribute Name</span></span> | <span data-ttu-id="69809-297">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-297">Is Required</span></span> | <span data-ttu-id="69809-298">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="69809-299">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-299">**Name**</span></span>       | <span data-ttu-id="69809-300">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-300">Yes</span></span>         | <span data-ttu-id="69809-301">Název kontejneru entity.</span><span class="sxs-lookup"><span data-stu-id="69809-301">The name of the entity container.</span></span> <span data-ttu-id="69809-302">Tento název nesmí obsahovat tečky (.).</span><span class="sxs-lookup"><span data-stu-id="69809-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-303">Pro element **EntityContainer** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="69809-304">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-305">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-306">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-306">Example</span></span>

<span data-ttu-id="69809-307">Následující příklad ukazuje element **EntityContainer** , který definuje dvě sady entit a jednu sadu přidružení.</span><span class="sxs-lookup"><span data-stu-id="69809-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="69809-308">Všimněte si, že názvy entit a typů přidružení jsou kvalifikovány názvem oboru názvů konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="69809-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="69809-309">EntitySet – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="69809-310">Element **EntitySet** ve službě Store Schema Definition Language (SSDL) představuje tabulku nebo zobrazení v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="69809-311">Element EntityType ve SSDL představuje řádek v tabulce nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="69809-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="69809-312">Atribut **EntityType** elementu **EntitySet** určuje konkrétní typ entity SSDL reprezentující řádky v sadě entit SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="69809-313">Mapování mezi sadou entit CSDL a sadou entit SSDL je zadáno v elementu EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="69809-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="69809-314">Element **EntitySet** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-315">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="69809-316">DefiningQuery (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="69809-317">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="69809-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-318">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-318">Applicable Attributes</span></span>

<span data-ttu-id="69809-319">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="69809-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="69809-320">Některé atributy (zde nejsou uvedeny) mohou být kvalifikovány s aliasem **úložiště** .</span><span class="sxs-lookup"><span data-stu-id="69809-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="69809-321">Tyto atributy používá Průvodce modelem aktualizace při aktualizaci modelu.</span><span class="sxs-lookup"><span data-stu-id="69809-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="69809-322">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-322">Attribute Name</span></span> | <span data-ttu-id="69809-323">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-323">Is Required</span></span> | <span data-ttu-id="69809-324">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-325">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-325">**Name**</span></span>       | <span data-ttu-id="69809-326">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-326">Yes</span></span>         | <span data-ttu-id="69809-327">Název sady entit.</span><span class="sxs-lookup"><span data-stu-id="69809-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="69809-328">**Objektu**</span><span class="sxs-lookup"><span data-stu-id="69809-328">**EntityType**</span></span> | <span data-ttu-id="69809-329">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-329">Yes</span></span>         | <span data-ttu-id="69809-330">Plně kvalifikovaný název typu entity, pro který sada entit obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="69809-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="69809-331">**XSD**</span><span class="sxs-lookup"><span data-stu-id="69809-331">**Schema**</span></span>     | <span data-ttu-id="69809-332">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-332">No</span></span>          | <span data-ttu-id="69809-333">Schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="69809-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="69809-334">**Tabulka**</span><span class="sxs-lookup"><span data-stu-id="69809-334">**Table**</span></span>      | <span data-ttu-id="69809-335">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-335">No</span></span>          | <span data-ttu-id="69809-336">Databázová tabulka</span><span class="sxs-lookup"><span data-stu-id="69809-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="69809-337">Pro element **EntitySet** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="69809-338">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-339">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-340">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-340">Example</span></span>

<span data-ttu-id="69809-341">Následující příklad ukazuje element **EntityContainer** , který má dva elementy **EntitySet** a jeden element **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="69809-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="69809-342">EntityType – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="69809-343">Element **EntityType** ve službě Store Schema Definition Language (SSDL) představuje řádek v tabulce nebo zobrazení podkladové databáze.</span><span class="sxs-lookup"><span data-stu-id="69809-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="69809-344">Element EntitySet ve SSDL představuje tabulku nebo zobrazení, ve kterých se vyskytují řádky.</span><span class="sxs-lookup"><span data-stu-id="69809-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="69809-345">Atribut **EntityType** elementu **EntitySet** určuje konkrétní typ entity SSDL reprezentující řádky v sadě entit SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="69809-346">Mapování mezi typem entity SSDL a typem entity CSDL je zadáno v elementu element entitytypemapping.</span><span class="sxs-lookup"><span data-stu-id="69809-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="69809-347">Element **EntityType** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-348">Dokumentace (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="69809-349">Klíč (nula nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="69809-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="69809-350">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="69809-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-351">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-351">Applicable Attributes</span></span>

<span data-ttu-id="69809-352">Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="69809-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="69809-353">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-353">Attribute Name</span></span> | <span data-ttu-id="69809-354">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-354">Is Required</span></span> | <span data-ttu-id="69809-355">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-356">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-356">**Name**</span></span>       | <span data-ttu-id="69809-357">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-357">Yes</span></span>         | <span data-ttu-id="69809-358">Název typu entity</span><span class="sxs-lookup"><span data-stu-id="69809-358">The name of the entity type.</span></span> <span data-ttu-id="69809-359">Tato hodnota je obvykle stejná jako název tabulky, ve které typ entity představuje řádek.</span><span class="sxs-lookup"><span data-stu-id="69809-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="69809-360">Tato hodnota nesmí obsahovat žádné tečky (.).</span><span class="sxs-lookup"><span data-stu-id="69809-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-361">Pro element **EntityType** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="69809-362">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-363">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-364">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-364">Example</span></span>

<span data-ttu-id="69809-365">Následující příklad ukazuje element **EntityType** se dvěma vlastnostmi:</span><span class="sxs-lookup"><span data-stu-id="69809-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="69809-366">Function – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-366">Function Element (SSDL)</span></span>

<span data-ttu-id="69809-367">Prvek **funkce** v úložišti SSDL (Store Schema Definition Language) určuje uloženou proceduru, která existuje v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="69809-368">Element **Function** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-369">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-370">Parametr (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="69809-371">CommandText (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="69809-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="69809-372">ReturnType (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="69809-373">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="69809-374">Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** , nebo atributu **ReturnType** (viz níže), ale nikoli obojího.</span><span class="sxs-lookup"><span data-stu-id="69809-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="69809-375">Uložené procedury, které jsou zadány v modelu úložiště, lze importovat do koncepčního modelu aplikace.</span><span class="sxs-lookup"><span data-stu-id="69809-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="69809-376">Další informace najdete v tématu [dotazování s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="69809-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="69809-377">Element **Function** lze také použít k definování vlastních funkcí v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="69809-378">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-378">Applicable Attributes</span></span>

<span data-ttu-id="69809-379">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Function** .</span><span class="sxs-lookup"><span data-stu-id="69809-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="69809-380">Některé atributy (zde nejsou uvedeny) mohou být kvalifikovány s aliasem **úložiště** .</span><span class="sxs-lookup"><span data-stu-id="69809-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="69809-381">Tyto atributy používá Průvodce modelem aktualizace při aktualizaci modelu.</span><span class="sxs-lookup"><span data-stu-id="69809-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="69809-382">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-382">Attribute Name</span></span>             | <span data-ttu-id="69809-383">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-383">Is Required</span></span> | <span data-ttu-id="69809-384">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-385">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-385">**Name**</span></span>                   | <span data-ttu-id="69809-386">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-386">Yes</span></span>         | <span data-ttu-id="69809-387">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="69809-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="69809-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="69809-388">**ReturnType**</span></span>             | <span data-ttu-id="69809-389">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-389">No</span></span>          | <span data-ttu-id="69809-390">Návratový typ uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="69809-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="69809-391">**Souhrnné**</span><span class="sxs-lookup"><span data-stu-id="69809-391">**Aggregate**</span></span>              | <span data-ttu-id="69809-392">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-392">No</span></span>          | <span data-ttu-id="69809-393">**True** , pokud uložená procedura vrátí agregovanou hodnotu; v opačném případě **false**.</span><span class="sxs-lookup"><span data-stu-id="69809-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="69809-394">**Integrované**</span><span class="sxs-lookup"><span data-stu-id="69809-394">**BuiltIn**</span></span>                | <span data-ttu-id="69809-395">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-395">No</span></span>          | <span data-ttu-id="69809-396">**True** , pokud je funkce vestavěnou funkcí<sup>1</sup> ; v opačném případě **false**.</span><span class="sxs-lookup"><span data-stu-id="69809-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="69809-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="69809-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="69809-398">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-398">No</span></span>          | <span data-ttu-id="69809-399">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="69809-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="69809-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="69809-400">**NiladicFunction**</span></span>        | <span data-ttu-id="69809-401">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-401">No</span></span>          | <span data-ttu-id="69809-402">**True** , pokud je funkce funkce niladic<sup>2</sup> ; V opačném případě **NEPRAVDA** .</span><span class="sxs-lookup"><span data-stu-id="69809-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="69809-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="69809-403">**IsComposable**</span></span>           | <span data-ttu-id="69809-404">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-404">No</span></span>          | <span data-ttu-id="69809-405">**True** , pokud je funkce sestavitelnou funkcí<sup>3</sup> ; V opačném případě **NEPRAVDA** .</span><span class="sxs-lookup"><span data-stu-id="69809-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="69809-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="69809-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="69809-407">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-407">No</span></span>          | <span data-ttu-id="69809-408">Výčet definující sémantiku typu, která se používá k vyřešení přetížení funkce.</span><span class="sxs-lookup"><span data-stu-id="69809-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="69809-409">Výčet je definován v manifestu zprostředkovatele podle definice funkce.</span><span class="sxs-lookup"><span data-stu-id="69809-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="69809-410">Výchozí hodnota je **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="69809-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="69809-411">**XSD**</span><span class="sxs-lookup"><span data-stu-id="69809-411">**Schema**</span></span>                 | <span data-ttu-id="69809-412">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-412">No</span></span>          | <span data-ttu-id="69809-413">Název schématu, ve kterém je uložená procedura definovaná.</span><span class="sxs-lookup"><span data-stu-id="69809-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="69809-414"><sup>1</sup> integrovaná funkce je funkce, která je definována v databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="69809-415">Informace o funkcích, které jsou definovány v modelu úložiště, naleznete v tématu CommandText element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="69809-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="69809-416"><sup>2</sup> funkce niladic je funkce, která nepřijímá žádné parametry a při volání nepožaduje závorky.</span><span class="sxs-lookup"><span data-stu-id="69809-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="69809-417"><sup>3</sup> dvě funkce jsou sestavitelné, pokud výstup jedné funkce může být vstupem pro druhou funkci.</span><span class="sxs-lookup"><span data-stu-id="69809-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="69809-418">Pro element **Function** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="69809-419">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-420">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-421">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-421">Example</span></span>

<span data-ttu-id="69809-422">Následující příklad ukazuje prvek **funkce** , který odpovídá uložené proceduře **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="69809-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="69809-423">Uložená procedura přijímá dva parametry a nevrací hodnotu.</span><span class="sxs-lookup"><span data-stu-id="69809-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="69809-424">Key – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-424">Key Element (SSDL)</span></span>

<span data-ttu-id="69809-425">**Klíčovým** prvkem ve službě Store Schema Definition Language (SSDL) představuje primární klíč tabulky v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="69809-426">**Key** je podřízený prvek elementu EntityType, který představuje řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="69809-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="69809-427">Primární klíč je definován v elementu **Key** odkazem na jeden nebo více prvků vlastností, které jsou definovány v elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="69809-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="69809-428">**Klíčový** prvek může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-429">PropertyRef (jedna nebo víc)</span><span class="sxs-lookup"><span data-stu-id="69809-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="69809-430">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="69809-430">Annotation elements</span></span>

<span data-ttu-id="69809-431">Pro **klíčový** element nejsou použity žádné atributy.</span><span class="sxs-lookup"><span data-stu-id="69809-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="69809-432">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-432">Example</span></span>

<span data-ttu-id="69809-433">Následující příklad ukazuje element **EntityType** s klíčem, který odkazuje na jednu vlastnost:</span><span class="sxs-lookup"><span data-stu-id="69809-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="69809-434">IsDeleted – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="69809-435">Element **IsDeleted** ve službě Store Schema Definition Language (SSDL) odráží chování databáze při odstranění řádku, který je součástí omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="69809-436">Pokud je akce nastavena na **Cascade**, odstraní se i řádky, které odkazují na odstraněný řádek.</span><span class="sxs-lookup"><span data-stu-id="69809-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="69809-437">Pokud je akce nastavena na **hodnotu None**, nebudou odstraněny také řádky, které odkazují na odstraněný řádek.</span><span class="sxs-lookup"><span data-stu-id="69809-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="69809-438">Element **IsDeleted** je podřízeným prvkem elementu end.</span><span class="sxs-lookup"><span data-stu-id="69809-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="69809-439">Element **IsDeleted** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-440">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-441">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-442">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-442">Applicable Attributes</span></span>

<span data-ttu-id="69809-443">Následující tabulka popisuje atributy, které mohou být aplikovány na element **IsDeleted** .</span><span class="sxs-lookup"><span data-stu-id="69809-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="69809-444">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-444">Attribute Name</span></span> | <span data-ttu-id="69809-445">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-445">Is Required</span></span> | <span data-ttu-id="69809-446">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-447">**Akce**</span><span class="sxs-lookup"><span data-stu-id="69809-447">**Action**</span></span>     | <span data-ttu-id="69809-448">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-448">Yes</span></span>         | <span data-ttu-id="69809-449">**Cascade** nebo **none**.</span><span class="sxs-lookup"><span data-stu-id="69809-449">**Cascade** or **None**.</span></span> <span data-ttu-id="69809-450">(Hodnota **s omezením** je platná, ale má stejné chování jako **žádné**.)</span><span class="sxs-lookup"><span data-stu-id="69809-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-451">Pro element **IsDeleted** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="69809-452">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-453">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-454">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-454">Example</span></span>

<span data-ttu-id="69809-455">Následující příklad ukazuje element **Association** , který definuje omezení cizího klíče **CustomerOrders\_** .</span><span class="sxs-lookup"><span data-stu-id="69809-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="69809-456">Element **IsDeleted** označuje, že všechny řádky v tabulce **Orders** , které odkazují na konkrétní řádek v tabulce **Customers** , se odstraní, když se řádek v tabulce **Customers** odstraní.</span><span class="sxs-lookup"><span data-stu-id="69809-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="69809-457">Parameter – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="69809-458">Element **Parameter** ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku funkce, který určuje parametry pro uloženou proceduru v databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="69809-459">Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-460">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-461">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-462">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-462">Applicable Attributes</span></span>

<span data-ttu-id="69809-463">Následující tabulka popisuje atributy, které lze použít pro element **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="69809-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="69809-464">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-464">Attribute Name</span></span> | <span data-ttu-id="69809-465">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-465">Is Required</span></span> | <span data-ttu-id="69809-466">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-467">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-467">**Name**</span></span>       | <span data-ttu-id="69809-468">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-468">Yes</span></span>         | <span data-ttu-id="69809-469">Název parametru</span><span class="sxs-lookup"><span data-stu-id="69809-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="69809-470">**Typ**</span><span class="sxs-lookup"><span data-stu-id="69809-470">**Type**</span></span>       | <span data-ttu-id="69809-471">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-471">Yes</span></span>         | <span data-ttu-id="69809-472">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="69809-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="69809-473">**Mode**</span><span class="sxs-lookup"><span data-stu-id="69809-473">**Mode**</span></span>       | <span data-ttu-id="69809-474">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-474">No</span></span>          | <span data-ttu-id="69809-475">**In**, **out**nebo **InOut** v závislosti na tom, zda je parametr vstupní, výstupní nebo vstupní/výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="69809-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="69809-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="69809-476">**MaxLength**</span></span>  | <span data-ttu-id="69809-477">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-477">No</span></span>          | <span data-ttu-id="69809-478">Maximální délka parametru.</span><span class="sxs-lookup"><span data-stu-id="69809-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="69809-479">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="69809-479">**Precision**</span></span>  | <span data-ttu-id="69809-480">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-480">No</span></span>          | <span data-ttu-id="69809-481">Přesnost parametru.</span><span class="sxs-lookup"><span data-stu-id="69809-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="69809-482">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="69809-482">**Scale**</span></span>      | <span data-ttu-id="69809-483">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-483">No</span></span>          | <span data-ttu-id="69809-484">Měřítko parametru.</span><span class="sxs-lookup"><span data-stu-id="69809-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="69809-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="69809-485">**SRID**</span></span>       | <span data-ttu-id="69809-486">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-486">No</span></span>          | <span data-ttu-id="69809-487">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="69809-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="69809-488">Platné pouze pro parametry prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="69809-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="69809-489">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="69809-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-490">Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="69809-491">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-492">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-493">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-493">Example</span></span>

<span data-ttu-id="69809-494">Následující příklad ukazuje prvek **funkce** , který má dva prvky **parametru** , které určují vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="69809-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="69809-495">Element zabezpečení (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="69809-496">Element **Principal** ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje hlavní konec omezení cizího klíče (označuje se také jako referenční omezení).</span><span class="sxs-lookup"><span data-stu-id="69809-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="69809-497">Element **Principal** určuje sloupec (nebo sloupce) primárního klíče v tabulce, na kterou odkazuje jiný sloupec (nebo sloupce).</span><span class="sxs-lookup"><span data-stu-id="69809-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="69809-498">Prvky **PropertyRef** určují, na které sloupce se odkazuje.</span><span class="sxs-lookup"><span data-stu-id="69809-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="69809-499">Závislý element určuje sloupce, které odkazují na sloupce primárního klíče, které jsou zadány v elementu **Principal** .</span><span class="sxs-lookup"><span data-stu-id="69809-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="69809-500">Element **Principal** může mít následující podřízené elementy (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="69809-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="69809-501">PropertyRef (jedna nebo víc)</span><span class="sxs-lookup"><span data-stu-id="69809-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="69809-502">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-503">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-503">Applicable Attributes</span></span>

<span data-ttu-id="69809-504">Následující tabulka popisuje atributy, které mohou být aplikovány na element **Principal** .</span><span class="sxs-lookup"><span data-stu-id="69809-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="69809-505">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-505">Attribute Name</span></span> | <span data-ttu-id="69809-506">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-506">Is Required</span></span> | <span data-ttu-id="69809-507">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-508">**Role**</span><span class="sxs-lookup"><span data-stu-id="69809-508">**Role**</span></span>       | <span data-ttu-id="69809-509">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-509">Yes</span></span>         | <span data-ttu-id="69809-510">Stejná hodnota jako atribut **role** (Pokud se používá) odpovídajícího elementu end; jinak název tabulky, která obsahuje odkazovaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="69809-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-511">U elementu **Principal** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="69809-512">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="69809-513">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-514">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-514">Example</span></span>

<span data-ttu-id="69809-515">Následující příklad ukazuje element Association, který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="69809-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="69809-516">Element **Principal** určuje sloupec **KódZákazníka** tabulky **Customer** jako hlavní konec omezení.</span><span class="sxs-lookup"><span data-stu-id="69809-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="69809-517">Property – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-517">Property Element (SSDL)</span></span>

<span data-ttu-id="69809-518">Element **Property** ve službě Store Schema Definition Language (SSDL) představuje sloupec v tabulce v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="69809-519">Prvky **vlastností** jsou podřízené prvky EntityType, které představují řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="69809-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="69809-520">Každý element **Property** definovaný na elementu **EntityType** reprezentuje sloupec.</span><span class="sxs-lookup"><span data-stu-id="69809-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="69809-521">Element **Property** nemůže mít žádné podřízené elementy.</span><span class="sxs-lookup"><span data-stu-id="69809-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-522">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-522">Applicable Attributes</span></span>

<span data-ttu-id="69809-523">Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="69809-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="69809-524">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-524">Attribute Name</span></span>            | <span data-ttu-id="69809-525">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-525">Is Required</span></span> | <span data-ttu-id="69809-526">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-527">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-527">**Name**</span></span>                  | <span data-ttu-id="69809-528">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-528">Yes</span></span>         | <span data-ttu-id="69809-529">Název odpovídajícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="69809-530">**Typ**</span><span class="sxs-lookup"><span data-stu-id="69809-530">**Type**</span></span>                  | <span data-ttu-id="69809-531">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-531">Yes</span></span>         | <span data-ttu-id="69809-532">Typ odpovídajícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="69809-533">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="69809-533">**Nullable**</span></span>              | <span data-ttu-id="69809-534">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-534">No</span></span>          | <span data-ttu-id="69809-535">**True** (výchozí hodnota) nebo **false** v závislosti na tom, zda odpovídající sloupec může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="69809-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="69809-536">**Hodnot**</span><span class="sxs-lookup"><span data-stu-id="69809-536">**DefaultValue**</span></span>          | <span data-ttu-id="69809-537">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-537">No</span></span>          | <span data-ttu-id="69809-538">Výchozí hodnota odpovídajícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="69809-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="69809-539">**MaxLength**</span></span>             | <span data-ttu-id="69809-540">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-540">No</span></span>          | <span data-ttu-id="69809-541">Maximální délka odpovídajícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="69809-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="69809-542">**FixedLength**</span></span>           | <span data-ttu-id="69809-543">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-543">No</span></span>          | <span data-ttu-id="69809-544">**True** nebo **false** v závislosti na tom, jestli se odpovídající hodnota sloupce uloží jako řetězec s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="69809-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="69809-545">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="69809-545">**Precision**</span></span>             | <span data-ttu-id="69809-546">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-546">No</span></span>          | <span data-ttu-id="69809-547">Přesnost odpovídajícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="69809-548">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="69809-548">**Scale**</span></span>                 | <span data-ttu-id="69809-549">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-549">No</span></span>          | <span data-ttu-id="69809-550">Měřítko odpovídajícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="69809-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="69809-551">**Unicode**</span></span>               | <span data-ttu-id="69809-552">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-552">No</span></span>          | <span data-ttu-id="69809-553">**True** nebo **false** v závislosti na tom, jestli se odpovídající hodnota sloupce uloží jako řetězec Unicode.</span><span class="sxs-lookup"><span data-stu-id="69809-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="69809-554">**Velké**</span><span class="sxs-lookup"><span data-stu-id="69809-554">**Collation**</span></span>             | <span data-ttu-id="69809-555">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-555">No</span></span>          | <span data-ttu-id="69809-556">Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="69809-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="69809-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="69809-557">**SRID**</span></span>                  | <span data-ttu-id="69809-558">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-558">No</span></span>          | <span data-ttu-id="69809-559">Identifikátor odkazu prostorového systému</span><span class="sxs-lookup"><span data-stu-id="69809-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="69809-560">Platné pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="69809-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="69809-561">Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="69809-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="69809-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="69809-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="69809-563">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-563">No</span></span>          | <span data-ttu-id="69809-564">**None**, **identity** (Pokud je odpovídající hodnota sloupce identita, která je generována v databázi), nebo **vypočítána** (Pokud je v databázi vypočítána odpovídající hodnota sloupce).</span><span class="sxs-lookup"><span data-stu-id="69809-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="69809-565">Neplatný pro vlastnosti RowType.</span><span class="sxs-lookup"><span data-stu-id="69809-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-566">Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="69809-567">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-568">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-569">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-569">Example</span></span>

<span data-ttu-id="69809-570">Následující příklad ukazuje element **EntityType** se dvěma podřízenými prvky **vlastnosti** :</span><span class="sxs-lookup"><span data-stu-id="69809-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="69809-571">PropertyRef – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="69809-572">Element **PropertyRef** v úložišti pro definici schématu (SSDL) odkazuje na vlastnost definovanou na elementu EntityType, aby označoval, že vlastnost provede jednu z následujících rolí:</span><span class="sxs-lookup"><span data-stu-id="69809-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="69809-573">Být součástí primárního klíče tabulky, který představuje **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="69809-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="69809-574">Jeden nebo více elementů **PropertyRef** lze použít k definování primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="69809-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="69809-575">Další informace naleznete v tématu klíčový element.</span><span class="sxs-lookup"><span data-stu-id="69809-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="69809-576">Být závislé nebo hlavní zakončení referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="69809-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="69809-577">Další informace naleznete v tématu elementu ReferentialConstraint element.</span><span class="sxs-lookup"><span data-stu-id="69809-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="69809-578">Element **PropertyRef** může mít pouze následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="69809-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="69809-579">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-580">Prvky poznámky</span><span class="sxs-lookup"><span data-stu-id="69809-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-581">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-581">Applicable Attributes</span></span>

<span data-ttu-id="69809-582">Následující tabulka popisuje atributy, které mohou být aplikovány na element **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="69809-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="69809-583">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-583">Attribute Name</span></span> | <span data-ttu-id="69809-584">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-584">Is Required</span></span> | <span data-ttu-id="69809-585">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="69809-586">**Název**</span><span class="sxs-lookup"><span data-stu-id="69809-586">**Name**</span></span>       | <span data-ttu-id="69809-587">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-587">Yes</span></span>         | <span data-ttu-id="69809-588">Název odkazované vlastnosti</span><span class="sxs-lookup"><span data-stu-id="69809-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="69809-589">Pro element **PropertyRef** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="69809-590">Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="69809-591">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-592">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-592">Example</span></span>

<span data-ttu-id="69809-593">Následující příklad ukazuje element **PropertyRef** , který slouží k definování primárního klíče odkazem na vlastnost, která je definována v elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="69809-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="69809-594">Elementu ReferentialConstraint – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="69809-595">Element **elementu ReferentialConstraint** ve službě Store Schema Definition Language (SSDL) představuje omezení cizího klíče (označované také jako omezení referenční integrity) v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="69809-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="69809-596">Hlavní a závislé elementy na konci omezení jsou určeny hlavními a závislými podřízenými prvky v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="69809-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="69809-597">Sloupce, které se podílejí na koncích objektu zabezpečení a závislých, jsou odkazovány pomocí elementů PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="69809-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="69809-598">Element **elementu ReferentialConstraint** je volitelný podřízený prvek elementu Association.</span><span class="sxs-lookup"><span data-stu-id="69809-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="69809-599">Pokud se nepoužívá element **elementu ReferentialConstraint** k mapování omezení cizího klíče, který je určen v elementu **Association** , je nutné použít element AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="69809-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="69809-600">Element **elementu ReferentialConstraint** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="69809-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="69809-601">Dokumentace (0 nebo 1)</span><span class="sxs-lookup"><span data-stu-id="69809-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="69809-602">Objekt zabezpečení (právě jeden)</span><span class="sxs-lookup"><span data-stu-id="69809-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="69809-603">Závislý (právě jeden)</span><span class="sxs-lookup"><span data-stu-id="69809-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="69809-604">Prvky poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-605">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-605">Applicable Attributes</span></span>

<span data-ttu-id="69809-606">Pro element **elementu ReferentialConstraint** může být použit libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="69809-607">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-608">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-609">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-609">Example</span></span>

<span data-ttu-id="69809-610">Následující příklad ukazuje element **Association** , který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_FK** :</span><span class="sxs-lookup"><span data-stu-id="69809-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="69809-611">ReturnType – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="69809-612">Element **ReturnType** v rámci služby Store Schema Definition Language (SSDL) určuje návratový typ pro funkci, která je definována v elementu **Function** .</span><span class="sxs-lookup"><span data-stu-id="69809-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="69809-613">Návratový typ funkce lze také zadat s atributem **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="69809-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="69809-614">Návratový typ funkce je zadán pomocí atributu **Type** nebo elementu **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="69809-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="69809-615">Element **ReturnType** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="69809-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="69809-616">CollectionType (jeden)</span><span class="sxs-lookup"><span data-stu-id="69809-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="69809-617">Pro element **ReturnType** lze použít libovolný počet atributů poznámky (vlastní atributy XML).</span><span class="sxs-lookup"><span data-stu-id="69809-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="69809-618">Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="69809-619">Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="69809-620">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-620">Example</span></span>

<span data-ttu-id="69809-621">Následující příklad používá **funkci** , která vrací kolekci řádků.</span><span class="sxs-lookup"><span data-stu-id="69809-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="69809-622">RowType – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="69809-623">Element **RowType** ve službě Store Schema Definition Language (SSDL) definuje nepojmenované struktury jako návratový typ pro funkci definovanou v úložišti.</span><span class="sxs-lookup"><span data-stu-id="69809-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="69809-624">Element **RowType** je podřízeným prvkem elementu **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="69809-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="69809-625">Element **RowType** může mít následující podřízené prvky:</span><span class="sxs-lookup"><span data-stu-id="69809-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="69809-626">Vlastnost (jedna nebo více)</span><span class="sxs-lookup"><span data-stu-id="69809-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="69809-627">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-627">Example</span></span>

<span data-ttu-id="69809-628">Následující příklad ukazuje funkci úložiště, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="69809-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="69809-629">Schema – element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="69809-630">Element **Schema** v nástroji Store Schema Definition Language (SSDL) je kořenovým prvkem definice modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="69809-631">Obsahuje definice pro objekty, funkce a kontejnery, které tvoří model úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="69809-632">Element **Schema** může obsahovat nula nebo více z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="69809-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="69809-633">Řídí</span><span class="sxs-lookup"><span data-stu-id="69809-633">Association</span></span>
-   <span data-ttu-id="69809-634">Typ entity</span><span class="sxs-lookup"><span data-stu-id="69809-634">EntityType</span></span>
-   <span data-ttu-id="69809-635">Kontejneru</span><span class="sxs-lookup"><span data-stu-id="69809-635">EntityContainer</span></span>
-   <span data-ttu-id="69809-636">Funkce</span><span class="sxs-lookup"><span data-stu-id="69809-636">Function</span></span>

<span data-ttu-id="69809-637">Element **Schema** používá atribut **Namespace** k definování oboru názvů pro typ entity a objekty přidružení v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="69809-638">V rámci oboru názvů nemohou mít žádné dva objekty stejný název.</span><span class="sxs-lookup"><span data-stu-id="69809-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="69809-639">Obor názvů modelu úložiště se liší od oboru názvů XML elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="69809-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="69809-640">Obor názvů modelu úložiště (jak je definováno atributem **Namespace** ) je logický kontejner pro typy entit a typy přidružení.</span><span class="sxs-lookup"><span data-stu-id="69809-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="69809-641">Obor názvů XML (uvedený atributem **xmlns** ) elementu **Schema** je výchozí obor názvů pro podřízené elementy a atributy elementu **Schema** .</span><span class="sxs-lookup"><span data-stu-id="69809-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="69809-642">Obory názvů XML formuláře https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (kde RRRR a MM představují rok a měsíc) jsou vyhrazené pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="69809-643">Vlastní elementy a atributy nemůžou být v oborech názvů, které mají tento formulář.</span><span class="sxs-lookup"><span data-stu-id="69809-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="69809-644">Použitelné atributy</span><span class="sxs-lookup"><span data-stu-id="69809-644">Applicable Attributes</span></span>

<span data-ttu-id="69809-645">Následující tabulka popisuje atributy, které lze použít pro element **Schema** .</span><span class="sxs-lookup"><span data-stu-id="69809-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="69809-646">Název atributu</span><span class="sxs-lookup"><span data-stu-id="69809-646">Attribute Name</span></span>            | <span data-ttu-id="69809-647">Je povinné</span><span class="sxs-lookup"><span data-stu-id="69809-647">Is Required</span></span> | <span data-ttu-id="69809-648">Hodnota</span><span class="sxs-lookup"><span data-stu-id="69809-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-649">**Hosting**</span><span class="sxs-lookup"><span data-stu-id="69809-649">**Namespace**</span></span>             | <span data-ttu-id="69809-650">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-650">Yes</span></span>         | <span data-ttu-id="69809-651">Obor názvů modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-651">The namespace of the storage model.</span></span> <span data-ttu-id="69809-652">Hodnota atributu **Namespace** slouží k vytvoření plně kvalifikovaného názvu typu.</span><span class="sxs-lookup"><span data-stu-id="69809-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="69809-653">Pokud je například **EntityType** s názvem *Zákazník* v oboru názvů ExampleModel. Store, pak plně kvalifikovaný název třídy **EntityType** je ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="69809-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="69809-654">Následující řetězce nelze použít jako hodnotu pro atribut **Namespace** : **System**, **přechodný**nebo **EDM**.</span><span class="sxs-lookup"><span data-stu-id="69809-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="69809-655">Hodnota atributu **Namespace** nemůže být stejná jako hodnota atributu **Namespace** v elementu schématu CSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="69809-656">**Zástupný**</span><span class="sxs-lookup"><span data-stu-id="69809-656">**Alias**</span></span>                 | <span data-ttu-id="69809-657">Ne</span><span class="sxs-lookup"><span data-stu-id="69809-657">No</span></span>          | <span data-ttu-id="69809-658">Identifikátor použitý místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="69809-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="69809-659">Pokud je například **EntityType** s názvem *Zákazník* v oboru názvů ExampleModel. Store a hodnota atributu **alias** je *StorageModel*, pak můžete jako plně kvalifikovaný název **objektu EntityType** použít StorageModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="69809-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="69809-660">**Poskytovatel**</span><span class="sxs-lookup"><span data-stu-id="69809-660">**Provider**</span></span>              | <span data-ttu-id="69809-661">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-661">Yes</span></span>         | <span data-ttu-id="69809-662">Zprostředkovatel dat.</span><span class="sxs-lookup"><span data-stu-id="69809-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="69809-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="69809-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="69809-664">Ano</span><span class="sxs-lookup"><span data-stu-id="69809-664">Yes</span></span>         | <span data-ttu-id="69809-665">Token, který indikuje poskytovateli, který manifest Provider vrátí.</span><span class="sxs-lookup"><span data-stu-id="69809-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="69809-666">Není definován žádný formát pro token.</span><span class="sxs-lookup"><span data-stu-id="69809-666">No format for the token is defined.</span></span> <span data-ttu-id="69809-667">Hodnoty pro token jsou definovány zprostředkovatelem.</span><span class="sxs-lookup"><span data-stu-id="69809-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="69809-668">Informace o tokenech manifestu poskytovatele SQL Server najdete v tématu SqlClient for Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="69809-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="69809-669">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-669">Example</span></span>

<span data-ttu-id="69809-670">Následující příklad ukazuje element **schématu** , který obsahuje element **EntityContainer** , dva elementy **EntityType** a jeden element **Association** .</span><span class="sxs-lookup"><span data-stu-id="69809-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="69809-671">Atributy poznámek</span><span class="sxs-lookup"><span data-stu-id="69809-671">Annotation Attributes</span></span>

<span data-ttu-id="69809-672">Atributy poznámek ve službě Store Schema Definition Language (SSDL) jsou vlastní atributy XML v modelu úložiště, které poskytují dodatečná metadata o prvcích v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="69809-673">Kromě existence platné struktury XML platí následující omezení pro atributy poznámek:</span><span class="sxs-lookup"><span data-stu-id="69809-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="69809-674">Atributy poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="69809-675">Plně kvalifikované názvy všech dvou atributů poznámek nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="69809-676">U daného elementu SSDL lze použít více než jeden atribut poznámky.</span><span class="sxs-lookup"><span data-stu-id="69809-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="69809-677">K metadatům obsaženým v prvcích poznámky lze za běhu přistup použít třídy v oboru názvů System. data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="69809-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="69809-678">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-678">Example</span></span>

<span data-ttu-id="69809-679">Následující příklad ukazuje element EntityType, který má atribut poznámky aplikovaný na vlastnost **ČísloObjednávky** .</span><span class="sxs-lookup"><span data-stu-id="69809-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="69809-680">V příkladu se zobrazí také element poznámky přidaný do elementu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="69809-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="69809-681">Prvky poznámky (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="69809-682">Prvky poznámek ve službě Store Schema Definition Language (SSDL) jsou vlastní prvky XML v modelu úložiště, které poskytují dodatečná metadata o modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="69809-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="69809-683">Kromě existence platné struktury XML platí následující omezení pro prvky poznámky:</span><span class="sxs-lookup"><span data-stu-id="69809-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="69809-684">Prvky poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="69809-685">Plně kvalifikované názvy všech dvou prvků poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="69809-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="69809-686">Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích daného elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="69809-687">Více než jeden element poznámky může být podřízeným prvku daného elementu SSDL.</span><span class="sxs-lookup"><span data-stu-id="69809-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="69809-688">Počínaje verzí .NET Framework 4 je k metadatům obsaženým v prvcích poznámky možné přistupovat za běhu pomocí tříd v oboru názvů System. data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="69809-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="69809-689">Příklad</span><span class="sxs-lookup"><span data-stu-id="69809-689">Example</span></span>

<span data-ttu-id="69809-690">Následující příklad ukazuje element EntityType, který má element Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="69809-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="69809-691">V příkladu se také zobrazuje atribut poznámky, který se použije na vlastnost **KódObjednávky** .</span><span class="sxs-lookup"><span data-stu-id="69809-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="69809-692">Omezující vlastnosti (SSDL)</span><span class="sxs-lookup"><span data-stu-id="69809-692">Facets (SSDL)</span></span>

<span data-ttu-id="69809-693">Charakteristiky v úložišti SSDL (Schema Definition Language) představují omezení pro typy sloupců, které jsou zadány v prvcích vlastností.</span><span class="sxs-lookup"><span data-stu-id="69809-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="69809-694">Omezující vlastnosti jsou implementovány jako atributy XML v prvcích **vlastností** .</span><span class="sxs-lookup"><span data-stu-id="69809-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="69809-695">Následující tabulka obsahuje popis omezujících vlastností, které jsou podporovány ve službě SSDL:</span><span class="sxs-lookup"><span data-stu-id="69809-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="69809-696">Omezující</span><span class="sxs-lookup"><span data-stu-id="69809-696">Facet</span></span>           | <span data-ttu-id="69809-697">Popis</span><span class="sxs-lookup"><span data-stu-id="69809-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69809-698">**Velké**</span><span class="sxs-lookup"><span data-stu-id="69809-698">**Collation**</span></span>   | <span data-ttu-id="69809-699">Určuje pořadí kompletování (nebo řazení sekvence), které se má použít při provádění operací porovnání a řazení u hodnot vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="69809-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="69809-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="69809-700">**FixedLength**</span></span> | <span data-ttu-id="69809-701">Určuje, zda může být délka hodnoty sloupce odlišná.</span><span class="sxs-lookup"><span data-stu-id="69809-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="69809-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="69809-702">**MaxLength**</span></span>   | <span data-ttu-id="69809-703">Určuje maximální délku hodnoty sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="69809-704">**Číslic**</span><span class="sxs-lookup"><span data-stu-id="69809-704">**Precision**</span></span>   | <span data-ttu-id="69809-705">Pro vlastnosti typu **Decimal**určuje počet číslic, které může mít hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="69809-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="69809-706">Pro vlastnosti typu **Time**, **DateTime**a **DateTimeOffset**určuje počet číslic pro zlomkovou část hodnoty sloupce v sekundách.</span><span class="sxs-lookup"><span data-stu-id="69809-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="69809-707">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="69809-707">**Scale**</span></span>       | <span data-ttu-id="69809-708">Určuje počet číslic vpravo od desetinné čárky pro hodnotu sloupce.</span><span class="sxs-lookup"><span data-stu-id="69809-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="69809-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="69809-709">**Unicode**</span></span>     | <span data-ttu-id="69809-710">Určuje, zda je hodnota sloupce uložena jako Unicode.</span><span class="sxs-lookup"><span data-stu-id="69809-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
