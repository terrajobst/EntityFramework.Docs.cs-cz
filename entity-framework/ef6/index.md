---
title: Přehled – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 00e5f36788be599ea2e2b44480107f14e2f026c3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998026"
---
# <a name="entity-framework-6"></a><span data-ttu-id="2d7af-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2d7af-102">Entity Framework 6</span></span>
<span data-ttu-id="2d7af-103">Entity Framework 6 (EF6) je vyzkoušená a otestovaná objektově relační Mapovač (O/RM) pro .NET s mnoha letech řídit vývoj funkcí a stabilizaci.</span><span class="sxs-lookup"><span data-stu-id="2d7af-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="2d7af-104">Jako O/RM, snižuje EF6 vzniklé vzájemné napětí Neshoda mezi relačními a objektově orientované světů a umožňuje vývojářům psát aplikace, které pracují s daty uloženými v relačních databází pomocí .NET objektů se silným typem, které představují domény a aplikace není potom potřeba velkou částí dat přístup "tedy jakousi instalaci" kód, který je obvykle potřeba psát.</span><span class="sxs-lookup"><span data-stu-id="2d7af-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="2d7af-105">EF6 implementuje mnoho oblíbených funkcí O/RM:</span><span class="sxs-lookup"><span data-stu-id="2d7af-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="2d7af-106">Mapování [POCO](~/ef6/resources/glossary.md#poco) tříd entit, které nezávisí na typy, které EF</span><span class="sxs-lookup"><span data-stu-id="2d7af-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="2d7af-107">Automatické sledování změn</span><span class="sxs-lookup"><span data-stu-id="2d7af-107">Automatic change tracking</span></span>
- <span data-ttu-id="2d7af-108">Rozlišení identity a jednotek práce</span><span class="sxs-lookup"><span data-stu-id="2d7af-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="2d7af-109">Nemůžou dočkat, až, opožděné a explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="2d7af-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="2d7af-110">Překlad dotazů silného typu pomocí LINQ (Language INtegrated Query)</span><span class="sxs-lookup"><span data-stu-id="2d7af-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="2d7af-111">Bohaté možnosti mapování, včetně podpory pro:</span><span class="sxs-lookup"><span data-stu-id="2d7af-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="2d7af-112">Relace 1: 1, 1 n a n n</span><span class="sxs-lookup"><span data-stu-id="2d7af-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="2d7af-113">Dědičnost (tabulka na hierarchii, tabulky podle typu a na konkrétní třídy)</span><span class="sxs-lookup"><span data-stu-id="2d7af-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="2d7af-114">Komplexní typy</span><span class="sxs-lookup"><span data-stu-id="2d7af-114">Complex types</span></span>
  - <span data-ttu-id="2d7af-115">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="2d7af-115">Stored procedures</span></span>
- <span data-ttu-id="2d7af-116">Vizuálního návrháře pro vytváření modelů entity.</span><span class="sxs-lookup"><span data-stu-id="2d7af-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="2d7af-117">"Code First" prostředí pro vytváření modelů entity napsáním kódu.</span><span class="sxs-lookup"><span data-stu-id="2d7af-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="2d7af-118">Modely můžou být existující databáze generovaném formuláři a potom ručně upravit, nebo mohou být vytvořené z nuly a pak použije k vygenerování nových databází.</span><span class="sxs-lookup"><span data-stu-id="2d7af-118">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="2d7af-119">Integrace s modely aplikace rozhraní .NET Framework, včetně ASP.NET a pomocí vazby dat s WPF a WinForms.</span><span class="sxs-lookup"><span data-stu-id="2d7af-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="2d7af-120">Připojení k databázi na základě technologie ADO.NET a řada poskytovatelů k dispozici pro připojení k serveru SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, atd.</span><span class="sxs-lookup"><span data-stu-id="2d7af-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="2d7af-121">Můžu použít EF6 a EF Core?</span><span class="sxs-lookup"><span data-stu-id="2d7af-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="2d7af-122">EF Core je verzi Entity Frameworku, který má velmi podobné funkce a výhody, které EF6 Modernější, jednoduchý a rozšiřitelný.</span><span class="sxs-lookup"><span data-stu-id="2d7af-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="2d7af-123">EF Core je kompletní revize a obsahuje mnoho nových funkcí nejsou k dispozici v EF6, i když pořád chybí některé z EF6 nejpokročilejší funkce mapování.</span><span class="sxs-lookup"><span data-stu-id="2d7af-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="2d7af-124">Doporučujeme, abyste pomocí EF Core v nové aplikace, tak dlouho, dokud sada funkcí odpovídá vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="2d7af-124">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="2d7af-125">[Porovnání EF Core a EF6](xref:efcore-and-ef6/index) zkontroluje tato volba podrobněji.</span><span class="sxs-lookup"><span data-stu-id="2d7af-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="2d7af-126">Začínáme</span><span class="sxs-lookup"><span data-stu-id="2d7af-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="2d7af-127">Přidejte do projektu balíček EntityFramework NuGet nebo nainstalujte Entity Framework Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d7af-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="2d7af-128">Potom sledujte videa, přečtěte si kurzy a pokročilé dokumentací a pomohou vám využít na maximum EF6.</span><span class="sxs-lookup"><span data-stu-id="2d7af-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="2d7af-129">Starší verze Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2d7af-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="2d7af-130">Toto je v dokumentaci k nejnovější verzi Entity Framework 6, i když většina článku platí také pro minulých verzích.</span><span class="sxs-lookup"><span data-stu-id="2d7af-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="2d7af-131">Podívejte se na [novinky](~/ef6/what-is-new/index.md) a [posledních verzí](~/ef6/what-is-new/past-releases.md) úplný seznam EF vydaných verzí a funkce tyto funkce poprvé představeny.</span><span class="sxs-lookup"><span data-stu-id="2d7af-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
