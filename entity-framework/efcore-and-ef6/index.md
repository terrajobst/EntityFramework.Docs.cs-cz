---
title: Porovnání EF Core a EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997121"
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF Core a EF6

Existují dvě různé verze rozhraní Entity Framework, Entity Framework Core a Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) je technologie přístup vyzkoušená a otestovaná data mnohaletých zkušeností s funkcemi a stabilizací. To 2008, původně vydaný jako součást rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1. Od verze EF4.1 byla odeslaná jako [balíček EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) – právě jeden z nejoblíbenějších balíčků na NuGet.org.

EF6 i nadále podporované produkty a bude dál zobrazovat, opravy chyb a menší vylepšení nechystáte nějakou dobu pocházet.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (jádro EF) je jednoduchý, rozšiřitelná a multiplatformní verze Entity Framework. EF Core přináší řadu vylepšení a nových funkcí ve srovnání s EF6. EF Core ve stejnou dobu, je nový kód pro základní a ne vyspělá jako EF6.

EF Core udržuje prostředí pro vývojáře z EF6 a většina nejvyšší úrovně rozhraní API zůstanou stejné, v tak působí povědomý lidé, kteří použili EF6 EF Core. EF Core ve stejnou dobu, je sestavena zcela novou sadu základních komponent. To znamená, že EF Core nedědí automaticky všechny funkce z EF6. Zatímco některé z těchto funkcí se zobrazí v budoucích verzích, jiné méně často používaným funkcím se nebude implementovat v EF Core.

Nové, rozšiřitelné a zjednodušené core má zároveň nám to umožnilo přidáte některé funkce na EF Core, který se nebude implementovat v EF6 (například alternativní klíče, dávkové aktualizace a smíšené klientské/databáze vyhodnocení v LINQ dotazy).
