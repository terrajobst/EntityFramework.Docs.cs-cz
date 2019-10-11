---
title: Přenos z EF6 do EF Core-portový model založený na EDMX – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182066"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="37296-102">Přenos EF6 modelu založeného na EDMX do EF Core</span><span class="sxs-lookup"><span data-stu-id="37296-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="37296-103">EF Core nepodporuje formát souboru EDMX pro modely.</span><span class="sxs-lookup"><span data-stu-id="37296-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="37296-104">Nejlepší možností pro portování těchto modelů je vygenerování nového modelu založeného na kódu z databáze pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37296-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="37296-105">Nainstalovat EF Core balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="37296-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="37296-106">Nainstalujte balíček NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="37296-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="37296-107">Znovu vygenerovat model</span><span class="sxs-lookup"><span data-stu-id="37296-107">Regenerate the model</span></span>

<span data-ttu-id="37296-108">Nyní můžete pomocí funkce zpětná analýza vytvořit model založený na vaší stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="37296-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="37296-109">Spusťte následující příkaz v konzole správce balíčků (nástroje – > Správce balíčků NuGet – > konzolu Správce balíčků).</span><span class="sxs-lookup"><span data-stu-id="37296-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="37296-110">V tématu [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) jsou uvedeny možnosti příkazu k vytvoření uživatelského rozhraní podmnožiny tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="37296-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="37296-111">Tady je příklad příkazu pro vytvoření uživatelského rozhraní modelu z blogovací databáze na instanci SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="37296-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="37296-112">Odebrat model EF6</span><span class="sxs-lookup"><span data-stu-id="37296-112">Remove EF6 model</span></span>

<span data-ttu-id="37296-113">Nyní byste z aplikace odebrali EF6 model.</span><span class="sxs-lookup"><span data-stu-id="37296-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="37296-114">Je také dobré opustit balíček NuGet EF6 (EntityFramework), protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37296-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="37296-115">Pokud ale nehodláte používat EF6 v žádné oblasti aplikace, pak se při odinstalaci balíčku pomůže Chyba kompilace v částech kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="37296-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="37296-116">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="37296-116">Update your code</span></span>

<span data-ttu-id="37296-117">V tomto okamžiku se jedná o řešení chyb při kompilaci a revizi kódu, abyste viděli, jestli se to bude mít vliv na změny chování mezi EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="37296-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="37296-118">Otestování portu</span><span class="sxs-lookup"><span data-stu-id="37296-118">Test the port</span></span>

<span data-ttu-id="37296-119">Vzhledem k tomu, že se vaše aplikace zkompiluje, neznamená to, že je úspěšně přepravovaná na EF Core.</span><span class="sxs-lookup"><span data-stu-id="37296-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="37296-120">Budete muset otestovat všechny oblasti aplikace, aby se zajistilo, že žádná z změn chování nemá negativně vliv na vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37296-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
