---
title: Zpětná analýza – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 775a929982b9f4fb10aad9cd43bbb555ce632ad1
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149020"
---
# <a name="reverse-engineering"></a><span data-ttu-id="ff74d-102">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="ff74d-102">Reverse Engineering</span></span>

<span data-ttu-id="ff74d-103">Zpětná analýza je proces třídy typu entity vytváření uživatelského rozhraní a třída DbContext založená na schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="ff74d-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="ff74d-104">Dá se provést pomocí `Scaffold-DbContext` EF Core příkazu nástroje PMC (Správce balíčků správce) `dotnet ef dbcontext scaffold` nebo příkazu rozhraní příkazového řádku (CLI) rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="ff74d-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="ff74d-105">Instalují</span><span class="sxs-lookup"><span data-stu-id="ff74d-105">Installing</span></span>

<span data-ttu-id="ff74d-106">Před zpětnou metodologií budete muset nainstalovat buď [nástroje PMC](xref:core/miscellaneous/cli/powershell) (pouze Visual Studio), nebo nástroje rozhraní příkazového [řádku](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="ff74d-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="ff74d-107">Podrobnosti najdete v tématu odkazy.</span><span class="sxs-lookup"><span data-stu-id="ff74d-107">See links for details.</span></span>

<span data-ttu-id="ff74d-108">Také budete muset nainstalovat vhodného [poskytovatele databáze](xref:core/providers/index) pro schéma databáze, u kterého chcete provést zpětnou analýzu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="ff74d-109">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="ff74d-109">Connection string</span></span>

<span data-ttu-id="ff74d-110">První argument příkazu je připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="ff74d-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="ff74d-111">Nástroje použijí tento připojovací řetězec ke čtení schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="ff74d-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="ff74d-112">Způsob uvozovek a řídicího řetězce závisí na tom, jaké prostředí používáte ke spuštění příkazu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="ff74d-113">Konkrétní informace najdete v dokumentaci ke svému prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff74d-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="ff74d-114">Například prostředí PowerShell vyžaduje, abyste `$` znak vyhnuli, ale ne. `\`</span><span class="sxs-lookup"><span data-stu-id="ff74d-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="ff74d-115">Konfigurace a tajné klíče uživatele</span><span class="sxs-lookup"><span data-stu-id="ff74d-115">Configuration and User Secrets</span></span>

<span data-ttu-id="ff74d-116">Pokud máte projekt ASP.NET Core, můžete použít `Name=<connection-string>` syntaxi ke čtení připojovacího řetězce z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ff74d-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="ff74d-117">Tato funkce dobře funguje s [nástrojem Správce tajných klíčů](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) , aby vaše heslo databáze bylo oddělené od základu kódu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="ff74d-118">Název zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="ff74d-118">Provider name</span></span>

<span data-ttu-id="ff74d-119">Druhým argumentem je název poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="ff74d-119">The second argument is the provider name.</span></span> <span data-ttu-id="ff74d-120">Název zprostředkovatele je obvykle stejný jako název balíčku NuGet poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="ff74d-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="ff74d-121">Určení tabulek</span><span class="sxs-lookup"><span data-stu-id="ff74d-121">Specifying tables</span></span>

<span data-ttu-id="ff74d-122">Všechny tabulky ve schématu databáze jsou ve výchozím nastavení zpětně analyzovány do typů entit.</span><span class="sxs-lookup"><span data-stu-id="ff74d-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="ff74d-123">Můžete omezit, které tabulky budou zpětně analyzovány zadáním schémat a tabulek.</span><span class="sxs-lookup"><span data-stu-id="ff74d-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="ff74d-124">Parametr v PMC `--schema` a možnost v rozhraní příkazového řádku lze použít k zahrnutí všech tabulek v rámci schématu. `-Schemas`</span><span class="sxs-lookup"><span data-stu-id="ff74d-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="ff74d-125">`-Tables`(PMC) a `--table` (CLI) lze použít k zahrnutí specifických tabulek.</span><span class="sxs-lookup"><span data-stu-id="ff74d-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="ff74d-126">Chcete-li do PMC zahrnout více tabulek, použijte pole.</span><span class="sxs-lookup"><span data-stu-id="ff74d-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="ff74d-127">Chcete-li v rozhraní příkazového řádku zahrnout více tabulek, určete možnost několikrát.</span><span class="sxs-lookup"><span data-stu-id="ff74d-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="ff74d-128">Zachování názvů</span><span class="sxs-lookup"><span data-stu-id="ff74d-128">Preserving names</span></span>

<span data-ttu-id="ff74d-129">Názvy tabulek a sloupců jsou pevně nastavené tak, aby lépe odpovídaly konvencím názvů .NET pro typy a vlastnosti ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ff74d-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="ff74d-130">Když zadáte `--use-database-names` přepínač v PMC nebo v rozhraní příkazového řádku, zakážete tím toto chování s původními názvy databází co nejvíce. `-UseDatabaseNames`</span><span class="sxs-lookup"><span data-stu-id="ff74d-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="ff74d-131">Neplatné identifikátory .NET budou pořád opravené a syntetizované názvy, jako jsou vlastnosti navigace, budou pořád odpovídat konvencím vytváření názvů .NET.</span><span class="sxs-lookup"><span data-stu-id="ff74d-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="ff74d-132">Rozhraní Fluent API nebo datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ff74d-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="ff74d-133">Typy entit se ve výchozím nastavení konfigurují pomocí rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ff74d-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="ff74d-134">Pokud `-DataAnnotations` je to možné, `--data-annotations` zadejte (PMC) nebo (CLI), aby se místo toho používaly datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="ff74d-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="ff74d-135">Například při použití rozhraní Fluent API dojde k následujícímu generování uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="ff74d-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="ff74d-136">Když použijete datové poznámky, vytvoří se toto uživatelské rozhraní:</span><span class="sxs-lookup"><span data-stu-id="ff74d-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="ff74d-137">Název DbContext</span><span class="sxs-lookup"><span data-stu-id="ff74d-137">DbContext name</span></span>

<span data-ttu-id="ff74d-138">Název třídy DbContext ve vygenerovaném *obsahu* bude ve výchozím nastavení názvem databáze s příponou.</span><span class="sxs-lookup"><span data-stu-id="ff74d-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="ff74d-139">Chcete-li zadat jiný než jeden `-Context` , použijte v `--context` PMC a v rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ff74d-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="ff74d-140">Adresáře a obory názvů</span><span class="sxs-lookup"><span data-stu-id="ff74d-140">Directories and namespaces</span></span>

<span data-ttu-id="ff74d-141">Třídy entit a třída DbContext jsou vygenerované do kořenového adresáře projektu a používají výchozí obor názvů projektu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="ff74d-142">Můžete zadat adresář, ve kterém jsou třídy vygenerované pomocí `-OutputDir` (PMC) nebo `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="ff74d-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="ff74d-143">Obor názvů bude kořenový obor názvů a názvy všech podadresářů v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="ff74d-144">Můžete také použít `-ContextDir` (PMC) a `--context-dir` (CLI) pro generování uživatelského rozhraní třídy DbContext do samostatného adresáře z tříd typu entity.</span><span class="sxs-lookup"><span data-stu-id="ff74d-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="ff74d-145">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="ff74d-145">How it works</span></span>

<span data-ttu-id="ff74d-146">Zpětná analýza začíná čtením schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="ff74d-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="ff74d-147">Čte informace o tabulkách, sloupcích, omezeních a indexech.</span><span class="sxs-lookup"><span data-stu-id="ff74d-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="ff74d-148">V dalším kroku se pomocí informací o schématu vytvoří model EF Core.</span><span class="sxs-lookup"><span data-stu-id="ff74d-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="ff74d-149">Tabulky slouží k vytváření typů entit. sloupce slouží k vytváření vlastností; a k vytváření relací se používají cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="ff74d-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="ff74d-150">Nakonec se model používá ke generování kódu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="ff74d-151">Odpovídající třídy typů entit, rozhraní Fluent API a datové poznámky jsou vygenerované z důvodu opětovného vytvoření stejného modelu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff74d-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="ff74d-152">Co nefunguje</span><span class="sxs-lookup"><span data-stu-id="ff74d-152">What doesn't work</span></span>

<span data-ttu-id="ff74d-153">Ne vše o modelu lze znázornit pomocí schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="ff74d-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="ff74d-154">Například informace o [**hierarchiích dědičnosti**](../modeling/inheritance.md), [**vlastněných typech**](../modeling/owned-entities.md)a [**rozdělení tabulky**](../modeling/table-splitting.md) nejsou k dispozici ve schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="ff74d-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="ff74d-155">Z tohoto důvodu tyto konstrukce nebudou nikdy zpětně analyzovány.</span><span class="sxs-lookup"><span data-stu-id="ff74d-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="ff74d-156">Kromě toho poskytovatel EF Core nemusí podporovat **některé typy sloupců** .</span><span class="sxs-lookup"><span data-stu-id="ff74d-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="ff74d-157">Tyto sloupce nebudou zahrnuty do modelu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="ff74d-158">V EF Coreovém modelu můžete definovat [**tokeny souběžnosti**](../modeling/concurrency.md), aby se uživatelé nemohli současně aktualizovat stejnou entitu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="ff74d-159">Některé databáze mají speciální typ, který představuje tento typ sloupce (například rowversion v SQL Server). v takovém případě můžeme tyto informace zpětně analyzovat; jiné tokeny souběžnosti však nebudou zpětně analyzovány.</span><span class="sxs-lookup"><span data-stu-id="ff74d-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="ff74d-160">Přizpůsobení modelu</span><span class="sxs-lookup"><span data-stu-id="ff74d-160">Customizing the model</span></span>

<span data-ttu-id="ff74d-161">Kód vygenerovaný EF Core je váš kód.</span><span class="sxs-lookup"><span data-stu-id="ff74d-161">The code generated by EF Core is your code.</span></span> <span data-ttu-id="ff74d-162">Nebojte se změnit.</span><span class="sxs-lookup"><span data-stu-id="ff74d-162">Feel free to change it.</span></span> <span data-ttu-id="ff74d-163">Bude znovu vygenerována pouze v případě, že znovu budete provádět zpětnou analýzu stejného modelu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-163">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="ff74d-164">Generovaný kód reprezentuje *jeden* model, který se dá použít pro přístup k databázi, ale není to *jediný* model, který se dá použít.</span><span class="sxs-lookup"><span data-stu-id="ff74d-164">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="ff74d-165">Přizpůsobte třídy typu entity a třídu DbContext tak, aby vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="ff74d-165">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="ff74d-166">Například se můžete rozhodnout přejmenovat typy a vlastnosti, zavést Hierarchie dědičnosti nebo rozdělit tabulku do více entit.</span><span class="sxs-lookup"><span data-stu-id="ff74d-166">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="ff74d-167">Z modelu můžete také odebrat nejedinečné indexy, nepoužívané sekvence a navigační vlastnosti, volitelné skalární vlastnosti a názvy omezení.</span><span class="sxs-lookup"><span data-stu-id="ff74d-167">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="ff74d-168">Můžete také přidat další konstruktory, metody, vlastnosti atd.</span><span class="sxs-lookup"><span data-stu-id="ff74d-168">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="ff74d-169">použití jiné částečné třídy v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="ff74d-169">using another partial class in a separate file.</span></span> <span data-ttu-id="ff74d-170">Tento přístup funguje i v případě, že máte v úmyslu provést zpětnou analýzu modelu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-170">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="ff74d-171">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="ff74d-171">Updating the model</span></span>

<span data-ttu-id="ff74d-172">Po provedení změn v databázi bude pravděpodobně nutné aktualizovat model EF Core, aby odrážel tyto změny.</span><span class="sxs-lookup"><span data-stu-id="ff74d-172">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="ff74d-173">Pokud se databáze změní na jednoduchá, může být nejjednodušší pouze ručně provést změny v modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="ff74d-173">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="ff74d-174">Například přejmenování tabulky nebo sloupce, odebrání sloupce nebo aktualizace typu sloupce jsou triviální změny v kódu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-174">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="ff74d-175">Důležitější změny se ale nedají snadno provést ručně.</span><span class="sxs-lookup"><span data-stu-id="ff74d-175">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="ff74d-176">Jedním z běžných pracovních postupů je provést zpětnou analýzu modelu z databáze znovu `-Force` pomocí (PMC) `--force` nebo (CLI) a přepsat existující model pomocí aktualizovaného typu.</span><span class="sxs-lookup"><span data-stu-id="ff74d-176">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="ff74d-177">Další běžně vyžadovaná funkce je možnost aktualizovat model z databáze a přitom zachovat přizpůsobení jako přejmenování, hierarchií typů atd. Průběh této funkce můžete sledovat pomocí [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) problému.</span><span class="sxs-lookup"><span data-stu-id="ff74d-177">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="ff74d-178">Pokud znovu provedete zpětnou analýzu modelu z databáze, všechny změny, které jste provedli v souborech, budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="ff74d-178">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
