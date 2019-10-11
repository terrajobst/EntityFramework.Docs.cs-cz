---
title: Ověření – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 4162c2eb60109459c799da7cf4c1a9c8e84548b6
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182134"
---
# <a name="data-validation"></a>Ověřování dat
> [!NOTE]
> **EF 4.1 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 4,1. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Obsah na této stránce je upravený podle článku, který původně napsal Julie Lerman ([https://thedatafarm.com](http://thedatafarm.com)).

Entity Framework poskytuje skvělou škálu funkcí ověřování, které se můžou dodávat do uživatelského rozhraní pro ověřování na straně klienta nebo používat při ověřování na straně serveru. Při prvním použití kódu můžete zadat ověření pomocí konfigurace anotace nebo Fluent rozhraní API. Další ověření a složitější, mohou být specifikovány v kódu a budou fungovat bez ohledu na to, zda model Susan z kódu jako první, model First nebo Database First.

## <a name="the-model"></a>Model

Ukážeme ověřování pomocí jednoduché dvojice tříd: Blog a příspěvek.

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

Code First používá poznámky z sestavení `System.ComponentModel.DataAnnotations` jako jeden způsob konfigurace třídy Code First. Mezi tyto poznámky patří ta, která poskytují pravidla, například `Required`, `MaxLength` a `MinLength`. Řada klientských aplikací .NET také rozeznává tyto poznámky, například ASP.NET MVC. Pomocí těchto poznámek můžete dosáhnout ověřování na straně klienta i na straně serveru. Například můžete vynutit, aby vlastnost nadpisu blogu byla požadovaná vlastnost.

``` csharp
[Required]
public string Title { get; set; }
```

Bez jakýchkoli dalších změn kódu nebo kódu v aplikaci bude existující aplikace MVC provádět ověřování na straně klienta, a to i dynamicky sestavovat zprávu pomocí názvů vlastností a poznámek.

![Obrázek 1](~/ef6/media/figure01.png)

V metodě post back tohoto zobrazení pro vytváření Entity Framework slouží k uložení nového blogu do databáze, ale ověřování na straně klienta MVC se aktivuje předtím, než aplikace dosáhne tohoto kódu.

Ověřování na straně klienta není ale odrážka. Uživatelé můžou mít vliv na funkce prohlížeče nebo horšího dopadu, hacker může použít určitou podmnožinu, aby nedocházelo k ověřování uživatelského rozhraní. Ale Entity Framework taky rozpozná anotaci `Required` a ověří ji.

Jedním z možností, jak tuto možnost otestovat, je zakázat funkci ověřování na straně klienta MVC. To můžete provést v souboru Web. config aplikace MVC. Oddíl appSettings obsahuje klíč pro ClientValidationEnabled. Nastavením tohoto klíče na false zabráníte uživatelskému rozhraní v provádění ověření.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

I v případě, že je ověřování na straně klienta zakázané, získáte stejnou odpověď v aplikaci. Chybová zpráva "pole název je povinné" se zobrazí jako dříve. S výjimkou této chvíle bude výsledkem ověřování na straně serveru. Entity Framework provede ověření v poznámce `Required` (předtím, než oba výrobci vytvoří příkaz `INSERT` pro odeslání do databáze) a vrátí chybu do MVC, která zobrazí zprávu.

## <a name="fluent-api"></a>Rozhraní Fluent API

K získání stejné klientské strany & ověřování na straně serveru můžete použít rozhraní Fluent API pro první použití kódu, nikoli poznámky. Místo použití `Required` ukážeme to pomocí ověření MaxLength.

Konfigurace Fluent rozhraní API se aplikují jako první kód sestavuje model z tříd. Můžete vložit konfigurace přepsáním metody OnModelCreating třídy DbContext. Tady je konfigurace, která určuje, že vlastnost blogger nemůže být delší než 10 znaků.

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

Chyby ověřování vyvolané pomocí konfigurací Fluent rozhraní API nebudou automaticky přicházet k uživatelskému rozhraní, ale můžete je zachytit v kódu a následně na něj reagovat.

Tady je několik chybových kódů zpracování výjimek ve třídě BlogController aplikace, která zachycuje chybu ověřování při Entity Framework se pokusí uložit blog s vlastností blogger, který překračuje maximum 10 znaků.

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

Ověřování se automaticky nevrátí zpět do zobrazení, což znamená, proč se používá další kód, který používá `ModelState.AddModelError`. Tím se zajistí, že se podrobnosti o chybě změní na zobrazení, které pak použije `ValidationMessageFor` HtmlHelper k zobrazení chyby.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject` je rozhraní, které žije v `System.ComponentModel.DataAnnotations`. I když není součástí rozhraní Entity Framework API, můžete ho i nadále využít pro ověřování na straně serveru ve vašich Entity Frameworkch třídách. `IValidatableObject` poskytuje metodu `Validate`, kterou Entity Framework volá během metody SaveChanges, nebo můžete zavolat sami kdykoli, když chcete třídy ověřit.

Konfigurace, jako je například `Required` a `MaxLength`, provádějí ověřování v jednom poli. V metodě `Validate` můžete mít ještě složitější logiku, například porovnávání dvou polí.

V následujícím příkladu byla třída `Blog` rozšířena tak, aby implementovala `IValidatableObject` a pak poskytovala pravidlo, že `Title` a `BloggerName` se nesmí shodovat.

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

Konstruktor `ValidationResult` přebírá chybu `string`, která představuje chybovou zprávu, a pole `string`S, které představují názvy členů přidružené k ověření. Vzhledem k tomu, že toto ověření kontroluje `Title` i `BloggerName`, budou vráceny oba názvy vlastností.

Na rozdíl od ověření, které poskytuje rozhraní Fluent API, tento výsledek ověření rozpozná zobrazení a obslužná rutina výjimky, kterou jsme použili dříve k přidání chyby do `ModelState` je zbytečné. Vzhledem k tomu, že se v `ValidationResult` nastavují názvy obou vlastností, zobrazení MVC HtmlHelpers zobrazí chybovou zprávu pro obě tyto vlastnosti.

![Obrázek 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

`DbContext` má přepsatelné metody nazvané `ValidateEntity`. Když zavoláte `SaveChanges`, Entity Framework tuto metodu zavolá pro každou entitu v mezipaměti, jejíž stav není `Unchanged`. Logiku ověřování můžete umístit přímo sem nebo dokonce použít k volání metody, například `Blog.Validate` přidané v předchozí části.

Tady je příklad přepsaného `ValidateEntity`, který ověřuje nové `Post`, aby se zajistilo, že se název příspěvku už nepoužívá. Nejprve zkontroluje, zda je entita příspěvek a zda je její stav přidán. V takovém případě se v databázi vyhledá, zda již existuje příspěvek se stejným názvem. Pokud už existuje nějaký příspěvek, vytvoří se nový `DbEntityValidationResult`.

`DbEntityValidationResult` domy `DbEntityEntry` a `ICollection<DbValidationErrors>` pro jednu entitu. Na začátku této metody je vytvořena instance `DbEntityValidationResult` a všechny zjištěné chyby jsou přidány do své kolekce `ValidationErrors`.

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a>Explicitní aktivace ověřování

Volání `SaveChanges` aktivuje všechna ověření uvedená v tomto článku. Nemusíte ale spoléhat na `SaveChanges`. Možná dáváte přednost ověřování jinde v aplikaci.

`DbContext.GetValidationErrors` spustí všechna ověření, která jsou definována poznámkami nebo rozhraním API Fluent, ověřování vytvořené v `IValidatableObject` (například `Blog.Validate`) a validace provedené v metodě `DbContext.ValidateEntity`.

Následující kód bude volat `GetValidationErrors` v aktuální instanci `DbContext`. `ValidationErrors` jsou seskupeny podle typu entity do `DbEntityValidationResult`. Kód nejprve provede iteraci `DbEntityValidationResult`s vrácenou metodou a potom každou `DbValidationError` uvnitř.

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a>Další důvody při použití ověřování

Toto je několik dalších bodů, které je potřeba vzít v úvahu při použití Entity Framework ověřování:

- Opožděné načítání je během ověřování zakázané.
- EF ověří datové poznámky u nemapovaných vlastností (vlastnosti, které nejsou namapované na sloupec v databázi).
- Ověřování se provádí po zjištění změn během `SaveChanges`. Pokud během ověřování provedete změny, bude vám vaše zodpovědnost při sledování změn informovat.
- `DbUnexpectedValidationException` se vyvolá, pokud dojde k chybám během ověřování.
- Omezující vlastnosti, které Entity Framework zahrnuje do modelu (maximální délka, požadováno atd.), způsobí ověření, i když ve vašich třídách nejsou žádné poznámky k datům nebo jste pomocí návrháře EF vytvořili svůj model.
- Pravidla priority:
  - Volání rozhraní API Fluent přepíšou odpovídající datové poznámky
- Pořadí spouštění:
  - K ověření vlastnosti dojde před ověřením typu.
  - Ověřování typu se vyskytuje jenom v případě, že se ověření vlastnosti zdaří.
- Pokud je vlastnost složitá, bude její ověření zahrnovat také:
  - Ověřování na úrovni vlastností u vlastností komplexního typu
  - Ověřování na úrovni typů u komplexního typu, včetně ověřování `IValidatableObject` u komplexního typu

## <a name="summary"></a>Souhrn

Rozhraní API pro ověřování v Entity Framework se při ověřování na straně klienta v MVC velmi pečlivě přehrává, ale nemusíte spoléhat na ověřování na straně klienta. Entity Framework se postará o ověřování na straně serveru pro dataanotace nebo konfigurace, které jste použili s prvním kódem rozhraní API Fluent.

Také jste viděli řadu rozšiřujících bodů pro přizpůsobení chování, ať už používáte rozhraní @no__t 0, nebo klepněte do metody `DbContext.ValidateEntity`. A tyto poslední dva způsoby ověřování jsou k dispozici prostřednictvím `DbContext`, ať už používáte Code First, Model First nebo Database First pracovní postup k popisu koncepčního modelu.
