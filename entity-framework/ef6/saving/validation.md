---
title: Ověřování - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: eec834888e2e3efaadc8acf9d4f64307f394ea4a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994442"
---
# <a name="data-validation"></a><span data-ttu-id="f538f-102">Ověřování dat</span><span class="sxs-lookup"><span data-stu-id="f538f-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="f538f-103">**EF4.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="f538f-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="f538f-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí</span><span class="sxs-lookup"><span data-stu-id="f538f-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="f538f-105">Obsah na této stránce jsou upraveny z a článek původně vydané společností Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="f538f-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="f538f-106">Entity Framework obsahuje velké množství ověření funkcí, které můžou informační kanál prostřednictvím uživatelského rozhraní pro ověřování na straně klienta nebo použít pro ověřování na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="f538f-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="f538f-107">Při prvním použití kódu, můžete zadat ověření pomocí poznámky nebo fluent API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f538f-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="f538f-108">Další ověření a složitější, dá se zadat v kódu a bude fungovat, jestli váš model má za sebou kód nejprve model nejprve nebo databáze nejprve.</span><span class="sxs-lookup"><span data-stu-id="f538f-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="f538f-109">Model</span><span class="sxs-lookup"><span data-stu-id="f538f-109">The model</span></span>

<span data-ttu-id="f538f-110">Vám předvedu ověření pomocí jednoduchého páru tříd: Blog a příspěvku.</span><span class="sxs-lookup"><span data-stu-id="f538f-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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
## <a name="data-annotations"></a><span data-ttu-id="f538f-111">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="f538f-111">Data Annotations</span></span>

<span data-ttu-id="f538f-112">Poznámky ze sestavení System.ComponentModel.DataAnnotations kód nejprve používá jako jeden prostředek konfigurace kód první třídy.</span><span class="sxs-lookup"><span data-stu-id="f538f-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="f538f-113">Mezi tyto poznámky jsou ty, které poskytují pravidla, jako jsou požadované, MinLength a MaxLength.</span><span class="sxs-lookup"><span data-stu-id="f538f-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="f538f-114">Počet klientských aplikací .NET také rozpoznává tyto anotace, například technologie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f538f-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="f538f-115">Můžete dosáhnout i na straně a serverem ověřování na straně klienta pomocí těchto poznámek.</span><span class="sxs-lookup"><span data-stu-id="f538f-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="f538f-116">Například můžete vynutit název blogu vlastnost jako povinnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f538f-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="f538f-117">Žádný další kód nebo změny kódu v aplikaci existující aplikaci MVC provede ověřování na straně klienta, dokonce i dynamické vytvoření zprávu pomocí názvy vlastností a poznámek.</span><span class="sxs-lookup"><span data-stu-id="f538f-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![figure01](~/ef6/media/figure01.png)

<span data-ttu-id="f538f-119">V příspěvku back – metoda tohoto vytvořit zobrazení, Entity Framework se používá pro uložení nového blogu k databázi, ale ověřování na straně klienta MVC se aktivuje před aplikace dosáhne tento kód.</span><span class="sxs-lookup"><span data-stu-id="f538f-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="f538f-120">Ověřování na straně klienta však není odrážky testování.</span><span class="sxs-lookup"><span data-stu-id="f538f-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="f538f-121">Uživatelé můžou ovlivnit funkce jejich prohlížeče nebo horší ještě, se hacker použít některé podvodů, aby ověření uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f538f-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="f538f-122">Ale také rozpozná požadované poznámky a ověřte ho Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f538f-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="f538f-123">Zakázat funkci ověřování na straně klienta pro MVC je jednoduchý způsob, jak to otestovat.</span><span class="sxs-lookup"><span data-stu-id="f538f-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="f538f-124">Můžete to provést v souboru web.config aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="f538f-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="f538f-125">Sekci appSettings má klíč pro ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="f538f-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="f538f-126">Nastavení tohoto klíče na hodnotu false zabrání uživatelském rozhraní provádí ověření.</span><span class="sxs-lookup"><span data-stu-id="f538f-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="f538f-127">I při ověřování na straně klienta, zakázán obdržíte stejnou odpověď ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f538f-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="f538f-128">Chybová zpráva "název pole je povinné" se zobrazí jako.</span><span class="sxs-lookup"><span data-stu-id="f538f-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="f538f-129">S tím rozdílem, teď bude výsledek ověření na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="f538f-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="f538f-130">Entity Framework provede ověření na požadované poznámky (před bothers i k sestavení a příkaz INSERT pro odeslání do databáze) a vrátí chybu MVC, která se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="f538f-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f538f-131">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="f538f-131">Fluent API</span></span>

<span data-ttu-id="f538f-132">Kód můžete použít pro první rozhraní API fluent místo poznámky zobrazíte stejného klienta na straně & serveru straně ověření.</span><span class="sxs-lookup"><span data-stu-id="f538f-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="f538f-133">Místo použití jsou povinné, ukážeme vám pomocí ověření MaxLength.</span><span class="sxs-lookup"><span data-stu-id="f538f-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="f538f-134">Konfigurace Fluent API jsou použity v kódu nejprve se sestavení modelu z tříd.</span><span class="sxs-lookup"><span data-stu-id="f538f-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="f538f-135">Konfigurace můžete vložit tak, že přepíšete metodu OnModelCreating třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="f538f-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="f538f-136">Tady je konfigurace určující, zda vlastnost BloggerName nesmí být delší než 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="f538f-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="f538f-137">Chyby ověření vyvolána konfigurace rozhraní Fluent API nebude automaticky dosah uživatelského rozhraní, ale můžete je i zachytávat v kódu a pak odpověď na ni odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="f538f-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="f538f-138">Tady je kód chyby v třídě BlogController vaší aplikace, která zachycuje tuto chybu ověření, když se pokusí uložit blog s BloggerName, která překračuje maximální počet 10 znaků Entity Framework pro zpracování některé výjimky.</span><span class="sxs-lookup"><span data-stu-id="f538f-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="f538f-139">Ověření, nebude se automaticky předat zpět do zobrazení, což je důvod, proč se používá další kód, který používá ModelState.AddModelError.</span><span class="sxs-lookup"><span data-stu-id="f538f-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="f538f-140">Tím se zajistí, že se podrobnosti o chybě vytvořit zobrazení, která se pak použije ValidationMessageFor Htmlhelper k zobrazení chyby.</span><span class="sxs-lookup"><span data-stu-id="f538f-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="f538f-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="f538f-141">IValidatableObject</span></span>

<span data-ttu-id="f538f-142">IValidatableObject je rozhraní, které se nachází v System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="f538f-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="f538f-143">I když není součástí rozhraní API Entity Framework, můžete stále využít ji pro ověřování na straně serveru ve třídách rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f538f-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="f538f-144">IValidatableObject poskytuje ověřit metodu, která bude volat rozhraní Entity Framework během SaveChanges, nebo můžete volat sami kdykoli chcete ověřit třídy.</span><span class="sxs-lookup"><span data-stu-id="f538f-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="f538f-145">Konfigurace, jako požadované a MaxLength proveďte validaton na jedno pole.</span><span class="sxs-lookup"><span data-stu-id="f538f-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="f538f-146">V metodě ověření můžete mít i složitější logiku, například porovnání dvou polí.</span><span class="sxs-lookup"><span data-stu-id="f538f-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="f538f-147">V následujícím příkladu bylo rozšířeno blogu třídy k implementaci IValidatableObject a pak zadejte pravidlo, které neodpovídá názvu a BloggerName.</span><span class="sxs-lookup"><span data-stu-id="f538f-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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

<span data-ttu-id="f538f-148">Konstruktor ValidationResult přebírá řetězec představující chybovou zprávu a pole řetězců, které představují názvy členů, které jsou spojeny s ověření.</span><span class="sxs-lookup"><span data-stu-id="f538f-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="f538f-149">Protože toto ověření kontroluje název i BloggerName, vrátí se oba názvy vlastností.</span><span class="sxs-lookup"><span data-stu-id="f538f-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="f538f-150">Na rozdíl od ověřování poskytované rozhraní Fluent API tento výsledek ověření bude rozpoznán zobrazením a jsem použili dříve k přidání do ModelState chyba obslužné rutiny výjimky je zbytečné.</span><span class="sxs-lookup"><span data-stu-id="f538f-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="f538f-151">Protože jsem nastavil ValidationResult oba názvy vlastností, MVC HtmlHelpers zobrazí chybová zpráva pro obě tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f538f-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="f538f-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="f538f-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="f538f-154">Kontext databáze má přepisovatelnou metodu volá ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="f538f-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="f538f-155">Při volání SaveChanges, Entity Framework tuto metodu volat pro každou entitu v mezipaměti, jejichž stav není beze změny.</span><span class="sxs-lookup"><span data-stu-id="f538f-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="f538f-156">Můžete vložit logiku ověřování přímo v zde nebo dokonce použít tuto metodu za účelem volání, například metoda Blog.Validate přidali v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="f538f-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="f538f-157">Tady je příklad ValidateEntity přepsání, která ověřuje nové příspěvky k zajištění, že nadpis příspěvku nebylo se už používá.</span><span class="sxs-lookup"><span data-stu-id="f538f-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="f538f-158">Nejprve zkontroluje podívejte se, jestli je entita příspěvek a přidaná svůj stav.</span><span class="sxs-lookup"><span data-stu-id="f538f-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="f538f-159">Pokud je to tento případ, pak vypadá v databázi pro zobrazení, pokud je již příspěvek se stejným názvem.</span><span class="sxs-lookup"><span data-stu-id="f538f-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="f538f-160">Pokud již existující příspěvek, se vytvoří nová DbEntityValidationResult.</span><span class="sxs-lookup"><span data-stu-id="f538f-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="f538f-161">DbEntityValidationResult jsou uloženy DbEntityEntry a rozhraní ICollection DbValidationErrors jedné entity.</span><span class="sxs-lookup"><span data-stu-id="f538f-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="f538f-162">Na začátku této metody DbEntityValidationResult je vytvořena instance a potom se přidají všechny chyby, které jsou zjištěny do jeho ValidationErrors kolekce.</span><span class="sxs-lookup"><span data-stu-id="f538f-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="f538f-163">Aktivuje se explicitně ověření</span><span class="sxs-lookup"><span data-stu-id="f538f-163">Explicitly triggering validation</span></span>

<span data-ttu-id="f538f-164">Volání SaveChanges aktivuje všechny ověřovací popsaná v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f538f-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="f538f-165">Ale nemusíte spoléhat na SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f538f-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="f538f-166">Můžete chtít ověřit jinde v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f538f-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="f538f-167">DbContext.GetValidationErrors aktivují všech ověření, těmi definovanými ve poznámky nebo rozhraní Fluent API, ověření vytvořené v IValidatableObject (například Blog.Validate) a ověření provádět v DbContext.ValidateEntity Metoda.</span><span class="sxs-lookup"><span data-stu-id="f538f-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="f538f-168">Následující kód zavolá GetValidationErrors na aktuální instancí třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="f538f-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="f538f-169">ValidationErrors jsou seskupené podle typu entity do DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="f538f-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="f538f-170">Kód prochází nejprve prostřednictvím DbValidationResults vrácený metodou a potom každý ValidationError uvnitř.</span><span class="sxs-lookup"><span data-stu-id="f538f-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="f538f-171">Další informace týkající se použití ověřování</span><span class="sxs-lookup"><span data-stu-id="f538f-171">Other considerations when using validation</span></span>

<span data-ttu-id="f538f-172">Tady je několik věcí, které je třeba zvážit při použití rozhraní Entity Framework ověření:</span><span class="sxs-lookup"><span data-stu-id="f538f-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="f538f-173">Opožděné načítání je zakázáno během ověřování.</span><span class="sxs-lookup"><span data-stu-id="f538f-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="f538f-174">EF ověří anotacemi dat na jiných mapované vlastnosti (vlastnosti, které nejsou namapované na sloupec v databázi).</span><span class="sxs-lookup"><span data-stu-id="f538f-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="f538f-175">Po zjištění změny během SaveChanges, provede se ověření.</span><span class="sxs-lookup"><span data-stu-id="f538f-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="f538f-176">Pokud provedete změny během ověřování je vaší odpovědností, abyste upozornit modul sledování změny.</span><span class="sxs-lookup"><span data-stu-id="f538f-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="f538f-177">DbUnexpectedValidationException je vyvolána, pokud dojde k chybám při ověřování.</span><span class="sxs-lookup"><span data-stu-id="f538f-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="f538f-178">Omezující vlastnosti, které zahrnuje Entity Framework v modelu (maximální délka povolená, atd.) způsobí ověření, i když již nejsou anotacemi dat ve vašich třídách a/nebo jste použili k vytvoření modelu EF designeru.</span><span class="sxs-lookup"><span data-stu-id="f538f-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="f538f-179">Priorita pravidla:</span><span class="sxs-lookup"><span data-stu-id="f538f-179">Precedence rules:</span></span>
    -   <span data-ttu-id="f538f-180">Přepsat odpovídající anotacemi dat Fluent volání rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f538f-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="f538f-181">Pořadí spuštění:</span><span class="sxs-lookup"><span data-stu-id="f538f-181">Execution order:</span></span>
    -   <span data-ttu-id="f538f-182">Ověření vlastností předchází ověření typu</span><span class="sxs-lookup"><span data-stu-id="f538f-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="f538f-183">Ověření typu dochází pouze v případě úspěšného ověření vlastnost</span><span class="sxs-lookup"><span data-stu-id="f538f-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="f538f-184">Pokud je komplexní vlastnost bude obsahovat také jeho ověření:</span><span class="sxs-lookup"><span data-stu-id="f538f-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="f538f-185">Vlastnost úroveň ověření na komplexní typ vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f538f-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="f538f-186">Zadejte úroveň ověření na komplexní typ, včetně ověření IValidatableObject na komplexní typ</span><span class="sxs-lookup"><span data-stu-id="f538f-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="f538f-187">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f538f-187">Summary</span></span>

<span data-ttu-id="f538f-188">Ověřování rozhraní API v Entity Framework hraje velmi krásně pomocí ověřování na straně klienta v MVC, ale nemusíte spoléhat na ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f538f-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="f538f-189">Entity Framework se postará o ověřování na straně serveru pro DataAnnotations nebo konfigurace, které jste použili s první Fluent API kódu.</span><span class="sxs-lookup"><span data-stu-id="f538f-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="f538f-190">Také jste zjistili několik bodů rozšiřitelnosti pro přizpůsobení chování, ať už pomocí rozhraní IValidatableObject nebo využijte metodu DbContet.ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="f538f-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="f538f-191">A tyto poslední dva způsoby ověření jsou k dispozici prostřednictvím uvolněn objekt DbContext, ať už používáte pracovní postup Code First, Model první nebo Database First k popisu koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="f538f-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
