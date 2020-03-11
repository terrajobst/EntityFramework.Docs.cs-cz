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
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code First uložených procedur vložení, aktualizace a odstranění
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Ve výchozím nastavení Code First nastaví všechny entity, aby prováděly příkazy INSERT, Update a Delete pomocí přímého přístupu k tabulce. Počínaje EF6 můžete nakonfigurovat model Code First tak, aby používal uložené procedury pro některé nebo všechny entity v modelu.  

## <a name="basic-entity-mapping"></a>Mapování základních entit  

Můžete se rozhodnout použít uložené procedury pro vkládání, aktualizaci a odstraňování pomocí rozhraní Fluent API.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

To způsobí, že Code First použít některé konvence k sestavení očekávaného tvaru uložených procedur v databázi.  

- Tři uložené procedury s názvem **\<type_name\>_Insert** **\<** type_name\>_Update a **\<** TYPE_NAME\>_Delete (například Blog_Insert Blog_Update Blog_Delete a).  
- Názvy parametrů odpovídají názvům vlastností.  
  > [!NOTE]
  > Použijete-li HasColumnName () nebo atribut Column k přejmenování sloupce pro danou vlastnost, bude tento název použit pro parametry namísto názvu vlastnosti.  
- **Uložená procedura INSERT** bude mít parametr pro každou vlastnost, s výjimkou těch, které jsou označeny jako vygenerované úložištěm (identita nebo vypočítáno). Uložená procedura by měla vrátit sadu výsledků dotazu se sloupcem pro každou generovanou vlastnost úložiště.  
- **Uložená procedura aktualizace** bude mít parametr pro každou vlastnost s výjimkou těch, které jsou označené vzorem generovaným úložištěm "počítaného". Některé tokeny souběžnosti vyžadují parametr pro původní hodnotu. Podrobnosti najdete níže v části *tokeny souběžnosti* . Uložená procedura by měla vrátit sadu výsledků dotazu se sloupcem pro každou vypočítanou vlastnost.  
- **Uložená procedura pro odstranění** by měla mít parametr pro hodnotu klíče entity (nebo více parametrů, pokud má entita složený klíč). Kromě toho by měl mít procedura Delete také parametry pro jakékoli nezávislé cizí klíče přidružení v cílové tabulce (relace, které nemají odpovídající vlastnosti cizího klíče deklarované v entitě). Některé tokeny souběžnosti vyžadují parametr pro původní hodnotu. Podrobnosti najdete níže v části *tokeny souběžnosti* .  

Jako příklad použijte následující třídu:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Výchozí uložené procedury by byly:  

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

### <a name="overriding-the-defaults"></a>Přepsání výchozích hodnot  

Můžete přepsat část nebo vše, co bylo ve výchozím nastavení nakonfigurováno.  

Můžete změnit název jednoho nebo více uložených procedur. Tento příklad přejmenuje pouze uloženou proceduru aktualizace.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Tento příklad přejmenuje všechny tři uložené procedury.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

V těchto příkladech jsou volání zřetězena, ale můžete také použít syntaxi bloku lambda.  

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

Tento příklad přejmenuje parametr pro vlastnost BlogId v uložené proceduře aktualizace.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Tato volání jsou všechna zřetězená a sestavitelná. Tady je příklad, který přejmenuje všechny tři uložené procedury a jejich parametry.  

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

Můžete také změnit název sloupců v sadě výsledků dotazu, která obsahuje hodnoty generované databází.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relace bez cizího klíče ve třídě (nezávislá přidružení)  

Pokud je v definici třídy obsažena vlastnost cizího klíče, odpovídající parametr lze přejmenovat stejným způsobem jako jakoukoli jinou vlastnost. Pokud existuje relace bez vlastnosti cizího klíče ve třídě, je výchozí název parametru **\<navigation_property_name\>_\<primary_key_name\>** .  

Například následující definice tříd budou mít za následek, že v uložených procedurách pro vložení a aktualizaci příspěvků bude očekáván parametr Blog_BlogId.  

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

### <a name="overriding-the-defaults"></a>Přepsání výchozích hodnot  

Můžete změnit parametry pro cizí klíče, které nejsou zahrnuty ve třídě, zadáním cesty k vlastnosti primárního klíče metodě parametru.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Pokud v závislé entitě nemáte navigační vlastnost (tj. vlastnost post. blog není k dispozici, a poté můžete použít metodu Association k identifikaci druhého konce relace a potom nakonfigurovat parametry, které odpovídají jednotlivým klíčovým vlastnostem.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokeny souběžnosti  

Aktualizace a odstraňování uložených procedur může být také potřeba řešit s concurrency:  

- Pokud entita obsahuje tokeny souběžnosti, uložená procedura může volitelně mít výstupní parametr, který vrátí počet aktualizovaných nebo odstraněných řádků (ovlivněné řádky). Takový parametr musí být konfigurován pomocí metody RowsAffectedParameter.  
Ve výchozím nastavení EF používá návratovou hodnotu z ExecuteNonQuery k určení, kolik řádků bylo ovlivněno. Zadání výstupního parametru ovlivněného řádky je užitečné, pokud v sproc provedete jakoukoli logiku, která by způsobila, že vrácená hodnota ExecuteNonQuery není správná (od perspektivy EF) na konci provádění.  
- Pro každý token souběžnosti se použije parametr s názvem **\<property_name\>_Original** (například Timestamp_Original). Tím se předává původní hodnota této vlastnosti – hodnota při dotazování z databáze.  
    - Tokeny souběžnosti, které jsou vypočítány databází – například časová razítka – budou mít pouze parametr původní hodnoty.  
    - Nepočítané vlastnosti, které jsou nastaveny jako tokeny souběžnosti, budou mít také parametr pro novou hodnotu v postupu aktualizace. Používá konvence pojmenování, které už jsou popsané pro nové hodnoty. Příkladem takového tokenu je použití adresy URL blogu jako tokenu souběžnosti. nová hodnota je povinná, protože může být aktualizována na novou hodnotu vaším kódem (na rozdíl od tokenu časového razítka, který je aktualizován pouze databází).  

Toto je ukázková třída a aktualizuje uloženou proceduru s tokenem Concurrency pro časové razítko.  

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

Tady je příklad třídy a aktualizace uložené procedury s nepočítaným tokenem souběžnosti.  

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

### <a name="overriding-the-defaults"></a>Přepsání výchozích hodnot  

Volitelně můžete zavést parametr ovlivněných řádků.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Pro vypočítané tokeny souběžnosti databáze – kde se předává jenom původní hodnota – k přejmenování parametru původní hodnoty můžete použít jenom standardní mechanismus Přejmenování parametru.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Pro nepočítané tokeny souběžnosti – kde jsou předány původní i nová hodnota – můžete použít přetížení parametru, které umožňuje zadat název pro každý parametr.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Mnoho k mnoha vztahů  

Následující třídy budeme používat jako příklad v této části.  

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

K uloženým procedurám lze pomocí následující syntaxe namapovat mnoho vztahů na mnoho.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Pokud se nezadá žádná další konfigurace, použije se ve výchozím nastavení následující obrazec uložené procedury.  

- Dva uložené procedury s názvem **\<type_one\>\<type_two\>_Insert** a **\<type_one\>\<type_two\>_Delete** (například PostTag_Insert a PostTag_Delete).  
- Parametry budou klíčovou hodnotou pro každý typ. Název každého parametru, který se má **\<type_name\>_\<property_name\>** (například Post_PostId a Tag_TagId).

Tady je příklad vložení a aktualizace uložených procedur.  

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

### <a name="overriding-the-defaults"></a>Přepsání výchozích hodnot  

Proceduru a názvy parametrů lze nakonfigurovat podobným způsobem jako uložené procedury entit.  

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
