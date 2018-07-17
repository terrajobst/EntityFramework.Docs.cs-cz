---
title: Definování DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067490"
---
# <a name="defining-dbsets"></a><span data-ttu-id="8e3ce-102">Definování DbSets</span><span class="sxs-lookup"><span data-stu-id="8e3ce-102">Defining DbSets</span></span>
<span data-ttu-id="8e3ce-103">Při vývoji se kód prvního pracovního postupu můžete definovat odvozené DbContext, který reprezentuje relaci s databází a zveřejňuje DbSet pro každý typ v modelu.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="8e3ce-104">Toto téma popisuje různé způsoby, jak můžete definovat vlastnosti DbSet.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="8e3ce-105">Kontext databáze s vlastnostmi DbSet</span><span class="sxs-lookup"><span data-stu-id="8e3ce-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="8e3ce-106">Běžné Code First příkladů je DbContext s veřejné vlastnosti automatické DbSet pro typy entity z modelu.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="8e3ce-107">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8e3ce-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="8e3ce-108">Při použití v režimu Code First, bude to jako typy entit, jakož i jiné typy, které jsou dostupné z těchto konfigurací konfigurace blogů a příspěvky.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="8e3ce-109">Kromě toho DbContext automaticky zavolá metodu setter pro každou z těchto vlastností lze nastavit instanci odpovídající DbSet.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="8e3ce-110">Kontext databáze s vlastnostmi IDbSet</span><span class="sxs-lookup"><span data-stu-id="8e3ce-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="8e3ce-111">Existují situace, jako je například při vytváření mocks nebo napodobeniny, kde je další užitečné k deklaraci vlastnosti sady pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="8e3ce-112">V takových případech IDbSet rozhraní může být zastoupen DbSet.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="8e3ce-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8e3ce-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="8e3ce-114">Tento kontext funguje přesně stejným způsobem jako kontext, který se používá třídu DbSet sady vlastností.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="8e3ce-115">Nastavit vlastnosti DbContext s jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="8e3ce-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="8e3ce-116">Pokud nechcete, aby k vystavení veřejné metody setter pro DbSet nebo IDbSet vlastností můžete místo toho vytvořit vlastnosti jen pro čtení a vytvoření instancí sady sami.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="8e3ce-117">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8e3ce-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="8e3ce-118">Všimněte si, že DbContext ukládá do mezipaměti instance DbSet vrátil z metody Set tak, aby každá z těchto vlastností vrátí stejnou instanci pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="8e3ce-119">Zjišťování typů entit pro Code First lze použít stejně jako jako to udělá za vlastnosti veřejné metody getter a setter.</span><span class="sxs-lookup"><span data-stu-id="8e3ce-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
