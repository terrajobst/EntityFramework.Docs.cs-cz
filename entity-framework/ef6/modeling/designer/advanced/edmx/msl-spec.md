---
title: Specifikace MSL – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182558"
---
# <a name="msl-specification"></a>Specifikace MSL
Specifikace jazyka MSL (Mapping Specification Language) je jazyk založený na jazyce XML, který popisuje mapování mezi koncepčním modelem a modelem úložiště aplikace Entity Framework.

V Entity Framework aplikaci jsou mapování metadat načtena ze souboru. MSL (napsaného v MSL) v čase sestavení. Entity Framework používá mapování metadat za běhu k překladu dotazů na koncepční model pro ukládání specifických příkazů.

Entity Framework Designer (EF Designer) ukládá informace o mapování v souboru EDMX v době návrhu. V době sestavování Entity Designer používá informace v souboru. edmx k vytvoření souboru. MSL, který je vyžadován Entity Framework za běhu.

Názvy všech typů konceptuálního modelu nebo modelů úložiště, které jsou odkazovány v MSL, musí být kvalifikovány svými příslušnými názvy oborů názvů. Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu [specifikace CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Informace o názvu oboru názvů modelu úložiště najdete v tématu [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Verze MSL jsou odlišeny obory názvů XML.

| Verze MSL | Obor názvů XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com: Windows: Storage: mapování: CS |
| MSL v2      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| MSL V3      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Alias – element (MSL)

Element **alias** v jazyce MSL (Mapping Specification Language) je podřízeným prvkem mapování, který se používá k definování aliasů pro koncepční model a obory názvů modelu úložiště. Názvy všech typů konceptuálního modelu nebo modelů úložiště, které jsou odkazovány v MSL, musí být kvalifikovány svými příslušnými názvy oborů názvů. Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu Schema – element (CSDL). Informace o názvu oboru názvů modelu úložiště najdete v tématu schéma – element (SSDL).

Element **alias** nemůže mít podřízené elementy.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít na prvek **alias** .

| Název atributu | Je povinné | Value                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Klíč**        | Ano         | Alias pro obor názvů, který je určen atributem **Value** . |
| **Hodnota**      | Ano         | Obor názvů, pro který je hodnota **klíčového** prvku alias.     |

### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **alias** definující alias `c` pro typy, které jsou definovány v koncepčním modelu.

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a>AssociationEnd – element (MSL)

Element **AssociationEnd** v jazyce MSL (Mapping Specification Language) se používá, pokud jsou funkce úprav typu entity v koncepčním modelu mapovány na uložené procedury v podkladové databázi. Pokud uložená procedura úpravy převezme parametr, jehož hodnota je uložena ve vlastnosti Association, element **AssociationEnd** mapuje hodnotu vlastnosti na parametr. Další informace naleznete v níže uvedeném příkladu.

Další informace o mapování funkcí úprav typů entit na uložené procedury naleznete v tématu ModificationFunctionMapping element (MSL) a návod: Mapování entity na uložené procedury.

Element **AssociationEnd** může mít následující podřízené prvky:

-   ScalarProperty

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na element **AssociationEnd** .

| Název atributu     | Je povinné | Value                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Vlastností** | Ano         | Název asociace, který je namapován.                                                                                                                                 |
| **from**           | Ano         | Hodnota atributu **FromRole** vlastnosti navigace, která odpovídá mapovanému přidružení. Další informace naleznete v tématu element NavigationProperty (CSDL). |
| **To**             | Ano         | Hodnota atributu **ToRole** vlastnosti navigace, která odpovídá mapovanému přidružení. Další informace naleznete v tématu element NavigationProperty (CSDL).   |

### <a name="example"></a>Příklad

Vezměte v úvahu následující typ entity koncepčního modelu:

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

Zvažte také následující uloženou proceduru:

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

Aby bylo možné namapovat funkci Update entity `Course` na tuto uloženou proceduru, je nutné do parametru **DepartmentID** dodat hodnotu. Hodnota parametru `DepartmentID` neodpovídá vlastnosti typu entity; je obsažen v nezávislém přidružení, jehož mapování je zobrazeno zde:

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

Následující kód ukazuje element **AssociationEnd** , který slouží k mapování vlastnosti **DepartmentID** třídy **FK @ no__t-3Course @ no__t-4Department** na uloženou proceduru **UpdateCourse** (do které funkce Update Typ entity **kurzu** je namapován):

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping – element (MSL)

Element **AssociationSetMapping** v jazyce MSL (Mapping Specification Language) definuje mapování mezi přidružením v koncepčním modelu a sloupcích tabulky v podkladové databázi.

Asociace v koncepčním modelu jsou typy, jejichž vlastnosti představují sloupce primárního a cizího klíče v podkladové databázi. Element **AssociationSetMapping** používá dva elementy endproperty lze mapovat k definování mapování mezi vlastnostmi typu přidružení a sloupci v databázi. Můžete umístit podmínky na tato mapování s prvkem podmínky. Namapujte funkce INSERT, Update a DELETE pro přidružení k uloženým procedurám v databázi pomocí elementu ModificationFunctionMapping. Definujte mapování jen pro čtení mezi přidruženími a sloupci tabulky pomocí řetězce Entity SQL v elementu QueryView.

> [!NOTE]
> Pokud je pro přidružení v koncepčním modelu definováno referenční omezení, není nutné přidružení namapovat na element **AssociationSetMapping** . Pokud je k dispozici element **AssociationSetMapping** pro asociaci s referenčním omezením, mapování definovaná v elementu **AssociationSetMapping** budou ignorována. Další informace naleznete v tématu elementu ReferentialConstraint element (CSDL).

Element **AssociationSetMapping** může mít následující podřízené elementy.

-   QueryView (nula nebo jeden)
-   Endproperty lze mapovat (nula nebo 2)
-   Podmínka (nula nebo více)
-   ModificationFunctionMapping (nula nebo jeden)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **AssociationSetMapping** .

| Název atributu     | Je povinné | Value                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Název**           | Ano         | Název namapované sady přidružení koncepčního modelu.                      |
| **Popisuje**       | Ne          | Obor názvů kvalifikovaný název typu přidružení koncepčního modelu, který je namapován. |
| **StoreEntitySet** | Ne          | Název mapované tabulky.                                                 |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **AssociationSetMapping** , ve kterém je přidružení **FK @ no__t-2Course @ no__t-3Department** namapováno v koncepčním modelu na tabulku **kurzů** v databázi. Mapování mezi vlastnostmi typu přidružení a sloupci tabulky jsou specifikovány v podřízených elementech **endproperty lze mapovat** .

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a>ComplexProperty – element (MSL)

Element **ComplexProperty** v jazyce MSL (Mapping Specification Language) definuje mapování mezi vlastností komplexního typu u typu entity koncepčního modelu a sloupců tabulky v podkladové databázi. Mapování sloupců vlastností je zadáno v podřízených elementech ScalarProperty.

Element vlastnosti **complexType** může mít následující podřízené prvky:

-   ScalarProperty (nula nebo více)
-   **ComplexProperty** (nula nebo více)
-   ComplextTypeMapping (nula nebo více)
-   Podmínka (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na element **ComplexProperty** :

| Název atributu | Je povinné | Value                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Název**       | Ano         | Název komplexní vlastnosti typu entity v koncepčním modelu, který je namapován. |
| **Popisuje**   | Ne          | Obor názvů kvalifikovaný název typu vlastnosti koncepčního modelu.                              |

### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu. Do koncepčního modelu byl přidán následující komplexní typ:

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

Vlastnosti **LastName** a **FirstName** typu entity **osoba** byly nahrazeny jednou komplexní vlastností, **název**:

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

Následující MSL ukazuje element **ComplexProperty** použitý k mapování vlastnosti **Name** na sloupce v podkladové databázi:

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping – element (MSL)

Element **ComplexTypeMapping** v jazyce MSL (Mapping Specification Language) je podřízeným prvkem prvku ResultMapping a definuje mapování mezi funkcí importovanou v koncepčním modelu a uloženou procedurou v podkladové databázi, když následující jsou splněné:

-   Import funkce vrací koncepční komplexní typ.
-   Názvy sloupců vrácené uloženou procedurou neodpovídají přesně názvům vlastností komplexního typu.

Ve výchozím nastavení je mapování mezi sloupci vrácenou uloženou procedurou a komplexním typem založeno na názvech sloupců a vlastností. Pokud názvy sloupců neodpovídají přesně názvům vlastností, je nutné k definování mapování použít element **ComplexTypeMapping** . Příklad výchozího mapování naleznete v tématu FunctionImportMapping element (MSL).

Element **ComplexTypeMapping** může mít následující podřízené prvky:

-   ScalarProperty (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na element **ComplexTypeMapping** .

| Název atributu | Je povinné | Value                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Popisuje**   | Ano         | Obor názvů kvalifikovaný název komplexního typu, který je namapován. |

### <a name="example"></a>Příklad

Vezměte v úvahu následující uložený postup:

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

Zvažte také následující komplexní typ koncepčního modelu:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Aby bylo možné vytvořit import funkce, který vrací instance předchozího komplexního typu, mapování mezi sloupci vráceným uloženou procedurou a typem entity musí být definováno v elementu **ComplexTypeMapping** :

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a>Condition – element (MSL)

Element **Condition** v Mapping Specification Language (MSL) ukládá podmínky pro mapování mezi koncepčním modelem a podkladovou databází. Mapování, které je definováno v uzlu XML je platné, pokud jsou splněny všechny podmínky, jak je uvedeno v podřízených prvcích **podmínky** . Jinak mapování není platné. Například pokud prvek MappingFragment obsahuje jeden nebo více podřízených prvků **podmínky** , mapování definované v rámci uzlu **MappingFragment** bude platné pouze v případě, že jsou splněny všechny podmínky prvků podřízené **podmínky** .

Každá podmínka se může vztahovat buď na **název** (název vlastnosti entity koncepčního modelu, který je určený atributem **Name** ), nebo na vlastnost **ColumnName** (název sloupce v databázi, který určuje atribut **ColumnName** ). Pokud je nastaven atribut **Name** , je podmínka zkontrolována na hodnotu vlastnosti entity. Pokud je nastaven atribut **ColumnName** , je podmínka porovnána s hodnotou sloupce. V elementu **podmínky** lze zadat pouze jeden z atributů **Name** nebo **ColumnName** .

> [!NOTE]
> Pokud je element **Condition** použit v rámci elementu FunctionImportMapping, není platný pouze atribut **Name** .

Element **Condition** může být podřízeným prvkem následujících prvků:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   Element entitytypemapping

Element **Condition** nemůže mít žádné podřízené prvky.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na prvek **podmínky** :

| Název atributu | Je povinné | Value                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Ne          | Název sloupce tabulky, jehož hodnota se používá k vyhodnocení podmínky.                                                                                                                                                                                                                   |
| **IsNull**     | Ne          | **True** nebo **false**. Pokud je hodnota **true** a hodnota sloupce je **null**, nebo pokud je hodnota **false** a hodnota sloupce není **null**, podmínka je pravdivá. V opačném případě je podmínka nepravdivá. <br/> Atributy **IsNull** a **Value** nelze použít současně. |
| **Hodnota**      | Ne          | Hodnota, se kterou je porovnána hodnota sloupce. Pokud jsou hodnoty stejné, je podmínka pravdivá. V opačném případě je podmínka nepravdivá. <br/> Atributy **IsNull** a **Value** nelze použít současně.                                                                       |
| **Název**       | Ne          | Název vlastnosti entity koncepčního modelu, jejíž hodnota se používá k vyhodnocení podmínky. <br/> Tento atribut nelze použít, je-li element **Condition** použit v rámci elementu FunctionImportMapping.                                                                           |

### <a name="example"></a>Příklad

Následující příklad ukazuje prvky **podmínky** jako podřízené objekty **MappingFragment** prvků. Pokud **ZaměstnánOd** není null a **EnrollmentDate** má hodnotu null, jsou data mapována mezi sloupci **SchoolModel. Instructor** a **PersonID** a **ZaměstnánOd** tabulky **Person** . Pokud **EnrollmentDate** není null a **ZaměstnánOd** má hodnotu null, data jsou namapována mezi **SchoolModel. student** Type a sloupce **PersonID** a **zápis** v tabulce **Person** .

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a>DeleteFunction – element (MSL)

Element **DeleteFunction** v Mapping Specification Language (MSL) mapuje funkci Delete typu entity nebo přidružení v koncepčním modelu na uloženou proceduru v podkladové databázi. Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště. Další informace naleznete v tématu Function Element (SSDL).

> [!NOTE]
> Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction se aplikuje na element entitytypemapping.

Při použití na element element entitytypemapping mapuje element **DeleteFunction** funkci Delete typu entity v koncepčním modelu na uloženou proceduru.

Element **DeleteFunction** může mít při použití na element **element entitytypemapping** následující podřízené prvky:

-   AssociationEnd (nula nebo více)
-   ComplexProperty (nula nebo více)
-   ScarlarProperty (nula nebo více)

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **DeleteFunction** při použití na element **element entitytypemapping** .

| Název atributu            | Je povinné | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce DELETE. Uložená procedura musí být deklarována v modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupního parametru, který vrátí počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu a ukazuje element **DeleteFunction** , který mapuje funkci Delete typu entity **Person** na uloženou proceduru **DeletePerson** . Uložená procedura **DeletePerson** je deklarována v modelu úložiště.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a>DeleteFunction se aplikuje na AssociationSetMapping.

Při použití na element AssociationSetMapping mapuje element **DeleteFunction** funkci Delete přidružení v koncepčním modelu na uloženou proceduru.

Element **DeleteFunction** může mít při použití na element **AssociationSetMapping** následující podřízené prvky:

-   Endproperty lze mapovat

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **DeleteFunction** při použití na element **AssociationSetMapping** .

| Název atributu            | Je povinné | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce DELETE. Uložená procedura musí být deklarována v modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupního parametru, který vrátí počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu a ukazuje element **DeleteFunction** , který se používá k mapování funkce Delete **CourseInstructor** přidružení k uložené proceduře **DeleteCourseInstructor** . Uložená procedura **DeleteCourseInstructor** je deklarována v modelu úložiště.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a>Endproperty lze mapovat – element (MSL)

Element **endproperty lze mapovat** v jazyce MSL definuje mapování mezi funkcí end nebo úpravou přidružení koncepčního modelu a podkladové databáze. Mapování sloupce vlastnost je určeno v podřízeném elementu ScalarProperty.

Když je použit element **endproperty lze mapovat** k definování mapování pro konec přidružení koncepčního modelu, je podřízeným prvkem AssociationSetMapping elementu. Pokud je použit element **endproperty lze mapovat** k definování mapování pro funkci úprav v rámci přidružení koncepčního modelu, jedná se o podřízený prvek InsertFunction elementu nebo elementu DeleteFunction.

Element **endproperty lze mapovat** může mít následující podřízené prvky:

-   ScalarProperty (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na element **endproperty lze mapovat** :

| Název atributu | Je povinné | Value                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Name           | Ano         | Název elementu Association, který je namapován. |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **AssociationSetMapping** , ve kterém je přidružení **FK @ no__t-2Course @ no__t-3Department** v koncepčním modelu mapováno na tabulku **kurzů** v databázi. Mapování mezi vlastnostmi typu přidružení a sloupci tabulky jsou specifikovány v podřízených elementech **endproperty lze mapovat** .

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a>Příklad

Následující příklad ukazuje element **endproperty lze mapovat** mapující funkce INSERT a DELETE elementu Association (**CourseInstructor**) na uložené procedury v podkladové databázi. Funkce, které jsou mapovány na, jsou deklarovány v modelu úložiště.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping – element (MSL)

Element **EntityContainerMapping** ve službě Mapping Specification Language (MSL) mapuje kontejner entit v koncepčním modelu na kontejner entit v modelu úložiště. Element **EntityContainerMapping** je podřízeným prvkem mapování elementu.

Element **EntityContainerMapping** může mít následující podřízené elementy (v uvedeném pořadí):

-   EntitySetMapping (nula nebo více)
-   AssociationSetMapping (nula nebo více)
-   FunctionImportMapping (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntityContainerMapping** .

| Název atributu            | Je povinné | Value                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Ano         | Název mapovaného kontejneru entit modelu úložiště.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Ano         | Název kontejneru entity koncepčního modelu, který je namapován.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Ne          | **True** nebo **false**. Pokud je **hodnota false**, nejsou vygenerována žádná zobrazení aktualizací. Tento atribut by měl být nastaven na **hodnotu false** , pokud máte mapování jen pro čtení, které by bylo neplatné, protože data se nemusí úspěšně odcyklovat. <br/> Výchozí hodnota je **true (pravda**). |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntityContainerMapping** , který mapuje kontejner **SchoolModelEntities** (kontejner entity koncepčního modelu) do kontejneru **SchoolModelStoreContainer** (entita modelu úložiště kontejner):

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a>EntitySetMapping – element (MSL)

Element **EntitySetMapping** v jazyce MSL (Mapping Specification Language) mapuje všechny typy v koncepčním modelu entit sady na sady entit v modelu úložiště. Sada entit v koncepčním modelu je logický kontejner pro instance entit stejného typu (a odvozené typy). Sada entit v modelu úložiště představuje tabulku nebo zobrazení v podkladové databázi. Sada entit koncepčního modelu je určena hodnotou atributu **Name** elementu **EntitySetMapping** . Mapování na tabulku nebo zobrazení je určené atributem **StoreEntitySet** v každém podřízeném elementu MappingFragment nebo samotném elementu **EntitySetMapping** .

Element **EntitySetMapping** může mít následující podřízené prvky:

-   Element entitytypemapping (nula nebo více)
-   QueryView (nula nebo jeden)
-   MappingFragment (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **EntitySetMapping** .

| Název atributu           | Je povinné | Value                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**                 | Ano         | Název namapované sady entit koncepčního modelu.                                                                                                                                                             |
| **TypeName** **1**       | Ne          | Název typu entity koncepčního modelu, který je namapován.                                                                                                                                                            |
| **StoreEntitySet** **1** | Ne          | Název sady entit modelu úložiště, která je mapována na.                                                                                                                                                             |
| **Atributem makecolumnsdistinct nastaveným**  | Ne          | **Hodnota true** nebo **false** v závislosti na tom, zda jsou vráceny pouze odlišné řádky. <br/> Pokud je tento atribut nastaven na **hodnotu true**, atribut **GenerateUpdateViews** elementu EntityContainerMapping musí být nastaven na **hodnotu false**. |

 

**1** atributy **TypeName** a **StoreEntitySet** lze použít místo podřízených prvků element entitytypemapping a MappingFragment k namapování jednoho typu entity na jednu tabulku.

### <a name="example"></a>Příklad

Následující příklad ukazuje element **EntitySetMapping** , který mapuje tři typy (základní typ a dva odvozené typy) v sadě entit **výuky** konceptuálního modelu do tří různých tabulek v podkladové databázi. Tabulky jsou určeny atributem **StoreEntitySet** v každém elementu **MappingFragment** .

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a>Element entitytypemapping – element (MSL)

Element **element entitytypemapping** v jazyce MSL (Mapping Specification Language) definuje mapování mezi typem entity v koncepčním modelu a tabulkách nebo zobrazeními v podkladové databázi. Informace o typech entit koncepčního modelu a podkladových databázových tabulkách a zobrazeních naleznete v tématu EntityType element (CSDL) a EntitySet element (SSDL). Typ entity koncepčního modelu, který je mapován, je určen atributem **TypeName** elementu **element entitytypemapping** . Namapovaná tabulka nebo zobrazení je určeno atributem **StoreEntitySet** podřízeného elementu MappingFragment.

K namapování funkcí pro vložení, aktualizaci nebo odstranění typů entit na uložené procedury v databázi lze použít podřízený element ModificationFunctionMapping.

Element **element entitytypemapping** může mít následující podřízené prvky:

-   MappingFragment (nula nebo více)
-   ModificationFunctionMapping (nula nebo jeden)
-   ScalarProperty
-   Podmínka

> [!NOTE]
> Prvky **MappingFragment** a **ModificationFunctionMapping** nemohou být podřízenými prvky elementu **element entitytypemapping** současně.


> [!NOTE]
> Prvky **ScalarProperty** a **Condition** mohou být pouze podřízené prvky elementu **element entitytypemapping** , pokud jsou použity v rámci elementu FunctionImportMapping.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **element entitytypemapping** .

| Název atributu | Je povinné | Value                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Popisuje**   | Ano         | Obor názvů kvalifikovaný název typu entity koncepčního modelu, který je namapován. <br/> Pokud je typ abstraktní nebo odvozený typ, musí být hodnota `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Příklad

Následující příklad ukazuje element EntitySetMapping se dvěma podřízenými elementy **element entitytypemapping** . V prvním elementu **element entitytypemapping** je jako typ entity **SchoolModel. person** namapována tabulka **Person (osoba** ). Ve druhém elementu **element entitytypemapping** je funkce aktualizace typu **SchoolModel. person** namapována na uloženou proceduru **UpdatePerson**v databázi.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Příklad

Následující příklad ukazuje mapování hierarchie typů, ve kterém je kořenový typ abstraktní. Všimněte si použití syntaxe `IsOfType` pro atributy **TypeName** .

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping – element (MSL)

Element **FunctionImportMapping** v jazyce MSL definuje mapování mezi funkcí importovanou v koncepčním modelu a uloženou procedurou nebo funkcí v podkladové databázi. Importy funkcí musí být deklarovány v koncepčním modelu a uložené procedury musí být deklarovány v modelu úložiště. Další informace naleznete v tématu element FunctionImport (CSDL) a element Function (SSDL).

> [!NOTE]
> Ve výchozím nastavení, pokud import funkce vrátí typ entity koncepčního modelu nebo komplexní typ, pak názvy sloupců vrácené základní uloženou procedurou musí přesně odpovídat názvům vlastností typu koncepčního modelu. Pokud názvy sloupců neodpovídají přesně názvům vlastností, mapování musí být definováno v elementu ResultMapping.

Element **FunctionImportMapping** může mít následující podřízené prvky:

-   ResultMapping (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na element **FunctionImportMapping** :

| Název atributu         | Je povinné | Value                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **Názevfunkceimportu** | Ano         | Název importované funkce v koncepčním modelu, který je namapován.           |
| **FunctionName**       | Ano         | Kvalifikovaný název oboru názvů funkce v modelu úložiště, který je namapován. |

### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu. V modelu úložiště zvažte následující funkci:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Zvažte také, že je tato funkce importovaná v koncepčním modelu:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Následující příklad ukazuje element **FunctionImportMapping** použitý k namapování funkce a importu funkcí výše na sebe:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction – element (MSL)

Element **InsertFunction** v Mapping Specification Language (MSL) mapuje funkci INSERT typu entity nebo přidružení v koncepčním modelu na uloženou proceduru v podkladové databázi. Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště. Další informace naleznete v tématu Function Element (SSDL).

> [!NOTE]
> Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.

Element **InsertFunction** může být podřízeným prvkem prvku ModificationFunctionMapping a použit pro element element entitytypemapping nebo prvek AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction se aplikuje na element entitytypemapping.

Při použití na element element entitytypemapping mapuje element **InsertFunction** funkci INSERT typu entity v koncepčním modelu na uloženou proceduru.

Element **InsertFunction** může mít při použití na element **element entitytypemapping** následující podřízené prvky:

-   AssociationEnd (nula nebo více)
-   ComplexProperty (nula nebo více)
-   ResultBinding (nula nebo jeden)
-   ScarlarProperty (nula nebo více)

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **InsertFunction** při použití na element **element entitytypemapping** .

| Název atributu            | Je povinné | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce INSERT. Uložená procedura musí být deklarována v modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupního parametru, který vrátí počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu a ukazuje element **InsertFunction** , který se používá k mapování funkce INSERT typu entity Person do uložené procedury **InsertPerson** . Uložená procedura **InsertPerson** je deklarována v modelu úložiště.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a>InsertFunction se aplikuje na AssociationSetMapping.

Při použití na element AssociationSetMapping mapuje element **InsertFunction** funkci INSERT přidružení v koncepčním modelu na uloženou proceduru.

Element **InsertFunction** může mít při použití na element **AssociationSetMapping** následující podřízené prvky:

-   Endproperty lze mapovat

#### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **InsertFunction** při použití na element **AssociationSetMapping** .

| Název atributu            | Je povinné | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Obor názvů kvalifikovaný název uložené procedury, ke které je namapována funkce INSERT. Uložená procedura musí být deklarována v modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupního parametru, který vrátí počet ovlivněných řádků.                                                                               |

#### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu a ukazuje element **InsertFunction** , který se používá k mapování funkce vložení **CourseInstructor** přidružení k uložené proceduře **InsertCourseInstructor** . Uložená procedura **InsertCourseInstructor** je deklarována v modelu úložiště.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a>Mapping – element (MSL)

Element **Mapping** v Mapping Specification Language (MSL) obsahuje informace pro mapování objektů, které jsou definovány v koncepčním modelu, do databáze (jak je popsáno v modelu úložiště). Další informace najdete v tématu Specifikace CSDL a SSDL.

Element **Mapping** je kořenovým prvkem specifikace mapování. Obor názvů XML pro specifikace mapování je https://schemas.microsoft.com/ado/2009/11/mapping/cs.

Element Mapping může mít následující podřízené elementy (v uvedeném pořadí):

-   Alias (nula nebo více)
-   EntityContainerMapping (právě jeden)

Názvy koncepčních a typů modelů úložiště, které jsou odkazovány v MSL, musí být kvalifikovány svými příslušnými názvy oborů názvů. Informace o názvu oboru názvů konceptuálního modelu naleznete v tématu Schema – element (CSDL). Informace o názvu oboru názvů modelu úložiště najdete v tématu schéma – element (SSDL). Aliasy pro obory názvů, které jsou používány v MSL, lze definovat pomocí elementu alias.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které lze použít na element **Mapping** .

| Název atributu | Je povinné | Value                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Space**      | Ano         | **C-S**. Toto je pevná hodnota a nelze ji změnit. |

### <a name="example"></a>Příklad

Následující příklad ukazuje prvek **mapování** , který je založen na rámci školního modelu. Další informace o školním modelu najdete v tématu rychlý Start (Entity Framework):

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a>MappingFragment – element (MSL)

Element **MappingFragment** v jazyce MSL (Mapping Specification Language) definuje mapování mezi vlastnostmi typu entity koncepčního modelu a tabulkou nebo zobrazením v databázi. Informace o typech entit koncepčního modelu a podkladových databázových tabulkách a zobrazeních naleznete v tématu EntityType element (CSDL) a EntitySet element (SSDL). **MappingFragment** může být podřízeným prvkem elementu element entitytypemapping nebo elementu EntitySetMapping.

Element **MappingFragment** může mít následující podřízené prvky:

-   ComplexType (nula nebo více)
-   ScalarProperty (nula nebo více)
-   Podmínka (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **MappingFragment** .

| Název atributu          | Je povinné | Value                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Ano         | Název tabulky nebo zobrazení, které je mapováno.                                                                                                                                                                           |
| **Atributem makecolumnsdistinct nastaveným** | Ne          | **Hodnota true** nebo **false** v závislosti na tom, zda jsou vráceny pouze odlišné řádky. <br/> Pokud je tento atribut nastaven na **hodnotu true**, atribut **GenerateUpdateViews** elementu EntityContainerMapping musí být nastaven na **hodnotu false**. |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **MappingFragment** jako podřízený prvek **element entitytypemapping** elementu. V tomto příkladu jsou vlastnosti typu **kurzu** v koncepčním modelu mapovány na sloupce tabulky **kurzů** v databázi.

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Příklad

Následující příklad ukazuje element **MappingFragment** jako podřízený prvek **EntitySetMapping** elementu. Stejně jako v předchozím příkladu jsou vlastnosti typu **kurzu** v koncepčním modelu mapovány na sloupce tabulky **Course** v databázi.

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping – element (MSL)

Element **ModificationFunctionMapping** ve službě Mapping Specification Language (MSL) mapuje funkce vložení, aktualizace a odstranění typu entity koncepčního modelu na uložené procedury v podkladové databázi. Element **ModificationFunctionMapping** může také mapovat funkce INSERT a DELETE pro asociace m:n v koncepčním modelu na uložené procedury v podkladové databázi. Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště. Další informace naleznete v tématu Function Element (SSDL).

> [!NOTE]
> Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.


> [!NOTE]
> Pokud jsou funkce úprav pro jednu entitu v hierarchii dědičnosti mapovány na uložené procedury, musí být funkce úprav pro všechny typy v hierarchii namapovány na uložené procedury.

Element **ModificationFunctionMapping** může být podřízeným prvkem elementu element entitytypemapping nebo elementu AssociationSetMapping.

Element **ModificationFunctionMapping** může mít následující podřízené prvky:

-   DeleteFunction (nula nebo jeden)
-   InsertFunction (nula nebo jeden)
-   UpdateFunction (nula nebo jeden)

Pro element **ModificationFunctionMapping** se nevztahují žádné atributy.

### <a name="example"></a>Příklad

Následující příklad ukazuje mapování sady entit pro entitu **lidé** nastavenou ve škole modelu. Vedle mapování sloupce pro typ entity **osoba** se zobrazí mapování funkcí pro vložení, aktualizaci a odstranění typu **osoba** . Funkce, které jsou mapovány na, jsou deklarovány v modelu úložiště.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Příklad

Následující příklad ukazuje mapování sady přidružení pro přidružení **CourseInstructor** nastavené ve škole modelu. Kromě mapování sloupce pro přidružení **CourseInstructor** se zobrazí mapování funkcí INSERT a DELETE v přidružení **CourseInstructor** . Funkce, které jsou mapovány na, jsou deklarovány v modelu úložiště.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a>QueryView – element (MSL)

Element **QueryView** v jazyce MSL (Mapping Specification Language) definuje mapování jen pro čtení mezi typem entity nebo přidružením v koncepčním modelu a tabulkou v podkladové databázi. Mapování je definováno s Entity SQL dotazem, který je vyhodnocován na základě modelu úložiště a vyjadřuje sadu výsledků dotazu v souvislosti s entitou nebo přidružením v koncepčním modelu. Vzhledem k tomu, že zobrazení dotazů jsou jen pro čtení, nemůžete použít standardní příkazy aktualizace k aktualizaci typů, které jsou definovány zobrazeními dotazů. Můžete provádět aktualizace těchto typů pomocí funkcí úprav. Další informace naleznete v tématu How to: Namapujte funkce úprav na uložené procedury.

> [!NOTE]
> V elementu **QueryView** se nepodporuje Entity SQL výrazy, které obsahují **GroupBy**, agregace skupin nebo navigační vlastnosti.

 

Element **QueryView** může být podřízeným prvkem elementu EntitySetMapping nebo elementu AssociationSetMapping. V bývalém případě zobrazení dotazu definuje mapování pro entitu v koncepčním modelu jen pro čtení. V druhém případě zobrazení dotazu definuje mapování jen pro čtení pro přidružení v koncepčním modelu.

> [!NOTE]
> Pokud je element **AssociationSetMapping** pro přidružení s referenčním omezením, element **AssociationSetMapping** se ignoruje. Další informace naleznete v tématu elementu ReferentialConstraint element (CSDL).

Element **QueryView** nemůže mít žádné podřízené elementy.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **QueryView** .

| Název atributu | Je povinné | Value                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Popisuje**   | Ne          | Název typu koncepčního modelu, který je namapován zobrazením dotazu. |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **QueryView** jako podřízený prvek **EntitySetMapping** a definuje mapování zobrazení dotazu pro typ entity **oddělení** v modelu školy.

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

Vzhledem k tomu, že dotaz vrací pouze podmnožinu členů typu **oddělení** v modelu úložiště, typ **oddělení** ve školním modelu byl změněn na základě tohoto mapování následujícím způsobem:

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a>Příklad

Následující příklad ukazuje element **QueryView** jako podřízenou položku prvku **AssociationSetMapping** a definuje mapování jen pro čtení pro asociaci `FK_Course_Department` v modelu školy.

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a>Komentáře

Můžete definovat zobrazení dotazů a povolit následující scénáře:

-   Definujte entitu v koncepčním modelu, která neobsahuje všechny vlastnosti entity v modelu úložiště. To zahrnuje vlastnosti, které nemají výchozí hodnoty a nepodporují hodnoty **null** .
-   Namapujte počítané sloupce v modelu úložiště na vlastnosti typů entit v koncepčním modelu.
-   Definujte mapování, kde podmínky používané pro rozdělení entit v koncepčním modelu nejsou založené na rovnosti. Když zadáte podmíněné mapování pomocí elementu **Condition** , dodaná podmínka se musí rovnat zadané hodnotě. Další informace naleznete v tématu Condition element (MSL).
-   Namapujte stejný sloupec v modelu úložiště na více typů v koncepčním modelu.
-   Namapujte více typů na stejnou tabulku.
-   Definujte asociace v koncepčním modelu, které nejsou založené na cizích klíčích v relačním schématu.
-   Použijte vlastní obchodní logiku k nastavení hodnoty vlastností v koncepčním modelu. Například můžete namapovat hodnotu řetězce "T" ve zdroji dat na hodnotu **true**, logická hodnota v koncepčním modelu.
-   Definujte podmíněné filtry pro výsledky dotazu.
-   Vyvynuťte méně omezení pro data v koncepčním modelu než v modelu úložiště. Můžete například vytvořit vlastnost v koncepčním modelu s možnou hodnotou null i v případě, že sloupec, ke kterému je namapován, nepodporuje hodnoty **null**.

Při definování zobrazení dotazu pro entity platí následující požadavky:

-   Zobrazení dotazu jsou jen pro čtení. Aktualizace entit můžete provádět pouze pomocí funkcí úprav.
-   Při definování typu entity pomocí zobrazení dotazu je nutné definovat také všechny související entity pomocí zobrazení dotazu.
-   Pokud namapujete přidružení m:n k entitě v modelu úložiště, která představuje tabulku odkazů v relačním schématu, je nutné definovat element **QueryView** v elementu **AssociationSetMapping** pro tuto tabulku odkazů.
-   Zobrazení dotazu musí být definována pro všechny typy v hierarchii typu. Můžete to udělat následujícími způsoby:
-   -   Pomocí jednoho prvku **QueryView** , který určuje jeden Entity SQL dotaz, který vrací sjednocení všech typů entit v hierarchii.
    -   Pomocí jednoho prvku **QueryView** , který určuje jeden Entity SQL dotaz, který používá operátor Case k vrácení konkrétního typu entity v hierarchii založeného na konkrétní podmínce.
    -   S dalším prvkem **QueryView** pro konkrétní typ v hierarchii. V takovém případě použijte atribut **TypeName** elementu **QueryView** k určení typu entity pro každé zobrazení.
-   Pokud je definováno zobrazení dotazu, nelze zadat atribut **StorageSetName** elementu **EntitySetMapping** .
-   Pokud je definováno zobrazení dotazu, element **EntitySetMapping**nemůže také obsahovat mapování **vlastností** .

## <a name="resultbinding-element-msl"></a>ResultBinding – element (MSL)

Element **ResultBinding** v Mapping Specification Language (MSL) mapuje hodnoty sloupce, které jsou vraceny uloženými procedurami do vlastností entity v koncepčním modelu, pokud jsou funkce změny typu entity mapovány na uložené procedury v podkladová databáze. Například pokud je hodnota sloupce identity vrácena uloženou procedurou INSERT, element **ResultBinding** mapuje vrácenou hodnotu na vlastnost typu entity v koncepčním modelu.

Element **ResultBinding** může být podřízeným prvkem elementu InsertFunction nebo elementu UpdateFunction.

Element **ResultBinding** nemůže mít žádné podřízené elementy.

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které se vztahují na element **ResultBinding** :

| Název atributu | Je povinné | Value                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Název**       | Ano         | Název vlastnosti entity v koncepčním modelu, který je namapován. |
| **ColumnName** | Ano         | Název mapovaného sloupce                                          |

### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu a ukazuje element **InsertFunction** , který slouží k mapování funkce INSERT typu entity **Person** na uloženou proceduru **InsertPerson** . (Uloženou proceduru **InsertPerson** se zobrazuje níže a deklaruje se v modelu úložiště.) Element **ResultBinding** se používá k mapování hodnoty sloupce, který je vrácený uloženou procedurou (**NewPersonID**) na vlastnost typu entity (**PersonID**).

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

Následující příkaz Transact-SQL popisuje uloženou proceduru **InsertPerson** :

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a>ResultMapping – element (MSL)

Element **ResultMapping** v mapování specifikace jazyka MSL definuje mapování mezi funkcí importovanou v koncepčním modelu a uloženou procedurou v podkladové databázi, pokud jsou splněny následující podmínky:

-   Import funkce vrací typ entity koncepčního modelu nebo komplexní typ.
-   Názvy sloupců vrácené uloženou procedurou neodpovídají přesně názvům vlastností typu entity nebo komplexního typu.

Ve výchozím nastavení je mapování mezi sloupci vrácenou uloženou procedurou a typem entity nebo komplexním typem založeno na názvech sloupců a vlastností. Pokud názvy sloupců neodpovídají přesně názvům vlastností, je nutné k definování mapování použít element **ResultMapping** . Příklad výchozího mapování naleznete v tématu FunctionImportMapping element (MSL).

Element **ResultMapping** je podřízeným prvkem elementu FunctionImportMapping.

Element **ResultMapping** může mít následující podřízené prvky:

-   Element entitytypemapping (nula nebo více)
-   ComplexTypeMapping

Pro element **ResultMapping** se nevztahují žádné atributy.

### <a name="example"></a>Příklad

Vezměte v úvahu následující uložený postup:

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

Zvažte také následující typ entity koncepčního modelu:

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

Aby bylo možné vytvořit import funkce, který vrací instance předchozí entity typu, mapování mezi sloupci vrácenou uloženou procedurou a typem entity musí být definováno v elementu **ResultMapping** :

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a>ScalarProperty – element (MSL)

Element **ScalarProperty** v Mapping Specification Language (MSL) mapuje vlastnost u typu entity koncepčního modelu, komplexního typu nebo přidružení k sloupci tabulky nebo parametru uložené procedury v podkladové databázi.

> [!NOTE]
> Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště. Další informace naleznete v tématu Function Element (SSDL).

Element **ScalarProperty** může být podřízeným prvkem následujících prvků:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   Endproperty lze mapovat
-   ComplexProperty
-   ResultMapping

Jako podřízený element **MappingFragment**, **ComplexProperty**nebo **endproperty lze mapovat** prvek **ScalarProperty** mapuje vlastnost v koncepčním modelu na sloupec v databázi. Jako podřízený element **InsertFunction**, **UpdateFunction**nebo **DeleteFunction** prvek **ScalarProperty** mapuje vlastnost v koncepčním modelu na parametr uložené procedury.

Element **ScalarProperty** nemůže mít žádné podřízené elementy.

### <a name="applicable-attributes"></a>Použitelné atributy

Atributy, které se vztahují na element **ScalarProperty** , se liší v závislosti na roli elementu.

Následující tabulka popisuje atributy, které jsou použity při použití prvku **ScalarProperty** k namapování vlastnosti koncepčního modelu na sloupec v databázi:

| Název atributu | Je povinné | Value                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Název**       | Ano         | Název vlastnosti konceptuálního modelu, který je mapován. |
| **ColumnName** | Ano         | Název sloupce tabulky, který je namapován.              |

Následující tabulka popisuje atributy, které se vztahují na element **ScalarProperty** při použití k namapování vlastnosti koncepčního modelu na parametr uložené procedury:

| Název atributu    | Je povinné | Value                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Název**          | Ano         | Název vlastnosti konceptuálního modelu, který je mapován.                                                                                 |
| **ParameterName** | Ano         | Název parametru, který je namapován.                                                                                                 |
| **Verze**       | Ne          | **Aktuální** nebo **původní** v závislosti na tom, zda má být aktuální hodnota nebo původní hodnota vlastnosti použita pro kontrolu souběžnosti. |

### <a name="example"></a>Příklad

Následující příklad ukazuje element **ScalarProperty** použitý dvěma způsoby:

-   Pro mapování vlastností typu entity **Person** na sloupce tabulky **Person**.
-   Pro mapování vlastností typu entity **Person** na parametry uložené procedury **UpdatePerson** . Uložené procedury jsou deklarovány v modelu úložiště.

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a>Příklad

Následující příklad ukazuje element **ScalarProperty** , který slouží k mapování funkcí INSERT a DELETE přidružení koncepčního modelu k uloženým procedurám v databázi. Uložené procedury jsou deklarovány v modelu úložiště.

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a>UpdateFunction – element (MSL)

Element **UpdateFunction** ve službě Mapping Specification Language (MSL) mapuje funkci Update typu entity v koncepčním modelu na uloženou proceduru v podkladové databázi. Uložené procedury, do kterých jsou mapovány funkce úprav, musí být deklarovány v modelu úložiště. Další informace naleznete v tématu Function Element (SSDL).

> [!NOTE]
>  Pokud nemapujete všechny tři operace vložení, aktualizace nebo odstranění typu entity na uložené procedury, nemapované operace selžou při spuštění za běhu a vyvolá se UpdateException.

Element **UpdateFunction** může být podřízeným prvkem prvku ModificationFunctionMapping a použit pro element element entitytypemapping.

Element **UpdateFunction** může mít následující podřízené prvky:

-   AssociationEnd (nula nebo více)
-   ComplexProperty (nula nebo více)
-   ResultBinding (nula nebo jeden)
-   ScarlarProperty (nula nebo více)

### <a name="applicable-attributes"></a>Použitelné atributy

Následující tabulka popisuje atributy, které mohou být aplikovány na element **UpdateFunction** .

| Název atributu            | Je povinné | Value                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Ano         | Obor názvů kvalifikovaný název uložené procedury, na kterou je namapovaná funkce Update. Uložená procedura musí být deklarována v modelu úložiště. |
| **RowsAffectedParameter** | Ne          | Název výstupního parametru, který vrátí počet ovlivněných řádků.                                                                               |

### <a name="example"></a>Příklad

Následující příklad je založen na školním modelu a ukazuje element **UpdateFunction** , který se používá k namapování funkce Update typu entity **Person** na uloženou proceduru **UpdatePerson** . Uložená procedura **UpdatePerson** je deklarována v modelu úložiště.

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
