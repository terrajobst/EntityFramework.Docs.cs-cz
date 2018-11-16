---
title: Zpětná analýza – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688677"
---
# <a name="reverse-engineering"></a><span data-ttu-id="e866f-102">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="e866f-102">Reverse Engineering</span></span>

<span data-ttu-id="e866f-103">Zpětná analýza je proces generování uživatelského rozhraní entity typu třídy a třídy DbContext na základě schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e866f-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="e866f-104">Je možné provádět pomocí `Scaffold-DbContext` příkaz nástroje EF Core Package Manageru konzoly (PMC) nebo `dotnet ef dbcontext scaffold` příkaz nástroje .NET rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="e866f-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="e866f-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="e866f-105">Installing</span></span>

<span data-ttu-id="e866f-106">Před zpětné analýzy, budete muset nainstalovat buď [PMC nástroje](xref:core/miscellaneous/cli/powershell) (pouze Visual Studio) nebo [nástroje rozhraní příkazového řádku](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="e866f-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="e866f-107">Zobrazit odkazy na podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e866f-107">See links for details.</span></span>

<span data-ttu-id="e866f-108">Budete také muset nainstalovat odpovídající [poskytovatele databáze](xref:core/providers/index) pro schéma databáze, kterou chcete provést zpětnou analýzu.</span><span class="sxs-lookup"><span data-stu-id="e866f-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="e866f-109">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="e866f-109">Connection string</span></span>

<span data-ttu-id="e866f-110">Prvním argumentem příkazu je připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="e866f-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="e866f-111">Nástroje použije tento připojovací řetězec k načtení schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e866f-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="e866f-112">Jak poptávka a escape připojovací řetězec, závisí na jaké prostředí používáte ke spuštění příkazu.</span><span class="sxs-lookup"><span data-stu-id="e866f-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="e866f-113">V dokumentaci pro vaše prostředí pro konkrétní.</span><span class="sxs-lookup"><span data-stu-id="e866f-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="e866f-114">Například, prostředí PowerShell vyžaduje, abyste řídicí `$` znaků, ale ne `\`.</span><span class="sxs-lookup"><span data-stu-id="e866f-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="e866f-115">Konfigurace a tajných klíčů uživatelů</span><span class="sxs-lookup"><span data-stu-id="e866f-115">Configuration and User Secrets</span></span>

<span data-ttu-id="e866f-116">Pokud máte projekt ASP.NET Core, můžete použít `Name=<connection-string>` syntaxe získat připojovací řetězec z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e866f-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="e866f-117">Tento postup funguje dobře [nástroj tajný klíč správce](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) uchovávat toto heslo databáze odděleně od vašeho základu kódu.</span><span class="sxs-lookup"><span data-stu-id="e866f-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="e866f-118">Název poskytovatele</span><span class="sxs-lookup"><span data-stu-id="e866f-118">Provider name</span></span>

<span data-ttu-id="e866f-119">Druhým argumentem je název poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="e866f-119">The second argument is the provider name.</span></span> <span data-ttu-id="e866f-120">Název zprostředkovatele je obvykle stejný jako název balíčku NuGet poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="e866f-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="e866f-121">Určení tabulky</span><span class="sxs-lookup"><span data-stu-id="e866f-121">Specifying tables</span></span>

<span data-ttu-id="e866f-122">Všechny tabulky ve schématu databáze jsou zpětně analyzovány na typy entit ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e866f-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="e866f-123">Můžete omezit, které tabulky jsou zpětně navržené tak, že zadáte schémat a tabulek.</span><span class="sxs-lookup"><span data-stu-id="e866f-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="e866f-124">`-Schemas` Parametr v konzole PMC a `--schema` možnost v rozhraní příkazového řádku je možné zahrnout každá tabulka v rámci schématu.</span><span class="sxs-lookup"><span data-stu-id="e866f-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="e866f-125">`-Tables` (PMC) a `--table` (CLI) umožňuje zahrnout konkrétní tabulky.</span><span class="sxs-lookup"><span data-stu-id="e866f-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="e866f-126">Zahrnout více tabulek v konzole PMC, použijte pole.</span><span class="sxs-lookup"><span data-stu-id="e866f-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="e866f-127">Chcete-li zahrnout více tabulek v rozhraní příkazového řádku, zadejte možnost více než jednou.</span><span class="sxs-lookup"><span data-stu-id="e866f-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="e866f-128">Zachování názvy</span><span class="sxs-lookup"><span data-stu-id="e866f-128">Preserving names</span></span>

<span data-ttu-id="e866f-129">Názvy tabulek a sloupců tak, aby lépe odpovídaly zásady vytváření názvů .NET pro typy a vlastnosti jsou oprava ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e866f-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="e866f-130">Zadání `-UseDatabaseNames` přepínače v konzole PMC nebo `--use-database-names` možnost v rozhraní příkazového řádku se zakázat toto chování zachovat původní názvy databází co největší míře.</span><span class="sxs-lookup"><span data-stu-id="e866f-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="e866f-131">Neplatné identifikátory rozhraní .NET stále opravíme a syntetizovaný názvy jako vlastnosti navigace se stále odpovídat zásady vytváření názvů .NET.</span><span class="sxs-lookup"><span data-stu-id="e866f-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="e866f-132">Rozhraní Fluent API nebo datové poznámky</span><span class="sxs-lookup"><span data-stu-id="e866f-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="e866f-133">Typy entit jsou nakonfigurované pomocí rozhraní Fluent API ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e866f-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="e866f-134">Zadejte `-DataAnnotations` (PMC) nebo `--data-annotations` (CLI) pro náhradní použití anotací dat, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="e866f-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="e866f-135">Například pomocí rozhraní Fluent API bude generování uživatelského rozhraní to.</span><span class="sxs-lookup"><span data-stu-id="e866f-135">For example, using the Fluent API will scaffold the this.</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="e866f-136">Při používání datových poznámek bude generování uživatelského rozhraní to.</span><span class="sxs-lookup"><span data-stu-id="e866f-136">While using Data Annotations will scaffold this.</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="e866f-137">Název DbContext</span><span class="sxs-lookup"><span data-stu-id="e866f-137">DbContext name</span></span>

<span data-ttu-id="e866f-138">Název vygenerované třídy DbContext bude název databáze doplněny *kontextu* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e866f-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="e866f-139">Pokud chcete zadat jinou, použijte `-Context` v konzole PMC a `--context` v rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e866f-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="e866f-140">Adresáře a obory názvů</span><span class="sxs-lookup"><span data-stu-id="e866f-140">Directories and namespaces</span></span>

<span data-ttu-id="e866f-141">Entity třídy a třídy DbContext jsou automaticky do kořenového adresáře projektu a použijte výchozí obor názvů projektu.</span><span class="sxs-lookup"><span data-stu-id="e866f-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="e866f-142">Lze určit adresář, kde tříd jsou automaticky pomocí `-OutputDir` (PMC) nebo `--output-dir` (rozhraní příkazového řádku).</span><span class="sxs-lookup"><span data-stu-id="e866f-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="e866f-143">Obor názvů bude kořenového oboru názvů a názvů jakéhokoliv podadresáře v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="e866f-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="e866f-144">Můžete také použít `-ContextDir` (PMC) a `--context-dir` (CLI) do samostatných adresáře z tříd entit typu scaffold třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="e866f-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="e866f-145">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="e866f-145">How it works</span></span>

<span data-ttu-id="e866f-146">Zpětná analýza začne schématu databáze pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e866f-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="e866f-147">Načte informace o tabulky, sloupce, omezení a indexy.</span><span class="sxs-lookup"><span data-stu-id="e866f-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="e866f-148">V dalším kroku použije informace o schématu pro vytvoření modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="e866f-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="e866f-149">Tabulky se používají k vytvoření typů entit; sloupce se použijí k vytvoření vlastnosti; a cizí klíče slouží k vytvoření relací.</span><span class="sxs-lookup"><span data-stu-id="e866f-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="e866f-150">Nakonec model se používá ke generování kódu.</span><span class="sxs-lookup"><span data-stu-id="e866f-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="e866f-151">Chcete-li znovu vytvořit stejný model z vaší aplikace jsou automaticky generovaný odpovídající entity typu třídy, rozhraní Fluent API a data poznámky.</span><span class="sxs-lookup"><span data-stu-id="e866f-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="e866f-152">Co nefunguje</span><span class="sxs-lookup"><span data-stu-id="e866f-152">What doesn't work</span></span>

<span data-ttu-id="e866f-153">Ne vše, co o modelu lze znázornit pomocí schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e866f-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="e866f-154">Například informace o **hierarchie dědičnosti**, **vlastní typy**, a **tabulky rozdělení** nejsou k dispozici ve schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="e866f-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="e866f-155">Z toho důvodu tyto konstrukce nikdy se vrátíte zpět inženýrství.</span><span class="sxs-lookup"><span data-stu-id="e866f-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="e866f-156">Kromě toho **některé typy sloupců** nemusí být podporována zprostředkovatelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="e866f-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="e866f-157">Tyto sloupce nebudou zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="e866f-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="e866f-158">EF Core vyžaduje, aby každý typ entity klíč.</span><span class="sxs-lookup"><span data-stu-id="e866f-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="e866f-159">Tabulky, není však nutné nastavit primární klíč.</span><span class="sxs-lookup"><span data-stu-id="e866f-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="e866f-160">**Tabulky s primárním klíčem** jsou aktuálně není zpětnou analýzou.</span><span class="sxs-lookup"><span data-stu-id="e866f-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="e866f-161">Můžete definovat **tokeny souběžnosti** v modelu EF Core dvě uživatelům zabránit v aktualizaci stejné entity ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="e866f-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="e866f-162">Některé databáze mají speciální typ pro reprezentaci tohoto typu sloupce (například rowversion v systému SQL Server) v takovém případě lze zrušit jsme pracovníkovi tyto informace; však další tokeny souběžnosti nesmí být zpětná analýza.</span><span class="sxs-lookup"><span data-stu-id="e866f-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="e866f-163">Přizpůsobení modelu</span><span class="sxs-lookup"><span data-stu-id="e866f-163">Customizing the model</span></span>

<span data-ttu-id="e866f-164">Kód vygenerovaný EF Core je váš kód.</span><span class="sxs-lookup"><span data-stu-id="e866f-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="e866f-165">Můžete ho změnit.</span><span class="sxs-lookup"><span data-stu-id="e866f-165">Feel free to change it.</span></span> <span data-ttu-id="e866f-166">To se znovu vygeneruje jenom Pokud znovu provést zpětnou analýzu stejného modelu.</span><span class="sxs-lookup"><span data-stu-id="e866f-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="e866f-167">Automaticky generovaný kód představuje *jeden* model, který můžete použít pro přístup k databázi, ale určitě není *pouze* model, který lze použít.</span><span class="sxs-lookup"><span data-stu-id="e866f-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="e866f-168">Upravte entity typu třídy a třídy DbContext podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="e866f-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="e866f-169">Můžete například přejmenovat typy a vlastnosti, zavést hierarchie dědičnosti nebo rozdělení tabulky do více entit.</span><span class="sxs-lookup"><span data-stu-id="e866f-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="e866f-170">Jedinečné indexy, nevyužité pořadí a navigačních vlastností, volitelné Skalární vlastnosti a omezení názvů můžete také odebrat z modelu.</span><span class="sxs-lookup"><span data-stu-id="e866f-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="e866f-171">Můžete také přidat další konstruktorů, metod, vlastností, atd.</span><span class="sxs-lookup"><span data-stu-id="e866f-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="e866f-172">pomocí jiné částečné třídy v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="e866f-172">using another partial class in a separate file.</span></span> <span data-ttu-id="e866f-173">Tento postup funguje i v případě, že máte v úmyslu znovu provést zpětnou analýzu modelu.</span><span class="sxs-lookup"><span data-stu-id="e866f-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="e866f-174">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="e866f-174">Updating the model</span></span>

<span data-ttu-id="e866f-175">Po provedení změn v databázi, budete muset aktualizovat tak, aby odrážela tyto změny modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="e866f-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="e866f-176">Pokud jsou jednoduché změny databáze, může být nejjednodušší jenom ručně provést změny modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="e866f-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="e866f-177">Třeba přejmenování tabulky nebo sloupce, odebráním sloupce nebo aktualizace sloupce typu jsou jednoduché změny v kódu.</span><span class="sxs-lookup"><span data-stu-id="e866f-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="e866f-178">Další významné změny, ale nejsou jako snadno vytvořit ručně.</span><span class="sxs-lookup"><span data-stu-id="e866f-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="e866f-179">Jednou z běžných pracovních postupů je provést zpětnou analýzu model z databáze, znovu pomocí `-Force` (PMC) nebo `--force` (CLI) k přepsání existujícího modelu aktualizované sadou.</span><span class="sxs-lookup"><span data-stu-id="e866f-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="e866f-180">Další běžně požadovaných funkcí je schopnost aktualizace modelu z databáze při zachování vlastního nastavení, jako je přejmenování, hierarchie typů, atd. Použít problém [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) sledovat průběh této funkce.</span><span class="sxs-lookup"><span data-stu-id="e866f-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="e866f-181">Pokud zpětné analýze modelu z databáze znovu, budou ztraceny všechny změny, které jste provedli v souborech.</span><span class="sxs-lookup"><span data-stu-id="e866f-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
