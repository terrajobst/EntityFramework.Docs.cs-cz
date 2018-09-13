---
title: Upgrade na rozhraní Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 2e2dacfe67238bdb7fd1f31f784319049f0f2cb0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490945"
---
# <a name="upgrading-to-entity-framework-6"></a>Upgrade na rozhraní Entity Framework 6

V předchozích verzích EF kód byl rozdělen mezi základní knihovny (primárně knihovně System.Data.Entity.dll) dodán jako součást rozhraní .NET Framework a out-of-band (OOB) knihovny (primárně EntityFramework.dll) součástí balíčku NuGet. EF6 má kód ze základní knihovny a zahrnuje je do knihovny OOB. To bylo nezbytné, aby se povolit EF má být provedeno opensourcový a, abyste mohli rozvíjet věci jinak rychle z rozhraní .NET Framework. Důsledkem tohoto je, že aplikace bude nutné znovu sestavit na jakou přesunutý.

To by měl být jednoduché pro aplikace, které využívají DbContext jako dodané v EF 4.1 a novějším. O něco více práce je nutná pro aplikace, které využívají ObjectContext, ale stále není těžko provádět.

Tady je kontrolní seznam toho, co je třeba provést upgrade existující aplikaci do EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Nainstalujte balíček EF6 NuGet

Budete muset upgradovat na nový modul runtime Entity Framework 6.

1. Klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet...**  
2. V části **Online** kartě vyberte **EntityFramework** a klikněte na tlačítko **instalace**  
   > [!NOTE]
   > Pokud předchozí verze EntityFramework NuGet byl nainstalován balíček to bude ho upgradovat na EF6.

Alternativně můžete spustit následující příkaz z konzoly Správce balíčků:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Ujistěte se, že jsou odebrány odkazy na knihovně System.Data.Entity.dll sestavení

Instaluje se balíček EF6 NuGet by měl automaticky odebrání veškerých odkazů na System.Data.Entity z projektu za vás.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Zaměnit žádné modely EF návrháře (EDMX) Chcete-li použít generování kódu 6.x EF

Pokud máte jakékoli modelů vytvořených pomocí EF designeru, je potřeba aktualizovat šablony generování kódu pro generování kódu EF6 kompatibilní.

> [!NOTE]
> Aktuálně nejsou k dispozici pro sadu Visual Studio 2012 a 2013 pouze EF 6.x DbContext generátor šablony.

1. Odstraňte existující šablony generování kódu. Tyto soubory budou mít názvy obvykle  **\<edmx_file_name\>.tt** a  **\<edmx_file_name\>. Context.tt** a vnořit souboru edmx, v Průzkumníku řešení. Můžete vybrat šablony v Průzkumníku řešení a stiskněte klávesu **Del** klíč k jejich odstranění.  
   > [!NOTE]
   > V webových projektů šablony nesmí být vnořen v souladu s souboru edmx, ale uvedené spolu s ho v Průzkumníku řešení.  

   > [!NOTE]
   > V projektech VB.NET je potřeba povolit "Zobrazit všechny soubory" bude moci zobrazit soubory vnořené šablony.
2. Přidejte příslušné šablony EF 6.x pro generování kódu. Otevřete váš model v EF designeru klikněte pravým tlačítkem na návrhové ploše a vyberte **přidat položku generování kódu...**
    - Pokud používáte rozhraní API DbContext (doporučeno) pak **EF 6.x DbContext generátor** bude k dispozici v části **Data** kartu.  
      > [!NOTE]
      > Pokud používáte sadu Visual Studio 2012, musíte nainstalovat nástroje EF 6 na používají tuto šablonu. Zobrazit [získat Entity Framework](~/ef6/fundamentals/install.md) podrobnosti.  

    - Pokud používáte rozhraní API pro objekt ObjectContext pak bude nutné vybrat **Online** kartu a vyhledejte **EF 6.x EntityObject generátor**.  
3. Pokud jste použili všechna vlastní nastavení pro šablony generování kódu, budete muset znovu aplikovat na aktualizované šablony.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Obory názvů aktualizace pro všechny typy EF core používá

Obory názvů pro typy DbContext a Code First nezměnily. To znamená pro mnoho aplikací, které používají EF 4.1 nebo novější nemusíte nic měnit.

Typy jako objekt ObjectContext, které byly dříve v knihovně System.Data.Entity.dll byly přesunuty do nové obory názvů. To znamená, že je potřeba aktualizovat vaše *pomocí* nebo *Import* direktivy pro sestavení EF6.

Obecným pravidlem pro obor názvů změny je, že jakýkoli typ v System.Data.* se přesune do System.Data.Entity.Core.*. Jinými slovy, stačí vložit **Entity.Core.** Po System.Data. Příklad:

- System.Data.EntityException = > System.Data. **Entity.Core.** EntityException  
- System.Data.Objects.ObjectContext = > System.Data. **Entity.Core.** Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core.** Objects.DataClasses.RelationshipManager  

Tyto typy jsou *Core* obory názvů vzhledem k tomu, že nejsou použity přímo pro většinu aplikací na základě DbContext. Některé typy, které byly součástí knihovně System.Data.Entity.dll se stále používají často a přímo pro aplikace založené na kontext databáze a dosud byl přesunut do *Core* obory názvů. Toto jsou:

- System.Data.EntityState = > System.Data. **Entity.** EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Tato třída se přejmenoval; Třída s názvem starého stále existuje a funguje, ale je nyní označeny jako zastaralé.  
- System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Tato třída se přejmenoval; Třída s názvem starého stále existuje a funguje, ale nyní označeny jako zastaralé.)  
- Prostorový třídy (například DbGeography, DbGeometry) byly přesunuty z System.Data.Spatial = > System.Data. **Entity.** Prostorových

> [!NOTE]
> Některé typy v oboru názvů System.Data jsou System.Data.dll, který není EF sestavení. Tyto typy nepřesunuli a tak zůstanou beze změny jejich obory názvů.
