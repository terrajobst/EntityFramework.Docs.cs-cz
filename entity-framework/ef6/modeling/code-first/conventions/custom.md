---
title: Vlastní konvence Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419225"
---
# <a name="custom-code-first-conventions"></a>Vlastní konvence Code First
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

Při použití Code First se model počítá z vašich tříd pomocí sady konvencí. Výchozí [konvence Code First](~/ef6/modeling/code-first/conventions/built-in.md) určují věci, jako je například vlastnost, která se změní na primární klíč entity, název tabulky, na kterou se entita mapuje, a jaká přesnost a velikost sloupce desetinného čísla mají ve výchozím nastavení.

Tyto výchozí konvence jsou někdy ideální pro váš model a vy je budete muset vyřešit tak, že nakonfigurujete mnoho jednotlivých entit pomocí datových poznámek nebo rozhraní API Fluent. Vlastní konvence Code First umožňují definovat vlastní konvence, které pro váš model poskytují výchozí nastavení konfigurace. V tomto návodu budeme prozkoumat různé typy vlastních konvencí a postup, jak je vytvořit.


## <a name="model-based-conventions"></a>Konvence založené na modelu

Tato stránka pokrývá rozhraní DbModelBuilder API pro vlastní konvence. Toto rozhraní API by mělo být dostačující pro vytváření většiny vlastních konvencí. Existuje však také možnost vytvářet konvence založené na modelu, které zpracovávají konečný model po jeho vytvoření – pro zpracování pokročilých scénářů. Další informace najdete v tématu [konvence založené na modelu](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Náš model

Pojďme začít definováním jednoduchého modelu, který můžeme použít s našimi konvencemi. Do projektu přidejte následující třídy.

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

 

## <a name="introducing-custom-conventions"></a>Představujeme vlastní konvence

Pojďme napsat konvenci, která nakonfiguruje libovolnou vlastnost s názvem klíč na primární klíč pro svůj typ entity.

V Tvůrci modelů jsou povoleny konvence, které mohou být zpřístupněny přepsáním OnModelCreating v kontextu. Aktualizujte třídu ProductContext následujícím způsobem:

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

Teď bude jakákoli vlastnost v našem modelu s názvem Key nakonfigurovaná jako primární klíč jakékoli entity jeho součásti.

Naše konvence můžeme také lépe specifikovat filtrováním typu vlastnosti, kterou chceme nakonfigurovat:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Tím se nakonfigurují všechny vlastnosti s názvem klíč jako primární klíč své entity, ale jenom v případě, že se jedná o celé číslo.

Zajímavou funkcí metody vlastností IsKey nastavenou je, že je aditivní. To znamená, že pokud voláte vlastností IsKey nastavenou u více vlastností a všechny budou stávají součástí složeného klíče. Tato výstraha je taková, že když pro klíč zadáte více vlastností, musíte zadat také pořadí těchto vlastností. To lze provést voláním metody HasColumnOrder, například:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Tento kód nakonfiguruje typy v našem modelu tak, aby měly složený klíč tvořený sloupcem klíče int a sloupce názvu řetězce. Pokud se model zobrazuje v návrháři, může vypadat takto:

![složený klíč](~/ef6/media/compositekey.png)

Dalším příkladem konvence vlastností je nakonfigurovat všechny vlastnosti DateTime v modelu můj model tak, aby se místo hodnoty DateTime namapovaly na datetime2 typ v SQL Server. Můžete to dosáhnout následujícím způsobem:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Třídy konvence

Dalším způsobem definování konvencí je použití třídy konvence k zapouzdření vaší konvence. Při použití třídy konvence pak vytvoříte typ, který dědí z třídy konvence v oboru názvů System. data. entity. ModelConfiguration. Convention.

Můžete vytvořit třídu konvence s konvencí datetime2, kterou jsme dříve zjistili pomocí následujícího postupu:

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

Chcete-li, aby EF používalo tuto konvenci, přidejte ji do kolekce konvence v OnModelCreating, kterou jste měli v případě, že budete postupovat podle pokynů společně s návodem, bude vypadat takto:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Jak vidíte, přidáme instanci naší konvence do kolekce konvence. Dědění z konvence nabízí pohodlný způsob seskupování a sdílení v rámci týmů nebo projektů. Můžete například mít knihovnu tříd se společnou sadou konvencí, kterou používají všechny projekty vaší organizace.

 

## <a name="custom-attributes"></a>Vlastní atributy

Dalším velkým používáním konvencí je povolit použití nových atributů při konfiguraci modelu. Pro ilustraci si vytvoříme atribut, který můžeme použít k označení vlastností řetězce jako jiné než Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Nyní vytvoříme konvenci pro použití tohoto atributu pro náš model:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

V této úmluvě můžeme přidat atribut nepodporující sadu Unicode do žádné z našich vlastností řetězce, což znamená, že sloupec v databázi bude uložen jako varchar namísto typu nvarchar.

Jednou z věcí k této konvenci je, že pokud zadáte atribut nepodporující kódování Unicode na jinou hodnotu než vlastnost řetězce, vyvolá se výjimka. To je způsobeno tím, že nemůžete nakonfigurovat kódování UTF pro jakýkoliv typ jiný než řetězec. Pokud k tomu dojde, můžete nastavit konkrétní konvenci tak, aby odfiltroval cokoli, co není řetězec.

I když výše uvedená konvence funguje pro definování vlastních atributů, existuje jiné rozhraní API, které může být mnohem jednodušší, zejména v případě, že chcete použít vlastnosti z třídy atributů.

V tomto příkladu budeme aktualizovat náš atribut a změnit ho na atribut typu s kódováním Unicode, aby vypadal takto:

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

Jakmile to máme, můžeme pro náš atribut nastavit bool, která určuje, jestli má být vlastnost nastavená na Unicode. To jsme mohli udělat v konvenci, kterou už máme přístup k ClrProperty třídy konfigurace, jako je tato:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

To je dostatečně snadné, ale existuje stručnější způsob, jak toho dosáhnout pomocí metody rozhraní API pro konvence. Metoda having má parametr typu Func&lt;PropertyInfo, T&gt;, který přijímá PropertyInfo stejné jako metoda Where, ale očekává se, že vrátí objekt. Pokud je vrácený objekt null, vlastnost nebude nakonfigurována, což znamená, že můžete vyfiltrovat vlastnosti, které mají, stejně jako v případě, že se ale liší v tom, že bude také zachytit vrácený objekt a předat ho metodě configure. To funguje podobně jako u následujících:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Vlastní atributy nejsou jediným důvodem, proč použít metodu having, je užitečné všude, kde je třeba mít důvod na něco, co filtrujete při konfiguraci typů nebo vlastností.

 

## <a name="configuring-types"></a>Konfigurace typů

Všechny naše konvence byly proto pro vlastnosti, ale existuje další oblast rozhraní API konvence pro konfiguraci typů v modelu. Prostředí je podobné konvencím, které jsme doposud viděli, ale možnosti v části konfigurovat budou na entitě namísto úrovně vlastností.

Jedna z věcí, ke kterým se konvence na úrovni typů může skutečně hodit, je změna zásady vytváření názvů tabulek, buď k namapování na existující schéma, které se liší od výchozího nastavení EF, nebo k vytvoření nové databáze s různými konvencemi vytváření názvů. Abychom to mohli udělat, nejdřív potřebujeme metodu, která může přijmout volání TypeInfo pro typ v našem modelu a vracet, jak by měl být název tabulky tohoto typu:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Tato metoda přebírá typ a vrátí řetězec, který používá malá písmena s podtržítky namísto CamelCase. V našem modelu to znamená, že třída ProductCategory bude namapována na tabulku s názvem produkt\_kategorie místo na ProductCategories.

Až tuto metodu máme, můžeme ji volat v konvenci, jako je tato:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Tato konvence nakonfiguruje všechny typy v našem modelu pro mapování na název tabulky, který je vrácen z naší metody GetTable. Tato konvence je ekvivalentem volání metody ToTable pro každou entitu v modelu pomocí rozhraní Fluent API.

Všimněte si, že když zavoláte ToTable EF, převezme řetězec, který zadáte jako přesný název tabulky, bez jakékoli plurality, která by obvykle při určování názvů tabulek. Důvodem je, že název tabulky z naší konvence je produkt\_kategorie místo kategorií produktů\_. Můžeme to vyřešit v naší úmluvě tím, že zavoláte dodržovali služby pro pluralitování.

V následujícím kódu budeme používat funkci [překladu závislosti](~/ef6/fundamentals/configuring/dependency-resolution.md) přidanou v EF6 k načtení služby pro pojmenování, kterou použil EF, a doplnit jednotné našeho názvu tabulky.

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
> Obecná verze GetService je metoda rozšíření v oboru názvů System. data. entity. Infrastructure. DependencyResolution, budete muset přidat příkaz using do svého kontextu, aby ho bylo možné použít.

### <a name="totable-and-inheritance"></a>ToTable a dědičnost

Dalším důležitým aspektem ToTable je, že pokud explicitně namapujete typ na danou tabulku, můžete změnit strategii mapování, kterou EF bude používat. Pokud zavoláte ToTable pro každý typ v hierarchii dědičnosti, dáte název typu jako název tabulky, jako jsme výše, a pak změníte výchozí strategii mapování tabulky na typ (TPT) na typ Table-per-Type (). Nejlepším způsobem, jak to popsat, je whith konkrétní příklad:

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

Ve výchozím nastavení jsou zaměstnanci i manažer namapováni na stejnou tabulku (zaměstnanci) v databázi. Tabulka bude obsahovat zaměstnance i manažery se sloupcem diskriminátoru, který vám sdělí, jaký typ instance je uložený v jednotlivých řádcích. Toto je mapování typu TPH, protože pro hierarchii existuje jedna tabulka. Nicméně pokud voláte ToTable na obou Classe, pak každý typ bude mapován na vlastní tabulku, označovanou také jako TPT, protože každý typ má svou vlastní tabulku.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Výše uvedený kód se namapuje na strukturu tabulky, která vypadá nějak takto:

![Příklad TPT](~/ef6/media/tptexample.jpg)

Můžete tomu předejít a zachovat výchozí mapování TPH několika způsoby:

1.  Pro každý typ v hierarchii zavolejte ToTable se stejným názvem tabulky.
2.  Volejte ToTable jenom pro základní třídu hierarchie, v našem příkladu, který by byl zaměstnanec.

 

## <a name="execution-order"></a>Pořadí spouštění

Konvence fungují jako poslední způsob služby WINS, stejně jako rozhraní Fluent API. To znamená, že pokud píšete dvě konvence, které nakonfigurují stejnou možnost stejné vlastnosti, pak poslední z nich spustí službu WINS. Například v kódu níže je maximální délka všech řetězců nastavená na 500, ale my nakonfigurujeme všechny vlastnosti s názvem název v modelu tak, aby měly maximální délku 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Vzhledem k tomu, že konvence, která má nastavit maximální délku na 250, je po jedné, která nastaví všechny řetězce na 500, všechny vlastnosti s názvem v našem modelu budou mít hodnotu MaxLength 250, zatímco jakékoli jiné řetězce, například popisy, by byly 500. Použití konvencí tímto způsobem znamená, že můžete zadat obecnou konvenci pro typy nebo vlastnosti v modelu a pak je overide pro podmnožiny, které jsou odlišné.

Rozhraní Fluent API a datové poznámky lze použít také k přepsání konvence v určitých případech. Pokud jsme v našem příkladu používali rozhraní Fluent API k nastavení maximální délky vlastnosti, můžeme ji vložit před nebo po této konvenci, protože konkrétnější rozhraní API Fluent se podaří v obecnější konvenci konfigurace.

 

## <a name="built-in-conventions"></a>Předdefinované konvence

Vzhledem k tomu, že vlastní konvence můžou být ovlivněné výchozími konvencemi Code First, může být užitečné přidat konvence, které se spustí před nebo po jiné úmluvě. K tomu můžete použít metody AddBefore a AddAfter kolekce konvence na odvozeném DbContext. Následující kód by přidal třídu konvence, kterou jsme vytvořili dříve, aby se spustila před integrovanou konvencí zjišťování klíčů.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Tato možnost se bude přecházet z největšího počtu použití při přidávání konvencí, které je třeba spustit před nebo po předdefinovaných konvencích. seznam integrovaných konvencí najdete tady: [System. data. entity. ModelConfiguration. Conventions obor názvů](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).

Můžete také odebrat konvence, které nechcete použít pro váš model. Chcete-li odebrat konvenci, použijte metodu Remove. Tady je příklad odebrání PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
