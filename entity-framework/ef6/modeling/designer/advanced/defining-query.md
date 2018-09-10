---
title: Definování dotazu – EF designeru - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 8415a265cdbe078422e0467ee97da955a81b873d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250969"
---
# <a name="defining-query---ef-designer"></a>Definování dotazu – EF designeru
Tento návod ukazuje, jak přidat, definování dotazů a odpovídající entita typ modelu pomocí EF designeru. Definování dotazu se běžně používá k zajištění funkce podobné, která poskytuje zobrazení databáze, ale zobrazení je definováno v modelu, ne databáze. Definování dotazu umožňuje provedení příkazu SQL, který je zadán v **DefiningQuery** prvek souboru .edmx. Další informace najdete v tématu **DefiningQuery** v [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Při použití definice dotazů, máte také definovat typ entity v modelu. Typ entity se používá k poskytování data vystavená prostřednictvím definice dotazu. Všimněte si, že data prezentované prostřednictvím tohoto typu entity je jen pro čtení.

Parametrizované dotazy nejde provést, protože definice dotazů. Data však můžete aktualizovat pomocí mapování insert, update a delete funkce typ entity tohoto rovin dat pro uložené procedury. Další informace najdete v tématu [Insert, Update a Delete pomocí uložené procedury](~/ef6/modeling/designer/stored-procedures/cud.md).

Toto téma ukazuje, jak provádět následující úlohy.

-   Přidat dotaz definující
-   Přidat typ Entity do modelu
-   Definování dotazu mapovat na typ Entity

## <a name="prerequisites"></a>Požadavky

K dokončení toho návodu budete potřebovat:

- Nejnovější verzi sady Visual Studio.
- [Ukázkové databáze školy](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Nastavení projektu

Tento názorný postup je pomocí sady Visual Studio 2012 nebo novější.

-   Otevřít Visual Studio.
-   Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**.
-   V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzolovou aplikaci** šablony.
-   Zadejte **DefiningQuerySample** jako název projektu a klikněte na tlačítko **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Vytvořit Model založený na databáze školy

-   Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.
-   Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.
-   Zadejte **DefiningQueryModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.
-   V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.
-   Klikněte na tlačítko nové připojení. V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.
    Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.
-   V dialogovém okně Zvolte vaše databázové objekty, zkontrolujte, **tabulky** uzlu. Tato možnost přidá všech tabulek, které **School** modelu.
-   Klikněte na tlačítko **Dokončit**.
-   V Průzkumníku řešení klikněte pravým tlačítkem myši **DefiningQueryModel.edmx** a vyberte možnost **otevřít v programu...** .
-   Vyberte **Editor (textový) XML**.

    ![Editor XML](~/ef6/media/xmleditor.png)

-   Klikněte na tlačítko **Ano** Pokud se zobrazí výzva s následující zprávou:

    ![Upozornění 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Přidat dotaz definující

V tomto kroku, který budeme používat editoru XML, který chcete přidat, definování dotazů a zadejte entitu do části SSDL soubor .edmx. 

-   Přidat **objektu EntitySet** prvek do části SSDL soubor .edmx (řádku 5 až 13). Zadejte následující informace:
    -   Pouze **název** a **EntityType** atributy **objektu EntitySet** zadávají se element.
    -   Plně kvalifikovaný název typu entity se používá v **EntityType** atribut.
    -   Je zadaný příkaz jazyka SQL, který se spustí v **DefiningQuery** elementu.

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

-   Přidat **EntityType** prvek do části SSDL edmx. soubor jak je znázorněno níže. Vezměte na vědomí následující:
    -   Hodnota **název** atribut odpovídá hodnotě **EntityType** atribut **objektu EntitySet** element výše, i když plně kvalifikovaný název Typ entity se používá v **EntityType** atribut.
    -   Názvy vlastností, které odpovídají názvům sloupce vrácený příkazem SQL v **DefiningQuery** – element (viz výše).
    -   V tomto příkladu klíč entity tvoří tři vlastnosti zajistit jedinečnou hodnotu klíče.

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
> Pokud později spustíte **aktualizace modelu průvodce** dialogového okna, všechny změny provedené v modelu úložiště, včetně definice dotazů, bude přepsán.

 

## <a name="add-an-entity-type-to-the-model"></a>Přidat typ Entity do modelu

V tomto kroku přidáme typ entity pro koncepční model pomocí EF designeru.  Vezměte na vědomí následující:

-   **Název** entity odpovídá hodnotě **EntityType** atribut **objektu EntitySet** element výše.
-   Názvy vlastností, které odpovídají názvům sloupce vrácený příkazem SQL v **DefiningQuery** element výše.
-   V tomto příkladu klíč entity tvoří tři vlastnosti zajistit jedinečnou hodnotu klíče.

Otevřete model v EF designeru.

-   Dvakrát klikněte DefiningQueryModel.edmx.
-   Řekněme, že **Ano** následující zprávu:

    ![Upozornění 2](~/ef6/media/warning2.png)

 

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.

-   Klikněte pravým tlačítkem na Návrhář ploše a vyberte možnost **přidat nový**-&gt;**Entity...** .
-   Zadejte **GradeReport** u názvu entity a **CourseID** pro **vlastnost Key**.
-   Klikněte pravým tlačítkem myši **GradeReport** entity a vyberte **přidat nový** - &gt; **skalární vlastnost**.
-   Změnit výchozí název vlastnosti, která má **FirstName**.
-   Přidat jiné Skalární vlastnosti a určit **LastName** pro název.
-   Přidat jiné Skalární vlastnosti a určit **na podnikové úrovni** pro název.
-   V **vlastnosti** okno Změnit **na podnikové úrovni**společnosti **typ** vlastnost **desítkové**.
-   Vyberte **FirstName** a **LastName** vlastnosti.
-   V **vlastnosti** okno Změnit **EntityKey** hodnoty vlastnosti **True**.

V důsledku toho byly přidány následující prvky do **CSDL** část souboru .edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Definování dotazu mapovat na typ Entity

V tomto kroku použijeme okno Podrobnosti o mapování pro mapování koncepční a typy entit úložiště.

-   Klikněte pravým tlačítkem myši **GradeReport** entity na návrhové ploše a vyberte **mapování tabulky,**.  
    **Podrobnosti mapování** se zobrazí okno.
-   Vyberte **GradeReport** z **&lt;přidat tabulku nebo zobrazení&gt;** rozevíracího seznamu (umístěný ve skupinovém rámečku **tabulky**s).  
    Výchozí mapování mezi koncepční a úložiště **GradeReport** zobrazí typ entity.  
    ![Mapování Details3](~/ef6/media/mappingdetails.png)

V důsledku toho **elementu EntitySetMapping** prvek se přidá do části mapování souboru .edmx. 

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

 

## <a name="call-the-defining-query-in-your-code"></a>Definování dotazu volání v kódu

Definování dotazu teď můžete spustit pomocí **GradeReport** typu entity. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
