---
title: Ověření – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 457af0c32f0fe4804dbfe6e348664efb1af517c9
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565373"
---
# <a name="data-validation"></a><span data-ttu-id="1fbf0-102">Ověření dat</span><span class="sxs-lookup"><span data-stu-id="1fbf0-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="1fbf0-103">**EF 4.1 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="1fbf0-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="1fbf0-105">Obsah na této stránce je přizpůsoben z článku, který původně napsal Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="1fbf0-105">The content on this page is adapted from an article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="1fbf0-106">Entity Framework poskytuje skvělou škálu funkcí ověřování, které se můžou dodávat do uživatelského rozhraní pro ověřování na straně klienta nebo používat při ověřování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="1fbf0-107">Při prvním použití kódu můžete zadat ověření pomocí konfigurace anotace nebo Fluent rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="1fbf0-108">Další ověření a složitější, mohou být specifikovány v kódu a budou fungovat bez ohledu na to, zda model Susan z kódu jako první, model First nebo Database First.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="1fbf0-109">Model</span><span class="sxs-lookup"><span data-stu-id="1fbf0-109">The model</span></span>

<span data-ttu-id="1fbf0-110">Ukážeme ověřování pomocí jednoduché dvojice tříd: Blog a příspěvek.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="1fbf0-111">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="1fbf0-111">Data Annotations</span></span>

<span data-ttu-id="1fbf0-112">Code First používá poznámky ze `System.ComponentModel.DataAnnotations` sestavení jako jeden způsob konfigurace třídy Code First.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="1fbf0-113">Mezi tyto poznámky patří ta `Required`, která poskytují pravidla jako například, `MaxLength` a. `MinLength`</span><span class="sxs-lookup"><span data-stu-id="1fbf0-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="1fbf0-114">Řada klientských aplikací .NET také rozeznává tyto poznámky, například ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="1fbf0-115">Pomocí těchto poznámek můžete dosáhnout ověřování na straně klienta i na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="1fbf0-116">Například můžete vynutit, aby vlastnost nadpisu blogu byla požadovaná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="1fbf0-117">Bez jakýchkoli dalších změn kódu nebo kódu v aplikaci bude existující aplikace MVC provádět ověřování na straně klienta, a to i dynamicky sestavovat zprávu pomocí názvů vlastností a poznámek.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Obrázek 1](~/ef6/media/figure01.png)

<span data-ttu-id="1fbf0-119">V metodě post back tohoto zobrazení pro vytváření Entity Framework slouží k uložení nového blogu do databáze, ale ověřování na straně klienta MVC se aktivuje předtím, než aplikace dosáhne tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="1fbf0-120">Ověřování na straně klienta není ale odrážka.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="1fbf0-121">Uživatelé můžou mít vliv na funkce prohlížeče nebo horšího dopadu, hacker může použít určitou podmnožinu, aby nedocházelo k ověřování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="1fbf0-122">Ale Entity Framework také rozpozná `Required` anotaci a ověří ji.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="1fbf0-123">Jedním z možností, jak tuto možnost otestovat, je zakázat funkci ověřování na straně klienta MVC.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="1fbf0-124">To můžete provést v souboru Web. config aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="1fbf0-125">Oddíl appSettings obsahuje klíč pro ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="1fbf0-126">Nastavením tohoto klíče na false zabráníte uživatelskému rozhraní v provádění ověření.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="1fbf0-127">I v případě, že je ověřování na straně klienta zakázané, získáte stejnou odpověď v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="1fbf0-128">Chybová zpráva "pole název je povinné" se zobrazí jako dříve.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="1fbf0-129">S výjimkou této chvíle bude výsledkem ověřování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="1fbf0-130">Entity Framework provede ověření `Required` poznámky (před tím, než oba `INSERT` výrobci vytvoří příkaz k odeslání do databáze) a vrátí chybu do MVC, která zobrazí zprávu.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1fbf0-131">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="1fbf0-131">Fluent API</span></span>

<span data-ttu-id="1fbf0-132">K získání stejné klientské strany & ověřování na straně serveru můžete použít rozhraní Fluent API pro první použití kódu, nikoli poznámky.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="1fbf0-133">`Required`Místo toho vám ukážeme to pomocí ověření MaxLength.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="1fbf0-134">Konfigurace Fluent rozhraní API se aplikují jako první kód sestavuje model z tříd.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="1fbf0-135">Můžete vložit konfigurace přepsáním metody OnModelCreating třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="1fbf0-136">Tady je konfigurace, která určuje, že vlastnost blogger nemůže být delší než 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="1fbf0-137">Chyby ověřování vyvolané pomocí konfigurací Fluent rozhraní API nebudou automaticky přicházet k uživatelskému rozhraní, ale můžete je zachytit v kódu a následně na něj reagovat.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="1fbf0-138">Tady je několik chybových kódů zpracování výjimek ve třídě BlogController aplikace, která zachycuje chybu ověřování při Entity Framework se pokusí uložit blog s vlastností blogger, který překračuje maximum 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="1fbf0-139">Ověřování se automaticky nevrátí zpět do zobrazení, což znamená, proč se používá další kód, `ModelState.AddModelError` který používá.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="1fbf0-140">Tím se zajistí, že se podrobnosti o chybě změní na zobrazení, které pak k `ValidationMessageFor` zobrazení chyby použije HtmlHelper.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="1fbf0-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="1fbf0-141">IValidatableObject</span></span>

<span data-ttu-id="1fbf0-142">`IValidatableObject`je rozhraní, které žije v `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="1fbf0-143">I když není součástí rozhraní Entity Framework API, můžete ho i nadále využít pro ověřování na straně serveru ve vašich Entity Frameworkch třídách.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="1fbf0-144">`IValidatableObject``Validate` poskytuje metodu, kterou Entity Framework voláním metody SaveChanges, nebo můžete zavolat sami, kdykoli chcete třídy ověřit.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="1fbf0-145">Konfigurace, jako `Required` jsou `MaxLength` a provádějí ověřování v jednom poli.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="1fbf0-146">`Validate` V metodě můžete mít ještě složitější logiku, například porovnávání dvou polí.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="1fbf0-147">V následujícím příkladu `Blog` byla třída rozšířena k implementaci `IValidatableObject` a pak `Title` poskytuje pravidlo, které a `BloggerName` nemůže souhlasit.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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

<span data-ttu-id="1fbf0-148">Konstruktor přebírá chybovou `string`zprávu a pole s, které představují názvy členů, které jsou přidruženy k ověření. `string` `ValidationResult`</span><span class="sxs-lookup"><span data-stu-id="1fbf0-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="1fbf0-149">Vzhledem k tomu `BloggerName`, že toto `Title` ověření kontroluje jak a, jsou vráceny oba názvy vlastností.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="1fbf0-150">Na rozdíl od ověření, které poskytuje rozhraní Fluent API, tento výsledek ověření rozpozná zobrazení a obslužná rutina výjimky, kterou jsem použil (a) k přidání chyby do `ModelState` není potřeba.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="1fbf0-151">Vzhledem k tomu `ValidationResult`, že jsou oba názvy vlastností nastaveny v, MVC HtmlHelpers zobrazí chybovou zprávu pro obě tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Obrázek 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="1fbf0-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="1fbf0-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="1fbf0-154">`DbContext`má metodu s názvem `ValidateEntity`přepsatelné metody.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="1fbf0-155">Při volání `SaveChanges`bude Entity Framework volat tuto metodu pro každou entitu v mezipaměti, jejíž stav `Unchanged`není.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="1fbf0-156">Logiku ověřování můžete umístit přímo sem nebo dokonce použít tuto metodu k volání `Blog.Validate` metody, například metody přidané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="1fbf0-157">Tady je příklad `ValidateEntity` přepsání, které ověřuje nové `Post`s, aby se zajistilo, že se název příspěvku už nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="1fbf0-158">Nejprve zkontroluje, zda je entita příspěvek a zda je její stav přidán.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="1fbf0-159">V takovém případě se v databázi vyhledá, zda již existuje příspěvek se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="1fbf0-160">Pokud už existuje nějaký příspěvek, vytvoří se nový `DbEntityValidationResult` .</span><span class="sxs-lookup"><span data-stu-id="1fbf0-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="1fbf0-161">`DbEntityValidationResult``DbEntityEntry` domy`ICollection<DbValidationErrors>` a a pro jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="1fbf0-162">Na začátku této metody `DbEntityValidationResult` je vytvořena instance a pak všechny zjištěné chyby jsou přidány do své `ValidationErrors` kolekce.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="1fbf0-163">Explicitní aktivace ověřování</span><span class="sxs-lookup"><span data-stu-id="1fbf0-163">Explicitly triggering validation</span></span>

<span data-ttu-id="1fbf0-164">Volání `SaveChanges` triggeru aktivuje všechna ověření uvedená v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="1fbf0-165">Nemusíte ale spoléhat na `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="1fbf0-166">Možná dáváte přednost ověřování jinde v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="1fbf0-167">`DbContext.GetValidationErrors`aktivuje všechna ověření, která jsou definována poznámkami nebo rozhraním Fluent API, ověřování vytvořené v `IValidatableObject` ( `Blog.Validate`například) a validace provedené v `DbContext.ValidateEntity` metodě.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="1fbf0-168">Následující kód bude volat `GetValidationErrors` aktuální instanci `DbContext`třídy.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="1fbf0-169">`ValidationErrors`jsou seskupeny podle typu entity `DbEntityValidationResult`do.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="1fbf0-170">Kód nejprve projde prostřednictvím `DbEntityValidationResult`metody s vrácenou metodou a potom skrze každou `DbValidationError` zevnitř.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="1fbf0-171">Další důvody při použití ověřování</span><span class="sxs-lookup"><span data-stu-id="1fbf0-171">Other considerations when using validation</span></span>

<span data-ttu-id="1fbf0-172">Toto je několik dalších bodů, které je potřeba vzít v úvahu při použití Entity Framework ověřování:</span><span class="sxs-lookup"><span data-stu-id="1fbf0-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="1fbf0-173">Opožděné načítání je během ověřování zakázané.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="1fbf0-174">EF ověří datové poznámky u nemapovaných vlastností (vlastnosti, které nejsou namapované na sloupec v databázi).</span><span class="sxs-lookup"><span data-stu-id="1fbf0-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="1fbf0-175">Ověřování se provádí po zjištění změn během `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="1fbf0-176">Pokud během ověřování provedete změny, bude vám vaše zodpovědnost při sledování změn informovat.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="1fbf0-177">`DbUnexpectedValidationException`je vyvolána, pokud dojde k chybám během ověřování.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="1fbf0-178">Omezující vlastnosti, které Entity Framework zahrnuje do modelu (maximální délka, požadováno atd.), způsobí ověření, i když ve vašich třídách nejsou žádné poznámky k datům nebo jste pomocí návrháře EF vytvořili svůj model.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="1fbf0-179">Pravidla priority:</span><span class="sxs-lookup"><span data-stu-id="1fbf0-179">Precedence rules:</span></span>
  - <span data-ttu-id="1fbf0-180">Volání rozhraní API Fluent přepíšou odpovídající datové poznámky</span><span class="sxs-lookup"><span data-stu-id="1fbf0-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="1fbf0-181">Pořadí spouštění:</span><span class="sxs-lookup"><span data-stu-id="1fbf0-181">Execution order:</span></span>
  - <span data-ttu-id="1fbf0-182">K ověření vlastnosti dojde před ověřením typu.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="1fbf0-183">Ověřování typu se vyskytuje jenom v případě, že se ověření vlastnosti zdaří.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="1fbf0-184">Pokud je vlastnost složitá, bude její ověření zahrnovat také:</span><span class="sxs-lookup"><span data-stu-id="1fbf0-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="1fbf0-185">Ověřování na úrovni vlastností u vlastností komplexního typu</span><span class="sxs-lookup"><span data-stu-id="1fbf0-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="1fbf0-186">Ověřování na úrovni typu u komplexního typu, `IValidatableObject` včetně ověřování u komplexního typu</span><span class="sxs-lookup"><span data-stu-id="1fbf0-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="1fbf0-187">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1fbf0-187">Summary</span></span>

<span data-ttu-id="1fbf0-188">Rozhraní API pro ověřování v Entity Framework se při ověřování na straně klienta v MVC velmi pečlivě přehrává, ale nemusíte spoléhat na ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="1fbf0-189">Entity Framework se postará o ověřování na straně serveru pro dataanotace nebo konfigurace, které jste použili s prvním kódem rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="1fbf0-190">Také jste viděli několik bodů rozšiřitelnosti pro přizpůsobení chování bez ohledu na to, zda `IValidatableObject` používáte rozhraní nebo klepněte `DbContext.ValidateEntity` do metody.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="1fbf0-191">A tyto poslední dva způsoby ověřování jsou k dispozici prostřednictvím `DbContext`, ať už používáte Code First, model First nebo Database First pracovní postup k popisu konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="1fbf0-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
