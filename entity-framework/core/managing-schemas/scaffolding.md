---
title: Zpětná analýza – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459182"
---
# <a name="reverse-engineering"></a>Zpětná analýza

Zpětná analýza je proces generování uživatelského rozhraní entity typu třídy a třídy DbContext na základě schématu databáze. Je možné provádět pomocí `Scaffold-DbContext` příkaz nástroje EF Core Package Manageru konzoly (PMC) nebo `dotnet ef dbcontext scaffold` příkaz nástroje .NET rozhraní příkazového řádku (CLI).

## <a name="installing"></a>Instalace

Před zpětné analýzy, budete muset nainstalovat buď [PMC nástroje](xref:core/miscellaneous/cli/powershell) (pouze Visual Studio) nebo [nástroje rozhraní příkazového řádku](xref:core/miscellaneous/cli/dotnet). Zobrazit odkazy na podrobnosti.

Budete také muset nainstalovat odpovídající [poskytovatele databáze](xref:core/providers/index) pro schéma databáze, kterou chcete provést zpětnou analýzu.

## <a name="connection-string"></a>Připojovací řetězec

Prvním argumentem příkazu je připojovací řetězec k databázi. Nástroje použije tento připojovací řetězec k načtení schématu databáze.

Jak poptávka a escape připojovací řetězec, závisí na jaké prostředí používáte ke spuštění příkazu. V dokumentaci pro vaše prostředí pro konkrétní. Například, prostředí PowerShell vyžaduje, abyste řídicí `$` znaků, ale ne `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Konfigurace a tajných klíčů uživatelů

Pokud máte projekt ASP.NET Core, můžete použít `Name=<connection-string>` syntaxe získat připojovací řetězec z konfigurace.

Tento postup funguje dobře [nástroj tajný klíč správce](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) uchovávat toto heslo databáze odděleně od vašeho základu kódu.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Název poskytovatele

Druhým argumentem je název poskytovatele. Název zprostředkovatele je obvykle stejný jako název balíčku NuGet poskytovatele.

## <a name="specifying-tables"></a>Určení tabulky

Všechny tabulky ve schématu databáze jsou zpětně analyzovány na typy entit ve výchozím nastavení. Můžete omezit, které tabulky jsou zpětně navržené tak, že zadáte schémat a tabulek.

`-Schemas` Parametr v konzole PMC a `--schema` možnost v rozhraní příkazového řádku je možné zahrnout každá tabulka v rámci schématu.

`-Tables` (PMC) a `--table` (CLI) umožňuje zahrnout konkrétní tabulky.

Zahrnout více tabulek v konzole PMC, použijte pole.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Chcete-li zahrnout více tabulek v rozhraní příkazového řádku, zadejte možnost více než jednou.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Zachování názvy

Názvy tabulek a sloupců tak, aby lépe odpovídaly zásady vytváření názvů .NET pro typy a vlastnosti jsou oprava ve výchozím nastavení. Zadání `-UseDatabaseNames` přepínače v konzole PMC nebo `--use-database-names` možnost v rozhraní příkazového řádku se zakázat toto chování zachovat původní názvy databází co největší míře. Neplatné identifikátory rozhraní .NET stále opravíme a syntetizovaný názvy jako vlastnosti navigace se stále odpovídat zásady vytváření názvů .NET.

## <a name="fluent-api-or-data-annotations"></a>Rozhraní Fluent API nebo datové poznámky

Typy entit jsou nakonfigurované pomocí rozhraní Fluent API ve výchozím nastavení. Zadejte `-DataAnnotations` (PMC) nebo `--data-annotations` (CLI) pro náhradní použití anotací dat, pokud je to možné.

Například pomocí rozhraní Fluent API bude generování uživatelského rozhraní toto:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Při používání datových poznámek bude generování uživatelského rozhraní toto:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Název DbContext

Název vygenerované třídy DbContext bude název databáze doplněny *kontextu* ve výchozím nastavení. Pokud chcete zadat jinou, použijte `-Context` v konzole PMC a `--context` v rozhraní příkazového řádku.

## <a name="directories-and-namespaces"></a>Adresáře a obory názvů

Entity třídy a třídy DbContext jsou automaticky do kořenového adresáře projektu a použijte výchozí obor názvů projektu. Lze určit adresář, kde tříd jsou automaticky pomocí `-OutputDir` (PMC) nebo `--output-dir` (rozhraní příkazového řádku). Obor názvů bude kořenového oboru názvů a názvů jakéhokoliv podadresáře v kořenovém adresáři projektu.

Můžete také použít `-ContextDir` (PMC) a `--context-dir` (CLI) do samostatných adresáře z tříd entit typu scaffold třídy DbContext.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Jak to funguje

Zpětná analýza začne schématu databáze pro čtení. Načte informace o tabulky, sloupce, omezení a indexy.

V dalším kroku použije informace o schématu pro vytvoření modelu EF Core. Tabulky se používají k vytvoření typů entit; sloupce se použijí k vytvoření vlastnosti; a cizí klíče slouží k vytvoření relací.

Nakonec model se používá ke generování kódu. Chcete-li znovu vytvořit stejný model z vaší aplikace jsou automaticky generovaný odpovídající entity typu třídy, rozhraní Fluent API a data poznámky.

## <a name="what-doesnt-work"></a>Co nefunguje

Ne vše, co o modelu lze znázornit pomocí schématu databáze. Například informace o **hierarchie dědičnosti**, **vlastní typy**, a **tabulky rozdělení** nejsou k dispozici ve schématu databáze. Z toho důvodu tyto konstrukce nikdy se vrátíte zpět inženýrství.

Kromě toho **některé typy sloupců** nemusí být podporována zprostředkovatelem EF Core. Tyto sloupce nebudou zahrnuty v modelu.

EF Core vyžaduje, aby každý typ entity klíč. Tabulky, není však nutné nastavit primární klíč. **Tabulky s primárním klíčem** jsou aktuálně není zpětnou analýzou.

Můžete definovat **tokeny souběžnosti** v modelu EF Core dvě uživatelům zabránit v aktualizaci stejné entity ve stejnou dobu. Některé databáze mají speciální typ pro reprezentaci tohoto typu sloupce (například rowversion v systému SQL Server) v takovém případě lze zrušit jsme pracovníkovi tyto informace; však další tokeny souběžnosti nesmí být zpětná analýza.

## <a name="customizing-the-model"></a>Přizpůsobení modelu

Kód vygenerovaný EF Core je váš kód. Můžete ho změnit. To se znovu vygeneruje jenom Pokud znovu provést zpětnou analýzu stejného modelu. Automaticky generovaný kód představuje *jeden* model, který můžete použít pro přístup k databázi, ale určitě není *pouze* model, který lze použít.

Upravte entity typu třídy a třídy DbContext podle vašich potřeb. Můžete například přejmenovat typy a vlastnosti, zavést hierarchie dědičnosti nebo rozdělení tabulky do více entit. Jedinečné indexy, nevyužité pořadí a navigačních vlastností, volitelné Skalární vlastnosti a omezení názvů můžete také odebrat z modelu.

Můžete také přidat další konstruktorů, metod, vlastností, atd. pomocí jiné částečné třídy v samostatném souboru. Tento postup funguje i v případě, že máte v úmyslu znovu provést zpětnou analýzu modelu.

## <a name="updating-the-model"></a>Aktualizace modelu

Po provedení změn v databázi, budete muset aktualizovat tak, aby odrážela tyto změny modelu EF Core. Pokud jsou jednoduché změny databáze, může být nejjednodušší jenom ručně provést změny modelu EF Core. Třeba přejmenování tabulky nebo sloupce, odebráním sloupce nebo aktualizace sloupce typu jsou jednoduché změny v kódu.

Další významné změny, ale nejsou jako snadno vytvořit ručně. Jednou z běžných pracovních postupů je provést zpětnou analýzu model z databáze, znovu pomocí `-Force` (PMC) nebo `--force` (CLI) k přepsání existujícího modelu aktualizované sadou.

Další běžně požadovaných funkcí je schopnost aktualizace modelu z databáze při zachování vlastního nastavení, jako je přejmenování, hierarchie typů, atd. Použít problém [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) sledovat průběh této funkce.

> [!WARNING]
> Pokud zpětné analýze modelu z databáze znovu, budou ztraceny všechny změny, které jste provedli v souborech.
