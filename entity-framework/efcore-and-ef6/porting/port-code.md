---
title: Portování ze EF6 na jádro EF – portování Model založené na kódu
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054283"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="88e0e-102">Portování Model založené na kódu EF6 na jádro EF</span><span class="sxs-lookup"><span data-stu-id="88e0e-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="88e0e-103">Pokud jste si přečetli všechna upozornění a jste připraveni k portu, potom tady jsou některé pokyny, které vám pomůže začít.</span><span class="sxs-lookup"><span data-stu-id="88e0e-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="88e0e-104">Instalace balíčků NuGet EF jádra</span><span class="sxs-lookup"><span data-stu-id="88e0e-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="88e0e-105">Pokud chcete použít EF jádra, nainstalujte balíček NuGet pro zprostředkovatele databáze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="88e0e-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="88e0e-106">Například pokud je cílem systém SQL Server, by instalaci `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="88e0e-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="88e0e-107">V tématu [zprostředkovatelů databáze](../../core/providers/index.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="88e0e-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="88e0e-108">Pokud máte v úmyslu použít migrace, pak musíte taky nainstalovat `Microsoft.EntityFrameworkCore.Tools` balíčku.</span><span class="sxs-lookup"><span data-stu-id="88e0e-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="88e0e-109">Je v pořádku nechte balíček EF6 NuGet (EntityFramework) nainstalované, tak, jak EF jádra a EF6 můžou být použité-souběžného ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="88e0e-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="88e0e-110">Ale pokud nejsou hodláte použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže poskytnout chyby při kompilaci na části kódu, které vyžadují pozornost.</span><span class="sxs-lookup"><span data-stu-id="88e0e-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="88e0e-111">Swap – obory názvů</span><span class="sxs-lookup"><span data-stu-id="88e0e-111">Swap namespaces</span></span>

<span data-ttu-id="88e0e-112">Většina rozhraní API, které používáte ve EF6 jsou v `System.Data.Entity` obor názvů (a související dílčí-obory názvů).</span><span class="sxs-lookup"><span data-stu-id="88e0e-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="88e0e-113">První změna kódu je do odkládacího souboru `Microsoft.EntityFrameworkCore` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="88e0e-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="88e0e-114">By obvykle začínat souboru kódu odvozené kontextu a pak vycházejí z ní, vyřešte chyby kompilace, kdy k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="88e0e-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="88e0e-115">Kontext konfigurace (připojení atd.)</span><span class="sxs-lookup"><span data-stu-id="88e0e-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="88e0e-116">Jak je popsáno v [zkontrolujte EF základní bude práci pro vaše aplikace](ensure-requirements.md), EF základní má méně magic kolem zjišťování pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="88e0e-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="88e0e-117">Je nutné přepsat `OnConfiguring` metoda v odvozené kontextu a použití rozhraní API konkrétní zprostředkovatele databáze k nastavení připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="88e0e-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="88e0e-118">Většina aplikací EF6 připojovací řetězec uložit v aplikacích `App/Web.config` souboru.</span><span class="sxs-lookup"><span data-stu-id="88e0e-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="88e0e-119">V EF Core, číst, tento připojovací řetězec pomocí `ConfigurationManager` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="88e0e-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="88e0e-120">Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní, abyste mohli používat toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="88e0e-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="88e0e-121">Aktualizace kódu</span><span class="sxs-lookup"><span data-stu-id="88e0e-121">Update your code</span></span>

<span data-ttu-id="88e0e-122">V tomto okamžiku je řádu adresování chyby při kompilaci a kontrola kódu, které chcete zobrazit, pokud změny chování ovlivní můžete.</span><span class="sxs-lookup"><span data-stu-id="88e0e-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="88e0e-123">Existující migrace</span><span class="sxs-lookup"><span data-stu-id="88e0e-123">Existing migrations</span></span>

<span data-ttu-id="88e0e-124">Ve skutečnosti není k dispozici vhodný způsob k portu existující EF6 migrací EF jádra.</span><span class="sxs-lookup"><span data-stu-id="88e0e-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="88e0e-125">Pokud je to možné je nejvhodnější předpokládají, že byly použity všechny předchozí migrace z EF6 k databázi a potom spustit migraci schématu z tohoto bodu pomocí EF jádra.</span><span class="sxs-lookup"><span data-stu-id="88e0e-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="88e0e-126">Chcete-li to provést, použijte `Add-Migration` příkaz pro přidání migrace po modelu je přesně do EF jádra.</span><span class="sxs-lookup"><span data-stu-id="88e0e-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="88e0e-127">By pak odeberte všechny kód z `Up` a `Down` metody vygenerované migrace.</span><span class="sxs-lookup"><span data-stu-id="88e0e-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="88e0e-128">Následné migrace se porovná do modelu, při které počáteční migrace se vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="88e0e-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="88e0e-129">Testování port</span><span class="sxs-lookup"><span data-stu-id="88e0e-129">Test the port</span></span>

<span data-ttu-id="88e0e-130">Právě, protože aplikace zkompiluje, neznamená, že je úspěšně přesně do EF jádra.</span><span class="sxs-lookup"><span data-stu-id="88e0e-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="88e0e-131">Musíte se k testování všech oblastí aplikace k zajištění, že žádná ze změn chování negativní dopad vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="88e0e-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
