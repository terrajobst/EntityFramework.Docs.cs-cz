---
title: Zpětná analýza – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: afe2c865305ade93dd10c8838b80c8b4177e7e8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197197"
---
# <a name="reverse-engineering"></a>Zpětná analýza

Zpětná analýza je proces třídy typu entity vytváření uživatelského rozhraní a třída DbContext založená na schématu databáze. Dá se provést pomocí `Scaffold-DbContext` EF Core příkazu nástroje PMC (Správce balíčků správce) `dotnet ef dbcontext scaffold` nebo příkazu rozhraní příkazového řádku (CLI) rozhraní .NET.

## <a name="installing"></a>Instalují

Před zpětnou metodologií budete muset nainstalovat buď [nástroje PMC](xref:core/miscellaneous/cli/powershell) (pouze Visual Studio), nebo nástroje rozhraní příkazového [řádku](xref:core/miscellaneous/cli/dotnet). Podrobnosti najdete v tématu odkazy.

Také budete muset nainstalovat vhodného [poskytovatele databáze](xref:core/providers/index) pro schéma databáze, u kterého chcete provést zpětnou analýzu.

## <a name="connection-string"></a>Připojovací řetězec

První argument příkazu je připojovací řetězec k databázi. Nástroje použijí tento připojovací řetězec ke čtení schématu databáze.

Způsob uvozovek a řídicího řetězce závisí na tom, jaké prostředí používáte ke spuštění příkazu. Konkrétní informace najdete v dokumentaci ke svému prostředí. Například prostředí PowerShell vyžaduje, abyste `$` znak vyhnuli, ale ne. `\`

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Konfigurace a tajné klíče uživatele

Pokud máte projekt ASP.NET Core, můžete použít `Name=<connection-string>` syntaxi ke čtení připojovacího řetězce z konfigurace.

Tato funkce dobře funguje s [nástrojem Správce tajných klíčů](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) , aby vaše heslo databáze bylo oddělené od základu kódu.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Název zprostředkovatele

Druhým argumentem je název poskytovatele. Název zprostředkovatele je obvykle stejný jako název balíčku NuGet poskytovatele.

## <a name="specifying-tables"></a>Určení tabulek

Všechny tabulky ve schématu databáze jsou ve výchozím nastavení zpětně analyzovány do typů entit. Můžete omezit, které tabulky budou zpětně analyzovány zadáním schémat a tabulek.

Parametr v PMC `--schema` a možnost v rozhraní příkazového řádku lze použít k zahrnutí všech tabulek v rámci schématu. `-Schemas`

`-Tables`(PMC) a `--table` (CLI) lze použít k zahrnutí specifických tabulek.

Chcete-li do PMC zahrnout více tabulek, použijte pole.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Chcete-li v rozhraní příkazového řádku zahrnout více tabulek, určete možnost několikrát.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Zachování názvů

Názvy tabulek a sloupců jsou pevně nastavené tak, aby lépe odpovídaly konvencím názvů .NET pro typy a vlastnosti ve výchozím nastavení. Když zadáte `--use-database-names` přepínač v PMC nebo v rozhraní příkazového řádku, zakážete tím toto chování s původními názvy databází co nejvíce. `-UseDatabaseNames` Neplatné identifikátory .NET budou pořád opravené a syntetizované názvy, jako jsou vlastnosti navigace, budou pořád odpovídat konvencím vytváření názvů .NET.

## <a name="fluent-api-or-data-annotations"></a>Rozhraní Fluent API nebo datové poznámky

Typy entit se ve výchozím nastavení konfigurují pomocí rozhraní API Fluent. Pokud `-DataAnnotations` je to možné, `--data-annotations` zadejte (PMC) nebo (CLI), aby se místo toho používaly datové poznámky.

Například při použití rozhraní Fluent API dojde k následujícímu generování uživatelského rozhraní:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Když použijete datové poznámky, vytvoří se toto uživatelské rozhraní:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Název DbContext

Název třídy DbContext ve vygenerovaném *obsahu* bude ve výchozím nastavení názvem databáze s příponou. Chcete-li zadat jiný než jeden `-Context` , použijte v `--context` PMC a v rozhraní příkazového řádku.

## <a name="directories-and-namespaces"></a>Adresáře a obory názvů

Třídy entit a třída DbContext jsou vygenerované do kořenového adresáře projektu a používají výchozí obor názvů projektu. Můžete zadat adresář, ve kterém jsou třídy vygenerované pomocí `-OutputDir` (PMC) nebo `--output-dir` (CLI). Obor názvů bude kořenový obor názvů a názvy všech podadresářů v kořenovém adresáři projektu.

Můžete také použít `-ContextDir` (PMC) a `--context-dir` (CLI) pro generování uživatelského rozhraní třídy DbContext do samostatného adresáře z tříd typu entity.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Jak to funguje

Zpětná analýza začíná čtením schématu databáze. Čte informace o tabulkách, sloupcích, omezeních a indexech.

V dalším kroku se pomocí informací o schématu vytvoří model EF Core. Tabulky slouží k vytváření typů entit. sloupce slouží k vytváření vlastností; a k vytváření relací se používají cizí klíče.

Nakonec se model používá ke generování kódu. Odpovídající třídy typů entit, rozhraní Fluent API a datové poznámky jsou vygenerované z důvodu opětovného vytvoření stejného modelu z vaší aplikace.

## <a name="limitations"></a>Omezení

* Ne vše o modelu lze znázornit pomocí schématu databáze. Například informace o [**hierarchiích dědičnosti**](../modeling/inheritance.md), [**vlastněných typech**](../modeling/owned-entities.md)a [**rozdělení tabulky**](../modeling/table-splitting.md) nejsou k dispozici ve schématu databáze. Z tohoto důvodu tyto konstrukce nebudou nikdy zpětně analyzovány.
* Kromě toho poskytovatel EF Core nemusí podporovat **některé typy sloupců** . Tyto sloupce nebudou zahrnuty do modelu.
* V EF Coreovém modelu můžete definovat [**tokeny souběžnosti**](../modeling/concurrency.md), aby se uživatelé nemohli současně aktualizovat stejnou entitu. Některé databáze mají speciální typ, který představuje tento typ sloupce (například rowversion v SQL Server). v takovém případě můžeme tyto informace zpětně analyzovat; jiné tokeny souběžnosti však nebudou zpětně analyzovány.
* [Funkce C# referenčního typu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types) není v současné době v zpětné analýze podporovaná: EF Core vždy generuje C# kód, který předpokládá, že funkce je zakázána. Například textové sloupce s možnou hodnotou null budou vygenerované jako vlastnost s typem `string` , ne `string?`, pomocí rozhraní Fluent API nebo datových poznámek, které slouží ke konfiguraci, jestli je vlastnost požadovaná nebo ne. Můžete upravit generovaný kód a nahradit je poznámkami, které C# mají hodnotu null. Podpora generování uživatelského rozhraní pro typy odkazů s možnou hodnotou null je sledována pomocí [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)problému.

## <a name="customizing-the-model"></a>Přizpůsobení modelu

Kód vygenerovaný EF Core je váš kód. Nebojte se změnit. Bude znovu vygenerována pouze v případě, že znovu budete provádět zpětnou analýzu stejného modelu. Generovaný kód reprezentuje *jeden* model, který se dá použít pro přístup k databázi, ale není to *jediný* model, který se dá použít.

Přizpůsobte třídy typu entity a třídu DbContext tak, aby vyhovovala vašim potřebám. Například se můžete rozhodnout přejmenovat typy a vlastnosti, zavést Hierarchie dědičnosti nebo rozdělit tabulku do více entit. Z modelu můžete také odebrat nejedinečné indexy, nepoužívané sekvence a navigační vlastnosti, volitelné skalární vlastnosti a názvy omezení.

Můžete také přidat další konstruktory, metody, vlastnosti atd. použití jiné částečné třídy v samostatném souboru. Tento přístup funguje i v případě, že máte v úmyslu provést zpětnou analýzu modelu.

## <a name="updating-the-model"></a>Aktualizace modelu

Po provedení změn v databázi bude pravděpodobně nutné aktualizovat model EF Core, aby odrážel tyto změny. Pokud se databáze změní na jednoduchá, může být nejjednodušší pouze ručně provést změny v modelu EF Core. Například přejmenování tabulky nebo sloupce, odebrání sloupce nebo aktualizace typu sloupce jsou triviální změny v kódu.

Důležitější změny se ale nedají snadno provést ručně. Jedním z běžných pracovních postupů je provést zpětnou analýzu modelu z databáze znovu `-Force` pomocí (PMC) `--force` nebo (CLI) a přepsat existující model pomocí aktualizovaného typu.

Další běžně vyžadovaná funkce je možnost aktualizovat model z databáze a přitom zachovat přizpůsobení jako přejmenování, hierarchií typů atd. Průběh této funkce můžete sledovat pomocí [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) problému.

> [!WARNING]
> Pokud znovu provedete zpětnou analýzu modelu z databáze, všechny změny, které jste provedli v souborech, budou ztraceny.
