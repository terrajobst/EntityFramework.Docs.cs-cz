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
# <a name="csdl-specification"></a>Specifikace CSDL
Konceptuální schéma definici jazyka (CSDL) je jazyk založený na formátu XML, který popisuje entity, vztahy a funkce, které tvoří konceptuálního modelu aplikace řízené daty. Entity Framework nebo WCF Data Services je možné tento koncepčního modelu. Metadata, která je popsána pomocí CSDL používá Entity Framework pro mapování entit a vztahů, které jsou definované v konceptuálním modelu na zdroji dat. Další informace najdete v tématu [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) a [specifikace MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

Soubor CSDL je implementace Entity Framework modelu Entity Data Model.

V aplikaci Entity Framework koncepčního modelu metadat je načteno ze souboru .csdl (napsaného v CSDL) do instance System.Data.Metadata.Edm.EdmItemCollection a je přístupný pomocí metod v Třída System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework používá metadata koncepčního modelu dotazy na koncepční model příkazů specifických pro zdroj dat převodu.

EF designeru ukládá informace o konceptuální model v souboru .edmx v době návrhu. V okamžiku sestavení Návrhář EF používá informace v souboru .edmx vytvořit soubor .csdl, který je potřeba pro Entity Framework za běhu.

Verze CSDL jsou rozlišené pomocí obory názvů XML.

| Verze CSDL | Namespace XML                                |
|:-------------|:---------------------------------------------|
| Soubor CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| Soubor CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| Soubor CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Element Association (CSDL)

**Přidružení** element definuje vztah mezi dvěma typy entit. Asociace musíte zadat typy entit, které jsou zahrnuty v relaci a možný počet typů entit na každém konci vztahu, který je označován jako násobnost. Násobnost zakončení přidružení může mít hodnotu jedna (1), žádný nebo jeden (0..1) nebo mnoho (\*). Tato informace zadána v dva podřízené elementy End.

Instance typu entity na jednom konci asociace je přístupný prostřednictvím vlastnosti navigace nebo cizí klíče, pokud jsou zveřejněné na typ entity.

V aplikaci, která představuje instance přidružení konkrétní přidružení mezi instancemi typů entit. Přidružení instance jsou logicky rozdělena do nastavení přidružení.

**Přidružení** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   End (přesně 2 prvky)
-   Elementu ReferentialConstraint (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **přidružení** elementu.

| Název atributu | Vyžaduje se | Hodnota                        |
|:---------------|:------------|:-----------------------------|
| **Jméno**       | Ano         | Název přidružení. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **přidružení** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení při cizí klíče dosud byl zpřístupněn na **zákazníka** a  **Pořadí** typy entit. **Násobnost** hodnoty pro každou **koncový** přidružení označuje tolik **objednávky** lze přidružit **zákazníka**, ale pouze jeden **zákazníka** je možné přidružit **pořadí**. Kromě toho **elementy OnDelete** element znamená, že všechny **objednávky** , která se vztahují ke konkrétní **zákazníka** a byla načtena do objektu ObjectContext se odstraní. Pokud **zákazníka** se odstraní.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení při cizí klíče mají byl zpřístupněn na **zákazníka** a  **Pořadí** typy entit. S vystavený cizí klíče, je vztah mezi entitami spravované pomocí **elementu ReferentialConstraint** elementu. Odpovídající prvek AssociationSetMapping není nutné mapovat tato přidružení ke zdroji dat.

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
 

 

## <a name="associationset-element-csdl"></a>Element AssociationSet (CSDL)

**AssociationSet** element v Konceptuální schéma definici jazyka (CSDL) je logický kontejner pro instance přiřazení stejného typu. Skupinu přidružení obsahuje definici pro seskupení přidružení instance tak, aby se lze mapovat na zdroj dat.  

**AssociationSet** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden prvků povolený)
-   End (vyžaduje právě dva elementy)
-   Elementů poznámky (nula nebo více prvků povolený)

**Přidružení** atribut určuje typ přidružení, které obsahuje sadu přidružení. Sady entit, které tvoří konce sady přidružení se zadaným přesně dva podřízené **End** elementy.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **AssociationSet** elementu.

| Název atributu  | Vyžaduje se | Hodnota                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**        | Ano         | Název sady entit. Hodnota **název** atribut nemůže být stejná jako hodnota **přidružení** atribut.                                 |
| **Přidružení** | Ano         | Plně kvalifikovaný název přidružení, na kterou přidružení nastavení obsahuje instance. Přidružení musí být ve stejném oboru názvů jako sada přidružení. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **AssociationSet** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element se dvěma **AssociationSet** prvky:

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
 

 

## <a name="collectiontype-element-csdl"></a>Element CollectionType (CSDL)

**CollectionType** element v Konceptuální schéma definici jazyka (CSDL) určuje, že parametr funkce nebo funkce, návratový typ je kolekce. **CollectionType** element může být podřízený element parametru nebo element ReturnType (funkce). Typ kolekce se dá nastavit pomocí **typ** atribut nebo jednu z následujících podřízených elementů:

-   **Objekt CollectionType**
-   Hodnota ReferenceType
-   RowType
-   Odkaz TypeRef

> [!NOTE]
> Model neověří, pokud je zadán typ kolekce s oběma **typ** atribut a podřízený element.

 

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **CollectionType** elementu. Všimněte si, že **DefaultValue**, **MaxLength**, **FixedLength**, **přesnost**, **škálování**,  **Kódování Unicode**, a **kolace** atributy platí pouze pro kolekce **EDMSimpleTypes**.

| Název atributu                                                          | Vyžaduje se | Hodnota                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                                | Ne          | Typ kolekce.                                                                                                                                                                                                      |
| **S povolenou hodnotou Null**                                                            | Ne          | **Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                                 |
| > V CSDL v1, musí mít vlastnost komplexní typ `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **Výchozí hodnota**                                                        | Ne          | Výchozí hodnota vlastnosti.                                                                                                                                                                                               |
| **MaxLength**                                                           | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                        |
| **Hodnoty**                                                         | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.                                                                                                                           |
| **Přesnost**                                                           | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                             |
| **Škálování**                                                               | Ne          | Měřítko pro hodnotu vlastnosti.                                                                                                                                                                                                 |
| **SRID**                                                                | Ne          | Odkaz na identifikátor spatial systému. Platí pouze pro vlastnosti prostorových typů.   Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.                                                                                                                                |
| **Kolace**                                                           | Ne          | Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.                                                                                                                                                    |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **CollectionType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje funkce definované v modelu, která, která používá **CollectionType** elementu k určení, že funkce vrátí kolekci **osoba** typy entit (jako zadaný **ElementType** atributu).

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
 

Následující příklad ukazuje funkce definované v modelu, která používá **CollectionType** elementu k určení, že funkce vrátí kolekce řádků (jak je uvedeno v **RowType** element).

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
 

Následující příklad ukazuje funkce definované v modelu, která používá **CollectionType** elementu k určení, že funkce přijímá jako parametr kolekci **oddělení** typy entit.

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
 

 

## <a name="complextype-element-csdl"></a>Element ComplexType (CSDL)

A **ComplexType** element definuje datová struktura tvořený **EdmSimpleType** vlastnosti nebo jiné komplexní typy.  Komplexní typ může být vlastnost typu entity nebo jiného komplexního typu. Komplexní typ je podobný typ entity, komplexní typ definuje data. Existují však některé hlavní rozdíly mezi typy entit a komplexní typy:

-   Komplexní typy nemají identit (nebo klíče) a proto nemůže existovat nezávisle na sobě. Komplexní typy může existovat pouze jako typy entit nebo jiné komplexní typy vlastností.
-   Komplexní typy se nemůže podílet na přidružení. Ani end přidružení může být komplexní typ a proto nejde definovat navigační vlastnosti pro komplexní typy.
-   Vlastnost komplexní typ nemůže mít hodnotu null, i když Skalární vlastnosti složitého typu každý je možné nastavit na hodnotu null.

A **ComplexType** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Vlastnosti (nula nebo více prvků)
-   Elementů poznámky (nula nebo více prvků)

Následující tabulka popisuje atributy, které mohou být použity **ComplexType** elementu.

| Název atributu                                                                                                 | Vyžaduje se | Hodnota                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Název                                                                                                           | Ano         | Název komplexního typu. Název komplexního typu nemůže být stejný jako název jiného komplexní typ, typ entity nebo přidružení, které je v rámci oboru modelu. |
| BaseType                                                                                                       | Ne          | Název jiného komplexní typ, který je základním typem komplexní typ, který se zrovna definuje. <br/> [!NOTE]                                                                     |
| > Tento atribut není platných CSDL v1. V této verzi nepodporuje dědičnosti pro komplexní typy. |             |                                                                                                                                                                                     |
| Abstraktní                                                                                                       | Ne          | **Hodnota TRUE** nebo **False** (výchozí hodnota) v závislosti na tom, zda komplexní typ je typ abstraktní. <br/> [!NOTE]                                                                  |
| > Tento atribut není platných CSDL v1. Komplexní typy v této verzi nemohou být abstraktní typy.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ComplexType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje komplexní typ, **adresu**, se **EdmSimpleType** vlastnosti **StreetAddress**, **Město**,  **StátNeboKraj**, **země**, a **PSČ**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Chcete-li definovat komplexní typ **adresu** (výše) jako vlastnost typu entity, je třeba deklarovat typ vlastnosti v definici typu entity. Následující příklad ukazuje **adresu** vlastnost jako komplexní typ na typ entity (**vydavatele**):

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
 

 

## <a name="definingexpression-element-csdl"></a>Element DefiningExpression (CSDL)

**DefiningExpression** element v Konceptuální schéma definici jazyka (CSDL) obsahuje Entity SQL výraz, který definuje funkci v konceptuálním modelu.  

> [!NOTE]
> Pro účely ověření **DefiningExpression** element může obsahovat libovolný obsah. Nicméně, Entity Framework vyvolá výjimku za běhu Pokud **DefiningExpression** element neobsahuje platný Entity SQL.

 

### <a name="applicable-attributes"></a>Příslušné atributy

Libovolný počet atributů poznámky (vlastní atributy XML) lze na **DefiningExpression** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad používá **DefiningExpression** prvek, který chcete definovat funkci, která vrátí počet roků, protože knize publikoval. Obsah **DefiningExpression** element je napsána v Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Závislé – Element (CSDL)

**Závislé** element v Konceptuální schéma definici jazyka (CSDL) je podřízeným prvkem prvku elementu ReferentialConstraint a definuje závislé konec referenčního omezení. A **elementu ReferentialConstraint** element definuje funkci, která je podobná omezení referenční integrity v relační databázi. Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč z jiné tabulky lze vlastnost (nebo vlastnosti) typu entity odkazují na klíč entity, která jiný typ entity. Je volána typ entity, na který odkazuje *hlavní koncový* omezení. Typ entity, která odkazuje na konci instančního objektu je volána *závislé end* omezení. **PropertyRef** elementy se používají k určení klíče, které odkazují na konci instančního objektu.

**Závislé** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   PropertyRef (jeden nebo více prvků)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **závislé** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Role**       | Ano         | Název typu entity na závislé straně asociace. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **závislé** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **elementu ReferentialConstraint** element se používá jako součást definice **PublishedBy** přidružení. **PublisherId** vlastnost **knihy** typ entity tvoří závislé konec referenčního omezení.

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
 

 

## <a name="documentation-element-csdl"></a>Element Documentation (CSDL)

**Dokumentaci** element v Konceptuální schéma definici jazyka (CSDL) slouží k zadání informací o objektu, který je definován v nadřazeném elementu. V souboru .edmx při **dokumentaci** element je podřízeným prvkem elementu, který se zobrazí jako objekt na návrhové ploše návrháře EF (například entit, přidružení nebo vlastnost), obsah **dokumentace**  prvek se zobrazí v sadě Visual Studio **vlastnosti** okna pro objekt.

**Dokumentaci** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   **Souhrn**: stručný popis nadřazeného elementu. (žádný nebo jeden element)
-   **LongDescription**: rozsáhlý popis nadřazeného elementu. (žádný nebo jeden element)
-   Elementů poznámky. (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Libovolný počet atributů poznámky (vlastní atributy XML) lze na **dokumentaci** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **dokumentaci** element jako podřízený prvek elementu EntityType. Následující fragment kódu byl v CSDL obsah edmx soubor, obsah **Souhrn** a **LongDescription** prvky objevuje v aplikaci Visual Studio **vlastnosti** okno po kliknutí na `Customer` typu entity.

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
 

 

## <a name="end-element-csdl"></a>Element end (CSDL)

**End** element v Konceptuální schéma definici jazyka (CSDL) může být podřízeným prvkem elementu Association elementu nebo elementu AssociationSet. V každém případě role **End** element se liší a příslušné atributy se liší.

### <a name="end-element-as-a-child-of-the-association-element"></a>Koncový Element jako podřízený Element přidružení

**End** – element (jako podřízený objekt **přidružení** element) identifikuje typ entity na jednom konci asociace a počtu instancí typu entity, které mohou existovat za tímto účelem přidružení. Zakončení jsou definované jako součást přidružení; přidružení musí mít přesně dva elementy přidružení. Instance typu entity na jednom konci asociace lze přistupovat prostřednictvím vlastnosti navigace nebo cizí klíče, pokud jsou zveřejněné na typ entity.  

**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementy OnDelete (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **přidružení** elementu.

| Název atributu   | Vyžaduje se | Hodnota                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Ano         | Název typu entity na jednom konci přidružení.                                                                                                                                                                                                                                                                                                                                                         |
| **Role**         | Ne          | Název přidružení. Pokud není název zadán, použije se název typu entity na konci přidružení.                                                                                                                                                                                                                                                                                           |
| **Násobnost** | Ano         | **1**, **0..1**, nebo **\*** v závislosti na počtu instancí typu entity, které mohou být na konci přidružení. <br/> **1** znamená tuto instanci typu přesně jedna entita existuje na konci přidružení. <br/> **0..1** udává, že existuje žádnou nebo jednou instancí typu entity na konci přidružení. <br/> **\*** Udává, že existuje žádného, jednoho nebo více instancí typu entity na konci přidružení. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení. **Násobnost** hodnoty pro každou **koncový** přidružení označuje tolik **objednávky** lze přidružit **zákazníka**, ale pouze jeden **zákazníka** je možné přidružit **pořadí**. Kromě toho **elementy OnDelete** element znamená, že všechny **objednávky** , která se vztahují ke konkrétní **zákazníka** a, která byla načtena do objektu ObjectContext bude odstraněné if **zákazníka** se odstraní.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Koncový Element jako podřízený AssociationSet Element

**End** jednom konci sady přidružení určuje element. **AssociationSet** element musí obsahovat dvě **End** elementy. Informace obsažené v **End** element je použít v mapování sada ke zdroji dat přidružení.

**End** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

> [!NOTE]
> Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky. Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.

 

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **End** prvku, když je podřízeným **AssociationSet** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Objekt EntitySet**  | Ano         | Název **objektu EntitySet** element, který definuje jeden konec nadřazené **AssociationSet** elementu. **Objektu EntitySet** elementu musí být definován ve stejném kontejneru jako nadřazená entita **AssociationSet** elementu. |
| **Role**       | Ne          | Název přidružení, nastavit konec. Pokud **Role** atribut není použit, název elementu end přidružení sady bude název sady entit.                                                                   |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **End** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element se dvěma **AssociationSet** prvky, každého se dvěma **koncové** prvky:

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
 

 

## <a name="entitycontainer-element-csdl"></a>Element EntityContainer (CSDL)

**EntityContainer** prvek Konceptuální schéma definici jazyka (CSDL) je logický kontejner sady entit, sad přidružení nebo importované funkce. Kontejner entit koncepčního modelu mapuje na kontejner úložiště modelu entity pomocí elementu EntityContainerMapping. Struktura databáze popisuje kontejner úložiště modelu entity: sady entit popisují tabulky sad přidružení popisují omezení cizího klíče a importované funkce popisují uložené procedury v databázi.

**EntityContainer** element může mít nejvýše jedno dokumentace prvků. Pokud **dokumentaci** element je k dispozici, musí být všechny **objektu EntitySet**, **AssociationSet**, a **element FunctionImport** elementy.

**EntityContainer** prvek může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):

-   Objekt EntitySet
-   Element AssociationSet
-   Element FunctionImport
-   Elementů poznámky

Můžete rozšířit **EntityContainer** prvek, který chcete zahrnout obsah jiné **EntityContainer** , který je v rámci stejného oboru názvů. Zahrnout obsah jiné **EntityContainer**, v referenčních **EntityContainer** elementu, nastavte hodnotu **rozšiřuje** atribut name  **EntityContainer** element, který chcete zahrnout. Všechny podřízené prvky prvku zahrnutou **EntityContainer** elementu, bude zacházeno jako podřízené prvky prvku odkazujícího **EntityContainer** elementu.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **použití** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Jméno**       | Ano         | Název kontejneru entity.                               |
| **Rozšiřuje**    | Ne          | Název jiný kontejner entity v rámci stejného oboru názvů. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityContainer** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element, který definuje tři sad entit a sad přidružení dvě.

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
 

 

## <a name="entityset-element-csdl"></a>Element EntitySet (CSDL)

**Objektu EntitySet** element v Konceptuální schéma definici jazyka je logický kontejner pro instance typu entity a instance typu, který je odvozený z tohoto typu entity. Vztah mezi typem entity a sady entit je obdobou vztah mezi řádek a tabulky v relační databázi. Stejně jako řádek typ entity, který definuje množinu souvisejících dat a jako tabulka, obsahuje sadu entit instance definice. Sadu entit poskytuje konstrukci pro tak, aby může být namapována na související datové struktury ve zdroji dat seskupení instancí typu entity.  

Může být definován více než jednu sadu entit pro typ konkrétní entity.

> [!NOTE]
> EF designeru nepodporuje koncepčními modely, které obsahují více sad entit podle typu.

 

**Objektu EntitySet** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Element Documentation (žádný nebo jeden prvků povolený)
-   Elementů poznámky (nula nebo více prvků povolený)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **objektu EntitySet** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název sady entit.                                                              |
| **Typ entity** | Ano         | Plně kvalifikovaný název typu entity, pro kterou sada entit obsahuje instance. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **objektu EntitySet** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityContainer** element s tři **objektu EntitySet** prvky:

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
 

Je možné definovat více sad entit podle typu (MEST). Následující příklad definuje kontejner služby entity se dvěma sadami entity pro **knihy** typ entity:

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
 

 

## <a name="entitytype-element-csdl"></a>Element EntityType (CSDL)

**EntityType** element reprezentuje strukturu nejvyšší úrovně konceptů, jako je například zákazník nebo pořadí, v konceptuálním modelu. Typ entity je šablona pro instance typů entit v aplikaci. Každá šablona obsahuje následující informace:

-   Jedinečný název. (Povinné).
-   Klíč entity, který je definován jeden nebo více vlastností. (Povinné).
-   Vlastnosti obsahující data. (Volitelné).
-   Navigační vlastnosti, které umožňují navigace z jednoho konce asociace na druhém konci. (Volitelné).

V aplikaci, která představuje instance typu entity na konkrétní objekt (například konkrétního zákazníka nebo pořadí). Každá instance typu entity musí mít jedinečné entity klíč v rámci sady entit.

Dvě instance typu entity se považovat za rovné pouze v případě, že jsou stejného typu a hodnoty jejich klíče entity jsou stejné.

**EntityType** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Klíč (žádný nebo jeden element)
-   Vlastnosti (nula nebo více prvků)
-   Objekt NavigationProperty (nula nebo více prvků)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **EntityType** elementu.

| Název atributu                                                                                                                                  | Vyžaduje se | Hodnota                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Jméno**                                                                                                                                        | Ano         | Název typu entity.                                                                     |
| **BaseType**                                                                                                                                    | Ne          | Název jiného typu entity, která je základní typ, který definuje typ entity.  |
| **Abstraktní**                                                                                                                                    | Ne          | **Hodnota TRUE** nebo **False**, v závislosti na tom, jestli je typ entity abstraktní typ.                 |
| **OpenType**                                                                                                                                    | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, zda typ entity je typu open entity. <br/> [!NOTE] |
| > Tím **OpenType** atribut platí pouze pro typy entit, které jsou definovány v konceptuálních modelů, které se používají s využitím ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EntityType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** element s tři **vlastnost** elementy a dva **NavigationProperty** prvky:

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
 

 

## <a name="enumtype-element-csdl"></a>Element EnumType (CSDL)

**EnumType** představuje Výčtový typ elementu.

**EnumType** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Člen (nula nebo více prvků)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **EnumType** elementu.

| Název atributu     | Vyžaduje se | Hodnota                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**           | Ano         | Název typu entity.                                                                                                                                                                  |
| **IsFlags**        | Ne          | **Hodnota TRUE** nebo **False**, v závislosti na tom, zda typ výčtu může sloužit jako sada příznaků. Výchozí hodnota je **hodnotu False.**.                                                                     |
| **UnderlyingType** | Ne          | **Edm.Byte**, **Edm.Int16**, **typem Edm.Int32**, **Edm.Int64** nebo **Edm.SByte** definování rozsahu hodnot typu.   Výchozí základní typ výčtu prvků je **typem Edm.Int32.**. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **EnumType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **EnumType** element s tři **člen** prvky:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Element Function (CSDL)

**Funkce** element v Konceptuální schéma definici jazyka (CSDL) se používá k definování nebo deklarace funkcí v konceptuálním modelu. Funkce je definován pomocí DefiningExpression elementu.  

A **funkce** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Parametr (nula nebo více prvků)
-   DefiningExpression (žádný nebo jeden element)
-   Vlastnost ReturnType (Function) (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

Návratová typ pro funkci musí být určen buď **ReturnType** elementu (Function) nebo **ReturnType** atribut (viz níže), ale ne obojí. Je to možné návratové typy jsou všechny EdmSimpleType, entity typu, komplexní typ, typ řádku, nebo Typ ref (nebo kolekce jednoho z těchto typů).

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **funkce** elementu.

| Název atributu | Vyžaduje se | Hodnota                              |
|:---------------|:------------|:-----------------------------------|
| **Jméno**       | Ano         | Název funkce.          |
| **Vlastnost ReturnType** | Ne          | Typ vrácené funkcí. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **funkce** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad používá **funkce** prvek, který chcete definovat funkci, která vrátí počet roků, protože byl přijat instruktorem.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>Element FunctionImport (CSDL)

**Element FunctionImport** prvek v Konceptuální schéma definici jazyka (CSDL) představuje funkci, která je definovaná ve zdroji dat, ale k dispozici pro objekty prostřednictvím konceptuálního modelu. Například funkce elementu v modelu úložiště slouží k reprezentaci uloženou proceduru v databázi. A **element FunctionImport** element v konceptuálním modelu představuje odpovídající funkce v aplikaci Entity Framework a je namapována na funkci úložiště modelu s použitím elementu FunctionImportMapping. Při volání funkce v aplikaci, spustit je odpovídající uložené procedury v databázi.

**Element FunctionImport** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden prvků povolený)
-   Parametr (nula nebo více prvků povolený)
-   Elementů poznámky (nula nebo více prvků povolený)
-   Vlastnost ReturnType (element FunctionImport) (nula nebo více prvků povolený)

Jeden **parametr** element by měl být definován pro každý parametr, který přijímá funkce.

Návratová typ pro funkci musí být určen buď **ReturnType** – element (element FunctionImport) nebo **ReturnType** atribut (viz níže), ale ne obojí. Hodnota návratový typ musí být kolekce EdmSimpleType, EntityType nebo ComplexType.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **element FunctionImport** elementu.

| Název atributu   | Vyžaduje se | Hodnota                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**         | Ano         | Název importované funkce.                                                                                                                                                                    |
| **Vlastnost ReturnType**   | Ne          | Typ, který funkce vrátí. Tento atribut nepoužívejte, pokud funkce nevrací hodnotu. V opačném případě hodnota musí být kolekce ComplexType, EntityType nebo EDMSimpleType.        |
| **Objekt EntitySet**    | Ne          | Pokud funkce vrátí kolekci entit typy, hodnota **objektu EntitySet** musí být sada entit, ke které patří do kolekce. V opačném případě **objektu EntitySet** atribut nesmí být použit. |
| **IsComposable** | Ne          | Pokud je hodnota nastavena na hodnotu true, funkce je složení (funkce vracející tabulku) a lze použít v dotazu LINQ.  Výchozí hodnota je **false**.                                                           |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **element FunctionImport** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **element FunctionImport** element, který přijímá jeden parametr a vrátí kolekci typů entit:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Klíčovým prvkem (CSDL)

**Klíč** element je podřízeným prvkem elementu EntityType a definuje *klíč entity* (vlastnost nebo sadu vlastností typu entity, které určují identity). Vlastnosti, které tvoří klíč entity jsou vybrán v době návrhu. Hodnoty vlastnosti klíče entity musí jednoznačně identifikovat instanci entity typu v rámci entity nastavit v době běhu. Vlastnosti, které tvoří klíč entity, je třeba zvolit pro zajištění jedinečnosti instancí v sadu entit. **Klíč** element definuje klíč entity pomocí odkazu na jeden nebo více vlastností typu entity.

**Klíč** prvek může mít následujících podřízených elementů:

-   PropertyRef (jeden nebo více prvků)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Libovolný počet atributů poznámky (vlastní atributy XML) lze na **klíč** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad definuje typ entity s názvem **knihy**. Klíč entity je definována odkazováním **ISBN** vlastnost typu entity.

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
 

**ISBN** vlastnost je dobrou volbou pro klíč entity, protože mezinárodní čísla pro standardní knihy (ISBN) jednoznačně identifikuje knihy.

Následující příklad ukazuje typ entity (**Autor**), který má klíč entity, která se skládá ze dvou vlastností **název** a **adresu**.

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
 

Pomocí **název** a **adresu** entity klíč je vhodná volba, protože je nepravděpodobné, že za provozu na stejné adrese dvě autoři se stejným názvem. Nicméně tato volba pro klíč entity naprosto nezaručuje klíče jedinečné entity v sadě entit. Přidání vlastnosti, jako například **AuthorId**, který může použít k jednoznačné identifikaci Autor by v tomto případě doporučujeme.

 

## <a name="member-element-csdl"></a>Člen – Element (CSDL)

**Člen** element je podřízeným prvkem elementu EnumType a definuje člen výčtového typu.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **element FunctionImport** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název člena.                                                                                                                                                                  |
| **Hodnota**      | Ne          | Hodnota člena. Ve výchozím nastavení první člen má hodnotu 0 a hodnotu každé po sobě jdoucích čítač se zvyšuje o 1. Může existovat více členů se stejnými hodnotami. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **element FunctionImport** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **EnumType** element s tři **člen** prvky:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>Element NavigationProperty (CSDL)

A **NavigationProperty** element definuje vlastnost navigace, který poskytuje odkaz na druhém konci přidružení. Na rozdíl od vlastnosti definované s elementem vlastností navigačních vlastností nedefinují tvar a vlastnosti dat. Poskytují způsob, jak procházet přidružení mezi dvěma typy entit.

Všimněte si, že jsou volitelné na oba typy entit na konci asociace navigační vlastnosti. Pokud definujete navigační vlastnost jednoho typu entity na konci asociace, není nutné definovat vlastnost navigace typu entity na druhém konci přidružení.

Datový typ vracený navigační vlastnost se určuje podle násobnost ukončení vzdálené přidružení. Předpokládejme například, vlastnost navigace, **OrdersNavProp**, existuje na **zákazníka** typu entity a přejde na více přidružení mezi **zákazníka** a  **Pořadí**. Protože ukončení vzdálené přidružení pro navigační vlastnost má násobnost mnoho (\*), jeho datový typ je kolekce (z **pořadí**). Podobně, pokud vlastnost navigace, **CustomerNavProp**, existuje na **pořadí** typ entity, jeho datový typ by **zákazníka** protože má násobnost vzdáleným koncem jedna (1).

A **NavigationProperty** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **NavigationProperty** elementu.

| Název atributu   | Vyžaduje se | Hodnota                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**         | Ano         | Název navigační vlastnosti.                                                                                                                                                                                                             |
| **Relace** | Ano         | Název přidružení, které je v rámci oboru modelu.                                                                                                                                                                                |
| **ToRole**       | Ano         | End přidružení, na které koncích navigace. Hodnota **ToRole** atribut musí být stejná jako hodnota některého **Role** atributy definovaného u jednoho z zakončení (definovaný v elementu AssociationEnd).       |
| **FromRole**     | Ano         | Konec přidružení, ze kterého se spustí navigace. Hodnota **FromRole** atribut musí být stejná jako hodnota některého **Role** atributy definovaného u jednoho z zakončení (definovaný v elementu AssociationEnd). |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **NavigationProperty** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad definuje typ entity (**knihy**) s dvěma navigační vlastnosti (**PublishedBy** a **WrittenBy**):

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
 

 

## <a name="ondelete-element-csdl"></a>Elementy OnDelete – Element (CSDL)

**Elementy OnDelete** prvek v Konceptuální schéma definici jazyka (CSDL) definuje chování, která je propojená s přidružení. Pokud **akce** atribut je nastaven na **Cascade** na jednom konci asociace související s typy entity na druhém konci přidružení se odstraní při odstranění typu entity na konci prvního. Pokud je primární klíč primárního klíče vztah přidružení mezi dvěma typy entity a pak načíst závislý objekt se odstraní při odstranění objektu na druhém konci přidružení, bez ohledu na to **elementy OnDelete** specifikace.  

> [!NOTE]
> **Elementy OnDelete** element má vliv pouze chování modulu runtime aplikace; nemá vliv na chování ve zdroji dat. Chování definované ve zdroji dat by měl být stejný jako chování definované v aplikaci.

 

**Elementy OnDelete** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **elementy OnDelete** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Akce**     | Ano         | **Kaskádové** nebo **žádný**. Pokud **Cascade**, typy závislé entit se odstraní při odstranění typ objektu zabezpečení entity. Pokud **žádný**, typy závislé entit se neodstraní při odstraňování instančního objektu entity typu. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **přidružení** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **přidružení** element, který definuje **CustomerOrders** přidružení. **Elementy OnDelete** element znamená, že všechny **objednávky** , která se vztahují ke konkrétní **zákazníka** a byly načtené do objektu ObjectContext se odstraní při  **Zákazník** se odstraní.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter – Element (CSDL)

**Parametr** element v Konceptuální schéma definici jazyka (CSDL) může být podřízený FunctionImport element nebo elementu Function.

### <a name="functionimport-element-application"></a>FunctionImport Element aplikace

A **parametr** – element (jako podřízený objekt **element FunctionImport** element) se používá k definování vstupních a výstupních parametrů pro importované funkce, které jsou deklarovány v CSDL.

**Parametr** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden prvků povolený)
-   Elementů poznámky (nula nebo více prvků povolený)

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **parametr** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**       | Ano         | Název parametru                                                                                                                                                                                                      |
| **Typ**       | Ano         | Typ parametru. Hodnota musí být **EDMSimpleType** nebo komplexní typ, který spadá do rozsahu modelu.                                                                                                             |
| **Režim**       | Ne          | **V**, **si**, nebo **InOut** v závislosti na tom, zda je parametr vstup, výstup nebo vstupně výstupní parametr.                                                                                                                |
| **MaxLength**  | Ne          | Maximální povolená délka parametru.                                                                                                                                                                                    |
| **Přesnost**  | Ne          | Přesnost parametru.                                                                                                                                                                                                 |
| **Škálování**      | Ne          | Měřítko parametru.                                                                                                                                                                                                     |
| **SRID**       | Ne          | Odkaz na identifikátor spatial systému. Platí jenom pro parametry prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **parametr** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje **element FunctionImport** element s jedním **parametr** podřízený element. Funkce přijímá jeden vstupní parametr a vrátí kolekci typů entit.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Aplikaci prvku – funkce

A **parametr** – element (jako podřízený objekt **funkce** element) definuje parametry pro funkce, které jsou definována nebo deklarována v konceptuálním modelu.

**Parametr** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden prvky)
-   Objekt CollectionType (žádný nebo jeden prvky)
-   Hodnota ReferenceType (žádný nebo jeden prvky)
-   RowType (žádný nebo jeden prvky)

> [!NOTE]
> Pouze jeden z **CollectionType**, **ReferenceType**, nebo **RowType** prvků může být podřízený prvek **vlastnost** elementu.

 

-   Elementů poznámky (nula nebo více prvků povolený)

> [!NOTE]
> Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky. Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.

 

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **parametr** elementu.

| Název atributu   | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**         | Ano         | Název parametru                                                                                                                                                                                                      |
| **Typ**         | Ne          | Typ parametru. Parametr může být některý z následujících typů (nebo kolekcí z těchto typů): <br/> **EdmSimpleType** <br/> Typ entity <br/> komplexní typ <br/> Typ řádku <br/> odkazový typ                             |
| **S povolenou hodnotou Null**     | Ne          | **Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít **null** hodnotu.                                                                                                                          |
| **Výchozí hodnota** | Ne          | Výchozí hodnota vlastnosti.                                                                                                                                                                                              |
| **MaxLength**    | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **Hodnoty**  | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.                                                                                                                          |
| **Přesnost**    | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**        | Ne          | Měřítko pro hodnotu vlastnosti.                                                                                                                                                                                                |
| **SRID**         | Ne          | Odkaz na identifikátor spatial systému. Platí pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.                                                                                                                               |
| **Kolace**    | Ne          | Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.                                                                                                                                                   |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **parametr** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje **funkce** element, který používá jednu **parametr** podřízený element definovat parametr funkce.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a>Instanční objekt – Element (CSDL)

**Hlavní** element v Konceptuální schéma definici jazyka (CSDL) je podřízeným prvkem prvku elementu ReferentialConstraint, který definuje hlavní konec referenčního omezení. A **elementu ReferentialConstraint** element definuje funkci, která je podobná omezení referenční integrity v relační databázi. Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč z jiné tabulky lze vlastnost (nebo vlastnosti) typu entity odkazují na klíč entity, která jiný typ entity. Je volána typ entity, na který odkazuje *hlavní koncový* omezení. Typ entity, která odkazuje na konci instančního objektu je volána *závislé end* omezení. **PropertyRef** elementy se používají k určení klíče, které je odkazováno dle závislé end.

**Hlavní** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   PropertyRef (jeden nebo více prvků)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **hlavní** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Role**       | Ano         | Název typu entity na hlavní konec asociace. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **hlavní** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **elementu ReferentialConstraint** element, který je součástí definice **PublishedBy** přidružení. **Id** vlastnost **vydavatele** typ entity tvoří hlavní konec referenčního omezení.

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
 

 

## <a name="property-element-csdl"></a>Property – Element (CSDL)

**Vlastnost** element v Konceptuální schéma definici jazyka (CSDL) může být podřízený EntityType element, ComplexType element nebo RowType element.

### <a name="entitytype-and-complextype-element-applications"></a>Třída EntityType a ComplexType Element aplikací

**Vlastnost** prvky (jako podřízené objekty **EntityType** nebo **ComplexType** elementy) definovat tvar a vlastnosti dat, která bude obsahovat instance typu entity nebo komplexní typ instance . Vlastnosti v konceptuálním modelu jsou podobná vlastnosti, které jsou definovány ve třídě. Stejným způsobem, že vlastnosti třídy definovat tvar třídy a budou mít informace o objektech vlastnosti v konceptuálním modelu definovat tvar typu entity a nesou informaci o instancí typu entity.

**Vlastnost** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Element Documentation (žádný nebo jeden prvků povolený)
-   Elementů poznámky (nula nebo více prvků povolený)

Lze použít následující omezující vlastnosti **vlastnost** element: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **přesnost**, **škálování**, **Unicode**, **kolace**,  **Režim ConcurrencyMode**. Omezující vlastnosti jsou atributy ve formátu XML, které obsahují informace o způsobu uložení hodnoty vlastnosti dat v úložišti.

> [!NOTE]
> Charakteristiky může používat jedině pro vlastnosti typu **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **vlastnost** elementu.

| Název atributu                                                         | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**                                                               | Ano         | Název vlastnosti                                                                                                                                                                                                       |
| **Typ**                                                               | Ano         | Typ hodnoty vlastnosti. Musí být typ hodnoty vlastnosti **EDMSimpleType** nebo komplexní typ (označená plně kvalifikovaný název), který je v rámci oboru modelu.                                                 |
| **S povolenou hodnotou Null**                                                           | Ne          | **Hodnota TRUE** (výchozí hodnota) nebo <strong>False</strong> v závislosti na tom, zda tato vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                   |
| > CSDL v1 komplexní typ vlastnosti musí mít `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **Výchozí hodnota**                                                       | Ne          | Výchozí hodnota vlastnosti.                                                                                                                                                                                              |
| **MaxLength**                                                          | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **Hodnoty**                                                        | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.                                                                                                                          |
| **Přesnost**                                                          | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**                                                              | Ne          | Měřítko pro hodnotu vlastnosti.                                                                                                                                                                                                |
| **SRID**                                                               | Ne          | Odkaz na identifikátor spatial systému. Platí pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.                                                                                                                               |
| **Kolace**                                                          | Ne          | Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.                                                                                                                                                   |
| **Režim ConcurrencyMode**                                                    | Ne          | **Žádný** (výchozí hodnota) nebo **Fixed**. Pokud je hodnota nastavena na **Fixed**, bude použita hodnota vlastnosti v kontroly optimistické souběžnosti.                                                                                  |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **vlastnost** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** element s tři **vlastnost** prvky:

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
 

Následující příklad ukazuje **ComplexType** element s pěti **vlastnost** prvky:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType Element aplikace

**Vlastnost** prvky (jako podřízených prvků **RowType** element) definovat tvar a vlastnosti dat, které mohou být předány nebo vrácena z funkce modelově definovaných.  

**Vlastnost** prvek může mít nastavený právě jeden z následujících podřízených elementů:

-   Objekt CollectionType
-   Hodnota ReferenceType
-   RowType

**Vlastnost** prvek může mít libovolný počet podřízených elementů poznámky.

> [!NOTE]
> Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.

 

#### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **vlastnost** elementu.

| Název atributu                                                     | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Jméno**                                                           | Ano         | Název vlastnosti                                                                                                                                                                                                       |
| **Typ**                                                           | Ano         | Typ hodnoty vlastnosti.                                                                                                                                                                                                 |
| **S povolenou hodnotou Null**                                                       | Ne          | **Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                                |
| > CSDL v1 komplexní typ vlastnosti musí mít `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **Výchozí hodnota**                                                   | Ne          | Výchozí hodnota vlastnosti.                                                                                                                                                                                              |
| **MaxLength**                                                      | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **Hodnoty**                                                    | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.                                                                                                                          |
| **Přesnost**                                                      | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**                                                          | Ne          | Měřítko pro hodnotu vlastnosti.                                                                                                                                                                                                |
| **SRID**                                                           | Ne          | Odkaz na identifikátor spatial systému. Platí pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.                                                                                                                               |
| **Kolace**                                                      | Ne          | Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.                                                                                                                                                   |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **vlastnost** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje **vlastnost** prvků, které slouží k definování tvar návratový typ modelu uživatelsky definované funkce.

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
 

 

## <a name="propertyref-element-csdl"></a>Element PropertyRef (CSDL)

**PropertyRef** element v Konceptuální schéma definici jazyka (CSDL) odkazuje na vlastnost typu entity k označení, že vlastnost bude provádět jednu z následujících rolí:

-   Část klíče entity (vlastnost nebo sadu vlastností typu entity, které určují identity). Jeden nebo více **PropertyRef** prvky můžete použít k definování klíč entity.
-   Závislé nebo hlavní konec referenčního omezení.

**PropertyRef** element může obsahovat pouze prvky poznámky (nula nebo více) jako podřízené prvky.

> [!NOTE]
> Elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.

 

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **PropertyRef** elementu.

| Název atributu | Vyžaduje se | Hodnota                                |
|:---------------|:------------|:-------------------------------------|
| **Jméno**       | Ano         | Název odkazované vlastnosti. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **PropertyRef** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad definuje typ entity (**knihy**). Klíč entity je definována odkazováním **ISBN** vlastnost typu entity.

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
 

V následujícím příkladu dvě **PropertyRef** elementy se používají k označení tohoto dvě vlastnosti (**Id** a **PublisherId**) jsou konců objektem zabezpečení a závislými referenční omezení.

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
 

 

## <a name="referencetype-element-csdl"></a>Element ReferenceType (CSDL)

**ReferenceType** element v Konceptuální schéma definici jazyka (CSDL) určuje odkaz na typ entity. **ReferenceType** element může být podřízená následující prvky:

-   Vlastnost ReturnType (Function)
-   Parametr
-   Objekt CollectionType

**ReferenceType** element se používá při definování parametr nebo návratový typ pro funkci.

A **ReferenceType** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **ReferenceType** elementu.

| Název atributu | Vyžaduje se | Hodnota                                         |
|:---------------|:------------|:----------------------------------------------|
| **Typ**       | Ano         | Název typu entity, na kterou se odkazuje. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReferenceType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **ReferenceType** používá jako podřízený element **parametr** elementu v modelu definován funkci, která přijímá odkaz na **osoba** entity Typ:

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
 

Následující příklad ukazuje **ReferenceType** používá jako podřízený element **ReturnType** – element (funkce) v modelu definován funkci, která vrátí odkaz na **osoba**typ entity:

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
 

 

## <a name="referentialconstraint-element-csdl"></a>Element elementu ReferentialConstraint (CSDL)

A **elementu ReferentialConstraint** prvek v Konceptuální schéma definici jazyka (CSDL) definuje funkci, která je podobná omezení referenční integrity v relační databázi. Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč z jiné tabulky lze vlastnost (nebo vlastnosti) typu entity odkazují na klíč entity, která jiný typ entity. Je volána typ entity, na který odkazuje *hlavní koncový* omezení. Typ entity, která odkazuje na konci instančního objektu je volána *závislé end* omezení.

Pokud cizí klíč, který je zveřejněný na jednu entitu typu odkazuje na vlastnost na jiný typ entity, **elementu ReferentialConstraint** element definuje přidružení mezi tyto dva typy entity. Vzhledem k tomu, **elementu ReferentialConstraint** element poskytuje informace o tom, jak dva typy entit souvisejí, bez odpovídajícího prvku AssociationSetMapping je nezbytné v mapování specification language (MSL). Přidružení mezi dvěma typy entit, které nemají cizí klíče, které jsou vystaveny musí mít odpovídající **AssociationSetMapping** prvku abyste mohli mapovat na zdroj dat informací o přidružení.

Pokud cizí klíč není dostupná na typ entity, **elementu ReferentialConstraint** elementu lze definovat pouze klíč primárního omezení primárního klíče mezi typu entity a jiný typ entity.

A **elementu ReferentialConstraint** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Objekt zabezpečení (právě jeden element)
-   Závislé (právě jeden element)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

**Elementu ReferentialConstraint** prvek může mít libovolný počet atributů poznámky (vlastní atributy XML). Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje **elementu ReferentialConstraint** element se používá jako součást definice **PublishedBy** přidružení.

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
 

 

## <a name="returntype-function-element-csdl"></a>Element ReturnType (Function) (CSDL)

**ReturnType** – element (funkce) v Konceptuální schéma definici jazyka (CSDL) určuje návratový typ pro funkci, která je definována v elementu Function. Funkce, návratový typ je také možné zadat při **ReturnType** atribut.

Vrátí typy mohou být některé **EdmSimpleType**, typ entity, komplexní typ, typ řádku, typ odkazu nebo kolekce jednoho z těchto typů.

Návratový typ funkce se dá nastavit některým **typ** atribut **ReturnType** – element (funkce), nebo některou z následujících podřízených elementů:

-   Objekt CollectionType
-   Hodnota ReferenceType
-   RowType

> [!NOTE]
> Model neověří při zadání funkce, návratový typ se službami **typ** atribut **ReturnType** elementu (funkce) a jeden z podřízených prvků.

 

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **ReturnType** – element (funkce).

| Název atributu | Vyžaduje se | Hodnota                              |
|:---------------|:------------|:-----------------------------------|
| **Vlastnost ReturnType** | Ne          | Typ vrácené funkcí. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReturnType** – element (funkce). Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

V následujícím příkladu **funkce** prvek, který chcete definovat funkci, která vrátí počet roků knihy se při tisku. Všimněte si, že je návratový typ určený **typ** atribut **ReturnType** – element (funkce).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>Vlastnost ReturnType (element FunctionImport) – Element (CSDL)

**ReturnType** – element (element FunctionImport) v Konceptuální schéma definici jazyka (CSDL) určuje návratový typ pro funkci, která je definována v elementu FunctionImport. Funkce, návratový typ je také možné zadat při **ReturnType** atribut.

Vrátí typy mohou být jakoukoli kolekci typu entity, komplexní typ, nebo **EdmSimpleType**,

Návratový typ funkce se zadaným **typ** atribut **ReturnType** – element (element FunctionImport).

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **ReturnType** – element (element FunctionImport).

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**       | Ne          | Typ, který funkce vrátí. Hodnota musí být kolekce ComplexType, EntityType nebo EDMSimpleType.                                                                                      |
| **Objekt EntitySet**  | Ne          | Pokud funkce vrátí kolekci entit typy, hodnota **objektu EntitySet** musí být sada entit, ke které patří do kolekce. V opačném případě **objektu EntitySet** atribut nesmí být použit. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **ReturnType** – element (element FunctionImport). Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad používá **element FunctionImport** , která vrací knihy a vydavatelích. Všimněte si, že funkce vrátí dvou sad výsledků a tedy dvě **ReturnType** zadaných elementů (element FunctionImport).

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>Element RowType (CSDL)

A **RowType** prvek v Konceptuální schéma definici jazyka (CSDL) definuje nepojmenovanou strukturu jako parametr nebo návratový typ funkce definované v konceptuálním modelu.

A **RowType** element může být podřízená následující prvky:

-   Objekt CollectionType
-   Parametr
-   Vlastnost ReturnType (Function)

A **RowType** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Vlastnosti (jeden nebo více)
-   Elementů poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Příslušné atributy

Libovolný počet atributů poznámky (vlastní atributy XML) lze na **RowType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje funkce definované v modelu, která používá **CollectionType** elementu k určení, že funkce vrátí kolekce řádků (jak je uvedeno v **RowType** element).

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

## <a name="schema-element-csdl"></a>Element schématu (CSDL)

**Schématu** prvek je kořenovým elementem definici konceptuálního modelu. Obsahuje definice pro objekty, funkce a kontejnerů, které tvoří konceptuálního modelu.

**Schématu** element může obsahovat nula nebo více z následujících podřízených elementů:

-   Použití
-   EntityContainer
-   Typ entity
-   EnumType
-   Přidružení
-   Typ ComplexType
-   Funkce

A **schématu** element může obsahovat nejvýše jedno elementů poznámky.

> [!NOTE]
> **Funkce** elementu a elementů poznámky jsou povolené jenom v CSDL v2 nebo novější.

 

**Schématu** prvek používá **Namespace** atribut pro definování oboru názvů pro typ entity, komplexní typ a přidružení objekty v konceptuálním modelu. V rámci oboru názvů může mít žádné dva objekty se stejným názvem. Obory názvů může zahrnovat více **schématu** prvků a více souborů .csdl.

Obor názvů koncepčního modelu se liší od oboru názvů XML **schématu** elementu. Obor názvů koncepčního modelu (podle definice **Namespace** atribut) je logický kontejner pro typy entit a komplexní typy, přidružení typů. Obor názvů XML (indikován **xmlns** atribut) z **schématu** element je výchozí obor názvů pro podřízené prvky a atributy **schématu** elementu. Obory názvů XML formuláře http://schemas.microsoft.com/ado/YYYY/MM/edm (kde RRRR a MM představují roku a měsíce v uvedeném pořadí) jsou vyhrazené pro CSDL. Vlastní elementy a atributy nemohou být v oborech názvů, které mají tento formulář.

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy lze použít **schématu** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Ano         | Obor názvů konceptuálního modelu. Hodnota **Namespace** atribut se používá k vytvoření plně kvalifikovaný název typu. Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů Simple.Example.Model pak plně kvalifikovaný název **EntityType** je SimpleExampleModel.Customer. <br/> Následující řetězce nelze použít jako hodnotu **Namespace** atribut: **systému**, **přechodné**, nebo **Edm**. Hodnota **Namespace** atribut nemůže být stejná jako hodnota pro **Namespace** atribut v prvek schématu SSDL. |
| **Alias**      | Ne          | Identifikátor, použijí se místo názvu oboru názvů. Například pokud **EntityType** s názvem *zákazníka* je v oboru názvů Simple.Example.Model a hodnoty **Alias** atribut je *modelu*, můžete použít Model.Customer jako plně kvalifikovaný název **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **schématu** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **schématu** element, který obsahuje **EntityContainer** elementu, dvěma **EntityType** elementy a jeden **přidružení** elementu.

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
 

 

## <a name="typeref-element-csdl"></a>Element TypeRef (CSDL)

**TypeRef** element v Konceptuální schéma definici jazyka (CSDL) poskytuje odkaz na existující s názvem typu. **TypeRef** element může být podřízený element CollectionType, který se používá k určení, že má kolekce jako parametr nebo návratový typ funkce.

A **TypeRef** prvek může mít následující podřízené prvky (v uvedeném pořadí):

-   Dokumentace ke službě (žádný nebo jeden element)
-   Elementů poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy, které mohou být použity **TypeRef** elementu. Všimněte si, že **DefaultValue**, **MaxLength**, **FixedLength**, **přesnost**, **škálování**,  **Kódování Unicode**, a **kolace** atributy se vztahují jenom **EDMSimpleTypes**.

| Název atributu                                                     | Vyžaduje se | Hodnota                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                           | Ne          | Název typu, který se odkazuje.                                                                                                                                                                                          |
| **S povolenou hodnotou Null**                                                       | Ne          | **Hodnota TRUE** (výchozí hodnota) nebo **False** v závislosti na tom, zda tato vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                                |
| > CSDL v1 komplexní typ vlastnosti musí mít `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **Výchozí hodnota**                                                   | Ne          | Výchozí hodnota vlastnosti.                                                                                                                                                                                              |
| **MaxLength**                                                      | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **Hodnoty**                                                    | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec pevné délky.                                                                                                                          |
| **Přesnost**                                                      | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**                                                          | Ne          | Měřítko pro hodnotu vlastnosti.                                                                                                                                                                                                |
| **SRID**                                                           | Ne          | Odkaz na identifikátor spatial systému. Platí pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Ne          | **Hodnota TRUE** nebo **False** v závislosti na tom, jestli se uloží hodnoty vlastnosti jako řetězec s kódováním Unicode.                                                                                                                               |
| **Kolace**                                                      | Ne          | Řetězec, který určuje pořadí řazení, který se má použít ve zdroji dat.                                                                                                                                                   |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **CollectionType** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje funkce definované v modelu, která používá **TypeRef** – element (jako podřízený objekt **CollectionType** element) k určení, že funkce přijímá kolekce  **Oddělení** typy entit.

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
 

 

## <a name="using-element-csdl"></a>Pomocí elementu (CSDL)

**Použití** element v Konceptuální schéma definici jazyka (CSDL) Importuje obsah Koncepční model, který existuje v jiné oboru názvů. Nastavením hodnoty **Namespace** atributu, mohou odkazovat na typy entit, komplexní typy a typy přidružení, které jsou definovány v jiném koncepčního modelu. Více než jeden **použití** element může být podřízený **schématu** elementu.

> [!NOTE]
> **Použití** nebude fungovat stejně jako prvek v CSDL **pomocí** příkaz v programovacím jazyce. Importováním oboru názvů s **pomocí** příkaz v programovacím jazyce, nemají vliv objekty v oboru názvů původní. V CSDL importovaným oborem názvů může obsahovat typ entity, který je odvozen z typu entity v původní oboru názvů. To může ovlivnit sady entit deklarované v původní obor názvů.

 

**Použití** prvek může mít následujících podřízených elementů:

-   Dokumentace ke službě (žádný nebo jeden prvků povolený)
-   Elementů poznámky (nula nebo více prvků povolený)

### <a name="applicable-attributes"></a>Příslušné atributy

Následující tabulka popisuje atributy lze použít **použití** elementu.

| Název atributu | Vyžaduje se | Hodnota                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Ano         | Importované oboru názvů.                                                                                                                                                |
| **Alias**      | Ano         | Identifikátor, použijí se místo názvu oboru názvů. I když tento atribut je vyžadován, není nutné název oboru názvů se být zastoupen kvalifikovat názvy objektů. |

 

> [!NOTE]
> Libovolný počet atributů poznámky (vlastní atributy XML) lze na **použití** elementu. Uživatelských atributů, které však nemusí patřit do libovolný obor názvů XML, který je vyhrazen pro CSDL. Plně kvalifikovaných názvů pro všemi dvě vlastními atributy nemůže být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje, **použití** element se používá pro import oboru názvů, který je definovaný jinde. Všimněte si, že je obor názvů pro **schématu** uvedeného prvku je `BooksModel`. `Address` Vlastnost `Publisher` **EntityType** je komplexní typ, který je definován v `ExtendedBooksModel` obor názvů (importovat s **použití** element).

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
 

 

## <a name="annotation-attributes-csdl"></a>Atributy poznámek (CSDL)

Atributy poznámky v Konceptuální schéma definici jazyka (CSDL) jsou vlastní atributy XML v konceptuálním modelu. Kromě s platnou strukturu XML, musí být splněné anotace atributů následující podmínky:

-   Atributy poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro CSDL.
-   Více než jeden atribut poznámky se můžou vztahovat na daný prvek CSDL.
-   Plně kvalifikovaných názvů atributů dvě poznámky nesmí být stejné.

Anotace atributy je možné poskytnout další metadata o prvky v konceptuálním modelu. Metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** element s atributem poznámky (**Atribut CustomAttribute**). Příklad také ukazuje element poznámky na prvek typu entity.

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
 

Následující kód načte metadata v atributu poznámky a zapíše jej do konzoly:

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
 

Výše uvedený kód předpokládá, že `School.csdl` soubor v adresáři výstupu projektu a že jste přidali následující `Imports` a `Using` příkazy do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementů poznámky (CSDL)

Elementů poznámky v Konceptuální schéma definici jazyka (CSDL) jsou vlastní elementy XML v konceptuálním modelu. Kromě s platnou strukturu XML, musí být elementů poznámky platí následující podmínky:

-   Elementů poznámky nesmí být v libovolný obor názvů XML, který je vyhrazen pro CSDL.
-   Více než jeden element anotace může být podřízeným daného prvku CSDL.
-   Plně kvalifikovaných názvů všech elementů dvě poznámky nesmí být stejné.
-   Elementů poznámky se musí vyskytovat za všechny ostatní podřízené prvky daného prvku CSDL.

Elementů poznámky je možné poskytnout další metadata o prvky v konceptuálním modelu. Od verze rozhraní .NET Framework verze 4, metadat obsažených v elementů poznámky je možný za běhu pomocí tříd v oboru názvů System.Data.Metadata.Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje **EntityType** prvek s elementem poznámky (**CustomElement**). V příkladu také zobrazit poznámky atribut na prvek typu entity.

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
 

Následující kód načte metadata v elementu poznámky a zapíše jej do konzoly:

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
 

Výše uvedený kód předpokládá, že je School.csdl soubor v adresáři výstupu projektu a že jste přidali následující `Imports` a `Using` příkazy do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Typy konceptuálních modelů (CSDL)

Konceptuální schéma definici jazyka (CSDL) podporuje sadu abstraktní primitivní datové typy, volá **EDMSimpleTypes**, které definují vlastnosti v konceptuálním modelu. **EDMSimpleTypes** jsou proxy servery pro primitivní datové typy, které jsou podporovány ve službě storage nebo hostitelského prostředí.

Následující tabulka uvádí primitivní datové typy, které jsou podporovány CSDL. V tabulce jsou uvedeny také omezující vlastnosti, které lze použít u každého **EDMSimpleType**.

| EDMSimpleType                    | Popis                                                | Použít omezující vlastnosti                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | Obsahuje binární data.                                      | MaxLength, hodnoty, s možnou hodnotou Null, výchozí                                |
| **Typem Edm.Boolean**                  | Obsahuje hodnotu **true** nebo **false**.                  | S povolenou hodnotou Null, výchozí                                                        |
| **Edm.Byte**                     | Obsahuje hodnotu bez znaménka 8bitové celé číslo.                  | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.DateTime**                 | Představuje datum a čas.                                | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.DateTimeOffset**           | Obsahuje data a času jako posun během několika minut od GMT. | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Decimal**                  | Obsahuje číselná hodnota s pevnou hodnot precision a scale.   | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Double**                   | Obsahuje plovoucí desetinnou čárkou s přesností na 15 číslic čísla   | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Float**                    | Obsahuje bod s plovoucí desetinnou čárkou s přesností 7 číslic čísla.   | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Guid**                     | Obsahuje jedinečný identifikátor 16 bajtů.                      | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Int16**                    | Obsahuje hodnotu se znaménkem 16 bitů.                    | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Typem Edm.Int32**                    | Obsahuje hodnotu se znaménkem 32-bit.                    | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Int64**                    | Obsahuje hodnotu se znaménkem 64-bit.                    | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.SByte**                    | Obsahuje hodnotu se znaménkem 8 bitů.                     | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.String**                   | Obsahuje znaková data.                                   | Výchozí kódování Unicode, hodnoty, MaxLength, kolace, přesností, s možnou hodnotou Null, |
| **Edm.Time**                     | Obsahuje denní dobu.                                    | Přesnost, s možnou hodnotou Null, výchozí                                             |
| **Edm.Geography**                |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.Geometry**                 |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | S povolenou hodnotou Null, výchozí, SRID                                                  |

## <a name="facets-csdl"></a>Omezující vlastnosti (CSDL)

Omezující vlastnosti v Konceptuální schéma definici jazyka (CSDL) představují omezení na vlastnosti typů entit a komplexní typy. Omezující vlastnosti zobrazí jako atributy ve formátu XML na následujících prvcích CSDL:

-   Vlastnost
-   Odkaz TypeRef
-   Parametr

Následující tabulka popisuje omezující vlastnosti, které jsou podporovány v CSDL. Všechny omezující vlastnosti jsou volitelné. Některé omezujících vlastností uvedených níže používá Entity Framework při generování databáze z Koncepční model.

> [!NOTE]
> Informace o datových typech v konceptuálním modelu najdete v tématu typy koncepční Model (CSDL).

| Omezující vlastnost               | Popis                                                                                                                                                                                                                                                   | Platí pro                                                                                                                                                                                                                                                                                                                                                                           | Používá pro generování databáze | Použít modul runtime |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Kolace**       | Určuje pořadí řazení (nebo pořadí řazení) pro použití při provádění porovnání a řazení operací na hodnotách vlastnosti.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ne                  |
| **Režim ConcurrencyMode** | Označuje, že hodnota vlastnosti má být použita pro kontroly optimistické souběžnosti.                                                                                                                                                                    | Všechny **EDMSimpleType** vlastnosti                                                                                                                                                                                                                                                                                                                                                     | Ne                               | Ano                 |
| **Default**         | Určuje výchozí hodnotu vlastnosti, pokud není zadána žádná hodnota, při vytváření instance.                                                                                                                                                                       | Všechny **EDMSimpleType** vlastnosti                                                                                                                                                                                                                                                                                                                                                     | Ano                              | Ano                 |
| **Hodnoty**     | Určuje, zda se může lišit délka hodnoty vlastnosti.                                                                                                                                                                                                  | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ne                  |
| **MaxLength**       | Určuje maximální délku hodnoty vlastnosti.                                                                                                                                                                                                           | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ne                  |
| **S povolenou hodnotou Null**        | Určuje, zda tato vlastnost může mít **null** hodnotu.                                                                                                                                                                                                     | Všechny **EDMSimpleType** vlastnosti                                                                                                                                                                                                                                                                                                                                                     | Ano                              | Ano                 |
| **Přesnost**       | Pro vlastnosti typu **desítkové**, určuje počet číslic, může mít hodnotu vlastnosti. Pro vlastnosti typu **čas**, **data a času**, a **DateTimeOffset**, určuje počet číslic za desetinnou čárkou sady sekund hodnoty vlastnosti. | **Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | Ano                              | Ne                  |
| **Škálování**           | Určuje počet číslic vpravo od desetinné čárky pro hodnotu vlastnosti.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Ano                              | Ne                  |
| **SRID**            | Určuje ID prostorových systému odkaz na systém. Další informace najdete v tématu [SRID](http://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | Ne                               | Ano                 |
| **Unicode**         | Označuje, zda hodnota vlastnosti je uložen jako kódování Unicode.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ano                 |

>[!NOTE]
> Při generování z Koncepční model databáze, databáze průvodce generovat rozpozná hodnoty **výčet StoreGeneratedPattern** atribut na **vlastnost** elementu, jestliže je v následujícím obor názvů: http://schemas.microsoft.com/ado/2009/02/edm/annotation. Podporované hodnoty pro atribut **Identity** a **vypočítané**. Hodnota **Identity** vytvoří sloupec databáze hodnotu identity, který je vygenerován v databázi. Hodnota **vypočítané** vytvoří sloupec hodnot, které je vypočítán v databázi.

### <a name="example"></a>Příklad

Následující příklad ukazuje použitý pro vlastnosti typu entity omezující vlastnosti:

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
