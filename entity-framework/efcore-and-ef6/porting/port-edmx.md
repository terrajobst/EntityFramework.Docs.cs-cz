---
title: Přenos z EF6 na EF Core – portování modelu založeného na EDMX – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416921"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="ece9c-102">Přenesení modelu založeného na EF6 EDMX na ef core</span><span class="sxs-lookup"><span data-stu-id="ece9c-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="ece9c-103">EF Core nepodporuje formát souboru EDMX pro modely.</span><span class="sxs-lookup"><span data-stu-id="ece9c-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="ece9c-104">Nejlepší možností pro přenos těchto modelů je generovat nový model založený na kódu z databáze pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece9c-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="ece9c-105">Instalace balíčků EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="ece9c-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="ece9c-106">Nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ece9c-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="ece9c-107">Regenerovat model</span><span class="sxs-lookup"><span data-stu-id="ece9c-107">Regenerate the model</span></span>

<span data-ttu-id="ece9c-108">Nyní můžete použít funkci zpětné analýzy k vytvoření modelu na základě existující databáze.</span><span class="sxs-lookup"><span data-stu-id="ece9c-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="ece9c-109">Spusťte následující příkaz v konzole Správce balíčků (Nástroje –> Správce balíčků NuGet –> konzoli správce balíčků).</span><span class="sxs-lookup"><span data-stu-id="ece9c-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="ece9c-110">Viz [Console Správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pro možnosti příkazu pro vytvoření uživatelského okna podmnožinu tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="ece9c-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="ece9c-111">Například zde je příkaz k generování uživatelského terminálu modelu z databáze blogování na instanci SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ece9c-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="ece9c-112">Odebrat model EF6</span><span class="sxs-lookup"><span data-stu-id="ece9c-112">Remove EF6 model</span></span>

<span data-ttu-id="ece9c-113">Nyní byste odebrat model EF6 z aplikace.</span><span class="sxs-lookup"><span data-stu-id="ece9c-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="ece9c-114">Je v pořádku ponechat balíček EF6 NuGet (EntityFramework) nainstalovaný, protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece9c-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="ece9c-115">Pokud však nemáte v úmyslu používat EF6 v žádné oblasti aplikace, pak odinstalace balíčku pomůže poskytnout chyby kompilace na části kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="ece9c-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="ece9c-116">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="ece9c-116">Update your code</span></span>

<span data-ttu-id="ece9c-117">V tomto okamžiku je to otázka adresování chyb kompilace a revize kódu, abyste zjistili, zda se změní chování mezi EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="ece9c-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="ece9c-118">Testování portu</span><span class="sxs-lookup"><span data-stu-id="ece9c-118">Test the port</span></span>

<span data-ttu-id="ece9c-119">Jen proto, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenesena na EF Core.</span><span class="sxs-lookup"><span data-stu-id="ece9c-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="ece9c-120">Budete muset otestovat všechny oblasti aplikace, abyste zajistili, že žádná ze změn chování nemá nepříznivý vliv na vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece9c-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
