---
title: Nezpracované dotazy SQL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283781"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="ef1f4-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="ef1f4-102">Raw SQL Queries</span></span>
<span data-ttu-id="ef1f4-103">Entity Framework umožňuje dotazování pomocí jazyka LINQ pomocí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="ef1f4-104">Mohou však nastat situace, které chcete spouštět dotazy s využitím nezpracovaná SQL přímo na databázi.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="ef1f4-105">To zahrnuje volání uložené procedury, které mohou být užitečné pro Code First modely, které se aktuálně nepodporují mapování na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="ef1f4-106">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="ef1f4-107">Psaní dotazů SQL pro entity</span><span class="sxs-lookup"><span data-stu-id="ef1f4-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="ef1f4-108">Metoda SqlQuery na DbSet umožňuje neupraveného dotazu SQL má být proveden zápis, který vrátí instance entity.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="ef1f4-109">Vrácených objektů bude sledovat podle kontextu, stejně jako by se použily, pokud byly vrácené dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="ef1f4-110">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ef1f4-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="ef1f4-111">Všimněte si, že stejně jako u dotazů LINQ dotaz není udělat, dokud jsou uvedené výsledky – v předchozím příkladu to provádí pomocí volání ToList.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="ef1f4-112">Pokaždé, když se nezpracované dotazy SQL jsou určeny pro dva důvody měli věnovat pozornost.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="ef1f4-113">Nejprve by měly být napsány dotaz k zajištění, že vrátí jenom entity, které jsou v zásadě požadovaného typu.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="ef1f4-114">Například při použití funkce, jako je dědičnost je snadné napsat dotaz, který bude vytvářet entity, které jsou chybného typu CLR.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="ef1f4-115">Za druhé některé typy neupraveného dotazu SQL vystavit potenciální ohrožení zabezpečení, zejména útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="ef1f4-116">Ujistěte se, že správným způsobem pro ochranu před těmito útoky použít parametry v dotazu.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="ef1f4-117">Načítání entit z uložených procedur</span><span class="sxs-lookup"><span data-stu-id="ef1f4-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="ef1f4-118">DbSet.SqlQuery můžete použít k načtení entity z výsledků uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="ef1f4-119">Například následující kód volá vlastníka. GetBlogs procedury v databázi:</span><span class="sxs-lookup"><span data-stu-id="ef1f4-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="ef1f4-120">Můžete také předat parametry uložené procedury pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="ef1f4-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="ef1f4-121">Psaní dotazů SQL pro typy bez entit</span><span class="sxs-lookup"><span data-stu-id="ef1f4-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="ef1f4-122">Jazyka SQL vracející instance libovolného typu, včetně primitivní typy, je možné vytvořit pomocí metody SqlQuery ve třídě databáze.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="ef1f4-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ef1f4-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="ef1f4-124">Výsledky vrácené SqlQuery v databázi se nikdy sledovat pomocí kontextu i v případě, objekty jsou instancemi typu entity.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="ef1f4-125">Odesílání nezpracovaná příkazů do databáze</span><span class="sxs-lookup"><span data-stu-id="ef1f4-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="ef1f4-126">Příkazy bez dotazu je odeslat do databáze pomocí metody ExecuteSqlCommand v databázi.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="ef1f4-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="ef1f4-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="ef1f4-128">Všimněte si, že jsou všechny změny dat v databázi pomocí ExecuteSqlCommand neprůhledné kontextu dokud entity jsou načteny nebo znovu načíst z databáze.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="ef1f4-129">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="ef1f4-129">Output Parameters</span></span>  

<span data-ttu-id="ef1f4-130">Pokud se používají výstupních parametrů, jejich hodnoty nebudou dostupné, dokud nebude mít zcela načíst výsledky.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="ef1f4-131">Je to z důvodu základní chování DbDataReader naleznete v tématu [načítání dat pomocí čtečky dat](https://go.microsoft.com/fwlink/?LinkID=398589) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ef1f4-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
