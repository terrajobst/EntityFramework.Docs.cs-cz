---
title: Kód první vložení, aktualizace a odstranění uložených procedur - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489619"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Kód první vložení, aktualizace a odstranění uložených procedur
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Ve výchozím nastavení Code First nakonfiguruje všechny entity, které provést vložení, aktualizace a odstranění příkazů pomocí přímý přístup k tabulce. Počínaje EF6, můžete nakonfigurovat váš model Code First použití uložené procedury pro některé nebo všechny entity v modelu.  

## <a name="basic-entity-mapping"></a>Základní Entity mapování  

Můžete přihlašují pomocí uložené procedury pro vložení, aktualizovat a odstranit pomocí rozhraní Fluent API.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

To způsobí, že Code First pomocí některé konvence můžete vytvářet očekávané tvar uložené procedury v databázi.  

- Tři uložené procedury s názvem  **\<type_name\>_komentářů**,  **\<type_name\>_aktualizovat** a  **\<type_ název\>_odstranit** (například Blog_Insert Blog_Update a Blog_Delete).  
- Názvy parametrů odpovídají názvům vlastností.  
  > [!NOTE]
  > Pokud používáte HasColumnName() nebo atribut sloupce přejmenujte sloupec pro danou vlastnost tento název se používá pro parametry místo názvu vlastnosti.  
- **Uložená procedura insert** bude mít parametr pro každou vlastnost, kromě těch, které označené jako úložiště vygeneruje (identity nebo vypočítat). Uloženou proceduru by měla vrátit sadu sloupec pro každou vlastnost úložiště vygeneruje výsledků.  
- **Aktualizace uložené procedury** bude mít parametr pro každou vlastnost, kromě těch, které označené úložiště vygeneruje vzor "Vypočítané". Některé tokeny souběžnosti vyžadují parametr pro původní hodnoty, najdete v článku *tokeny souběžnosti* níže v části Podrobnosti. Uloženou proceduru by měla vrátit sadu výsledků obsahující sloupec pro každé počítané vlastnosti.  
- **Odstranit uloženou proceduru** by měl mít parametr pro hodnotu klíče entity (nebo více parametrů, pokud má entita složený klíč). Kromě toho postup odstranění musí být také parametry pro jakékoli nezávislé přidružení cizího klíče v cílové tabulce (vztahy, které nemají odpovídající vlastnosti cizího klíče deklarované v entitě). Některé tokeny souběžnosti vyžadují parametr pro původní hodnoty, najdete v článku *tokeny souběžnosti* níže v části Podrobnosti.  

Jako příklad použijeme následující třídy:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Výchozí nastavení, které bude uložené procedury:  

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

### <a name="overriding-the-defaults"></a>Přepíše výchozí hodnoty  

Část nebo všechny co bylo nakonfigurováno ve výchozím nastavení můžete přepsat.  

Můžete změnit název jedné nebo více uložených procedur. Tento příklad přejmenuje uložená procedura update pouze.  

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

V těchto příkladech jsou volání zřetězeno, ale můžete také použít syntaxi blok výrazu lambda.  

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

Tento příklad přejmenuje parametrů pro vlastnost BlogId na uložená procedura update.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Tato volání jsou všechny chainable a možnosti složení. Tady je příklad, který se přejmenuje všechny tři uložených procedur a jejich parametry.  

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

Můžete také změnit název sloupce sady výsledků dotazu, který obsahuje databázi generované hodnoty.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relace bez cizí klíče v třídě (nezávislé přidružení)  

Vlastnost cizího klíče je obsažen v definici třídy, příslušného parametru lze přejmenovat stejně jako jakoukoli jinou vlastnosti. Pokud mezi doménami existuje vztah bez vlastností cizího klíče ve třídě, je výchozí název parametru  **\<navigation_property_name\>_\<primary_key_name\>**.  

Například následující definice tříd způsobí Blog_BlogId parametr se očekává v uložené procedury pro vkládání a aktualizace příspěvků.  

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

### <a name="overriding-the-defaults"></a>Přepíše výchozí hodnoty  

Můžete změnit parametry pro cizí klíče, které nejsou zahrnuté ve třídě zadáním cesty pro vlastnost primárního klíče do parametru metody.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Pokud nemáte k dispozici vlastnost navigace u entity závislé (např.) žádná vlastnost Post.Blog) můžete použít metodu přidružení identifikovat druhém konci vztahu a potom nakonfigurujte parametry, které odpovídají jednotlivým klíčové vlastnosti.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokeny souběžnosti  

Update a delete uložené procedury vypořádat se souběžností také potřebovat:  

- Pokud entita obsahuje tokeny souběžnosti, uložené procedury můžou mít výstupní parametr, který vrací počet řádků, aktualizovat ani odstranit, (ovlivněných řádků). Takový parametr musí být nakonfigurovaný pomocí metody RowsAffectedParameter.  
Ve výchozím nastavení používá EF návratovou hodnotu z metodu ExecuteNonQuery k určení, kolik řádků vliv. Určení výstupní parametr ovlivněných řádků je užitečné, pokud provedete jakékoli logiky ve vašich sproc, výsledkem by byla návratová hodnota metodu ExecuteNonQuery byla zadána nesprávná (z hlediska na EF) na konci spuštění.  
- Pro každé souběžnosti existuje token bude parametr s názvem  **\<%{Property_Name/\>_Original** (například Timestamp_Original). Tím se předají původní hodnota této vlastnosti – hodnotu, pokud se dotaz z databáze.  
    - Tokeny souběžnosti, které se vypočítávají v databázi – například časová razítka – bude mít jenom původní parametr hodnoty.  
    - Bez vypočítané vlastnosti, které jsou nastaveny jako tokeny souběžnosti má také parametr nové hodnoty v procesu aktualizace. Tato služba využívá zásady vytváření názvů pro nové hodnoty již probírali. Příkladem takových token by pomocí adresy URL blogu jako tokenem souběžnosti, nová hodnota je povinné, protože to je možné aktualizovat na novou hodnotu podle kódu (na rozdíl od časové razítko token, který se aktualizují v databázi).  

Toto je příklad třídy a aktualizovat uložené procedury s tokenem souběžnosti časové razítko.  

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

Tady je příklad třídy a aktualizovat uložené procedury s tokenem neobsahující nepočítané souběžnosti.  

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

### <a name="overriding-the-defaults"></a>Přepíše výchozí hodnoty  

Volitelně můžete zavést parametr ovlivněných řádků.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Tokeny souběžnosti databáze vypočítané – kde pouze původní hodnota je předána – stačí vám pomůže standardní parametr přejmenování mechanismus přejmenovat parametr pro původní hodnotu.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Tokeny souběžnosti neobsahující nepočítané – kde i jejich původní a nové hodnoty se předávají – můžete použít přetížení parametru, který umožňuje zadat název pro každý parametr.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Mnoho k mnoha vztahů  

Následující třídy použijeme jako příklad v této části.  

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

Mnoho na mnoho vztahů lze mapovat na uložené procedury s následující syntaxí.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Pokud není zadána žádná další konfigurace se standardně používá následující tvar uloženou proceduru.  

- Dvě uložené procedury s názvem  **\<type_one\>\<type_two\>_komentářů** a  **\<type_one\>\<type_two \>_Odstranit** (například PostTag_Insert a PostTag_Delete).  
- Parametry budou klíčové hodnoty, které pro každý typ. Název každého parametru se **\<type_name\>_\<%{Property_Name/\>** (například Post_PostId a Tag_TagId).

Tady je příklad vkládací a aktualizační uložené procedury.  

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

### <a name="overriding-the-defaults"></a>Přepíše výchozí hodnoty  

Název procedury a parametru lze nakonfigurovat podobným způsobem jako na entity uložené procedury.  

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
