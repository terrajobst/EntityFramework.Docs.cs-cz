---
title: Code First uložených procedur vložení, aktualizace a odstranění – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419085"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="23a99-102">Code First uložených procedur vložení, aktualizace a odstranění</span><span class="sxs-lookup"><span data-stu-id="23a99-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="23a99-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="23a99-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="23a99-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="23a99-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="23a99-105">Ve výchozím nastavení Code First nastaví všechny entity, aby prováděly příkazy INSERT, Update a Delete pomocí přímého přístupu k tabulce.</span><span class="sxs-lookup"><span data-stu-id="23a99-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="23a99-106">Počínaje EF6 můžete nakonfigurovat model Code First tak, aby používal uložené procedury pro některé nebo všechny entity v modelu.</span><span class="sxs-lookup"><span data-stu-id="23a99-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="23a99-107">Mapování základních entit</span><span class="sxs-lookup"><span data-stu-id="23a99-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="23a99-108">Můžete se rozhodnout použít uložené procedury pro vkládání, aktualizaci a odstraňování pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="23a99-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="23a99-109">To způsobí, že Code First použít některé konvence k sestavení očekávaného tvaru uložených procedur v databázi.</span><span class="sxs-lookup"><span data-stu-id="23a99-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="23a99-110">Tři uložené procedury s názvem **\<type_name\>_Insert** **\<** type_name\>_Update a **\<** TYPE_NAME\>_Delete (například Blog_Insert Blog_Update Blog_Delete a).</span><span class="sxs-lookup"><span data-stu-id="23a99-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="23a99-111">Názvy parametrů odpovídají názvům vlastností.</span><span class="sxs-lookup"><span data-stu-id="23a99-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="23a99-112">Použijete-li HasColumnName () nebo atribut Column k přejmenování sloupce pro danou vlastnost, bude tento název použit pro parametry namísto názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="23a99-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="23a99-113">**Uložená procedura INSERT** bude mít parametr pro každou vlastnost, s výjimkou těch, které jsou označeny jako vygenerované úložištěm (identita nebo vypočítáno).</span><span class="sxs-lookup"><span data-stu-id="23a99-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="23a99-114">Uložená procedura by měla vrátit sadu výsledků dotazu se sloupcem pro každou generovanou vlastnost úložiště.</span><span class="sxs-lookup"><span data-stu-id="23a99-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="23a99-115">**Uložená procedura aktualizace** bude mít parametr pro každou vlastnost s výjimkou těch, které jsou označené vzorem generovaným úložištěm "počítaného".</span><span class="sxs-lookup"><span data-stu-id="23a99-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="23a99-116">Některé tokeny souběžnosti vyžadují parametr pro původní hodnotu. Podrobnosti najdete níže v části *tokeny souběžnosti* .</span><span class="sxs-lookup"><span data-stu-id="23a99-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="23a99-117">Uložená procedura by měla vrátit sadu výsledků dotazu se sloupcem pro každou vypočítanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23a99-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="23a99-118">**Uložená procedura pro odstranění** by měla mít parametr pro hodnotu klíče entity (nebo více parametrů, pokud má entita složený klíč).</span><span class="sxs-lookup"><span data-stu-id="23a99-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="23a99-119">Kromě toho by měl mít procedura Delete také parametry pro jakékoli nezávislé cizí klíče přidružení v cílové tabulce (relace, které nemají odpovídající vlastnosti cizího klíče deklarované v entitě).</span><span class="sxs-lookup"><span data-stu-id="23a99-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="23a99-120">Některé tokeny souběžnosti vyžadují parametr pro původní hodnotu. Podrobnosti najdete níže v části *tokeny souběžnosti* .</span><span class="sxs-lookup"><span data-stu-id="23a99-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="23a99-121">Jako příklad použijte následující třídu:</span><span class="sxs-lookup"><span data-stu-id="23a99-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="23a99-122">Výchozí uložené procedury by byly:</span><span class="sxs-lookup"><span data-stu-id="23a99-122">The default stored procedures would be:</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="23a99-123">Přepsání výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="23a99-123">Overriding the Defaults</span></span>  

<span data-ttu-id="23a99-124">Můžete přepsat část nebo vše, co bylo ve výchozím nastavení nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="23a99-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="23a99-125">Můžete změnit název jednoho nebo více uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="23a99-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="23a99-126">Tento příklad přejmenuje pouze uloženou proceduru aktualizace.</span><span class="sxs-lookup"><span data-stu-id="23a99-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="23a99-127">Tento příklad přejmenuje všechny tři uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="23a99-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="23a99-128">V těchto příkladech jsou volání zřetězena, ale můžete také použít syntaxi bloku lambda.</span><span class="sxs-lookup"><span data-stu-id="23a99-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

<span data-ttu-id="23a99-129">Tento příklad přejmenuje parametr pro vlastnost BlogId v uložené proceduře aktualizace.</span><span class="sxs-lookup"><span data-stu-id="23a99-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="23a99-130">Tato volání jsou všechna zřetězená a sestavitelná.</span><span class="sxs-lookup"><span data-stu-id="23a99-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="23a99-131">Tady je příklad, který přejmenuje všechny tři uložené procedury a jejich parametry.</span><span class="sxs-lookup"><span data-stu-id="23a99-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

<span data-ttu-id="23a99-132">Můžete také změnit název sloupců v sadě výsledků dotazu, která obsahuje hodnoty generované databází.</span><span class="sxs-lookup"><span data-stu-id="23a99-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="23a99-133">Relace bez cizího klíče ve třídě (nezávislá přidružení)</span><span class="sxs-lookup"><span data-stu-id="23a99-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="23a99-134">Pokud je v definici třídy obsažena vlastnost cizího klíče, odpovídající parametr lze přejmenovat stejným způsobem jako jakoukoli jinou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23a99-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="23a99-135">Pokud existuje relace bez vlastnosti cizího klíče ve třídě, je výchozí název parametru **\<navigation_property_name\>_\<primary_key_name\>** .</span><span class="sxs-lookup"><span data-stu-id="23a99-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="23a99-136">Například následující definice tříd budou mít za následek, že v uložených procedurách pro vložení a aktualizaci příspěvků bude očekáván parametr Blog_BlogId.</span><span class="sxs-lookup"><span data-stu-id="23a99-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="23a99-137">Přepsání výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="23a99-137">Overriding the Defaults</span></span>  

<span data-ttu-id="23a99-138">Můžete změnit parametry pro cizí klíče, které nejsou zahrnuty ve třídě, zadáním cesty k vlastnosti primárního klíče metodě parametru.</span><span class="sxs-lookup"><span data-stu-id="23a99-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="23a99-139">Pokud v závislé entitě nemáte navigační vlastnost (tj.</span><span class="sxs-lookup"><span data-stu-id="23a99-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="23a99-140">vlastnost post. blog není k dispozici, a poté můžete použít metodu Association k identifikaci druhého konce relace a potom nakonfigurovat parametry, které odpovídají jednotlivým klíčovým vlastnostem.</span><span class="sxs-lookup"><span data-stu-id="23a99-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="23a99-141">Tokeny souběžnosti</span><span class="sxs-lookup"><span data-stu-id="23a99-141">Concurrency Tokens</span></span>  

<span data-ttu-id="23a99-142">Aktualizace a odstraňování uložených procedur může být také potřeba řešit s concurrency:</span><span class="sxs-lookup"><span data-stu-id="23a99-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="23a99-143">Pokud entita obsahuje tokeny souběžnosti, uložená procedura může volitelně mít výstupní parametr, který vrátí počet aktualizovaných nebo odstraněných řádků (ovlivněné řádky).</span><span class="sxs-lookup"><span data-stu-id="23a99-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="23a99-144">Takový parametr musí být konfigurován pomocí metody RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="23a99-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="23a99-145">Ve výchozím nastavení EF používá návratovou hodnotu z ExecuteNonQuery k určení, kolik řádků bylo ovlivněno.</span><span class="sxs-lookup"><span data-stu-id="23a99-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="23a99-146">Zadání výstupního parametru ovlivněného řádky je užitečné, pokud v sproc provedete jakoukoli logiku, která by způsobila, že vrácená hodnota ExecuteNonQuery není správná (od perspektivy EF) na konci provádění.</span><span class="sxs-lookup"><span data-stu-id="23a99-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="23a99-147">Pro každý token souběžnosti se použije parametr s názvem **\<property_name\>_Original** (například Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="23a99-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="23a99-148">Tím se předává původní hodnota této vlastnosti – hodnota při dotazování z databáze.</span><span class="sxs-lookup"><span data-stu-id="23a99-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="23a99-149">Tokeny souběžnosti, které jsou vypočítány databází – například časová razítka – budou mít pouze parametr původní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="23a99-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="23a99-150">Nepočítané vlastnosti, které jsou nastaveny jako tokeny souběžnosti, budou mít také parametr pro novou hodnotu v postupu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="23a99-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="23a99-151">Používá konvence pojmenování, které už jsou popsané pro nové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="23a99-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="23a99-152">Příkladem takového tokenu je použití adresy URL blogu jako tokenu souběžnosti. nová hodnota je povinná, protože může být aktualizována na novou hodnotu vaším kódem (na rozdíl od tokenu časového razítka, který je aktualizován pouze databází).</span><span class="sxs-lookup"><span data-stu-id="23a99-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="23a99-153">Toto je ukázková třída a aktualizuje uloženou proceduru s tokenem Concurrency pro časové razítko.</span><span class="sxs-lookup"><span data-stu-id="23a99-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

<span data-ttu-id="23a99-154">Tady je příklad třídy a aktualizace uložené procedury s nepočítaným tokenem souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="23a99-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="23a99-155">Přepsání výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="23a99-155">Overriding the Defaults</span></span>  

<span data-ttu-id="23a99-156">Volitelně můžete zavést parametr ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="23a99-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="23a99-157">Pro vypočítané tokeny souběžnosti databáze – kde se předává jenom původní hodnota – k přejmenování parametru původní hodnoty můžete použít jenom standardní mechanismus Přejmenování parametru.</span><span class="sxs-lookup"><span data-stu-id="23a99-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="23a99-158">Pro nepočítané tokeny souběžnosti – kde jsou předány původní i nová hodnota – můžete použít přetížení parametru, které umožňuje zadat název pro každý parametr.</span><span class="sxs-lookup"><span data-stu-id="23a99-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="23a99-159">Mnoho k mnoha vztahů</span><span class="sxs-lookup"><span data-stu-id="23a99-159">Many to Many Relationships</span></span>  

<span data-ttu-id="23a99-160">Následující třídy budeme používat jako příklad v této části.</span><span class="sxs-lookup"><span data-stu-id="23a99-160">We’ll use the following classes as an example in this section.</span></span>  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="23a99-161">K uloženým procedurám lze pomocí následující syntaxe namapovat mnoho vztahů na mnoho.</span><span class="sxs-lookup"><span data-stu-id="23a99-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="23a99-162">Pokud se nezadá žádná další konfigurace, použije se ve výchozím nastavení následující obrazec uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="23a99-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="23a99-163">Dva uložené procedury s názvem **\<type_one\>\<type_two\>_Insert** a **\<type_one\>\<type_two\>_Delete** (například PostTag_Insert a PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="23a99-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="23a99-164">Parametry budou klíčovou hodnotou pro každý typ.</span><span class="sxs-lookup"><span data-stu-id="23a99-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="23a99-165">Název každého parametru, který se má **\<type_name\>_\<property_name\>** (například Post_PostId a Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="23a99-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="23a99-166">Tady je příklad vložení a aktualizace uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="23a99-166">Here are example insert and update stored procedures.</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="23a99-167">Přepsání výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="23a99-167">Overriding the Defaults</span></span>  

<span data-ttu-id="23a99-168">Proceduru a názvy parametrů lze nakonfigurovat podobným způsobem jako uložené procedury entit.</span><span class="sxs-lookup"><span data-stu-id="23a99-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
