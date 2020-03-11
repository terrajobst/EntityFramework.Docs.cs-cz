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
# <a name="csdl-specification"></a>Specifikace CSDL
Jazyk CSDL (konceptuální schéma Definition Language) je jazyk založený na jazyce XML, který popisuje entity, vztahy a funkce tvořící koncepční model aplikace řízené daty. Tento koncepční model lze použít Entity Framework nebo WCF Data Services. Metadata popsaná pomocí CSDL používá Entity Framework k mapování entit a vztahů, které jsou definovány v koncepčním modelu na zdroj dat. Další informace najdete v tématu Specifikace [SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) a [specifikace MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL je Entity Framework implementace model EDM (Entity Data Model).

V Entity Framework aplikaci jsou metadata konceptuálního modelu načtena ze souboru. csdl (napsaného v CSDL) do instance System. data. Metadata. Edm. EdmItemCollection a je přístupná pomocí metod v System. data. Metadata. Edm. MetadataWorkspace – třída Entity Framework používá metadata konceptuálního modelu k překladu dotazů na koncepční model na příkazy specifické pro zdroj dat.

Návrhář EF ukládá informace o koncepčním modelu do souboru. edmx v době návrhu. V okamžiku sestavení používá Návrhář EF informace v souboru. edmx k vytvoření souboru. csdl, který je vyžadován Entity Framework za běhu.

Verze CSDL jsou odlišeny obory názvů XML.

| Verze CSDL | Obor názvů XML                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | https://schemas.microsoft.com/ado/2008/09/edm |
| CSDL V3      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association – element (CSDL)

Element **Association** definuje vztah mezi dvěma typy entit. Přidružení musí určovat typy entit, které jsou součástí relace, a možný počet typů entit na každém konci relace, který se označuje jako násobnost. Násobnost elementu end přidružení může mít hodnotu jedna (1), 0 nebo 1 (0.. 1) nebo mnoho (\*). Tyto informace jsou zadány ve dvou podřízených elementech end.

Instance typu entity na jednom konci přidružení lze zpřístupnit prostřednictvím vlastností navigace nebo cizích klíčů, pokud jsou vystaveny na typu entity.

V aplikaci instance přidružení představuje konkrétní přidružení mezi instancemi typů entit. Instance přidružení jsou logicky seskupeny v sadě přidružení.

Element **Association** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   End (přesně 2 elementy)
-   Elementu ReferentialConstraint (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Association** .

| Název atributu | Je povinné | Hodnota                        |
|:---------------|:------------|:-----------------------------|
| **Název**       | Ano         | Název přidružení. |

 

> [!NOTE]
> Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** definující přidružení **CustomerOrders** v případě, že cizí klíče nebyly zveřejněny na typech entit **zákazníka** a **objednávky** . Hodnoty **násobnosti** pro každý **konec** přidružení označují, že mnoho **objednávek** může být přidruženo k **zákazníkovi**, ale k **objednávce**je možné přidružit pouze jednoho **zákazníka** . Kromě toho element **IsDeleted** označuje, že všechny **objednávky** , které souvisejí s konkrétním **zákazníkem** a byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

Následující příklad ukazuje element **Association** definující přidružení **CustomerOrders** , pokud byly cizí klíče vystaveny na typech entit **Zákazník** a **objednávka** . Při zpřístupnění cizích klíčů je vztah mezi entitami spravován pomocí elementu **elementu ReferentialConstraint** . Odpovídající element AssociationSetMapping není nutný k mapování tohoto přidružení ke zdroji dat.

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
 

 

## <a name="associationset-element-csdl"></a>AssociationSet – Element (CSDL)

Element **AssociationSet** v jazyce CSDL (konceptuální schéma Definition Language) je logický kontejner pro instance přidružení stejného typu. Sada přidružení poskytuje definici pro seskupení instancí přidružení, aby bylo možné je namapovat na zdroj dat.  

Element **AssociationSet** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (povolené nula nebo jeden prvek)
-   End (vyžaduje se přesně dva prvky)
-   Prvky poznámky (povoleny nula nebo více prvků)

Atribut **Association** určuje typ přidružení, které sada přidružení obsahuje. Sady entit, které tvoří konce sady přidružení, jsou zadány pomocí přesně dvou podřízených elementů **End** .

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSet** .

| Název atributu  | Je povinné | Hodnota                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**        | Ano         | Název sady entit. Hodnota atributu **Name** nemůže být stejná jako hodnota atributu **Association** .                                 |
| **Řídí** | Ano         | Plně kvalifikovaný název asociace, který sada přidružení obsahuje instance. Přidružení musí být ve stejném oboru názvů jako sada přidružení. |

 

> [!NOTE]
> Pro element **AssociationSet** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** se dvěma elementy **AssociationSet** :

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
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType – element (CSDL)

Element **CollectionType** v jazyce CSDL (konceptuální schéma Definition Language) určuje, že parametr funkce nebo návratový typ funkce je kolekce. Element **CollectionType** může být podřízeným elementem parametru nebo elementem ReturnType (Function). Typ kolekce lze zadat buď pomocí atributu **Type** , nebo jednoho z následujících podřízených elementů:

-   **CollectionType**
-   Hodnota ReferenceType
-   RowType
-   Odkaz

> [!NOTE]
> Model nebude ověřen, pokud je zadán typ kolekce s atributem **typu** i s podřízeným prvkem.

 

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **CollectionType** . Všimněte si, že atributy **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**a **kolace** platí pouze pro kolekce **EDMSimpleTypes**.

| Název atributu                                                          | Je povinné | Hodnota                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                                | Ne          | Typ kolekce                                                                                                                                                                                                      |
| **Povoleno**                                                            | Ne          | **True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                                 |
| > V CSDL V1 musí mít vlastnost komplexního typu `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **Hodnot**                                                        | Ne          | Výchozí hodnota vlastnosti                                                                                                                                                                                               |
| **MaxLength**                                                           | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                        |
| **FixedLength**                                                         | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.                                                                                                                           |
| **Číslic**                                                           | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                             |
| **Škálování**                                                               | Ne          | Měřítko hodnoty vlastnosti.                                                                                                                                                                                                 |
| **SRID**                                                                | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro vlastnosti prostorových typů.   Další informace najdete v tématech [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) . |
| **Unicode**                                                             | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.                                                                                                                                |
| **Velké**                                                           | Ne          | Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.                                                                                                                                                    |

 

> [!NOTE]
> Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci typů entit **Person** (jak je určeno atributem **ElementType** ).

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
 

Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).

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
 

Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce přijímá jako parametr kolekce typů entit **oddělení** .

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
 

 

## <a name="complextype-element-csdl"></a>ComplexType – element (CSDL)

Element **complexType** definuje strukturu dat složenou z vlastností **EdmSimpleType** nebo jiných komplexních typů.  Komplexní typ může být vlastnost typu entity nebo jiného komplexního typu. Komplexní typ je podobný typu entity v tom, že komplexní typ definuje data. Existují však některé klíčové rozdíly mezi komplexními typy a typy entit:

-   Komplexní typy nemají identity (nebo klíče), a proto nemohou existovat nezávisle. Komplexní typy mohou existovat pouze jako vlastnosti typů entit nebo jiných komplexních typů.
-   Komplexní typy se nemůžou účastnit přidružení. Ani konec přidružení může být komplexní typ, a proto nelze definovat vlastnosti navigace pro komplexní typy.
-   Vlastnost komplexního typu nemůže mít hodnotu null, ačkoliv skalární vlastnosti komplexního typu mohou být nastaveny na hodnotu null.

Element **complexType** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Vlastnost (nula nebo více prvků)
-   Prvky poznámky (nula nebo více prvků)

Následující tabulka popisuje atributy, které lze použít pro element **complexType** .

| Název atributu                                                                                                 | Je povinné | Hodnota                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Název                                                                                                           | Ano         | Název komplexního typu. Název komplexního typu nemůže být stejný jako název jiného komplexního typu, typu entity nebo asociace, který je v rámci oboru modelu. |
| BaseType                                                                                                       | Ne          | Název jiného komplexního typu, který je základním typem komplexního typu, který je definován. <br/> [!NOTE]                                                                     |
| > Tento atribut nelze použít v CSDL v1. Dědičnost pro komplexní typy není v této verzi podporována. |             |                                                                                                                                                                                     |
| Shrnutí                                                                                                       | Ne          | **True** nebo **false** (výchozí hodnota) v závislosti na tom, zda je komplexní typ abstraktní typ. <br/> [!NOTE]                                                                  |
| > Tento atribut nelze použít v CSDL v1. Komplexní typy v této verzi nemohou být abstraktní typy.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Pro element **complexType** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje komplexní typ a **adresu**s vlastností **EdmSimpleType** **StreetAddress**, **City**, **StateOrProvince**, **Country**a **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Chcete-li definovat **adresu** komplexního typu (výše) jako vlastnost typu entity, je nutné deklarovat typ vlastnosti v definici typu entity. Následující příklad ukazuje vlastnost **adresa** jako komplexní typ pro typ entity (**Vydavatel**):

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
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression – element (CSDL)

Element **DefiningExpression** v jazyce CSDL (konceptuální schéma Definition Language) obsahuje výraz Entity SQL, který definuje funkci v koncepčním modelu.  

> [!NOTE]
> Pro účely ověřování může element **DefiningExpression** obsahovat libovolný obsah. Nicméně Entity Framework vyvolá výjimku za běhu, pokud element **DefiningExpression** neobsahuje platný Entity SQL.

 

### <a name="applicable-attributes"></a>Použitelné atributy

Pro element **DefiningExpression** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad používá element **DefiningExpression** k definování funkce, která vrátí počet roků od publikování knihy. Obsah elementu **DefiningExpression** je napsaný v Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Závislý element (CSDL)

**Závislý** element v jazyce CSDL (konceptuální schéma Definition Language) je podřízeným elementem elementu elementu ReferentialConstraint a definuje závislý konec referenčního omezení. Element **elementu ReferentialConstraint** definuje funkčnost, která je podobná omezení referenční integrity v relační databázi. Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity. Odkazovaný typ entity se nazývá *hlavní konec* omezení. Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení. Prvky **PropertyRef** slouží k určení, které klíče odkazují na hlavní element end.

**Závislý** element může mít následující podřízené elementy (v uvedeném pořadí):

-   PropertyRef (jeden nebo více prvků)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na **závislý** element.

| Název atributu | Je povinné | Hodnota                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Role**       | Ano         | Název typu entity na závislém konci přidružení. |

 

> [!NOTE]
> Pro **závislý** element lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **elementu ReferentialConstraint** , který se používá jako součást definice asociace **PublishedBy** . Vlastnost **PublisherId** typu entity **Book** vytváří závislý konec referenčního omezení.

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
 

 

## <a name="documentation-element-csdl"></a>Element dokumentace (CSDL)

Element **dokumentace** v jazyce CSDL (konceptuální schéma Definition Language) lze použít k poskytnutí informací o objektu, který je definován v nadřazeném elementu. V souboru EDMX, pokud je prvek **dokumentace** podřízenosti prvku, který se zobrazuje jako objekt na návrhové ploše návrháře EF (například entita, asociace nebo vlastnost), obsah elementu **dokumentace** se zobrazí v okně **vlastností** sady Visual Studio pro daný objekt.

Prvek **dokumentace** může mít následující podřízené prvky (v uvedeném pořadí):

-   **Summary**: stručný popis nadřazeného elementu. (žádný nebo jeden element)
-   **Longdescription**: rozsáhlý popis nadřazeného elementu. (žádný nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Pro prvek **dokumentace** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **dokumentace** jako podřízený prvek elementu EntityType. Pokud následující fragment kódu byl v obsahu CSDL souboru. edmx, obsah **souhrnu** a elementů **Longdescription** se zobrazí v okně **vlastnosti** sady Visual Studio, když kliknete na typ entity `Customer`.

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
 

 

## <a name="end-element-csdl"></a>End – element (CSDL)

**Koncovým** elementem v jazyce CSDL (koncepční schéma Definition Language) může být podřízeným prvkem elementu Association nebo elementu AssociationSet. V každém případě je role elementu **End** odlišná a příslušné atributy se liší.

### <a name="end-element-as-a-child-of-the-association-element"></a>Ukončit element jako podřízený elementu přidružení

Element **End** (jako podřízený prvek elementu **Association** ) identifikuje typ entity na jednom konci přidružení a počet instancí typu entity, které mohou existovat na konci přidružení. Zakončení přidružení jsou definována jako součást přidružení; přidružení musí mít přesně dvě zakončení přidružení. Instance typu entity na jednom konci přidružení lze zpřístupnit prostřednictvím vlastností navigace nebo cizích klíčů, pokud jsou vystaveny na typu entity.  

Element **End** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Při odstranění (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízenou položkou elementu **Association** .

| Název atributu   | Je povinné | Hodnota                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**         | Ano         | Název typu entity na jednom konci přidružení.                                                                                                                                                                                                                                                                                                                                                         |
| **Role**         | Ne          | Název zakončení přidružení. Pokud není zadaný žádný název, použije se název typu entity na konci přidružení.                                                                                                                                                                                                                                                                                           |
| **Násobnost** | Ano         | **1**, **0.. 1**nebo **\*** v závislosti na počtu instancí typu entity, které mohou být na konci přidružení. <br/> **1** znamená, že na konci přidružení existuje přesně jedna instance typu entity. <br/> **0.. 1** znamená, že na konci přidružení existují žádné nebo jedna instance typu entity. <br/> **\*** označuje, že na konci přidružení existují žádné instance typu entity nula, jedna nebo více. |

 

> [!NOTE]
> Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** , který definuje přidružení **CustomerOrders** . Hodnoty **násobnosti** pro každý **konec** přidružení označují, že mnoho **objednávek** může být přidruženo k **zákazníkovi**, ale k **objednávce**je možné přidružit pouze jednoho **zákazníka** . Kromě toho element **IsDeleted** označuje, že všechny **objednávky** , které se vztahují k určitému **zákazníkovi** a které byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Ukončit element jako podřízený element elementu AssociationSet

Element **End** Určuje jeden konec sady přidružení. Element **AssociationSet** musí obsahovat dva elementy **End** . Informace obsažené v elementu **End** se používají v mapování sady přidružení na zdroj dat.

Element **End** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

> [!NOTE]
> Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích. Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.

 

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **End** , pokud je podřízeným elementem elementu **AssociationSet** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Sada**  | Ano         | Název elementu **EntitySet** , který definuje jeden element end nadřazeného elementu **AssociationSet** . Element **EntitySet** musí být definován ve stejném kontejneru entity jako nadřazený element **AssociationSet** . |
| **Role**       | Ne          | Název zakončení sady přidružení Pokud se atribut **role** nepoužívá, název zakončení sady přidružení bude název sady entit.                                                                   |

 

> [!NOTE]
> Pro element **End** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** se dvěma elementy **AssociationSet** , z nichž každý má dva elementy **End** :

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
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer – element (CSDL)

Element **EntityContainer** v jazyce CSDL (konceptuální schéma Definition Language) je logický kontejner pro sady entit, sady přidružení a importy funkcí. Kontejner entity koncepčního modelu se mapuje na kontejner entit modelu úložiště prostřednictvím elementu EntityContainerMapping. Kontejner entity modelu úložiště popisuje strukturu databáze: sady entit popisují tabulky, sady přidružení popisují omezení cizího klíče a importy funkcí popisují uložené procedury v databázi.

Element **EntityContainer** může mít nula nebo jeden prvek dokumentace. Pokud je přítomen prvek **dokumentace** , musí předcházet všechny elementy **EntitySet**, **AssociationSet**a **FunctionImport** .

Element **EntityContainer** může mít nula nebo více z následujících podřízených elementů (v uvedeném pořadí):

-   Sada
-   Vlastností
-   FunctionImport
-   Prvky poznámky

Element **EntityContainer** můžete roztáhnout tak, aby zahrnoval obsah jiného **elementu EntityContainer** , který je ve stejném oboru názvů. Chcete-li zahrnout obsah jiného **elementu EntityContainer**, nastavte v odkazujícím elementu **EntityContainer** hodnotu atributu **extends** na název elementu **EntityContainer** , který chcete zahrnout. Všechny podřízené elementy vloženého elementu **EntityContainer** budou považovány za podřízené prvky odkazujícího elementu **EntityContainer** .

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **using** .

| Název atributu | Je povinné | Hodnota                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Název**       | Ano         | Název kontejneru entity.                               |
| **Nachází**    | Ne          | Název jiného kontejneru entity v rámci stejného oboru názvů. |

 

> [!NOTE]
> Pro element **EntityContainer** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** , který definuje tři sady entit a dvě sady přidružení.

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
 

 

## <a name="entityset-element-csdl"></a>EntitySet – element (CSDL)

Element **EntitySet** v jazyce koncepční definice schématu je logický kontejner pro instance typu entity a instance libovolného typu, který je odvozen z daného typu entity. Vztah mezi typem entity a sadou entit je podobný relaci mezi řádkem a tabulkou v relační databázi. Podobně jako u řádku typ entity definuje sadu souvisejících dat, podobně jako tabulka, sada entit obsahuje instance této definice. Sada entit poskytuje konstrukci pro seskupování instancí typů entit tak, aby mohly být mapovány na související datové struktury ve zdroji dat.  

Je možné definovat více než jednu sadu entit pro konkrétní typ entity.

> [!NOTE]
> Návrhář EF nepodporuje koncepční modely, které obsahují více sad entit na typ.

 

Element **EntitySet** může mít následující podřízené elementy (v uvedeném pořadí):

-   Element dokumentace (povolené nula nebo jeden prvek)
-   Prvky poznámky (povoleny nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **EntitySet** .

| Název atributu | Je povinné | Hodnota                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název sady entit.                                                              |
| **Objektu** | Ano         | Plně kvalifikovaný název typu entity, pro který sada entit obsahuje instance. |

 

> [!NOTE]
> Pro element **EntitySet** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainer** se třemi elementy **EntitySet** :

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
 

Je možné definovat několik sad entit na typ (MEST). Následující příklad definuje kontejner entit se dvěma sadami entit pro typ entity **Book** :

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
 

 

## <a name="entitytype-element-csdl"></a>EntityType – element (CSDL)

Element **EntityType** reprezentuje strukturu konceptu nejvyšší úrovně, jako je například zákazník nebo objednávka, v koncepčním modelu. Typ entity je šablona pro instance typů entit v aplikaci. Každá šablona obsahuje následující informace:

-   Jedinečný název. (Povinné.)
-   Klíč entity, který je definován jednou nebo více vlastnostmi. (Povinné.)
-   Vlastnosti pro obsahující data (Volitelné.)
-   Navigační vlastnosti, které umožňují navigaci od jednoho konce přidružení k druhému konci. (Volitelné.)

V aplikaci instance typu entity představuje konkrétní objekt (například konkrétního zákazníka nebo objednávky). Každá instance typu entity musí mít v sadě entit jedinečný klíč entity.

Dvě instance typu entity jsou považovány za rovné pouze v případě, že jsou stejného typu a hodnoty jejich klíčů entit jsou stejné.

Element **EntityType** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Klíč (nula nebo jeden element)
-   Vlastnost (nula nebo více prvků)
-   NavigationProperty (nula nebo více prvků)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityType** .

| Název atributu                                                                                                                                  | Je povinné | Hodnota                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Název**                                                                                                                                        | Ano         | Název typu entity                                                                     |
| **BaseType**                                                                                                                                    | Ne          | Název jiného typu entity, který je základním typem entity typu, který je definován.  |
| **Výtah**                                                                                                                                    | Ne          | **Hodnota true** nebo **false**v závislosti na tom, zda je typ entity abstraktní typ.                 |
| **OpenType**                                                                                                                                    | Ne          | **True** nebo **false** v závislosti na tom, zda je typ entity typ otevřené entity. <br/> [!NOTE] |
| > Atributu **OpenType** platí pouze pro typy entit, které jsou definovány v koncepčních modelech, které jsou používány s Data Services ADO.NET. |             |                                                                                                  |

 

> [!NOTE]
> Pro element **EntityType** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** se třemi prvky **vlastností** a dvěma elementy **NavigationProperty** :

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
 

 

## <a name="enumtype-element-csdl"></a>EnumType – element (CSDL)

Element **enumType** reprezentuje Výčtový typ.

Element **enumType** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Člen (nula nebo více prvků)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **enumType** .

| Název atributu     | Je povinné | Hodnota                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**           | Ano         | Název typu entity                                                                                                                                                                  |
| **Příznak**        | Ne          | **Hodnota true** nebo **false**v závislosti na tom, zda lze typ výčtu použít jako sadu příznaků. Výchozí hodnota je **false.** .                                                                     |
| **UnderlyingType** | Ne          | **EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** nebo **EDM. SByte** definující rozsah hodnot typu.   Výchozí podkladový typ prvků výčtu je **EDM. Int32.** . |

 

> [!NOTE]
> Pro element **enumType** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **enumType** se třemi prvky **členů** :

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function – Element (CSDL)

Element **Function** v jazyce CSDL (konceptuální schéma Definition Language) se používá k definování nebo deklaraci funkcí v koncepčním modelu. Funkce je definována pomocí elementu DefiningExpression.  

Element **Function** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Parametr (nula nebo více prvků)
-   DefiningExpression (nula nebo jeden element)
-   ReturnType (Function) (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** (Function), nebo atributu **ReturnType** (viz níže), ale nikoli obou. Možné návratové typy jsou jakékoli EdmSimpleType, typ entity, komplexní typ, typ řádku nebo typ ref (nebo kolekce jednoho z těchto typů).

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Function** .

| Název atributu | Je povinné | Hodnota                              |
|:---------------|:------------|:-----------------------------------|
| **Název**       | Ano         | Název funkce          |
| **ReturnType** | Ne          | Typ vrácený funkcí |

 

> [!NOTE]
> Pro element **Function** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad používá element **Function** k definování funkce, která vrátí počet roků od přijetí instruktora.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport – element (CSDL)

Element **FunctionImport** v koncepčním definičním jazyce (CSDL) představuje funkci, která je definována ve zdroji dat, ale je k dispozici pro objekty prostřednictvím koncepčního modelu. Například prvek funkce v modelu úložiště lze použít k reprezentaci uložené procedury v databázi. Element **FunctionImport** v koncepčním modelu představuje odpovídající funkci v aplikaci Entity Framework a je mapována na funkci modelu úložiště pomocí elementu FunctionImportMapping. Když je funkce volána v aplikaci, odpovídající uložená procedura je spuštěna v databázi.

Element **FunctionImport** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (povolené nula nebo jeden prvek)
-   Parametr (povolený nula nebo více elementů)
-   Prvky poznámky (povoleny nula nebo více prvků)
-   ReturnType (FunctionImport) (povolené nula nebo více elementů)

Jeden prvek **parametru** by měl být definován pro každý parametr, který funkce přijímá.

Návratový typ pro funkci musí být zadán buď pomocí elementu **ReturnType** (FunctionImport), nebo atributu **ReturnType** (viz níže), ale nikoli obou. Hodnota návratového typu musí být kolekce elementů EdmSimpleType, EntityType nebo ComplexType.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **FunctionImport** .

| Název atributu   | Je povinné | Hodnota                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**         | Ano         | Název importované funkce                                                                                                                                                                    |
| **ReturnType**   | Ne          | Typ, který funkce vrátí. Nepoužívejte tento atribut, pokud funkce nevrátí hodnotu. V opačném případě musí být hodnotou kolekce ComplexType, EntityType nebo EDMSimpleType.        |
| **Sada**    | Ne          | Pokud funkce vrátí kolekci typů entit, hodnota **objektu EntitySet** musí být sada entit, do které kolekce patří. V opačném případě nesmí být použit atribut **EntitySet** . |
| **IsComposable** | Ne          | Pokud je hodnota nastavena na true, funkce je vytvořena (funkce vracející tabulku) a lze ji použít v dotazu LINQ.  Výchozí hodnota je **false**.                                                           |

 

> [!NOTE]
> Pro element **FunctionImport** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **FunctionImport** , který přijímá jeden parametr a vrací kolekci typů entit:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key – element (CSDL)

**Klíčový** prvek je podřízený prvek elementu EntityType a definuje *klíč entity* (vlastnost nebo sadu vlastností typu entity, který určuje identitu). Vlastnosti, které tvoří klíč entity, se volí v době návrhu. Hodnoty vlastností klíče entity musí jedinečně určovat instanci typu entity v sadě entit za běhu. Pro zajištění jedinečnosti instancí v sadě entit by se měla zvolit vlastnosti, které tvoří klíč entity. **Klíčový** prvek definuje klíč entity odkazem na jednu nebo více vlastností typu entity.

**Klíčový** prvek může mít následující podřízené prvky:

-   PropertyRef (jeden nebo více prvků)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Pro **klíčový** element lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad definuje typ entity s názvem **Book**. Klíč entity je definován pomocí odkazu na vlastnost **ISBN** typu entity.

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
 

Vlastnost **ISBN** je dobrou volbou pro klíč entity, protože mezinárodní číslo knihy (ISBN) jednoznačně identifikuje knihu.

Následující příklad ukazuje typ entity (**Autor**), který má klíč entity, který se skládá ze dvou vlastností, **názvů** a **adres**.

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
 

Použití **názvu** a **adresy** pro klíč entity je rozumnou volbou, protože dva autoři se stejným názvem pravděpodobně nebudou v provozu na stejné adrese. Tato volba pro klíč entity ale v sadě entit naprosto nezaručuje jedinečné klíče entit. V tomto případě se doporučuje přidat vlastnost, jako je například **AuthorID**, která se dá použít k jednoznačné identifikaci autora.

 

## <a name="member-element-csdl"></a>Element member (CSDL)

**Členský** prvek je podřízeným prvkem prvku enumType a definuje člena výčtového typu.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **FunctionImport** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název členu                                                                                                                                                                  |
| **Hodnota**      | Ne          | Hodnota člena Ve výchozím nastavení má první člen hodnotu 0 a hodnota každého úspěšného enumerátoru se zvýší o 1. Může existovat více členů se stejnými hodnotami. |

 

> [!NOTE]
> Pro element **FunctionImport** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **enumType** se třemi prvky **členů** :

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty – Element (CSDL)

Element **NavigationProperty** definuje vlastnost navigace, která poskytuje odkaz na druhý konec přidružení. Na rozdíl od vlastností, které jsou definovány pomocí elementu Property, navigační vlastnosti nedefinují tvar a vlastnosti dat. Poskytují způsob, jak procházet přidružení mezi dvěma typy entit.

Všimněte si, že navigační vlastnosti jsou volitelné na obou typech entit na konci přidružení. Definujete-li vlastnost navigace na jednom typu entity na konci přidružení, nemusíte definovat vlastnost navigace na typu entity na druhém konci přidružení.

Datový typ vrácený navigační vlastností je určen násobností jeho vzdáleného zakončení přidružení. Předpokládejme například, že vlastnost navigace **OrdersNavProp**existuje v typu entity **zákazníka** a prochází přidružení 1:1 mezi **zákazníkem** a **objednávkou**. Vzhledem k tomu, že vzdálené zakončení přidružení pro navigační vlastnost má mnoho (\*), je jeho datovým typem kolekce ( **pořadí**). Podobně platí, že pokud v typu entity **objednávky** existuje navigační vlastnost **CustomerNavProp**, její datový typ by byl **zákazníkem** , protože násobnost vzdáleného elementu end je jedna (1).

Element **NavigationProperty** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **NavigationProperty** .

| Název atributu   | Je povinné | Hodnota                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**         | Ano         | Název navigační vlastnosti.                                                                                                                                                                                                             |
| **Vztah** | Ano         | Název asociace, který je v rámci oboru modelu.                                                                                                                                                                                |
| **ToRole**       | Ano         | Konec přidružení, na kterém končí navigace. Hodnota atributu **ToRole** musí být stejná jako hodnota jednoho z atributů **role** definované v jednom z elementů Association (definovaných v elementu AssociationEnd).       |
| **FromRole**     | Ano         | Konec přidružení, ze kterého začíná navigace. Hodnota atributu **FromRole** musí být stejná jako hodnota jednoho z atributů **role** definované v jednom z elementů Association (definovaných v elementu AssociationEnd). |

 

> [!NOTE]
> Pro element **NavigationProperty** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad definuje typ entity (**Book**) se dvěma navigačními vlastnostmi (**PublishedBy** a **WrittenBy**):

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
 

 

## <a name="ondelete-element-csdl"></a>IsDeleted – element (CSDL)

Element **IsDeleted** v koncepčním definičním jazyce (CSDL) definuje chování, které je připojeno k přidružení. Je-li atribut **Action** nastaven na hodnotu **Cascade** na jednom konci přidružení, budou související typy entit na druhém konci přidružení odstraněny při odstranění typu entity v prvním elementu end. Pokud je přidružení mezi dvěma typy entit primárním vztahem klíče k primárnímu klíči, pak se načtený závislý objekt odstraní při odstranění objektu zabezpečení na druhém konci přidružení bez ohledu na specifikaci při **odstranění** .  

> [!NOTE]
> Element **IsDeleted** má vliv pouze na chování aplikace za běhu. nemá vliv na chování ve zdroji dat. Chování definované ve zdroji dat by mělo být stejné jako chování definované v aplikaci.

 

Element **IsDeleted** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **IsDeleted** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Akce**     | Ano         | **Cascade** nebo **none**. V případě, že se **kaskády**odstraní, odstraní se závislé typy entit při odstranění typu objektu zabezpečení. Pokud **žádný**není, typy závislých entit nebudou odstraněny při odstranění typu hlavní entity. |

 

> [!NOTE]
> Pro element **Association** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **Association** , který definuje přidružení **CustomerOrders** . Element **IsDeleted** označuje, že všechny **objednávky** , které se vztahují ke konkrétnímu **zákazníkovi** a které byly načteny do objektu ObjectContext, budou odstraněny při odstranění **zákazníka** .

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter – element (CSDL)

Element **Parameter** v koncepčním definičním jazyce (CSDL) může být podřízeným elementem FunctionImport nebo prvkem funkce.

### <a name="functionimport-element-application"></a>Element FunctionImport – aplikace

Element **parametru** (jako podřízený prvek elementu **FunctionImport** ) slouží k definování vstupních a výstupních parametrů pro importy funkce, které jsou deklarovány v CSDL.

Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (povolené nula nebo jeden prvek)
-   Prvky poznámky (povoleny nula nebo více prvků)

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Parameter** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název parametru                                                                                                                                                                                                      |
| **Typ**       | Ano         | Typ parametru. Hodnota musí být **EDMSimpleType** nebo komplexní typ, který je v rámci oboru modelu.                                                                                                             |
| **Mode**       | Ne          | **In**, **out**nebo **InOut** v závislosti na tom, zda je parametr vstupní, výstupní nebo vstupní/výstupní parametr.                                                                                                                |
| **MaxLength**  | Ne          | Maximální povolená délka parametru.                                                                                                                                                                                    |
| **Číslic**  | Ne          | Přesnost parametru.                                                                                                                                                                                                 |
| **Škálování**      | Ne          | Měřítko parametru.                                                                                                                                                                                                     |
| **SRID**       | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro parametry prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje element **FunctionImport** s jedním podřízeným elementem **parametru** . Funkce přijímá jeden vstupní parametr a vrací kolekci typů entit.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Aplikace prvku funkce

Element **Parameter** (jako podřízený prvek **funkce** ) definuje parametry pro funkce, které jsou definovány nebo deklarovány v koncepčním modelu.

Element **Parameter** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   CollectionType (nula nebo jeden element)
-   Hodnota ReferenceType (nula nebo jeden element)
-   RowType (nula nebo jeden element)

> [!NOTE]
> Pouze jeden z prvků typu **CollectionType**, **hodnota ReferenceType**nebo **RowType** může být podřízeným prvkem elementu **vlastnosti** .

 

-   Prvky poznámky (povoleny nula nebo více prvků)

> [!NOTE]
> Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích. Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.

 

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Parameter** .

| Název atributu   | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**         | Ano         | Název parametru                                                                                                                                                                                                      |
| **Typ**         | Ne          | Typ parametru. Parametr může být libovolný z následujících typů (nebo kolekcí těchto typů): <br/> **EdmSimpleType** <br/> entity type <br/> complex type <br/> Typ řádku <br/> odkazový typ                             |
| **Povoleno**     | Ne          | **True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu **null** .                                                                                                                          |
| **Hodnot** | Ne          | Výchozí hodnota vlastnosti                                                                                                                                                                                              |
| **MaxLength**    | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **FixedLength**  | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.                                                                                                                          |
| **Číslic**    | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**        | Ne          | Měřítko hodnoty vlastnosti.                                                                                                                                                                                                |
| **SRID**         | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.                                                                                                                               |
| **Velké**    | Ne          | Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.                                                                                                                                                   |

 

> [!NOTE]
> Pro element **Parameter** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **funkce** , který používá jeden podřízený element **Parameter** k definování parametru funkce.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Element zabezpečení (CSDL)

Element **Principal** v jazyce CSDL (konceptuální schéma Definition Language) je podřízeným prvkem elementu elementu ReferentialConstraint, který definuje hlavní konec referenčního omezení. Element **elementu ReferentialConstraint** definuje funkčnost, která je podobná omezení referenční integrity v relační databázi. Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity. Odkazovaný typ entity se nazývá *hlavní konec* omezení. Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení. Prvky **PropertyRef** slouží k určení, na které klíče se odkazuje závislý konec.

Element **Principal** může mít následující podřízené elementy (v uvedeném pořadí):

-   PropertyRef (jeden nebo více prvků)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **Principal** .

| Název atributu | Je povinné | Hodnota                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Role**       | Ano         | Název typu entity na hlavním konci přidružení. |

 

> [!NOTE]
> U elementu **Principal** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **elementu ReferentialConstraint** , který je součástí definice přidružení **PublishedBy** . Vlastnost **ID** typu entity **vydavatele** vytvoří hlavní konec referenčního omezení.

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
 

 

## <a name="property-element-csdl"></a>Property – element (CSDL)

Element **Property** v jazyce CSDL (koncepční schéma Definition Language) může být podřízeným elementem EntityType, elementem complexType nebo elementu RowType.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType a ComplexType – aplikace elementu

Prvky **vlastností** (jako podřízené prvky **EntityType** nebo **complexType** ) definují tvar a vlastnosti dat, které instance typu entity nebo instance komplexního typu budou obsahovat. Vlastnosti v koncepčním modelu jsou analogické k vlastnostem, které jsou definovány ve třídě. Stejně jako vlastnosti třídy definují tvar třídy a přenášejí informace o objektech, vlastnosti v koncepčním modelu definují tvar typu entity a přenášejí informace o instancích typu entity.

Element **Property** může mít následující podřízené elementy (v uvedeném pořadí):

-   Element dokumentace (povolené nula nebo jeden prvek)
-   Prvky poznámky (povoleny nula nebo více prvků)

Následující omezující vlastnosti lze použít na prvek **vlastnosti** : **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **COLLATE**, **ConcurrencyMode**. Omezující vlastnosti jsou atributy XML, které poskytují informace o tom, jak jsou hodnoty vlastností uloženy v úložišti dat.

> [!NOTE]
> Omezující vlastnosti lze použít pouze pro vlastnosti typu **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .

| Název atributu                                                         | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**                                                               | Ano         | Název vlastnosti                                                                                                                                                                                                       |
| **Typ**                                                               | Ano         | Typ hodnoty vlastnosti. Typ hodnoty vlastnosti musí být **EDMSimpleType** nebo komplexní typ (určený plně kvalifikovaným názvem), který je v rozsahu modelu.                                                 |
| **Povoleno**                                                           | Ne          | **True** (výchozí hodnota) nebo <strong>false</strong> v závislosti na tom, zda vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                   |
| > V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **Hodnot**                                                       | Ne          | Výchozí hodnota vlastnosti                                                                                                                                                                                              |
| **MaxLength**                                                          | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **FixedLength**                                                        | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.                                                                                                                          |
| **Číslic**                                                          | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**                                                              | Ne          | Měřítko hodnoty vlastnosti.                                                                                                                                                                                                |
| **SRID**                                                               | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.                                                                                                                               |
| **Velké**                                                          | Ne          | Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Ne          | **Žádná** (výchozí hodnota) nebo **pevná**. Pokud je hodnota nastavena na **pevná**, bude hodnota vlastnosti použita při kontrole optimistického řízení souběžnosti.                                                                                  |

 

> [!NOTE]
> Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** se třemi prvky **vlastností** :

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
 

Následující příklad ukazuje element **complexType** s pěti prvky **vlastností** :

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>Aplikace prvku RowType

Prvky **vlastností** (jako podřízené objekty elementu **RowType** ) definují tvar a charakteristiky dat, která lze předat nebo vrátit z modelu definované funkce.  

Element **Property** může mít přesně jeden z následujících podřízených elementů:

-   CollectionType
-   Hodnota ReferenceType
-   RowType

Element **Property** může mít libovolný počet podřízených elementů poznámky.

> [!NOTE]
> Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.

 

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **vlastnosti** .

| Název atributu                                                     | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**                                                           | Ano         | Název vlastnosti                                                                                                                                                                                                       |
| **Typ**                                                           | Ano         | Typ hodnoty vlastnosti.                                                                                                                                                                                                 |
| **Povoleno**                                                       | Ne          | **True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                                |
| > V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **Hodnot**                                                   | Ne          | Výchozí hodnota vlastnosti                                                                                                                                                                                              |
| **MaxLength**                                                      | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **FixedLength**                                                    | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.                                                                                                                          |
| **Číslic**                                                      | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**                                                          | Ne          | Měřítko hodnoty vlastnosti.                                                                                                                                                                                                |
| **SRID**                                                           | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.                                                                                                                               |
| **Velké**                                                      | Ne          | Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.                                                                                                                                                   |

 

> [!NOTE]
> Pro element **Property** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

#### <a name="example"></a>Příklad

Následující příklad ukazuje prvky **vlastností** používané k definování tvaru návratového typu funkce definované modelem.

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
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef – element (CSDL)

Element **PropertyRef** v jazyce CSDL (konceptuální schéma Definition Language) odkazuje na vlastnost typu entity, která označuje, že vlastnost provede jednu z následujících rolí:

-   Část klíče entity (vlastnost nebo sada vlastností typu entity, která určuje identitu). Jeden nebo více elementů **PropertyRef** lze použít k definování klíče entity.
-   Závislý nebo hlavní konec referenčního omezení.

Element **PropertyRef** může mít jako podřízené elementy pouze prvky poznámky (nula nebo více).

> [!NOTE]
> Prvky poznámek jsou povoleny pouze v CSDL v2 a novějších.

 

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **PropertyRef** .

| Název atributu | Je povinné | Hodnota                                |
|:---------------|:------------|:-------------------------------------|
| **Název**       | Ano         | Název odkazované vlastnosti |

 

> [!NOTE]
> Pro element **PropertyRef** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad definuje typ entity (**Book**). Klíč entity je definován pomocí odkazu na vlastnost **ISBN** typu entity.

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
 

V následujícím příkladu jsou použity dva prvky **PropertyRef** k označení toho, že dvě vlastnosti (**ID** a **PublisherId**) jsou objekty zabezpečení a závislé na konci referenčního omezení.

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
 

 

## <a name="referencetype-element-csdl"></a>Hodnota ReferenceType – element (CSDL)

Element **hodnota ReferenceType** v jazyce CSDL (konceptuální schéma Definition Language) určuje odkaz na typ entity. Element **hodnota ReferenceType** může být podřízeným prvkem následujících prvků:

-   ReturnType (Function)
-   Parametr
-   CollectionType

Element **hodnota ReferenceType** se používá při definování parametru nebo návratového typu pro funkci.

Element **hodnota ReferenceType** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **hodnota ReferenceType** .

| Název atributu | Je povinné | Hodnota                                         |
|:---------------|:------------|:----------------------------------------------|
| **Typ**       | Ano         | Název odkazovaného typu entity |

 

> [!NOTE]
> Pro element **hodnota ReferenceType** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **hodnota ReferenceType** , který se používá jako podřízený prvek **parametru** v rámci funkce definované modelem, která přijímá odkaz na typ entity **Person** :

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
 

Následující příklad ukazuje element **hodnota ReferenceType** , který se používá jako podřízený prvek **ReturnType** (Function) ve funkci definovaného modelu, která vrací odkaz na typ entity **Person** :

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
 

 

## <a name="referentialconstraint-element-csdl"></a>Elementu ReferentialConstraint – element (CSDL)

Element **elementu ReferentialConstraint** v jazyce CSDL (konceptuální schéma Definition Language) definuje funkce, které jsou podobné omezení referenční integrity v relační databázi. Stejným způsobem, že sloupec (nebo sloupce) z tabulky databáze může odkazovat na primární klíč jiné tabulky, může vlastnost (nebo vlastnosti) typu entity odkazovat na klíč entity jiného typu entity. Odkazovaný typ entity se nazývá *hlavní konec* omezení. Typ entity, který odkazuje na hlavní konec, se nazývá *závislý konec* omezení.

Pokud cizí klíč, který je vystaven na jednom typu entity, odkazuje na vlastnost v jiném typu entity, element **elementu ReferentialConstraint** definuje přidružení mezi dvěma typy entit. Vzhledem k tomu, že element **elementu ReferentialConstraint** poskytuje informace o tom, jak se vztahují dva typy entit, není v jazyce MSL (Mapping Specification Language) nutný žádný odpovídající element AssociationSetMapping. Přidružení mezi dvěma typy entit, které nemají vystavené cizími klíči, musí mít odpovídající element **AssociationSetMapping** , aby bylo možné mapovat informace o přidružení ke zdroji dat.

Pokud cizí klíč není vystavený pro typ entity, element **elementu ReferentialConstraint** může definovat jenom primární omezení klíče na primární klíč mezi typem entity a jiným typem entity.

Element **elementu ReferentialConstraint** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Objekt zabezpečení (právě jeden element)
-   Závislý (právě jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Element **elementu ReferentialConstraint** může mít libovolný počet atributů anotace (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **elementu ReferentialConstraint** , který se používá jako součást definice asociace **PublishedBy** .

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
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType – element (Function) (CSDL)

Element **ReturnType** (Function) v jazyce CSDL (konceptuální schéma Definition Language) určuje návratový typ pro funkci, která je definována v elementu Function. Návratový typ funkce lze také zadat s atributem **ReturnType** .

Návratové typy mohou být libovolné **EdmSimpleType**, typ entity, komplexní typ, typ řádku, typ odkazu nebo kolekce jednoho z těchto typů.

Návratový typ funkce lze zadat buď pomocí atributu **Type** elementu **ReturnType** (Function), nebo s jedním z následujících podřízených elementů:

-   CollectionType
-   Hodnota ReferenceType
-   RowType

> [!NOTE]
> Model nebude ověřen, pokud zadáte návratový typ funkce s atributem **Type** elementu **ReturnType** (Function) a jedním z podřízených elementů.

 

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **ReturnType** (Function).

| Název atributu | Je povinné | Hodnota                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Ne          | Typ vrácený funkcí |

 

> [!NOTE]
> Pro element **ReturnType** (Function) lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad používá element **Function** k definování funkce, která vrátí počet roků, po který se kniha tiskne. Všimněte si, že návratový typ je určen atributem **typu** elementu **ReturnType** (Function).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) – element (CSDL)

Element **ReturnType** (FunctionImport) v jazyce CSDL (konceptuální schéma Definition Language) určuje návratový typ pro funkci, která je definována v elementu FunctionImport. Návratový typ funkce lze také zadat s atributem **ReturnType** .

Návratové typy mohou být libovolné kolekce typu entity, komplexního typu nebo **EdmSimpleType**.

Návratový typ funkce je zadán s atributem **Type** elementu **ReturnType** (FunctionImport).

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít na element **ReturnType** (FunctionImport).

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**       | Ne          | Typ, který funkce vrátí. Hodnotou musí být kolekce ComplexType, EntityType nebo EDMSimpleType.                                                                                      |
| **Sada**  | Ne          | Pokud funkce vrátí kolekci typů entit, hodnota **objektu EntitySet** musí být sada entit, do které kolekce patří. V opačném případě nesmí být použit atribut **EntitySet** . |

 

> [!NOTE]
> Pro element **ReturnType** (FunctionImport) lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

V následujícím příkladu je použita funkce **FunctionImport** , která vrací knihy a vydavatele. Všimněte si, že funkce vrací dvě sady výsledků, a proto jsou zadány dva prvky **ReturnType** (FunctionImport).

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType – element (CSDL)

Element **RowType** v koncepčním definičním jazyce (CSDL) definuje nepojmenované struktury jako parametr nebo návratový typ pro funkci definovanou v koncepčním modelu.

Element **RowType** může být podřízeným prvkem následujících prvků:

-   CollectionType
-   Parametr
-   ReturnType (Function)

Element **RowType** může mít následující podřízené elementy (v uvedeném pořadí):

-   Vlastnost (jedna nebo více)
-   Prvky poznámky (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Pro element **RowType** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci definovanou modelem, která používá element **CollectionType** k určení, že funkce vrací kolekci řádků (jak je uvedeno v elementu **RowType** ).

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

## <a name="schema-element-csdl"></a>Schema – element (CSDL)

Element **Schema** je kořenovým prvkem definice koncepčního modelu. Obsahuje definice pro objekty, funkce a kontejnery, které tvoří koncepční model.

Element **Schema** může obsahovat nula nebo více z následujících podřízených elementů:

-   Použití
-   Kontejneru
-   Typ entity
-   EnumType
-   Řídí
-   Elementu
-   Funkce

Prvek **schématu** může obsahovat nula nebo jeden prvek poznámky.

> [!NOTE]
> Element **funkce** a prvky poznámky jsou povoleny pouze v CSDL v2 a novějším.

 

Element **Schema** používá atribut **Namespace** k definování oboru názvů pro typ entity, komplexní typ a objekty přidružení v koncepčním modelu. V rámci oboru názvů nemohou mít žádné dva objekty stejný název. Obory názvů mohou zahrnovat více prvků **schématu** a více souborů. csdl.

Obor názvů koncepčního modelu je jiný než obor názvů XML elementu **Schema** . Obor názvů koncepčního modelu (jak je definováno atributem **Namespace** ) je logický kontejner pro typy entit, komplexní typy a typy přidružení. Obor názvů XML (uvedený atributem **xmlns** ) elementu **Schema** je výchozí obor názvů pro podřízené elementy a atributy elementu **Schema** . Obory názvů XML ve formátu https://schemas.microsoft.com/ado/YYYY/MM/edm (kde RRRR a MM představují rok a měsíc) jsou vyhrazené pro CSDL. Vlastní elementy a atributy nemůžou být v oborech názvů, které mají tento formulář.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **Schema** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Hosting**  | Ano         | Obor názvů koncepčního modelu. Hodnota atributu **Namespace** slouží k vytvoření plně kvalifikovaného názvu typu. Pokud je například **EntityType** s názvem *Zákazník* v jednoduchém oboru názvů. example. model, pak plně kvalifikovaný název **objektu EntityType** je SimpleExampleModel. Customer. <br/> Následující řetězce nelze použít jako hodnotu pro atribut **Namespace** : **System**, **přechodný**nebo **EDM**. Hodnota atributu **Namespace** nemůže být stejná jako hodnota atributu **Namespace** v elementu schématu SSDL. |
| **Zástupný**      | Ne          | Identifikátor použitý místo názvu oboru názvů. Pokud je například **EntityType** s názvem *Zákazník* v jednoduchém oboru názvů. example. model a hodnota atributu **alias** je *model*, pak můžete použít model. Customer jako plně kvalifikovaný název **objektu EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Pro element **Schema** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje element **schématu** , který obsahuje element **EntityContainer** , dva elementy **EntityType** a jeden element **Association** .

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
 

 

## <a name="typeref-element-csdl"></a>TypeRef – element (CSDL)

Element **TypeRef** v jazyce CSDL (konceptuální schéma Definition Language) poskytuje odkaz na existující pojmenovaný typ. Element **TypeRef** může být podřízeným elementem CollectionType, který se používá k určení toho, že funkce má kolekci jako parametr nebo návratový typ.

Element **TypeRef** může mít následující podřízené elementy (v uvedeném pořadí):

-   Dokumentace (nula nebo jeden element)
-   Prvky poznámky (nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na prvek **TypeRef** . Všimněte si, že atributy **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**a **kolace** se vztahují pouze na **EDMSimpleTypes**.

| Název atributu                                                     | Je povinné | Hodnota                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typ**                                                           | Ne          | Název odkazovaného typu                                                                                                                                                                                          |
| **Povoleno**                                                       | Ne          | **True** (výchozí hodnota) nebo **false** v závislosti na tom, zda vlastnost může mít hodnotu null. <br/> [!NOTE]                                                                                                                |
| > V CSDL v1 vlastnost komplexního typu musí mít `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **Hodnot**                                                   | Ne          | Výchozí hodnota vlastnosti                                                                                                                                                                                              |
| **MaxLength**                                                      | Ne          | Maximální délka hodnoty vlastnosti.                                                                                                                                                                                       |
| **FixedLength**                                                    | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec s pevnou délkou.                                                                                                                          |
| **Číslic**                                                      | Ne          | Přesnost hodnoty vlastnosti.                                                                                                                                                                                            |
| **Škálování**                                                          | Ne          | Měřítko hodnoty vlastnosti.                                                                                                                                                                                                |
| **SRID**                                                           | Ne          | Identifikátor odkazu prostorového systému Platné pouze pro vlastnosti prostorových typů. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Ne          | **True** nebo **false** v závislosti na tom, jestli se hodnota vlastnosti uloží jako řetězec Unicode.                                                                                                                               |
| **Velké**                                                      | Ne          | Řetězec, který určuje pořadí kompletování, které má být použito ve zdroji dat.                                                                                                                                                   |

 

> [!NOTE]
> Pro element **CollectionType** může být použit libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje funkci definovanou modelem, která používá prvek **TypeRef** (jako podřízený prvek typu **CollectionType** ) k určení, že funkce přijímá kolekci typů entit **oddělení** .

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
 

 

## <a name="using-element-csdl"></a>Using – element (CSDL)

Element **using** v jazyce CSDL (konceptuální schéma Definition Language) importuje obsah koncepčního modelu, který existuje v jiném oboru názvů. Nastavením hodnoty atributu **Namespace** můžete odkazovat na typy entit, komplexní typy a typy přidružení, které jsou definovány v jiném koncepčním modelu. Více než jeden element **using** může být podřízeným elementu **schématu** .

> [!NOTE]
> Element **using** v CSDL nefunguje úplně stejně jako příkaz **using** v programovacím jazyce. Importováním oboru názvů pomocí příkazu **using** v programovacím jazyce neovlivníte objekty v původním oboru názvů. V CSDL může importovaný obor názvů obsahovat typ entity, který je odvozen z typu entity v původním oboru názvů. To může mít vliv na sady entit deklarované v původním oboru názvů.

 

Element **using** může mít následující podřízené prvky:

-   Dokumentace (povolené nula nebo jeden prvek)
-   Prvky poznámky (povoleny nula nebo více prvků)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít pro element **using** .

| Název atributu | Je povinné | Hodnota                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Hosting**  | Ano         | Název importovaného oboru názvů.                                                                                                                                                |
| **Zástupný**      | Ano         | Identifikátor použitý místo názvu oboru názvů. I když je tento atribut vyžadován, není vyžadováno, aby byl použit místo názvu oboru názvů k získání názvů objektů. |

 

> [!NOTE]
> Pro element **using** lze použít libovolný počet atributů poznámky (vlastní atributy XML). Vlastní atributy ale nemůžou patřit do žádného oboru názvů XML, který je vyhrazený pro CSDL. Plně kvalifikované názvy všech dvou vlastních atributů nemůžou být stejné.

 

### <a name="example"></a>Příklad

Následující příklad ukazuje **použití** prvku, který je použit pro import oboru názvů, který je definován jinde. Všimněte si, že obor názvů pro zobrazený element **schématu** je `BooksModel`. Vlastnost `Address` na `Publisher`**EntityType** je komplexní typ, který je definován v oboru názvů `ExtendedBooksModel` (importované pomocí elementu **using** ).

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
 

 

## <a name="annotation-attributes-csdl"></a>Atributy poznámek (CSDL)

Atributy poznámek v nástroji CSDL (konceptuální schéma Definition Language) jsou vlastní atributy XML v koncepčním modelu. Kromě toho, že mají platnou strukturu XML, musí mít následující hodnoty true atributů Anotace:

-   Atributy poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro CSDL.
-   U daného elementu CSDL lze použít více než jeden atribut poznámky.
-   Plně kvalifikované názvy všech dvou atributů poznámek nesmí být stejné.

Atributy poznámek lze použít k poskytnutí dalších metadat o prvcích v koncepčním modelu. K metadatům obsaženým v prvcích poznámky lze za běhu přistup použít třídy v oboru názvů System. data. Metadata. Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** s atributem poznámky (**CustomAttribute**). Příklad také ukazuje prvek poznámky aplikovaný na prvek typu entity.

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
 

Následující kód načte metadata v atributu anotace a zapíše je do konzoly:

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
 

Výše uvedený kód předpokládá, že soubor `School.csdl` je ve výstupním adresáři projektu a že jste přidali následující `Imports` a `Using` příkazy do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Prvky poznámek (CSDL)

Prvky poznámek v jazyce CSDL (konceptuální schéma Definition Language) jsou vlastní prvky XML v koncepčním modelu. Kromě toho, že mají platnou strukturu XML, musí mít následující hodnoty true pro prvky poznámky:

-   Prvky poznámky nesmí být v žádném oboru názvů XML, který je vyhrazený pro CSDL.
-   Více než jeden element poznámky může být podřízeným prvku daného elementu CSDL.
-   Plně kvalifikované názvy všech dvou prvků poznámky nesmí být stejné.
-   Prvky poznámky se musí vyskytovat po všech ostatních podřízených prvcích daného elementu CSDL.

Prvky poznámky lze použít k poskytnutí dalších metadat o prvcích v koncepčním modelu. Počínaje verzí .NET Framework 4 je k metadatům obsaženým v prvcích poznámky možné přistupovat za běhu pomocí tříd v oboru názvů System. data. Metadata. Edm.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityType** s elementem Annotation (**CustomElement**). Příklad také ukazuje atribut anotace aplikovaný na prvek typu entity.

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
 

Následující kód načte metadata v prvku anotace a zapíše jej do konzoly:

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
 

Výše uvedený kód předpokládá, že soubor School. csdl je ve výstupním adresáři projektu a že jste přidali následující `Imports` a `Using` příkazů do projektu:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Typy konceptuálních modelů (CSDL)

Model CSDL (konceptuální schéma Definition Language) podporuje sadu abstraktních primitivních datových typů s názvem **EDMSimpleTypes**, které definují vlastnosti v koncepčním modelu. **EDMSimpleTypes** jsou proxy pro primitivní datové typy, které jsou podporovány v úložišti nebo hostitelském prostředí.

Následující tabulka uvádí primitivní datové typy, které podporuje CSDL. Tabulka obsahuje také seznam omezujících vlastností, které lze použít na jednotlivé **EDMSimpleType**.

| EDMSimpleType                    | Popis                                                | Použitelné omezující vlastnosti                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **EDM. Binary**                   | Obsahuje binární data.                                      | MaxLength, FixedLength, Nullable, default                                |
| **Edm.Boolean**                  | Obsahuje hodnotu **true** nebo **false**.                  | Nullable, výchozí                                                        |
| **EDM. Byte**                     | Obsahuje 8bitové celočíselnou hodnotu bez znaménka.                  | Přesnost, Nullable, výchozí                                             |
| **EDM. DateTime**                 | Představuje datum a čas.                                | Přesnost, Nullable, výchozí                                             |
| **Edm.DateTimeOffset**           | Obsahuje datum a čas jako posun v minutách od času GMT. | Přesnost, Nullable, výchozí                                             |
| **EDM. Decimal**                  | Obsahuje číselnou hodnotu s pevnou přesností a škálováním.   | Přesnost, Nullable, výchozí                                             |
| **Edm.Double**                   | Obsahuje číslo s plovoucí desetinnou čárkou s přesností na 15 číslic.   | Přesnost, Nullable, výchozí                                             |
| **EDM. float**                    | Obsahuje číslo s plovoucí desetinnou čárkou s přesností na 7 číslic.   | Přesnost, Nullable, výchozí                                             |
| **EDM. GUID**                     | Obsahuje jedinečný identifikátor o velikosti 16 bajtů.                      | Přesnost, Nullable, výchozí                                             |
| **EDM. Int16**                    | Obsahuje 16bitová celočíselnou hodnotu se znaménkem.                    | Přesnost, Nullable, výchozí                                             |
| **Edm.Int32**                    | Obsahuje podepsaná 32 celočíselná hodnota.                    | Přesnost, Nullable, výchozí                                             |
| **Edm.Int64**                    | Obsahuje podepsaná 64 celočíselná hodnota.                    | Přesnost, Nullable, výchozí                                             |
| **EDM. SByte**                    | Obsahuje 8bitové celočíselnou hodnotu se znaménkem.                     | Přesnost, Nullable, výchozí                                             |
| **Edm.String**                   | Obsahuje znaková data.                                   | Unicode, FixedLength, MaxLength, kolace, přesnost, Nullable, default |
| **EDM. time**                     | Obsahuje denní dobu.                                    | Přesnost, Nullable, výchozí                                             |
| **EDM. geografie**                |                                                            | Nullable, Default, SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeographyLineString**      |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeographyPolygon**         |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeographyMultiPoint**      |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeographyMultiLineString** |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeographyMultiPolygon**    |                                                            | Nullable, Default, SRID                                                  |
| **EDM. geografie –Collection**      |                                                            | Nullable, Default, SRID                                                  |
| **EDM. Geometry**                 |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryPoint**            |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryLineString**       |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryPolygon**          |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryMultiPoint**       |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryMultiLineString**  |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryMultiPolygon**     |                                                            | Nullable, Default, SRID                                                  |
| **EDM. GeometryCollection**       |                                                            | Nullable, Default, SRID                                                  |

## <a name="facets-csdl"></a>Omezující vlastnosti (CSDL)

Charakteristiky v jazyce CSDL (konceptuální schéma Definition Language) reprezentují omezení vlastností typů entit a komplexních typů. Omezující vlastnosti se zobrazí jako atributy XML u následujících elementů CSDL:

-   Vlastnost
-   Odkaz
-   Parametr

Následující tabulka obsahuje popis omezujících vlastností, které jsou podporovány v CSDL. Všechny omezující vlastnosti jsou volitelné. Některé omezující vlastnosti, které jsou uvedeny níže, jsou používány Entity Framework při generování databáze z koncepčního modelu.

> [!NOTE]
> Informace o datových typech v koncepčním modelu naleznete v tématu typy konceptuálních modelů (CSDL).

| Omezující               | Popis                                                                                                                                                                                                                                                   | Platí pro                                                                                                                                                                                                                                                                                                                                                                           | Používá se pro generování databáze. | Používáno modulem runtime |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Velké**       | Určuje pořadí kompletování (nebo řazení sekvence), které se má použít při provádění operací porovnání a řazení u hodnot vlastnosti.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ne                  |
| **ConcurrencyMode** | Označuje, že hodnota vlastnosti by měla být použita pro kontroly optimistického řízení souběžnosti.                                                                                                                                                                    | Všechny vlastnosti **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Ne                               | Ano                 |
| **Výchozí**         | Určuje výchozí hodnotu vlastnosti, pokud není při vytváření instance zadána žádná hodnota.                                                                                                                                                                       | Všechny vlastnosti **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Ano                              | Ano                 |
| **FixedLength**     | Určuje, zda může být délka hodnoty vlastnosti odlišná.                                                                                                                                                                                                  | **EDM. Binary**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ne                  |
| **MaxLength**       | Určuje maximální délku hodnoty vlastnosti.                                                                                                                                                                                                           | **EDM. Binary**, **EDM. String**                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ne                  |
| **Povoleno**        | Určuje, zda vlastnost může mít hodnotu **null** .                                                                                                                                                                                                     | Všechny vlastnosti **EDMSimpleType**                                                                                                                                                                                                                                                                                                                                                     | Ano                              | Ano                 |
| **Číslic**       | Pro vlastnosti typu **Decimal**určuje počet číslic, které může mít hodnota vlastnosti. Pro vlastnosti typu **Time**, **DateTime**a **DateTimeOffset**určuje počet číslic pro zlomkovou část hodnoty vlastnosti v sekundách. | **EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **EDM. time**                                                                                                                                                                                                                                                                                                              | Ano                              | Ne                  |
| **Škálování**           | Určuje počet číslic vpravo od desetinné čárky pro hodnotu vlastnosti.                                                                                                                                                                      | **EDM. Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Ano                              | Ne                  |
| **SRID**            | Určuje ID referenčního systému prostorového systému. Další informace najdete v tématu [SRID](https://en.wikipedia.org/wiki/SRID) a [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **EDM. geografie, Edm. GeographyPoint, Edm. GeographyLineString, Edm. GeographyPolygon, Edm. GeographyMultiPoint, Edm. GeographyMultiLineString, Edm. GeographyMultiPolygon, Edm. geografie, Edm. Geometry, Edm. GeometryPoint, EDM. GeometryLineString, Edm. GeometryPolygon, Edm. GeometryMultiPoint, Edm. GeometryMultiLineString, Edm. GeometryMultiPolygon, Edm. GeometryCollection** | Ne                               | Ano                 |
| **Unicode**         | Určuje, zda je hodnota vlastnosti uložena jako Unicode.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Ano                              | Ano                 |

>[!NOTE]
> Při generování databáze z koncepčního modelu Průvodce generováním databáze rozpozná hodnotu atributu **StoreGeneratedPattern** elementu **Property** , pokud je v následujícím oboru názvů: https://schemas.microsoft.com/ado/2009/02/edm/annotation. Podporované hodnoty atributu jsou **identity** a **počítané**. Hodnota **identity** vytvoří sloupec databáze s hodnotou identity, která je vygenerována v databázi. Hodnota **počítaná** vygeneruje sloupec s hodnotou, která je vypočítána v databázi.

### <a name="example"></a>Příklad

Následující příklad ukazuje charakteristiky použité pro vlastnosti typu entity:

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
