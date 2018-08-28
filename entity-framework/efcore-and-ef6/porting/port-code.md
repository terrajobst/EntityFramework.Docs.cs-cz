---
title: Přenesení z EF6 do EF Core – přenesení modelu na úrovni kódu
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997046"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="fcc22-102">Přenesení modelu na základě kódu EF6 do EF Core</span><span class="sxs-lookup"><span data-stu-id="fcc22-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="fcc22-103">Pokud jste si přečetli všechna upozornění a jste připraveni k portu, pak tady jsou některé pokyny, které vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="fcc22-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="fcc22-104">Instalace balíčků EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="fcc22-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="fcc22-105">Pokud chcete použít EF Core, nainstalujte balíček NuGet pro poskytovatele databáze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="fcc22-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="fcc22-106">Například při cílení na systém SQL Server, nainstalujte `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="fcc22-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="fcc22-107">Zobrazit [poskytovatelé databází](../../core/providers/index.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="fcc22-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="fcc22-108">Pokud máte v úmyslu použít migrace, pak byste měli nainstalovat také `Microsoft.EntityFrameworkCore.Tools` balíčku.</span><span class="sxs-lookup"><span data-stu-id="fcc22-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="fcc22-109">Je v pořádku nechte instalaci balíčku EF6 NuGet (EntityFramework), jak EF Core a EF6 můžou být použité vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fcc22-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="fcc22-110">Ale pokud nejsou máte v úmyslu použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže dojít k chybám kompilace na části kódu, který je potřeba věnovat pozornost.</span><span class="sxs-lookup"><span data-stu-id="fcc22-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="fcc22-111">Obory názvů prohození</span><span class="sxs-lookup"><span data-stu-id="fcc22-111">Swap namespaces</span></span>

<span data-ttu-id="fcc22-112">Většina rozhraní API, které můžete použít EF6 jsou v `System.Data.Entity` obor názvů (a související sub obory názvů).</span><span class="sxs-lookup"><span data-stu-id="fcc22-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="fcc22-113">První změny kódu, je do odkládacího souboru `Microsoft.EntityFrameworkCore` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fcc22-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="fcc22-114">By obvykle začínat odvozené kontextu souboru s kódem a pak vycházejí z něj, vyřešení chyb kompilace, když k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="fcc22-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="fcc22-115">Konfigurace kontextu (připojení atd.)</span><span class="sxs-lookup"><span data-stu-id="fcc22-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="fcc22-116">Jak je popsáno v [zajistit EF Core bude práce pro aplikace](ensure-requirements.md), EF Core má méně magic kolem zjišťování pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="fcc22-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="fcc22-117">Je potřeba přepsat `OnConfiguring` metoda v odvozené kontextu a použití rozhraní API konkrétní poskytovatele databáze nastavit připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="fcc22-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="fcc22-118">Většina aplikací EF6 uložit připojovací řetězec v aplikacích `App/Web.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="fcc22-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="fcc22-119">V EF Core čtete tento připojovací řetězec pomocí `ConfigurationManager` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fcc22-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="fcc22-120">Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní framework bude moct pomocí tohoto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fcc22-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="fcc22-121">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="fcc22-121">Update your code</span></span>

<span data-ttu-id="fcc22-122">V tomto okamžiku je otázkou vyřešení chyb kompilace a revizi kódu zobrazíte, pokud změny chování ovlivní vás.</span><span class="sxs-lookup"><span data-stu-id="fcc22-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="fcc22-123">Migrace existující</span><span class="sxs-lookup"><span data-stu-id="fcc22-123">Existing migrations</span></span>

<span data-ttu-id="fcc22-124">Ve skutečnosti není k dispozici vhodná způsob, jak přenést existující migrace EF6 do EF Core.</span><span class="sxs-lookup"><span data-stu-id="fcc22-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="fcc22-125">Pokud je to možné doporučujeme předpokládají, že se použily všechny předchozí přenesení z EF6 do databáze a pak zahájení migrace schématu od bodu, pomocí EF Core.</span><span class="sxs-lookup"><span data-stu-id="fcc22-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="fcc22-126">Chcete-li to provést, použijte `Add-Migration` příkaz pro přidání migrace po modelu se přenáší na EF Core.</span><span class="sxs-lookup"><span data-stu-id="fcc22-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="fcc22-127">Potom odeberte všechen kód z `Up` a `Down` metody vygenerované migrace.</span><span class="sxs-lookup"><span data-stu-id="fcc22-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="fcc22-128">Následné migrace při porovnání modelu při této počáteční migrace se automaticky.</span><span class="sxs-lookup"><span data-stu-id="fcc22-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="fcc22-129">Port testu</span><span class="sxs-lookup"><span data-stu-id="fcc22-129">Test the port</span></span>

<span data-ttu-id="fcc22-130">To, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenést do EF Core.</span><span class="sxs-lookup"><span data-stu-id="fcc22-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="fcc22-131">Je potřeba otestovat všechny oblasti vaší aplikace k zajištění, že žádné změny chování mít nepříznivý vliv na aplikace.</span><span class="sxs-lookup"><span data-stu-id="fcc22-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
