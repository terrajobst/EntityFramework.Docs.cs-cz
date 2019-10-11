---
title: Prostor – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182506"
---
# <a name="spatial---ef-designer"></a>Prostor – Návrhář EF
> [!NOTE]
> **EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Video a podrobný návod vám ukáže, jak namapovat prostorové typy pomocí Entity Framework Designer. Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.

Tento návod použije Model First k vytvoření nové databáze, ale Návrhář EF lze použít také s pracovním postupem [Database First](~/ef6/modeling/designer/workflows/database-first.md) k mapování existující databáze.

Podpora prostorového typu byla představena v Entity Framework 5. Všimněte si, že pro použití nových funkcí, jako je prostorový typ, výčty a funkce vracející tabulku, je nutné cílit .NET Framework 4,5. Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.

Chcete-li použít typy prostorových dat, musíte také použít poskytovatele Entity Framework, který má prostorovou podporu. Další informace najdete v tématu [Podpora poskytovatele pro prostorové typy](~/ef6/fundamentals/providers/spatial-support.md) .

Existují dva hlavní typy prostorových dat: Geografie a geometrie. Zeměpisný datový typ ukládá data ellipsoidal (například souřadnice GPS zeměpisné šířky a délky). Typ dat geometrie představuje systém souřadnic Euclidean (plochý).

## <a name="watch-the-video"></a>Přehrát video
Toto video ukazuje, jak namapovat prostorové typy pomocí Entity Framework Designer. Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.

**Prezentující**: Helena Kornich

**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřete Visual Studio 2012
2.  V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .
3.  V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .
4.  Jako název projektu zadejte **SpatialEFDesigner** a klikněte na **OK** .

## <a name="create-a-new-model-using-the-ef-designer"></a>Vytvoření nového modelu pomocí návrháře EF

1.  Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka** .
2.  V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
3.  Jako název souboru zadejte **UniversityModel. edmx** a pak klikněte na **Přidat** .
4.  Na stránce průvodce model EDM (Entity Data Model) vyberte v dialogovém okně Vybrat obsah modelu možnost **prázdný model** .
5.  Klikněte na **Dokončit** .

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.

Průvodce provede následující akce:

-   Vygeneruje soubor EnumTestModel. edmx definující koncepční model, model úložiště a mapování mezi nimi. Nastaví vlastnost zpracování artefaktu metadat souboru EDMX pro vložení do výstupního sestavení tak, aby generované soubory metadat byly vloženy do sestavení.
-   Přidá odkaz na následující sestavení: EntityFramework, System. ComponentModel. DataAnnotations a System. data. entity.
-   Vytvoří soubory UniversityModel.tt a UniversityModel.Context.tt a přidá je do souboru. edmx. Tyto soubory šablon T4 generují kód, který definuje DbContext odvozený typ a POCO typy, které se mapují na entity v modelu. edmx.

## <a name="add-a-new-entity-type"></a>Přidat nový typ entity

1.  Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, vyberte **Add-&gt; entita –** zobrazí se dialogové okno nová entita.
2.  Zadejte **univerzita** pro název typu a jako název vlastnosti zadejte **UniversityID** . ponechte typ jako **Int32** .
3.  Klikněte na **OK** .
4.  Klikněte pravým tlačítkem na entitu a vyberte možnost **Přidat skalární vlastnost New-&gt;** .
5.  Přejmenujte novou vlastnost na **název** .
6.  Přidejte další skalární vlastnost a přejmenujte ji na **umístění** otevřít okno Vlastnosti a změňte typ nové vlastnosti na **geografie** .
7.  Uložte model a sestavte projekt.
    > [!NOTE]
    > Při sestavování se mohou v Seznam chyb zobrazit upozornění na nemapované entity a přidružení. Tato upozornění můžete ignorovat, protože po tom, co se rozhodneme vygenerovat databázi z modelu, budou tyto chyby pryč.

## <a name="generate-database-from-model"></a>Generování databáze z modelu

Teď můžeme vygenerovat databázi založenou na modelu.

1.  Klikněte pravým tlačítkem myši na prázdné místo na Entity Designer povrchu a vyberte možnost **Generovat databázi z modelu** .
2.  Zobrazí se dialogové okno vybrat datové připojení v průvodci generováním databáze, klikněte na tlačítko **nové připojení** zadat **(LocalDB) \\mssqllocaldb** pro název serveru a **University** pro databázi a klikněte na tlačítko **OK.**
3.  Dialogové okno s dotazem, zda chcete vytvořit novou databázi, se zobrazí automaticky, klikněte na tlačítko **Ano**.
4.  Klikněte na tlačítko **Další** a Průvodce vytvořením databáze GENERUJE příkaz DDL (Data Definition Language) pro vytvoření databáze. VYGENEROVANÝ skript DDL se zobrazí v dialogovém okně Souhrn a nastavení – Poznámka, že skript DDL neobsahuje definici tabulky, která je mapována na typ výčtu
5.  Kliknutím na **Dokončit** kliknutím na Dokončit nespustíte skript DDL.
6.  Průvodce vytvořením databáze provede následující akce: Otevře soubor **UniversityModel. edmx. SQL** v editoru T-SQL, vygeneruje oddíly schématu úložiště a mapování souboru EDMX přidá informace o připojovacím řetězci do souboru App. config.
7.  Klikněte pravým tlačítkem myši v editoru T-SQL a vyberte **Spustit** dialog připojit k serveru, zadejte informace o připojení z kroku 2 a klikněte na **připojit** .
8.  Chcete-li zobrazit vygenerované schéma, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat** .

## <a name="persist-and-retrieve-data"></a>Zachovat a načíst data

Otevřete soubor Program.cs, kde je definována metoda Main. Do funkce Main přidejte následující kód.

Kód přidá do kontextu dva nové objekty vysoké školy. Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography. FromText. Zeměpisný bod reprezentovaný jako WellKnownText je předán metodě. Kód pak data uloží. Pak dotaz LINQ, který vrací objekt univerzity, kde je jeho umístění nejblíže zadanému umístění, je vytvořen a proveden.

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

Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup:

```console
The closest University to you is: School of Fine Art.
```

Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat**. Pak klikněte pravým tlačítkem myši na tabulku a vyberte **Zobrazit data**.

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na to, jak namapovat typy prostorů pomocí Entity Framework Designer a jak používat prostorové typy v kódu. 
