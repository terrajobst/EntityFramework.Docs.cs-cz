---
title: Přehled rámce entity 6 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416332"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) je osvědčená objekt-relační mapovač (O/RM) pro .NET s mnohaletou vývojem a stabilizací funkcí.

Jako O/RM EF6 snižuje nesoulad impedance mezi relačnía objekty a objektově orientované světy, umožňuje vývojářům psát aplikace, které interagují s daty uloženými v relačních databázích pomocí objektů silného typu .NET, které představují doménu aplikace, a eliminuje potřebu velké části kódu "instalatérské" přístupu k datům, který obvykle potřebují k zápisu.

EF6 implementuje mnoho populárních funkcí O/RM:
- Mapování tříd entit [POCO,](xref:ef6/resources/glossary#poco) které nezávisí na žádných typech EF
- Automatické sledování změn
- Řešení identity a jednotka práce
- Dychtivé, líné a explicitní načítání
- Překlad dotazů silného typu pomocí [LINQ (Jazyk INtegrated Query)](https://aka.ms/AA6hsvu)
- Bohaté možnosti mapování, včetně podpory:
  - Vztahy 1:1, 1:N a N:N
  - Dědičnost (tabulka podle hierarchie, tabulka podle typu a tabulka na konkrétní třídu)
  - Komplexní typy
  - Uložené procedury
- Vizuální návrhář pro vytváření modelů entit.
- "Code First" zkušenosti vytvořit modely entit zápisem kódu.
- Modely mohou být buď generovány z existujících databází a pak ručně editovány, nebo je lze vytvořit od začátku a poté použít ke generování nových databází.
- Integrace s aplikačními modely rozhraní .NET Framework, včetně ASP.NET a prostřednictvím datové vazby, s WPF a WinForms.
- Připojení k databázi na základě ADO.NET a mnoha [poskytovatelů, kteří](xref:ef6/fundamentals/providers/index) jsou k dispozici pro připojení k SQL Serveru, Oracle, MySQL, SQLite, PostgreSQL, DB2 atd.

## <a name="should-i-use-ef6-or-ef-core"></a>Mám použít EF6 nebo EF Core?

EF Core je modernější, lehčí a rozšiřitelná verze entity frameworku, která má velmi podobné funkce a výhody jako EF6.
EF Core je kompletní přepsání a obsahuje mnoho nových funkcí, které nejsou k dispozici v EF6, i když také stále postrádá některé z nejpokročilejších možností mapování EF6.
Zvažte použití EF Core v nových aplikacích, pokud sada funkcí odpovídá vašim požadavkům.
[Porovnejte EF Core & EF6](xref:efcore-and-ef6/index) zkoumá tuto volbu podrobněji.

## <a name="get-started"></a>[Začínáme](xref:ef6/get-started)

Přidejte balíček EntityFramework NuGet do projektu nebo nainstalujte [nástroje entity framework pro visual studio](https://aka.ms/AA6i8c5). Pak sledujte videa, přečtěte si kurzy a pokročilou dokumentaci, která vám pomůže co nejlépe využít EF6.

## <a name="past-entity-framework-versions"></a>Verze rozhraní pro minulé entity

Toto je dokumentace pro nejnovější verzi entity Framework 6, i když velká část z toho platí i pro minulé verze.
Podívejte se [na what's new](xref:ef6/what-is-new/index) a [past releases](xref:ef6/what-is-new/past-releases) a najdete úplný seznam verzí EF a funkcí, které zavedli.
