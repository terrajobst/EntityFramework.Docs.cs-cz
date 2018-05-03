---
title: Portování ze EF6 na jádro EF – portování Model na základě EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="0d97d-102">Portování Model na základě EDMX EF6 na jádro EF</span><span class="sxs-lookup"><span data-stu-id="0d97d-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="0d97d-103">Základní EF nepodporuje formát souboru EDMX pro modely.</span><span class="sxs-lookup"><span data-stu-id="0d97d-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="0d97d-104">Nejlepší možnost k portu tyto modely je chcete generovat nový model založené na kódu z databáze pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d97d-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="0d97d-105">Instalace balíčků NuGet EF jádra</span><span class="sxs-lookup"><span data-stu-id="0d97d-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="0d97d-106">Nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0d97d-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="0d97d-107">Znovu vygenerovat modelu</span><span class="sxs-lookup"><span data-stu-id="0d97d-107">Regenerate the model</span></span>

<span data-ttu-id="0d97d-108">Nyní můžete funkci zpětné analýzy pro vytvoření modelu na základě existující databáze.</span><span class="sxs-lookup"><span data-stu-id="0d97d-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="0d97d-109">Spusťte následující příkaz v konzole Správce balíčků (Nástroje –> Správce balíčků NuGet –> Konzola správce balíčků).</span><span class="sxs-lookup"><span data-stu-id="0d97d-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="0d97d-110">V tématu [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) možnosti příkazu k automatickému vygenerování podmnožinu tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="0d97d-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="0d97d-111">Zde je třeba příkaz chcete vygenerovat model z databáze blogu ve vaší instanci SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="0d97d-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="0d97d-112">Odebrat EF6 modelu</span><span class="sxs-lookup"><span data-stu-id="0d97d-112">Remove EF6 model</span></span>

<span data-ttu-id="0d97d-113">EF6 model by nyní odebrat z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d97d-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="0d97d-114">Je v pořádku nechte balíček EF6 NuGet (EntityFramework) nainstalované, tak, jak EF jádra a EF6 můžou být použité-souběžného ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0d97d-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="0d97d-115">Ale pokud nejsou hodláte použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže poskytnout chyby při kompilaci na části kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="0d97d-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="0d97d-116">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="0d97d-116">Update your code</span></span>

<span data-ttu-id="0d97d-117">V tomto okamžiku bude stačit adresování chyby při kompilaci a prostudovali kód pro případ, že změny chování mezi EF6 a EF základní ovlivní.</span><span class="sxs-lookup"><span data-stu-id="0d97d-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="0d97d-118">Testování port</span><span class="sxs-lookup"><span data-stu-id="0d97d-118">Test the port</span></span>

<span data-ttu-id="0d97d-119">Právě, protože aplikace zkompiluje, neznamená, že je úspěšně přesně do EF jádra.</span><span class="sxs-lookup"><span data-stu-id="0d97d-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="0d97d-120">Musíte se k testování všech oblastí aplikace k zajištění, že žádná ze změn chování negativní dopad vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d97d-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="0d97d-121">V tématu [Začínáme s EF základní na ASP.NET Core s existující databázi](xref:core/get-started/aspnetcore/existing-db) Další informace o tom, jak pracovat s existující databázi,</span><span class="sxs-lookup"><span data-stu-id="0d97d-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
