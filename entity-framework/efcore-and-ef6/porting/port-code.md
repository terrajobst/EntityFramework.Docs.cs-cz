---
title: Přenos z EF6 na ef core – portování modelu založeného na kódu – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419635"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="88ff9-102">Přenesení modelu ef6 založeného na kódu na základní EF</span><span class="sxs-lookup"><span data-stu-id="88ff9-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="88ff9-103">Pokud jste si přečetli všechny upozornění a jste připraveni k portu, pak zde je několik pokynů, které vám pomohou začít.</span><span class="sxs-lookup"><span data-stu-id="88ff9-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="88ff9-104">Instalace balíčků EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="88ff9-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="88ff9-105">Chcete-li použít EF Core, nainstalujete balíček NuGet pro poskytovatele databáze, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="88ff9-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="88ff9-106">Například při cílení sql server, `Microsoft.EntityFrameworkCore.SqlServer`byste nainstalovat .</span><span class="sxs-lookup"><span data-stu-id="88ff9-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="88ff9-107">Podrobnosti najdete v [tématu Zprostředkovatelé databáze.](../../core/providers/index.md)</span><span class="sxs-lookup"><span data-stu-id="88ff9-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="88ff9-108">Pokud plánujete používat migrace, měli byste také `Microsoft.EntityFrameworkCore.Tools` nainstalovat balíček.</span><span class="sxs-lookup"><span data-stu-id="88ff9-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="88ff9-109">Je v pořádku ponechat balíček EF6 NuGet (EntityFramework) nainstalovaný, protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="88ff9-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="88ff9-110">Pokud však nemáte v úmyslu používat EF6 v žádné oblasti aplikace, pak odinstalace balíčku pomůže poskytnout chyby kompilace na části kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="88ff9-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="88ff9-111">Zaměnit obory názvů</span><span class="sxs-lookup"><span data-stu-id="88ff9-111">Swap namespaces</span></span>

<span data-ttu-id="88ff9-112">Většina api, které používáte v `System.Data.Entity` EF6 jsou v oboru názvů (a související podobory).</span><span class="sxs-lookup"><span data-stu-id="88ff9-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="88ff9-113">První změna kódu je přepnutí do oboru `Microsoft.EntityFrameworkCore` názvů.</span><span class="sxs-lookup"><span data-stu-id="88ff9-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="88ff9-114">Obvykle byste začít s odvozené matné kód souboru a pak pracovat z toho, adresování chybkompilace, jak k nim dochází.</span><span class="sxs-lookup"><span data-stu-id="88ff9-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="88ff9-115">Kontextová konfigurace (připojení atd.)</span><span class="sxs-lookup"><span data-stu-id="88ff9-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="88ff9-116">Jak je popsáno v [části Ujistěte se, že EF Core bude fungovat pro vaši aplikaci](ensure-requirements.md), EF Core má méně magie kolem zjišťování databáze pro připojení.</span><span class="sxs-lookup"><span data-stu-id="88ff9-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="88ff9-117">Budete muset přepsat metodu v `OnConfiguring` odvozeném kontextu a použít rozhraní API specifické pro poskytovatele databáze k nastavení připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="88ff9-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="88ff9-118">Většina aplikací EF6 ukládá připojovací řetězec do souboru aplikace. `App/Web.config`</span><span class="sxs-lookup"><span data-stu-id="88ff9-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="88ff9-119">V EF Core, načtete `ConfigurationManager` tento připojovací řetězec pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="88ff9-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="88ff9-120">Možná budete muset přidat odkaz `System.Configuration` na sestavení rozhraní, abyste mohli používat toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="88ff9-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="88ff9-121">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="88ff9-121">Update your code</span></span>

<span data-ttu-id="88ff9-122">V tomto okamžiku je to otázka adresování chyb kompilace a revize kódu, abyste zjistili, zda změny chování ovlivní vás.</span><span class="sxs-lookup"><span data-stu-id="88ff9-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="88ff9-123">Stávající migrace</span><span class="sxs-lookup"><span data-stu-id="88ff9-123">Existing migrations</span></span>

<span data-ttu-id="88ff9-124">Neexistuje žádný skutečně proveditelný způsob, jak portovat stávající ef6 migrace ef6 na EF Core.</span><span class="sxs-lookup"><span data-stu-id="88ff9-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="88ff9-125">Pokud je to možné, je nejlepší předpokládat, že všechny předchozí migrace z EF6 byly použity do databáze a potom začít migraci schématu z tohoto bodu pomocí EF Core.</span><span class="sxs-lookup"><span data-stu-id="88ff9-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="88ff9-126">Chcete-li to provést, `Add-Migration` pomocí příkazu přidat migraci, jakmile je model portován na EF Core.</span><span class="sxs-lookup"><span data-stu-id="88ff9-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="88ff9-127">Potom by odebrat veškerý `Up` `Down` kód z a metody generování šavle migrace.</span><span class="sxs-lookup"><span data-stu-id="88ff9-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="88ff9-128">Následné migrace budou porovnány s modelem, když byla počáteční migrace přeložena.</span><span class="sxs-lookup"><span data-stu-id="88ff9-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="88ff9-129">Testování portu</span><span class="sxs-lookup"><span data-stu-id="88ff9-129">Test the port</span></span>

<span data-ttu-id="88ff9-130">Jen proto, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenesena na EF Core.</span><span class="sxs-lookup"><span data-stu-id="88ff9-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="88ff9-131">Budete muset otestovat všechny oblasti aplikace, abyste zajistili, že žádná ze změn chování nemá nepříznivý vliv na vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="88ff9-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
