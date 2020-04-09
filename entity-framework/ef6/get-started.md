---
title: Začínáme s rámcem entity 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 729dea2c474c35f638ccaf6673550f76e88e2667
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136270"
---
# <a name="get-started-with-entity-framework-6"></a><span data-ttu-id="75902-102">Začínáme s rozhraním entity 6</span><span class="sxs-lookup"><span data-stu-id="75902-102">Get started with Entity Framework 6</span></span>

<span data-ttu-id="75902-103">Tato příručka obsahuje kolekci odkazů na vybrané dokumenty, návody a videa, které vám pomohou rychle začít.</span><span class="sxs-lookup"><span data-stu-id="75902-103">This guide contains a collection of links to selected documentation articles, walkthroughs and videos that can help you get started quickly.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="75902-104">Základy</span><span class="sxs-lookup"><span data-stu-id="75902-104">Fundamentals</span></span>

* [<span data-ttu-id="75902-105">Jak získat Entity Framework</span><span class="sxs-lookup"><span data-stu-id="75902-105">Get Entity Framework</span></span>](~/ef6/fundamentals/install.md)

  <span data-ttu-id="75902-106">Zde se dozvíte, jak přidat entity framework do vašich aplikací a pokud chcete použít EF Designer, ujistěte se, že si to nainstalovat v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75902-106">Here you will learn how to add Entity Framework to your applications and, if you want to use the EF Designer, make sure you get it installed in Visual Studio.</span></span>

* [<span data-ttu-id="75902-107">Vytvoření modelu: Nejprve kód, návrhář EF a pracovní postupy EF</span><span class="sxs-lookup"><span data-stu-id="75902-107">Creating a Model: Code First, the EF Designer, and the EF Workflows</span></span>](~/ef6/modeling/index.md)

  <span data-ttu-id="75902-108">Dáváte přednost zadání ef modelu psaní kódu nebo kreslení polí a čar?</span><span class="sxs-lookup"><span data-stu-id="75902-108">Do you prefer to specify your EF model writing code or drawing boxes and lines?</span></span>
<span data-ttu-id="75902-109">Budete používat EF k mapování objektů na existující databázi nebo chcete EF vytvořit databázi přizpůsobenou vašim objektům?</span><span class="sxs-lookup"><span data-stu-id="75902-109">Are you going to use EF to map your objects to an existing database or would you like EF to create a database tailored for your objects?</span></span>
<span data-ttu-id="75902-110">Zde se dozvíte o dvou různých přístupech k použití EF6: EF Designer a Code First.</span><span class="sxs-lookup"><span data-stu-id="75902-110">Here you learn about two different approaches to use EF6: EF Designer and Code First.</span></span>
<span data-ttu-id="75902-111">Ujistěte se, že budete sledovat diskusi a podívejte se na video o rozdílu.</span><span class="sxs-lookup"><span data-stu-id="75902-111">Make sure you follow the discussion and watch the video about the difference.</span></span>

* [<span data-ttu-id="75902-112">Práce s DbContext</span><span class="sxs-lookup"><span data-stu-id="75902-112">Working with DbContext</span></span>](~/ef6/fundamentals/working-with-dbcontext.md)

  <span data-ttu-id="75902-113">DbContext je první a nejdůležitější EF typ, který potřebujete naučit, jak používat.</span><span class="sxs-lookup"><span data-stu-id="75902-113">DbContext is the first and most important EF type that you need to learn how to use.</span></span> <span data-ttu-id="75902-114">Slouží jako odpalovací rampa pro databázové dotazy a sleduje změny, které provedete v objektech, aby mohly být zachovány zpět do databáze.</span><span class="sxs-lookup"><span data-stu-id="75902-114">It serves as the launchpad for database queries and keeps track of changes you make to objects so that they can be persisted back to the database.</span></span>

* [<span data-ttu-id="75902-115">Položit otázku</span><span class="sxs-lookup"><span data-stu-id="75902-115">Ask a Question</span></span>](~/ef6/resources/get-help.md)

  <span data-ttu-id="75902-116">Zjistěte, jak získat pomoc od odborníků a přispět vlastními odpověďmi komunitě.</span><span class="sxs-lookup"><span data-stu-id="75902-116">Find out how to get help from the experts and contribute your own answers to the community.</span></span>

* [<span data-ttu-id="75902-117">Přispět</span><span class="sxs-lookup"><span data-stu-id="75902-117">Contribute</span></span>](https://github.com/aspnet/EntityFramework6/)

  <span data-ttu-id="75902-118">Entity Framework 6 používá otevřený vývojový model.</span><span class="sxs-lookup"><span data-stu-id="75902-118">Entity Framework 6 uses an open development model.</span></span> <span data-ttu-id="75902-119">Zjistěte, jak můžete ef ještě vylepšit návštěvou našeho úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="75902-119">Find out how you can help make EF even better by visiting our GitHub repository.</span></span>

## <a name="code-first-resources"></a><span data-ttu-id="75902-120">Kód první zdroje</span><span class="sxs-lookup"><span data-stu-id="75902-120">Code First resources</span></span>

  - [<span data-ttu-id="75902-121">Kód nejprve do pracovního postupu existující databáze</span><span class="sxs-lookup"><span data-stu-id="75902-121">Code First to an Existing Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [<span data-ttu-id="75902-122">Kód nejprve do pracovního postupu nové databáze</span><span class="sxs-lookup"><span data-stu-id="75902-122">Code First to a New Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)
  - [<span data-ttu-id="75902-123">Mapování výčtů pomocí kódu jako první</span><span class="sxs-lookup"><span data-stu-id="75902-123">Mapping Enums Using Code First</span></span>](~/ef6/modeling/code-first/data-types/enums.md)
  - [<span data-ttu-id="75902-124">Mapování prostorových typů pomocí kódu jako první</span><span class="sxs-lookup"><span data-stu-id="75902-124">Mapping Spatial Types Using Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)
  - [<span data-ttu-id="75902-125">Psaní vlastní kód první konvence</span><span class="sxs-lookup"><span data-stu-id="75902-125">Writing Custom Code First Conventions</span></span>](~/ef6/modeling/code-first/conventions/custom.md)
  - [<span data-ttu-id="75902-126">Použití první konfigurace code fluent s visual basicem</span><span class="sxs-lookup"><span data-stu-id="75902-126">Using Code First Fluent Configuration with Visual Basic</span></span>](~/ef6/modeling/code-first/fluent/vb.md)
  - [<span data-ttu-id="75902-127">První migrace kódu</span><span class="sxs-lookup"><span data-stu-id="75902-127">Code First Migrations</span></span>](~/ef6/modeling/code-first/migrations/index.md)
  - [<span data-ttu-id="75902-128">Migrace první ho kódu v týmových prostředích</span><span class="sxs-lookup"><span data-stu-id="75902-128">Code First Migrations in Team Environments</span></span>](~/ef6/modeling/code-first/migrations/teams.md)
  - <span data-ttu-id="75902-129">[Automatické migrace prvního kódu](~/ef6/modeling/code-first/migrations/automatic.md) (to se již nedoporučuje)</span><span class="sxs-lookup"><span data-stu-id="75902-129">[Automatic Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (This is no longer recommended)</span></span>

## <a name="ef-designer-resources"></a><span data-ttu-id="75902-130">Zdroje návrháře EF</span><span class="sxs-lookup"><span data-stu-id="75902-130">EF Designer resources</span></span>
  - [<span data-ttu-id="75902-131">První pracovní postup databáze</span><span class="sxs-lookup"><span data-stu-id="75902-131">Database First Workflow</span></span>](~/ef6/modeling/designer/workflows/database-first.md)
  - [<span data-ttu-id="75902-132">První pracovní postup modelu</span><span class="sxs-lookup"><span data-stu-id="75902-132">Model First Workflow</span></span>](~/ef6/modeling/designer/workflows/model-first.md)
  - [<span data-ttu-id="75902-133">Mapování výčtů</span><span class="sxs-lookup"><span data-stu-id="75902-133">Mapping Enums</span></span>](~/ef6/modeling/designer/data-types/enums.md)
  - [<span data-ttu-id="75902-134">Mapování prostorových typů</span><span class="sxs-lookup"><span data-stu-id="75902-134">Mapping Spatial Types</span></span>](~/ef6/modeling/designer/data-types/spatial.md)
  - [<span data-ttu-id="75902-135">Mapování dědičnosti podle hierarchie</span><span class="sxs-lookup"><span data-stu-id="75902-135">Table-Per Hierarchy Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tph.md)
  - [<span data-ttu-id="75902-136">Mapování dědičnosti typu tabulka na typ</span><span class="sxs-lookup"><span data-stu-id="75902-136">Table-Per Type Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tpt.md)
  - [<span data-ttu-id="75902-137">Mapování uložených procedur pro aktualizace</span><span class="sxs-lookup"><span data-stu-id="75902-137">Stored Procedure Mapping for Updates</span></span>](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [<span data-ttu-id="75902-138">Mapování uložené procedury pro dotaz</span><span class="sxs-lookup"><span data-stu-id="75902-138">Stored Procedure Mapping for Query</span></span>](~/ef6/modeling/designer/stored-procedures/query.md)
  - [<span data-ttu-id="75902-139">Dělení entity</span><span class="sxs-lookup"><span data-stu-id="75902-139">Entity Splitting</span></span>](~/ef6/modeling/designer/entity-splitting.md)
  - [<span data-ttu-id="75902-140">Dělení tabulky</span><span class="sxs-lookup"><span data-stu-id="75902-140">Table Splitting</span></span>](~/ef6/modeling/designer/table-splitting.md)
  - <span data-ttu-id="75902-141">[Definování dotazu](~/ef6/modeling/designer/advanced/defining-query.md) (upřesnit)</span><span class="sxs-lookup"><span data-stu-id="75902-141">[Defining Query](~/ef6/modeling/designer/advanced/defining-query.md) (Advanced)</span></span>
  - <span data-ttu-id="75902-142">[Funkce s hodnotou tabulky](~/ef6/modeling/designer/advanced/tvfs.md) (upřesnit)</span><span class="sxs-lookup"><span data-stu-id="75902-142">[Table-Valued Functions](~/ef6/modeling/designer/advanced/tvfs.md) (Advanced)</span></span>

## <a name="other-resources"></a><span data-ttu-id="75902-143">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="75902-143">Other resources</span></span>
  - [<span data-ttu-id="75902-144">Asynchronní dotaz a uložení</span><span class="sxs-lookup"><span data-stu-id="75902-144">Async Query and Save</span></span>](~/ef6/fundamentals/async.md)
  - [<span data-ttu-id="75902-145">Datová vazba s winforms</span><span class="sxs-lookup"><span data-stu-id="75902-145">Databinding with WinForms</span></span>](~/ef6/fundamentals/databinding/winforms.md)
  - [<span data-ttu-id="75902-146">Datová vazba s WPF</span><span class="sxs-lookup"><span data-stu-id="75902-146">Databinding with WPF</span></span>](~/ef6/fundamentals/databinding/wpf.md)
  - <span data-ttu-id="75902-147">[Odpojené scénáře s entitami samočinného sledování](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (to se již nedoporučuje)</span><span class="sxs-lookup"><span data-stu-id="75902-147">[Disconnected scenarios with Self-Tracking Entities](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (This is no longer recommended)</span></span>
