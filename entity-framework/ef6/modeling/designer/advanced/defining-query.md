---
title: Definování dotazu-EF Designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418796"
---
# <a name="defining-query---ef-designer"></a>Definice dotazu – Návrhář EF
Tento návod ukazuje, jak přidat definiční dotaz a odpovídající typ entity do modelu pomocí návrháře EF. Definiční dotaz se běžně používá k poskytování funkcí podobných těm, které poskytuje zobrazení databáze, ale zobrazení je definováno v modelu, nikoli v databázi. Definiční dotaz umožňuje spustit příkaz SQL, který je určen v **DefiningQuery** elementu souboru. edmx. Další informace najdete v tématu **DefiningQuery** ve [specifikaci SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Při použití definování dotazů musíte také definovat typ entity v modelu. Typ entity se používá k vytvoření Surface dat vystavených definičním dotazem. Všimněte si, že data provedená prostřednictvím tohoto typu entity jsou jen pro čtení.

Parametrizované dotazy nelze provést jako definování dotazů. Data se ale dají aktualizovat mapováním funkcí INSERT, Update a DELETE typu entity, která doplňují data do uložených procedur. Další informace najdete v tématu [vkládání, aktualizace a odstraňování s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/cud.md).

V tomto tématu se dozvíte, jak provádět následující úlohy.

-   Přidat definiční dotaz
-   Přidání typu entity do modelu
-   Namapujte definiční dotaz na typ entity.

## <a name="prerequisites"></a>Předpoklady

K dokončení toho návodu budete potřebovat:

- Poslední verze sady Visual Studio.
- [Ukázková databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

Tento návod používá aplikaci Visual Studio 2012 nebo novější.

-   Otevřete sadu Visual Studio.
-   V nabídce **soubor** přejděte na příkaz **Nový**a klikněte na **projekt**.
-   V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **Konzolová aplikace** .
-   Jako název projektu zadejte **DefiningQuerySample** a klikněte na **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Vytvoření modelu založeného na školní databázi

-   Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka**.
-   V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
-   Jako název souboru zadejte **DefiningQueryModel. edmx** a pak klikněte na **Přidat**.
-   V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.
-   Klikněte na nové připojení. V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.
    Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.
-   V dialogovém okně zvolit objekty databáze zaškrtněte uzel **tabulky** . Tím se do **školního** modelu přidá všechny tabulky.
-   Klikněte na tlačítko **Dokončit**.
-   V Průzkumník řešení klikněte pravým tlačítkem na soubor **DefiningQueryModel. edmx** a vyberte **otevřít s...** .
-   Vyberte **Editor XML (text)** .

    ![Editor XML](~/ef6/media/xmleditor.png)

-   Po zobrazení výzvy s následující zprávou klikněte na **Ano** :

    ![Upozornění 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Přidat definiční dotaz

V tomto kroku použijeme editor XML k přidání definičního dotazu a typu entity do oddílu SSDL souboru. edmx. 

-   Přidejte element **EntitySet** do oddílu SSDL souboru. edmx (řádek 5 až 13). Určete následující nastavení:
    -   Jsou zadány pouze atributy **Name** a **EntityType** elementu **EntitySet** .
    -   Plně kvalifikovaný název typu entity se používá v atributu **EntityType** .
    -   Příkaz jazyka SQL, který má být spuštěn, je určen v prvku  **DefiningQuery** .

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Přidejte element **EntityType** do oddílu SSDL v. edmx. souboru, jak je znázorněno níže. Je třeba počítat s následujícím:
    -   Hodnota atributu **Name** odpovídá hodnotě atributu **EntityType** v elementu **EntitySet** výše, i když je plně kvalifikovaný název typu entity použit v atributu **EntityType** .
    -   Názvy vlastností odpovídají názvům sloupců vráceným příkazem SQL v elementu **DefiningQuery** (výše).
    -   V tomto příkladu se klíč entity skládá ze tří vlastností, aby se zajistila hodnota jedinečného klíče.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Pokud budete později spouštět dialog **Průvodce aktualizací modelu** , budou přepsány všechny změny modelu úložiště, včetně definování dotazů.

 

## <a name="add-an-entity-type-to-the-model"></a>Přidání typu entity do modelu

V tomto kroku přidáme typ entity do koncepčního modelu pomocí návrháře EF.  Vezměte na vědomí následující:

-   **Název** entity odpovídá hodnotě atributu **EntityType** v elementu **EntitySet** výše.
-   Názvy vlastností odpovídají názvům sloupců vráceným příkazem SQL ve výše uvedeném elementu **DefiningQuery** .
-   V tomto příkladu se klíč entity skládá ze tří vlastností, aby se zajistila hodnota jedinečného klíče.

Otevřete model v Návrháři EF.

-   Dvakrát klikněte na DefiningQueryModel. edmx.
-   Řekněte **Ano** pro následující zprávu:

    ![Upozornění 2](~/ef6/media/warning2.png)

 

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.

-   Klikněte pravým tlačítkem myši na plochu návrháře a vyberte **Přidat novou**-&gt;**entitu...** .
-   Zadejte **GradeReport** pro název entity a **CourseID** pro **klíčovou vlastnost**.
-   Klikněte pravým tlačítkem na entitu **GradeReport** a vyberte **přidat novou**-&gt; **skalární vlastnost**.
-   Změňte výchozí název vlastnosti na **FirstName**.
-   Přidejte další skalární vlastnost a jako název zadejte **LastName** .
-   Přidejte další skalární vlastnost a jako název zadejte **třídu** .
-   V okně **vlastnosti** změňte vlastnost **typ** **třídy**na hodnotu **Decimal**.
-   Vyberte vlastnosti **FirstName** a **LastName** .
-   V okně **vlastnosti** změňte hodnotu vlastnosti **EntityKey** na hodnotu **true**.

V důsledku toho byly do oddílu **CSDL** souboru. edmx přidány následující prvky.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Namapujte definiční dotaz na typ entity.

V tomto kroku použijeme okno Podrobnosti mapování k mapování typů koncepčních a entit úložišť.

-   Na návrhové ploše klikněte pravým tlačítkem na entitu **GradeReport** a vyberte **mapování tabulky**.  
    Zobrazí se okno **Podrobnosti mapování** .
-   Vyberte **GradeReport** z **&lt;přidat tabulku nebo zobrazení&gt;** rozevírací seznam (umístěný v **tabulce**s).  
    Zobrazí se výchozí mapování mezi typem entity koncepčního a **GradeReportho** úložiště.  
    Mapování ![Details3](~/ef6/media/mappingdetails.png)

V důsledku toho se prvek **EntitySetMapping** přidá do oddílu mapování v souboru. edmx. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Zkompilujte aplikaci.

 

## <a name="call-the-defining-query-in-your-code"></a>Volání definičního dotazu ve vašem kódu

Nyní můžete spustit definiční dotaz pomocí typu entity **GradeReport** . 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
