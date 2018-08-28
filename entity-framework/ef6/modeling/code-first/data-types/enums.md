---
title: Podpora výčtu - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 529370ef7aba3975a36cbfdf5c439ee87b563c7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997231"
---
# <a name="enum-support---code-first"></a>Podpora výčtu – kód nejprve
> [!NOTE]
> **EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Tato videa a podrobný návod ukazuje, jak používat typy výčtu s Entity Framework Code First. Také ukazuje, jak používat výčty v dotazu LINQ.

Tento názorný postup použije Code First k vytvoření nové databáze, ale můžete také použít [Code First pro mapování na existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md).

Podpora výčtu byla zavedena v Entity Framework 5. Pokud chcete použít nové funkce jako výčty prostorové datové typy a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5. Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.

V rozhraní Entity Framework výčet může mít následující základní typy: **bajtů**, **Int16**, **Int32**, **Int64** , nebo **SByte**.

## <a name="watch-the-video"></a>Podívejte se na video
Toto video ukazuje, jak používat typy výčtu s Entity Framework Code First. Také ukazuje, jak používat výčty v dotazu LINQ.

**Přednášející:**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Předpoklady

Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.

 

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřít Visual Studio 2012
2.  Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**
3.  V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony
4.  Zadejte **EnumCodeFirst** jako název projektu a klikněte na tlačítko **OK**

## <a name="define-a-new-model-using-code-first"></a>Definovat nový Model využívající Code First

Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény). Následující kód definuje třídu oddělení.

Kód také definuje výčet DepartmentNames. Ve výchozím nastavení, je výčet **int** typu. Vlastnost Name u třídy oddělení je DepartmentNames typu.

Otevřete soubor Program.cs a vložte následující definice tříd.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
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

Všimněte si, že kromě sestavení EntityFramework System.ComponentModel.DataAnnotations a System.Data.Entity sestavení jsou přidány odkazy na také.

V horní části souboru Program.cs přidejte následující příkaz using:

``` csharp
using System.Data.Entity;
```

V souboru Program.cs přidejte definici kontextu. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Zachovat a načtení dat

Otevřete soubor Program.cs, ve kterém je definována metoda Main. Přidejte následující kód do funkce Main. Kód přidá nový objekt oddělení do kontextu. Pak uloží data. Kód také spustí dotaz LINQ, který vrátí oddělení, kde název je DepartmentNames.English.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Kompilace a spuštění aplikace. Program vygeneruje následující výstup:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Zobrazí se vygenerovaný databáze

Když spustíte aplikaci poprvé, Entity Framework vytvoří databázi za vás. Když máme nainstalované Visual Studio 2012, vytvoří se databáze na instanci LocalDB. Ve výchozím nastavení, Entity Framework pojmenuje databázi za plně kvalifikovaný název odvozené kontextu (v tomto příkladu, který je **EnumCodeFirst.EnumTestContext**). Následující časy, které se použije existující databázi.  

Všimněte si, že pokud provedete změny modelu po vytvoření databáze, by měl použít migrace Code First aktualizovat schéma databáze. Zobrazit [Code First pro novou databázi](~/ef6/modeling/code-first/workflows/new-database.md) pro příklad použití migrace.

Chcete-li zobrazit databáze a dat, postupujte takto:

1.  V hlavní nabídce sady Visual Studio 2012 vyberte **zobrazení**  - &gt; **Průzkumník objektů systému SQL Server**.
2.  Pokud LocalDB se nenachází v seznamu serverů, klikněte pravým tlačítkem myši na **systému SQL Server** a vyberte **přidat SQL Server** použijte výchozí **ověřování Windows** pro připojení k Instanci LocalDB
3.  Rozbalte uzel LocalDB
4.  Rozbalit **databází** složky a zobrazí novou databázi procházením **oddělení** tabulky Poznámka Code First nevytvoří tabulku, která se mapuje na typ výčtu
5.  K zobrazení dat, klikněte pravým tlačítkem na tabulku a vyberte **zobrazení dat**

## <a name="summary"></a>Souhrn

V tomto názorném postupu jsme se podívali na tom, jak používat typy výčtu s Entity Framework Code First. 
