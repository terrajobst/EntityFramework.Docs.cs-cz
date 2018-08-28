---
title: Kód anotací dat při prvním - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 0ab66afa3babafe657b3ddb32c02c3fba0ae310e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994583"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="7eb6f-102">Kód první datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7eb6f-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="7eb6f-103">**EF4.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="7eb6f-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="7eb6f-105">Obsah na této stránce jsou upraveny z a článek původně vydané společností Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="7eb6f-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="7eb6f-106">Entity Framework Code First umožňuje používat vaše vlastní třídy domény k vyjádření modelu, které EF spoléhá na k provedení dotazu, změňte sledování a aktualizuje se funkce.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="7eb6f-107">Kód nejprve využívá programovací model označovány jako konvence nad konfigurací.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="7eb6f-108">To znamená, že kód nejprve bude předpokládat, že třídy dodržují konvence, které EF používá.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="7eb6f-109">V takovém případě EF budou moct pracovat podrobnosti ji potřebuje pro výkon své práce.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="7eb6f-110">Ale pokud tříd nepostupujte podle těchto konvence, máte možnost přidání konfigurace do vaší třídy, které poskytují EF s informacemi, které potřebuje.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="7eb6f-111">Kód nejprve poskytuje dva způsoby, jak tyto konfigurace přidat do vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="7eb6f-112">Jeden používá jednoduché atributy, které volá DataAnnotations a druhý je použití kódu nejprve je rozhraní Fluent API, která vám poskytuje způsob, jak popisují konfiguraci toho, v kódu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="7eb6f-113">Tento článek se zaměří na používání DataAnnotations (v oboru názvů System.ComponentModel.DataAnnotations) ke konfiguraci třídy – zvýraznění nejčastěji potřebných konfigurací.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="7eb6f-114">DataAnnotations také rozumí počet aplikací .NET, jako je například technologie ASP.NET MVC, která umožňuje tyto aplikace využívat stejné poznámky k ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="7eb6f-115">Model</span><span class="sxs-lookup"><span data-stu-id="7eb6f-115">The model</span></span>

<span data-ttu-id="7eb6f-116">Vám předvedu kódu první DataAnnotations pomocí jednoduchého páru tříd: Blog a příspěvku.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="7eb6f-117">Jsou, blogu a příspěvku třídy pohodlně postupujte podle úmluvy první kódu a vyžaduje žádné vylepšení usnadňují práci s nimi EF.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="7eb6f-118">Ale můžete také použít poznámky vám poskytneme Další informace o třídách a databáze, které jsou mapovány na EF.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="7eb6f-119">Key</span><span class="sxs-lookup"><span data-stu-id="7eb6f-119">Key</span></span>

<span data-ttu-id="7eb6f-120">Entity Framework spoléhá na každé entitě s hodnotou klíče, který se používá ke sledování entity.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="7eb6f-121">Jednu z konvencí kód nejprve, na kterých závisí je, jak vyplývá vlastností, které se klíč v každém z první třídy kódu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="7eb6f-122">Úmluvy je pro vlastnost s názvem "Id" nebo disk, který kombinuje název třídy a "Id", jako je například "BlogId".</span><span class="sxs-lookup"><span data-stu-id="7eb6f-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="7eb6f-123">Vlastnost bude mapovat na sloupec primárního klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="7eb6f-124">Třídy blogu a účtovat podle Tato konvence.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="7eb6f-125">Ale co když se nepovedlo?</span><span class="sxs-lookup"><span data-stu-id="7eb6f-125">But what if they didn’t?</span></span> <span data-ttu-id="7eb6f-126">Co když blogu použít název *PrimaryTrackingKey* místo nebo dokonce *foo*?</span><span class="sxs-lookup"><span data-stu-id="7eb6f-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="7eb6f-127">Pokud kód nejprve nenajde vlastnost, která odpovídá tato konvence vyvolá výjimku z důvodu požadavku Entity Framework, že musí mít vlastnost klíče.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="7eb6f-128">Poznámka klíče můžete použít k určení vlastností, které se má použít jako EntityKey.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="7eb6f-129">Pokud jste nejdřív pomocí kódu je funkce generování databáze, blogu tabulka bude obsahovat sloupec primárního klíče s názvem PrimaryTrackingKey, který je definovaný i jako identitu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="7eb6f-131">Složené klíče</span><span class="sxs-lookup"><span data-stu-id="7eb6f-131">Composite keys</span></span>

<span data-ttu-id="7eb6f-132">Entity Framework podporuje složené klíče – primární klíče, které se skládají z více než jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="7eb6f-133">Například vašeho účtu služby Passport třídy, jejichž primární klíč je kombinací PassportNumber a IssuingCountry může mít.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="7eb6f-134">Pokud byste chtěli akci a použijte výše uvedené třídu ve vašem modelu EF, které by zobrazí zpráva InvalidOperationExceptions;</span><span class="sxs-lookup"><span data-stu-id="7eb6f-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="7eb6f-135">*Nepovedlo se určit složený primární klíče řazení pro typ "Passport". Použijte ColumnAttribute nebo metodu HasKey k určení pořadí pro složený primární klíče.*</span><span class="sxs-lookup"><span data-stu-id="7eb6f-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="7eb6f-136">Až budete mít složených klíčů, Entity Framework vyžaduje definování objednávku klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="7eb6f-137">Můžete udělat pomocí sloupce Poznámka k určení pořadí.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="7eb6f-138">Hodnota pořadí je relativní (a ne na základě indexu), můžete použít všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="7eb6f-139">Například 100 až 200 bude přijatelné namísto 1 a 2.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="7eb6f-140">Pokud máte složené cizího klíče entity, které musíte zadat stejný sloupec řazení, který jste použili pro odpovídající vlastnosti primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="7eb6f-141">Pouze relativní řazení v rámci vlastnosti cizího klíče musí být stejný přesné hodnoty přiřazené k **pořadí** nemusí odpovídat.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="7eb6f-142">Například ve třídě následující 3 a 4 by mohla být zastoupen 1 a 2.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="7eb6f-143">Požadováno</span><span class="sxs-lookup"><span data-stu-id="7eb6f-143">Required</span></span>

<span data-ttu-id="7eb6f-144">Poznámka vyžaduje říká EF vyžádáním určité vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="7eb6f-145">Přidání vyžaduje vlastnost názvu vynutí EF (a MVC) ujistěte se, že vlastnost obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="7eb6f-146">Žádné další bez změny kódu nebo značek v aplikaci, aplikace MVC provede ověřování na straně klienta, dokonce i dynamické vytvoření zprávu pomocí názvy vlastností a poznámek.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="7eb6f-148">Požadovaný atribut ovlivní také generován databázový tím, že namapovanou vlastnost Null.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="7eb6f-149">Všimněte si, že pole název se změnil na "not null".</span><span class="sxs-lookup"><span data-stu-id="7eb6f-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="7eb6f-150">V některých případech nemusí být možné pro sloupec v databázi být null, i když tato vlastnost je vyžadovaná.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="7eb6f-151">Například při použití dat TPH dědičnosti strategie pro více typů je uložené v jediné tabulce.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="7eb6f-152">Pokud odvozený typ obsahuje požadovanou vlastnost sloupec nelze nastavit jako Null protože ne všechny typy v hierarchii, bude mít tato vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="7eb6f-154">MaxLength a MinLength</span><span class="sxs-lookup"><span data-stu-id="7eb6f-154">MaxLength and MinLength</span></span>

<span data-ttu-id="7eb6f-155">Atributy MaxLength a MinLength umožňují zadat další vlastnosti ověření, stejně jako u požadované.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="7eb6f-156">Tady je BloggerName s požadavky na délku.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="7eb6f-157">Tento příklad také ukazuje, jak kombinovat atributy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="7eb6f-158">Poznámka MaxLength ovlivní databázi nastavením vlastnosti length na 10.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="7eb6f-160">MVC na straně klienta poznámky a anotace EF 4.1 na straně serveru i dodrží toto ověření znovu dynamické vytvoření chybová zpráva: "pole BloggerName musí být typu řetězce nebo pole s maximální délkou"10"." Tato zpráva je trochu dlouhý.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="7eb6f-161">Mnoho anotace umožňují zadat chybovou zprávu s atributem chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="7eb6f-162">Můžete také zadat chybová zpráva v poznámce požadované.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="7eb6f-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="7eb6f-164">NotMapped</span></span>

<span data-ttu-id="7eb6f-165">První konvence kódu určí, že všech vlastností, které je podporované datového typu je reprezentován v databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="7eb6f-166">Ale to není vždy případ ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="7eb6f-167">Například může mít vlastnost ve třídě blogu, který vytvoří kódu na základě názvu a BloggerName polí.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="7eb6f-168">Tato vlastnost se dají vytvářet dynamicky a není potřeba ukládat.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="7eb6f-169">Můžete označit všechny vlastnosti, které se nemapují na databázi s poznámkou NotMapped například tuto vlastnost BlogCode.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="7eb6f-170">Typ ComplexType</span><span class="sxs-lookup"><span data-stu-id="7eb6f-170">ComplexType</span></span>

<span data-ttu-id="7eb6f-171">Není k popisu entity domény mezi sadu tříd a potom vrstvy těchto tříd k popisu úplnou entitu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="7eb6f-172">Můžete například přidat třídu s názvem BlogDetails do modelu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="7eb6f-173">Všimněte si, že BlogDetails nemá libovolného typu klíčovou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="7eb6f-174">V návrhu na základě domény BlogDetails označuje jako objekt hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="7eb6f-175">Entity Framework odkazují na objekty hodnotu jako komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="7eb6f-176">Komplexní typy nelze sledovat na své vlastní.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="7eb6f-177">Nicméně jako vlastnost ve třídě blogu BlogDetails ho budou sledovány jako součást objektu blogu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="7eb6f-178">Aby code first pro to rozpoznat je třeba označit třídu BlogDetails jako element ComplexType.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="7eb6f-179">Nyní můžete přidat vlastnost ve třídě blogu k reprezentaci BlogDetails pro tento blog.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="7eb6f-180">V databázi v blogu tabulce bude obsahovat všechny vlastnosti včetně vlastnosti obsažené v jeho vlastnost BlogDetail blogu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="7eb6f-181">Ve výchozím nastavení každý z nich je před názvem komplexní typ, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="7eb6f-183">Další zajímavé Poznámka je sice DateCreated vlastnost byla definována jako neumožňující hodnotu data a času ve třídě, pole příslušnou databázi s povolenou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="7eb6f-184">Pokud chcete mít vliv na schéma databáze, je nutné použít požadované poznámky.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="7eb6f-185">Atribut ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="7eb6f-185">ConcurrencyCheck</span></span>

<span data-ttu-id="7eb6f-186">Poznámka atribut ConcurrencyCheck umožňuje označit jednu nebo více vlastností, které má být použit pro souběžnost kontroly v databázi, když uživatel upraví nebo odstraní entitu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="7eb6f-187">Pokud jste pracovali s EF designeru, ten je v souladu s nastavení vlastnosti režim ConcurrencyMode na pevný.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="7eb6f-188">Podívejme se, jak atribut ConcurrencyCheck funguje tak, že přidáte BloggerName vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="7eb6f-189">Při volání SaveChanges, protože atribut ConcurrencyCheck Poznámka u pole BloggerName původní hodnota dané vlastnosti se použije v aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="7eb6f-190">Příkaz se pokusí najít správný řádek pomocí filtrování pouze na hodnotě klíče, ale také u původní hodnoty BloggerName.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="7eb6f-191">Tady jsou důležité části příkazu UPDATE odeslán do databáze, ve kterém uvidíte příkaz aktualizuje řádek, který má PrimaryTrackingKey je 1 a BloggerName z "Julie", který byl původní hodnotu při tomto blogu byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="7eb6f-192">Pokud do té doby někdo bloggeru název pro tento blog, tato aktualizace se nezdaří a získáte DbUpdateConcurrencyException, které budete potřebovat pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="7eb6f-193">Časové razítko</span><span class="sxs-lookup"><span data-stu-id="7eb6f-193">TimeStamp</span></span>

<span data-ttu-id="7eb6f-194">Je běžné použití pole rowversion nebo časového razítka pro kontrolu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="7eb6f-195">Ale místo použití atribut ConcurrencyCheck poznámky, můžete použít konkrétnější anotaci časového razítka jako typ vlastnosti je bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="7eb6f-196">Kód nejprve bude považovat za vlastnosti časového razítka stejný atribut ConcurrencyCheck vlastnosti, ale také pomohou zajistit, že pole databáze, které kód nejprve vytvoří hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="7eb6f-197">Máte jenom jednu vlastnost časového razítka v dané třídě.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="7eb6f-198">Přidávání do třídy blogu následující vlastnost:</span><span class="sxs-lookup"><span data-stu-id="7eb6f-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="7eb6f-199">výsledky v kódu nejprve vytvořit sloupec časového razítka Null v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="7eb6f-201">Tabulky a sloupce</span><span class="sxs-lookup"><span data-stu-id="7eb6f-201">Table and Column</span></span>

<span data-ttu-id="7eb6f-202">Pokud umožníte tím Code First vytvořit databázi, můžete změnit název tabulky a sloupce, které se vytváří.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="7eb6f-203">Code First můžete použít také k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="7eb6f-204">Ale to není vždy případ, že názvy tříd a vlastností ve vaší doméně shodovat s názvy tabulek a sloupců ve vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="7eb6f-205">Moje třída se nazývá blogu a podle konvence kódu nejprve předpokládá, že to se namapuje na tabulku s názvem blogy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="7eb6f-206">Pokud se nejedná o tento případ můžete zadat název tabulky s atributem tabulky.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="7eb6f-207">Zde například Poznámka je určení, že název tabulky je InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="7eb6f-208">Poznámka sloupec je další nimiž v určeném atributy pro mapovanou sloupec.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="7eb6f-209">Můžete stanovit, název, datový typ nebo dokonce pořadí, ve kterém se zobrazí sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="7eb6f-210">Tady je příklad sloupce atributu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="7eb6f-211">Nepleťte si Atribut TypeName sloupce s DataAnnotation datového typu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="7eb6f-212">Datový typ se má poznámka použít pro uživatelské rozhraní a je ignorován Code First.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="7eb6f-213">Tady je tabulka po je byly znovu vygenerovány.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="7eb6f-214">InternalBlogs změnil název tabulky a sloupce s popisem od komplexní typ je nyní BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="7eb6f-215">Protože v poznámce byl zadán název, kód nejprve nebudeme používat konvenci od názvu sloupce s názvem komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="7eb6f-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="7eb6f-217">DatabaseGenerated</span></span>

<span data-ttu-id="7eb6f-218">Důležité databázové funkce je schopnost mít vypočítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="7eb6f-219">Pokud máte mapování Code First tříd do tabulek, které obsahují vypočítané sloupce, nechcete, aby rozhraní Entity Framework a pokuste se aktualizovat tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="7eb6f-220">Ale být vhodné EF vracet tyto hodnoty z databáze po vložení nebo aktualizovaná data.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="7eb6f-221">Poznámka DatabaseGenerated můžete označit, že tyto vlastnosti ve třídě spolu s vypočítané výčtu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="7eb6f-222">Jiných výčtech k žádným nedošlo a Identity.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="7eb6f-223">Můžete použít databáze, které jsou generovány pro sloupce bajtů nebo jako časové razítko, pokud kód je nejprve generování databáze, v opačném případě byste měli používat jenom to při odkazující na stávající databáze, protože kód nejprve nebude schopna určit vzorec pro počítaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="7eb6f-224">Řekli, která ve výchozím nastavení, klíčová vlastnost, která je celé číslo se stanou klíč identity v databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="7eb6f-225">Která budou stejné jako nastavení DatabaseGenerated DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="7eb6f-226">Pokud nechcete, aby to přijde klíč identity, můžete nastavit hodnotu na DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="7eb6f-227">Index</span><span class="sxs-lookup"><span data-stu-id="7eb6f-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="7eb6f-228">**EF6.1 a vyšší pouze** – atribut indexu byla zavedena v Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="7eb6f-229">Pokud používáte starší verzi informace v této části se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="7eb6f-230">Můžete vytvořit index na jeden nebo více sloupců pomocí **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="7eb6f-231">Přidání atributu na jeden nebo více vlastností se způsobit EF k vytvoření odpovídající index v databázi při vytváření databáze, nebo generování uživatelského rozhraní k odpovídající položce **CreateIndex** zavolá, pokud používáte migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="7eb6f-232">Například následující kód způsobí indexu vytvářen **hodnocení** sloupec **příspěvky** tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="7eb6f-233">Ve výchozím nastavení, bude mít název indexu **IX\_&lt;název vlastnosti&gt;**  (IX\_hodnocení v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="7eb6f-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="7eb6f-234">Můžete také zadat název pro index ale.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="7eb6f-235">Následující příklad určuje, že index s názvem **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="7eb6f-236">Indexy jsou ve výchozím nastavení, není jedinečný, ale můžete použít **IsUnique** s názvem parametru k určení, že index musí být jedinečné.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="7eb6f-237">Následující příklad představuje jedinečný index na **uživatele**pro přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-237">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="7eb6f-238">Sloupec indexů</span><span class="sxs-lookup"><span data-stu-id="7eb6f-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="7eb6f-239">Indexy, které přesahují do více sloupců je určené vlastností se stejným názvem ve více poznámek Index pro danou tabulku.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="7eb6f-240">Při vytváření indexů více sloupci, je třeba zadat pořadí sloupců v indexu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="7eb6f-241">Například následující kód vytvoří index více sloupců na **hodnocení** a **BlogId** volá **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="7eb6f-242">**BlogId** je první sloupec v indexu a **hodnocení** je druhý.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="7eb6f-243">Relace atributů: InverseProperty a cizí klíč</span><span class="sxs-lookup"><span data-stu-id="7eb6f-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="7eb6f-244">Tato stránka obsahuje informace o nastavení relace v modelu Code First pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="7eb6f-245">Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="7eb6f-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="7eb6f-246">První konvence kódu se postará o nejběžnějších relace v modelu, ale existují případy, kdy je potřebuje pomoc.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="7eb6f-247">Změna názvu klíčová vlastnost ve třídě blogu vytvoří problém s jeho vztah k příspěvku.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="7eb6f-248">Při generování databáze, kód nejprve uvidí BlogId vlastnost ve třídě Post a rozpozná, podle konvence, že se shoduje názvem třídy plus "Id", jako cizí klíč třídy blogu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="7eb6f-249">Ale neexistuje žádná vlastnost BlogId ve třídě blogu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="7eb6f-250">Řešení pro tento je vytvořit vlastnost navigace v příspěvku a používat cizí DataAnnotation ke kódu nejprve porozumět postupu při vytvoření vztahu mezi dvěma třídami – pomocí vlastnosti Post.BlogId – jak se dá zadat omezení databáze.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="7eb6f-251">Omezení v databázi znázorňuje relaci mezi InternalBlogs.PrimaryTrackingKey a Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="7eb6f-253">InverseProperty se používá v případě, že máte více vztahů mezi třídami.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="7eb6f-254">Ve třídě příspěvek můžete chtít sledovat, který napsal příspěvek na blog a také ji upravila.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="7eb6f-255">Tady jsou dvě nové navigační vlastnosti pro třídu příspěvku.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="7eb6f-256">Také budete muset přidat v třídě osoba odkazuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="7eb6f-257">Třída osoba má vlastnosti navigace zpět na příspěvek, jeden pro všechny příspěvky autorem osoby a jeden pro všechny příspěvky aktualizoval tuto osobu.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="7eb6f-258">Kód nejprve není schopen odpovídat vlastnosti ve dvou tříd sama o sobě.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="7eb6f-259">Databázové tabulky pro příspěvky by měl mít jeden cizí klíč pro osobu, CreatedBy a jeden pro osoby UpdatedBy ale kód nejprve vytvoří čtyři se vlastnosti cizího klíče: osoba\_Id, osoba\_Id1, CreatedBy\_Id a UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="7eb6f-261">Chcete-li vyřešit tyto problémy, můžete použít poznámku InverseProperty k určení zarovnání vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="7eb6f-262">Protože vlastnost PostsWritten osobně ví, že to se vztahuje na typu příspěvku, vytvoří vztah Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="7eb6f-263">PostsUpdated podobně připojí Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="7eb6f-264">A kód nejprve nevytvoří velmi cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="7eb6f-266">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7eb6f-266">Summary</span></span>

<span data-ttu-id="7eb6f-267">DataAnnotations nejen umožňují popsat ověřování na straně klienta a serveru ve třídách první kódu, ale také umožňují vylepšit a dokonce i opravit předpoklady, které kód nejprve vytvoří o třídách podle jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="7eb6f-268">S DataAnnotations nelze jenom jednotky generování schématu databáze, ale můžete také namapovat první třídy kódu k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="7eb6f-269">Když jsou flexibilní, mějte na paměti, poskytující DataAnnotations pouze většinu běžně potřebné změny konfigurace provedené na váš kód první třídy.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="7eb6f-270">Ke konfiguraci třídy pro některý ze hraniční případy, by měl vypadat mechanismu, který alternativní konfiguraci, Code First rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="7eb6f-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
