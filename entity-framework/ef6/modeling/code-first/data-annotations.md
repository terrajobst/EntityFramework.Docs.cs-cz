---
title: Kód anotací dat při prvním - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 38ae52543ed99e5a1c1da7d19a2e15d168e3a1bd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490103"
---
# <a name="code-first-data-annotations"></a>Kód první datové poznámky
> [!NOTE]
> **EF4.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 4.1. Pokud používáte starší verzi, některé nebo všechny tyto informace se nevztahují.

Obsah na této stránce jsou upraveny z článku původně vytvořeny pomocí Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First umožňuje používat vaše vlastní třídy domény k vyjádření modelu EF využívá k provedení dotazu, změňte sledování a aktualizuje se funkce. Kód nejprve využívá programovací model označovány jako "úmluva nad konfigurací." Kód nejprve převezme, Entity Framework pro vytváření tříd a v takovém případě budou automaticky fungovat možnostmi, jak provést její úlohy. Ale pokud tříd nepostupujte podle těchto konvence, máte možnost přidání konfigurace do vaší třídy, které poskytují EF s požadovanými informacemi.

Kód nejprve poskytuje dva způsoby, jak tyto konfigurace přidat do vaší třídy. Jeden používá jednoduché atributy, které volá DataAnnotations a druhý je použití Code First Fluent API, která vám poskytuje způsob, jak popisují konfiguraci toho, v kódu.

Tento článek se zaměří na používání DataAnnotations (v oboru názvů System.ComponentModel.DataAnnotations) ke konfiguraci třídy – zvýraznění nejčastěji potřebných konfigurací. DataAnnotations také rozumí počet aplikací .NET, jako je například technologie ASP.NET MVC, která umožňuje tyto aplikace využívat stejné poznámky k ověření na straně klienta.


## <a name="the-model"></a>Model

Vám předvedu první DataAnnotations kódu pomocí jednoduchého páru tříd: Blog a příspěvku.

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

Jsou, blogu a příspěvku třídy pohodlně postupujte podle úmluvy první kód a vyžadují žádné vylepšení umožňující EF kompatibilitou. Ale můžete také použít poznámky vám poskytneme Další informace o třídách a databáze, ke které jsou mapovány na EF.

 

## <a name="key"></a>Key

Entity Framework spoléhá na každé entitě s hodnotou klíče, který se používá pro entitu pro sledování. Jeden konvence Code First je implicitní klíčové vlastnosti; Kód nejprve bude hledat vlastnost s názvem "Id" nebo kombinace názvu třídy a "Id", jako je například "BlogId". Tato vlastnost bude mapovat na sloupec primárního klíče v databázi.

Třídy blogu a účtovat podle Tato konvence. Co když se nepovedlo? Co když blogu použít název *PrimaryTrackingKey* místo, nebo dokonce *foo*? Pokud kód nejprve nenajde vlastnost, která odpovídá tato konvence vyvolá výjimku z důvodu požadavku Entity Framework, že musí mít vlastnost klíče. Poznámka klíče můžete použít k určení vlastností, které se má použít jako EntityKey.

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

Pokud jste nejdřív pomocí kódu je funkce generování databáze, blogu tabulka bude obsahovat sloupec primárního klíče s názvem PrimaryTrackingKey, který je definovaný i jako identitu ve výchozím nastavení.

![Blog tabulky s primárním klíčem](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Složené klíče

Entity Framework podporuje složené klíče – primární klíče, které se skládají z více než jednu vlastnost. Například můžete mít Passport třídy, jejichž primární klíč je kombinací PassportNumber a IssuingCountry.

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

Pokus o použití ve vašem modelu EF třídu výše by výsledkem `InvalidOperationException`:

*Nepovedlo se určit složený primární klíče řazení pro typ "Passport". Použijte ColumnAttribute nebo metodu HasKey k určení pořadí pro složený primární klíče.*

Chcete-li použít složené klíče, Entity Framework vyžaduje, abyste k definování objednávku ke klíčové vlastnosti. Můžete to provést s použitím sloupce Poznámka k určení pořadí.

>[!NOTE]
> Hodnota pořadí je relativní (a ne na základě indexu), můžete použít všechny hodnoty. Například 100 až 200 bude přijatelné namísto 1 a 2.

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

Pokud máte entity složené cizí klíče, musíte zadat stejný sloupec řazení, který jste použili pro odpovídající vlastnosti primárního klíče.

Pouze relativní řazení v rámci vlastnosti cizího klíče musí být stejný přesné hodnoty přiřazené k **pořadí** nemusí odpovídat. Například ve třídě následující 3 a 4 by mohla být zastoupen 1 a 2.

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

## <a name="required"></a>Požadováno

Poznámka vyžaduje říká EF vyžádáním určité vlastnosti.

Přidání vyžaduje vlastnost názvu vynutí EF (a MVC) ujistěte se, že vlastnost obsahuje data.

``` csharp
    [Required]
    public string Title { get; set; }
```

Žádné další bez změny kódu nebo značek v aplikaci, aplikace MVC provede ověřování na straně klienta, dokonce i dynamické vytvoření zprávu pomocí názvy vlastností a poznámek.

![Vytvoření stránky s názvem je povinné chyba](~/ef6/media/jj591583-figure02.png)

Požadovaný atribut ovlivní také generován databázový tím, že namapovanou vlastnost Null. Všimněte si, že pole název se změnil na "not null".

>[!NOTE]
> V některých případech nemusí být možné pro sloupec v databázi být null, i když tato vlastnost je vyžadovaná. Například při použití dat TPH dědičnosti strategie pro více typů je uložené v jediné tabulce. Pokud odvozený typ obsahuje požadovanou vlastnost sloupec nelze nastavit jako Null protože ne všechny typy v hierarchii, bude mít tato vlastnost.

 

![Blogy tabulky](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength a MinLength

Atributy MaxLength a MinLength umožňují zadat další vlastnosti ověření, stejně jako u požadované.

Tady je BloggerName s požadavky na délku. Tento příklad také ukazuje, jak kombinovat atributy.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

Poznámka MaxLength ovlivní databázi nastavením vlastnosti length na 10.

![Blogy tabulka zobrazující maximální délka pro sloupec BloggerName](~/ef6/media/jj591583-figure04.png)

MVC na straně klienta poznámky a anotace EF 4.1 na straně serveru i dodrží toto ověření znovu dynamické vytvoření chybová zpráva: "pole BloggerName musí být typu řetězce nebo pole s maximální délkou"10"." Tato zpráva je trochu dlouhý. Mnoho anotace umožňují zadat chybovou zprávu s atributem chybová zpráva.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Můžete také zadat chybová zpráva v poznámce požadované.

![Vytvoření stránky se vlastní chybová zpráva](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

První konvence kódu určí, že všech vlastností, které je podporované datového typu je reprezentován v databázi. Ale to není vždy případ ve svých aplikacích. Například může mít vlastnost ve třídě blogu, který vytvoří kódu na základě názvu a BloggerName polí. Tato vlastnost se dají vytvářet dynamicky a není potřeba ukládat. Můžete označit všechny vlastnosti, které se nemapují na databázi s poznámkou NotMapped například tuto vlastnost BlogCode.

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

 

## <a name="complextype"></a>Typ ComplexType

Není k popisu entity domény mezi sadu tříd a potom vrstvy těchto tříd k popisu úplnou entitu. Můžete například přidat třídu s názvem BlogDetails do modelu.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Všimněte si, že BlogDetails nemá libovolného typu klíčovou vlastnost. V návrhu na základě domény BlogDetails označuje jako objekt hodnoty. Entity Framework odkazují na objekty hodnotu jako komplexní typy.  Komplexní typy nelze sledovat na své vlastní.

Nicméně jako vlastnost ve třídě blogu BlogDetails ho budou sledovány jako součást objektu blogu. Aby code first pro to rozpoznat je třeba označit třídu BlogDetails jako element ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Nyní můžete přidat vlastnost ve třídě blogu k reprezentaci BlogDetails pro tento blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

V databázi v blogu tabulce bude obsahovat všechny vlastnosti včetně vlastnosti obsažené v jeho vlastnost BlogDetail blogu. Ve výchozím nastavení každý z nich je před názvem komplexní typ, BlogDetail.

![Blog tabulku s komplexní typ](~/ef6/media/jj591583-figure06.png)

Další zajímavé Poznámka je sice DateCreated vlastnost byla definována jako neumožňující hodnotu data a času ve třídě, pole příslušnou databázi s povolenou hodnotou Null. Pokud chcete mít vliv na schéma databáze, je nutné použít požadované poznámky.

 

## <a name="concurrencycheck"></a>Atribut ConcurrencyCheck

Poznámka atribut ConcurrencyCheck umožňuje označit jednu nebo více vlastností, které má být použit pro souběžnost kontroly v databázi, když uživatel upraví nebo odstraní entitu. Pokud jste pracovali s EF designeru, ten je v souladu s nastavení vlastnosti režim ConcurrencyMode na pevný.

Podívejme se, jak atribut ConcurrencyCheck funguje tak, že přidáte BloggerName vlastnosti.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Při volání SaveChanges, protože atribut ConcurrencyCheck Poznámka u pole BloggerName původní hodnota dané vlastnosti se použije v aktualizaci. Příkaz se pokusí najít správný řádek pomocí filtrování pouze na hodnotě klíče, ale také u původní hodnoty BloggerName.  Tady jsou důležité části příkazu UPDATE odeslán do databáze, ve kterém uvidíte příkaz aktualizuje řádek, který má PrimaryTrackingKey je 1 a BloggerName z "Julie", který byl původní hodnotu při tomto blogu byla načtena z databáze.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Pokud do té doby někdo bloggeru název pro tento blog, tato aktualizace se nezdaří a získáte DbUpdateConcurrencyException, které budete potřebovat pro zpracování.

 

## <a name="timestamp"></a>Časové razítko

Je běžné použití pole rowversion nebo časového razítka pro kontrolu souběžnosti. Ale místo použití atribut ConcurrencyCheck poznámky, můžete použít konkrétnější anotaci časového razítka jako typ vlastnosti je bajtové pole. Kód nejprve bude považovat za vlastnosti časového razítka stejný atribut ConcurrencyCheck vlastnosti, ale také pomohou zajistit, že pole databáze, které kód nejprve vytvoří hodnotu Null. Máte jenom jednu vlastnost časového razítka v dané třídě.

Přidávání do třídy blogu následující vlastnost:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

výsledky v kódu nejprve vytvořit sloupec časového razítka Null v tabulce databáze.

![Blogy tabulku se sloupci razítko času](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabulky a sloupce

Pokud umožníte tím Code First vytvořit databázi, můžete změnit název tabulky a sloupce, které se vytváří. Code First můžete použít také k existující databázi. Ale to není vždy případ, že názvy tříd a vlastností ve vaší doméně shodovat s názvy tabulek a sloupců ve vaší databázi.

Moje třída se nazývá blogu a podle konvence kódu nejprve předpokládá, že to se namapuje na tabulku s názvem blogy. Pokud se nejedná o tento případ můžete zadat název tabulky s atributem tabulky. Zde například Poznámka je určení, že název tabulky je InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Poznámka sloupec je další nimiž v určeném atributy pro mapovanou sloupec. Můžete stanovit, název, datový typ nebo dokonce pořadí, ve kterém se zobrazí sloupec v tabulce. Tady je příklad sloupce atributu.

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Nepleťte si Atribut TypeName sloupce s DataAnnotation datového typu. Datový typ se má poznámka použít pro uživatelské rozhraní a je ignorován Code First.

Tady je tabulka po je byly znovu vygenerovány. InternalBlogs změnil název tabulky a sloupce s popisem od komplexní typ je nyní BlogDescription. Protože v poznámce byl zadán název, kód nejprve nebudeme používat konvenci od názvu sloupce s názvem komplexního typu.

![Blogy tabulku a sloupec přejmenovat](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Důležité databázové funkce je schopnost mít vypočítané vlastnosti. Pokud máte mapování Code First tříd do tabulek, které obsahují vypočítané sloupce, nechcete, aby rozhraní Entity Framework a pokuste se aktualizovat tyto sloupce. Ale být vhodné EF vracet tyto hodnoty z databáze po vložení nebo aktualizovaná data. Poznámka DatabaseGenerated můžete označit, že tyto vlastnosti ve třídě spolu s vypočítané výčtu. Jiných výčtech k žádným nedošlo a Identity.

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Můžete použít databáze, které jsou generovány pro sloupce bajtů nebo jako časové razítko, pokud kód je nejprve generování databáze, v opačném případě byste měli používat jenom to při odkazující na stávající databáze, protože kód nejprve nebude schopna určit vzorec pro počítaný sloupec.

Řekli, která ve výchozím nastavení, klíčová vlastnost, která je celé číslo se stanou klíč identity v databázi. Která budou stejné jako nastavení DatabaseGenerated DatabaseGenerationOption.Identity. Pokud nechcete, aby to přijde klíč identity, můžete nastavit hodnotu na DatabaseGenerationOption.None.

 

## <a name="index"></a>Index

> [!NOTE]
> **EF6.1 a vyšší pouze** – atribut indexu byla zavedena v Entity Framework 6.1. Pokud používáte starší verzi informace v této části se nevztahují.

Můžete vytvořit index na jeden nebo více sloupců pomocí **IndexAttribute**. Přidání atributu na jeden nebo více vlastností se způsobit EF k vytvoření odpovídající index v databázi při vytváření databáze, nebo generování uživatelského rozhraní k odpovídající položce **CreateIndex** zavolá, pokud používáte migrace Code First.

Například následující kód způsobí indexu vytvářen **hodnocení** sloupec **příspěvky** tabulky v databázi.

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

Ve výchozím nastavení, bude mít název indexu **IX\_&lt;název vlastnosti&gt;**  (IX\_hodnocení v předchozím příkladu). Můžete také zadat název pro index ale. Následující příklad určuje, že index s názvem **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Indexy jsou ve výchozím nastavení, není jedinečný, ale můžete použít **IsUnique** s názvem parametru k určení, že index musí být jedinečné. Následující příklad představuje jedinečný index na **uživatele**pro přihlašovací jméno.

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

### <a name="multiple-column-indexes"></a>Sloupec indexů

Indexy, které přesahují do více sloupců je určené vlastností se stejným názvem ve více poznámek Index pro danou tabulku. Při vytváření indexů více sloupci, je třeba zadat pořadí sloupců v indexu. Například následující kód vytvoří index více sloupců na **hodnocení** a **BlogId** volá **IX\_BlogAndRating**. **BlogId** je první sloupec v indexu a **hodnocení** je druhý.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Relace atributů: InverseProperty a cizí klíč

> [!NOTE]
> Tato stránka obsahuje informace o nastavení relace v modelu Code First pomocí datových poznámek. Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md). *

První konvence kódu se postará o nejběžnějších relace v modelu, ale existují případy, kdy je potřebuje pomoc.

Změna názvu klíčová vlastnost ve třídě blogu vytvoří problém s jeho vztah k příspěvku. 

Při generování databáze, kód nejprve uvidí BlogId vlastnost ve třídě Post a rozpozná, podle konvence, že se shoduje názvem třídy plus "Id", jako cizí klíč třídy blogu. Ale neexistuje žádná vlastnost BlogId ve třídě blogu. Řešení pro tento je vytvořit vlastnost navigace v příspěvku a používat cizí DataAnnotation ke kódu nejprve porozumět postupu při vytvoření vztahu mezi dvěma třídami – pomocí vlastnosti Post.BlogId – jak se dá zadat omezení databáze.

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

Omezení v databázi znázorňuje relaci mezi InternalBlogs.PrimaryTrackingKey a Posts.BlogId. 

![vztah mezi InternalBlogs.PrimaryTrackingKey a Posts.BlogId](~/ef6/media/jj591583-figure09.png)

InverseProperty se používá v případě, že máte více vztahů mezi třídami.

Ve třídě příspěvek můžete chtít sledovat, který napsal příspěvek na blog a také ji upravila. Tady jsou dvě nové navigační vlastnosti pro třídu příspěvku.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Také budete muset přidat v třídě osoba odkazuje tyto vlastnosti. Třída osoba má vlastnosti navigace zpět na příspěvek, jeden pro všechny příspěvky autorem osoby a jeden pro všechny příspěvky aktualizoval tuto osobu.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Kód nejprve není schopen odpovídat vlastnosti ve dvou tříd sama o sobě. Databázové tabulky pro příspěvky by měl mít jeden cizí klíč pro osobu, CreatedBy a jeden pro osoby UpdatedBy ale kód nejprve vytvoří čtyři se vlastnosti cizího klíče: osoba\_Id, osoba\_Id1, CreatedBy\_Id a UpdatedBy\_ID.

![Příspěvky tabulku s velmi cizí klíče](~/ef6/media/jj591583-figure10.png)

Chcete-li vyřešit tyto problémy, můžete použít poznámku InverseProperty k určení zarovnání vlastnosti.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Protože vlastnost PostsWritten osobně ví, že to se vztahuje na typu příspěvku, vytvoří vztah Post.CreatedBy. PostsUpdated podobně připojí Post.UpdatedBy. A kód nejprve nevytvoří velmi cizí klíče.

![Tabulka příspěvky bez dalších cizí klíče](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Souhrn

DataAnnotations nejen umožňují popsat ověřování na straně klienta a serveru ve třídách první kódu, ale také umožňují vylepšit a dokonce i opravit předpoklady, které kód nejprve vytvoří o třídách podle jeho vytváření. S DataAnnotations nelze jenom jednotky generování schématu databáze, ale můžete také namapovat první třídy kódu k existující databázi.

Když jsou flexibilní, mějte na paměti, poskytující DataAnnotations pouze většinu běžně potřebné změny konfigurace provedené na váš kód první třídy. Ke konfiguraci třídy pro některý ze hraniční případy, by měl vypadat mechanismu, který alternativní konfiguraci, Code First rozhraní Fluent API.
