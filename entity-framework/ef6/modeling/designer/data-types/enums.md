---
title: Podpora výčtu – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182523"
---
# <a name="enum-support---ef-designer"></a>Podpora výčtu – Návrhář EF
> [!NOTE]
> **EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Toto video a podrobný návod vám ukáže, jak používat typy výčtu s Entity Framework Designer. Také ukazuje, jak používat výčty v dotazu LINQ.

Tento návod použije Model First k vytvoření nové databáze, ale Návrhář EF lze použít také s pracovním postupem [Database First](~/ef6/modeling/designer/workflows/database-first.md) k mapování existující databáze.

Podpora výčtu byla představena v Entity Framework 5. Chcete-li používat nové funkce, jako jsou například výčty, prostorové datové typy a funkce vracející tabulku, je nutné cílit .NET Framework 4,5. Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.

V Entity Framework výčet může mít následující základní typy: **Byte**, **Int16**, **Int32**, **Int64** nebo **SByte**.

## <a name="watch-the-video"></a>Přehrát video
Toto video ukazuje, jak používat typy výčtu s Entity Framework Designer. Také ukazuje, jak používat výčty v dotazu LINQ.

**Prezentující**: Helena Kornich

**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Předpoklady

K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřete Visual Studio 2012
2.  V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .
3.  V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .
4.  Jako název projektu zadejte **EnumEFDesigner** a klikněte na **OK** .

## <a name="create-a-new-model-using-the-ef-designer"></a>Vytvoření nového modelu pomocí návrháře EF

1.  Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka** .
2.  V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .
3.  Jako název souboru zadejte **EnumTestModel. edmx** a pak klikněte na **Přidat** .
4.  Na stránce průvodce model EDM (Entity Data Model) vyberte v dialogovém okně Vybrat obsah modelu možnost **prázdný model** .
5.  Klikněte na **Dokončit** .

Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.

Průvodce provede následující akce:

-   Vygeneruje soubor EnumTestModel. edmx definující koncepční model, model úložiště a mapování mezi nimi. Nastaví vlastnost zpracování artefaktu metadat souboru EDMX pro vložení do výstupního sestavení tak, aby generované soubory metadat byly vloženy do sestavení.
-   Přidá odkaz na následující sestavení: EntityFramework, System. ComponentModel. DataAnnotations a System. data. entity.
-   Vytvoří soubory EnumTestModel.tt a EnumTestModel.Context.tt a přidá je do souboru. edmx. Tyto soubory šablon T4 generují kód, který definuje DbContext odvozený typ a POCO typy, které se mapují na entity v modelu. edmx.

## <a name="add-a-new-entity-type"></a>Přidat nový typ entity

1.  Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, vyberte **Add-&gt; entita –** zobrazí se dialogové okno nová entita.
2.  Zadejte pro název typu **oddělení** a pro název vlastnosti Key zadejte **DepartmentID** a ponechte typ jako typ **Int32** .
3.  Klikněte na **OK** .
4.  Klikněte pravým tlačítkem na entitu a vyberte možnost **Přidat skalární vlastnost New-&gt;** .
5.  Přejmenujte novou vlastnost na **název** .
6.  Změnit typ nové vlastnosti na **Int32** (ve výchozím nastavení je nová vlastnost typu String), aby se typ změnil, otevřete okno Vlastnosti a změňte vlastnost Type na **Int32** .
7.  Přidejte další skalární vlastnost a přejmenujte ji na **rozpočet**, změňte typ na **Decimal** .

## <a name="add-an-enum-type"></a>Přidat typ výčtu

1.  V Entity Framework Designer klikněte pravým tlačítkem na vlastnost Name, vyberte **převést na enum** .

    ![Převést na výčet](~/ef6/media/converttoenum.png)

2.  V dialogovém okně **Přidat výčet** zadejte **DepartmentNames** pro název typu výčtu, změňte podkladový typ na **Int32**a pak přidejte následující členy do typu: Angličtina, matematický a ekonomie

    ![Přidat typ výčtu](~/ef6/media/addenumtype.png)

3.  Stiskněte **OK**
4.  Uložte model a sestavte projekt.
    > [!NOTE]
    > Při sestavování se mohou v Seznam chyb zobrazit upozornění na nemapované entity a přidružení. Tato upozornění můžete ignorovat, protože po tom, co se rozhodneme vygenerovat databázi z modelu, budou tyto chyby pryč.

Pokud se podíváte na okno Vlastnosti, všimnete si, že typ vlastnosti Name byl změněn na **DepartmentNames** a nově přidaný typ výčtu byl přidán do seznamu typů.

Pokud přepnete do okna prohlížeče modelů, uvidíte, že typ byl také přidán do uzlu typy výčtu.

![Prohlížeč modelů](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Můžete také přidat nové typy výčtu z tohoto okna kliknutím pravým tlačítkem myši a výběrem možnosti **Přidat typ výčtu**. Jakmile se typ vytvoří, zobrazí se v seznamu typů a bude možné ho přidružit k vlastnosti.

## <a name="generate-database-from-model"></a>Generování databáze z modelu

Teď můžeme vygenerovat databázi založenou na modelu.

1.  Klikněte pravým tlačítkem myši na prázdné místo na Entity Designer povrchu a vyberte možnost **Generovat databázi z modelu** .
2.  Zobrazí se dialogové okno vybrat datové připojení v průvodci generováním databáze, klikněte na tlačítko **nové připojení** zadat **(LocalDB) \\mssqllocaldb** pro název serveru a **EnumTest** pro databázi a klikněte na **OK** .
3.  Dialogové okno s dotazem, zda chcete vytvořit novou databázi, se zobrazí automaticky, klikněte na tlačítko **Ano**.
4.  Klikněte na tlačítko **Další** a Průvodce vytvořením databáze GENERUJE příkaz DDL (Data Definition Language) pro vytvoření databáze. VYGENEROVANÝ skript DDL se zobrazí v dialogovém okně Souhrn a nastavení – Poznámka, že skript DDL neobsahuje definici tabulky, která je mapována na typ výčtu
5.  Kliknutím na **Dokončit** kliknutím na Dokončit nespustíte skript DDL.
6.  Průvodce vytvořením databáze provede následující akce: Otevře soubor **EnumTest. edmx. SQL** v editoru T-SQL, vygeneruje oddíly schématu úložiště a mapování souboru EDMX přidá informace o připojovacím řetězci do souboru App. config.
7.  Klikněte pravým tlačítkem myši v editoru T-SQL a vyberte **Spustit** dialog připojit k serveru, zadejte informace o připojení z kroku 2 a klikněte na **připojit** .
8.  Chcete-li zobrazit vygenerované schéma, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat** .

## <a name="persist-and-retrieve-data"></a>Zachovat a načíst data

Otevřete soubor Program.cs, kde je definována metoda Main. Do funkce Main přidejte následující kód. Kód přidá do kontextu nový objekt oddělení. Pak data uloží. Kód také spustí dotaz LINQ, který vrací oddělení, kde název je DepartmentNames. English.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup:

```console
DepartmentID: 1 Name: English
```

Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat**. Pak klikněte pravým tlačítkem myši na tabulku a vyberte **Zobrazit data**.

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na to, jak namapovat typy výčtu pomocí Entity Framework Designer a jak používat výčty v kódu. 
