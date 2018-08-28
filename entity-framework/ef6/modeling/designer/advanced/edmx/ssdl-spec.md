---
title: Specifikace SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: 35c560d88e5078a7fc4c07b76020f3ad7d0735e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995276"
---
# <a name="ssdl-specification"></a><span data-ttu-id="70df8-102">Specifikace SSDL</span><span class="sxs-lookup"><span data-stu-id="70df8-102">SSDL Specification</span></span>
<span data-ttu-id="70df8-103">Store schema definition language (SSDL) je jazyk založený na formátu XML, který popisuje model úložiště aplikace Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="70df8-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="70df8-104">V aplikaci Entity Framework je načteno ze souboru ssdl (napsaného v SSDL) do instance System.Data.Metadata.Edm.StoreItemCollection metadat modelu úložiště a je přístupný pomocí metod v Třída System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="70df8-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="70df8-105">Entity Framework používá metadata modelu úložiště pro převod dotazy na koncepční model příkazů specifických pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="70df8-106">Návrhář Entity Framework (EF designeru) ukládá informace o modelu úložiště v souboru .edmx v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="70df8-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="70df8-107">V návrháři entit v okamžiku sestavení používá informace v souboru .edmx k vytvoření souboru ssdl, která je potřeba pro Entity Framework za běhu.</span><span class="sxs-lookup"><span data-stu-id="70df8-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="70df8-108">Verze souborů SSDL, jsou rozlišené pomocí obory názvů XML.</span><span class="sxs-lookup"><span data-stu-id="70df8-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="70df8-109">Verze souborů SSDL</span><span class="sxs-lookup"><span data-stu-id="70df8-109">SSDL Version</span></span> | <span data-ttu-id="70df8-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="70df8-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="70df8-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="70df8-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="70df8-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="70df8-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="70df8-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="70df8-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="70df8-114">Element Association (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-114">Association Element (SSDL)</span></span>

<span data-ttu-id="70df8-115">**Přidružení** element v store schema definition language (SSDL) určuje sloupce tabulky, které se účastní v omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="70df8-116">Dva elementy End požadovaný podřízený zadejte tabulek na konci přidružení a násobnost na každém konci.</span><span class="sxs-lookup"><span data-stu-id="70df8-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="70df8-117">Volitelný prvek elementu ReferentialConstraint určuje objektem zabezpečení a závislými konce přidružení, jakož i zúčastněných sloupců.</span><span class="sxs-lookup"><span data-stu-id="70df8-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="70df8-118">Pokud ne **elementu ReferentialConstraint** element je k dispozici, AssociationSetMapping elementu musí použít k určení mapování sloupců pro přidružení.</span><span class="sxs-lookup"><span data-stu-id="70df8-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="70df8-119">**Přidružení** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-120">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-121">End (přesně dvě)</span><span class="sxs-lookup"><span data-stu-id="70df8-121">End (exactly two)</span></span>
-   <span data-ttu-id="70df8-122">Elementu ReferentialConstraint (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="70df8-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="70df8-123">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-124">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-124">Applicable Attributes</span></span>

<span data-ttu-id="70df8-125">Následující tabulka popisuje atributy, které mohou být použity **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="70df8-126">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-126">Attribute Name</span></span> | <span data-ttu-id="70df8-127">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-127">Is Required</span></span> | <span data-ttu-id="70df8-128">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-129">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-129">**Name**</span></span>       | <span data-ttu-id="70df8-130">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-130">Yes</span></span>         | <span data-ttu-id="70df8-131">Název odpovídající omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-132">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="70df8-133">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-134">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-135">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-135">Example</span></span>

<span data-ttu-id="70df8-136">Následující příklad ukazuje **přidružení** element, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders**  omezení cizího klíče:</span><span class="sxs-lookup"><span data-stu-id="70df8-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="70df8-137">Element AssociationSet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="70df8-138">**AssociationSet** prvek v store schema definition language (SSDL) představuje omezení cizího klíče mezi dvěma tabulkami v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="70df8-139">Sloupce tabulky, které jsou součástí omezení cizího klíče jsou určené v elementu Association.</span><span class="sxs-lookup"><span data-stu-id="70df8-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="70df8-140">**Přidružení** element, který odpovídá daný **AssociationSet** prvek je zadán v **přidružení** atribut **AssociationSet**  elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="70df8-141">Nastaví přidružení souborů SSDL se mapují na sady přidružení CSDL prvkem AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="70df8-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="70df8-142">Ale pokud přidružení CSDL pro danou CSDL přidružení se definuje pomocí prvek elementu ReferentialConstraint odpovídající **AssociationSetMapping** element je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="70df8-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="70df8-143">V takovém případě pokud **AssociationSetMapping** element je k dispozici, mapování, definuje, budou přepsaná identifikátorem **elementu ReferentialConstraint** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="70df8-144">**AssociationSet** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-145">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-146">End (nula nebo dva)</span><span class="sxs-lookup"><span data-stu-id="70df8-146">End (zero or two)</span></span>
-   <span data-ttu-id="70df8-147">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-148">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-148">Applicable Attributes</span></span>

<span data-ttu-id="70df8-149">Následující tabulka popisuje atributy, které mohou být použity **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="70df8-150">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-150">Attribute Name</span></span>  | <span data-ttu-id="70df8-151">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-151">Is Required</span></span> | <span data-ttu-id="70df8-152">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-153">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-153">**Name**</span></span>        | <span data-ttu-id="70df8-154">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-154">Yes</span></span>         | <span data-ttu-id="70df8-155">Název omezení cizího klíče, nastavte přidružení představuje.</span><span class="sxs-lookup"><span data-stu-id="70df8-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="70df8-156">**Přidružení**</span><span class="sxs-lookup"><span data-stu-id="70df8-156">**Association**</span></span> | <span data-ttu-id="70df8-157">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-157">Yes</span></span>         | <span data-ttu-id="70df8-158">Název přidružení, které definuje sloupce, které se účastní v omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-159">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="70df8-160">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-161">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-162">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-162">Example</span></span>

<span data-ttu-id="70df8-163">Následující příklad ukazuje **AssociationSet** elementu, který představuje `FK_CustomerOrders` omezení cizího klíče v podkladové databázi:</span><span class="sxs-lookup"><span data-stu-id="70df8-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="70df8-164">Element CollectionType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="70df8-165">**CollectionType** element v store schema definition language (SSDL) určuje, že je návratový typ funkce kolekce.</span><span class="sxs-lookup"><span data-stu-id="70df8-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="70df8-166">**CollectionType** element je podřízeným prvkem elementu ReturnType.</span><span class="sxs-lookup"><span data-stu-id="70df8-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="70df8-167">Typ kolekce je určena pomocí podřízený element RowType:</span><span class="sxs-lookup"><span data-stu-id="70df8-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="70df8-168">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **CollectionType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="70df8-169">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-170">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-171">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-171">Example</span></span>

<span data-ttu-id="70df8-172">Následující příklad ukazuje funkci, která se používá **CollectionType** elementu k určení, že funkce vrátí kolekci řádků.</span><span class="sxs-lookup"><span data-stu-id="70df8-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="70df8-173">Element CommandText (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="70df8-174">**CommandText** prvek store schema definition language (SSDL) je podřízeným prvkem elementu funkce, který umožňuje definovat příkaz SQL, který je proveden v databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="70df8-175">**CommandText** element slouží k přidání funkce, která se podobá uloženou proceduru v databázi, ale můžete definovat **CommandText** elementu v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="70df8-176">**CommandText** element nemůže mít podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="70df8-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="70df8-177">Text **CommandText** element musí být platný příkaz SQL pro podkladové databáze.</span><span class="sxs-lookup"><span data-stu-id="70df8-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="70df8-178">Žádné atributy se vztahují na **CommandText** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-179">Example</span></span>

<span data-ttu-id="70df8-180">Následující příklad ukazuje **funkce** element s podřízený **CommandText** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="70df8-181">Zveřejnit **UpdateProductInOrder** fungují jako metoda v rozhraní ObjectContext naimportujete do koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="70df8-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="70df8-182">Element DefiningQuery (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="70df8-183">**DefiningQuery** prvek store schema definition language (SSDL) umožňuje spouštění příkazu SQL přímo v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="70df8-184">**DefiningQuery** element se často používá jako zobrazení databáze, ale zobrazení je definováno v modelu úložiště, ne na databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="70df8-185">Definováno v zobrazení **DefiningQuery** elementu je možné mapovat na typ entity v konceptuálním modelu prostřednictvím EntitySetMapping element.</span><span class="sxs-lookup"><span data-stu-id="70df8-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="70df8-186">Tato mapování jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="70df8-186">These mappings are read-only.</span></span>  

<span data-ttu-id="70df8-187">Následující syntaxe SSDL znázorňuje deklaraci **objektu EntitySet** následovaný **DefiningQuery** element, který obsahuje dotaz použitý k načtení zobrazení.</span><span class="sxs-lookup"><span data-stu-id="70df8-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="70df8-188">Uložené procedury v Entity Framework slouží k povolení scénářů pro čtení a zápis přes zobrazení.</span><span class="sxs-lookup"><span data-stu-id="70df8-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="70df8-189">Zobrazení zdroje dat nebo zobrazení Entity SQL můžete použít jako základní tabulky pro načítání dat a změna zpracování uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="70df8-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="70df8-190">Můžete použít **DefiningQuery** element do cílového serveru Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="70df8-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="70df8-191">I když SQL Server Compact 3.5 nepodporuje uložené procedury, podobné funkce s můžete implementovat **DefiningQuery** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="70df8-192">Jiné místo, kde může být užitečné je při vytváření uložených procedur k překonání Neshoda mezi datovými typy v programovacím jazyce a těch, které zdroj dat použít.</span><span class="sxs-lookup"><span data-stu-id="70df8-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="70df8-193">Můžete napsat **DefiningQuery** , která používá sadu parametrů a pak volá uloženou proceduru s jinou sadu parametrů, třeba uloženou proceduru, která odstraní data.</span><span class="sxs-lookup"><span data-stu-id="70df8-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="70df8-194">Závislé – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="70df8-195">**Závislé** prvek store schema definition language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje závislé konec omezení cizího klíče (tzv. referenční omezení).</span><span class="sxs-lookup"><span data-stu-id="70df8-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="70df8-196">**Závislé** prvek určuje sloupec (nebo sloupce) v tabulce, která odkazují na sloupec primárního klíče (nebo sloupce).</span><span class="sxs-lookup"><span data-stu-id="70df8-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="70df8-197">**PropertyRef** určují prvky sloupce, které jsou odkazovány.</span><span class="sxs-lookup"><span data-stu-id="70df8-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="70df8-198">Hlavní prvek určuje primární klíčové sloupce, které jsou odkazovány podle sloupců, které jsou určené v **závislé** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="70df8-199">**Závislé** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-200">PropertyRef (jeden nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="70df8-201">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-202">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-202">Applicable Attributes</span></span>

<span data-ttu-id="70df8-203">Následující tabulka popisuje atributy, které mohou být použity **závislé** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="70df8-204">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-204">Attribute Name</span></span> | <span data-ttu-id="70df8-205">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-205">Is Required</span></span> | <span data-ttu-id="70df8-206">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-207">**Role**</span><span class="sxs-lookup"><span data-stu-id="70df8-207">**Role**</span></span>       | <span data-ttu-id="70df8-208">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-208">Yes</span></span>         | <span data-ttu-id="70df8-209">Stejná hodnota jako **Role** atribut (Pokud se používá) odpovídajícího prvku End; v opačném případě se název tabulky, která obsahuje objekt odkazujícího sloupce.</span><span class="sxs-lookup"><span data-stu-id="70df8-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-210">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **závislé** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="70df8-211">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="70df8-212">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-213">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-213">Example</span></span>

<span data-ttu-id="70df8-214">Následující příklad ukazuje element přidružení, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders** cizí klíč omezení.</span><span class="sxs-lookup"><span data-stu-id="70df8-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="70df8-215">**Závislé** určuje element **CustomerId** sloupec **pořadí** tabulky jako závislé konec omezení.</span><span class="sxs-lookup"><span data-stu-id="70df8-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="70df8-216">Element Documentation (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="70df8-217">**Dokumentaci** prvek store schema definition language (SSDL) slouží k zadání informací o objektu, který je definován v nadřazeném elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="70df8-218">**Dokumentaci** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-219">**Souhrn**: stručný popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="70df8-220">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-220">(zero or one element)</span></span>
-   <span data-ttu-id="70df8-221">**LongDescription**: rozsáhlý popis nadřazeného elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="70df8-222">(žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-223">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-223">Applicable Attributes</span></span>

<span data-ttu-id="70df8-224">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **dokumentaci** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="70df8-225">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="70df8-226">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-227">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-227">Example</span></span>

<span data-ttu-id="70df8-228">Následující příklad ukazuje **dokumentaci** element jako podřízený prvek elementu EntityType.</span><span class="sxs-lookup"><span data-stu-id="70df8-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="70df8-229">Element end (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-229">End Element (SSDL)</span></span>

<span data-ttu-id="70df8-230">**End** element v store schema definition language (SSDL) Určuje tabulku a počet řádků na jednom konci omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="70df8-231">**End** element může být podřízeným prvkem elementu Association elementu nebo elementu AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="70df8-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="70df8-232">V každém případě je to možné podřízené prvky a příslušné atributy se liší.</span><span class="sxs-lookup"><span data-stu-id="70df8-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="70df8-233">Koncový Element jako podřízený Element přidružení</span><span class="sxs-lookup"><span data-stu-id="70df8-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="70df8-234">**End** – element (jako podřízený objekt **přidružení** element) Určuje tabulku a počet řádků na konci omezení cizího klíče s **typ** a **Násobnost** atributy v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="70df8-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="70df8-235">Konce omezení cizího klíče jsou definované jako součást přidružení souborů SSDL; přidružení souborů SSDL musí mít přesně dva elementy.</span><span class="sxs-lookup"><span data-stu-id="70df8-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="70df8-236">**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-237">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="70df8-238">Elementy OnDelete (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="70df8-239">Elementů poznámky (nula nebo více prvků)</span><span class="sxs-lookup"><span data-stu-id="70df8-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="70df8-240">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-240">Applicable Attributes</span></span>

<span data-ttu-id="70df8-241">Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="70df8-242">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-242">Attribute Name</span></span>   | <span data-ttu-id="70df8-243">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-243">Is Required</span></span> | <span data-ttu-id="70df8-244">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-245">**Typ**</span><span class="sxs-lookup"><span data-stu-id="70df8-245">**Type**</span></span>         | <span data-ttu-id="70df8-246">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-246">Yes</span></span>         | <span data-ttu-id="70df8-247">Plně kvalifikovaný název sady entit SSDL, který je na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="70df8-248">**Role**</span><span class="sxs-lookup"><span data-stu-id="70df8-248">**Role**</span></span>         | <span data-ttu-id="70df8-249">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-249">No</span></span>          | <span data-ttu-id="70df8-250">Hodnota **Role** atribut v elementu objektu nebo závislé na odpovídající element v elementu ReferentialConstraint (Pokud se používá).</span><span class="sxs-lookup"><span data-stu-id="70df8-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="70df8-251">**Násobnost**</span><span class="sxs-lookup"><span data-stu-id="70df8-251">**Multiplicity**</span></span> | <span data-ttu-id="70df8-252">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-252">Yes</span></span>         | <span data-ttu-id="70df8-253">**1**, **0..1**, nebo **\*** v závislosti na počtu řádků, které mohou být na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="70df8-254">**1** označuje, že přesně jeden řádek existuje na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="70df8-255">**0..1** znamená, že neexistuje žádný nebo jeden řádek na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="70df8-256">**\*** Udává, že žádný, jeden nebo více řádků existuje na straně cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-257">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="70df8-258">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="70df8-259">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="70df8-260">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-260">Example</span></span>

<span data-ttu-id="70df8-261">Následující příklad ukazuje **přidružení** element, který definuje **FK\_CustomerOrders** omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="70df8-262">**Násobnost** hodnoty zadané v každém **koncové** označení daný počet řádků v elementu **objednávky** můžou být spojené s řádek v tabulce **zákazníkům**  tabulky, ale pouze jeden řádek v **zákazníkům** můžou být spojené s řádek v tabulce **objednávky** tabulky.</span><span class="sxs-lookup"><span data-stu-id="70df8-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="70df8-263">Kromě toho **elementy OnDelete** element označuje, že všechny řádky v **objednávky** tabulku, která odkazují na konkrétní řádek v **zákazníkům** tabulka bude odstraněno odpovídající řádek v **zákazníkům** tabulce se odstraní.</span><span class="sxs-lookup"><span data-stu-id="70df8-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="70df8-264">Koncový Element jako podřízený AssociationSet Element</span><span class="sxs-lookup"><span data-stu-id="70df8-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="70df8-265">**End** – element (jako podřízený objekt **AssociationSet** element) Určuje tabulku na jednom konci omezení cizího klíče v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="70df8-266">**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-267">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-268">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="70df8-269">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-269">Applicable Attributes</span></span>

<span data-ttu-id="70df8-270">Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **AssociationSet** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="70df8-271">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-271">Attribute Name</span></span> | <span data-ttu-id="70df8-272">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-272">Is Required</span></span> | <span data-ttu-id="70df8-273">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-274">**Objekt EntitySet**</span><span class="sxs-lookup"><span data-stu-id="70df8-274">**EntitySet**</span></span>  | <span data-ttu-id="70df8-275">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-275">Yes</span></span>         | <span data-ttu-id="70df8-276">Název sady entit SSDL, který je na konci omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="70df8-277">**Role**</span><span class="sxs-lookup"><span data-stu-id="70df8-277">**Role**</span></span>       | <span data-ttu-id="70df8-278">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-278">No</span></span>          | <span data-ttu-id="70df8-279">Hodnota jednoho z **Role** atributy určené v jednom **End** odpovídajícího elementu Association elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-280">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="70df8-281">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="70df8-282">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="70df8-283">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-283">Example</span></span>

<span data-ttu-id="70df8-284">Následující příklad ukazuje **EntityContainer** element s **AssociationSet** element se dvěma **koncové** prvky:</span><span class="sxs-lookup"><span data-stu-id="70df8-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="70df8-285">Element EntityContainer (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="70df8-286">**EntityContainer** prvek store schema definition language (SSDL) popisuje strukturu podkladového zdroje dat v aplikaci Entity Framework: sady entit SSDL (definováno v elementů EntitySet nemá) představují tabulky v databáze, typy entit SSDL (definované v objektu EntityType elementy) představují řádky v tabulce a sad přidružení (definované elementy AssociationSet) představují omezení cizího klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="70df8-287">Kontejner úložiště modelu entity se mapuje na kontejner koncepčního modelu entity pomocí elementu EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="70df8-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="70df8-288">**EntityContainer** element může mít nejvýše jedno dokumentace prvků.</span><span class="sxs-lookup"><span data-stu-id="70df8-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="70df8-289">Pokud **dokumentaci** element je k dispozici, se musí předcházet všem ostatním prvkům podřízené.</span><span class="sxs-lookup"><span data-stu-id="70df8-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="70df8-290">**EntityContainer** prvek může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-291">Objekt EntitySet</span><span class="sxs-lookup"><span data-stu-id="70df8-291">EntitySet</span></span>
-   <span data-ttu-id="70df8-292">Element AssociationSet</span><span class="sxs-lookup"><span data-stu-id="70df8-292">AssociationSet</span></span>
-   <span data-ttu-id="70df8-293">Elementů poznámky</span><span class="sxs-lookup"><span data-stu-id="70df8-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-294">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-294">Applicable Attributes</span></span>

<span data-ttu-id="70df8-295">Následující tabulka popisuje atributy, které mohou být použity **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="70df8-296">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-296">Attribute Name</span></span> | <span data-ttu-id="70df8-297">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-297">Is Required</span></span> | <span data-ttu-id="70df8-298">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="70df8-299">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-299">**Name**</span></span>       | <span data-ttu-id="70df8-300">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-300">Yes</span></span>         | <span data-ttu-id="70df8-301">Název kontejneru entity.</span><span class="sxs-lookup"><span data-stu-id="70df8-301">The name of the entity container.</span></span> <span data-ttu-id="70df8-302">Tento název nesmí obsahovat tečky (.).</span><span class="sxs-lookup"><span data-stu-id="70df8-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-303">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityContainer** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="70df8-304">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-305">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-306">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-306">Example</span></span>

<span data-ttu-id="70df8-307">Následující příklad ukazuje **EntityContainer** element, který definuje dvě sady entit a přidružení jedné sady.</span><span class="sxs-lookup"><span data-stu-id="70df8-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="70df8-308">Všimněte si, že jsou názvy entit typu a přidružení typu kvalifikován názvem oboru názvů koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="70df8-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="70df8-309">Element EntitySet (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="70df8-310">**Objektu EntitySet** prvek v store schema definition language (SSDL) představuje tabulku nebo zobrazení v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="70df8-311">Element EntityType v SSDL představuje řádek v tabulce nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="70df8-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="70df8-312">**EntityType** atribut **objektu EntitySet** prvek určuje konkrétní typ entity SSDL, který představuje řádků v SSDL sadu entit.</span><span class="sxs-lookup"><span data-stu-id="70df8-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="70df8-313">V elementu EntitySetMapping je určeno mapování mezi sadu entit CSDL a sadu entit SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="70df8-314">**Objektu EntitySet** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-315">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="70df8-316">DefiningQuery (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="70df8-317">Elementů poznámky</span><span class="sxs-lookup"><span data-stu-id="70df8-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-318">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-318">Applicable Attributes</span></span>

<span data-ttu-id="70df8-319">Následující tabulka popisuje atributy, které mohou být použity **objektu EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="70df8-320">Některé atributy (tu nejsou uvedené) může být kvalifikován s **ukládání** alias.</span><span class="sxs-lookup"><span data-stu-id="70df8-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="70df8-321">Tyto atributy jsou používány v Průvodci modelů aktualizace při aktualizaci modelu.</span><span class="sxs-lookup"><span data-stu-id="70df8-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="70df8-322">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-322">Attribute Name</span></span> | <span data-ttu-id="70df8-323">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-323">Is Required</span></span> | <span data-ttu-id="70df8-324">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-325">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-325">**Name**</span></span>       | <span data-ttu-id="70df8-326">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-326">Yes</span></span>         | <span data-ttu-id="70df8-327">Název sady entit.</span><span class="sxs-lookup"><span data-stu-id="70df8-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="70df8-328">**Typ entity**</span><span class="sxs-lookup"><span data-stu-id="70df8-328">**EntityType**</span></span> | <span data-ttu-id="70df8-329">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-329">Yes</span></span>         | <span data-ttu-id="70df8-330">Plně kvalifikovaný název typu entity, pro kterou sada entit obsahuje instance.</span><span class="sxs-lookup"><span data-stu-id="70df8-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="70df8-331">**schéma**</span><span class="sxs-lookup"><span data-stu-id="70df8-331">**Schema**</span></span>     | <span data-ttu-id="70df8-332">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-332">No</span></span>          | <span data-ttu-id="70df8-333">Schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="70df8-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="70df8-334">**Tabulka**</span><span class="sxs-lookup"><span data-stu-id="70df8-334">**Table**</span></span>      | <span data-ttu-id="70df8-335">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-335">No</span></span>          | <span data-ttu-id="70df8-336">Databázové tabulky.</span><span class="sxs-lookup"><span data-stu-id="70df8-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="70df8-337">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **objektu EntitySet** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="70df8-338">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-339">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-340">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-340">Example</span></span>

<span data-ttu-id="70df8-341">Následující příklad ukazuje **EntityContainer** element, který má dva **objektu EntitySet** elementy a jeden **AssociationSet** element:</span><span class="sxs-lookup"><span data-stu-id="70df8-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="70df8-342">Element EntityType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="70df8-343">**EntityType** prvek v store schema definition language (SSDL) představuje řádek v tabulce nebo zobrazení z databáze.</span><span class="sxs-lookup"><span data-stu-id="70df8-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="70df8-344">Element EntitySet v SSDL představuje tabulku nebo zobrazení, ve kterém dojde k řádků.</span><span class="sxs-lookup"><span data-stu-id="70df8-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="70df8-345">**EntityType** atribut **objektu EntitySet** prvek určuje konkrétní typ entity SSDL, který představuje řádků v SSDL sadu entit.</span><span class="sxs-lookup"><span data-stu-id="70df8-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="70df8-346">Mapování mezi typem entity SSDL a typ entity CSDL je zadán v mapování EntityTypeMapping elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="70df8-347">**EntityType** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-348">Dokumentace ke službě (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="70df8-349">Klíč (žádný nebo jeden element)</span><span class="sxs-lookup"><span data-stu-id="70df8-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="70df8-350">Elementů poznámky</span><span class="sxs-lookup"><span data-stu-id="70df8-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-351">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-351">Applicable Attributes</span></span>

<span data-ttu-id="70df8-352">Následující tabulka popisuje atributy, které mohou být použity **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="70df8-353">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-353">Attribute Name</span></span> | <span data-ttu-id="70df8-354">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-354">Is Required</span></span> | <span data-ttu-id="70df8-355">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-356">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-356">**Name**</span></span>       | <span data-ttu-id="70df8-357">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-357">Yes</span></span>         | <span data-ttu-id="70df8-358">Název typu entity.</span><span class="sxs-lookup"><span data-stu-id="70df8-358">The name of the entity type.</span></span> <span data-ttu-id="70df8-359">Tato hodnota je obvykle stejný jako název tabulky, ve které představuje typ entity řádek.</span><span class="sxs-lookup"><span data-stu-id="70df8-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="70df8-360">Tato hodnota může obsahovat žádné tečky (.).</span><span class="sxs-lookup"><span data-stu-id="70df8-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-361">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="70df8-362">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-363">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-364">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-364">Example</span></span>

<span data-ttu-id="70df8-365">Následující příklad ukazuje **EntityType** element s dvě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="70df8-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="70df8-366">Element Function (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-366">Function Element (SSDL)</span></span>

<span data-ttu-id="70df8-367">**Funkce** element v store schema definition language (SSDL) Určuje uloženou proceduru, která existuje v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="70df8-368">**Funkce** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-369">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-370">Parametr (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="70df8-371">Atribut CommandText (nula nebo jedna)</span><span class="sxs-lookup"><span data-stu-id="70df8-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="70df8-372">Vlastnost ReturnType (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="70df8-373">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="70df8-374">Návratová typ pro funkci musí být určen buď **ReturnType** element nebo **ReturnType** atribut (viz níže), ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="70df8-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="70df8-375">Uložené procedury, které jsou určené v modelu úložiště lze importovat do koncepčního modelu aplikace.</span><span class="sxs-lookup"><span data-stu-id="70df8-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="70df8-376">Další informace najdete v tématu [dotazování pomocí uložené procedury](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="70df8-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="70df8-377">**Funkce** element můžete také použít k definování vlastních funkcí v rámci modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-378">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-378">Applicable Attributes</span></span>

<span data-ttu-id="70df8-379">Následující tabulka popisuje atributy, které mohou být použity **funkce** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="70df8-380">Některé atributy (tu nejsou uvedené) může být kvalifikován s **ukládání** alias.</span><span class="sxs-lookup"><span data-stu-id="70df8-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="70df8-381">Tyto atributy jsou používány v Průvodci modelů aktualizace při aktualizaci modelu.</span><span class="sxs-lookup"><span data-stu-id="70df8-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="70df8-382">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-382">Attribute Name</span></span>             | <span data-ttu-id="70df8-383">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-383">Is Required</span></span> | <span data-ttu-id="70df8-384">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-385">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-385">**Name**</span></span>                   | <span data-ttu-id="70df8-386">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-386">Yes</span></span>         | <span data-ttu-id="70df8-387">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="70df8-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="70df8-388">**Vlastnost ReturnType**</span><span class="sxs-lookup"><span data-stu-id="70df8-388">**ReturnType**</span></span>             | <span data-ttu-id="70df8-389">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-389">No</span></span>          | <span data-ttu-id="70df8-390">Návratový typ uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="70df8-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="70df8-391">**Agregace**</span><span class="sxs-lookup"><span data-stu-id="70df8-391">**Aggregate**</span></span>              | <span data-ttu-id="70df8-392">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-392">No</span></span>          | <span data-ttu-id="70df8-393">**Hodnota TRUE** Pokud bude procedura vracet agregovanou hodnotu; jinak **False**.</span><span class="sxs-lookup"><span data-stu-id="70df8-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="70df8-394">**Předdefinované**</span><span class="sxs-lookup"><span data-stu-id="70df8-394">**BuiltIn**</span></span>                | <span data-ttu-id="70df8-395">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-395">No</span></span>          | <span data-ttu-id="70df8-396">**Hodnota TRUE** Pokud je integrovaná funkce<sup>1</sup> funkce; v opačném případě **False**.</span><span class="sxs-lookup"><span data-stu-id="70df8-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="70df8-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="70df8-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="70df8-398">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-398">No</span></span>          | <span data-ttu-id="70df8-399">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="70df8-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="70df8-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="70df8-400">**NiladicFunction**</span></span>        | <span data-ttu-id="70df8-401">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-401">No</span></span>          | <span data-ttu-id="70df8-402">**Hodnota TRUE** Pokud je funkce bez vstupních parametrů<sup>2</sup> funkce; **False** jinak.</span><span class="sxs-lookup"><span data-stu-id="70df8-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="70df8-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="70df8-403">**IsComposable**</span></span>           | <span data-ttu-id="70df8-404">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-404">No</span></span>          | <span data-ttu-id="70df8-405">**Hodnota TRUE** Pokud je funkce možností složení<sup>3</sup> funkce; **False** jinak.</span><span class="sxs-lookup"><span data-stu-id="70df8-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="70df8-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="70df8-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="70df8-407">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-407">No</span></span>          | <span data-ttu-id="70df8-408">Výčet, který definuje typ sémantice použité vyřešit přetížení funkce.</span><span class="sxs-lookup"><span data-stu-id="70df8-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="70df8-409">Výčet je definovaný v manifestu zprostředkovatele za definici funkce.</span><span class="sxs-lookup"><span data-stu-id="70df8-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="70df8-410">Výchozí hodnota je **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="70df8-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="70df8-411">**schéma**</span><span class="sxs-lookup"><span data-stu-id="70df8-411">**Schema**</span></span>                 | <span data-ttu-id="70df8-412">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-412">No</span></span>          | <span data-ttu-id="70df8-413">Název schématu, ve kterém je definována uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="70df8-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="70df8-414"><sup>1</sup> integrované funkce je funkce, která je definována v databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="70df8-415">Informace o funkcích, které jsou definovány v modelu úložiště najdete v tématu Element CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="70df8-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="70df8-416"><sup>2</sup> bez vstupních parametrů funkce je funkce, která nepřijímá žádné parametry a při volání, nevyžaduje závorky.</span><span class="sxs-lookup"><span data-stu-id="70df8-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="70df8-417"><sup>3</sup> dvě funkce jsou složení, pokud výstup jedné funkce může být vstup pro jiné funkce.</span><span class="sxs-lookup"><span data-stu-id="70df8-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="70df8-418">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **funkce** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="70df8-419">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-420">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-421">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-421">Example</span></span>

<span data-ttu-id="70df8-422">Následující příklad ukazuje **funkce** element, který odpovídá **UpdateOrderQuantity** uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="70df8-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="70df8-423">Uložené procedury přijímá dva parametry a vracet hodnotu.</span><span class="sxs-lookup"><span data-stu-id="70df8-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="70df8-424">Klíčovým prvkem (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-424">Key Element (SSDL)</span></span>

<span data-ttu-id="70df8-425">**Klíč** prvek v store schema definition language (SSDL) představuje primární klíč tabulky v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="70df8-426">**Klíč** je podřízeným prvkem elementu EntityType, který představuje řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="70df8-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="70df8-427">Primární klíč je definována v **klíč** element pomocí odkazu na jeden nebo více vlastností prvků, které jsou definovány na **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="70df8-428">**Klíč** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-429">PropertyRef (jeden nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="70df8-430">Elementů poznámky</span><span class="sxs-lookup"><span data-stu-id="70df8-430">Annotation elements</span></span>

<span data-ttu-id="70df8-431">Žádné atributy se vztahují na **klíč** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-432">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-432">Example</span></span>

<span data-ttu-id="70df8-433">Následující příklad ukazuje **EntityType** element s klíčem, který odkazuje na jednu vlastnost:</span><span class="sxs-lookup"><span data-stu-id="70df8-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="70df8-434">Elementy OnDelete – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="70df8-435">**Elementy OnDelete** prvek store schema definition language (SSDL) odráží databáze chování při odstranění řádku, který se podílí na omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="70df8-436">Pokud je akce nastavená **Cascade**, pak budou odstraněny také řádky, které odkazují na řádek, který se právě odstraňuje.</span><span class="sxs-lookup"><span data-stu-id="70df8-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="70df8-437">Pokud je akce nastavená **žádný**, pak se odstraní také řádky, které odkazují na řádek, který se právě odstraňuje.</span><span class="sxs-lookup"><span data-stu-id="70df8-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="70df8-438">**Elementy OnDelete** element je podřízeným prvkem elementu End.</span><span class="sxs-lookup"><span data-stu-id="70df8-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="70df8-439">**Elementy OnDelete** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-440">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-441">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-442">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-442">Applicable Attributes</span></span>

<span data-ttu-id="70df8-443">Následující tabulka popisuje atributy, které mohou být použity **elementy OnDelete** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="70df8-444">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-444">Attribute Name</span></span> | <span data-ttu-id="70df8-445">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-445">Is Required</span></span> | <span data-ttu-id="70df8-446">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-447">**Akce**</span><span class="sxs-lookup"><span data-stu-id="70df8-447">**Action**</span></span>     | <span data-ttu-id="70df8-448">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-448">Yes</span></span>         | <span data-ttu-id="70df8-449">**Kaskádové** nebo **žádný**.</span><span class="sxs-lookup"><span data-stu-id="70df8-449">**Cascade** or **None**.</span></span> <span data-ttu-id="70df8-450">(Hodnota **s omezeným přístupem** je platný, ale má stejné chování jako **žádný**.)</span><span class="sxs-lookup"><span data-stu-id="70df8-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-451">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **elementy OnDelete** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="70df8-452">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-453">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-454">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-454">Example</span></span>

<span data-ttu-id="70df8-455">Následující příklad ukazuje **přidružení** element, který definuje **FK\_CustomerOrders** omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="70df8-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="70df8-456">**Elementy OnDelete** element označuje, že všechny řádky v **objednávky** tabulku, která odkazují na konkrétní řádek v **zákazníkům** tabulka bude odstraněno řádku **Zákazníkům** tabulce se odstraní.</span><span class="sxs-lookup"><span data-stu-id="70df8-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="70df8-457">Parameter – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="70df8-458">**Parametr** prvek store schema definition language (SSDL) je podřízeným prvkem elementu funkce, která určuje parametry pro uloženou proceduru v databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="70df8-459">**Parametr** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-460">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-461">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-462">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-462">Applicable Attributes</span></span>

<span data-ttu-id="70df8-463">Následující tabulka popisuje atributy, které mohou být použity **parametr** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="70df8-464">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-464">Attribute Name</span></span> | <span data-ttu-id="70df8-465">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-465">Is Required</span></span> | <span data-ttu-id="70df8-466">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-467">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-467">**Name**</span></span>       | <span data-ttu-id="70df8-468">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-468">Yes</span></span>         | <span data-ttu-id="70df8-469">Název parametru</span><span class="sxs-lookup"><span data-stu-id="70df8-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="70df8-470">**Typ**</span><span class="sxs-lookup"><span data-stu-id="70df8-470">**Type**</span></span>       | <span data-ttu-id="70df8-471">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-471">Yes</span></span>         | <span data-ttu-id="70df8-472">Typ parametru.</span><span class="sxs-lookup"><span data-stu-id="70df8-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="70df8-473">**Režim**</span><span class="sxs-lookup"><span data-stu-id="70df8-473">**Mode**</span></span>       | <span data-ttu-id="70df8-474">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-474">No</span></span>          | <span data-ttu-id="70df8-475">**V**, **si**, nebo **InOut** v závislosti na tom, zda je parametr vstup, výstup nebo vstupně výstupní parametr.</span><span class="sxs-lookup"><span data-stu-id="70df8-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="70df8-476">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="70df8-476">**MaxLength**</span></span>  | <span data-ttu-id="70df8-477">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-477">No</span></span>          | <span data-ttu-id="70df8-478">Maximální délka parametru.</span><span class="sxs-lookup"><span data-stu-id="70df8-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="70df8-479">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="70df8-479">**Precision**</span></span>  | <span data-ttu-id="70df8-480">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-480">No</span></span>          | <span data-ttu-id="70df8-481">Přesnost parametru.</span><span class="sxs-lookup"><span data-stu-id="70df8-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="70df8-482">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="70df8-482">**Scale**</span></span>      | <span data-ttu-id="70df8-483">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-483">No</span></span>          | <span data-ttu-id="70df8-484">Měřítko parametru.</span><span class="sxs-lookup"><span data-stu-id="70df8-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="70df8-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="70df8-485">**SRID**</span></span>       | <span data-ttu-id="70df8-486">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-486">No</span></span>          | <span data-ttu-id="70df8-487">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="70df8-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="70df8-488">Platí jenom pro parametry prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="70df8-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="70df8-489">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="70df8-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-490">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **parametr** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="70df8-491">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-492">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-493">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-493">Example</span></span>

<span data-ttu-id="70df8-494">Následující příklad ukazuje **funkce** element, který má dva **parametr** elementy, které určují vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="70df8-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="70df8-495">Instanční objekt – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="70df8-496">**Hlavní** prvek store schema definition language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje hlavní konec omezení cizího klíče (tzv. referenční omezení).</span><span class="sxs-lookup"><span data-stu-id="70df8-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="70df8-497">**Hlavní** prvek určuje sloupec primárního klíče (nebo sloupce) v tabulce, na který odkazuje jiný sloupec (nebo sloupce).</span><span class="sxs-lookup"><span data-stu-id="70df8-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="70df8-498">**PropertyRef** určují prvky sloupce, které jsou odkazovány.</span><span class="sxs-lookup"><span data-stu-id="70df8-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="70df8-499">Určuje sloupce, které odkazují na sloupce primárního klíče, které jsou určené v elementu závislé **instančního objektu** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="70df8-500">**Hlavní** prvek může mít následující podřízené prvky (v uvedeném pořadí):</span><span class="sxs-lookup"><span data-stu-id="70df8-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="70df8-501">PropertyRef (jeden nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="70df8-502">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-503">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-503">Applicable Attributes</span></span>

<span data-ttu-id="70df8-504">Následující tabulka popisuje atributy, které mohou být použity **hlavní** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="70df8-505">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-505">Attribute Name</span></span> | <span data-ttu-id="70df8-506">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-506">Is Required</span></span> | <span data-ttu-id="70df8-507">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-508">**Role**</span><span class="sxs-lookup"><span data-stu-id="70df8-508">**Role**</span></span>       | <span data-ttu-id="70df8-509">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-509">Yes</span></span>         | <span data-ttu-id="70df8-510">Stejná hodnota jako **Role** atribut (Pokud se používá) odpovídajícího prvku End; v opačném případě se název tabulky, který obsahuje Odkazovaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-511">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **hlavní** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="70df8-512">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="70df8-513">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-514">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-514">Example</span></span>

<span data-ttu-id="70df8-515">Následující příklad ukazuje element přidružení, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders** cizí klíč omezení.</span><span class="sxs-lookup"><span data-stu-id="70df8-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="70df8-516">**Hlavní** určuje element **CustomerId** sloupec **zákazníka** tabulku jako hlavní konec omezení.</span><span class="sxs-lookup"><span data-stu-id="70df8-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="70df8-517">Property – Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-517">Property Element (SSDL)</span></span>

<span data-ttu-id="70df8-518">**Vlastnost** prvek v store schema definition language (SSDL) představuje sloupci v tabulce v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="70df8-519">**Vlastnost** prvky jsou podřízených elementů EntityType, které představují řádky v tabulce.</span><span class="sxs-lookup"><span data-stu-id="70df8-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="70df8-520">Každý **vlastnost** element definovaný v **EntityType** element představuje sloupci.</span><span class="sxs-lookup"><span data-stu-id="70df8-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="70df8-521">A **vlastnost** element nemůže mít žádné podřízené prvky.</span><span class="sxs-lookup"><span data-stu-id="70df8-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-522">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-522">Applicable Attributes</span></span>

<span data-ttu-id="70df8-523">Následující tabulka popisuje atributy, které mohou být použity **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="70df8-524">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-524">Attribute Name</span></span>            | <span data-ttu-id="70df8-525">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-525">Is Required</span></span> | <span data-ttu-id="70df8-526">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-527">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-527">**Name**</span></span>                  | <span data-ttu-id="70df8-528">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-528">Yes</span></span>         | <span data-ttu-id="70df8-529">Název odpovídající sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="70df8-530">**Typ**</span><span class="sxs-lookup"><span data-stu-id="70df8-530">**Type**</span></span>                  | <span data-ttu-id="70df8-531">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-531">Yes</span></span>         | <span data-ttu-id="70df8-532">Typ odpovídající sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="70df8-533">**S povolenou hodnotou Null**</span><span class="sxs-lookup"><span data-stu-id="70df8-533">**Nullable**</span></span>              | <span data-ttu-id="70df8-534">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-534">No</span></span>          | <span data-ttu-id="70df8-535">**Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda odpovídající sloupec může mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="70df8-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="70df8-536">**Výchozí hodnota**</span><span class="sxs-lookup"><span data-stu-id="70df8-536">**DefaultValue**</span></span>          | <span data-ttu-id="70df8-537">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-537">No</span></span>          | <span data-ttu-id="70df8-538">Výchozí hodnota odpovídající sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="70df8-539">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="70df8-539">**MaxLength**</span></span>             | <span data-ttu-id="70df8-540">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-540">No</span></span>          | <span data-ttu-id="70df8-541">Maximální délka odpovídající sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="70df8-542">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="70df8-542">**FixedLength**</span></span>           | <span data-ttu-id="70df8-543">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-543">No</span></span>          | <span data-ttu-id="70df8-544">**Hodnota TRUE** nebo **False** v závislosti na tom, zda se uloží odpovídající hodnotu sloupce jako řetězce pevné délky.</span><span class="sxs-lookup"><span data-stu-id="70df8-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="70df8-545">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="70df8-545">**Precision**</span></span>             | <span data-ttu-id="70df8-546">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-546">No</span></span>          | <span data-ttu-id="70df8-547">Přesnost odpovídající sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="70df8-548">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="70df8-548">**Scale**</span></span>                 | <span data-ttu-id="70df8-549">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-549">No</span></span>          | <span data-ttu-id="70df8-550">Škálování odpovídající sloupec.</span><span class="sxs-lookup"><span data-stu-id="70df8-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="70df8-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="70df8-551">**Unicode**</span></span>               | <span data-ttu-id="70df8-552">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-552">No</span></span>          | <span data-ttu-id="70df8-553">**Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží odpovídající hodnotu sloupce jako řetězec s kódováním Unicode.</span><span class="sxs-lookup"><span data-stu-id="70df8-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="70df8-554">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="70df8-554">**Collation**</span></span>             | <span data-ttu-id="70df8-555">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-555">No</span></span>          | <span data-ttu-id="70df8-556">Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="70df8-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="70df8-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="70df8-557">**SRID**</span></span>                  | <span data-ttu-id="70df8-558">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-558">No</span></span>          | <span data-ttu-id="70df8-559">Odkaz na identifikátor spatial systému.</span><span class="sxs-lookup"><span data-stu-id="70df8-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="70df8-560">Platí pouze pro vlastnosti prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="70df8-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="70df8-561">Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="70df8-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="70df8-562">**Výčet StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="70df8-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="70df8-563">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-563">No</span></span>          | <span data-ttu-id="70df8-564">**Žádný**, **Identity** (Pokud je hodnota odpovídající sloupec identity, který je vygenerován v databázi), nebo **vypočítané** (Pokud odpovídající sloupec hodnota byla vypočítána v databázi).</span><span class="sxs-lookup"><span data-stu-id="70df8-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="70df8-565">Není platné pro RowType vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="70df8-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-566">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **vlastnost** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="70df8-567">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-568">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-569">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-569">Example</span></span>

<span data-ttu-id="70df8-570">Následující příklad ukazuje **EntityType** element s dva podřízené **vlastnost** prvky:</span><span class="sxs-lookup"><span data-stu-id="70df8-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="70df8-571">Element PropertyRef (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="70df8-572">**PropertyRef** prvek store schema definition language (SSDL) odkazuje na vlastnost definovaný v elementu EntityType k označení, že vlastnost bude provádět jednu z následujících rolí:</span><span class="sxs-lookup"><span data-stu-id="70df8-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="70df8-573">Být součástí primárního klíče tabulky, který **EntityType** představuje.</span><span class="sxs-lookup"><span data-stu-id="70df8-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="70df8-574">Jeden nebo více **PropertyRef** prvky můžete použít k definování primární klíč.</span><span class="sxs-lookup"><span data-stu-id="70df8-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="70df8-575">Další informace najdete v elementu Key.</span><span class="sxs-lookup"><span data-stu-id="70df8-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="70df8-576">Být závislé nebo hlavní konec referenčního omezení.</span><span class="sxs-lookup"><span data-stu-id="70df8-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="70df8-577">Další informace najdete v elementu ReferentialConstraint elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="70df8-578">**PropertyRef** element může obsahovat pouze následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="70df8-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="70df8-579">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-580">Elementů poznámky</span><span class="sxs-lookup"><span data-stu-id="70df8-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-581">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-581">Applicable Attributes</span></span>

<span data-ttu-id="70df8-582">Následující tabulka popisuje atributy, které mohou být použity **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="70df8-583">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-583">Attribute Name</span></span> | <span data-ttu-id="70df8-584">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-584">Is Required</span></span> | <span data-ttu-id="70df8-585">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="70df8-586">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="70df8-586">**Name**</span></span>       | <span data-ttu-id="70df8-587">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-587">Yes</span></span>         | <span data-ttu-id="70df8-588">Název odkazované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="70df8-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="70df8-589">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **PropertyRef** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="70df8-590">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="70df8-591">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-592">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-592">Example</span></span>

<span data-ttu-id="70df8-593">Následující příklad ukazuje **PropertyRef** element sloužící k odkazování na vlastnost, která je definována na definovat primární klíč **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="70df8-594">Element elementu ReferentialConstraint (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="70df8-595">**Elementu ReferentialConstraint** prvek v store schema definition language (SSDL) představuje omezení cizího klíče (také nazývané omezení referenční integrity) v podkladové databázi.</span><span class="sxs-lookup"><span data-stu-id="70df8-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="70df8-596">Objektem zabezpečení a závislými konce omezení jsou určena pomocí podřízených prvků objektu zabezpečení a závislé.</span><span class="sxs-lookup"><span data-stu-id="70df8-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="70df8-597">Sloupce, které jsou součástí objektem zabezpečení a závislými elementy end jsou odkazovány pomocí PropertyRef elementy.</span><span class="sxs-lookup"><span data-stu-id="70df8-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="70df8-598">**Elementu ReferentialConstraint** element je volitelný podřízený prvek elementu Association.</span><span class="sxs-lookup"><span data-stu-id="70df8-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="70df8-599">Pokud **elementu ReferentialConstraint** element není slouží k omezení cizího klíče, který je zadán v mapování **přidružení** element, AssociationSetMapping element musí k tomu použít.</span><span class="sxs-lookup"><span data-stu-id="70df8-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="70df8-600">**Elementu ReferentialConstraint** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="70df8-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="70df8-601">Dokumentace ke službě (žádný nebo jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="70df8-602">Objekt zabezpečení (právě jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="70df8-603">Závislé (právě jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="70df8-604">Elementů poznámky (nula nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-605">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-605">Applicable Attributes</span></span>

<span data-ttu-id="70df8-606">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **elementu ReferentialConstraint** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="70df8-607">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-608">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-609">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-609">Example</span></span>

<span data-ttu-id="70df8-610">Následující příklad ukazuje **přidružení** element, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders**  omezení cizího klíče:</span><span class="sxs-lookup"><span data-stu-id="70df8-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="70df8-611">Element ReturnType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="70df8-612">**ReturnType** element v store schema definition language (SSDL) určuje návratový typ pro funkci, která je definována v **funkce** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="70df8-613">Funkce, návratový typ je také možné zadat při **ReturnType** atribut.</span><span class="sxs-lookup"><span data-stu-id="70df8-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="70df8-614">Návratový typ funkce se zadaným **typ** atribut nebo **ReturnType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="70df8-615">**ReturnType** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="70df8-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="70df8-616">Objekt CollectionType (jeden)</span><span class="sxs-lookup"><span data-stu-id="70df8-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="70df8-617">Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReturnType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="70df8-618">Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="70df8-619">Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-620">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-620">Example</span></span>

<span data-ttu-id="70df8-621">Následující příklad používá **funkce** , který vrátí kolekce řádků.</span><span class="sxs-lookup"><span data-stu-id="70df8-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="70df8-622">Element RowType (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="70df8-623">A **RowType** prvek v store schema definition language (SSDL) definuje nepojmenovanou strukturu jako návratový typ pro funkci definovanou v úložišti.</span><span class="sxs-lookup"><span data-stu-id="70df8-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="70df8-624">A **RowType** prvek je podřízený prvek **CollectionType** element:</span><span class="sxs-lookup"><span data-stu-id="70df8-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="70df8-625">A **RowType** prvek může mít následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="70df8-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="70df8-626">Vlastnosti (jeden nebo více)</span><span class="sxs-lookup"><span data-stu-id="70df8-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="70df8-627">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-627">Example</span></span>

<span data-ttu-id="70df8-628">Následující příklad ukazuje funkci úložiště, který používá **CollectionType** elementu k určení, že funkce vrátí kolekce řádků (jak je uvedeno v **RowType** element).</span><span class="sxs-lookup"><span data-stu-id="70df8-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="70df8-629">Element schématu (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="70df8-630">**Schématu** prvek store schema definition language (SSDL) je kořenovým elementem definice modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="70df8-631">Obsahuje definice pro objekty, funkce a kontejnerů, které tvoří modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="70df8-632">**Schématu** element může obsahovat nula nebo více z následujících podřízených elementů:</span><span class="sxs-lookup"><span data-stu-id="70df8-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="70df8-633">Přidružení</span><span class="sxs-lookup"><span data-stu-id="70df8-633">Association</span></span>
-   <span data-ttu-id="70df8-634">Typ entity</span><span class="sxs-lookup"><span data-stu-id="70df8-634">EntityType</span></span>
-   <span data-ttu-id="70df8-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="70df8-635">EntityContainer</span></span>
-   <span data-ttu-id="70df8-636">Funkce</span><span class="sxs-lookup"><span data-stu-id="70df8-636">Function</span></span>

<span data-ttu-id="70df8-637">**Schématu** prvek používá **Namespace** atribut pro definování oboru názvů pro objekty typu a přidružení entit v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="70df8-638">V rámci oboru názvů může mít žádné dva objekty se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="70df8-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="70df8-639">Obor názvů modelu úložiště se liší od oboru názvů XML **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="70df8-640">Obor názvů modelu úložiště (podle definice **Namespace** atribut) je logický kontejner pro typy entit a přidružení typů.</span><span class="sxs-lookup"><span data-stu-id="70df8-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="70df8-641">Obor názvů XML (indikován **xmlns** atribut) z **schématu** element je výchozí obor názvů pro podřízené prvky a atributy **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="70df8-642">Obory názvů XML formuláře http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (kde RRRR a MM představují roku a měsíce v uvedeném pořadí) jsou vyhrazené pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="70df8-643">Vlastní elementy a atributy nemohou být v oborech názvů, které mají tento formulář.</span><span class="sxs-lookup"><span data-stu-id="70df8-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="70df8-644">Příslušné atributy</span><span class="sxs-lookup"><span data-stu-id="70df8-644">Applicable Attributes</span></span>

<span data-ttu-id="70df8-645">Následující tabulka popisuje atributy lze použít **schématu** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="70df8-646">Název atributu</span><span class="sxs-lookup"><span data-stu-id="70df8-646">Attribute Name</span></span>            | <span data-ttu-id="70df8-647">Vyžaduje se</span><span class="sxs-lookup"><span data-stu-id="70df8-647">Is Required</span></span> | <span data-ttu-id="70df8-648">Hodnota</span><span class="sxs-lookup"><span data-stu-id="70df8-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="70df8-649">**Namespace**</span></span>             | <span data-ttu-id="70df8-650">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-650">Yes</span></span>         | <span data-ttu-id="70df8-651">Obor názvů model úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-651">The namespace of the storage model.</span></span> <span data-ttu-id="70df8-652">Hodnota **Namespace** atribut se používá k vytvoření plně kvalifikovaný název typu.</span><span class="sxs-lookup"><span data-stu-id="70df8-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="70df8-653">Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů ExampleModel.Store pak plně kvalifikovaný název **EntityType** je ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="70df8-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="70df8-654">Následující řetězce nelze použít jako hodnotu **Namespace** atribut: **systému**, **přechodné**, nebo **Edm**.</span><span class="sxs-lookup"><span data-stu-id="70df8-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="70df8-655">Hodnota **Namespace** atribut nemůže být stejná jako hodnota pro **Namespace** atribut v elementu CSDL Schema.</span><span class="sxs-lookup"><span data-stu-id="70df8-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="70df8-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="70df8-656">**Alias**</span></span>                 | <span data-ttu-id="70df8-657">Ne</span><span class="sxs-lookup"><span data-stu-id="70df8-657">No</span></span>          | <span data-ttu-id="70df8-658">Identifikátor, použijí se místo názvu oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="70df8-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="70df8-659">Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů ExampleModel.Store a hodnoty **Alias** atribut je *StorageModel*, můžete použít StorageModel.Customer jako plně kvalifikovaný název **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="70df8-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="70df8-660">**Zprostředkovatel**</span><span class="sxs-lookup"><span data-stu-id="70df8-660">**Provider**</span></span>              | <span data-ttu-id="70df8-661">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-661">Yes</span></span>         | <span data-ttu-id="70df8-662">Zprostředkovatel dat.</span><span class="sxs-lookup"><span data-stu-id="70df8-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="70df8-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="70df8-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="70df8-664">Ano</span><span class="sxs-lookup"><span data-stu-id="70df8-664">Yes</span></span>         | <span data-ttu-id="70df8-665">Token, který označuje které manifest zprostředkovatele se vraťte k poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="70df8-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="70df8-666">Je definován žádný formát pro daný token.</span><span class="sxs-lookup"><span data-stu-id="70df8-666">No format for the token is defined.</span></span> <span data-ttu-id="70df8-667">Hodnoty pro daný token je definována v poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="70df8-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="70df8-668">Informace o manifestu tokeny zprostředkovatele SQL Server najdete v tématu SqlClient pro Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="70df8-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="70df8-669">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-669">Example</span></span>

<span data-ttu-id="70df8-670">Následující příklad ukazuje **schématu** element, který obsahuje **EntityContainer** elementu, dvěma **EntityType** elementy a jeden **přidružení** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="70df8-671">Atributy poznámek</span><span class="sxs-lookup"><span data-stu-id="70df8-671">Annotation Attributes</span></span>

<span data-ttu-id="70df8-672">Atributy poznámek v store schema definition language (SSDL) jsou vlastní atributy XML v modelu úložiště, které poskytují další metadata o prvky v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="70df8-673">Kromě s platnou strukturu XML, platí následující omezení atributů poznámky:</span><span class="sxs-lookup"><span data-stu-id="70df8-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="70df8-674">Atributy poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="70df8-675">Plně kvalifikovaných názvů atributů dvě poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="70df8-676">Více než jeden atribut poznámky se můžou vztahovat na daný prvek SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="70df8-677">Metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="70df8-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-678">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-678">Example</span></span>

<span data-ttu-id="70df8-679">Následující příklad ukazuje element EntityType, který má anotaci atributem použitým **OrderId** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="70df8-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="70df8-680">V příkladu také zobrazit poznámky prvek přidán do **EntityType** elementu.</span><span class="sxs-lookup"><span data-stu-id="70df8-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="70df8-681">Elementů poznámky (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="70df8-682">Poznámka prvky store schema definition language (SSDL) jsou vlastní elementů XML v modelu úložiště, které poskytují další metadata týkající se modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="70df8-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="70df8-683">Kromě platný struktura XML elementů poznámky platí následující omezení:</span><span class="sxs-lookup"><span data-stu-id="70df8-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="70df8-684">Elementů poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="70df8-685">Plně kvalifikovaných názvů všech elementů dvě poznámky nesmí být stejné.</span><span class="sxs-lookup"><span data-stu-id="70df8-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="70df8-686">Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky daného prvku SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="70df8-687">Více než jeden element anotace může být podřízeným daného prvku SSDL.</span><span class="sxs-lookup"><span data-stu-id="70df8-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="70df8-688">Od verze rozhraní .NET Framework verze 4, metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="70df8-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="70df8-689">Příklad</span><span class="sxs-lookup"><span data-stu-id="70df8-689">Example</span></span>

<span data-ttu-id="70df8-690">Následující příklad ukazuje element EntityType, který má element anotace (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="70df8-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="70df8-691">Tento příklad také ukazuje anotace atributem použitým **OrderId** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="70df8-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="70df8-692">Omezující vlastnosti (SSDL)</span><span class="sxs-lookup"><span data-stu-id="70df8-692">Facets (SSDL)</span></span>

<span data-ttu-id="70df8-693">Omezující vlastnosti v store schema definition language (SSDL) představují omezení na typy sloupců, které jsou uvedeny v prvcích vlastností.</span><span class="sxs-lookup"><span data-stu-id="70df8-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="70df8-694">Omezující vlastnosti jsou implementovány jako atributy ve formátu XML na **vlastnost** elementy.</span><span class="sxs-lookup"><span data-stu-id="70df8-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="70df8-695">Následující tabulka popisuje, které jsou podporovány v SSDL omezující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="70df8-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="70df8-696">omezující vlastnost</span><span class="sxs-lookup"><span data-stu-id="70df8-696">Facet</span></span>           | <span data-ttu-id="70df8-697">Popis</span><span class="sxs-lookup"><span data-stu-id="70df8-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70df8-698">**Kolace**</span><span class="sxs-lookup"><span data-stu-id="70df8-698">**Collation**</span></span>   | <span data-ttu-id="70df8-699">Určuje pořadí řazení (nebo pořadí řazení) pro použití při provádění porovnání a řazení operací na hodnotách vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="70df8-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="70df8-700">**Hodnoty**</span><span class="sxs-lookup"><span data-stu-id="70df8-700">**FixedLength**</span></span> | <span data-ttu-id="70df8-701">Určuje, zda se může lišit délka hodnoty sloupce.</span><span class="sxs-lookup"><span data-stu-id="70df8-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="70df8-702">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="70df8-702">**MaxLength**</span></span>   | <span data-ttu-id="70df8-703">Určuje maximální délku hodnoty sloupce.</span><span class="sxs-lookup"><span data-stu-id="70df8-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="70df8-704">**Přesnost**</span><span class="sxs-lookup"><span data-stu-id="70df8-704">**Precision**</span></span>   | <span data-ttu-id="70df8-705">Pro vlastnosti typu **desítkové**, určuje počet číslic, může mít hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="70df8-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="70df8-706">Pro vlastnosti typu **čas**, **data a času**, a **DateTimeOffset**, určuje počet číslic za desetinnou čárkou sady sekund hodnotu ve sloupci.</span><span class="sxs-lookup"><span data-stu-id="70df8-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="70df8-707">**Škálování**</span><span class="sxs-lookup"><span data-stu-id="70df8-707">**Scale**</span></span>       | <span data-ttu-id="70df8-708">Určuje počet číslic vpravo od desetinné čárky pro hodnotu sloupce.</span><span class="sxs-lookup"><span data-stu-id="70df8-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="70df8-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="70df8-709">**Unicode**</span></span>     | <span data-ttu-id="70df8-710">Určuje, zda hodnota sloupce ukládá jako kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="70df8-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
