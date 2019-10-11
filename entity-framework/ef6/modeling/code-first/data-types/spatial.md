---
title: Prostorové Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182653"
---
# <a name="spatial---code-first"></a>Prostorové Code First
> [!NOTE]
> **EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Video a podrobný návod vám ukáže, jak namapovat prostorové typy pomocí Entity Framework Code First. Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.

Tento návod použije Code First k vytvoření nové databáze, ale můžete také použít [Code First do existující databáze](~/ef6/modeling/code-first/workflows/existing-database.md).

Podpora prostorového typu byla představena v Entity Framework 5. Všimněte si, že pro použití nových funkcí, jako je prostorový typ, výčty a funkce vracející tabulku, je nutné cílit .NET Framework 4,5. Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.

Chcete-li použít typy prostorových dat, musíte také použít poskytovatele Entity Framework, který má prostorovou podporu. Další informace najdete v tématu [Podpora poskytovatele pro prostorové typy](~/ef6/fundamentals/providers/spatial-support.md) .

Existují dva hlavní typy prostorových dat: Geografie a geometrie. Zeměpisný datový typ ukládá data ellipsoidal (například souřadnice GPS zeměpisné šířky a délky). Typ dat geometrie představuje systém souřadnic Euclidean (plochý).

## <a name="watch-the-video"></a>Přehrát video
Toto video ukazuje, jak namapovat prostorové typy pomocí Entity Framework Code First. Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.

**Prezentující**: Helena Kornich

**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřete Visual Studio 2012
2.  V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .
3.  V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .
4.  Jako název projektu zadejte **SpatialCodeFirst** a klikněte na **OK** .

## <a name="define-a-new-model-using-code-first"></a>Definování nového modelu pomocí Code First

Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model. Následující kód definuje třídu univerzity.

Univerzita má vlastnost Location typu DbGeography. Chcete-li použít typ DbGeography, musíte přidat odkaz na sestavení System. data. entity a také přidat příkaz System. data. prostor using.

Otevřete soubor Program.cs a vložte následující příkazy using do horní části souboru:

``` csharp
using System.Data.Spatial;
```

Přidejte do souboru Program.cs následující definici třídy University.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>Definice DbContext odvozeného typu

Kromě definování entit musíte definovat třídu, která je odvozena z DbContext a zpřístupňuje Negenerickými @ no__t-0TEntity @ no__t-1 vlastnosti. Vlastnosti Negenerickými @ no__t-0TEntity @ no__t-1 umožňují, aby kontext věděl, které typy chcete do modelu zahrnout.

Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.

Typy DbContext a Negenerickými jsou definovány v sestavení EntityFramework. Do této knihovny DLL přidáme odkaz pomocí balíčku NuGet EntityFramework.

1.  V Průzkumník řešení klikněte pravým tlačítkem myši na název projektu.
2.  Vyberte **Spravovat balíčky NuGet...**
3.  V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .
4.  Klikněte na **nainstalovat** .

Všimněte si, že kromě sestavení EntityFramework je také přidáno odkaz na sestavení System. ComponentModel. DataAnnotations.

V horní části souboru Program.cs přidejte následující příkaz using:

``` csharp
using System.Data.Entity;
```

Do Program.cs přidejte definici kontextu. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Zachovat a načíst data

Otevřete soubor Program.cs, kde je definována metoda Main. Do funkce Main přidejte následující kód.

Kód přidá do kontextu dva nové objekty vysoké školy. Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography. FromText. Zeměpisný bod reprezentovaný jako WellKnownText je předán metodě. Kód pak data uloží. Pak dotaz LINQ, který vrací objekt univerzity, kde je jeho umístění nejblíže zadanému umístění, je vytvořen a proveden.

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

Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup:

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Zobrazení vygenerované databáze

Při prvním spuštění aplikace Entity Framework vytvoří pro vás databázi. Vzhledem k tomu, že jsme nainstalovali Visual Studio 2012, databáze se vytvoří v instanci LocalDB. Ve výchozím nastavení Entity Framework pojmenuje databázi za plně kvalifikovaným názvem odvozeného kontextu (v tomto příkladu je to **SpatialCodeFirst. UniversityContext**). Následující časy budou použity při dalším použití existující databáze.  

Všimněte si, že pokud provedete změny modelu po vytvoření databáze, měli byste použít Migrace Code First k aktualizaci schématu databáze. Příklad použití migrace naleznete v tématu [Code First do nové databáze](~/ef6/modeling/code-first/workflows/new-database.md) .

Chcete-li zobrazit databázi a data, postupujte následovně:

1.  V hlavní nabídce sady Visual Studio 2012 vyberte **zobrazení** - @ no__t-2 **Průzkumník objektů systému SQL Server**.
2.  Pokud LocalDB není v seznamu serverů, klikněte pravým tlačítkem myši na **SQL Server** a vyberte **Přidat SQL Server** pro připojení k instanci LocalDB použijte výchozí **ověřování systému Windows** .
3.  Rozbalte uzel LocalDB
4.  Rozložte složku **databáze** , aby se zobrazila nová databáze, a přejděte do tabulky **univerzity** .
5.  Chcete-li zobrazit data, klikněte pravým tlačítkem myši na tabulku a vyberte možnost **Zobrazit data.**

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na to, jak používat prostorové typy s Entity Framework Code First. 
