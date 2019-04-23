---
title: Relace – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 9ef1a9269fc99f5b27a81c11a161ed5f9d74180d
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929934"
---
# <a name="relationships"></a>Relace

Jak dvě entity, které definuje vztah vzájemně souvisí. V relační databázi to je reprezentován omezení cizího klíče.

> [!NOTE]  
> Většina ukázky v tomto článku použijte vztah jeden mnoho k předvedení konceptů. Příklady relace 1: 1 a many-to-many viz [jiné vzory vztah](#other-relationship-patterns) na konci tohoto článku.

## <a name="definition-of-terms"></a>Definice pojmů

Existuje několik termínů používaných k popisu relací

* **Závislé entity:** Toto je entita, která obsahuje cizí klíčové vlastnosti. Někdy označovány jako "podřízený" relace.

* **Hlavní entita:** Toto je entita, která obsahuje primární a alternativní klíčové vlastnosti. Někdy označovány jako "nadřazený" relace.

* **Cizí klíč:** Vlastností v závislé entity, který se používá k ukládání hodnot hlavní klíčová vlastnost, která entita souvisí s.

* **Klíč instančního objektu:** Vlastnost, která jednoznačně identifikuje instančního objektu entity. To může být primární klíč nebo alternativního klíče.

* **Navigační vlastnost:** Vlastnost definovaná pro entitu instančního objektu a/nebo závislé, který obsahuje odkazy na související entity(s).

  * **Navigační vlastnost kolekce:** Navigační vlastnost, která obsahuje odkazy na související entity.

  * **Vlastnost navigace odkaz:** Navigační vlastnost, která obsahuje odkaz na jednu související entity.

  * **Inverzní navigační vlastnost:** Když hovoříte o konkrétní navigační vlastnost, tento výraz odkazuje na vlastnost navigace na druhém konci vztahu.

Následující výpis kódu ukazuje vztah jeden mnoho mezi `Blog` a `Post`

* `Post` je závislé entity

* `Blog` je entita, instančního objektu

* `Post.BlogId` je cizí klíč

* `Blog.BlogId` je klíč instančního objektu (v tomto případě je primární klíč, místo alternativního klíče)

* `Post.Blog` je odkaz na vlastnost navigace

* `Blog.Posts` je navigační vlastnost kolekce

* `Post.Blog` je inverzní navigační vlastnost `Blog.Posts` (a naopak)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konvence

Podle konvence se vytvořit relaci po navigační vlastnost nalezených v typu. Vlastnost je považován za vlastnost navigace, pokud jako skalární typ. nelze mapovat na typ, který odkazuje na aktuálním zprostředkovatelem databáze.

> [!NOTE]  
> Vztahy, které jsou zjištěny podle konvence se vždy cílit na primární klíč instančního objektu entity. Cílit na alternativního klíče, musí provést další konfigurace pomocí rozhraní Fluent API.

### <a name="fully-defined-relationships"></a>Plně definované relace

Nejběžnější vzor pro relace je definovaný na obou stranách relace a vlastnost cizího klíče definovaná ve třídě závislé entit navigační vlastnosti.

* Pokud se najde dvojici vlastností navigace mezi dvěma typy, bude nakonfigurován jako inverzní navigační vlastnosti stejné relace.

* Pokud subjektem závislé obsahuje vlastnost s názvem `<primary key property name>`, `<navigation property name><primary key property name>`, nebo `<principal entity name><primary key property name>` pak bude nakonfigurován jako cizí klíč.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Pokud jsou definované mezi dvěma typy více navigačních vlastností (to znamená, více než jeden odlišné pár navigaci, které odkazují na sobě navzájem), pak žádné relace, bude vytvořen pomocí konvence a budete muset ručně nakonfigurovat k identifikaci jak spojením navigační vlastnosti.

### <a name="no-foreign-key-property"></a>Žádná vlastnost cizího klíče

Doporučuje se mít vlastnost cizího klíče definovaná ve třídě závislé entity, není povinné. Pokud se nenajde žádná vlastnost cizího klíče, se bude zavádět stínové vlastnost cizího klíče s názvem `<navigation property name><principal key property name>` (viz [stínové vlastnosti](shadow-properties.md) Další informace).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Jednu navigační vlastnost

Včetně pouze jednu vlastnost navigace (žádné inverzní navigace a žádná vlastnost cizího klíče), stačí vztah definovaný podle konvence. Můžete mít také jednu navigační vlastnost a vlastnost cizího klíče.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Kaskádové odstranění

Podle konvence se nastaví kaskádové odstranění *Cascade* pro požadované relace a *ClientSetNull* pro volitelné relace. *Kaskádové* znamená, že se odstraní také závislých položek. *ClientSetNull* znamená, že závislých položek, které nejsou načtené do paměti zůstane beze změny a musí být ručně odstranit nebo aktualizovat tak, aby odkazovala na platný hlavní entitu. Pro entity, která jsou načtena do paměti EF Core se pokus o nastavení vlastnosti cizího klíče na hodnotu null.

Najdete v článku [povinné a nepovinné vztahy](#required-and-optional-relationships) části rozdíl mezi požadované a volitelné vztahy.

Zobrazit [Kaskádové odstraňování](../saving/cascade-delete.md) pro další podrobnosti o různých odstranit chování a výchozí hodnoty, které používají konvence.

## <a name="data-annotations"></a>Datové poznámky

Existují dva anotacemi dat, které lze použít ke konfiguraci relace, `[ForeignKey]` a `[InverseProperty]`. Tyto jsou dostupné v `System.ComponentModel.DataAnnotations.Schema` oboru názvů.

### <a name="foreignkey"></a>[ForeignKey]

Datové poznámky můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah. To se obvykle provádí při vlastnost cizího klíče nejde zjistit konvencí.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` Poznámky můžete umístit na žádnou vlastnost navigace v relaci. Není nutné přejít na navigační vlastnost ve třídě závislé entity.

### <a name="inverseproperty"></a>[InverseProperty]

Datové poznámky můžete nakonfigurovat jak vlastnosti navigace entit dependent a principal spárovat. To se obvykle provádí po více než jednu dvojici vlastností navigace mezi dvěma typy entit.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Ke konfiguraci vztahu v rozhraní Fluent API, spusťte určením navigační vlastnosti, které tvoří relace. `HasOne` nebo `HasMany` určuje vlastnost navigace typu entity jsou od konfigurace. Potom řetězit volání `WithOne` nebo `WithMany` k identifikaci inverzní navigace. `HasOne`/`WithOne` se používají pro vlastnosti navigačního odkazu a `HasMany` / `WithMany` se používají pro navigační vlastnosti kolekce.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Jednu navigační vlastnost

Pokud máte jenom jednu navigační vlastnost, existují konstruktor bez parametrů přetížení `WithOne` a `WithMany`. To znamená, že je koncepčně referencí nebo kolekcí na druhém konci vztahu, ale neexistuje žádné navigační vlastnost zahrnuta do entity třídy.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Cizí klíč

Rozhraní Fluent API můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

Následující výpis kódu ukazuje, jak nakonfigurovat složený cizí klíč.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

Můžete použít přetížení řetězce `HasForeignKey(...)` pro konfiguraci vlastnosti stínové jako cizí klíč (viz [stínové vlastnosti](shadow-properties.md) Další informace). Doporučujeme explicitně přidat vlastnost stínové modelu před jeho použitím jako cizí klíč (jak je vidět níže).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Klíč objektu

Pokud chcete cizí klíč k odkazování vlastnosti jiné než primární klíč, můžete použít rozhraní Fluent API nakonfigurovat vlastnost hlavního klíče pro daný vztah. Vlastnost, která se konfiguruje automaticky jako hlavní klíče se být nastaveny jako alternativního klíče (viz [alternativní klíče](alternate-keys.md) Další informace).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

Následující výpis kódu ukazuje, jak nakonfigurovat složený klíč instančního objektu.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> Pořadí, ve kterém můžete zadat hlavní klíčové vlastnosti musí odpovídat pořadí, ve kterém jsou uvedeny pro cizí klíč.

### <a name="required-and-optional-relationships"></a>Povinné a nepovinné relace

Rozhraní Fluent API můžete konfigurovat, jestli je vztah je povinná nebo volitelná. Takže v konečném důsledku Určuje, zda vlastnost cizího klíče je povinná nebo volitelná. To je nejužitečnější, pokud používáte cizí klíč stínové stavu. Pokud máte ve své třídě entity vlastnost cizího klíče pak requiredness relace je určen podle toho, jestli vlastnost cizího klíče požadované nebo volitelné (viz [požadované a volitelné vlastnosti](required-optional.md) Další informace informace o).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
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

### <a name="cascade-delete"></a>Kaskádové odstranění

Rozhraní Fluent API můžete explicitně nakonfigurovat chování kaskádové odstranění pro daný vztah.

Zobrazit [Kaskádové odstraňování](../saving/cascade-delete.md) v části ukládání dat pro podrobnou diskuzi o jednotlivých možností.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Další způsoby vztah

### <a name="one-to-one"></a>1: 1

Relace mít vlastnost navigace odkaz na obou stranách. Se řídí stejnými konvencemi jako jeden mnoho vztahů, ale je zaveden jedinečný index u vlastnosti cizího klíče k zajištění, že pouze jeden závislé souvisí s každý instanční objekt.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF zvolí jednu z entity, které chcete být závislé podle jeho schopnost detekovat vlastnost cizího klíče. Pokud je zvolená nesprávné entity jako závislé, můžete použít rozhraní Fluent API to pokud chcete opravit.

Při konfiguraci relace s rozhraním API Fluent, můžete použít `HasOne` a `WithOne` metody.

Při konfiguraci cizího klíče je třeba zadat typ entity závislé – Všimněte si, že obecný parametr předán `HasForeignKey` v seznamu níže. Ve vztahu jednoho k několika je jasné, že entity s navigační odkaz závisí a ten se shromažďováním je objekt zabezpečení. Ale to není tak v relaci 1: 1 - proto nutné explicitně definovat.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Many-to-many

Vztahy many-to-many bez třídu entity k reprezentaci tabulky spojení se zatím nepodporují. Můžete však představují vztah many-to-many zahrnutím třídu entity pro tabulku spojení a mapování dvě samostatné 1 n relace.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
