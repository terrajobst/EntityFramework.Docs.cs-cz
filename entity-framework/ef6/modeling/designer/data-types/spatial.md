---
title: Prostorový – EF designeru - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 39fe54ffe9d0ffd1753844f7bffcd1832d82eec5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994364"
---
# <a name="spatial---ef-designer"></a>Prostorový – EF designeru
> [!NOTE]
> **EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Videa a podrobný návod ukazuje, jak mapovat prostorové typy s Entity Framework Designer. Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.

Tento názorný postup použije první Model k vytvoření nové databáze, ale EF designeru je také možné se [Database First](~/ef6/modeling/designer/workflows/database-first.md) pracovního postupu pro mapování na existující databázi.

Podpora prostorových typů byla zavedena v Entity Framework 5. Všimněte si, že pokud chcete používat nové funkce jako prostorových typů výčtů a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5. Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.

Používat typy prostorových dat je nutné také použít poskytovatele Entity Framework, který zahrnuje prostorová podporu. Zobrazit [Podpora zprostředkovatele pro typy prostorových](~/ef6/fundamentals/providers/spatial-support.md) Další informace.

Existují dva typy hlavní prostorových dat: zeměpisné oblasti a geometry. Datový typ geography ukládá elipsoidním data (například GPS zeměpisné šířky a délky koordinuje). Datový typ geometry představuje Euclidean souřadnicovém systému (bez stromové struktury).

## <a name="watch-the-video"></a>Podívejte se na video
Toto video ukazuje, jak mapovat prostorové typy s Entity Framework Designer. Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.

**Přednášející:**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřít Visual Studio 2012
2.  Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**
3.  V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony
4.  Zadejte **SpatialEFDesigner** jako název projektu a klikněte na tlačítko **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Vytvořit nový Model využívající EF designeru

1.  Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**
2.  Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon
3.  Zadejte **UniversityModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**
4.  Na stránce Průvodce datovým modelem Entity vyberte **prázdný Model** v dialogovém okně Výběr obsahu modelu
5.  Klikněte na tlačítko **dokončit**

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.

Průvodce provede následující akce:

-   Generuje EnumTestModel.edmx soubor, který definuje mapování mezi nimi, konceptuálního modelu a model úložiště. Nastaví vlastnost Metadata artefaktů zpracování souboru vložit do výstupního sestavení tak získat soubory generované metadat vložen do sestavení.
-   Přidá odkaz na následující sestavení: objektu EntityFramework, System.ComponentModel.DataAnnotations a System.Data.Entity.
-   Vytvoří soubory UniversityModel.tt a UniversityModel.Context.tt a přidá je v souboru .edmx. Tyto soubory šablon T4 generovat kód, který definuje DbContext odvozený typ a POCO typy, které mapují na entity v modelu edmx

## <a name="add-a-new-entity-type"></a>Přidat nový typ Entity

1.  Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, vyberte **Add -&gt; Entity**, zobrazí se dialogové okno Nová entita
2.  Zadejte **University** pro typ název a určete **UniversityID** název klíče vlastnosti ponechat typ jako **Int32**
3.  Klikněte na tlačítko **OK**
4.  Klikněte pravým tlačítkem na entitu a vyberte **přidat nový -&gt; skalární vlastnost**
5.  Přejmenujte novou vlastnost **název**
6.  Přidat jiný skalární vlastnost a přejmenujte ho na **umístění** otevřete okno Vlastnosti a změna typu novou vlastnost **zeměpisné oblasti**
7.  Uložit model a sestavte projekt
    > [!NOTE]
    > Při sestavování se může zobrazit upozornění na nenamapované entit a přidružení v seznamu chyb. Tato upozornění můžete ignorovat, protože po zvolíme, aby se vygenerovala databáze z modelu, chyby zmizí.

## <a name="generate-database-from-model"></a>Generování databáze z modelu

Nyní jsme můžete vygenerovat databázi, která je založená na modelu.

1.  Klikněte pravým tlačítkem na prázdné místo na ploše a vyberte v návrháři entit **generovat databáze z modelu**
2.  Vyberte vaše Data Connection Dialog Box průvodce generovat databáze je zobrazit kliknutím **nové připojení** tlačítko zadejte **(localdb)\\mssqllocaldb** pro název serveru a  **University** databáze a klikněte na **OK**
3.  Dialogové okno s dotazem, zda chcete vytvořit novou databázi se automaticky otevře, klikněte na tlačítko **Ano**.
4.  Klikněte na tlačítko **Další** a Průvodce vytvořením databáze generuje jazyk pro definování dat (DDL) pro vytvoření databáze vygenerovaný DDL se zobrazí v souhrnu a nastavení dialogové okno pole Poznámka, která jazyka DDL neobsahuje definici pro Tabulka, která se mapuje na typ výčtu
5.  Klikněte na tlačítko **Dokončit** kliknutím na Dokončit nespustí skriptu jazyka DDL.
6.  Průvodce vytvořením databáze provede následující: otevře **UniversityModel.edmx.sql** v editoru jazyka T-SQL generuje úložiště schématu a mapování oddílů EDMX soubor přidá připojovacího řetězce do souboru App.config
7.  Klikněte pravým tlačítkem myši v editoru jazyka T-SQL a vyberte **Execute** připojení k serveru dialogové okno zobrazí, zadejte informace o připojení z kroku 2 a klikněte na tlačítko **Connect**
8.  Chcete-li zobrazit generované schéma, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**

## <a name="persist-and-retrieve-data"></a>Zachovat a načtení dat

Otevřete soubor Program.cs, ve kterém je definována metoda Main. Přidejte následující kód do funkce Main.

Kód přidá dva nové objekty University do kontextu. Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography.FromText. Zeměpisné oblasti bod zastoupeny EDT WellKnownText je předán metodě. Kód poté uloží data. Potom dotaz LINQ, který, který vrací objekt University, kde je nejbližší do zadaného umístění, jeho umístění je vytvořen a spustit.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Kompilace a spuštění aplikace. Program vygeneruje následující výstup:

```
The closest University to you is: School of Fine Art.
```

Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**. Klikněte pravým tlačítkem myši na tabulku a výběrem **Data zobrazení**.

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na tom, jak mapovat pomocí Entity Framework Designer prostorové typy a jak používat prostorové typy v kódu. 
