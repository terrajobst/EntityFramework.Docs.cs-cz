---
title: Přehled – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 1efadf4484a13d5df2a2f11aad3d0e8f9ceff543
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315630"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) je vyzkoušená a otestovaná objektově relační Mapovač (O/RM) pro .NET s mnoha letech řídit vývoj funkcí a stabilizaci.

Jako O/RM, snižuje EF6 vzniklé vzájemné napětí Neshoda mezi relačními a objektově orientované světů a umožňuje vývojářům psát aplikace, které pracují s daty uloženými v relačních databází pomocí .NET objektů se silným typem, které představují domény a aplikace není potom potřeba velkou částí dat přístup "tedy jakousi instalaci" kód, který je obvykle potřeba psát.

EF6 implementuje mnoho oblíbených funkcí O/RM:
- Mapování [POCO](~/ef6/resources/glossary.md#poco) tříd entit, které nezávisí na typy, které EF
- Automatické sledování změn
- Rozlišení identity a jednotek práce
- Nemůžou dočkat, až, opožděné a explicitní načtení
- Překlad dotazů silného typu pomocí LINQ (Language INtegrated Query)
- Bohaté možnosti mapování, včetně podpory pro:
  - Relace 1: 1, 1 n a n n
  - Dědičnost (tabulka na hierarchii, tabulky podle typu a na konkrétní třídy)
  - Komplexní typy
  - Uložené procedury
- Vizuálního návrháře pro vytváření modelů entity.
- "Code First" prostředí pro vytváření modelů entity napsáním kódu.
- Modely může být vygenerované z existující databáze a potom ručně upravit, nebo mohou být vytvořené z nuly a pak použije k vygenerování nových databází.
- Integrace s modely aplikace rozhraní .NET Framework, včetně ASP.NET a pomocí vazby dat s WPF a WinForms.
- Připojení k databázi na základě technologie ADO.NET a řada poskytovatelů k dispozici pro připojení k serveru SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, atd.

## <a name="should-i-use-ef6-or-ef-core"></a>Můžu použít EF6 a EF Core?

EF Core je verzi Entity Frameworku, který má velmi podobné funkce a výhody, které EF6 Modernější, jednoduchý a rozšiřitelný.
EF Core je kompletní revize a obsahuje mnoho nových funkcí nejsou k dispozici v EF6, i když pořád chybí některé z EF6 nejpokročilejší funkce mapování.
Zvažte použití EF Core v nové aplikace, pokud je sada funkcí odpovídá vašim požadavkům.
[Porovnání EF Core a EF6](xref:efcore-and-ef6/index) zkontroluje tato volba podrobněji.

## <a name="get-startedef6get-startedmd"></a>[Začínáme](~/ef6/get-started.md)

Přidejte do projektu balíček EntityFramework NuGet nebo nainstalujte Entity Framework Tools for Visual Studio. Potom sledujte videa, přečtěte si kurzy a pokročilé dokumentací a pomohou vám využít na maximum EF6.

## <a name="past-entity-framework-versions"></a>Starší verze Entity Framework

Toto je v dokumentaci k nejnovější verzi Entity Framework 6, i když většina článku platí také pro minulých verzích.
Podívejte se na [novinky](~/ef6/what-is-new/index.md) a [posledních verzí](~/ef6/what-is-new/past-releases.md) úplný seznam EF vydaných verzí a funkce tyto funkce poprvé představeny.
