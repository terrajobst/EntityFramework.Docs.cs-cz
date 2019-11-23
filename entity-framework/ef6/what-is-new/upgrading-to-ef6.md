---
title: Upgrade na Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182101"
---
# <a name="upgrading-to-entity-framework-6"></a>Upgrade na Entity Framework 6

V předchozích verzích EF byl kód rozdělen mezi základní knihovny (primárně System. data. entity. dll) dodávané jako součást .NET Framework a mimo jiné knihovny OOB (primárně EntityFramework. dll) dodávané do balíčku NuGet. EF6 přebírá kód ze základních knihoven a zahrnuje ho do knihoven OOB. To bylo nutné, aby bylo možné, že EF má být otevřen open source a v případě, že bude moci vyvíjet jiný tempo od .NET Framework. Důvodem je, že aplikace bude nutné znovu sestavit proti přesunutým typům.

To by mělo být jednoduché pro aplikace, které využívají DbContext jako dodávané v EF 4,1 a novějších. Pro aplikace, které využívají ObjectContext, se vyžaduje trochu více práce, ale stále není těžké.

Tady je kontrolní seznam věcí, které je třeba provést při upgradu existující aplikace na EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Nainstalujte balíček NuGet EF6.

Musíte upgradovat na nový modul runtime Entity Framework 6.

1. Klikněte pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet...**  
2. Na kartě **online** vyberte **EntityFramework** a klikněte na **nainstalovat** .  
   > [!NOTE]
   > Pokud byla nainstalována předchozí verze balíčku NuGet EntityFramework, bude upgradována na EF6.

Případně můžete z konzoly Správce balíčků spustit následující příkaz:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Ujistěte se, že jsou odebrány odkazy na sestavení System. data. entity. dll.

Instalace balíčku NuGet EF6 by měla automaticky odebrat všechny odkazy na System. data. entity z vašeho projektu.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Proměňte všechny modely EF Designer (EDMX), aby bylo možné použít kód EF 6. x.

Pokud máte v Návrháři EF vytvořené nějaké modely, budete muset aktualizovat šablony pro generování kódu, aby se generoval kód kompatibilní s EF6.

> [!NOTE]
> V současné době jsou k dispozici pouze šablony generátoru DbContext v EF 6. x pro Visual Studio 2012 a 2013.

1. Odstranit existující šablony pro generování kódu. Tyto soubory budou obvykle pojmenovány **\<edmx_file_name\>. TT** a **\<edmx_file_name\>. Context.tt** a vnořovat do souboru EDMX v Průzkumník řešení. Můžete vybrat šablony v Průzkumník řešení a stisknout klávesu **del** a odstranit je.  
   > [!NOTE]
   > V projektech webů nebudou šablony vnořené do souboru EDMX, ale jsou uvedeny vedle sebe v Průzkumník řešení.  

   > [!NOTE]
   > V projektech VB.NET budete muset povolit možnost Zobrazit všechny soubory, aby bylo možné zobrazit vnořené soubory šablon.
2. Přidejte příslušnou šablonu pro generování kódu EF 6. x. Otevřete model v Návrháři EF, klikněte pravým tlačítkem na návrhovou plochu a vyberte **Přidat položku pro generování kódu...**
    - Pokud používáte rozhraní API DbContext (doporučeno), bude na kartě **data** k dispozici **generátor EF 6. x DbContext** .  
      > [!NOTE]
      > Pokud používáte Visual Studio 2012, budete muset nainstalovat nástroje EF 6, abyste měli tuto šablonu. Podrobnosti najdete v tématu [získání Entity Framework](~/ef6/fundamentals/install.md) .  

    - Pokud používáte rozhraní API ObjectContext, budete muset vybrat **online** kartu a vyhledat **generátor EF 6. x objektů EntityObject**.  
3. Pokud jste v šablonách generování kódu použili jakékoli úpravy, budete je muset znovu použít na aktualizované šablony.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. aktualizujte obory názvů pro všechny používané typy EF Core.

Obory názvů pro typy DbContext a Code First se nezměnily. To znamená, že pro mnoho aplikací, které používají EF 4,1 nebo novější, nebudete muset cokoli měnit.

Typy, jako ObjectContext dříve v System. data. entity. dll byly přesunuty na nové obory názvů. To znamená, že možná budete muset aktualizovat direktivy *using* nebo *Import* pro sestavení pro EF6.

Obecné pravidlo pro změny oboru názvů je, že jakýkoliv typ v System. data. * je přesunut do System. data. entity. Core. *. Jinými slovy, stačí vložit **entity. Core.** po System. data. Příklad:

- System. data. EntityException = > System. data. **Entita. Core** EntityException  
- System. data. Objects. ObjectContext = > System. data. **Entita. Core** Objects. ObjectContext  
- System. data. Objects. DataClasses. RelationshipManager = > System. data. **Entita. Core** Objects. DataClasses. RelationshipManager  

Tyto typy jsou v *základních* oborech názvů, protože nejsou používány přímo pro většinu aplikací založených na DbContext. Některé typy, které byly součástí System. data. entity. dll, jsou stále používány často a přímo pro aplikace založené na DbContext, a proto nebyly přesunuty do *základních* oborů názvů. Toto jsou:

- System. data. EntityState = > System. data. **Entita**. EntityState  
- System. data. Objects. DataClasses. EdmFunctionAttribute = > System. data. **Entita. DbFunctionAttribute**  
  > [!NOTE]
  > Tato třída byla přejmenována; Třída se starým názvem stále existuje a funguje, ale nyní je označena jako zastaralá.  
- System. data. Objects. EntityFunctions = > System. data. **Entita. DbFunctions**  
  > [!NOTE]
  > Tato třída byla přejmenována; Třída se starým názvem stále existuje a funguje, ale nyní je označena jako zastaralá.)  
- Prostorové třídy (například DbGeography, DbGeometry) se přesunuly z System. data. prostor = > System. data. **Entita**. Klepání

> [!NOTE]
> Některé typy v oboru názvů System. data jsou v System. data. dll, což není sestavení EF. Tyto typy se nepřesunuly, takže jejich obory názvů zůstanou beze změny.
