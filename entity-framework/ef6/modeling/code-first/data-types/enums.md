---
title: Podpora výčtu – Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419106"
---
# <a name="enum-support---code-first"></a>Podpora výčtu – Code First
> [!NOTE]
> **EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Toto video a podrobný návod vám ukáže, jak používat typy výčtu s Entity Framework Code First. Také ukazuje, jak používat výčty v dotazu LINQ.

Tento návod použije Code First k vytvoření nové databáze, ale můžete také použít [Code First k namapování na existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md).

Podpora výčtu byla představena v Entity Framework 5. Chcete-li používat nové funkce, jako jsou například výčty, prostorové datové typy a funkce vracející tabulku, je nutné cílit .NET Framework 4,5. Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.

V Entity Framework výčet může mít následující základní typy: **Byte**, **Int16**, **Int32**, **Int64** nebo **SByte**.

## <a name="watch-the-video"></a>Přehrát video
Toto video ukazuje, jak používat typy výčtu s Entity Framework Code First. Také ukazuje, jak používat výčty v dotazu LINQ.

**Prezentující**: Helena Kornich

**Video**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Požadavky

K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.

 

## <a name="set-up-the-project"></a>Nastavení projektu

1.  Otevřete Visual Studio 2012
2.  V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .
3.  V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .
4.  Jako název projektu zadejte **EnumCodeFirst** a klikněte na **OK** .

## <a name="define-a-new-model-using-code-first"></a>Definování nového modelu pomocí Code First

Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model. Následující kód definuje třídu oddělení.

Kód také definuje výčet DepartmentNames. Ve výchozím nastavení je výčet typu **int** . Vlastnost Name třídy oddělení je typu DepartmentNames.

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
 

## <a name="define-the-dbcontext-derived-type"></a>Definice DbContext odvozeného typu

Kromě definování entit musíte definovat třídu, která je odvozena z DbContext a zpřístupňuje Negenerickými&lt;vlastnosti TEntity&gt;. Vlastnosti Negenerickými&lt;TEntity&gt; umožňují kontextu zjistit, které typy chcete do modelu zahrnout.

Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.

Typy DbContext a Negenerickými jsou definovány v sestavení EntityFramework. Do této knihovny DLL přidáme odkaz pomocí balíčku NuGet EntityFramework.

1.  V Průzkumník řešení klikněte pravým tlačítkem myši na název projektu.
2.  Vyberte **Spravovat balíčky NuGet...**
3.  V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .
4.  Klikněte na **nainstalovat** .

Všimněte si, že kromě sestavení EntityFramework jsou přidány také odkazy na System. ComponentModel. DataAnnotations a System. data. entity.

V horní části souboru Program.cs přidejte následující příkaz using:

``` csharp
using System.Data.Entity;
```

Do Program.cs přidejte definici kontextu. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Zachovat a načíst data

Otevřete soubor Program.cs, kde je definována metoda Main. Do funkce Main přidejte následující kód. Kód přidá do kontextu nový objekt oddělení. Pak data uloží. Kód také spustí dotaz LINQ, který vrací oddělení, kde název je DepartmentNames. English.

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

Zkompilujte a spusťte aplikaci. Program vytvoří následující výstup:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Zobrazení vygenerované databáze

Při prvním spuštění aplikace Entity Framework vytvoří pro vás databázi. Vzhledem k tomu, že jsme nainstalovali Visual Studio 2012, databáze se vytvoří v instanci LocalDB. Ve výchozím nastavení Entity Framework pojmenuje databázi za plně kvalifikovaným názvem odvozeného kontextu (v tomto příkladu, který je **EnumCodeFirst. EnumTestContext**). Následující časy budou použity při dalším použití existující databáze.  

Všimněte si, že pokud provedete změny modelu po vytvoření databáze, měli byste použít Migrace Code First k aktualizaci schématu databáze. Příklad použití migrace naleznete v tématu [Code First do nové databáze](~/ef6/modeling/code-first/workflows/new-database.md) .

Chcete-li zobrazit databázi a data, postupujte následovně:

1.  V hlavní nabídce sady Visual Studio 2012 vyberte možnost **zobrazit** -&gt; **Průzkumník objektů systému SQL Server**.
2.  Pokud LocalDB není v seznamu serverů, klikněte pravým tlačítkem myši na **SQL Server** a vyberte **Přidat SQL Server** pro připojení k instanci LocalDB použít výchozí **ověřování systému Windows** .
3.  Rozbalte uzel LocalDB
4.  Rozložte složku **databáze** , aby se zobrazila nová databáze, a přejděte na poznámku k tabulce **oddělení** , která Code First nevytváří tabulku, která se mapuje na typ výčtu.
5.  Chcete-li zobrazit data, klikněte pravým tlačítkem myši na tabulku a vyberte možnost **Zobrazit data.**

## <a name="summary"></a>Souhrn

V tomto návodu jsme se podívali na to, jak používat typy výčtu s Entity Framework Code First. 
