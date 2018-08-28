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
# <a name="ssdl-specification"></a>Specifikace SSDL
Store schema definition language (SSDL) je jazyk založený na formátu XML, který popisuje model úložiště aplikace Entity Framework.

V aplikaci Entity Framework je načteno ze souboru ssdl (napsaného v SSDL) do instance System.Data.Metadata.Edm.StoreItemCollection metadat modelu úložiště a je přístupný pomocí metod v Třída System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework používá metadata modelu úložiště pro převod dotazy na koncepční model příkazů specifických pro úložiště.

Návrhář Entity Framework (EF designeru) ukládá informace o modelu úložiště v souboru .edmx v době návrhu. V návrháři entit v okamžiku sestavení používá informace v souboru .edmx k vytvoření souboru ssdl, která je potřeba pro Entity Framework za běhu.

Verze souborů SSDL, jsou rozlišené pomocí obory názvů XML.

| Verze souborů SSDL | Namespace XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Element Association (SSDL)

**Přidružení** element v store schema definition language (SSDL) určuje sloupce tabulky, které se účastní v omezení cizího klíče v podkladové databázi. Dva elementy End požadovaný podřízený zadejte tabulek na konci přidružení a násobnost na každém konci. Volitelný prvek elementu ReferentialConstraint určuje objektem zabezpečení a závislými konce přidružení, jakož i zúčastněných sloupců. Pokud ne **elementu ReferentialConstraint** element je k dispozici, AssociationSetMapping elementu musí použít k určení mapování sloupců pro přidružení.

**Přidružení** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden)
-   End (přesně dvě)
-   Elementu ReferentialConstraint (nula nebo jedna)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **přidružení** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název odpovídající omezení cizího klíče v podkladové databázi. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **přidružení** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders**  omezení cizího klíče:

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

## <a name="associationset-element-ssdl"></a>Element AssociationSet (SSDL)

**AssociationSet** prvek v store schema definition language (SSDL) představuje omezení cizího klíče mezi dvěma tabulkami v podkladové databázi. Sloupce tabulky, které jsou součástí omezení cizího klíče jsou určené v elementu Association. **Přidružení** element, který odpovídá daný **AssociationSet** prvek je zadán v **přidružení** atribut **AssociationSet**  elementu.

Nastaví přidružení souborů SSDL se mapují na sady přidružení CSDL prvkem AssociationSetMapping. Ale pokud přidružení CSDL pro danou CSDL přidružení se definuje pomocí prvek elementu ReferentialConstraint odpovídající **AssociationSetMapping** element je nezbytné. V takovém případě pokud **AssociationSetMapping** element je k dispozici, mapování, definuje, budou přepsaná identifikátorem **elementu ReferentialConstraint** elementu.

**AssociationSet** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden)
-   End (nula nebo dva)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **AssociationSet** elementu.

| Název atributu  | Vyžaduje se | Hodnota                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Jméno**        | Ano         | Název omezení cizího klíče, nastavte přidružení představuje.                          |
| **Přidružení** | Ano         | Název přidružení, které definuje sloupce, které se účastní v omezení cizího klíče. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **AssociationSet** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **AssociationSet** elementu, který představuje `FK_CustomerOrders` omezení cizího klíče v podkladové databázi:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>Element CollectionType (SSDL)

**CollectionType** element v store schema definition language (SSDL) určuje, že je návratový typ funkce kolekce. **CollectionType** element je podřízeným prvkem elementu ReturnType. Typ kolekce je určena pomocí podřízený element RowType:

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **CollectionType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci, která se používá **CollectionType** elementu k určení, že funkce vrátí kolekci řádků.

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

## <a name="commandtext-element-ssdl"></a>Element CommandText (SSDL)

**CommandText** prvek store schema definition language (SSDL) je podřízeným prvkem elementu funkce, který umožňuje definovat příkaz SQL, který je proveden v databázi. **CommandText** element slouží k přidání funkce, která se podobá uloženou proceduru v databázi, ale můžete definovat **CommandText** elementu v modelu úložiště.

**CommandText** element nemůže mít podřízené prvky. Text **CommandText** element musí být platný příkaz SQL pro podkladové databáze.

Žádné atributy se vztahují na **CommandText** elementu.

### <a name="example"></a>Příklad

Následující příklad ukazuje **funkce** element s podřízený **CommandText** elementu. Zveřejnit **UpdateProductInOrder** fungují jako metoda v rozhraní ObjectContext naimportujete do koncepčního modelu.  

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

## <a name="definingquery-element-ssdl"></a>Element DefiningQuery (SSDL)

**DefiningQuery** prvek store schema definition language (SSDL) umožňuje spouštění příkazu SQL přímo v podkladové databázi. **DefiningQuery** element se často používá jako zobrazení databáze, ale zobrazení je definováno v modelu úložiště, ne na databázi. Definováno v zobrazení **DefiningQuery** elementu je možné mapovat na typ entity v konceptuálním modelu prostřednictvím EntitySetMapping element. Tato mapování jsou jen pro čtení.  

Následující syntaxe SSDL znázorňuje deklaraci **objektu EntitySet** následovaný **DefiningQuery** element, který obsahuje dotaz použitý k načtení zobrazení.

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

Uložené procedury v Entity Framework slouží k povolení scénářů pro čtení a zápis přes zobrazení. Zobrazení zdroje dat nebo zobrazení Entity SQL můžete použít jako základní tabulky pro načítání dat a změna zpracování uložených procedur.

Můžete použít **DefiningQuery** element do cílového serveru Microsoft SQL Server Compact 3.5. I když SQL Server Compact 3.5 nepodporuje uložené procedury, podobné funkce s můžete implementovat **DefiningQuery** elementu. Jiné místo, kde může být užitečné je při vytváření uložených procedur k překonání Neshoda mezi datovými typy v programovacím jazyce a těch, které zdroj dat použít. Můžete napsat **DefiningQuery** , která používá sadu parametrů a pak volá uloženou proceduru s jinou sadu parametrů, třeba uloženou proceduru, která odstraní data.

## <a name="dependent-element-ssdl"></a>Závislé – Element (SSDL)

**Závislé** prvek store schema definition language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje závislé konec omezení cizího klíče (tzv. referenční omezení). **Závislé** prvek určuje sloupec (nebo sloupce) v tabulce, která odkazují na sloupec primárního klíče (nebo sloupce). **PropertyRef** určují prvky sloupce, které jsou odkazovány. Hlavní prvek určuje primární klíčové sloupce, které jsou odkazovány podle sloupců, které jsou určené v **závislé** elementu.

**Závislé** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   PropertyRef (jeden nebo více)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **závislé** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Role**       | Ano         | Stejná hodnota jako **Role** atribut (Pokud se používá) odpovídajícího prvku End; v opačném případě se název tabulky, která obsahuje objekt odkazujícího sloupce. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **závislé** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element přidružení, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders** cizí klíč omezení. **Závislé** určuje element **CustomerId** sloupec **pořadí** tabulky jako závislé konec omezení.

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

## <a name="documentation-element-ssdl"></a>Element Documentation (SSDL)

**Dokumentaci** prvek store schema definition language (SSDL) slouží k zadání informací o objektu, který je definován v nadřazeném elementu.

**Dokumentaci** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   **Souhrn**: stručný popis nadřazeného elementu. (žádný nebo jeden element)
-   **LongDescription**: rozsáhlý popis nadřazeného elementu. (žádný nebo jeden element)

### <a name="applicable-attributes"></a>Příslušné atributy

Libovolný počet atributů poznámky (vlastní atributy XML) lze na **dokumentaci** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **dokumentaci** element jako podřízený prvek elementu EntityType.

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

## <a name="end-element-ssdl"></a>Element end (SSDL)

**End** element v store schema definition language (SSDL) Určuje tabulku a počet řádků na jednom konci omezení cizího klíče v podkladové databázi. **End** element může být podřízeným prvkem elementu Association elementu nebo elementu AssociationSet. V každém případě je to možné podřízené prvky a příslušné atributy se liší.

### <a name="end-element-as-a-child-of-the-association-element"></a>Koncový Element jako podřízený Element přidružení

**End** – element (jako podřízený objekt **přidružení** element) Určuje tabulku a počet řádků na konci omezení cizího klíče s **typ** a **Násobnost** atributy v uvedeném pořadí. Konce omezení cizího klíče jsou definované jako součást přidružení souborů SSDL; přidružení souborů SSDL musí mít přesně dva elementy.

**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementy OnDelete (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **přidružení** elementu.

| Název atributu   | Vyžaduje se | Hodnota                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Ano         | Plně kvalifikovaný název sady entit SSDL, který je na konci omezení cizího klíče.                                                                                                                                                                                                                                                                                          |
| **Role**         | Ne          | Hodnota **Role** atribut v elementu objektu nebo závislé na odpovídající element v elementu ReferentialConstraint (Pokud se používá).                                                                                                                                                                                                                                             |
| **Násobnost** | Ano         | **1**, **0..1**, nebo **\*** v závislosti na počtu řádků, které mohou být na konci omezení cizího klíče. <br/> **1** označuje, že přesně jeden řádek existuje na konci omezení cizího klíče. <br/> **0..1** znamená, že neexistuje žádný nebo jeden řádek na konci omezení cizího klíče. <br/> **\*** Udává, že žádný, jeden nebo více řádků existuje na straně cizího klíče. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

#### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který definuje **FK\_CustomerOrders** omezení cizího klíče. **Násobnost** hodnoty zadané v každém **koncové** označení daný počet řádků v elementu **objednávky** můžou být spojené s řádek v tabulce **zákazníkům**  tabulky, ale pouze jeden řádek v **zákazníkům** můžou být spojené s řádek v tabulce **objednávky** tabulky. Kromě toho **elementy OnDelete** element označuje, že všechny řádky v **objednávky** tabulku, která odkazují na konkrétní řádek v **zákazníkům** tabulka bude odstraněno odpovídající řádek v **zákazníkům** tabulce se odstraní.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Koncový Element jako podřízený AssociationSet Element

**End** – element (jako podřízený objekt **AssociationSet** element) Určuje tabulku na jednom konci omezení cizího klíče v podkladové databázi.

**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden)
-   Elementů poznámky (nula nebo více)

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **AssociationSet** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **Objekt EntitySet**  | Ano         | Název sady entit SSDL, který je na konci omezení cizího klíče.                                      |
| **Role**       | Ne          | Hodnota jednoho z **Role** atributy určené v jednom **End** odpovídajícího elementu Association elementu. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

#### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element s **AssociationSet** element se dvěma **koncové** prvky:

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

## <a name="entitycontainer-element-ssdl"></a>Element EntityContainer (SSDL)

**EntityContainer** prvek store schema definition language (SSDL) popisuje strukturu podkladového zdroje dat v aplikaci Entity Framework: sady entit SSDL (definováno v elementů EntitySet nemá) představují tabulky v databáze, typy entit SSDL (definované v objektu EntityType elementy) představují řádky v tabulce a sad přidružení (definované elementy AssociationSet) představují omezení cizího klíče v databázi. Kontejner úložiště modelu entity se mapuje na kontejner koncepčního modelu entity pomocí elementu EntityContainerMapping.

**EntityContainer** element může mít nejvýše jedno dokumentace prvků. Pokud **dokumentaci** element je k dispozici, se musí předcházet všem ostatním prvkům podřízené.

**EntityContainer** prvek může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):

-   Objekt EntitySet
-   Element AssociationSet
-   Elementů poznámky

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **EntityContainer** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název kontejneru entity. Tento název nesmí obsahovat tečky (.). |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityContainer** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element, který definuje dvě sady entit a přidružení jedné sady. Všimněte si, že jsou názvy entit typu a přidružení typu kvalifikován názvem oboru názvů koncepčního modelu.

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

## <a name="entityset-element-ssdl"></a>Element EntitySet (SSDL)

**Objektu EntitySet** prvek v store schema definition language (SSDL) představuje tabulku nebo zobrazení v podkladové databázi. Element EntityType v SSDL představuje řádek v tabulce nebo zobrazení. **EntityType** atribut **objektu EntitySet** prvek určuje konkrétní typ entity SSDL, který představuje řádků v SSDL sadu entit. V elementu EntitySetMapping je určeno mapování mezi sadu entit CSDL a sadu entit SSDL.

**Objektu EntitySet** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   DefiningQuery (žádný nebo jeden element)
-   Elementů poznámky

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **objektu EntitySet** elementu.

> [!NOTE]
> Některé atributy (tu nejsou uvedené) může být kvalifikován s **ukládání** alias. Tyto atributy jsou používány v Průvodci modelů aktualizace při aktualizaci modelu.

| Název atributu | Vyžaduje se | Hodnota                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název sady entit.                                                              |
| **Typ entity** | Ano         | Plně kvalifikovaný název typu entity, pro kterou sada entit obsahuje instance. |
| **schéma**     | Ne          | Schéma databáze.                                                                     |
| **Tabulka**      | Ne          | Databázové tabulky.                                                                      |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **objektu EntitySet** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element, který má dva **objektu EntitySet** elementy a jeden **AssociationSet** element:

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

## <a name="entitytype-element-ssdl"></a>Element EntityType (SSDL)

**EntityType** prvek v store schema definition language (SSDL) představuje řádek v tabulce nebo zobrazení z databáze. Element EntitySet v SSDL představuje tabulku nebo zobrazení, ve kterém dojde k řádků. **EntityType** atribut **objektu EntitySet** prvek určuje konkrétní typ entity SSDL, který představuje řádků v SSDL sadu entit. Mapování mezi typem entity SSDL a typ entity CSDL je zadán v mapování EntityTypeMapping elementu.

**EntityType** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Klíč (žádný nebo jeden element)
-   Elementů poznámky

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **EntityType** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název typu entity. Tato hodnota je obvykle stejný jako název tabulky, ve které představuje typ entity řádek. Tato hodnota může obsahovat žádné tečky (.). |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** element s dvě vlastnosti:

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

## <a name="function-element-ssdl"></a>Element Function (SSDL)

**Funkce** element v store schema definition language (SSDL) Určuje uloženou proceduru, která existuje v podkladové databázi.

**Funkce** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden)
-   Parametr (nula nebo více)
-   Atribut CommandText (nula nebo jedna)
-   Vlastnost ReturnType (nula nebo více)
-   Elementů poznámky (nula nebo více)

Návratová typ pro funkci musí být určen buď **ReturnType** element nebo **ReturnType** atribut (viz níže), ale ne obojí.

Uložené procedury, které jsou určené v modelu úložiště lze importovat do koncepčního modelu aplikace. Další informace najdete v tématu [dotazování pomocí uložené procedury](~/ef6/modeling/designer/stored-procedures/query.md). **Funkce** element můžete také použít k definování vlastních funkcí v rámci modelu úložiště.  

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **funkce** elementu.

> [!NOTE]
> Některé atributy (tu nejsou uvedené) může být kvalifikován s **ukládání** alias. Tyto atributy jsou používány v Průvodci modelů aktualizace při aktualizaci modelu.

| Název atributu             | Vyžaduje se | Hodnota                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**                   | Ano         | Název uložené procedury.                                                                                                                                                                                  |
| **Vlastnost ReturnType**             | Ne          | Návratový typ uložené procedury.                                                                                                                                                                           |
| **Agregace**              | Ne          | **Hodnota TRUE** Pokud bude procedura vracet agregovanou hodnotu; jinak **False**.                                                                                                                                  |
| **Předdefinované**                | Ne          | **Hodnota TRUE** Pokud je integrovaná funkce<sup>1</sup> funkce; v opačném případě **False**.                                                                                                                                  |
| **StoreFunctionName**      | Ne          | Název uložené procedury.                                                                                                                                                                                  |
| **NiladicFunction**        | Ne          | **Hodnota TRUE** Pokud je funkce bez vstupních parametrů<sup>2</sup> funkce; **False** jinak.                                                                                                                                   |
| **IsComposable**           | Ne          | **Hodnota TRUE** Pokud je funkce možností složení<sup>3</sup> funkce; **False** jinak.                                                                                                                                |
| **ParameterTypeSemantics** | Ne          | Výčet, který definuje typ sémantice použité vyřešit přetížení funkce. Výčet je definovaný v manifestu zprostředkovatele za definici funkce. Výchozí hodnota je **AllowImplicitConversion**. |
| **schéma**                 | Ne          | Název schématu, ve kterém je definována uloženou proceduru.                                                                                                                                                   |

<sup>1</sup> integrované funkce je funkce, která je definována v databázi. Informace o funkcích, které jsou definovány v modelu úložiště najdete v tématu Element CommandText (SSDL).

<sup>2</sup> bez vstupních parametrů funkce je funkce, která nepřijímá žádné parametry a při volání, nevyžaduje závorky.

<sup>3</sup> dvě funkce jsou složení, pokud výstup jedné funkce může být vstup pro jiné funkce.

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **funkce** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **funkce** element, který odpovídá **UpdateOrderQuantity** uložené procedury. Uložené procedury přijímá dva parametry a vracet hodnotu.

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

## <a name="key-element-ssdl"></a>Klíčovým prvkem (SSDL)

**Klíč** prvek v store schema definition language (SSDL) představuje primární klíč tabulky v podkladové databázi. **Klíč** je podřízeným prvkem elementu EntityType, který představuje řádek v tabulce. Primární klíč je definována v **klíč** element pomocí odkazu na jeden nebo více vlastností prvků, které jsou definovány na **EntityType** elementu.

**Klíč** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   PropertyRef (jeden nebo více)
-   Elementů poznámky

Žádné atributy se vztahují na **klíč** elementu.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** element s klíčem, který odkazuje na jednu vlastnost:

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

## <a name="ondelete-element-ssdl"></a>Elementy OnDelete – Element (SSDL)

**Elementy OnDelete** prvek store schema definition language (SSDL) odráží databáze chování při odstranění řádku, který se podílí na omezení cizího klíče. Pokud je akce nastavená **Cascade**, pak budou odstraněny také řádky, které odkazují na řádek, který se právě odstraňuje. Pokud je akce nastavená **žádný**, pak se odstraní také řádky, které odkazují na řádek, který se právě odstraňuje. **Elementy OnDelete** element je podřízeným prvkem elementu End.

**Elementy OnDelete** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **elementy OnDelete** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Akce**     | Ano         | **Kaskádové** nebo **žádný**. (Hodnota **s omezeným přístupem** je platný, ale má stejné chování jako **žádný**.) |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **elementy OnDelete** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který definuje **FK\_CustomerOrders** omezení cizího klíče. **Elementy OnDelete** element označuje, že všechny řádky v **objednávky** tabulku, která odkazují na konkrétní řádek v **zákazníkům** tabulka bude odstraněno řádku **Zákazníkům** tabulce se odstraní.

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

## <a name="parameter-element-ssdl"></a>Parameter – Element (SSDL)

**Parametr** prvek store schema definition language (SSDL) je podřízeným prvkem elementu funkce, která určuje parametry pro uloženou proceduru v databázi.

**Parametr** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **parametr** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název parametru                                                                                                                                                                                                      |
| **Typ**       | Ano         | Typ parametru.                                                                                                                                                                                                             |
| **Režim**       | Ne          | **V**, **si**, nebo **InOut** v závislosti na tom, zda je parametr vstup, výstup nebo vstupně výstupní parametr.                                                                                                                |
| **maxLength**  | Ne          | Maximální délka parametru.                                                                                                                                                                                            |
| **Přesnost**  | Ne          | Přesnost parametru.                                                                                                                                                                                                 |
| **Škálování**      | Ne          | Měřítko parametru.                                                                                                                                                                                                     |
| **SRID**       | Ne          | Odkaz na identifikátor spatial systému. Platí jenom pro parametry prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **parametr** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **funkce** element, který má dva **parametr** elementy, které určují vstupní parametry:

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

## <a name="principal-element-ssdl"></a>Instanční objekt – Element (SSDL)

**Hlavní** prvek store schema definition language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje hlavní konec omezení cizího klíče (tzv. referenční omezení). **Hlavní** prvek určuje sloupec primárního klíče (nebo sloupce) v tabulce, na který odkazuje jiný sloupec (nebo sloupce). **PropertyRef** určují prvky sloupce, které jsou odkazovány. Určuje sloupce, které odkazují na sloupce primárního klíče, které jsou určené v elementu závislé **instančního objektu** elementu.

**Hlavní** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   PropertyRef (jeden nebo více)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **hlavní** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Role**       | Ano         | Stejná hodnota jako **Role** atribut (Pokud se používá) odpovídajícího prvku End; v opačném případě se název tabulky, který obsahuje Odkazovaný sloupec. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **hlavní** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element přidružení, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders** cizí klíč omezení. **Hlavní** určuje element **CustomerId** sloupec **zákazníka** tabulku jako hlavní konec omezení.

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

## <a name="property-element-ssdl"></a>Property – Element (SSDL)

**Vlastnost** prvek v store schema definition language (SSDL) představuje sloupci v tabulce v podkladové databázi. **Vlastnost** prvky jsou podřízených elementů EntityType, které představují řádky v tabulce. Každý **vlastnost** element definovaný v **EntityType** element představuje sloupci.

A **vlastnost** element nemůže mít žádné podřízené prvky.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **vlastnost** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**                  | Ano         | Název odpovídající sloupec.                                                                                                                                                                                           |
| **Typ**                  | Ano         | Typ odpovídající sloupec.                                                                                                                                                                                           |
| **S povolenou hodnotou Null**              | Ne          | **Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda odpovídající sloupec může mít hodnotu null.                                                                                                                  |
| **Výchozí hodnota**          | Ne          | Výchozí hodnota odpovídající sloupec.                                                                                                                                                                                  |
| **maxLength**             | Ne          | Maximální délka odpovídající sloupec.                                                                                                                                                                                 |
| **Hodnoty**           | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, zda se uloží odpovídající hodnotu sloupce jako řetězce pevné délky.                                                                                                              |
| **Přesnost**             | Ne          | Přesnost odpovídající sloupec.                                                                                                                                                                                      |
| **Škálování**                 | Ne          | Škálování odpovídající sloupec.                                                                                                                                                                                          |
| **Unicode**               | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží odpovídající hodnotu sloupce jako řetězec s kódováním Unicode.                                                                                                                   |
| **Kolace**             | Ne          | Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.                                                                                                                                                   |
| **SRID**                  | Ne          | Odkaz na identifikátor spatial systému. Platí pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Výčet StoreGeneratedPattern** | Ne          | **Žádný**, **Identity** (Pokud je hodnota odpovídající sloupec identity, který je vygenerován v databázi), nebo **vypočítané** (Pokud odpovídající sloupec hodnota byla vypočítána v databázi). Není platné pro RowType vlastnosti. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **vlastnost** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** element s dva podřízené **vlastnost** prvky:

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

## <a name="propertyref-element-ssdl"></a>Element PropertyRef (SSDL)

**PropertyRef** prvek store schema definition language (SSDL) odkazuje na vlastnost definovaný v elementu EntityType k označení, že vlastnost bude provádět jednu z následujících rolí:

-   Být součástí primárního klíče tabulky, který **EntityType** představuje. Jeden nebo více **PropertyRef** prvky můžete použít k definování primární klíč. Další informace najdete v elementu Key.
-   Být závislé nebo hlavní konec referenčního omezení. Další informace najdete v elementu ReferentialConstraint elementu.

**PropertyRef** element může obsahovat pouze následujících podřízených elementů:

-   Dokumentace ke službě (žádný nebo jeden)
-   Elementů poznámky

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **PropertyRef** elementu.

| Název atributu | Vyžaduje se | Hodnota                                |
|:---------------|:------------|:-------------------------------------|
| **Jméno**       | Ano         | Název odkazované vlastnosti. |

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **PropertyRef** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **PropertyRef** element sloužící k odkazování na vlastnost, která je definována na definovat primární klíč **EntityType** elementu.

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

## <a name="referentialconstraint-element-ssdl"></a>Element elementu ReferentialConstraint (SSDL)

**Elementu ReferentialConstraint** prvek v store schema definition language (SSDL) představuje omezení cizího klíče (také nazývané omezení referenční integrity) v podkladové databázi. Objektem zabezpečení a závislými konce omezení jsou určena pomocí podřízených prvků objektu zabezpečení a závislé. Sloupce, které jsou součástí objektem zabezpečení a závislými elementy end jsou odkazovány pomocí PropertyRef elementy.

**Elementu ReferentialConstraint** element je volitelný podřízený prvek elementu Association. Pokud **elementu ReferentialConstraint** element není slouží k omezení cizího klíče, který je zadán v mapování **přidružení** element, AssociationSetMapping element musí k tomu použít.

**Elementu ReferentialConstraint** prvek může mít následujících podřízených elementů:

-   Dokumentace ke službě (žádný nebo jeden)
-   Objekt zabezpečení (právě jeden)
-   Závislé (právě jeden)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Libovolný počet atributů poznámky (vlastní atributy XML) lze na **elementu ReferentialConstraint** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který se používá **elementu ReferentialConstraint** elementu k určení sloupců, které jsou součástí **FK\_CustomerOrders**  omezení cizího klíče:

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

## <a name="returntype-element-ssdl"></a>Element ReturnType (SSDL)

**ReturnType** element v store schema definition language (SSDL) určuje návratový typ pro funkci, která je definována v **funkce** elementu. Funkce, návratový typ je také možné zadat při **ReturnType** atribut.

Návratový typ funkce se zadaným **typ** atribut nebo **ReturnType** elementu.

**ReturnType** prvek může mít následujících podřízených elementů:

- Objekt CollectionType (jeden)  

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReturnType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro SSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad používá **funkce** , který vrátí kolekce řádků.

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


## <a name="rowtype-element-ssdl"></a>Element RowType (SSDL)

A **RowType** prvek v store schema definition language (SSDL) definuje nepojmenovanou strukturu jako návratový typ pro funkci definovanou v úložišti.

A **RowType** prvek je podřízený prvek **CollectionType** element:

A **RowType** prvek může mít následujících podřízených elementů:

- Vlastnosti (jeden nebo více)  

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci úložiště, který používá **CollectionType** elementu k určení, že funkce vrátí kolekce řádků (jak je uvedeno v **RowType** element).


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

## <a name="schema-element-ssdl"></a>Element schématu (SSDL)

**Schématu** prvek store schema definition language (SSDL) je kořenovým elementem definice modelu úložiště. Obsahuje definice pro objekty, funkce a kontejnerů, které tvoří modelu úložiště.

**Schématu** element může obsahovat nula nebo více z následujících podřízených elementů:

-   Přidružení
-   Typ entity
-   EntityContainer
-   Funkce

**Schématu** prvek používá **Namespace** atribut pro definování oboru názvů pro objekty typu a přidružení entit v modelu úložiště. V rámci oboru názvů může mít žádné dva objekty se stejným názvem.

Obor názvů modelu úložiště se liší od oboru názvů XML **schématu** elementu. Obor názvů modelu úložiště (podle definice **Namespace** atribut) je logický kontejner pro typy entit a přidružení typů. Obor názvů XML (indikován **xmlns** atribut) z **schématu** element je výchozí obor názvů pro podřízené prvky a atributy **schématu** elementu. Obory názvů XML formuláře http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (kde RRRR a MM představují roku a měsíce v uvedeném pořadí) jsou vyhrazené pro SSDL. Vlastní elementy a atributy nemohou být v oborech názvů, které mají tento formulář.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy lze použít **schématu** elementu.

| Název atributu            | Vyžaduje se | Hodnota                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Ano         | Obor názvů model úložiště. Hodnota **Namespace** atribut se používá k vytvoření plně kvalifikovaný název typu. Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů ExampleModel.Store pak plně kvalifikovaný název **EntityType** je ExampleModel.Store.Customer. <br/> Následující řetězce nelze použít jako hodnotu **Namespace** atribut: **systému**, **přechodné**, nebo **Edm**. Hodnota **Namespace** atribut nemůže být stejná jako hodnota pro **Namespace** atribut v elementu CSDL Schema. |
| **Alias**                 | Ne          | Identifikátor, použijí se místo názvu oboru názvů. Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů ExampleModel.Store a hodnoty **Alias** atribut je *StorageModel*, můžete použít StorageModel.Customer jako plně kvalifikovaný název **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Zprostředkovatel**              | Ano         | Zprostředkovatel dat.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Ano         | Token, který označuje které manifest zprostředkovatele se vraťte k poskytovateli. Je definován žádný formát pro daný token. Hodnoty pro daný token je definována v poskytovateli. Informace o manifestu tokeny zprostředkovatele SQL Server najdete v tématu SqlClient pro Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Příklad

Následující příklad ukazuje **schématu** element, který obsahuje **EntityContainer** elementu, dvěma **EntityType** elementy a jeden **přidružení** elementu.

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

## <a name="annotation-attributes"></a>Atributy poznámek

Atributy poznámek v store schema definition language (SSDL) jsou vlastní atributy XML v modelu úložiště, které poskytují další metadata o prvky v modelu úložiště. Kromě s platnou strukturu XML, platí následující omezení atributů poznámky:

-   Atributy poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro SSDL.
-   Plně kvalifikovaných názvů atributů dvě poznámky nesmí být stejné.

Více než jeden atribut poznámky se můžou vztahovat na daný prvek SSDL. Metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje element EntityType, který má anotaci atributem použitým **OrderId** vlastnost. V příkladu také zobrazit poznámky prvek přidán do **EntityType** elementu.

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

## <a name="annotation-elements-ssdl"></a>Elementů poznámky (SSDL)

Poznámka prvky store schema definition language (SSDL) jsou vlastní elementů XML v modelu úložiště, které poskytují další metadata týkající se modelu úložiště. Kromě platný struktura XML elementů poznámky platí následující omezení:

-   Elementů poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro SSDL.
-   Plně kvalifikovaných názvů všech elementů dvě poznámky nesmí být stejné.
-   Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky daného prvku SSDL.

Více než jeden element anotace může být podřízeným daného prvku SSDL. Od verze rozhraní .NET Framework verze 4, metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje element EntityType, který má element anotace (**CustomElement**). Tento příklad také ukazuje anotace atributem použitým **OrderId** vlastnost.

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

## <a name="facets-ssdl"></a>Omezující vlastnosti (SSDL)

Omezující vlastnosti v store schema definition language (SSDL) představují omezení na typy sloupců, které jsou uvedeny v prvcích vlastností. Omezující vlastnosti jsou implementovány jako atributy ve formátu XML na **vlastnost** elementy.

Následující tabulka popisuje, které jsou podporovány v SSDL omezující vlastnosti:

| omezující vlastnost           | Popis                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Kolace**   | Určuje pořadí řazení (nebo pořadí řazení) pro použití při provádění porovnání a řazení operací na hodnotách vlastnosti.                                                                                                             |
| **Hodnoty** | Určuje, zda se může lišit délka hodnoty sloupce.                                                                                                                                                                                                  |
| **maxLength**   | Určuje maximální délku hodnoty sloupce.                                                                                                                                                                                                           |
| **Přesnost**   | Pro vlastnosti typu **desítkové**, určuje počet číslic, může mít hodnotu vlastnosti. Pro vlastnosti typu **čas**, **data a času**, a **DateTimeOffset**, určuje počet číslic za desetinnou čárkou sady sekund hodnotu ve sloupci. |
| **Škálování**       | Určuje počet číslic vpravo od desetinné čárky pro hodnotu sloupce.                                                                                                                                                                      |
| **Unicode**     | Určuje, zda hodnota sloupce ukládá jako kódování Unicode.                                                                                                                                                                                                    |
