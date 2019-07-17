---
title: Kód anotací dat při prvním - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: fcd01aef7303573001460b352f8099b2cc6e224a
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286482"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="3ed60-102">Kód první datové poznámky</span><span class="sxs-lookup"><span data-stu-id="3ed60-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="3ed60-103">**EF4.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="3ed60-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="3ed60-104">Pokud používáte starší verzi, některé nebo všechny tyto informace se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="3ed60-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="3ed60-105">Obsah na této stránce jsou upraveny z článku původně vytvořeny pomocí Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="3ed60-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="3ed60-106">Entity Framework Code First umožňuje používat vaše vlastní třídy domény k vyjádření modelu EF využívá k provedení dotazu, změňte sledování a aktualizuje se funkce.</span><span class="sxs-lookup"><span data-stu-id="3ed60-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="3ed60-107">Kód nejprve využívá programovací model označovány jako "úmluva nad konfigurací."</span><span class="sxs-lookup"><span data-stu-id="3ed60-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="3ed60-108">Kód nejprve převezme, Entity Framework pro vytváření tříd a v takovém případě budou automaticky fungovat možnostmi, jak provést její úlohy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform it's job.</span></span> <span data-ttu-id="3ed60-109">Ale pokud tříd nepostupujte podle těchto konvence, máte možnost přidání konfigurace do vaší třídy, které poskytují EF s požadovanými informacemi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="3ed60-110">Kód nejprve poskytuje dva způsoby, jak tyto konfigurace přidat do vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="3ed60-111">Jeden používá jednoduché atributy, které volá DataAnnotations a druhý je použití Code First Fluent API, která vám poskytuje způsob, jak popisují konfiguraci toho, v kódu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="3ed60-112">Tento článek se zaměří na používání DataAnnotations (v oboru názvů System.ComponentModel.DataAnnotations) ke konfiguraci třídy – zvýraznění nejčastěji potřebných konfigurací.</span><span class="sxs-lookup"><span data-stu-id="3ed60-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="3ed60-113">DataAnnotations také rozumí počet aplikací .NET, jako je například technologie ASP.NET MVC, která umožňuje tyto aplikace využívat stejné poznámky k ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="3ed60-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="3ed60-114">Model</span><span class="sxs-lookup"><span data-stu-id="3ed60-114">The model</span></span>

<span data-ttu-id="3ed60-115">Vám předvedu první DataAnnotations kódu pomocí jednoduchého páru tříd: Blog a příspěvku.</span><span class="sxs-lookup"><span data-stu-id="3ed60-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
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

<span data-ttu-id="3ed60-116">Jsou, blogu a příspěvku třídy pohodlně postupujte podle úmluvy první kód a vyžadují žádné vylepšení umožňující EF kompatibilitou.</span><span class="sxs-lookup"><span data-stu-id="3ed60-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="3ed60-117">Ale můžete také použít poznámky vám poskytneme Další informace o třídách a databáze, ke které jsou mapovány na EF.</span><span class="sxs-lookup"><span data-stu-id="3ed60-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="3ed60-118">Key</span><span class="sxs-lookup"><span data-stu-id="3ed60-118">Key</span></span>

<span data-ttu-id="3ed60-119">Entity Framework spoléhá na každé entitě s hodnotou klíče, který se používá pro entitu pro sledování.</span><span class="sxs-lookup"><span data-stu-id="3ed60-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="3ed60-120">Jeden konvence Code First je implicitní klíčové vlastnosti; Kód nejprve bude hledat vlastnost s názvem "Id" nebo kombinace názvu třídy a "Id", jako je například "BlogId".</span><span class="sxs-lookup"><span data-stu-id="3ed60-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="3ed60-121">Tato vlastnost bude mapovat na sloupec primárního klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="3ed60-122">Třídy blogu a účtovat podle Tato konvence.</span><span class="sxs-lookup"><span data-stu-id="3ed60-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="3ed60-123">Co když se nepovedlo?</span><span class="sxs-lookup"><span data-stu-id="3ed60-123">What if they didn’t?</span></span> <span data-ttu-id="3ed60-124">Co když blogu použít název *PrimaryTrackingKey* místo, nebo dokonce *foo*?</span><span class="sxs-lookup"><span data-stu-id="3ed60-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="3ed60-125">Pokud kód nejprve nenajde vlastnost, která odpovídá tato konvence vyvolá výjimku z důvodu požadavku Entity Framework, že musí mít vlastnost klíče.</span><span class="sxs-lookup"><span data-stu-id="3ed60-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="3ed60-126">Poznámka klíče můžete použít k určení vlastností, které se má použít jako EntityKey.</span><span class="sxs-lookup"><span data-stu-id="3ed60-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="3ed60-127">Pokud jste nejdřív pomocí kódu je funkce generování databáze, blogu tabulka bude obsahovat sloupec primárního klíče s názvem PrimaryTrackingKey, který je definovaný i jako identitu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3ed60-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Blog tabulky s primárním klíčem](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="3ed60-129">Složené klíče</span><span class="sxs-lookup"><span data-stu-id="3ed60-129">Composite keys</span></span>

<span data-ttu-id="3ed60-130">Entity Framework podporuje složené klíče – primární klíče, které se skládají z více než jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3ed60-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="3ed60-131">Například můžete mít Passport třídy, jejichž primární klíč je kombinací PassportNumber a IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="3ed60-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="3ed60-132">Pokus o použití ve vašem modelu EF třídu výše by výsledkem `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="3ed60-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="3ed60-133">*Nepovedlo se určit složený primární klíče řazení pro typ "Passport". Použijte ColumnAttribute nebo metodu HasKey k určení pořadí pro složený primární klíče.*</span><span class="sxs-lookup"><span data-stu-id="3ed60-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="3ed60-134">Chcete-li použít složené klíče, Entity Framework vyžaduje, abyste k definování objednávku ke klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="3ed60-135">Můžete to provést s použitím sloupce Poznámka k určení pořadí.</span><span class="sxs-lookup"><span data-stu-id="3ed60-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="3ed60-136">Hodnota pořadí je relativní (a ne na základě indexu), můžete použít všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3ed60-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="3ed60-137">Například 100 až 200 bude přijatelné namísto 1 a 2.</span><span class="sxs-lookup"><span data-stu-id="3ed60-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="3ed60-138">Pokud máte entity složené cizí klíče, musíte zadat stejný sloupec řazení, který jste použili pro odpovídající vlastnosti primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="3ed60-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="3ed60-139">Pouze relativní řazení v rámci vlastnosti cizího klíče musí být stejný přesné hodnoty přiřazené k **pořadí** nemusí odpovídat.</span><span class="sxs-lookup"><span data-stu-id="3ed60-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="3ed60-140">Například ve třídě následující 3 a 4 by mohla být zastoupen 1 a 2.</span><span class="sxs-lookup"><span data-stu-id="3ed60-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="3ed60-141">Požadováno</span><span class="sxs-lookup"><span data-stu-id="3ed60-141">Required</span></span>

<span data-ttu-id="3ed60-142">Poznámka vyžaduje říká EF vyžádáním určité vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="3ed60-143">Přidání vyžaduje vlastnost názvu vynutí EF (a MVC) ujistěte se, že vlastnost obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="3ed60-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="3ed60-144">Žádný další kód nebo změny kódu v aplikaci aplikace MVC provede ověřování na straně klienta, dokonce i dynamické vytvoření zprávu pomocí názvy vlastností a poznámek.</span><span class="sxs-lookup"><span data-stu-id="3ed60-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Vytvoření stránky s názvem je povinné chyba](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="3ed60-146">Požadovaný atribut ovlivní také generován databázový tím, že namapovanou vlastnost Null.</span><span class="sxs-lookup"><span data-stu-id="3ed60-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="3ed60-147">Všimněte si, že pole název se změnil na "not null".</span><span class="sxs-lookup"><span data-stu-id="3ed60-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="3ed60-148">V některých případech nemusí být možné pro sloupec v databázi být null, i když tato vlastnost je vyžadovaná.</span><span class="sxs-lookup"><span data-stu-id="3ed60-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="3ed60-149">Například při použití dat TPH dědičnosti strategie pro více typů je uložené v jediné tabulce.</span><span class="sxs-lookup"><span data-stu-id="3ed60-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="3ed60-150">Pokud odvozený typ obsahuje požadovanou vlastnost sloupec nelze nastavit jako Null protože ne všechny typy v hierarchii, bude mít tato vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3ed60-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Blogy tabulky](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="3ed60-152">MaxLength a MinLength</span><span class="sxs-lookup"><span data-stu-id="3ed60-152">MaxLength and MinLength</span></span>

<span data-ttu-id="3ed60-153">Atributy MaxLength a MinLength umožňují zadat další vlastnosti ověření, stejně jako u požadované.</span><span class="sxs-lookup"><span data-stu-id="3ed60-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="3ed60-154">Tady je BloggerName s požadavky na délku.</span><span class="sxs-lookup"><span data-stu-id="3ed60-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="3ed60-155">Tento příklad také ukazuje, jak kombinovat atributy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="3ed60-156">Poznámka MaxLength ovlivní databázi nastavením vlastnosti length na 10.</span><span class="sxs-lookup"><span data-stu-id="3ed60-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Blogy tabulka zobrazující maximální délka pro sloupec BloggerName](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="3ed60-158">Poznámka MVC na straně klienta a poznámky na straně serveru EF 4.1 dodrží i toto ověření znovu dynamické vytvoření chybová zpráva: "Pole BloggerName musí být typu řetězce nebo pole s maximální délkou"10"." Tato zpráva je trochu dlouhý.</span><span class="sxs-lookup"><span data-stu-id="3ed60-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="3ed60-159">Mnoho anotace umožňují zadat chybovou zprávu s atributem chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="3ed60-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="3ed60-160">Můžete také zadat chybová zpráva v poznámce požadované.</span><span class="sxs-lookup"><span data-stu-id="3ed60-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Vytvoření stránky se vlastní chybová zpráva](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="3ed60-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="3ed60-162">NotMapped</span></span>

<span data-ttu-id="3ed60-163">První konvence kódu určí, že všech vlastností, které je podporované datového typu je reprezentován v databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="3ed60-164">Ale to není vždy případ ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="3ed60-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="3ed60-165">Například může mít vlastnost ve třídě blogu, který vytvoří kódu na základě názvu a BloggerName polí.</span><span class="sxs-lookup"><span data-stu-id="3ed60-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="3ed60-166">Tato vlastnost se dají vytvářet dynamicky a není potřeba ukládat.</span><span class="sxs-lookup"><span data-stu-id="3ed60-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="3ed60-167">Můžete označit všechny vlastnosti, které se nemapují na databázi s poznámkou NotMapped například tuto vlastnost BlogCode.</span><span class="sxs-lookup"><span data-stu-id="3ed60-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="3ed60-168">Typ ComplexType</span><span class="sxs-lookup"><span data-stu-id="3ed60-168">ComplexType</span></span>

<span data-ttu-id="3ed60-169">Není k popisu entity domény mezi sadu tříd a potom vrstvy těchto tříd k popisu úplnou entitu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="3ed60-170">Můžete například přidat třídu s názvem BlogDetails do modelu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="3ed60-171">Všimněte si, že BlogDetails nemá libovolného typu klíčovou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3ed60-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="3ed60-172">V návrhu na základě domény BlogDetails označuje jako objekt hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3ed60-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="3ed60-173">Entity Framework odkazují na objekty hodnotu jako komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="3ed60-174">  Komplexní typy nelze sledovat na své vlastní.</span><span class="sxs-lookup"><span data-stu-id="3ed60-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="3ed60-175">Nicméně jako vlastnost ve třídě blogu BlogDetails ho budou sledovány jako součást objektu blogu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="3ed60-176">Aby code first pro to rozpoznat je třeba označit třídu BlogDetails jako element ComplexType.</span><span class="sxs-lookup"><span data-stu-id="3ed60-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="3ed60-177">Nyní můžete přidat vlastnost ve třídě blogu k reprezentaci BlogDetails pro tento blog.</span><span class="sxs-lookup"><span data-stu-id="3ed60-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="3ed60-178">V databázi v blogu tabulce bude obsahovat všechny vlastnosti včetně vlastnosti obsažené v jeho vlastnost BlogDetail blogu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="3ed60-179">Ve výchozím nastavení každý z nich je před názvem komplexní typ, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="3ed60-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Blog tabulku s komplexní typ](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="3ed60-181">Atribut ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="3ed60-181">ConcurrencyCheck</span></span>

<span data-ttu-id="3ed60-182">Poznámka atribut ConcurrencyCheck umožňuje označit jednu nebo více vlastností, které má být použit pro souběžnost kontroly v databázi, když uživatel upraví nebo odstraní entitu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="3ed60-183">Pokud jste pracovali s EF designeru, ten je v souladu s nastavení vlastnosti režim ConcurrencyMode na pevný.</span><span class="sxs-lookup"><span data-stu-id="3ed60-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="3ed60-184">Podívejme se, jak atribut ConcurrencyCheck funguje tak, že přidáte BloggerName vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="3ed60-185">Při volání SaveChanges, protože atribut ConcurrencyCheck Poznámka u pole BloggerName původní hodnota dané vlastnosti se použije v aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="3ed60-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="3ed60-186">Příkaz se pokusí najít správný řádek pomocí filtrování pouze na hodnotě klíče, ale také u původní hodnoty BloggerName.</span><span class="sxs-lookup"><span data-stu-id="3ed60-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="3ed60-187">  Tady jsou důležité části příkazu UPDATE odeslán do databáze, ve kterém uvidíte příkaz aktualizuje řádek, který má PrimaryTrackingKey je 1 a BloggerName z "Julie", který byl původní hodnotu při tomto blogu byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="3ed60-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="3ed60-188">Pokud do té doby někdo bloggeru název pro tento blog, tato aktualizace se nezdaří a získáte DbUpdateConcurrencyException, které budete potřebovat pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="3ed60-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="3ed60-189">Časové razítko</span><span class="sxs-lookup"><span data-stu-id="3ed60-189">TimeStamp</span></span>

<span data-ttu-id="3ed60-190">Je běžné použití pole rowversion nebo časového razítka pro kontrolu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="3ed60-191">Ale místo použití atribut ConcurrencyCheck poznámky, můžete použít konkrétnější anotaci časového razítka jako typ vlastnosti je bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="3ed60-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="3ed60-192">Kód nejprve bude považovat za vlastnosti časového razítka stejný atribut ConcurrencyCheck vlastnosti, ale také pomohou zajistit, že pole databáze, které kód nejprve vytvoří hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="3ed60-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="3ed60-193">Máte jenom jednu vlastnost časového razítka v dané třídě.</span><span class="sxs-lookup"><span data-stu-id="3ed60-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="3ed60-194">Přidávání do třídy blogu následující vlastnost:</span><span class="sxs-lookup"><span data-stu-id="3ed60-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="3ed60-195">výsledky v kódu nejprve vytvořit sloupec časového razítka Null v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="3ed60-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Blogy tabulku se sloupci razítko času](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="3ed60-197">Tabulky a sloupce</span><span class="sxs-lookup"><span data-stu-id="3ed60-197">Table and Column</span></span>

<span data-ttu-id="3ed60-198">Pokud umožníte tím Code First vytvořit databázi, můžete změnit název tabulky a sloupce, které se vytváří.</span><span class="sxs-lookup"><span data-stu-id="3ed60-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="3ed60-199">Code First můžete použít také k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="3ed60-200">Ale to není vždy případ, že názvy tříd a vlastností ve vaší doméně shodovat s názvy tabulek a sloupců ve vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="3ed60-201">Moje třída se nazývá blogu a podle konvence kódu nejprve předpokládá, že to se namapuje na tabulku s názvem blogy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="3ed60-202">Pokud se nejedná o tento případ můžete zadat název tabulky s atributem tabulky.</span><span class="sxs-lookup"><span data-stu-id="3ed60-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="3ed60-203">Zde například Poznámka je určení, že název tabulky je InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="3ed60-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="3ed60-204">Poznámka sloupec je další nimiž v určeném atributy pro mapovanou sloupec.</span><span class="sxs-lookup"><span data-stu-id="3ed60-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="3ed60-205">Můžete stanovit, název, datový typ nebo dokonce pořadí, ve kterém se zobrazí sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="3ed60-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="3ed60-206">Tady je příklad sloupce atributu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="3ed60-207">Nepleťte si Atribut TypeName sloupce s DataAnnotation datového typu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="3ed60-208">Datový typ se má poznámka použít pro uživatelské rozhraní a je ignorován Code First.</span><span class="sxs-lookup"><span data-stu-id="3ed60-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="3ed60-209">Tady je tabulka po je byly znovu vygenerovány.</span><span class="sxs-lookup"><span data-stu-id="3ed60-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="3ed60-210">InternalBlogs změnil název tabulky a sloupce s popisem od komplexní typ je nyní BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="3ed60-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="3ed60-211">Protože v poznámce byl zadán název, kód nejprve nebudeme používat konvenci od názvu sloupce s názvem komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Blogy tabulku a sloupec přejmenovat](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="3ed60-213">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="3ed60-213">DatabaseGenerated</span></span>

<span data-ttu-id="3ed60-214">Důležité databázové funkce je schopnost mít vypočítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="3ed60-215">Pokud máte mapování Code First tříd do tabulek, které obsahují vypočítané sloupce, nechcete, aby rozhraní Entity Framework a pokuste se aktualizovat tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="3ed60-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="3ed60-216">Ale být vhodné EF vracet tyto hodnoty z databáze po vložení nebo aktualizovaná data.</span><span class="sxs-lookup"><span data-stu-id="3ed60-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="3ed60-217">Poznámka DatabaseGenerated můžete označit, že tyto vlastnosti ve třídě spolu s vypočítané výčtu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="3ed60-218">Jiných výčtech k žádným nedošlo a Identity.</span><span class="sxs-lookup"><span data-stu-id="3ed60-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="3ed60-219">Můžete použít databáze, které jsou generovány pro sloupce bajtů nebo jako časové razítko, pokud kód je nejprve generování databáze, v opačném případě byste měli používat jenom to při odkazující na stávající databáze, protože kód nejprve nebude schopna určit vzorec pro počítaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="3ed60-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="3ed60-220">Řekli, která ve výchozím nastavení, klíčová vlastnost, která je celé číslo se stanou klíč identity v databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="3ed60-221">Která budou stejné jako nastavení DatabaseGenerated DatabaseGeneratedOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="3ed60-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="3ed60-222">Pokud nechcete, aby to přijde klíč identity, můžete nastavit hodnotu na DatabaseGeneratedOption.None.</span><span class="sxs-lookup"><span data-stu-id="3ed60-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="3ed60-223">Index</span><span class="sxs-lookup"><span data-stu-id="3ed60-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="3ed60-224">**EF6.1 a vyšší pouze** – atribut indexu byla zavedena v Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="3ed60-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="3ed60-225">Pokud používáte starší verzi informace v této části se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="3ed60-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="3ed60-226">Můžete vytvořit index na jeden nebo více sloupců pomocí **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="3ed60-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="3ed60-227">Přidání atributu na jeden nebo více vlastností se způsobit EF k vytvoření odpovídající index v databázi při vytváření databáze, nebo generování uživatelského rozhraní k odpovídající položce **CreateIndex** zavolá, pokud používáte migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="3ed60-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="3ed60-228">Například následující kód způsobí indexu vytvářen **hodnocení** sloupec **příspěvky** tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="3ed60-229">Ve výchozím nastavení, bude mít název indexu **IX\_&lt;název vlastnosti&gt;**  (IX\_hodnocení v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="3ed60-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="3ed60-230">Můžete také zadat název pro index ale.</span><span class="sxs-lookup"><span data-stu-id="3ed60-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="3ed60-231">Následující příklad určuje, že index s názvem **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="3ed60-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="3ed60-232">Indexy jsou ve výchozím nastavení, není jedinečný, ale můžete použít **IsUnique** s názvem parametru k určení, že index musí být jedinečné.</span><span class="sxs-lookup"><span data-stu-id="3ed60-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="3ed60-233">Následující příklad představuje jedinečný index na **uživatele**pro přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="3ed60-233">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="3ed60-234">Sloupec indexů</span><span class="sxs-lookup"><span data-stu-id="3ed60-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="3ed60-235">Indexy, které přesahují do více sloupců je určené vlastností se stejným názvem ve více poznámek Index pro danou tabulku.</span><span class="sxs-lookup"><span data-stu-id="3ed60-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="3ed60-236">Při vytváření indexů více sloupci, je třeba zadat pořadí sloupců v indexu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="3ed60-237">Například následující kód vytvoří index více sloupců na **hodnocení** a **BlogId** volá **IX\_BlogIdAndRating**.</span><span class="sxs-lookup"><span data-stu-id="3ed60-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="3ed60-238">**BlogId** je první sloupec v indexu a **hodnocení** je druhý.</span><span class="sxs-lookup"><span data-stu-id="3ed60-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="3ed60-239">Relace atributů: InverseProperty a cizí klíč</span><span class="sxs-lookup"><span data-stu-id="3ed60-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="3ed60-240">Tato stránka obsahuje informace o nastavení relace v modelu Code First pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="3ed60-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="3ed60-241">Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="3ed60-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="3ed60-242">První konvence kódu se postará o nejběžnějších relace v modelu, ale existují případy, kdy je potřebuje pomoc.</span><span class="sxs-lookup"><span data-stu-id="3ed60-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="3ed60-243">Změna názvu klíčová vlastnost ve třídě blogu vytvoří problém s jeho vztah k příspěvku.</span><span class="sxs-lookup"><span data-stu-id="3ed60-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="3ed60-244">Při generování databáze, kód nejprve uvidí BlogId vlastnost ve třídě Post a rozpozná, podle konvence, že se shoduje názvem třídy plus "Id", jako cizí klíč třídy blogu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="3ed60-245">Ale neexistuje žádná vlastnost BlogId ve třídě blogu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="3ed60-246">Řešení pro tento je vytvořit vlastnost navigace v příspěvku a používat cizí DataAnnotation ke kódu nejprve porozumět postupu při vytvoření vztahu mezi dvěma třídami – pomocí vlastnosti Post.BlogId – jak se dá zadat omezení databáze.</span><span class="sxs-lookup"><span data-stu-id="3ed60-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="3ed60-247">Omezení v databázi znázorňuje relaci mezi InternalBlogs.PrimaryTrackingKey a Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="3ed60-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![vztah mezi InternalBlogs.PrimaryTrackingKey a Posts.BlogId](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="3ed60-249">InverseProperty se používá v případě, že máte více vztahů mezi třídami.</span><span class="sxs-lookup"><span data-stu-id="3ed60-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="3ed60-250">Ve třídě příspěvek můžete chtít sledovat, který napsal příspěvek na blog a také ji upravila.</span><span class="sxs-lookup"><span data-stu-id="3ed60-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="3ed60-251">Tady jsou dvě nové navigační vlastnosti pro třídu příspěvku.</span><span class="sxs-lookup"><span data-stu-id="3ed60-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="3ed60-252">Také budete muset přidat v třídě osoba odkazuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="3ed60-253">Třída osoba má vlastnosti navigace zpět na příspěvek, jeden pro všechny příspěvky autorem osoby a jeden pro všechny příspěvky aktualizoval tuto osobu.</span><span class="sxs-lookup"><span data-stu-id="3ed60-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="3ed60-254">Kód nejprve není schopen odpovídat vlastnosti ve dvou tříd sama o sobě.</span><span class="sxs-lookup"><span data-stu-id="3ed60-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="3ed60-255">Databázové tabulky pro příspěvky měli mít jeden cizí klíč pro osoby CreatedBy a jeden pro osobu, UpdatedBy ale kód nejprve vytvoří čtyři vlastnosti cizího klíče: Osoba\_Id, osoba\_Id1, CreatedBy\_Id a UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="3ed60-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Příspěvky tabulku s velmi cizí klíče](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="3ed60-257">Chcete-li vyřešit tyto problémy, můžete použít poznámku InverseProperty k určení zarovnání vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ed60-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="3ed60-258">Protože vlastnost PostsWritten osobně ví, že to se vztahuje na typu příspěvku, vytvoří vztah Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="3ed60-259">PostsUpdated podobně připojí Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="3ed60-260">A kód nejprve nevytvoří velmi cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="3ed60-260">And code first will not create the extra foreign keys.</span></span>

![Tabulka příspěvky bez dalších cizí klíče](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="3ed60-262">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3ed60-262">Summary</span></span>

<span data-ttu-id="3ed60-263">DataAnnotations nejen umožňují popsat ověřování na straně klienta a serveru ve třídách první kódu, ale také umožňují vylepšit a dokonce i opravit předpoklady, které kód nejprve vytvoří o třídách podle jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="3ed60-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="3ed60-264">S DataAnnotations nelze jenom jednotky generování schématu databáze, ale můžete také namapovat první třídy kódu k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="3ed60-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="3ed60-265">Když jsou flexibilní, mějte na paměti, poskytující DataAnnotations pouze většinu běžně potřebné změny konfigurace provedené na váš kód první třídy.</span><span class="sxs-lookup"><span data-stu-id="3ed60-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="3ed60-266">Ke konfiguraci třídy pro některý ze hraniční případy, by měl vypadat mechanismu, který alternativní konfiguraci, Code First rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="3ed60-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
