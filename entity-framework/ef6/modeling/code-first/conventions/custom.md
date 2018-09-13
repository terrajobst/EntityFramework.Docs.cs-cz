---
title: Vlastní kód první konvence - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489841"
---
# <a name="custom-code-first-conventions"></a>První vytváření vlastního kódu
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

Při použití Code First modelu se počítá od tříd pomocí sady konvence. Výchozí hodnota [první konvence kódu](~/ef6/modeling/code-first/conventions/built-in.md) určit věci, třeba která vlastnost se stane primární klíč entity, názvu entity se mapuje na tabulku a jaké hodnot precision a scale desítkové sloupec má ve výchozím nastavení.

Někdy těchto výchozích konvencí nejsou ideální pro váš model, a je nutné je obejít tím, že nakonfigurujete jednotlivé entity pomocí anotacemi dat nebo rozhraní Fluent API. Vlastní první konvence kódu umožňují definovat vlastní zásady odvíjející, které poskytují výchozí konfigurační hodnoty pro váš model. V tomto názorném postupu se podíváme na různé typy vlastní konvence a jak každý z nich vytvořit.


## <a name="model-based-conventions"></a>Vytváření názvů založených na modelu

Tato stránka popisuje DbModelBuilder rozhraní API pro vlastní konvence. Toto rozhraní API by mělo být dostatečné pro většinu vlastní konvence vytváření. Existuje ale také možnost vytvářet založené na modelu konvencí – konvence, které pracují s finálního modelu, jakmile se vytvoří – pro pokročilé scénáře zpracování. Další informace najdete v tématu [založené na modelu konvencí](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Náš Model

Začněme tím, že definování jednoduchého modelu, můžeme použít s naší konvence. Přidejte následující třídy do projektu.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Úvod do vytváření vlastního

Napíšeme úmluvy, který konfiguruje všechny vlastnosti s názvem klíče se primární klíč pro daný typ entity.

Konvence jsou povolené na tvůrce modelu, který se dá dostat tak, že přepíšete OnModelCreating v kontextu. Aktualizace třídy ProductContext následujícím způsobem:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Nyní, žádné vlastnosti v náš model s názvem klíče budou nakonfigurované jako primární klíč entity, ať jeho součástí.

Může také neposkytujeme naše konvence konkrétnější pomocí filtrování podle typu vlastnosti, které budeme konfigurace:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Tím se nakonfiguruje všechny vlastnosti volá klíčem k zajištění primární klíče jejich entity, ale pouze v případě, že se celé číslo.

Zajímavé funkce IsKey metody je, že je sčítání. To znamená, že pokud zavoláte na více vlastností IsKey a se stanou součástí složený klíč. Jeden výstrahou to je, že pokud zadáte více vlastností pro klíč musíte zadat také objednávky pro tyto vlastnosti. Toto lze provést zavoláním HasColumnOrder metoda podobná níže uvedenému příkladu:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Tento kód se nakonfiguruje typy v náš model má složený klíč skládající se z klíčový sloupec int a ve sloupci Název řetězce. Pokud jsme v Návrháři zobrazení modelu by vypadalo takto:

![složený klíč](~/ef6/media/compositekey.png)

Další příklad konvence vlastnost je konfigurace všechny vlastnosti data a času v mé modelu mapovat na typ datetime2 v systému SQL Server namísto data a času. Toho můžete dosáhnout s následujícími možnostmi:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Vytváření názvů tříd

Dalším způsobem definování konvence je použít k zapouzdření vaše konvence vytváření názvů tříd. Při použití konvence třídy vytvořit typ, který dědí z třídy vytváření názvů v oboru názvů System.Data.Entity.ModelConfiguration.Conventions.

Můžeme vytvořit třídu konvence s konvencí datetime2, který jsme vám ukázali výše následujícím způsobem:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Chcete-li zjistit EF používat tato konvence je přidat do kolekce konvence OnModelCreating, což bude v případě, že jste postupovali podle spolu s návodu vypadat například takto:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Jak je vidět, že můžeme přidat instanci naše konvence ke kolekci konvence. Dědění z konvencí poskytuje pohodlný způsob seskupení a konvence sdílení napříč týmy nebo projekty. Můžete například mít knihovny tříd pomocí společnou sadu konvence, že všechny vaše organizace projekty použití.

 

## <a name="custom-attributes"></a>Vlastní atributy

Další skvělé konvence slouží k povolení nové atributy pro použití při konfiguraci modelu. Pro znázornění, Pojďme vytvořit atribut, který můžete použít k označení vlastnosti řetězce jako kódování Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Teď vytvoříme konvence použití tohoto atributu na náš model:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

S touto konvencí jsme můžete přidat atribut NonUnicode k některé z našich vlastnosti řetězce, které znamená, že sloupce v databázi se uloží jako varchar místo nvarchar.

Jedna věc, kterou byste měli znát Tato konvence je, že když vložíte NonUnicode atribut na nic jiného než vlastnost řetězce a vyvolá výjimku. Dělá to, protože nelze nakonfigurovat IsUnicode v jakéhokoli typu než řetězce. Pokud k tomu dojde, pak můžete vytvořit vaše konvence konkrétnější, tak, aby odfiltruje cokoli, co není řetězec.

Při výše konvence se dá použít pro definování uživatelských atributů, které je jiné rozhraní API, které může být mnohem jednodušší, pokud chcete použít, zejména, pokud chcete použít vlastnosti z třídy atributů.

V tomto příkladu budeme aktualizovat naše atribut a změňte ji na atribut IsUnicode tak vypadá takto:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Jakmile budeme mít, jsme nastavili bool na naše atribut říct úmluvy Určuje, jestli vlastnost by měla být v kódování Unicode. Můžeme to udělat v konvenci, která jsme již díky přístupu do ClrProperty třídu konfigurace takto:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Toto je docela jednoduché, ale není tak stručnější hodí pomocí Having metodu vytváření rozhraní API. S metoda má parametr typu Func&lt;PropertyInfo, T&gt; která přijímá PropertyInfo stejný jako Where metody, ale očekává se vrátit objekt. Pokud vrácený objekt má hodnotu null, pak vlastnost nenakonfigurují, což znamená, že můžete filtrovat vlastnosti s ní stejně jako Where, ale se liší, bude také zaznamenání vráceného objektu a předejte metodě konfigurace. Tento postup funguje takto:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Vlastní atributy nejsou pouze z důvodu použití Having metoda, je užitečné kdekoli, budete muset o něco, co se na filtrování při konfiguraci typy nebo vlastnosti.

 

## <a name="configuring-types"></a>Konfigurace typů

Zatím byly všechny naše konvence pro vlastnosti, ale existuje jiné oblasti vytváření rozhraní API pro konfiguraci typy ve vašem modelu. Možnosti jsou podobné zásady, které jsme zatím viděli, ale možnosti konfigurace uvnitř bude na entitu namísto vlastnosti úrovně.

Jednou z věcí, které mohou být velmi užitečné pro typ úrovně konvence je změna zásady vytváření tabulky mapování na stávajícím schématu, která se liší od výchozí EF nebo vytvořit novou databázi pomocí jiné zásady vytváření názvů. K tomu potřeba nejdřív metodu, která může přijmout TypeInfo pro typ v náš model a vrátit, co by měl být název tabulky pro daný typ:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Tato metoda přebírá typ a vrátí řetězec, který používá malé místo CamelCase podtržítky. V našem modelu to znamená, že třída ProductCategory budou zmapována do tabulky nazvané produktu\_místo oddělovaly kategorie.

Jakmile budeme mít metody říkáme mu v konvenci takto:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Tato konvence nakonfiguruje všechny typy v náš model mapovat na název tabulky, která je vrácena z našich GetTableName metody. Tato konvence je ekvivalentem k volání metody ToTable pro každou entitu v modelu s použitím rozhraní Fluent API.

Poznámka o tomto je, že při volání bude trvat ToTable EF, řetězec, který zadáte jako název tabulky přesné, bez jakékoli pluralizace, kterou by obvykle provádět při určování názvů tabulek. To je důvod, proč je název tabulky z našich konvence produkt\_kategorie místo produktu\_kategorií. Můžeme vyřešit, v našem konvence tím, že zavoláte na službu pluralizace sami.

V následujícím kódu budeme používat [řešení závislostí](~/ef6/fundamentals/configuring/dependency-resolution.md) funkce přidá EF6 načtení pluralizace služby, které byste použili EF a pluralize naše název tabulky.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> Obecné verzi GetService je rozšiřující metodu v oboru názvů System.Data.Entity.Infrastructure.DependencyResolution, budete muset přidat, pomocí příkazu pro váš kontext, aby bylo možné ho použít.

### <a name="totable-and-inheritance"></a>ToTable a dědičnost

Další důležitý aspekt ToTable je, že pokud namapujete typ explicitně do dané tabulky, je možné změnit mapování strategie, kterou bude používat EF. Při volání ToTable pro každý typ v hierarchii dědičnosti, předá název typu jako název tabulky, jak jsme to udělali výše, pak změníte výchozí strategie mapování na hierarchii tabulky (TPH) na typ tabulky (TPT). Nejlepší způsob, jak to popisují je whith konkrétní příklad:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Ve výchozím nastavení zaměstnance a správce jsou namapovány na stejnou tabulku (zaměstnanci) v databázi. V tabulce bude obsahovat zaměstnancům i sloupec diskriminátoru, která vám dá vědět, jaký typ instance je uložen v jednotlivých řádcích. Toto je TPH mapování je jedné tabulky v hierarchii. Ale při volání ToTable na obou clase pak každý typ bude místo toho mapovat na svou vlastní tabulku, označované také jako TPT vzhledem k tomu, že každý typ má svou vlastní tabulku.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Výše uvedený kód se namapuje do struktury tabulky, který vypadá takto:

![Tpt příklad](~/ef6/media/tptexample.jpg)

Můžete-li tomu zabránit a udržovat TPH výchozí mapování několika způsoby:

1.  Volání ToTable se stejným názvem tabulky pro každý typ v hierarchii.
2.  ToTable volejte pouze pro základní třídy v hierarchii, v našem příkladu, která by byla zaměstnance.

 

## <a name="execution-order"></a>Pořadí provádění

Konvence pracovat v posledním způsobem wins, stejná jako rozhraní Fluent API. Co to znamená, že je, že pokud napíšete dvě vytváření názvů, které se stejným nastavením možnosti této vlastnosti a pak poslední z nich ke spuštění služby wins. Například v následujícím kódu maximální délka všechny řetězce nastavená na 500, ale potom nakonfigurujeme všechny vlastnosti název v modelu, který má mít maximální délku 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Protože konvence pro nastavení maximální délky na 250 je po ten, který nastaví všechny řetězce na 500, všechny vlastnosti v našich modelu jako název budou mít MaxLength 250 při libovolné řetězce, jako jsou popisy, 500. Pomocí konvencí tímto způsobem znamená, že můžete zadat obecné konvence pro typy nebo vlastnosti v modelu a poté toto pro podmnožiny, které se liší.

Rozhraní Fluent API a anotacemi dat lze použít také k přepsání konvence ve zvláštních případech. V našem příkladu rozhraní Fluent API měli použili k nastavení maximální délka vlastnosti pak jsme může mít ji umístit před nebo po konvence, protože konkrétnější rozhraní Fluent API vyhraje přes obecnější konvence konfigurace.

 

## <a name="built-in-conventions"></a>Integrované konvence

Protože konvence vlastní mohou být ovlivněny výchozích konvencí Code First, může být užitečné pro přidání konvence pro spuštění před nebo po jiném konvence. K tomu můžete použít metody AddBefore a AddAfter konvence kolekce na odvozené DbContext. Následující kód přidejte třídu konvence jsme vytvořili dříve, tak, aby se spustí před integrovaná v klíčových zjišťování konvence.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

To bude nejvíc použití při přidávání vytváření názvů, které je potřeba spustit před nebo po integrované konvence, seznam integrované vytváření najdete tady: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Můžete také odebrat vytváření názvů, které nechcete použít pro váš model. K odebrání konvence, použijte metodu odebrat. Tady je příklad odebrání PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
