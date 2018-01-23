---
title: "Načítají se související Data – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="5d407-102">Načítání související Data</span><span class="sxs-lookup"><span data-stu-id="5d407-102">Loading Related Data</span></span>

<span data-ttu-id="5d407-103">Entity Framework Core umožňuje používat navigační vlastnosti v modelu se načíst související entity.</span><span class="sxs-lookup"><span data-stu-id="5d407-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="5d407-104">Existují tři obecné vzory O/RM používají k zatížení související data.</span><span class="sxs-lookup"><span data-stu-id="5d407-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="5d407-105">**Přes načítání** znamená, že související data načtená z databáze jako součást počáteční dotazu.</span><span class="sxs-lookup"><span data-stu-id="5d407-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="5d407-106">**Explicitní načítání** znamená, že související data se explicitně načíst z databáze později.</span><span class="sxs-lookup"><span data-stu-id="5d407-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="5d407-107">**Opožděného načítání** znamená, že související transparentně načtení dat z databáze při přístupu k navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5d407-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="5d407-108">Opožděného načítání ještě není možné s EF jádra.</span><span class="sxs-lookup"><span data-stu-id="5d407-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="5d407-109">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5d407-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="5d407-110">přes načítání</span><span class="sxs-lookup"><span data-stu-id="5d407-110">Eager loading</span></span>

<span data-ttu-id="5d407-111">Můžete použít `Include` metoda k určení související data mají být zahrnuty do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="5d407-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="5d407-112">V následujícím příkladu, bude mít blogy, které jsou vráceny ve výsledcích jejich `Posts` vlastnost naplněný související příspěvky.</span><span class="sxs-lookup"><span data-stu-id="5d407-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="5d407-113">Entity Framework Core bude automaticky opravu up navigačních vlastností pro ostatní entity, které byly dříve načteny do instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="5d407-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="5d407-114">Takže i v případě, že není výslovně zahrnout data pro navigační vlastnost, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.</span><span class="sxs-lookup"><span data-stu-id="5d407-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="5d407-115">Související data z více vztahů můžete zahrnout jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="5d407-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="5d407-116">Více úrovní</span><span class="sxs-lookup"><span data-stu-id="5d407-116">Including multiple levels</span></span>

<span data-ttu-id="5d407-117">Podrobnostem přispívají vztahů s zahrnují více úrovní související data pomocí `ThenInclude` metoda.</span><span class="sxs-lookup"><span data-stu-id="5d407-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="5d407-118">Následující příklad načte všechny blogy, jejich související příspěvky a Autor každé post.</span><span class="sxs-lookup"><span data-stu-id="5d407-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="5d407-119">Aktuální verze sady Visual Studio nabízí možnosti dokončení nesprávný kód a mohou způsobit správné výrazy označen příznakem chyby syntaxe při použití `ThenInclude` metoda po navigační vlastnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="5d407-119">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="5d407-120">Toto je příznakem IntelliSense chyb sledovány v https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="5d407-120">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="5d407-121">Je bezpečné tyto chyby syntaxe nesprávné ignorovat, dokud je správný kód a mohou být zkompilovány úspěšně.</span><span class="sxs-lookup"><span data-stu-id="5d407-121">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="5d407-122">Můžete zřetězit více volání `ThenInclude` Chcete-li pokračovat, včetně další úrovně související data.</span><span class="sxs-lookup"><span data-stu-id="5d407-122">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="5d407-123">Můžete sloučit všechny tohoto zahrnout související data z více úrovní a více kořenových certifikačních autorit ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="5d407-123">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="5d407-124">Můžete zahrnout více entit v relaci pro jednu z entity, které bude zahrnut.</span><span class="sxs-lookup"><span data-stu-id="5d407-124">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="5d407-125">Například při dotazování `Blog`s, zahrnete `Posts` a chcete současně obsahovat `Author` a `Tags` z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="5d407-125">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="5d407-126">K tomuto účelu, budete muset zadat jednotlivé obsahovat cestu spouštění v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="5d407-126">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="5d407-127">Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="5d407-127">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="5d407-128">To neznamená, že budete mít redundantní spojení, ve většině případů, které budou konsolidovat EF spojení při generování SQL.</span><span class="sxs-lookup"><span data-stu-id="5d407-128">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="5d407-129">Ignorovat zahrnuje</span><span class="sxs-lookup"><span data-stu-id="5d407-129">Ignored includes</span></span>

<span data-ttu-id="5d407-130">Pokud změníte dotaz tak, že už vrátí instance typu entity, které začne dotaz s, se ignorují operátory zahrnout.</span><span class="sxs-lookup"><span data-stu-id="5d407-130">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="5d407-131">V následujícím příkladu jsou na základě operátory zahrnout `Blog`, ale pak `Select` operátor se používá ke změně dotaz vrátit instanci anonymního typu.</span><span class="sxs-lookup"><span data-stu-id="5d407-131">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="5d407-132">V takovém případě operátory zahrnout nemají žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="5d407-132">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="5d407-133">Ve výchozím nastavení, bude EF základní zaprotokolovat upozornění při zahrnují operátory jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="5d407-133">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="5d407-134">V tématu [protokolování](../miscellaneous/logging.md) Další informace o zobrazení výstupu protokolování.</span><span class="sxs-lookup"><span data-stu-id="5d407-134">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="5d407-135">Chování lze změnit, pokud zahrnout operátor je ignorován throw nebo nic nestane.</span><span class="sxs-lookup"><span data-stu-id="5d407-135">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="5d407-136">To se provádí při nastavování možnosti pro váš kontext – obvykle ve `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d407-136">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="5d407-137">explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="5d407-137">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="5d407-138">Tato funkce byla zavedená v EF základní 1.1.</span><span class="sxs-lookup"><span data-stu-id="5d407-138">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="5d407-139">Můžete explicitně načíst navigační vlastnost prostřednictvím `DbContext.Entry(...)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5d407-139">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="5d407-140">Můžete také explicitně načíst navigační vlastnost spuštěním další dotaz, který vrací související entity.</span><span class="sxs-lookup"><span data-stu-id="5d407-140">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="5d407-141">Pokud je povoleno sledování změn, pak při načítání entitu, EF základní bude automaticky nastavit navigační vlastnosti nově načíst entitiy odkazovat na všechny entity již načten a vlastnosti navigace entit již načten k odkazování na nově načíst entity.</span><span class="sxs-lookup"><span data-stu-id="5d407-141">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="5d407-142">Dotazování entit v relaci</span><span class="sxs-lookup"><span data-stu-id="5d407-142">Querying related entities</span></span>

<span data-ttu-id="5d407-143">Můžete také získat LINQ dotazu, který reprezentuje obsah navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5d407-143">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="5d407-144">To umožňuje provádět akce, jako je například spuštění agregační operátor přes související entity bez jejich načtení do paměti.</span><span class="sxs-lookup"><span data-stu-id="5d407-144">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="5d407-145">Můžete také filtrovat, které entit v relaci jsou načtena do paměti.</span><span class="sxs-lookup"><span data-stu-id="5d407-145">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="5d407-146">opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="5d407-146">Lazy loading</span></span>

<span data-ttu-id="5d407-147">Opožděného načítání ještě nepodporuje EF jádra.</span><span class="sxs-lookup"><span data-stu-id="5d407-147">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="5d407-148">Můžete zobrazit [opožděného načítání položky na našem nevyřízených položek](https://github.com/aspnet/EntityFramework/issues/3797) sledovat tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5d407-148">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="5d407-149">Související data a serializace</span><span class="sxs-lookup"><span data-stu-id="5d407-149">Related data and serialization</span></span>

<span data-ttu-id="5d407-150">Protože základní EF bude automaticky opravu up navigační vlastnosti, můžete skončili s cykly v grafu objektu.</span><span class="sxs-lookup"><span data-stu-id="5d407-150">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="5d407-151">Například načítání blog a nesouvisí způsobí příspěvcích na blogu objekt, který odkazuje na kolekci zpráv.</span><span class="sxs-lookup"><span data-stu-id="5d407-151">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="5d407-152">Každý z těchto příspěvcích bude mít odkaz na blogu.</span><span class="sxs-lookup"><span data-stu-id="5d407-152">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="5d407-153">Některé architektury serializace neumožňují takové cykly.</span><span class="sxs-lookup"><span data-stu-id="5d407-153">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="5d407-154">Například Json.NET vyvolá následující výjimky, pokud je encoutered cyklus.</span><span class="sxs-lookup"><span data-stu-id="5d407-154">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="5d407-155">Newtonsoft.Json.JsonSerializationException: Vlastní odkazující na smyčky zjištěna vlastnost 'Blog' typu 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="5d407-155">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="5d407-156">Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="5d407-156">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="5d407-157">To se provádí v `ConfigureServices(...)` metoda v `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="5d407-157">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
