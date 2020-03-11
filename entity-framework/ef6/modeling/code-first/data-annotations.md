---
title: Code First datové poznámky – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419183"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="7b907-102">Code First datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7b907-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="7b907-103">**EF 4.1 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="7b907-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="7b907-104">Pokud používáte starší verzi, některé nebo všechny tyto informace se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="7b907-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="7b907-105">Obsah na této stránce je přizpůsoben z článku, který původně napsal Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="7b907-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="7b907-106">Entity Framework Code First umožňuje použít vlastní třídy domény k reprezentaci modelu, na kterém se EF spoléhá, aby prováděl dotazování, sledování změn a aktualizaci funkcí.</span><span class="sxs-lookup"><span data-stu-id="7b907-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="7b907-107">Code First využívá programovací model, který je označován jako "konvence před konfigurací".</span><span class="sxs-lookup"><span data-stu-id="7b907-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="7b907-108">Code First předpokládá, že vaše třídy dodržují konvence Entity Framework a v takovém případě bude automaticky fungovat, jak provést úlohu.</span><span class="sxs-lookup"><span data-stu-id="7b907-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform its job.</span></span> <span data-ttu-id="7b907-109">Nicméně pokud vaše třídy nedodržují tyto konvence, máte možnost Přidat konfigurace do tříd a poskytnout EF základní informace o požadavcích.</span><span class="sxs-lookup"><span data-stu-id="7b907-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="7b907-110">Code First poskytuje dva způsoby, jak přidat tyto konfigurace do tříd.</span><span class="sxs-lookup"><span data-stu-id="7b907-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="7b907-111">Jedna z nich používá jednoduché atributy s názvem DataAnnotations a druhá používá Code First rozhraní API Fluent, které poskytuje způsob, jak v kódu imperativně popsat konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7b907-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="7b907-112">Tento článek se zaměří na použití DataAnnotations (v oboru názvů System. ComponentModel. DataAnnotations) ke konfiguraci tříd – zvýrazňování nejčastěji potřebných konfigurací.</span><span class="sxs-lookup"><span data-stu-id="7b907-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="7b907-113">K dataannotationům se rozumí také řada aplikací .NET, jako je ASP.NET MVC, což umožňuje těmto aplikacím využívat stejné poznámky pro ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="7b907-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="7b907-114">Model</span><span class="sxs-lookup"><span data-stu-id="7b907-114">The model</span></span>

<span data-ttu-id="7b907-115">Ukážeme Code First dataanotace pomocí jednoduché dvojice tříd: blog a příspěvek.</span><span class="sxs-lookup"><span data-stu-id="7b907-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="7b907-116">V takovém případě se třídy blogů a post vhodně řídí konvencí Code First a nevyžadují žádné vylepšení pro povolení kompatibility EF.</span><span class="sxs-lookup"><span data-stu-id="7b907-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="7b907-117">Poznámky však můžete použít také k poskytnutí dalších informací k EF o třídách a databázi, ke které jsou mapovány.</span><span class="sxs-lookup"><span data-stu-id="7b907-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="7b907-118">Klíč</span><span class="sxs-lookup"><span data-stu-id="7b907-118">Key</span></span>

<span data-ttu-id="7b907-119">Entity Framework spoléhá na každou entitu, která má hodnotu klíče, která se používá ke sledování entit.</span><span class="sxs-lookup"><span data-stu-id="7b907-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="7b907-120">Jedna konvence Code First je implicitní vlastnosti klíče; Code First bude hledat vlastnost s názvem "ID" nebo kombinaci názvu třídy a "ID", například "BlogId".</span><span class="sxs-lookup"><span data-stu-id="7b907-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="7b907-121">Tato vlastnost bude namapována na sloupec primárního klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="7b907-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="7b907-122">Blogové a poštovní třídy se budou řídit touto konvencí.</span><span class="sxs-lookup"><span data-stu-id="7b907-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="7b907-123">Co když neudělal?</span><span class="sxs-lookup"><span data-stu-id="7b907-123">What if they didn’t?</span></span> <span data-ttu-id="7b907-124">Co když blog místo toho použil název *PrimaryTrackingKey* nebo dokonce i *foo*?</span><span class="sxs-lookup"><span data-stu-id="7b907-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="7b907-125">Pokud kód nenalezne vlastnost, která odpovídá této konvenci, vyvolá výjimku z důvodu požadavku Entity Framework, že musíte mít vlastnost Key.</span><span class="sxs-lookup"><span data-stu-id="7b907-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="7b907-126">Klíčovou poznámku můžete použít k určení, která vlastnost se má použít jako EntityKey.</span><span class="sxs-lookup"><span data-stu-id="7b907-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="7b907-127">Pokud používáte funkci generování databáze prvního kódu, tabulka blogu bude mít sloupec primárního klíče s názvem PrimaryTrackingKey, který je ve výchozím nastavení definován jako identita.</span><span class="sxs-lookup"><span data-stu-id="7b907-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Tabulka blogu s primárním klíčem](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="7b907-129">Složené klíče</span><span class="sxs-lookup"><span data-stu-id="7b907-129">Composite keys</span></span>

<span data-ttu-id="7b907-130">Entity Framework podporuje složené klíče – primární klíče, které se skládají z více než jedné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b907-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="7b907-131">Například můžete mít třídu služby Passport, jejíž primární klíč je kombinací hodnot PassportNumber a IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="7b907-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="7b907-132">Pokus o použití výše uvedené třídy v modelu EF má za následek `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="7b907-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="7b907-133">*Nelze určit řazení složených primárních klíčů pro typ Passport. Použijte metodu ColumnAttribute nebo Haskey – k určení objednávky složených primárních klíčů.*</span><span class="sxs-lookup"><span data-stu-id="7b907-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="7b907-134">Aby bylo možné používat složené klíče, Entity Framework vyžaduje, abyste definovali pořadí vlastností klíče.</span><span class="sxs-lookup"><span data-stu-id="7b907-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="7b907-135">Můžete to provést pomocí anotace sloupce a určit objednávku.</span><span class="sxs-lookup"><span data-stu-id="7b907-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="7b907-136">Hodnota Order je relativní (spíše než index založený na indexu), takže lze použít jakékoli hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7b907-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="7b907-137">Například 100 a 200 by byly přijatelné místo 1 a 2.</span><span class="sxs-lookup"><span data-stu-id="7b907-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="7b907-138">Pokud máte entity se složenými cizími klíči, je nutné zadat stejné řazení sloupců, které jste použili pro příslušné vlastnosti primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="7b907-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="7b907-139">Pouze relativní řazení v rámci vlastností cizích klíčů musí být stejné, přesné hodnoty přiřazené k **objednávce** nemusejí odpovídat.</span><span class="sxs-lookup"><span data-stu-id="7b907-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="7b907-140">Například v následujících třídách lze použít místo 1 a 2 na úrovni 3 a 4.</span><span class="sxs-lookup"><span data-stu-id="7b907-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="7b907-141">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7b907-141">Required</span></span>

<span data-ttu-id="7b907-142">Požadovaná Poznámka oznamuje EF, že je požadována konkrétní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7b907-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="7b907-143">Přidání vyžadované pro vlastnost title vynutí EF (a MVC), aby bylo zajištěno, že vlastnost obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="7b907-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="7b907-144">Aplikace MVC provede ověřování na straně klienta, a to i v případě, že v aplikaci nejsou žádné další změny kódu nebo kódu, a to i při dynamickém vytváření zprávy pomocí názvů vlastností a poznámek.</span><span class="sxs-lookup"><span data-stu-id="7b907-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Při vytváření stránky s nadpisem se vyžaduje chyba.](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="7b907-146">Požadovaný atribut bude mít vliv také na vygenerovanou databázi tím, že namapuje vlastnost, která není null.</span><span class="sxs-lookup"><span data-stu-id="7b907-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="7b907-147">Všimněte si, že pole název se změnilo na hodnotu not null.</span><span class="sxs-lookup"><span data-stu-id="7b907-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="7b907-148">V některých případech nemusí být možné, aby sloupec v databázi mohl mít hodnotu null, i když je požadovaná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7b907-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="7b907-149">Pokud například použijete strategii dědičnosti TPH pro více typů, uloží se do jedné tabulky.</span><span class="sxs-lookup"><span data-stu-id="7b907-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="7b907-150">Pokud odvozený typ obsahuje povinnou vlastnost, sloupec nemůže být nastaven na hodnotu null, protože ne všechny typy v hierarchii budou mít tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7b907-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Tabulka blogů](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="7b907-152">MaxLength a MinLength</span><span class="sxs-lookup"><span data-stu-id="7b907-152">MaxLength and MinLength</span></span>

<span data-ttu-id="7b907-153">Atributy MaxLength a MinLength umožňují zadat další ověření vlastností, stejně jako u požadovaných.</span><span class="sxs-lookup"><span data-stu-id="7b907-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="7b907-154">Tady je požadavek blogger s požadavky na délku.</span><span class="sxs-lookup"><span data-stu-id="7b907-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="7b907-155">Příklad také ukazuje, jak kombinovat atributy.</span><span class="sxs-lookup"><span data-stu-id="7b907-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="7b907-156">Poznámka MaxLength bude mít vliv na databázi nastavením délky vlastnosti na hodnotu 10.</span><span class="sxs-lookup"><span data-stu-id="7b907-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Tabulka blogů zobrazující maximální délku sloupce blogger](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="7b907-158">Poznámka na straně klienta MVC a Poznámka k serveru EF 4,1 na straně serveru budou respektovat toto ověření, znovu dynamicky sestavit chybovou zprávu: "pole blogger musí být řetězec nebo typ pole s maximální délkou 10." Tato zpráva je trochu dlouhá.</span><span class="sxs-lookup"><span data-stu-id="7b907-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="7b907-159">Mnoho poznámek vám umožní zadat chybovou zprávu s atributem ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="7b907-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="7b907-160">V požadované anotaci můžete také zadat ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="7b907-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Vytvořit stránku s vlastní chybovou zprávou](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="7b907-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="7b907-162">NotMapped</span></span>

<span data-ttu-id="7b907-163">První konvence kódu určuje, že všechny vlastnosti, které jsou podporovaným datovým typem, jsou v databázi reprezentovány.</span><span class="sxs-lookup"><span data-stu-id="7b907-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="7b907-164">Nejedná se ale vždy o případ ve vašich aplikacích.</span><span class="sxs-lookup"><span data-stu-id="7b907-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="7b907-165">Můžete mít například vlastnost ve třídě blog, která vytvoří kód založený na názvu a poli blogger.</span><span class="sxs-lookup"><span data-stu-id="7b907-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="7b907-166">Tuto vlastnost lze vytvořit dynamicky a není nutné ji ukládat.</span><span class="sxs-lookup"><span data-stu-id="7b907-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="7b907-167">Můžete označit všechny vlastnosti, které se mapují k databázi, pomocí anotace NotMapped, jako je tato vlastnost BlogCode.</span><span class="sxs-lookup"><span data-stu-id="7b907-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="7b907-168">Elementu</span><span class="sxs-lookup"><span data-stu-id="7b907-168">ComplexType</span></span>

<span data-ttu-id="7b907-169">Není neobvyklé popsání doménových entit v rámci sady tříd a následnou vrstvou těchto tříd, abyste popsali úplnou entitu.</span><span class="sxs-lookup"><span data-stu-id="7b907-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="7b907-170">Do modelu můžete například přidat třídu s názvem BlogDetails.</span><span class="sxs-lookup"><span data-stu-id="7b907-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="7b907-171">Všimněte si, že BlogDetails nemá žádný typ klíčové vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b907-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="7b907-172">V návrhu založeném na doméně se BlogDetails označuje jako objekt hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7b907-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="7b907-173">Entity Framework odkazuje na objekty hodnot jako komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="7b907-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="7b907-174">  Komplexní typy nelze sledovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="7b907-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="7b907-175">Nicméně jako vlastnost ve třídě blogu BlogDetails bude sledována jako součást objektu blogu.</span><span class="sxs-lookup"><span data-stu-id="7b907-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="7b907-176">Aby kód nejprve rozpoznal, je nutné označit třídu BlogDetails jako ComplexType.</span><span class="sxs-lookup"><span data-stu-id="7b907-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="7b907-177">Nyní můžete do třídy blogu přidat vlastnost, která bude představovat BlogDetails pro daný blog.</span><span class="sxs-lookup"><span data-stu-id="7b907-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="7b907-178">V databázi bude tabulka blogu obsahovat všechny vlastnosti blogu včetně vlastností obsažených ve vlastnosti BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="7b907-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="7b907-179">Ve výchozím nastavení předá každý z nich název komplexního typu, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="7b907-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Tabulka blogu se složitým typem](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="7b907-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="7b907-181">ConcurrencyCheck</span></span>

<span data-ttu-id="7b907-182">ConcurrencyCheck anotace umožňuje označit jednu nebo více vlastností, které mají být použity pro kontrolu souběžnosti v databázi, když uživatel upraví nebo odstraní entitu.</span><span class="sxs-lookup"><span data-stu-id="7b907-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="7b907-183">Pokud jste pracovali s návrhářem EF, tato možnost se zarovnává s nastavením vlastnosti ConcurrencyMode na pevnou.</span><span class="sxs-lookup"><span data-stu-id="7b907-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="7b907-184">Pojďme se podívat, jak ConcurrencyCheck funguje tím, že ho přidáte do vlastnosti blogger.</span><span class="sxs-lookup"><span data-stu-id="7b907-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="7b907-185">Při volání metody SaveChanges z důvodu ConcurrencyCheck poznámky v poli Blogger se v aktualizaci použije původní hodnota této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b907-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="7b907-186">Příkaz se pokusí vyhledat správný řádek filtrováním nejen na hodnotu klíče, ale také na původní hodnotě blogger.</span><span class="sxs-lookup"><span data-stu-id="7b907-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="7b907-187">  Tady jsou důležité části příkazu UPDATE odeslaného do databáze, kde vidíte, že příkaz aktualizuje řádek, který má PrimaryTrackingKey, je 1 a blogger pro "Julie", což byla původní hodnota, když byl tento blog načten z databáze.</span><span class="sxs-lookup"><span data-stu-id="7b907-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="7b907-188">Pokud někdo mezitím změnil název blogger pro tento blog, tato aktualizace se nezdaří a získáte DbUpdateConcurrencyException, který budete muset zpracovat.</span><span class="sxs-lookup"><span data-stu-id="7b907-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="7b907-189">Časové razítko</span><span class="sxs-lookup"><span data-stu-id="7b907-189">TimeStamp</span></span>

<span data-ttu-id="7b907-190">Je obvyklejší používat rowversion nebo pole timestamp pro kontrolu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="7b907-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="7b907-191">Místo toho, abyste použili anotaci ConcurrencyCheck, můžete použít konkrétnější anotaci časového razítka, pokud je typ vlastnosti bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="7b907-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="7b907-192">Kód nejprve zpracuje vlastnosti časového razítka stejně jako vlastnosti ConcurrencyCheck, ale také zajistí, že pole databáze, které kód poprvé generuje, nemůže mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="7b907-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="7b907-193">V dané třídě lze mít pouze jednu vlastnost časového razítka.</span><span class="sxs-lookup"><span data-stu-id="7b907-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="7b907-194">Přidání následující vlastnosti do třídy blogu:</span><span class="sxs-lookup"><span data-stu-id="7b907-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="7b907-195">Výsledkem je, že kód nejprve vytvoří sloupec časového razítka bez hodnoty null v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="7b907-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Tabulka blogů se sloupcem s časovým razítkem](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="7b907-197">Tabulka a sloupec</span><span class="sxs-lookup"><span data-stu-id="7b907-197">Table and Column</span></span>

<span data-ttu-id="7b907-198">Pokud Code First vytvořit databázi, možná budete chtít změnit názvy tabulek a sloupců, které vytváří.</span><span class="sxs-lookup"><span data-stu-id="7b907-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="7b907-199">Code First můžete použít i u existující databáze.</span><span class="sxs-lookup"><span data-stu-id="7b907-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="7b907-200">Nejedná se vždy o případ, že názvy tříd a vlastností ve vaší doméně odpovídají názvům tabulek a sloupců ve vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="7b907-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="7b907-201">Moje třída je pojmenovaná jako blog a podle konvence. jako první předpokládáme, že se namapuje na tabulku s názvem Blogy.</span><span class="sxs-lookup"><span data-stu-id="7b907-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="7b907-202">Pokud tomu tak není, můžete zadat název tabulky s atributem Table.</span><span class="sxs-lookup"><span data-stu-id="7b907-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="7b907-203">Například Poznámka určuje, že název tabulky je InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="7b907-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="7b907-204">Anotace sloupce je více dobře při zadávání atributů namapovaného sloupce.</span><span class="sxs-lookup"><span data-stu-id="7b907-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="7b907-205">Můžete určit název, datový typ nebo i pořadí, ve kterém se sloupec v tabulce zobrazuje.</span><span class="sxs-lookup"><span data-stu-id="7b907-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="7b907-206">Tady je příklad atributu sloupce.</span><span class="sxs-lookup"><span data-stu-id="7b907-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="7b907-207">Nepleťte si atribut TypeName sloupce s datovým typem DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="7b907-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="7b907-208">DataType je anotace, která se používá pro uživatelské rozhraní a je ignorována Code First.</span><span class="sxs-lookup"><span data-stu-id="7b907-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="7b907-209">Tady je tabulka, která se po opětovném vygenerování znovu vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="7b907-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="7b907-210">Název tabulky se změnil na InternalBlogs a sloupec Description ze komplexního typu je teď BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="7b907-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="7b907-211">Vzhledem k tomu, že název byl zadán v anotaci, kód nejprve nepoužije konvenci začátku názvu sloupce s názvem komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="7b907-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Přejmenování tabulky a sloupce blogů](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="7b907-213">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="7b907-213">DatabaseGenerated</span></span>

<span data-ttu-id="7b907-214">Důležité funkce databáze je možnost mít vypočítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b907-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="7b907-215">Pokud namapujete Code First třídy na tabulky, které obsahují počítané sloupce, nechcete, aby se Entity Framework pokusit tyto sloupce aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7b907-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="7b907-216">Ale chcete, aby EF vrátil tyto hodnoty z databáze poté, co jste vložili nebo aktualizovali data.</span><span class="sxs-lookup"><span data-stu-id="7b907-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="7b907-217">Můžete použít poznámku DatabaseGenerated k označení těchto vlastností ve třídě spolu s vypočítaným výčtem.</span><span class="sxs-lookup"><span data-stu-id="7b907-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="7b907-218">Jiné výčty jsou žádné a identita.</span><span class="sxs-lookup"><span data-stu-id="7b907-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="7b907-219">Databázi, která je vygenerována ve sloupcích bajtů nebo časových razítek, můžete použít, když kód poprvé generuje databázi. v opačném případě byste měli tuto možnost používat pouze při přechodu na existující databáze, protože kód nejprve nebude schopen určit vzorec pro vypočítaný sloupec.</span><span class="sxs-lookup"><span data-stu-id="7b907-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="7b907-220">Přečtěte si, že ve výchozím nastavení se klíčovou vlastností, která je celé číslo, stane klíč identity v databázi.</span><span class="sxs-lookup"><span data-stu-id="7b907-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="7b907-221">To by bylo stejné jako nastavení DatabaseGenerated na DatabaseGeneratedOption. identity.</span><span class="sxs-lookup"><span data-stu-id="7b907-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="7b907-222">Pokud nechcete, aby to byl klíč identity, můžete nastavit hodnotu na DatabaseGeneratedOption. None.</span><span class="sxs-lookup"><span data-stu-id="7b907-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="7b907-223">Index</span><span class="sxs-lookup"><span data-stu-id="7b907-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="7b907-224">**EF 6.1 a vyšší pouze** – atribut indexu byl představen v Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="7b907-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="7b907-225">Pokud používáte starší verzi, informace v této části se nevztahují.</span><span class="sxs-lookup"><span data-stu-id="7b907-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="7b907-226">Index můžete vytvořit na jednom nebo více sloupcích pomocí **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="7b907-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="7b907-227">Přidáním atributu k jedné nebo více vlastnostem dojde k tomu, že EF vytvoří odpovídající index v databázi při vytváření databáze nebo pokud používáte Migrace Code First, uživatelské rozhraní odpovídající volání **CreateIndex** .</span><span class="sxs-lookup"><span data-stu-id="7b907-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="7b907-228">Například následující kód bude mít za následek vytvoření indexu ve sloupci **hodnocení** tabulky **příspěvky** v databázi.</span><span class="sxs-lookup"><span data-stu-id="7b907-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="7b907-229">Ve výchozím nastavení se index jmenuje **ix\_&lt;název vlastnosti&gt;** (IX\_hodnocení v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="7b907-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="7b907-230">Můžete také zadat název indexu, i když.</span><span class="sxs-lookup"><span data-stu-id="7b907-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="7b907-231">Následující příklad určuje, že index by měl mít název **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="7b907-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="7b907-232">Ve výchozím nastavení indexy nejsou jedinečné, ale k určení toho, zda má být index jedinečný, lze použít parametr s příponou " **Unique** ".</span><span class="sxs-lookup"><span data-stu-id="7b907-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="7b907-233">Následující příklad zavádí jedinečný index pro přihlašovací jméno **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7b907-233">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="7b907-234">Indexy s více sloupci</span><span class="sxs-lookup"><span data-stu-id="7b907-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="7b907-235">Indexy, které jsou rozloženy na více sloupcích, jsou určeny pomocí stejného názvu v několika poznámkách indexu pro danou tabulku.</span><span class="sxs-lookup"><span data-stu-id="7b907-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="7b907-236">Při vytváření indexů s více sloupci musíte zadat objednávku pro sloupce v indexu.</span><span class="sxs-lookup"><span data-stu-id="7b907-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="7b907-237">Například následující kód vytvoří index s více sloupci **hodnocení** a **BlogId** s názvem **IX\_BlogIdAndRating**.</span><span class="sxs-lookup"><span data-stu-id="7b907-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="7b907-238">**BlogId** je první sloupec v indexu a **hodnocení** je druhé.</span><span class="sxs-lookup"><span data-stu-id="7b907-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="7b907-239">Atributy vztahu: InverseProperty a klíč ForeignKey</span><span class="sxs-lookup"><span data-stu-id="7b907-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="7b907-240">Tato stránka poskytuje informace o nastavení vztahů v Code First modelu pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="7b907-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="7b907-241">Obecné informace o relacích v EF a o tom, jak přistupovat k datům a pracovat s nimi pomocí vztahů, najdete v tématu [relace & vlastnosti navigace](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="7b907-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="7b907-242">První konvence kódu postará o nejběžnější vztahy v modelu, ale v některých případech je potřeba, aby vám pomohla.</span><span class="sxs-lookup"><span data-stu-id="7b907-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="7b907-243">Změna názvu klíčové vlastnosti ve třídě blogu vytvořila problém se vztahem k příspěvku.</span><span class="sxs-lookup"><span data-stu-id="7b907-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="7b907-244">Při generování databáze kód nejprve uvidí vlastnost BlogId ve třídě post a rozpoznává ji podle konvence, která odpovídá názvu třídy plus "ID" jako cizímu klíči pro třídu blogu.</span><span class="sxs-lookup"><span data-stu-id="7b907-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="7b907-245">Ve třídě blogu ale není žádná vlastnost BlogId.</span><span class="sxs-lookup"><span data-stu-id="7b907-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="7b907-246">Řešením pro tuto metodu je vytvoření navigační vlastnosti v příspěvku a použití cizího dataanotace k tomu, abyste si před kódem usnadnili vytváření vztahů mezi dvěma třídami – pomocí vlastnosti post. BlogId – a určení omezení v databáze.</span><span class="sxs-lookup"><span data-stu-id="7b907-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="7b907-247">Omezení v databázi zobrazuje relaci mezi InternalBlogs. PrimaryTrackingKey a post. BlogId.</span><span class="sxs-lookup"><span data-stu-id="7b907-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![vztah mezi InternalBlogs. PrimaryTrackingKey a post. BlogId](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="7b907-249">InverseProperty se používá, pokud máte více vztahů mezi třídami.</span><span class="sxs-lookup"><span data-stu-id="7b907-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="7b907-250">V rámci třídy post můžete chtít sledovat, kdo napsal Blogový příspěvek, a kdo ho upravil.</span><span class="sxs-lookup"><span data-stu-id="7b907-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="7b907-251">Tady jsou dvě nové navigační vlastnosti pro třídu post.</span><span class="sxs-lookup"><span data-stu-id="7b907-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="7b907-252">Také budete muset přidat do třídy Person, na kterou odkazují tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7b907-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="7b907-253">Třída Person má navigační vlastnosti zpátky na příspěvek, jednu pro všechny příspěvky napsané osobou a jednu pro všechny příspěvky, které tato osoba aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="7b907-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="7b907-254">První kód nemůže porovnat vlastnosti ve dvou třídách sama o sobě.</span><span class="sxs-lookup"><span data-stu-id="7b907-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="7b907-255">Databázová tabulka pro příspěvky by měla mít jeden cizí klíč pro osobu CreatedBy a jednu pro osobu UpdatedBy, ale kód nejprve vytvoří čtyři vlastnosti cizího klíče: Person\_ID, person\_Id1, CreatedBy\_ID a UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="7b907-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Tabulka příspěvků s dalšími cizími klíči](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="7b907-257">Chcete-li tyto problémy vyřešit, můžete použít poznámku InverseProperty a zadat zarovnání vlastností.</span><span class="sxs-lookup"><span data-stu-id="7b907-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="7b907-258">Vzhledem k tomu, že vlastnost PostsWritten v osobě ví, že se jedná o typ příspěvku, vytvoří relaci pro post. CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="7b907-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="7b907-259">Podobně se PostsUpdated připojí k post. UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="7b907-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="7b907-260">A Code First nevytvoří nadbytečné cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="7b907-260">And code first will not create the extra foreign keys.</span></span>

![Tabulka příspěvků bez dalších cizích klíčů](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="7b907-262">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7b907-262">Summary</span></span>

<span data-ttu-id="7b907-263">Pomocí dataanotace nemůžete pouze popsat ověřování na straně klienta a serveru v kódu jako první třídy, ale také vám umožní vylepšit a dokonce opravit předpoklady, které kód poprvé provede pro vaše třídy na základě jeho konvencí.</span><span class="sxs-lookup"><span data-stu-id="7b907-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="7b907-264">S dataanotacemi nemůžete pouze generovat schéma databáze, ale můžete také namapovat první třídy kódu na stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="7b907-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="7b907-265">I když jsou velmi flexibilní, mějte na paměti, že dataanotace poskytují pouze nejčastěji potřebné změny konfigurace, které můžete provést na svých třídách Code First.</span><span class="sxs-lookup"><span data-stu-id="7b907-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="7b907-266">Pokud chcete konfigurovat třídy pro některé z hraničních případů, měli byste se podívat na alternativní konfigurační mechanismus Code First Fluent API.</span><span class="sxs-lookup"><span data-stu-id="7b907-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
