---
title: Přehled Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: fcd514eebbf09e50403b95c88db04c33520011e9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198053"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) je vyzkoušený a testovaný objektově-relační Mapovač (O/RM) pro .NET s mnoha roky vývoje a stabilizací funkcí.

EF6 v případě O/RM snižuje nesoulad mezi relačními a objektově orientovanými světů a umožňuje vývojářům psát aplikace, které pracují s daty uloženými v relačních databázích pomocí objektů .NET se silnými typy, které reprezentují doména aplikace a eliminují nutnost velké části kódu "domovníace" přístupu k datům, které obvykle potřebují zapisovat.

EF6 implementuje spoustu oblíbených funkcí O/RM:
- Mapování tříd entit [POCO](~/ef6/resources/glossary.md#poco) , které nezávisí na žádných typech EF
- Automatické sledování změn
- Rozlišení identity a jednotka práce
- Eager, opožděné a explicitní načítání
- Překlad silně typových dotazů pomocí LINQ (jazyk integrovaný dotaz)
- Možnosti formátovaného mapování, včetně podpory pro:
  - Relace 1:1, relací 1: n a m:n
  - Dědičnost (tabulka na hierarchii, tabulka na typ a tabulka na konkrétní třídu)
  - Komplexní typy
  - Uložené procedury
- Vizuální Návrhář pro vytváření modelů entit
- Prostředí "Code First" pro vytváření modelů entit pomocí psaní kódu.
- Modely lze buď vygenerovat z existujících databází, a pak je ručně upravit, nebo je můžete vytvořit od začátku a potom použít ke generování nových databází.
- Integrace s .NET Framework aplikačními modely, včetně ASP.NET, a prostřednictvím vázání dat s použitím WPF a WinForms.
- Připojení k databázi založené na ADO.NET a mnoha poskytovatelích dostupných pro připojení k SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 atd.

## <a name="should-i-use-ef6-or-ef-core"></a>Mám použít EF6 nebo EF Core?

EF Core je moderní, odlehčená a rozšiřitelná verze Entity Framework, která má velmi podobné možnosti a výhody EF6.
EF Core je kompletní přepisování a obsahuje mnoho nových funkcí, které nejsou v EF6 k dispozici, i když stále chybí některé z nejpokročilejších možností mapování EF6.
Pokud sada funkcí vyhovuje vašim požadavkům, zvažte použití EF Core v nových aplikacích.
[Compare EF Core &AMP; EF6](xref:efcore-and-ef6/index) prověřuje tuto volbu podrobněji.

## <a name="get-startedef6get-startedmd"></a>[Začínáme](~/ef6/get-started.md)

Přidejte do svého projektu balíček NuGet EntityFramework nebo nainstalujte Entity Framework Tools pro Visual Studio. Pak Sledujte videa, přečtěte si kurzy a pokročilou dokumentaci, která vám pomůžou s tím, aby vám EF6.

## <a name="past-entity-framework-versions"></a>Minulé verze Entity Framework

Toto je dokumentace pro nejnovější verzi Entity Framework 6, i když velká z nich platí i pro starší verze.
Úplný seznam verzí EF a funkcí, které zavádějí, najdete v části [co je nového](~/ef6/what-is-new/index.md) a [starší verze](~/ef6/what-is-new/past-releases.md) .
