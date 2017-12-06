---
title: "Vztahy - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a>Vztahy

Relaci definuje, jak dvě entity vzájemně souvisejí. V relační databázi to je reprezentována omezení cizího klíče.

> [!NOTE]  
> Většina ukázky v tomto článku používá vztah jeden mnoho k předvedení konceptů. Příkladem relace 1: 1 a m: n, najdete v tématu [další relace vzory](#other-relationship-patterns) části na konci tohoto článku.

## <a name="definition-of-terms"></a>Definice pojmů

Existuje několik termínů používaných k popisu vztahů

* **Závislé entity:** Toto je typ entity, která obsahuje cizí klíčové vlastnosti. Někdy označuje jako "podřízený" relace.

* **Hlavní entity:** Toto je typ entity, která obsahuje primární nebo alternativní klíčové vlastnosti. Někdy označuje jako "nadřazeného" relace.

* **Cizí klíč:** vlastností v závislé entity, který se používá k ukládání hodnot hlavní vlastnost klíče, která entita má vztah k.

* **Hlavní klíč:** vlastností, která jednoznačně identifikuje hlavní entity. To může být primární klíč nebo alternativního klíče.

* **Navigační vlastnost:** vlastnost definovanou u hlavní nebo závislé entity, který obsahuje odkazy na související entity(s).

  * **Navigační vlastnost kolekce:** navigační vlastnost, která obsahuje odkazy na související entity.

  * **Odkazovat na navigační vlastnost:** navigační vlastnost, která obsahuje odkaz na jedné související entity.

  * **Inverzní navigační vlastnost:** když hovoříte o konkrétní navigační vlastnost, tento termín se vztahuje k navigační vlastnosti na druhém konci vztahu.

Následující výpis kódu ukazuje vztah jeden mnoho mezi `Blog` a`Post`

* `Post`je závislé entity

* `Blog`je hlavní entity

* `Post.BlogId`je cizí klíč

* `Blog.BlogId`je hlavní klíč (v tomto případě je primární klíč, nikoli alternativní klíč)

* `Post.Blog`je navigační vlastností odkazu

* `Blog.Posts`je navigační vlastnost kolekce

* `Post.Blog`inverzní navigační vlastnost třídy `Blog.Posts` (a naopak)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konvence

Podle konvence vztah vytvoří při navigační vlastnost zjištěných v typu. Vlastnost je považován za navigační vlastnost, pokud nemůže být namapován na typ, který odkazuje na jako skalární typ. aktuálním zprostředkovatelem databáze.

> [!NOTE]  
> Relace, která jsou vyhledána pomocí konvence vždy se zaměří na primární klíč objektu entity. Pokud chcete zacílit alternativní klíč, musí být provedeno další konfiguraci, pomocí rozhraní Fluent API.

### <a name="fully-defined-relationships"></a>Plně definované relace

Nejběžnější vzor pro relace je tak, aby měl navigační vlastnosti definované na obou konců relace a vlastností cizího klíče definovaný ve třídě závislé entity.

* Pokud se najde pár navigační vlastnosti mezi dvěma typy, bude nakonfigurován jako inverzní navigační vlastnosti stejné relace.

* Pokud závislé entity obsahuje vlastnost s názvem `<primary key property name>`, `<navigation property name><primary key property name>`, nebo `<principal entity name><primary key property name>` pak bude nakonfigurován jako cizí klíč.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Pokud nejsou k dispozici více navigační vlastnosti definované mezi dvěma typy (tj. více než jednu dvojici odlišné navigace, které odkazují na sobě navzájem), pak se vytvoří žádné relace podle konvence a budete muset ručně nakonfigurovat je pro identifikaci jak navigační vlastnosti spárovat.

### <a name="no-foreign-key-property"></a>Žádné vlastností cizího klíče

Když se doporučuje mít vlastností cizího klíče definovaný ve třídě závislé entity, není nutné. Pokud se nenajde žádný vlastností cizího klíče, bude nutné zavést stínové vlastností cizího klíče s názvem `<navigation property name><principal key property name>` (viz [vlastnosti stínové](shadow-properties.md) informace).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Jedné navigační vlastnosti

Včetně právě jedné navigační vlastnosti (žádné inverzní navigační a žádné vlastností cizího klíče) je dost má vztahy definované konvence. Můžete taky nechat jedné navigační vlastnosti kolekce a vlastností cizího klíče.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Kaskádové odstranění

Podle konvence je možnost kaskádové odstranění *Cascade* pro požadované relace a *ClientSetNull* pro volitelné relace. *CASCADE* znamená závislé entity budou také odstraněny. *ClientSetNull* znamená, že závislé entity, které nejsou načten do paměti zůstane beze změny a musí být ručně odstraněn nebo aktualizovat tak, aby odkazoval na platný hlavní entitu. Pro entity, která jsou načtena do paměti se pokusí EF základní vlastnosti cizího klíče nastavena na hodnotu null.

Najdete v článku [požadované a volitelné vztahy](#required-and-optional-relationships) části rozdíl mezi požadované a volitelné vztahy.

V tématu [Cascade Delete](../saving/cascade-delete.md) pro další podrobnosti o různých odstranit chování a výchozí hodnoty používané konvence.

## <a name="data-annotations"></a>Datových poznámek

Existují dvě poznámky dat, které můžete použít ke konfiguraci relace, `[ForeignKey]` a `[InverseProperty]`.

### <a name="foreignkey"></a>[Cizí klíč]

Anotací dat můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah. To se obvykle provádí, pokud není názvů vlastností cizího klíče.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]` Poznámky je možné použít u buď navigační vlastnost v atributu relationship. Není nutné přejít na navigační vlastnost ve třídě závislé entity.

### <a name="inverseproperty"></a>[InverseProperty]

Můžete nakonfigurovat, jak navigační vlastnosti na entity dependent a principal spárovat datových poznámek. To se obvykle provádí po více než jednu dvojici vlastností navigace mezi dvěma typy entit.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Konfigurace vztahu v rozhraní Fluent API, spusťte určením navigační vlastnosti, které tvoří relace. `HasOne`nebo `HasMany` identifikuje navigační vlastnost u zahájíte konfiguraci na typ entity. Potom tvoří řetěz volání `WithOne` nebo `WithMany` k identifikaci inverzní navigační. `HasOne`/`WithOne`slouží k navigační vlastnosti odkazu a `HasMany` / `WithMany` se používají pro navigační vlastnosti kolekce.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Jedné navigační vlastnosti

Pokud máte pouze jednu navigační vlastnost pak existují bez parametrů přetížení `WithOne` a `WithMany`. To znamená, že je koncepčně reference nebo collection na druhém konci relace, ale není součástí třídy entita navigační vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Cizí klíč

Rozhraní Fluent API můžete použít ke konfiguraci vlastností, které slouží jako vlastnost cizího klíče pro daný vztah.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Výpis následující kód ukazuje, jak nakonfigurovat složeného cizího klíče.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Můžete použít přetížení řetězec `HasForeignKey(...)` konfigurovat vlastnosti stínové jako cizí klíč (viz [vlastnosti stínové](shadow-properties.md) Další informace). Doporučujeme, abyste výslovně přidávání vlastnost stínové do modelu před jeho použitím jako cizí klíč (jak je znázorněno níže).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Hlavní klíč

Pokud chcete cizí klíč k odkazování vlastnost než primární klíč, můžete nakonfigurovat vlastnost hlavní klíče pro relaci rozhraní Fluent API. Vlastnost, která se konfiguruje automaticky jako hlavní klíče budou se instalační program jako alternativní klíč (viz [alternativní klíče](alternate-keys.md) Další informace).

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

Výpis následující kód ukazuje, jak nakonfigurovat složený hlavní klíč.

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
> Pořadí, ve kterém můžete zadat vlastnosti hlavního klíče musí odpovídat pořadí, ve kterém jsou uvedené pro cizí klíč.

### <a name="required-and-optional-relationships"></a>Povinné a nepovinné relace

Rozhraní Fluent API můžete nakonfigurovat, jestli je vztah požadované nebo volitelné. Nakonec tato volba určuje, zda je vlastnost cizího klíče požadované nebo volitelné. Toto je velmi užitečné, pokud používáte cizí klíč, který stínové stavu. Pokud máte vlastností cizího klíče v třídě entity, pak requiredness relace je určen na základě toho, jestli vlastnost cizího klíče požadované nebo volitelné (viz [požadované a volitelné vlastnosti](required-optional.md) Další informace informace o).

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

Rozhraní Fluent API můžete explicitně nakonfigurovat chování cascade delete pro daný vztah.

V tématu [Cascade Delete](../saving/cascade-delete.md) v oddílu ukládání dat pro podrobnou diskuzi o jednotlivých možností.

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

## <a name="other-relationship-patterns"></a>Dalšími vzory relace

### <a name="one-to-one"></a>1: 1

Relace 1: 1 mít navigační vlastnosti odkazu na obou stranách. Se řídí stejnými názvů jako jeden mnoho relace, ale jedinečný index byla zavedená v vlastností cizího klíče k zajištění, že pouze jeden závislé souvisí se každý objekt zabezpečení.

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
> EF vybere jeden z entity jako závislé položky podle jeho schopnost rozpoznat vlastností cizího klíče. Pokud jste vybrali nesprávný entity jako závislé položky, můžete tuto chybu opravit rozhraní Fluent API.

Při konfiguraci pomocí rozhraní Fluent API relace, je použít `HasOne` a `WithOne` metody.

Při konfiguraci cizí klíč, je třeba zadat typ závislé entity - Všimněte si obecný parametr poskytované `HasForeignKey` v seznamu níže. V vztah jeden mnoho je zřejmé, že entity s navigační odkaz je závislé a ten se kolekce je objekt zabezpečení. Není to Ano v relaci 1: 1 - proto potřeba explicitně definovat.

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

### <a name="many-to-many"></a>M m

Relace m: n bez třídu entity k připojení k tabulce představují ještě nejsou podporované. Ale může představovat relace m: n zahrnutím třídu entity pro připojení k tabulce a mapování dvě samostatné 1 n relace.

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
