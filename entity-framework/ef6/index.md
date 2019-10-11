---
title: Přehled Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 9561a7c4b645896cb4e248cb094c6954ed4bcdf1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181431"
---
# <a name="entity-framework-6"></a><span data-ttu-id="3c46e-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3c46e-102">Entity Framework 6</span></span>
<span data-ttu-id="3c46e-103">Entity Framework 6 (EF6) je vyzkoušený a testovaný objektově-relační Mapovač (O/RM) pro .NET s mnoha roky vývoje a stabilizací funkcí.</span><span class="sxs-lookup"><span data-stu-id="3c46e-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="3c46e-104">EF6 v případě O/RM snižuje nesoulad mezi relačními a objektově orientovanými světů a umožňuje vývojářům psát aplikace, které pracují s daty uloženými v relačních databázích pomocí objektů .NET se silnými typy, které reprezentují doména aplikace a eliminují nutnost velké části kódu "domovníace" přístupu k datům, které obvykle potřebují zapisovat.</span><span class="sxs-lookup"><span data-stu-id="3c46e-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="3c46e-105">EF6 implementuje spoustu oblíbených funkcí O/RM:</span><span class="sxs-lookup"><span data-stu-id="3c46e-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="3c46e-106">Mapování tříd entit [POCO](~/ef6/resources/glossary.md#poco) , které nezávisí na žádných typech EF</span><span class="sxs-lookup"><span data-stu-id="3c46e-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="3c46e-107">Automatické sledování změn</span><span class="sxs-lookup"><span data-stu-id="3c46e-107">Automatic change tracking</span></span>
- <span data-ttu-id="3c46e-108">Rozlišení identity a jednotka práce</span><span class="sxs-lookup"><span data-stu-id="3c46e-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="3c46e-109">Eager, opožděné a explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="3c46e-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="3c46e-110">Překlad silně typových dotazů pomocí LINQ (jazyk integrovaný dotaz)</span><span class="sxs-lookup"><span data-stu-id="3c46e-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="3c46e-111">Možnosti formátovaného mapování, včetně podpory pro:</span><span class="sxs-lookup"><span data-stu-id="3c46e-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="3c46e-112">Relace 1:1, relací 1: n a m:n</span><span class="sxs-lookup"><span data-stu-id="3c46e-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="3c46e-113">Dědičnost (tabulka na hierarchii, tabulka na typ a tabulka na konkrétní třídu)</span><span class="sxs-lookup"><span data-stu-id="3c46e-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="3c46e-114">Komplexní typy</span><span class="sxs-lookup"><span data-stu-id="3c46e-114">Complex types</span></span>
  - <span data-ttu-id="3c46e-115">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="3c46e-115">Stored procedures</span></span>
- <span data-ttu-id="3c46e-116">Vizuální Návrhář pro vytváření modelů entit</span><span class="sxs-lookup"><span data-stu-id="3c46e-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="3c46e-117">Prostředí "Code First" pro vytváření modelů entit pomocí psaní kódu.</span><span class="sxs-lookup"><span data-stu-id="3c46e-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="3c46e-118">Modely lze buď vygenerovat z existujících databází, a pak je ručně upravit, nebo je můžete vytvořit od začátku a potom použít ke generování nových databází.</span><span class="sxs-lookup"><span data-stu-id="3c46e-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="3c46e-119">Integrace s .NET Framework aplikačními modely, včetně ASP.NET, a prostřednictvím vázání dat s použitím WPF a WinForms.</span><span class="sxs-lookup"><span data-stu-id="3c46e-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="3c46e-120">Připojení k databázi založené na ADO.NET a mnoha poskytovatelích dostupných pro připojení k SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 atd.</span><span class="sxs-lookup"><span data-stu-id="3c46e-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="3c46e-121">Mám použít EF6 nebo EF Core?</span><span class="sxs-lookup"><span data-stu-id="3c46e-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="3c46e-122">EF Core je moderní, odlehčená a rozšiřitelná verze Entity Framework, která má velmi podobné možnosti a výhody EF6.</span><span class="sxs-lookup"><span data-stu-id="3c46e-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="3c46e-123">EF Core je kompletní přepisování a obsahuje mnoho nových funkcí, které nejsou v EF6 k dispozici, i když stále chybí některé z nejpokročilejších možností mapování EF6.</span><span class="sxs-lookup"><span data-stu-id="3c46e-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="3c46e-124">Pokud sada funkcí vyhovuje vašim požadavkům, zvažte použití EF Core v nových aplikacích.</span><span class="sxs-lookup"><span data-stu-id="3c46e-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="3c46e-125">[Compare EF Core &AMP; EF6](xref:efcore-and-ef6/index) prověřuje tuto volbu podrobněji.</span><span class="sxs-lookup"><span data-stu-id="3c46e-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="3c46e-126">Začínáme</span><span class="sxs-lookup"><span data-stu-id="3c46e-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="3c46e-127">Přidejte do svého projektu balíček NuGet EntityFramework nebo nainstalujte Entity Framework Tools pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c46e-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="3c46e-128">Pak Sledujte videa, přečtěte si kurzy a pokročilou dokumentaci, která vám pomůžou s tím, aby vám EF6.</span><span class="sxs-lookup"><span data-stu-id="3c46e-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="3c46e-129">Minulé verze Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3c46e-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="3c46e-130">Toto je dokumentace pro nejnovější verzi Entity Framework 6, i když velká z nich platí i pro starší verze.</span><span class="sxs-lookup"><span data-stu-id="3c46e-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="3c46e-131">Úplný seznam verzí EF a funkcí, které zavádějí, najdete v části [co je nového](~/ef6/what-is-new/index.md) a [starší verze](~/ef6/what-is-new/past-releases.md) .</span><span class="sxs-lookup"><span data-stu-id="3c46e-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
