---
title: Přehled Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656182"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) je vyzkoušený a testovaný objektově-relační Mapovač (O/RM) pro .NET s mnoha roky vývoje a stabilizací funkcí.

EF6 v případě O/RM snižuje nesoulad mezi relačními a objektově orientovanými světů a umožňuje vývojářům psát aplikace, které pracují s daty uloženými v relačních databázích pomocí objektů .NET se silnými typy, které reprezentují doména aplikace a eliminují nutnost velké části kódu "domovníace" přístupu k datům, které obvykle potřebují zapisovat.

EF6 implementuje spoustu oblíbených funkcí O/RM:
- Mapování tříd entit [POCO](xref:ef6/resources/glossary#poco) , které nezávisí na žádných typech EF
- Automatické sledování změn
- Rozlišení identity a jednotka práce
- Eager, opožděné a explicitní načítání
- Překlad silně typových dotazů pomocí [LINQ (jazyk integrovaný dotaz)](https://aka.ms/AA6hsvu)
- Možnosti formátovaného mapování, včetně podpory pro:
  - Relace 1:1, relací 1: n a m:n
  - Dědičnost (tabulka na hierarchii, tabulka na typ a tabulka na konkrétní třídu)
  - Komplexní typy
  - Uložené procedury
- Vizuální Návrhář pro vytváření modelů entit
- Prostředí "Code First" pro vytváření modelů entit pomocí psaní kódu.
- Modely lze buď vygenerovat z existujících databází, a pak je ručně upravit, nebo je můžete vytvořit od začátku a potom použít ke generování nových databází.
- Integrace s .NET Framework aplikačními modely, včetně ASP.NET, a prostřednictvím vázání dat s použitím WPF a WinForms.
- Připojení k databázi založené na ADO.NET a mnoha [poskytovatelích](xref:ef6/fundamentals/providers/index) dostupných pro připojení k SQL Server, Oracle, MySQL, SQLite, POSTGRESQL, DB2 atd.

## <a name="should-i-use-ef6-or-ef-core"></a>Mám použít EF6 nebo EF Core?

EF Core je moderní, odlehčená a rozšiřitelná verze Entity Framework, která má velmi podobné možnosti a výhody EF6.
EF Core je kompletní přepisování a obsahuje mnoho nových funkcí, které nejsou v EF6 k dispozici, i když stále chybí některé z nejpokročilejších možností mapování EF6.
Pokud sada funkcí vyhovuje vašim požadavkům, zvažte použití EF Core v nových aplikacích.
[Compare EF Core & EF6](xref:efcore-and-ef6/index) prověřuje tuto volbu podrobněji.

## <a name="get-startedxrefef6get-started"></a>[Začínáme](xref:ef6/get-started)

Přidejte do svého projektu balíček NuGet EntityFramework nebo nainstalujte [Entity Framework Tools pro Visual Studio](https://aka.ms/AA6i8c5). Pak Sledujte videa, přečtěte si kurzy a pokročilou dokumentaci, která vám pomůžou s tím, aby vám EF6.

## <a name="past-entity-framework-versions"></a>Minulé verze Entity Framework

Toto je dokumentace pro nejnovější verzi Entity Framework 6, i když velká z nich platí i pro starší verze.
Úplný seznam verzí EF a funkcí, které zavádějí, najdete v části [co je nového](xref:ef6/what-is-new/index) a [starší verze](xref:ef6/what-is-new/past-releases) .
