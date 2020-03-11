---
title: Přenos z EF6 do EF Core-portový model založený na kódu – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419635"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="d4552-102">Přenos EF6 modelu založeného na kódu do EF Core</span><span class="sxs-lookup"><span data-stu-id="d4552-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="d4552-103">Pokud jste si přečetli všechna upozornění a jste připraveni na port, pak tady najdete pokyny, které vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="d4552-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="d4552-104">Nainstalovat EF Core balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="d4552-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="d4552-105">Chcete-li použít EF Core, nainstalujte balíček NuGet pro poskytovatele databáze, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="d4552-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="d4552-106">Například při cílení na SQL Server byste měli nainstalovat `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="d4552-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="d4552-107">Podrobnosti najdete v tématu [poskytovatelé databáze](../../core/providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="d4552-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="d4552-108">Pokud plánujete použít migrace, měli byste také nainstalovat balíček `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="d4552-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="d4552-109">Je také dobré opustit balíček NuGet EF6 (EntityFramework), protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d4552-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="d4552-110">Pokud ale nehodláte používat EF6 v žádné oblasti aplikace, pak se při odinstalaci balíčku pomůže Chyba kompilace v částech kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="d4552-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="d4552-111">Prohození oborů názvů</span><span class="sxs-lookup"><span data-stu-id="d4552-111">Swap namespaces</span></span>

<span data-ttu-id="d4552-112">Většina rozhraní API, která používáte v EF6, jsou v oboru názvů `System.Data.Entity` (a souvisejících dílčích oborech názvů).</span><span class="sxs-lookup"><span data-stu-id="d4552-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="d4552-113">První změnou kódu je přepnutí na obor názvů `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="d4552-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="d4552-114">Obvykle byste začínaly s vaším odvozeným souborem kódu kontextu a pak tam dostanou problémy, které řeší chyby kompilace.</span><span class="sxs-lookup"><span data-stu-id="d4552-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="d4552-115">Konfigurace kontextu (připojení atd.)</span><span class="sxs-lookup"><span data-stu-id="d4552-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="d4552-116">Jak je popsáno v tématu [zajistěte, aby pro vaši aplikaci fungovala EF Core](ensure-requirements.md), EF Core má méně Magic k detekci databáze, ke které se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="d4552-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="d4552-117">V odvozeném kontextu bude nutné přepsat metodu `OnConfiguring` a nastavit připojení k databázi pomocí rozhraní API pro konkrétního poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="d4552-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="d4552-118">Většina EF6ch aplikací ukládá připojovací řetězec do souboru aplikací `App/Web.config`.</span><span class="sxs-lookup"><span data-stu-id="d4552-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="d4552-119">V EF Core si tento připojovací řetězec načetli pomocí rozhraní `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="d4552-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="d4552-120">Možná budete muset přidat odkaz na sestavení rozhraní `System.Configuration` Framework, abyste mohli používat toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d4552-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="d4552-121">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="d4552-121">Update your code</span></span>

<span data-ttu-id="d4552-122">V tomto okamžiku se jedná o adresování chyb kompilace a revizi kódu, abyste viděli, zda vás ovlivní změny chování.</span><span class="sxs-lookup"><span data-stu-id="d4552-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="d4552-123">Existující migrace</span><span class="sxs-lookup"><span data-stu-id="d4552-123">Existing migrations</span></span>

<span data-ttu-id="d4552-124">Neexistuje žádný vhodný způsob, jak přenést existující migrace EF6 do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d4552-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="d4552-125">Je-li to možné, je vhodné předpokládat, že všechny předchozí migrace z EF6 byly použity v databázi, a pak začít migrovat schéma z tohoto bodu pomocí EF Core.</span><span class="sxs-lookup"><span data-stu-id="d4552-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="d4552-126">Chcete-li to provést, použijte příkaz `Add-Migration` pro přidání migrace, jakmile je model předaný do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d4552-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="d4552-127">Pak byste odebrali veškerý kód z `Up` a `Down` metod pro vygenerované migrace.</span><span class="sxs-lookup"><span data-stu-id="d4552-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="d4552-128">Další migrace se budou porovnávat s modelem, když se tato počáteční migrace vygenerovala z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d4552-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="d4552-129">Otestování portu</span><span class="sxs-lookup"><span data-stu-id="d4552-129">Test the port</span></span>

<span data-ttu-id="d4552-130">Vzhledem k tomu, že se vaše aplikace zkompiluje, neznamená to, že je úspěšně přepravovaná na EF Core.</span><span class="sxs-lookup"><span data-stu-id="d4552-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="d4552-131">Budete muset otestovat všechny oblasti aplikace, aby se zajistilo, že žádná z změn chování nemá negativně vliv na vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d4552-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
