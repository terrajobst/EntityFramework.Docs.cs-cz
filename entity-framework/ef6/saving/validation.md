---
title: Ověřování - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 65639b0f91f54ee2cd1336f6b6cd4caf45ede680
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251021"
---
# <a name="data-validation"></a>Ověřování dat
> [!NOTE]
> **EF4.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 4.1. Pokud používáte starší verzi, některé nebo všechny informace neplatí

Obsah na této stránce jsou upraveny z a článek původně vydané společností Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework obsahuje velké množství ověření funkcí, které můžou informační kanál prostřednictvím uživatelského rozhraní pro ověřování na straně klienta nebo použít pro ověřování na straně serveru. Při prvním použití kódu, můžete zadat ověření pomocí poznámky nebo fluent API konfigurace. Další ověření a složitější, dá se zadat v kódu a bude fungovat, jestli váš model má za sebou kód nejprve model nejprve nebo databáze nejprve.

## <a name="the-model"></a>Model

Vám předvedu ověření pomocí jednoduchého páru tříd: Blog a příspěvku.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a>Datové poznámky

Poznámky ze sestavení System.ComponentModel.DataAnnotations kód nejprve používá jako jeden prostředek konfigurace kód první třídy. Mezi tyto poznámky jsou ty, které poskytují pravidla, jako jsou požadované, MinLength a MaxLength. Počet klientských aplikací .NET také rozpoznává tyto anotace, například technologie ASP.NET MVC. Můžete dosáhnout i na straně a serverem ověřování na straně klienta pomocí těchto poznámek. Například můžete vynutit název blogu vlastnost jako povinnou vlastnost.

``` csharp
    [Required]
    public string Title { get; set; }
```

Žádný další kód nebo změny kódu v aplikaci existující aplikaci MVC provede ověřování na straně klienta, dokonce i dynamické vytvoření zprávu pomocí názvy vlastností a poznámek.

![Obrázek 1](~/ef6/media/figure01.png)

V příspěvku back – metoda tohoto vytvořit zobrazení, Entity Framework se používá pro uložení nového blogu k databázi, ale ověřování na straně klienta MVC se aktivuje před aplikace dosáhne tento kód.

Ověřování na straně klienta však není odrážky testování. Uživatelé můžou ovlivnit funkce jejich prohlížeče nebo horší ještě, se hacker použít některé podvodů, aby ověření uživatelského rozhraní. Ale také rozpozná požadované poznámky a ověřte ho Entity Framework.

Zakázat funkci ověřování na straně klienta pro MVC je jednoduchý způsob, jak to otestovat. Můžete to provést v souboru web.config aplikace MVC. Sekci appSettings má klíč pro ClientValidationEnabled. Nastavení tohoto klíče na hodnotu false zabrání uživatelském rozhraní provádí ověření.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

I při ověřování na straně klienta, zakázán obdržíte stejnou odpověď ve vaší aplikaci. Chybová zpráva "název pole je povinné" se zobrazí jako. S tím rozdílem, teď bude výsledek ověření na straně serveru. Entity Framework provede ověření na požadované poznámky (před bothers i k sestavení a příkaz INSERT pro odeslání do databáze) a vrátí chybu MVC, která se zobrazí zpráva.

## <a name="fluent-api"></a>Rozhraní Fluent API

Kód můžete použít pro první rozhraní API fluent místo poznámky zobrazíte stejného klienta na straně & serveru straně ověření. Místo použití jsou povinné, ukážeme vám pomocí ověření MaxLength.

Konfigurace Fluent API jsou použity v kódu nejprve se sestavení modelu z tříd. Konfigurace můžete vložit tak, že přepíšete metodu OnModelCreating třídy DbContext. Tady je konfigurace určující, zda vlastnost BloggerName nesmí být delší než 10 znaků.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

Chyby ověření vyvolána konfigurace rozhraní Fluent API nebude automaticky dosah uživatelského rozhraní, ale můžete je i zachytávat v kódu a pak odpověď na ni odpovídajícím způsobem.

Tady je kód chyby v třídě BlogController vaší aplikace, která zachycuje tuto chybu ověření, když se pokusí uložit blog s BloggerName, která překračuje maximální počet 10 znaků Entity Framework pro zpracování některé výjimky.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

Ověření, nebude se automaticky předat zpět do zobrazení, což je důvod, proč se používá další kód, který používá ModelState.AddModelError. Tím se zajistí, že se podrobnosti o chybě vytvořit zobrazení, která se pak použije ValidationMessageFor Htmlhelper k zobrazení chyby.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject je rozhraní, které se nachází v System.ComponentModel.DataAnnotations. I když není součástí rozhraní API Entity Framework, můžete stále využít ji pro ověřování na straně serveru ve třídách rozhraní Entity Framework. IValidatableObject poskytuje ověřit metodu, která bude volat rozhraní Entity Framework během SaveChanges, nebo můžete volat sami kdykoli chcete ověřit třídy.

Konfigurace, jako požadované a MaxLength proveďte validaton na jedno pole. V metodě ověření můžete mít i složitější logiku, například porovnání dvou polí.

V následujícím příkladu bylo rozšířeno blogu třídy k implementaci IValidatableObject a pak zadejte pravidlo, které neodpovídá názvu a BloggerName.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

Konstruktor ValidationResult přebírá řetězec představující chybovou zprávu a pole řetězců, které představují názvy členů, které jsou spojeny s ověření. Protože toto ověření kontroluje název i BloggerName, vrátí se oba názvy vlastností.

Na rozdíl od ověřování poskytované rozhraní Fluent API tento výsledek ověření bude rozpoznán zobrazením a jsem použili dříve k přidání do ModelState chyba obslužné rutiny výjimky je zbytečné. Protože jsem nastavil ValidationResult oba názvy vlastností, MVC HtmlHelpers zobrazí chybová zpráva pro obě tyto vlastnosti.

![Obrázek 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

Kontext databáze má přepisovatelnou metodu volá ValidateEntity. Při volání SaveChanges, Entity Framework tuto metodu volat pro každou entitu v mezipaměti, jejichž stav není beze změny. Můžete vložit logiku ověřování přímo v zde nebo dokonce použít tuto metodu za účelem volání, například metoda Blog.Validate přidali v předchozí části.

Tady je příklad ValidateEntity přepsání, která ověřuje nové příspěvky k zajištění, že nadpis příspěvku nebylo se už používá. Nejprve zkontroluje podívejte se, jestli je entita příspěvek a přidaná svůj stav. Pokud je to tento případ, pak vypadá v databázi pro zobrazení, pokud je již příspěvek se stejným názvem. Pokud již existující příspěvek, se vytvoří nová DbEntityValidationResult.

DbEntityValidationResult jsou uloženy DbEntityEntry a rozhraní ICollection DbValidationErrors jedné entity. Na začátku této metody DbEntityValidationResult je vytvořena instance a potom se přidají všechny chyby, které jsou zjištěny do jeho ValidationErrors kolekce.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>Aktivuje se explicitně ověření

Volání SaveChanges aktivuje všechny ověřovací popsaná v tomto článku. Ale nemusíte spoléhat na SaveChanges. Můžete chtít ověřit jinde v aplikaci.

DbContext.GetValidationErrors aktivují všech ověření, těmi definovanými ve poznámky nebo rozhraní Fluent API, ověření vytvořené v IValidatableObject (například Blog.Validate) a ověření provádět v DbContext.ValidateEntity Metoda.

Následující kód zavolá GetValidationErrors na aktuální instancí třídy DbContext. ValidationErrors jsou seskupené podle typu entity do DbValidationRestuls. Kód prochází nejprve prostřednictvím DbValidationResults vrácený metodou a potom každý ValidationError uvnitř.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>Další informace týkající se použití ověřování

Tady je několik věcí, které je třeba zvážit při použití rozhraní Entity Framework ověření:

-   Opožděné načítání je zakázáno během ověřování.
-   EF ověří anotacemi dat na jiných mapované vlastnosti (vlastnosti, které nejsou namapované na sloupec v databázi).
-   Po zjištění změny během SaveChanges, provede se ověření. Pokud provedete změny během ověřování je vaší odpovědností, abyste upozornit modul sledování změny.
-   DbUnexpectedValidationException je vyvolána, pokud dojde k chybám při ověřování.
-   Omezující vlastnosti, které zahrnuje Entity Framework v modelu (maximální délka povolená, atd.) způsobí ověření, i když již nejsou anotacemi dat ve vašich třídách a/nebo jste použili k vytvoření modelu EF designeru.
-   Priorita pravidla:
    -   Přepsat odpovídající anotacemi dat Fluent volání rozhraní API
-   Pořadí spuštění:
    -   Ověření vlastností předchází ověření typu
    -   Ověření typu dochází pouze v případě úspěšného ověření vlastnost
-   Pokud je komplexní vlastnost bude obsahovat také jeho ověření:
    -   Vlastnost úroveň ověření na komplexní typ vlastnosti
    -   Zadejte úroveň ověření na komplexní typ, včetně ověření IValidatableObject na komplexní typ

## <a name="summary"></a>Souhrn

Ověřování rozhraní API v Entity Framework hraje velmi krásně pomocí ověřování na straně klienta v MVC, ale nemusíte spoléhat na ověřování na straně klienta. Entity Framework se postará o ověřování na straně serveru pro DataAnnotations nebo konfigurace, které jste použili s první Fluent API kódu.

Také jste zjistili několik bodů rozšiřitelnosti pro přizpůsobení chování, ať už pomocí rozhraní IValidatableObject nebo využijte metodu DbContet.ValidateEntity. A tyto poslední dva způsoby ověření jsou k dispozici prostřednictvím uvolněn objekt DbContext, ať už používáte pracovní postup Code First, Model první nebo Database First k popisu koncepčního modelu.
