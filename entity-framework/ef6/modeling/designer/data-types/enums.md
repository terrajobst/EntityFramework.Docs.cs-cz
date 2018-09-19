---
title: EF6 podpory – EF designeru – výčet
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 331182c4311565c94cf072eb9b9ad372ac76180a
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283937"
---
# <a name="enum-support---ef-designer"></a>Podpora výčtu – EF designeru
> [!NOTE]
> **EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Tato videa a podrobný návod ukazuje, jak používat typy výčtu s Entity Framework Designer. Také ukazuje, jak používat výčty v dotazu LINQ.

Tento názorný postup použije první Model k vytvoření nové databáze, ale EF designeru je také možné se [Database First](~/ef6/modeling/designer/workflows/database-first.md) pracovního postupu pro mapování na existující databázi.

Podpora výčtu byla zavedena v Entity Framework 5. Pokud chcete použít nové funkce jako výčty prostorové datové typy a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5. Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.

V rozhraní Entity Framework výčet může mít následující základní typy: **bajtů**, **Int16**, **Int32**, **Int64** , nebo **SByte**.

## <a name="watch-the-video"></a>Podívejte se na Video
Toto video ukazuje, jak používat typy výčtu s Entity Framework Designer. Také ukazuje, jak používat výčty v dotazu LINQ.

**Přednášející:**: Julia Kornich

**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřít Visual Studio 2012
2.  Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**
3.  V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony
4.  Zadejte **EnumEFDesigner** jako název projektu a klikněte na tlačítko **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Vytvořit nový Model využívající EF designeru

1.  Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**
2.  Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon
3.  Zadejte **EnumTestModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**
4.  Na stránce Průvodce datovým modelem Entity vyberte **prázdný Model** v dialogovém okně Výběr obsahu modelu
5.  Klikněte na tlačítko **dokončit**

V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.

Průvodce provede následující akce:

-   Generuje EnumTestModel.edmx soubor, který definuje mapování mezi nimi, konceptuálního modelu a model úložiště. Nastaví vlastnost Metadata artefaktů zpracování souboru vložit do výstupního sestavení tak získat soubory generované metadat vložen do sestavení.
-   Přidá odkaz na následující sestavení: objektu EntityFramework, System.ComponentModel.DataAnnotations a System.Data.Entity.
-   Vytvoří soubory EnumTestModel.tt a EnumTestModel.Context.tt a přidá je v souboru .edmx. Tyto soubory šablon T4 generovat kód, který definuje DbContext odvozený typ a POCO typy, které mapují na entity v modelu edmx.

## <a name="add-a-new-entity-type"></a>Přidat nový typ Entity

1.  Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, vyberte **Add -&gt; Entity**, zobrazí se dialogové okno Nová entita
2.  Zadejte **oddělení** pro typ název a určete **DepartmentID** název klíče vlastnosti ponechat typ jako **Int32**
3.  Klikněte na tlačítko **OK**
4.  Klikněte pravým tlačítkem na entitu a vyberte **přidat nový -&gt; skalární vlastnost**
5.  Přejmenujte novou vlastnost **název**
6.  Změna typu novou vlastnost **Int32** (ve výchozím nastavení, nové vlastnosti je typu String) Chcete-li změnit typ, otevřete okno Vlastnosti a změňte vlastnost typ **Int32**
7.  Přidat jiný skalární vlastnost a přejmenujte ho na **rozpočtu**, změňte typ na **Decimal**

## <a name="add-an-enum-type"></a>Přidání typu výčtu

1.  V Návrháři Entity Framework Designer klikněte pravým tlačítkem na název vlastnosti, vyberte **převést na výčet**

    ![Převést na výčet](~/ef6/media/converttoenum.png)

2.  V **přidat výčtu** dialogového okna zadejte **DepartmentNames** názvem typu výčtu, změňte základní typ na **Int32**, a potom přidat následující členy typu: angličtině, Matematické a hospodárnost

    ![Přidat typ výčtu](~/ef6/media/addenumtype.png)

3.  Stisknutím klávesy **OK**
4.  Uložit model a sestavte projekt
    > [!NOTE]
    > Při sestavování se může zobrazit upozornění na nenamapované entit a přidružení v seznamu chyb. Tato upozornění můžete ignorovat, protože po zvolíme, aby se vygenerovala databáze z modelu, chyby zmizí.

Když se podíváte na okno Vlastnosti, všimnete si, že typ vlastnosti názvu byl změněn na **DepartmentNames** a typ výčtu nově přidaných byl přidán do seznamu typů.

Pokud přejdete do okna prohlížeče modelu, uvidíte, že typ se taky přidala do uzlu typy výčtu.

![Prohlížeč modelu](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Nové typy výčtu z tohoto okna můžete také přidat kliknutím pravým tlačítkem myši a vyberete **přidat typ výčtu**. Po vytvoření typu se zobrazí v seznamu typů a bylo by možné přiřadit k vlastnosti

## <a name="generate-database-from-model"></a>Generování databáze z modelu

Nyní jsme můžete vygenerovat databázi, která je založená na modelu.

1.  Klikněte pravým tlačítkem na prázdné místo na ploše a vyberte v návrháři entit **generovat databáze z modelu**
2.  Vyberte vaše Data Connection Dialog Box průvodce generovat databáze je zobrazit kliknutím **nové připojení** tlačítko zadejte **(localdb)\\mssqllocaldb** pro název serveru a  **EnumTest** databáze a klikněte na **OK**
3.  Dialogové okno s dotazem, zda chcete vytvořit novou databázi se automaticky otevře, klikněte na tlačítko **Ano**.
4.  Klikněte na tlačítko **Další** a Průvodce vytvořením databáze generuje jazyk pro definování dat (DDL) pro vytvoření databáze vygenerovaný DDL se zobrazí v souhrnu a nastavení dialogové okno pole Poznámka, která jazyka DDL neobsahuje definici pro Tabulka, která se mapuje na typ výčtu
5.  Klikněte na tlačítko **Dokončit** kliknutím na Dokončit nespustí skriptu jazyka DDL.
6.  Průvodce vytvořením databáze provede následující: otevře **EnumTest.edmx.sql** v editoru jazyka T-SQL generuje úložiště schématu a mapování oddílů EDMX soubor přidá připojovacího řetězce do souboru App.config
7.  Klikněte pravým tlačítkem myši v editoru jazyka T-SQL a vyberte **Execute** připojení k serveru dialogové okno zobrazí, zadejte informace o připojení z kroku 2 a klikněte na tlačítko **Connect**
8.  Chcete-li zobrazit generované schéma, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**

## <a name="persist-and-retrieve-data"></a>Zachovat a načtení dat

Otevřete soubor Program.cs, ve kterém je definována metoda Main. Přidejte následující kód do funkce Main. Kód přidá nový objekt oddělení do kontextu. Pak uloží data. Kód také spustí dotaz LINQ, který vrátí oddělení, kde název je DepartmentNames.English.

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

Kompilace a spuštění aplikace. Program vygeneruje následující výstup:

```
DepartmentID: 1 Name: English
```

Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**. Klikněte pravým tlačítkem myši na tabulku a výběrem **Data zobrazení**.

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na tom, jak mapovat typy výčtu pomocí Entity Framework Designer a jak používat výčty v kódu. 
