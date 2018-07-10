---
title: Kód první vložení, aktualizace a odstranění uložených procedur - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 6f8466601bedb705775b11e0b2732b1c4215aeac
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914468"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="9aee4-102">Kód první vložení, aktualizace a odstranění uložených procedur</span><span class="sxs-lookup"><span data-stu-id="9aee4-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="9aee4-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9aee4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9aee4-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="9aee4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="9aee4-105">Ve výchozím nastavení Code First nakonfiguruje všechny entity, které provést vložení, aktualizace a odstranění příkazů pomocí přímý přístup k tabulce.</span><span class="sxs-lookup"><span data-stu-id="9aee4-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="9aee4-106">Počínaje EF6, můžete nakonfigurovat váš model Code First použití uložené procedury pro některé nebo všechny entity v modelu.</span><span class="sxs-lookup"><span data-stu-id="9aee4-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="9aee4-107">Základní Entity mapování</span><span class="sxs-lookup"><span data-stu-id="9aee4-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="9aee4-108">Můžete přihlašují pomocí uložené procedury pro vložení, aktualizovat a odstranit pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="9aee4-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="9aee4-109">To způsobí, že Code First pomocí některé konvence můžete vytvářet očekávané tvar uložené procedury v databázi.</span><span class="sxs-lookup"><span data-stu-id="9aee4-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="9aee4-110">Tři uložené procedury s názvem  **\<type_name\>_komentářů**,  **\<type_name\>_aktualizovat** a  **\<type_ název\>_odstranit** (např. Blog_Insert, Blog_Update a Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="9aee4-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (e.g. Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="9aee4-111">Názvy parametrů odpovídají názvům vlastností.</span><span class="sxs-lookup"><span data-stu-id="9aee4-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="9aee4-112">Pokud používáte HasColumnName() nebo atribut sloupce přejmenujte sloupec pro danou vlastnost tento název se používá pro parametry místo názvu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="9aee4-113">**Uložená procedura insert** bude mít parametr pro každou vlastnost, kromě těch, které označené jako úložiště vygeneruje (identity nebo vypočítat).</span><span class="sxs-lookup"><span data-stu-id="9aee4-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="9aee4-114">Uloženou proceduru by měla vrátit sadu sloupec pro každou vlastnost úložiště vygeneruje výsledků.</span><span class="sxs-lookup"><span data-stu-id="9aee4-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="9aee4-115">**Aktualizace uložené procedury** bude mít parametr pro každou vlastnost, kromě těch, které označené úložiště vygeneruje vzor "Vypočítané".</span><span class="sxs-lookup"><span data-stu-id="9aee4-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="9aee4-116">Některé tokeny souběžnosti vyžadují parametr pro původní hodnoty, najdete v článku *tokeny souběžnosti* níže v části Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="9aee4-117">Uloženou proceduru by měla vrátit sadu výsledků obsahující sloupec pro každé počítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="9aee4-118">**Odstranit uloženou proceduru** by měl mít parametr pro hodnotu klíče entity (nebo více parametrů, pokud má entita složený klíč).</span><span class="sxs-lookup"><span data-stu-id="9aee4-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="9aee4-119">Kromě toho postup odstranění musí být také parametry pro jakékoli nezávislé přidružení cizího klíče v cílové tabulce (vztahy, které nemají odpovídající vlastnosti cizího klíče deklarované v entitě).</span><span class="sxs-lookup"><span data-stu-id="9aee4-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="9aee4-120">Některé tokeny souběžnosti vyžadují parametr pro původní hodnoty, najdete v článku *tokeny souběžnosti* níže v části Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="9aee4-121">Jako příklad použijeme následující třídy:</span><span class="sxs-lookup"><span data-stu-id="9aee4-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="9aee4-122">Výchozí nastavení, které bude uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="9aee4-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="9aee4-123">Přepíše výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="9aee4-123">Overriding the Defaults</span></span>  

<span data-ttu-id="9aee4-124">Část nebo všechny co bylo nakonfigurováno ve výchozím nastavení můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="9aee4-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="9aee4-125">Můžete změnit název jedné nebo více uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="9aee4-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="9aee4-126">Tento příklad přejmenuje uložená procedura update pouze.</span><span class="sxs-lookup"><span data-stu-id="9aee4-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="9aee4-127">Tento příklad přejmenuje všechny tři uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="9aee4-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="9aee4-128">V těchto příkladech jsou volání zřetězeno, ale můžete také použít syntaxi blok výrazu lambda.</span><span class="sxs-lookup"><span data-stu-id="9aee4-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="9aee4-129">Tento příklad přejmenuje parametrů pro vlastnost BlogId na uložená procedura update.</span><span class="sxs-lookup"><span data-stu-id="9aee4-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="9aee4-130">Tato volání jsou všechny chainable a možnosti složení.</span><span class="sxs-lookup"><span data-stu-id="9aee4-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="9aee4-131">Tady je příklad, který se přejmenuje všechny tři uložených procedur a jejich parametry.</span><span class="sxs-lookup"><span data-stu-id="9aee4-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="9aee4-132">Můžete také změnit název sloupce sady výsledků dotazu, který obsahuje databázi generované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9aee4-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="9aee4-133">Relace bez cizí klíče v třídě (nezávislé přidružení)</span><span class="sxs-lookup"><span data-stu-id="9aee4-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="9aee4-134">Vlastnost cizího klíče je obsažen v definici třídy, příslušného parametru lze přejmenovat stejně jako jakoukoli jinou vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="9aee4-135">Pokud mezi doménami existuje vztah bez vlastností cizího klíče ve třídě, je výchozí název parametru  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="9aee4-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="9aee4-136">Například následující definice tříd způsobí Blog_BlogId parametr se očekává v uložené procedury pro vkládání a aktualizace příspěvků.</span><span class="sxs-lookup"><span data-stu-id="9aee4-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="9aee4-137">Přepíše výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="9aee4-137">Overriding the Defaults</span></span>  

<span data-ttu-id="9aee4-138">Můžete změnit parametry pro cizí klíče, které nejsou zahrnuté ve třídě zadáním cesty pro vlastnost primárního klíče do parametru metody.</span><span class="sxs-lookup"><span data-stu-id="9aee4-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="9aee4-139">Pokud nemáte k dispozici vlastnost navigace u entity závislé (např.)</span><span class="sxs-lookup"><span data-stu-id="9aee4-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="9aee4-140">žádná vlastnost Post.Blog) můžete použít metodu přidružení identifikovat druhém konci vztahu a potom nakonfigurujte parametry, které odpovídají jednotlivým klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="9aee4-141">Tokeny souběžnosti</span><span class="sxs-lookup"><span data-stu-id="9aee4-141">Concurrency Tokens</span></span>  

<span data-ttu-id="9aee4-142">Update a delete uložené procedury vypořádat se souběžností také potřebovat:</span><span class="sxs-lookup"><span data-stu-id="9aee4-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="9aee4-143">Pokud entita obsahuje tokeny souběžnosti, uložené procedury můžou mít výstupní parametr, který vrací počet řádků, aktualizovat ani odstranit, (ovlivněných řádků).</span><span class="sxs-lookup"><span data-stu-id="9aee4-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="9aee4-144">Takový parametr musí být nakonfigurovaný pomocí metody RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="9aee4-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="9aee4-145">Ve výchozím nastavení používá EF návratovou hodnotu z metodu ExecuteNonQuery k určení, kolik řádků vliv.</span><span class="sxs-lookup"><span data-stu-id="9aee4-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="9aee4-146">Určení výstupní parametr ovlivněných řádků je užitečné, pokud provedete jakékoli logiky ve vašich sproc, výsledkem by byla návratová hodnota metodu ExecuteNonQuery byla zadána nesprávná (z hlediska na EF) na konci spuštění.</span><span class="sxs-lookup"><span data-stu-id="9aee4-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="9aee4-147">Pro každé souběžnosti existuje token bude parametr s názvem  **\<%{Property_Name/\>_Original** (tj. Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="9aee4-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (i.e. Timestamp_Original).</span></span> <span data-ttu-id="9aee4-148">Tím se předají původní hodnota této vlastnosti – hodnotu, pokud se dotaz z databáze.</span><span class="sxs-lookup"><span data-stu-id="9aee4-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="9aee4-149">Tokeny souběžnosti, které se vypočítávají v databázi – například časová razítka – bude mít jenom původní parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9aee4-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="9aee4-150">Bez vypočítané vlastnosti, které jsou nastaveny jako tokeny souběžnosti má také parametr nové hodnoty v procesu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9aee4-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="9aee4-151">Tato služba využívá zásady vytváření názvů pro nové hodnoty již probírali.</span><span class="sxs-lookup"><span data-stu-id="9aee4-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="9aee4-152">Příkladem takových token by pomocí adresy URL blogu jako tokenem souběžnosti, nová hodnota je povinné, protože to je možné aktualizovat na novou hodnotu podle kódu (na rozdíl od časové razítko token, který se aktualizují v databázi).</span><span class="sxs-lookup"><span data-stu-id="9aee4-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="9aee4-153">Toto je příklad třídy a aktualizovat uložené procedury s tokenem souběžnosti časové razítko.</span><span class="sxs-lookup"><span data-stu-id="9aee4-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="9aee4-154">Tady je příklad třídy a aktualizovat uložené procedury s tokenem neobsahující nepočítané souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="9aee4-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="9aee4-155">Přepíše výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="9aee4-155">Overriding the Defaults</span></span>  

<span data-ttu-id="9aee4-156">Volitelně můžete zavést parametr ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="9aee4-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="9aee4-157">Tokeny souběžnosti databáze vypočítané – kde pouze původní hodnota je předána – stačí vám pomůže standardní parametr přejmenování mechanismus přejmenovat parametr pro původní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9aee4-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="9aee4-158">Tokeny souběžnosti neobsahující nepočítané – kde i jejich původní a nové hodnoty se předávají – můžete použít přetížení parametru, který umožňuje zadat název pro každý parametr.</span><span class="sxs-lookup"><span data-stu-id="9aee4-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="9aee4-159">Mnoho k mnoha vztahů</span><span class="sxs-lookup"><span data-stu-id="9aee4-159">Many to Many Relationships</span></span>  

<span data-ttu-id="9aee4-160">Následující třídy použijeme jako příklad v této části.</span><span class="sxs-lookup"><span data-stu-id="9aee4-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="9aee4-161">Mnoho na mnoho vztahů lze mapovat na uložené procedury s následující syntaxí.</span><span class="sxs-lookup"><span data-stu-id="9aee4-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="9aee4-162">Pokud není zadána žádná další konfigurace se standardně používá následující tvar uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="9aee4-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="9aee4-163">Dvě uložené procedury s názvem  **\<type_one\>\<type_two\>_komentářů** a  **\<type_one\>\<type_two \>_Odstranit** (tj. PostTag_Insert a PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="9aee4-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (i.e. PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="9aee4-164">Parametry budou klíčové hodnoty, které pro každý typ.</span><span class="sxs-lookup"><span data-stu-id="9aee4-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="9aee4-165">Název každého parametru se **\<type_name\>_\<%{Property_Name/\>** (tj. Post_PostId a Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="9aee4-165">The name of each parameter being **\<type_name\>_\<property_name\>** (i.e. Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="9aee4-166">Tady je příklad vkládací a aktualizační uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="9aee4-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="9aee4-167">Přepíše výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="9aee4-167">Overriding the Defaults</span></span>  

<span data-ttu-id="9aee4-168">Název procedury a parametru lze nakonfigurovat podobným způsobem jako na entity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="9aee4-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
