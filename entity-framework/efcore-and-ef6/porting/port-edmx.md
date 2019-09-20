---
title: Přenos z EF6 do EF Coreho portu založeného na modelu EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148988"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="d5dd6-102">Přenos EF6 modelu založeného na EDMX do EF Core</span><span class="sxs-lookup"><span data-stu-id="d5dd6-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="d5dd6-103">EF Core nepodporuje formát souboru EDMX pro modely.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="d5dd6-104">Nejlepší možností pro portování těchto modelů je vygenerování nového modelu založeného na kódu z databáze pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="d5dd6-105">Nainstalovat EF Core balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="d5dd6-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="d5dd6-106">Nainstalujte balíček `Microsoft.EntityFrameworkCore.Tools` NuGet.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="d5dd6-107">Znovu vygenerovat model</span><span class="sxs-lookup"><span data-stu-id="d5dd6-107">Regenerate the model</span></span>

<span data-ttu-id="d5dd6-108">Nyní můžete pomocí funkce zpětná analýza vytvořit model založený na vaší stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="d5dd6-109">Spusťte následující příkaz v konzole správce balíčků (nástroje – > Správce balíčků NuGet – > konzolu Správce balíčků).</span><span class="sxs-lookup"><span data-stu-id="d5dd6-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="d5dd6-110">V tématu [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) jsou uvedeny možnosti příkazu k vytvoření uživatelského rozhraní podmnožiny tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="d5dd6-111">Tady je příklad příkazu pro vytvoření uživatelského rozhraní modelu z blogovací databáze na instanci SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="d5dd6-112">Odebrat model EF6</span><span class="sxs-lookup"><span data-stu-id="d5dd6-112">Remove EF6 model</span></span>

<span data-ttu-id="d5dd6-113">Nyní byste z aplikace odebrali EF6 model.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="d5dd6-114">Je také dobré opustit balíček NuGet EF6 (EntityFramework), protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="d5dd6-115">Pokud ale nehodláte používat EF6 v žádné oblasti aplikace, pak se při odinstalaci balíčku pomůže Chyba kompilace v částech kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="d5dd6-116">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="d5dd6-116">Update your code</span></span>

<span data-ttu-id="d5dd6-117">V tomto okamžiku se jedná o řešení chyb při kompilaci a revizi kódu, abyste viděli, jestli se to bude mít vliv na změny chování mezi EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="d5dd6-118">Otestování portu</span><span class="sxs-lookup"><span data-stu-id="d5dd6-118">Test the port</span></span>

<span data-ttu-id="d5dd6-119">Vzhledem k tomu, že se vaše aplikace zkompiluje, neznamená to, že je úspěšně přepravovaná na EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="d5dd6-120">Budete muset otestovat všechny oblasti aplikace, aby se zajistilo, že žádná z změn chování nemá negativně vliv na vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5dd6-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
