---
title: SSDL – specifikace – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182545"
---
# <a name="ssdl-specification"></a>Specifikace SSDL
Soubor SSDL (Schema Definition Language) je jazyk založený na jazyce XML, který popisuje model úložiště aplikace Entity Framework.

V Entity Framework aplikaci se metadata modelu úložiště načítají ze souboru. ssdl (napsaného ve verzi SSDL) do instance System. data. Metadata. Edm. StoreItemCollection a jsou přístupná pomocí metod v System. data. Metadata. Edm. MetadataWorkspace – třída Entity Framework používá metadata modelu úložiště k překladu dotazů na koncepční model pro ukládání specifických příkazů.

Entity Framework Designer (EF Designer) ukládá informace o modelu úložiště v souboru EDMX v době návrhu. V okamžiku sestavení Entity Designer používá informace v souboru. edmx k vytvoření souboru. ssdl, který je vyžadován Entity Framework za běhu.

Verze SSDL jsou odlišeny obory názvů XML.

| Verze SSDL | Obor názvů XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL V3      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Element Association (SSDL)

Element **Association** ve službě Store Schema Definition Language (SSDL) Určuje sloupce tabulky, které se účastní omezení cizího klíče v podkladové databázi. Dva povinné podřízené elementy end určují tabulky na konci přidružení a násobnost na každém konci. Volitelný element elementu ReferentialConstraint určuje hlavní a závislé konce přidružení a také zúčastněné sloupce. Pokud není přítomen žádný element **elementu ReferentialConstraint** , je nutné použít element AssociationSetMapping k určení mapování sloupce pro přidružení.

Element **Association** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (0 nebo 1)
-   End (přesně dva)
-   Elementu ReferentialConstraint (nula nebo jeden)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Association** .

| Název atributu | Je povinné | Hodnota                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Název**       | Ano         | Název odpovídajícího omezení cizího klíče v podkladové databázi. |

> [!NOTE]
> Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** , který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_FK** :

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

## <a name="associationset-element-ssdl"></a>AssociationSet – Element (SSDL)

Element **AssociationSet** ve službě Store Schema Definition Language (SSDL) představuje omezení cizího klíče mezi dvěma tabulkami v podkladové databázi. Sloupce tabulky, které jsou součástí omezení cizího klíče, jsou zadány v elementu Association. Element **Association** , který odpovídá danému elementu **AssociationSet** , je určen v atributu **Association** elementu **AssociationSet** .

Sady přidružení SSDL jsou namapovány na sady přidružení CSDL pomocí elementu AssociationSetMapping. Pokud je však přidružení CSDL pro danou sadu přidružení CSDL definováno pomocí elementu elementu ReferentialConstraint, není nutný žádný odpovídající prvek **AssociationSetMapping** . V tomto případě, pokud je přítomen element **AssociationSetMapping** , mapování, které definuje, bude přepsáno prvkem **elementu ReferentialConstraint** .

Element **AssociationSet** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (0 nebo 1)
-   End (nula nebo 2)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSet** .

| Název atributu  | Je povinné | Hodnota                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Název**        | Ano         | Název omezení cizího klíče, který představuje sada přidružení.                          |
| **Přidružení** | Ano         | Název asociace, který definuje sloupce, které jsou součástí omezení cizího klíče. |

> [!NOTE]
> Pro element **AssociationSet** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **AssociationSet** , který představuje omezení `FK_CustomerOrders` cizího klíče v podkladové databázi:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType – element (SSDL)

Element **CollectionType** ve službě Store Schema Definition Language (SSDL) určuje, že návratový typ funkce je kolekce. Element **CollectionType** je podřízeným prvkem atributu ReturnType. Typ kolekce je určen pomocí podřízeného prvku RowType:

> [!NOTE]
> Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků.

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

## <a name="commandtext-element-ssdl"></a>CommandText – element (SSDL)

Element **CommandText** ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku funkce, který umožňuje definovat příkaz SQL, který je spuštěn v databázi. Element **CommandText** umožňuje přidat funkce, které jsou podobné uložené proceduře v databázi, ale v modelu úložiště definujte element **CommandText** .

Element **CommandText** nemůže mít podřízené elementy. Tělo elementu **CommandText** musí být platným příkazem SQL pro podkladovou databázi.

Pro element **CommandText** nelze použít žádné atributy.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **Function** s podřízeným elementem **CommandText** . Vystavte funkci **UpdateProductInOrder** jako metodu objektu ObjectContext importem do koncepčního modelu.  

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

## <a name="definingquery-element-ssdl"></a>DefiningQuery – element (SSDL)

Element **DefiningQuery** v úložišti SSDL (Schema Definition Language) umožňuje spustit příkaz SQL přímo v podkladové databázi. Element **DefiningQuery** se běžně používá jako zobrazení databáze, ale zobrazení je definované v modelu úložiště místo databáze. Zobrazení definované v elementu **DefiningQuery** lze namapovat na typ entity v koncepčním modelu prostřednictvím elementu EntitySetMapping. Tato mapování jsou jen pro čtení.  

Následující syntaxe SSDL ukazuje deklaraci objektu **EntitySet** následovaný elementem **DefiningQuery** , který obsahuje dotaz použitý k načtení zobrazení.

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

Uložené procedury v Entity Framework můžete použít k povolení scénářů pro čtení i zápis v zobrazeních. Můžete použít buď zobrazení zdroje dat, nebo zobrazení Entity SQL jako základní tabulku pro načítání dat a pro zpracování změn v uložených procedurách.

Element **DefiningQuery** můžete použít k cíli Microsoft SQL Server Compact 3,5. I když SQL Server Compact 3,5 nepodporuje uložené procedury, můžete implementovat podobné funkce pomocí elementu **DefiningQuery** . Další místo, kde může být užitečné, je vytvoření uložených procedur k překonání neshody mezi datovými typy použitými v programovacím jazyce a zdroji dat. Můžete napsat **DefiningQuery** , který přebírá určitou sadu parametrů, a pak volá uloženou proceduru s jinou sadou parametrů, například uloženou proceduru, která odstraňuje data.

## <a name="dependent-element-ssdl"></a>Závislý element (SSDL)

**Závislý** element ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje závislý konec omezení cizího klíče (označuje se také jako referenční omezení). **Závislý** element určuje sloupec (nebo sloupce) v tabulce, která odkazuje na sloupec primárního klíče (nebo sloupce). Prvky **PropertyRef** určují, na které sloupce se odkazuje. Hlavní prvek určuje sloupce primárního klíče, na které jsou odkazovány sloupce, které jsou zadány v **závislém** elementu.

**Závislý** element může mít následující podřízené elementy (v uvedeném pořadí):

-   PropertyRef (jedna nebo víc)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na **závislý** element.

| Název atributu | Je povinné | Hodnota                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Role**       | Ano         | Stejná hodnota jako atribut **role** (Pokud se používá) odpovídajícího elementu end; jinak název tabulky, která obsahuje odkazující sloupec. |

> [!NOTE]
> Pro **závislý** element lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element Association, který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_CustomerOrders** . **Závislý** element určuje sloupec **KódZákazníka** tabulky **Order** jako závislý konec omezení.

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

## <a name="documentation-element-ssdl"></a>Element dokumentace (SSDL)

Element **dokumentace** v úložišti SSDL (Schema Definition Language) je možné použít k poskytnutí informací o objektu, který je definovaný v nadřazeném elementu.

Prvek **dokumentace** může mít následující podřízené prvky (v uvedeném pořadí):

-   **Summary**: stručný popis nadřazeného elementu. (žádný nebo jeden element)
-   **Longdescription**: rozsáhlý popis nadřazeného elementu. (žádný nebo jeden element)

### <a name="applicable-attributes"></a>Použitelné atributy

Pro prvek **dokumentace** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **dokumentace** jako podřízený prvek elementu EntityType.

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

## <a name="end-element-ssdl"></a>End – element (SSDL)

Element **End** ve službě Store Schema Definition Language (SSDL) Určuje tabulku a počet řádků na jednom konci omezení cizího klíče v podkladové databázi. Element **End** může být podřízeným prvkem elementu Association nebo elementu AssociationSet. V každém případě jsou možné podřízené prvky a příslušné atributy odlišné.

### <a name="end-element-as-a-child-of-the-association-element"></a>Ukončit element jako podřízený elementu přidružení

Element **End** (jako podřízený prvek elementu **Association** ) Určuje tabulku a počet řádků na konci omezení cizího klíče s atributy **typu** a **násobnosti** . Konce omezení cizího klíče jsou definovány jako součást přidružení SSDL; přidružení SSDL musí mít přesně dva konce.

Element **End** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Při odstranění (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízenou položkou elementu **Association** .

| Název atributu   | Je povinné | Hodnota                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Ano         | Plně kvalifikovaný název sady entit SSDL, která je na konci omezení cizího klíče.                                                                                                                                                                                                                                                                                          |
| **Role**         | Ne          | Hodnota atributu **role** v objektu zabezpečení nebo závislém elementu odpovídajícího prvku elementu ReferentialConstraint (Pokud se používá).                                                                                                                                                                                                                                             |
| **Násobnost** | Ano         | **1**, **0.. 1**nebo **\*** v závislosti na počtu řádků, které mohou být na konci omezení cizího klíče. <br/> **1** znamená, že v konci omezení cizího klíče existuje přesně jeden řádek. <br/> **0.. 1** znamená, že v konci omezení cizího klíče existuje žádný nebo jeden řádek. <br/> **\*** označuje, že v konci omezení cizího klíče existuje nula, jeden nebo více řádků. |

> [!NOTE]
> Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

#### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** , který definuje omezení cizího klíče **CustomerOrders\_** . Hodnoty **násobnosti** zadané u každého elementu **End** označují, že mnoho řádků v tabulce **Orders** je možné přidružit k řádku v tabulce **Customers** , ale k řádku v tabulce **Orders** může být přidružen pouze jeden řádek v tabulce **Customers** . Kromě toho element **IsDeleted** označuje, že všechny řádky v tabulce **Orders** , které odkazují na konkrétní řádek v tabulce **Customers** , se odstraní, pokud se řádek v tabulce **Customers (zákazníci** ) odstraní.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Ukončit element jako podřízený element elementu AssociationSet

Element **End** (jako podřízený prvek elementu **AssociationSet** ) Určuje tabulku na jednom konci omezení cizího klíče v podkladové databázi.

Element **End** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (0 nebo 1)
-   Prvky poznámky (nula nebo více)

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízeným elementem elementu **AssociationSet** .

| Název atributu | Je povinné | Hodnota                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **Sada**  | Ano         | Název sady entit SSDL, která je na konci omezení cizího klíče.                                      |
| **Role**       | Ne          | Hodnota jednoho z atributů **role** zadaná u jednoho elementu **End** odpovídajícího elementu Association. |

> [!NOTE]
> Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

#### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** s elementem **AssociationSet** se dvěma elementy **End** :

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

## <a name="entitycontainer-element-ssdl"></a>EntityContainer – element (SSDL)

Element **EntityContainer** v rámci služby Store Schema Definition Language (SSDL) popisuje strukturu podkladového zdroje dat v aplikaci Entity Framework: sady entit SSDL (definované v elementech EntitySet) reprezentují tabulky v databázi, typy entit SSDL (definované v elementech EntityType) reprezentují v databázi omezení cizího klíče. Kontejner entit modelu úložiště se mapuje na kontejner entit koncepčního modelu prostřednictvím elementu EntityContainerMapping.

Element **EntityContainer** může mít nula nebo jeden prvek dokumentace. Pokud je přítomen prvek **dokumentace** , musí předcházet všem ostatním podřízeným elementům.

Element **EntityContainer** může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):

-   Sada
-   Vlastností
-   Prvky poznámky

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityContainer** .

| Název atributu | Je povinné | Hodnota                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Název**       | Ano         | Název kontejneru entity. Tento název nesmí obsahovat tečky (.). |

> [!NOTE]
> Pro element **EntityContainer** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** , který definuje dvě sady entit a jednu sadu přidružení. Všimněte si, že názvy entit a typů přidružení jsou kvalifikovány názvem oboru názvů konceptuálního modelu.

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

## <a name="entityset-element-ssdl"></a>EntitySet – element (SSDL)

Element **EntitySet** ve službě Store Schema Definition Language (SSDL) představuje tabulku nebo zobrazení v podkladové databázi. Element EntityType ve SSDL představuje řádek v tabulce nebo zobrazení. Atribut **EntityType** elementu **EntitySet** určuje konkrétní typ entity SSDL reprezentující řádky v sadě entit SSDL. Mapování mezi sadou entit CSDL a sadou entit SSDL je zadáno v elementu EntitySetMapping.

Element **EntitySet** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   DefiningQuery (nula nebo jeden element)
-   Prvky poznámky

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntitySet** .

> [!NOTE]
> Některé atributy (zde nejsou uvedeny) mohou být kvalifikovány s aliasem **úložiště** . Tyto atributy používá Průvodce modelem aktualizace při aktualizaci modelu.

| Název atributu | Je povinné | Hodnota                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název sady entit.                                                              |
| **Objektu** | Ano         | Plně kvalifikovaný název typu entity, pro který sada entit obsahuje instance. |
| **schéma**     | Ne          | Schéma databáze.                                                                     |
| **Tabulka**      | Ne          | Databázová tabulka                                                                      |

> [!NOTE]
> Pro element **EntitySet** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** , který má dva elementy **EntitySet** a jeden element **AssociationSet** :

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

## <a name="entitytype-element-ssdl"></a>EntityType – element (SSDL)

Element **EntityType** ve službě Store Schema Definition Language (SSDL) představuje řádek v tabulce nebo zobrazení podkladové databáze. Element EntitySet ve SSDL představuje tabulku nebo zobrazení, ve kterých se vyskytují řádky. Atribut **EntityType** elementu **EntitySet** určuje konkrétní typ entity SSDL reprezentující řádky v sadě entit SSDL. Mapování mezi typem entity SSDL a typem entity CSDL je zadáno v elementu element entitytypemapping.

Element **EntityType** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Klíč (nula nebo jeden element)
-   Prvky poznámky

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityType** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název typu entity Tato hodnota je obvykle stejná jako název tabulky, ve které typ entity představuje řádek. Tato hodnota nesmí obsahovat žádné tečky (.). |

> [!NOTE]
> Pro element **EntityType** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** se dvěma vlastnostmi:

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

## <a name="function-element-ssdl"></a>Function – Element (SSDL)

Prvek **funkce** v úložišti SSDL (Store Schema Definition Language) určuje uloženou proceduru, která existuje v podkladové databázi.

Element **Function** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (0 nebo 1)
-   Parametr (nula nebo více)
-   CommandText (nula nebo jedna)
-   ReturnType (nula nebo více)
-   Prvky poznámky (nula nebo více)

Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** , nebo atributu **ReturnType** (viz níže), ale nikoli obojího.

Uložené procedury, které jsou zadány v modelu úložiště, lze importovat do koncepčního modelu aplikace. Další informace najdete v tématu [dotazování s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/query.md). Element **Function** lze také použít k definování vlastních funkcí v modelu úložiště.  

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Function** .

> [!NOTE]
> Některé atributy (zde nejsou uvedeny) mohou být kvalifikovány s aliasem **úložiště** . Tyto atributy používá Průvodce modelem aktualizace při aktualizaci modelu.

| Název atributu             | Je povinné | Hodnota                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**                   | Ano         | Název uložené procedury.                                                                                                                                                                                  |
| **ReturnType**             | Ne          | Návratový typ uložené procedury.                                                                                                                                                                           |
| **Souhrnné**              | Ne          | **True** , pokud uložená procedura vrátí agregovanou hodnotu; v opačném případě **false**.                                                                                                                                  |
| **Integrované**                | Ne          | **True** , pokud je funkce vestavěnou funkcí<sup>1</sup> ; v opačném případě **false**.                                                                                                                                  |
| **StoreFunctionName**      | Ne          | Název uložené procedury.                                                                                                                                                                                  |
| **NiladicFunction**        | Ne          | **True** , pokud je funkce funkce niladic<sup>2</sup> ; V opačném případě **NEPRAVDA** .                                                                                                                                   |
| **IsComposable**           | Ne          | **True** , pokud je funkce sestavitelnou funkcí<sup>3</sup> ; V opačném případě **NEPRAVDA** .                                                                                                                                |
| **ParameterTypeSemantics** | Ne          | Výčet definující sémantiku typu, která se používá k vyřešení přetížení funkce. Výčet je definován v manifestu zprostředkovatele podle definice funkce. Výchozí hodnota je **AllowImplicitConversion**. |
| **schéma**                 | Ne          | Název schématu, ve kterém je uložená procedura definovaná.                                                                                                                                                   |

<sup>1</sup> integrovaná funkce je funkce, která je definována v databázi. Informace o funkcích, které jsou definovány v modelu úložiště, naleznete v tématu CommandText element (SSDL).

<sup>2</sup> funkce niladic je funkce, která nepřijímá žádné parametry a při volání nepožaduje závorky.

<sup>3</sup> dvě funkce jsou sestavitelné, pokud výstup jedné funkce může být vstupem pro druhou funkci.

> [!NOTE]
> Pro element **Function** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **funkce** , který odpovídá uložené proceduře **UpdateOrderQuantity** . Uložená procedura přijímá dva parametry a nevrací hodnotu.

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

## <a name="key-element-ssdl"></a>Key – element (SSDL)

**Klíčovým** prvkem ve službě Store Schema Definition Language (SSDL) představuje primární klíč tabulky v podkladové databázi. **Key** je podřízený prvek elementu EntityType, který představuje řádek v tabulce. Primární klíč je definován v elementu **Key** odkazem na jeden nebo více prvků vlastností, které jsou definovány v elementu **EntityType** .

**Klíčový** prvek může mít následující podřízené elementy (v uvedeném pořadí):

-   PropertyRef (jedna nebo víc)
-   Prvky poznámky

Pro **klíčový** element nejsou použity žádné atributy.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** s klíčem, který odkazuje na jednu vlastnost:

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

## <a name="ondelete-element-ssdl"></a>IsDeleted – element (SSDL)

Element **IsDeleted** ve službě Store Schema Definition Language (SSDL) odráží chování databáze při odstranění řádku, který je součástí omezení cizího klíče. Pokud je akce nastavena na **Cascade**, odstraní se i řádky, které odkazují na odstraněný řádek. Pokud je akce nastavena na **hodnotu None**, nebudou odstraněny také řádky, které odkazují na odstraněný řádek. Element **IsDeleted** je podřízeným prvkem elementu end.

Element **IsDeleted** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (0 nebo 1)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **IsDeleted** .

| Název atributu | Je povinné | Hodnota                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Akce**     | Ano         | **Cascade** nebo **none**. (Hodnota **s omezením** je platná, ale má stejné chování jako **žádné**.) |

> [!NOTE]
> Pro element **IsDeleted** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** , který definuje omezení cizího klíče **CustomerOrders\_** . Element **IsDeleted** označuje, že všechny řádky v tabulce **Orders** , které odkazují na konkrétní řádek v tabulce **Customers** , se odstraní, když se řádek v tabulce **Customers** odstraní.

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

## <a name="parameter-element-ssdl"></a>Parameter – element (SSDL)

Element **Parameter** ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku funkce, který určuje parametry pro uloženou proceduru v databázi.

Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (0 nebo 1)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **Parameter** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název parametru                                                                                                                                                                                                      |
| **Typ**       | Ano         | Typ parametru.                                                                                                                                                                                                             |
| **Mode**       | Ne          | **In**, **out**nebo **InOut** v závislosti na tom, zda je parametr vstupní, výstupní nebo vstupní/výstupní parametr.                                                                                                                |
| **MaxLength**  | Ne          | Maximální délka parametru.                                                                                                                                                                                            |
| **Číslic**  | Ne          | Přesnost parametru.                                                                                                                                                                                                 |
| **Kapacity**      | Ne          | Měřítko parametru.                                                                                                                                                                                                     |
| **SRID**       | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro parametry prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **funkce** , který má dva prvky **parametru** , které určují vstupní parametry:

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

## <a name="principal-element-ssdl"></a>Element zabezpečení (SSDL)

Element **Principal** ve službě Store Schema Definition Language (SSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje hlavní konec omezení cizího klíče (označuje se také jako referenční omezení). Element **Principal** určuje sloupec (nebo sloupce) primárního klíče v tabulce, na kterou odkazuje jiný sloupec (nebo sloupce). Prvky **PropertyRef** určují, na které sloupce se odkazuje. Závislý element určuje sloupce, které odkazují na sloupce primárního klíče, které jsou zadány v elementu **Principal** .

Element **Principal** může mít následující podřízené elementy (v uvedeném pořadí):

-   PropertyRef (jedna nebo víc)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Principal** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Role**       | Ano         | Stejná hodnota jako atribut **role** (Pokud se používá) odpovídajícího elementu end; jinak název tabulky, která obsahuje odkazovaný sloupec. |

> [!NOTE]
> U elementu **Principal** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element Association, který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_CustomerOrders** . Element **Principal** určuje sloupec **KódZákazníka** tabulky **Customer** jako hlavní konec omezení.

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

## <a name="property-element-ssdl"></a>Property – element (SSDL)

Element **Property** ve službě Store Schema Definition Language (SSDL) představuje sloupec v tabulce v podkladové databázi. Prvky **vlastností** jsou podřízené prvky EntityType, které představují řádky v tabulce. Každý element **Property** definovaný na elementu **EntityType** reprezentuje sloupec.

Element **Property** nemůže mít žádné podřízené elementy.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .

| Název atributu            | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**                  | Ano         | Název odpovídajícího sloupce.                                                                                                                                                                                           |
| **Typ**                  | Ano         | Typ odpovídajícího sloupce.                                                                                                                                                                                           |
| **Povoleno**              | Ne          | **True** (výchozí hodnota) nebo **false** v závislosti na tom, zda odpovídající sloupec může mít hodnotu null.                                                                                                                  |
| **Hodnot**          | Ne          | Výchozí hodnota odpovídajícího sloupce.                                                                                                                                                                                  |
| **MaxLength**             | Ne          | Maximální délka odpovídajícího sloupce.                                                                                                                                                                                 |
| **FixedLength**           | Ne          | **True** nebo **false** v závislosti na tom, jestli se odpovídající hodnota sloupce uloží jako řetězec s pevnou délkou.                                                                                                              |
| **Číslic**             | Ne          | Přesnost odpovídajícího sloupce.                                                                                                                                                                                      |
| **Kapacity**                 | Ne          | Měřítko odpovídajícího sloupce.                                                                                                                                                                                          |
| **Unicode**               | Ne          | **True** nebo **false** v závislosti na tom, jestli se odpovídající hodnota sloupce uloží jako řetězec Unicode.                                                                                                                   |
| **Velké**             | Ne          | Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.                                                                                                                                                   |
| **SRID**                  | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Ne          | **None**, **identity** (Pokud je odpovídající hodnota sloupce identita, která je generována v databázi), nebo **vypočítána** (Pokud je v databázi vypočítána odpovídající hodnota sloupce). Neplatný pro vlastnosti RowType. |

> [!NOTE]
> Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** se dvěma podřízenými prvky **vlastnosti** :

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

## <a name="propertyref-element-ssdl"></a>PropertyRef – element (SSDL)

Element **PropertyRef** v úložišti pro definici schématu (SSDL) odkazuje na vlastnost definovanou na elementu EntityType, aby označoval, že vlastnost provede jednu z následujících rolí:

-   Být součástí primárního klíče tabulky, který představuje **EntityType** . Jeden nebo více elementů **PropertyRef** lze použít k definování primárního klíče. Další informace naleznete v tématu klíčový element.
-   Být závislé nebo hlavní zakončení referenčního omezení. Další informace naleznete v tématu elementu ReferentialConstraint element.

Element **PropertyRef** může mít pouze následující podřízené prvky:

-   Dokumentace (0 nebo 1)
-   Prvky poznámky

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **PropertyRef** .

| Název atributu | Je povinné | Hodnota                                |
|:---------------|:------------|:-------------------------------------|
| **Název**       | Ano         | Název odkazované vlastnosti |

> [!NOTE]
> Pro element **PropertyRef** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **PropertyRef** , který slouží k definování primárního klíče odkazem na vlastnost, která je definována v elementu **EntityType** .

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

## <a name="referentialconstraint-element-ssdl"></a>Elementu ReferentialConstraint – element (SSDL)

Element **elementu ReferentialConstraint** ve službě Store Schema Definition Language (SSDL) představuje omezení cizího klíče (označované také jako omezení referenční integrity) v podkladové databázi. Hlavní a závislé elementy na konci omezení jsou určeny hlavními a závislými podřízenými prvky v uvedeném pořadí. Sloupce, které se podílejí na koncích objektu zabezpečení a závislých, jsou odkazovány pomocí elementů PropertyRef.

Element **elementu ReferentialConstraint** je volitelný podřízený prvek elementu Association. Pokud se nepoužívá element **elementu ReferentialConstraint** k mapování omezení cizího klíče, který je určen v elementu **Association** , je nutné použít element AssociationSetMapping.

Element **elementu ReferentialConstraint** může mít následující podřízené prvky:

-   Dokumentace (0 nebo 1)
-   Objekt zabezpečení (právě jeden)
-   Závislý (právě jeden)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Pro element **elementu ReferentialConstraint** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** , který používá element **elementu ReferentialConstraint** k určení sloupců, které se účastní omezení cizího klíče **\_FK** :

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

## <a name="returntype-element-ssdl"></a>ReturnType – element (SSDL)

Element **ReturnType** v rámci služby Store Schema Definition Language (SSDL) určuje návratový typ pro funkci, která je definována v elementu **Function** . Návratový typ funkce lze také zadat s atributem **ReturnType** .

Návratový typ funkce je zadán pomocí atributu **Type** nebo elementu **ReturnType** .

Element **ReturnType** může mít následující podřízené prvky:

- CollectionType (jeden)  

> [!NOTE]
> Pro element **ReturnType** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nepatří do žádného oboru názvů XML, který je vyhrazený pro SSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad používá **funkci** , která vrací kolekci řádků.

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


## <a name="rowtype-element-ssdl"></a>RowType – element (SSDL)

Element **RowType** ve službě Store Schema Definition Language (SSDL) definuje nepojmenované struktury jako návratový typ pro funkci definovanou v úložišti.

Element **RowType** je podřízeným prvkem elementu **CollectionType** :

Element **RowType** může mít následující podřízené prvky:

- Vlastnost (jedna nebo více)  

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci úložiště, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).


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

## <a name="schema-element-ssdl"></a>Schema – element (SSDL)

Element **Schema** v nástroji Store Schema Definition Language (SSDL) je kořenovým prvkem definice modelu úložiště. Obsahuje definice pro objekty, funkce a kontejnery, které tvoří model úložiště.

Element **Schema** může obsahovat nula nebo více z následujících podřízených elementů:

-   Řídí
-   Typ entity
-   Kontejneru
-   Funkce

Element **Schema** používá atribut **Namespace** k definování oboru názvů pro typ entity a objekty přidružení v modelu úložiště. V rámci oboru názvů nemohou mít žádné dva objekty stejný název.

Obor názvů modelu úložiště se liší od oboru názvů XML elementu **Schema** . Obor názvů modelu úložiště (jak je definováno atributem **Namespace** ) je logický kontejner pro typy entit a typy přidružení. Obor názvů XML (uvedený atributem **xmlns** ) elementu **Schema** je výchozí obor názvů pro podřízené elementy a atributy elementu **Schema** . Obory názvů XML formuláře https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (kde RRRR a MM představují rok a měsíc) jsou vyhrazené pro SSDL. Vlastní elementy a atributy nemůžou být v oborech názvů, které mají tento formulář.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **Schema** .

| Název atributu            | Je povinné | Hodnota                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Ano         | Obor názvů modelu úložiště. Hodnota atributu **Namespace** slouží k vytvoření plně kvalifikovaného názvu typu. Pokud je například **EntityType** s názvem *Zákazník* v oboru názvů ExampleModel. Store, pak plně kvalifikovaný název třídy **EntityType** je ExampleModel. Store. Customer. <br/> Následující řetězce nelze použít jako hodnotu pro atribut **Namespace** : **System**, **přechodný**nebo **EDM**. Hodnota atributu **Namespace** nemůže být stejná jako hodnota atributu **Namespace** v elementu schématu CSDL. |
| **Alias**                 | Ne          | Identifikátor použitý místo názvu oboru názvů. Pokud je například **EntityType** s názvem *Zákazník* v oboru názvů ExampleModel. Store a hodnota atributu **alias** je *StorageModel*, pak můžete jako plně kvalifikovaný název **objektu EntityType** použít StorageModel. Customer.                                                                                                                                                                                                                                                                                    |
| **Poskytovatel**              | Ano         | Zprostředkovatel dat.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Ano         | Token, který indikuje poskytovateli, který manifest Provider vrátí. Není definován žádný formát pro token. Hodnoty pro token jsou definovány zprostředkovatelem. Informace o tokenech manifestu poskytovatele SQL Server najdete v tématu SqlClient for Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **schématu** , který obsahuje element **EntityContainer** , dva elementy **EntityType** a jeden element **Association** .

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

## <a name="annotation-attributes"></a>Atributy poznámek

Atributy poznámek ve službě Store Schema Definition Language (SSDL) jsou vlastní atributy XML v modelu úložiště, které poskytují dodatečná metadata o prvcích v modelu úložiště. Kromě existence platné struktury XML platí následující omezení pro atributy poznámek:

-   Atributy poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro SSDL.
-   Plně kvalifikované názvy všech dvou atributů poznámek nesmí být stejné.

U daného elementu SSDL lze použít více než jeden atribut poznámky. K metadatům obsaženým v prvcích poznámky lze za běhu přistup použít třídy v oboru názvů System. data. Metadata. Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje element EntityType, který má atribut poznámky aplikovaný na vlastnost **ČísloObjednávky** . V příkladu se zobrazí také element poznámky přidaný do elementu **EntityType** .

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

## <a name="annotation-elements-ssdl"></a>Prvky poznámky (SSDL)

Prvky poznámek ve službě Store Schema Definition Language (SSDL) jsou vlastní prvky XML v modelu úložiště, které poskytují dodatečná metadata o modelu úložiště. Kromě existence platné struktury XML platí následující omezení pro prvky poznámky:

-   Prvky poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro SSDL.
-   Plně kvalifikované názvy všech dvou prvků poznámky nesmí být stejné.
-   Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích daného elementu SSDL.

Více než jeden element poznámky může být podřízeným prvku daného elementu SSDL. Počínaje verzí .NET Framework 4 je k metadatům obsaženým v prvcích poznámky možné přistupovat za běhu pomocí tříd v oboru názvů System. data. Metadata. Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje element EntityType, který má element Annotation (**CustomElement**). V příkladu se také zobrazuje atribut poznámky, který se použije na vlastnost **KódObjednávky** .

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

Charakteristiky v úložišti SSDL (Schema Definition Language) představují omezení pro typy sloupců, které jsou zadány v prvcích vlastností. Omezující vlastnosti jsou implementovány jako atributy XML v prvcích **vlastností** .

Následující tabulka obsahuje popis omezujících vlastností, které jsou podporovány ve službě SSDL:

| Omezující           | Popis                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Velké**   | Určuje pořadí kompletování (nebo řazení sekvence), které se má použít při provádění operací porovnání a řazení u hodnot vlastnosti.                                                                                                             |
| **FixedLength** | Určuje, zda může být délka hodnoty sloupce odlišná.                                                                                                                                                                                                  |
| **MaxLength**   | Určuje maximální délku hodnoty sloupce.                                                                                                                                                                                                           |
| **Číslic**   | Pro vlastnosti typu **Decimal**určuje počet číslic, které může mít hodnota vlastnosti. Pro vlastnosti typu **Time**, **DateTime**a **DateTimeOffset**určuje počet číslic pro zlomkovou část hodnoty sloupce v sekundách. |
| **Kapacity**       | Určuje počet číslic vpravo od desetinné čárky pro hodnotu sloupce.                                                                                                                                                                      |
| **Unicode**     | Určuje, zda je hodnota sloupce uložena jako Unicode.                                                                                                                                                                                                    |
