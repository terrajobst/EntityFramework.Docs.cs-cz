---
title: Přenesení z EF6 do EF Core – přenesení modelu na bázi EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997408"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="d3605-102">Přenesení modelu na bázi EDMX EF6 do EF Core</span><span class="sxs-lookup"><span data-stu-id="d3605-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="d3605-103">EF Core nepodporuje formát souboru EDMX pro modely.</span><span class="sxs-lookup"><span data-stu-id="d3605-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="d3605-104">Nejlepší možností k portu tyto modely, je nový model založený na kódu Generovat z databáze pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3605-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="d3605-105">Instalace balíčků EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="d3605-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="d3605-106">Nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3605-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="d3605-107">Znovu generovat model</span><span class="sxs-lookup"><span data-stu-id="d3605-107">Regenerate the model</span></span>

<span data-ttu-id="d3605-108">Funkce zpětné analýzy nyní můžete vytvořit model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="d3605-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="d3605-109">Spuštěním následujícího příkazu v konzole Správce balíčků (Nástroje –> Správce balíčků NuGet –> Konzola správce balíčků).</span><span class="sxs-lookup"><span data-stu-id="d3605-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="d3605-110">Zobrazit [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pro parametry příkazu generovat uživatelské rozhraní pro podmnožinu tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="d3605-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="d3605-111">Například tady je příkaz scaffold modelu z blogovací databáze na instanci serveru SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d3605-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="d3605-112">Odebrat EF6 model</span><span class="sxs-lookup"><span data-stu-id="d3605-112">Remove EF6 model</span></span>

<span data-ttu-id="d3605-113">EF6 model by teď odebrat z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3605-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="d3605-114">Je v pořádku nechte instalaci balíčku EF6 NuGet (EntityFramework), jak EF Core a EF6 můžou být použité vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3605-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="d3605-115">Ale pokud nejsou máte v úmyslu použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže dojít k chybám kompilace na části kódu, který je potřeba věnovat pozornost.</span><span class="sxs-lookup"><span data-stu-id="d3605-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="d3605-116">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="d3605-116">Update your code</span></span>

<span data-ttu-id="d3605-117">V tomto okamžiku je otázkou adresování chyby při kompilaci a revize kódu a zjistěte, jestli změny chování mezi EF6 a EF Core ovlivní.</span><span class="sxs-lookup"><span data-stu-id="d3605-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="d3605-118">Port testu</span><span class="sxs-lookup"><span data-stu-id="d3605-118">Test the port</span></span>

<span data-ttu-id="d3605-119">To, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenést do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d3605-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="d3605-120">Je potřeba otestovat všechny oblasti vaší aplikace k zajištění, že žádné změny chování mít nepříznivý vliv na aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3605-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="d3605-121">Zobrazit [Začínáme s EF Core v ASP.NET Core s existující databázi](xref:core/get-started/aspnetcore/existing-db) Další informace o tom, jak pracovat s existující databázi,</span><span class="sxs-lookup"><span data-stu-id="d3605-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
