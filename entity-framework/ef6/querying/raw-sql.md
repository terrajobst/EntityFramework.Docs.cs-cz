---
title: Nezpracované dotazy SQL – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417093"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="52ee4-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="52ee4-102">Raw SQL Queries</span></span>
<span data-ttu-id="52ee4-103">Entity Framework umožňuje dotazování pomocí LINQ s vašimi třídami entit.</span><span class="sxs-lookup"><span data-stu-id="52ee4-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="52ee4-104">Nicméně mohou nastat situace, kdy budete chtít spouštět dotazy pomocí nezpracovaného SQL přímo proti databázi.</span><span class="sxs-lookup"><span data-stu-id="52ee4-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="52ee4-105">To zahrnuje volání uložených procedur, které mohou být užitečné pro Code First modely, které aktuálně nepodporují mapování na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="52ee4-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="52ee4-106">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="52ee4-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="52ee4-107">Zápis dotazů SQL pro entity</span><span class="sxs-lookup"><span data-stu-id="52ee4-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="52ee4-108">Metoda SqlQuery v Negenerickými umožňuje napsat nezpracovaný dotaz SQL, který bude vracet instance entit.</span><span class="sxs-lookup"><span data-stu-id="52ee4-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="52ee4-109">Vrácené objekty budou sledovány kontextem stejně, jako by byly vráceny dotazem LINQ.</span><span class="sxs-lookup"><span data-stu-id="52ee4-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="52ee4-110">Příklad:</span><span class="sxs-lookup"><span data-stu-id="52ee4-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="52ee4-111">Všimněte si, že stejně jako u dotazů LINQ není dotaz proveden, dokud nejsou výčty výsledků – v předchozím příkladu je provedeno volání ToList –.</span><span class="sxs-lookup"><span data-stu-id="52ee4-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="52ee4-112">Při každém zápisu nezpracovaných dotazů SQL ze dvou důvodů by měla být provedena péče.</span><span class="sxs-lookup"><span data-stu-id="52ee4-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="52ee4-113">Nejprve by měl být vytvořen dotaz, aby bylo zajištěno, že bude vracet pouze entity, které jsou skutečně požadovaného typu.</span><span class="sxs-lookup"><span data-stu-id="52ee4-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="52ee4-114">Například při použití funkcí, jako je dědičnost, je snadné napsat dotaz, který vytvoří entity, které mají nesprávný typ CLR.</span><span class="sxs-lookup"><span data-stu-id="52ee4-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="52ee4-115">Za druhé, některé typy nezpracovaných dotazů SQL zveřejňují potenciální bezpečnostní rizika, zejména při útokech prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="52ee4-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="52ee4-116">Ujistěte se, že používáte parametry v dotazu správným způsobem ochrany proti takovým útokům.</span><span class="sxs-lookup"><span data-stu-id="52ee4-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="52ee4-117">Načítání entit z uložených procedur</span><span class="sxs-lookup"><span data-stu-id="52ee4-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="52ee4-118">Pomocí Negenerickými. SqlQuery můžete načíst entity z výsledků uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="52ee4-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="52ee4-119">Například následující kód volá dbo. Procedura getbloges v databázi:</span><span class="sxs-lookup"><span data-stu-id="52ee4-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="52ee4-120">Můžete také předat parametry uložené proceduře pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="52ee4-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="52ee4-121">Zápis dotazů SQL pro typy bez entit</span><span class="sxs-lookup"><span data-stu-id="52ee4-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="52ee4-122">Dotaz SQL vracející instance libovolného typu, včetně primitivních typů, lze vytvořit pomocí metody SqlQuery třídy Database.</span><span class="sxs-lookup"><span data-stu-id="52ee4-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="52ee4-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="52ee4-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="52ee4-124">Výsledky vrácené z SqlQuery v databázi nikdy nebudou sledovány kontextem, i když jsou objekty instance typu entity.</span><span class="sxs-lookup"><span data-stu-id="52ee4-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="52ee4-125">Posílání nezpracovaných příkazů do databáze</span><span class="sxs-lookup"><span data-stu-id="52ee4-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="52ee4-126">Příkazy jiného typu než dotaz lze odeslat do databáze pomocí metody ExecuteSqlCommand v databázi.</span><span class="sxs-lookup"><span data-stu-id="52ee4-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="52ee4-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="52ee4-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="52ee4-128">Všimněte si, že všechny změny provedené v datech v databázi pomocí ExecuteSqlCommand jsou neprůhledné do kontextu, dokud se entity nenačte nebo znovu načtou z databáze.</span><span class="sxs-lookup"><span data-stu-id="52ee4-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="52ee4-129">Výstupní parametry</span><span class="sxs-lookup"><span data-stu-id="52ee4-129">Output Parameters</span></span>  

<span data-ttu-id="52ee4-130">Pokud se používají výstupní parametry, jejich hodnoty nebudou k dispozici, dokud nebudou výsledky zcela načteny.</span><span class="sxs-lookup"><span data-stu-id="52ee4-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="52ee4-131">Důvodem je základní chování DbDataReader. Další informace najdete v tématu [načtení dat pomocí objektu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .</span><span class="sxs-lookup"><span data-stu-id="52ee4-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
