---
title: Prostorový - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
caps.latest.revision: 3
ms.openlocfilehash: 03557820bc689d8e7389a1f84121b7eeaa904df1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914260"
---
# <a name="spatial---code-first"></a>Prostorový - Code First
> [!NOTE]
> **EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Videa a podrobný návod ukazuje, jak mapovat prostorové typy s Entity Framework Code First. Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.

Tento názorný postup použije Code First k vytvoření nové databáze, ale můžete také použít [Code First pro existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md).

Podpora prostorových typů byla zavedena v Entity Framework 5. Všimněte si, že pokud chcete používat nové funkce jako prostorových typů výčtů a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5. Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.

Používat typy prostorových dat je nutné také použít poskytovatele Entity Framework, který zahrnuje prostorová podporu. Zobrazit [Podpora zprostředkovatele pro typy prostorových](~/ef6/fundamentals/providers/spatial-support.md) Další informace.

Existují dva typy hlavní prostorových dat: zeměpisné oblasti a geometry. Datový typ geography ukládá elipsoidním data (například GPS zeměpisné šířky a délky koordinuje). Datový typ geometry představuje Euclidean souřadnicovém systému (bez stromové struktury).

## <a name="watch-the-video"></a>Podívejte se na video
Toto video ukazuje, jak mapovat prostorové typy s Entity Framework Code First. Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.

**Přednášející:**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřít Visual Studio 2012
2.  Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**
3.  V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony
4.  Zadejte **SpatialCodeFirst** jako název projektu a klikněte na tlačítko **OK**

## <a name="define-a-new-model-using-code-first"></a>Definovat nový Model využívající Code First

Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény). Následující kód definuje třídu University.

Univerzity má vlastnost umístění DbGeography typu. Pokud chcete použít typ DbGeography, musíte přidat odkaz na sestavení System.Data.Entity a také přidat System.Data.Spatial příkaz using.

Otevřete soubor Program.cs a vložte následující příkazy using v horní části souboru:

``` csharp
using System.Data.Spatial;
```

Do souboru Program.cs přidejte následující definice třídy University.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>Definování uvolněn objekt DbContext odvozený typ

Kromě definování entit, budete muset definovat třídu, která je odvozena od položky DbContext a zveřejňuje DbSet&lt;TEntity&gt; vlastnosti. DbSet&lt;TEntity&gt; vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu.

Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.

Kontext databáze a DbSet typy jsou definovány v sestavení objektu EntityFramework. Přidáme odkaz na tuto knihovnu DLL pomocí balíčku NuGet objektu EntityFramework.

1.  V Průzkumníku řešení klikněte pravým tlačítkem na název projektu.
2.  Vyberte **spravovat balíčky NuGet...**
3.  V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku.
4.  Klikněte na tlačítko **instalace**

Všimněte si, že kromě sestavení objektu EntityFramework, také se přidá odkaz na sestavení System.ComponentModel.DataAnnotations.

V horní části souboru Program.cs přidejte následující příkaz using:

``` csharp
using System.Data.Entity;
```

V souboru Program.cs přidejte definici kontextu. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Zachovat a načtení dat

Otevřete soubor Program.cs, ve kterém je definována metoda Main. Přidejte následující kód do funkce Main.

Kód přidá dva nové objekty University do kontextu. Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography.FromText. Zeměpisné oblasti bod zastoupeny EDT WellKnownText je předán metodě. Kód poté uloží data. Potom dotaz LINQ, který, který vrací objekt University, kde je nejbližší do zadaného umístění, jeho umístění je vytvořen a spustit.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
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

## <a name="view-the-generated-database"></a>Zobrazí se vygenerovaný databáze

Když spustíte aplikaci poprvé, Entity Framework vytvoří databázi za vás. Když máme nainstalované Visual Studio 2012, vytvoří se databáze na instanci LocalDB. Ve výchozím nastavení, Entity Framework pojmenuje databázi za plně kvalifikovaný název odvozené kontextu (v tomto příkladu, který je **SpatialCodeFirst.UniversityContext**). Následující časy, které se použije existující databázi.  

Všimněte si, že pokud provedete změny modelu po vytvoření databáze, by měl použít migrace Code First aktualizovat schéma databáze. Zobrazit [Code First pro novou databázi](~/ef6/modeling/code-first/workflows/new-database.md) pro příklad použití migrace.

Chcete-li zobrazit databáze a dat, postupujte takto:

1.  V hlavní nabídce sady Visual Studio 2012 vyberte **zobrazení**  - &gt; **Průzkumník objektů systému SQL Server**.
2.  Pokud LocalDB se nenachází v seznamu serverů, klikněte pravým tlačítkem myši na **systému SQL Server** a vyberte **přidat SQL Server** použijte výchozí **ověřování Windows** pro připojení k instanci LocalDB
3.  Rozbalte uzel LocalDB
4.  Rozbalit **databází** složky a zobrazí novou databázi procházením **univerzity** tabulky
5.  K zobrazení dat, klikněte pravým tlačítkem na tabulku a vyberte **zobrazení dat**

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na tom, jak používat prostorové typy s Entity Framework Code First. 
