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
# <a name="code-first-data-annotations"></a>Code First datové poznámky
> [!NOTE]
> **EF 4.1 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 4,1. Pokud používáte starší verzi, některé nebo všechny tyto informace se nevztahují.

Obsah na této stránce je přizpůsoben z článku, který původně napsal Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First umožňuje použít vlastní třídy domény k reprezentaci modelu, na kterém se EF spoléhá, aby prováděl dotazování, sledování změn a aktualizaci funkcí. Code First využívá programovací model, který je označován jako "konvence před konfigurací". Code First předpokládá, že vaše třídy dodržují konvence Entity Framework a v takovém případě bude automaticky fungovat, jak provést úlohu. Nicméně pokud vaše třídy nedodržují tyto konvence, máte možnost Přidat konfigurace do tříd a poskytnout EF základní informace o požadavcích.

Code First poskytuje dva způsoby, jak přidat tyto konfigurace do tříd. Jedna z nich používá jednoduché atributy s názvem DataAnnotations a druhá používá Code First rozhraní API Fluent, které poskytuje způsob, jak v kódu imperativně popsat konfigurace.

Tento článek se zaměří na použití DataAnnotations (v oboru názvů System. ComponentModel. DataAnnotations) ke konfiguraci tříd – zvýrazňování nejčastěji potřebných konfigurací. K dataannotationům se rozumí také řada aplikací .NET, jako je ASP.NET MVC, což umožňuje těmto aplikacím využívat stejné poznámky pro ověřování na straně klienta.


## <a name="the-model"></a>Model

Ukážeme Code First dataanotace pomocí jednoduché dvojice tříd: blog a příspěvek.

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

V takovém případě se třídy blogů a post vhodně řídí konvencí Code First a nevyžadují žádné vylepšení pro povolení kompatibility EF. Poznámky však můžete použít také k poskytnutí dalších informací k EF o třídách a databázi, ke které jsou mapovány.

 

## <a name="key"></a>Klíč

Entity Framework spoléhá na každou entitu, která má hodnotu klíče, která se používá ke sledování entit. Jedna konvence Code First je implicitní vlastnosti klíče; Code First bude hledat vlastnost s názvem "ID" nebo kombinaci názvu třídy a "ID", například "BlogId". Tato vlastnost bude namapována na sloupec primárního klíče v databázi.

Blogové a poštovní třídy se budou řídit touto konvencí. Co když neudělal? Co když blog místo toho použil název *PrimaryTrackingKey* nebo dokonce i *foo*? Pokud kód nenalezne vlastnost, která odpovídá této konvenci, vyvolá výjimku z důvodu požadavku Entity Framework, že musíte mít vlastnost Key. Klíčovou poznámku můžete použít k určení, která vlastnost se má použít jako EntityKey.

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

Pokud používáte funkci generování databáze prvního kódu, tabulka blogu bude mít sloupec primárního klíče s názvem PrimaryTrackingKey, který je ve výchozím nastavení definován jako identita.

![Tabulka blogu s primárním klíčem](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Složené klíče

Entity Framework podporuje složené klíče – primární klíče, které se skládají z více než jedné vlastnosti. Například můžete mít třídu služby Passport, jejíž primární klíč je kombinací hodnot PassportNumber a IssuingCountry.

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

Pokus o použití výše uvedené třídy v modelu EF má za následek `InvalidOperationException`:

*Nelze určit řazení složených primárních klíčů pro typ Passport. Použijte metodu ColumnAttribute nebo Haskey – k určení objednávky složených primárních klíčů.*

Aby bylo možné používat složené klíče, Entity Framework vyžaduje, abyste definovali pořadí vlastností klíče. Můžete to provést pomocí anotace sloupce a určit objednávku.

>[!NOTE]
> Hodnota Order je relativní (spíše než index založený na indexu), takže lze použít jakékoli hodnoty. Například 100 a 200 by byly přijatelné místo 1 a 2.

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

Pokud máte entity se složenými cizími klíči, je nutné zadat stejné řazení sloupců, které jste použili pro příslušné vlastnosti primárního klíče.

Pouze relativní řazení v rámci vlastností cizích klíčů musí být stejné, přesné hodnoty přiřazené k **objednávce** nemusejí odpovídat. Například v následujících třídách lze použít místo 1 a 2 na úrovni 3 a 4.

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

## <a name="required"></a>Požaduje se

Požadovaná Poznámka oznamuje EF, že je požadována konkrétní vlastnost.

Přidání vyžadované pro vlastnost title vynutí EF (a MVC), aby bylo zajištěno, že vlastnost obsahuje data.

``` csharp
    [Required]
    public string Title { get; set; }
```

Aplikace MVC provede ověřování na straně klienta, a to i v případě, že v aplikaci nejsou žádné další změny kódu nebo kódu, a to i při dynamickém vytváření zprávy pomocí názvů vlastností a poznámek.

![Při vytváření stránky s nadpisem se vyžaduje chyba.](~/ef6/media/jj591583-figure02.png)

Požadovaný atribut bude mít vliv také na vygenerovanou databázi tím, že namapuje vlastnost, která není null. Všimněte si, že pole název se změnilo na hodnotu not null.

>[!NOTE]
> V některých případech nemusí být možné, aby sloupec v databázi mohl mít hodnotu null, i když je požadovaná vlastnost. Pokud například použijete strategii dědičnosti TPH pro více typů, uloží se do jedné tabulky. Pokud odvozený typ obsahuje povinnou vlastnost, sloupec nemůže být nastaven na hodnotu null, protože ne všechny typy v hierarchii budou mít tuto vlastnost.

 

![Tabulka blogů](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength a MinLength

Atributy MaxLength a MinLength umožňují zadat další ověření vlastností, stejně jako u požadovaných.

Tady je požadavek blogger s požadavky na délku. Příklad také ukazuje, jak kombinovat atributy.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

Poznámka MaxLength bude mít vliv na databázi nastavením délky vlastnosti na hodnotu 10.

![Tabulka blogů zobrazující maximální délku sloupce blogger](~/ef6/media/jj591583-figure04.png)

Poznámka na straně klienta MVC a Poznámka k serveru EF 4,1 na straně serveru budou respektovat toto ověření, znovu dynamicky sestavit chybovou zprávu: "pole blogger musí být řetězec nebo typ pole s maximální délkou 10." Tato zpráva je trochu dlouhá. Mnoho poznámek vám umožní zadat chybovou zprávu s atributem ErrorMessage.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

V požadované anotaci můžete také zadat ErrorMessage.

![Vytvořit stránku s vlastní chybovou zprávou](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

První konvence kódu určuje, že všechny vlastnosti, které jsou podporovaným datovým typem, jsou v databázi reprezentovány. Nejedná se ale vždy o případ ve vašich aplikacích. Můžete mít například vlastnost ve třídě blog, která vytvoří kód založený na názvu a poli blogger. Tuto vlastnost lze vytvořit dynamicky a není nutné ji ukládat. Můžete označit všechny vlastnosti, které se mapují k databázi, pomocí anotace NotMapped, jako je tato vlastnost BlogCode.

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

 

## <a name="complextype"></a>Elementu

Není neobvyklé popsání doménových entit v rámci sady tříd a následnou vrstvou těchto tříd, abyste popsali úplnou entitu. Do modelu můžete například přidat třídu s názvem BlogDetails.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Všimněte si, že BlogDetails nemá žádný typ klíčové vlastnosti. V návrhu založeném na doméně se BlogDetails označuje jako objekt hodnoty. Entity Framework odkazuje na objekty hodnot jako komplexní typy.  Komplexní typy nelze sledovat samostatně.

Nicméně jako vlastnost ve třídě blogu BlogDetails bude sledována jako součást objektu blogu. Aby kód nejprve rozpoznal, je nutné označit třídu BlogDetails jako ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Nyní můžete do třídy blogu přidat vlastnost, která bude představovat BlogDetails pro daný blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

V databázi bude tabulka blogu obsahovat všechny vlastnosti blogu včetně vlastností obsažených ve vlastnosti BlogDetail. Ve výchozím nastavení předá každý z nich název komplexního typu, BlogDetail.

![Tabulka blogu se složitým typem](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a>ConcurrencyCheck

ConcurrencyCheck anotace umožňuje označit jednu nebo více vlastností, které mají být použity pro kontrolu souběžnosti v databázi, když uživatel upraví nebo odstraní entitu. Pokud jste pracovali s návrhářem EF, tato možnost se zarovnává s nastavením vlastnosti ConcurrencyMode na pevnou.

Pojďme se podívat, jak ConcurrencyCheck funguje tím, že ho přidáte do vlastnosti blogger.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Při volání metody SaveChanges z důvodu ConcurrencyCheck poznámky v poli Blogger se v aktualizaci použije původní hodnota této vlastnosti. Příkaz se pokusí vyhledat správný řádek filtrováním nejen na hodnotu klíče, ale také na původní hodnotě blogger.  Tady jsou důležité části příkazu UPDATE odeslaného do databáze, kde vidíte, že příkaz aktualizuje řádek, který má PrimaryTrackingKey, je 1 a blogger pro "Julie", což byla původní hodnota, když byl tento blog načten z databáze.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Pokud někdo mezitím změnil název blogger pro tento blog, tato aktualizace se nezdaří a získáte DbUpdateConcurrencyException, který budete muset zpracovat.

 

## <a name="timestamp"></a>Časové razítko

Je obvyklejší používat rowversion nebo pole timestamp pro kontrolu souběžnosti. Místo toho, abyste použili anotaci ConcurrencyCheck, můžete použít konkrétnější anotaci časového razítka, pokud je typ vlastnosti bajtové pole. Kód nejprve zpracuje vlastnosti časového razítka stejně jako vlastnosti ConcurrencyCheck, ale také zajistí, že pole databáze, které kód poprvé generuje, nemůže mít hodnotu null. V dané třídě lze mít pouze jednu vlastnost časového razítka.

Přidání následující vlastnosti do třídy blogu:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

Výsledkem je, že kód nejprve vytvoří sloupec časového razítka bez hodnoty null v tabulce databáze.

![Tabulka blogů se sloupcem s časovým razítkem](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabulka a sloupec

Pokud Code First vytvořit databázi, možná budete chtít změnit názvy tabulek a sloupců, které vytváří. Code First můžete použít i u existující databáze. Nejedná se vždy o případ, že názvy tříd a vlastností ve vaší doméně odpovídají názvům tabulek a sloupců ve vaší databázi.

Moje třída je pojmenovaná jako blog a podle konvence. jako první předpokládáme, že se namapuje na tabulku s názvem Blogy. Pokud tomu tak není, můžete zadat název tabulky s atributem Table. Například Poznámka určuje, že název tabulky je InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Anotace sloupce je více dobře při zadávání atributů namapovaného sloupce. Můžete určit název, datový typ nebo i pořadí, ve kterém se sloupec v tabulce zobrazuje. Tady je příklad atributu sloupce.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Nepleťte si atribut TypeName sloupce s datovým typem DataAnnotation. DataType je anotace, která se používá pro uživatelské rozhraní a je ignorována Code First.

Tady je tabulka, která se po opětovném vygenerování znovu vygenerovala. Název tabulky se změnil na InternalBlogs a sloupec Description ze komplexního typu je teď BlogDescription. Vzhledem k tomu, že název byl zadán v anotaci, kód nejprve nepoužije konvenci začátku názvu sloupce s názvem komplexního typu.

![Přejmenování tabulky a sloupce blogů](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Důležité funkce databáze je možnost mít vypočítané vlastnosti. Pokud namapujete Code First třídy na tabulky, které obsahují počítané sloupce, nechcete, aby se Entity Framework pokusit tyto sloupce aktualizovat. Ale chcete, aby EF vrátil tyto hodnoty z databáze poté, co jste vložili nebo aktualizovali data. Můžete použít poznámku DatabaseGenerated k označení těchto vlastností ve třídě spolu s vypočítaným výčtem. Jiné výčty jsou žádné a identita.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Databázi, která je vygenerována ve sloupcích bajtů nebo časových razítek, můžete použít, když kód poprvé generuje databázi. v opačném případě byste měli tuto možnost používat pouze při přechodu na existující databáze, protože kód nejprve nebude schopen určit vzorec pro vypočítaný sloupec.

Přečtěte si, že ve výchozím nastavení se klíčovou vlastností, která je celé číslo, stane klíč identity v databázi. To by bylo stejné jako nastavení DatabaseGenerated na DatabaseGeneratedOption. identity. Pokud nechcete, aby to byl klíč identity, můžete nastavit hodnotu na DatabaseGeneratedOption. None.

 

## <a name="index"></a>Index

> [!NOTE]
> **EF 6.1 a vyšší pouze** – atribut indexu byl představen v Entity Framework 6,1. Pokud používáte starší verzi, informace v této části se nevztahují.

Index můžete vytvořit na jednom nebo více sloupcích pomocí **IndexAttribute**. Přidáním atributu k jedné nebo více vlastnostem dojde k tomu, že EF vytvoří odpovídající index v databázi při vytváření databáze nebo pokud používáte Migrace Code First, uživatelské rozhraní odpovídající volání **CreateIndex** .

Například následující kód bude mít za následek vytvoření indexu ve sloupci **hodnocení** tabulky **příspěvky** v databázi.

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

Ve výchozím nastavení se index jmenuje **ix\_&lt;název vlastnosti&gt;** (IX\_hodnocení v předchozím příkladu). Můžete také zadat název indexu, i když. Následující příklad určuje, že index by měl mít název **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Ve výchozím nastavení indexy nejsou jedinečné, ale k určení toho, zda má být index jedinečný, lze použít parametr s příponou " **Unique** ". Následující příklad zavádí jedinečný index pro přihlašovací jméno **uživatele**.

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

### <a name="multiple-column-indexes"></a>Indexy s více sloupci

Indexy, které jsou rozloženy na více sloupcích, jsou určeny pomocí stejného názvu v několika poznámkách indexu pro danou tabulku. Při vytváření indexů s více sloupci musíte zadat objednávku pro sloupce v indexu. Například následující kód vytvoří index s více sloupci **hodnocení** a **BlogId** s názvem **IX\_BlogIdAndRating**. **BlogId** je první sloupec v indexu a **hodnocení** je druhé.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Atributy vztahu: InverseProperty a klíč ForeignKey

> [!NOTE]
> Tato stránka poskytuje informace o nastavení vztahů v Code First modelu pomocí datových poznámek. Obecné informace o relacích v EF a o tom, jak přistupovat k datům a pracovat s nimi pomocí vztahů, najdete v tématu [relace & vlastnosti navigace](~/ef6/fundamentals/relationships.md). *

První konvence kódu postará o nejběžnější vztahy v modelu, ale v některých případech je potřeba, aby vám pomohla.

Změna názvu klíčové vlastnosti ve třídě blogu vytvořila problém se vztahem k příspěvku. 

Při generování databáze kód nejprve uvidí vlastnost BlogId ve třídě post a rozpoznává ji podle konvence, která odpovídá názvu třídy plus "ID" jako cizímu klíči pro třídu blogu. Ve třídě blogu ale není žádná vlastnost BlogId. Řešením pro tuto metodu je vytvoření navigační vlastnosti v příspěvku a použití cizího dataanotace k tomu, abyste si před kódem usnadnili vytváření vztahů mezi dvěma třídami – pomocí vlastnosti post. BlogId – a určení omezení v databáze.

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

Omezení v databázi zobrazuje relaci mezi InternalBlogs. PrimaryTrackingKey a post. BlogId. 

![vztah mezi InternalBlogs. PrimaryTrackingKey a post. BlogId](~/ef6/media/jj591583-figure09.png)

InverseProperty se používá, pokud máte více vztahů mezi třídami.

V rámci třídy post můžete chtít sledovat, kdo napsal Blogový příspěvek, a kdo ho upravil. Tady jsou dvě nové navigační vlastnosti pro třídu post.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Také budete muset přidat do třídy Person, na kterou odkazují tyto vlastnosti. Třída Person má navigační vlastnosti zpátky na příspěvek, jednu pro všechny příspěvky napsané osobou a jednu pro všechny příspěvky, které tato osoba aktualizovala.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

První kód nemůže porovnat vlastnosti ve dvou třídách sama o sobě. Databázová tabulka pro příspěvky by měla mít jeden cizí klíč pro osobu CreatedBy a jednu pro osobu UpdatedBy, ale kód nejprve vytvoří čtyři vlastnosti cizího klíče: Person\_ID, person\_Id1, CreatedBy\_ID a UpdatedBy\_ID.

![Tabulka příspěvků s dalšími cizími klíči](~/ef6/media/jj591583-figure10.png)

Chcete-li tyto problémy vyřešit, můžete použít poznámku InverseProperty a zadat zarovnání vlastností.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Vzhledem k tomu, že vlastnost PostsWritten v osobě ví, že se jedná o typ příspěvku, vytvoří relaci pro post. CreatedBy. Podobně se PostsUpdated připojí k post. UpdatedBy. A Code First nevytvoří nadbytečné cizí klíče.

![Tabulka příspěvků bez dalších cizích klíčů](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Souhrn

Pomocí dataanotace nemůžete pouze popsat ověřování na straně klienta a serveru v kódu jako první třídy, ale také vám umožní vylepšit a dokonce opravit předpoklady, které kód poprvé provede pro vaše třídy na základě jeho konvencí. S dataanotacemi nemůžete pouze generovat schéma databáze, ale můžete také namapovat první třídy kódu na stávající databázi.

I když jsou velmi flexibilní, mějte na paměti, že dataanotace poskytují pouze nejčastěji potřebné změny konfigurace, které můžete provést na svých třídách Code First. Pokud chcete konfigurovat třídy pro některé z hraničních případů, měli byste se podívat na alternativní konfigurační mechanismus Code First Fluent API.
