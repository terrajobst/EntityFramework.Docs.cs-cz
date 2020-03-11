---
title: Zpětná analýza – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416761"
---
# <a name="reverse-engineering"></a><span data-ttu-id="f2bf3-102">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="f2bf3-102">Reverse Engineering</span></span>

<span data-ttu-id="f2bf3-103">Zpětná analýza je proces třídy typu entity vytváření uživatelského rozhraní a třída DbContext založená na schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="f2bf3-104">Dá se udělat pomocí příkazu `Scaffold-DbContext` nástrojů EF Core konzoly Správce balíčků (PMC) nebo příkazu `dotnet ef dbcontext scaffold` rozhraní příkazového řádku (CLI) rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="f2bf3-105">Instalace</span><span class="sxs-lookup"><span data-stu-id="f2bf3-105">Installing</span></span>

<span data-ttu-id="f2bf3-106">Před zpětnou metodologií budete muset nainstalovat buď [nástroje PMC](xref:core/miscellaneous/cli/powershell) (pouze Visual Studio), nebo nástroje rozhraní příkazového [řádku](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="f2bf3-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="f2bf3-107">Podrobnosti najdete v tématu odkazy.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-107">See links for details.</span></span>

<span data-ttu-id="f2bf3-108">Také budete muset nainstalovat vhodného [poskytovatele databáze](xref:core/providers/index) pro schéma databáze, u kterého chcete provést zpětnou analýzu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="f2bf3-109">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="f2bf3-109">Connection string</span></span>

<span data-ttu-id="f2bf3-110">První argument příkazu je připojovací řetězec k databázi.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="f2bf3-111">Nástroje použijí tento připojovací řetězec ke čtení schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="f2bf3-112">Způsob uvozovek a řídicího řetězce závisí na tom, jaké prostředí používáte ke spuštění příkazu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="f2bf3-113">Konkrétní informace najdete v dokumentaci ke svému prostředí.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="f2bf3-114">Například prostředí PowerShell vyžaduje, abyste `$` znak escape, ale ne `\`.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="f2bf3-115">Konfigurace a tajné klíče uživatele</span><span class="sxs-lookup"><span data-stu-id="f2bf3-115">Configuration and User Secrets</span></span>

<span data-ttu-id="f2bf3-116">Pokud máte projekt ASP.NET Core, můžete použít syntaxi `Name=<connection-string>` ke čtení připojovacího řetězce z konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="f2bf3-117">Tato funkce dobře funguje s [nástrojem Správce tajných klíčů](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) , aby vaše heslo databáze bylo oddělené od základu kódu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="f2bf3-118">Název zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="f2bf3-118">Provider name</span></span>

<span data-ttu-id="f2bf3-119">Druhým argumentem je název poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-119">The second argument is the provider name.</span></span> <span data-ttu-id="f2bf3-120">Název zprostředkovatele je obvykle stejný jako název balíčku NuGet poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="f2bf3-121">Určení tabulek</span><span class="sxs-lookup"><span data-stu-id="f2bf3-121">Specifying tables</span></span>

<span data-ttu-id="f2bf3-122">Všechny tabulky ve schématu databáze jsou ve výchozím nastavení zpětně analyzovány do typů entit.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="f2bf3-123">Můžete omezit, které tabulky budou zpětně analyzovány zadáním schémat a tabulek.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="f2bf3-124">Parametr `-Schemas` v PMC a možnost `--schema` v rozhraní příkazového řádku lze použít k zahrnutí všech tabulek v rámci schématu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="f2bf3-125">k zahrnutí specifických tabulek lze použít `-Tables` (PMC) a `--table` (CLI).</span><span class="sxs-lookup"><span data-stu-id="f2bf3-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="f2bf3-126">Chcete-li do PMC zahrnout více tabulek, použijte pole.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="f2bf3-127">Chcete-li v rozhraní příkazového řádku zahrnout více tabulek, určete možnost několikrát.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="f2bf3-128">Zachování názvů</span><span class="sxs-lookup"><span data-stu-id="f2bf3-128">Preserving names</span></span>

<span data-ttu-id="f2bf3-129">Názvy tabulek a sloupců jsou pevně nastavené tak, aby lépe odpovídaly konvencím názvů .NET pro typy a vlastnosti ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="f2bf3-130">Zadáním přepínače `-UseDatabaseNames` v PMC nebo možnosti `--use-database-names` v rozhraní příkazového řádku se toto chování předá tím, že původní názvy databází zachováte co nejvíce.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="f2bf3-131">Neplatné identifikátory .NET budou pořád opravené a syntetizované názvy, jako jsou vlastnosti navigace, budou pořád odpovídat konvencím vytváření názvů .NET.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="f2bf3-132">Rozhraní Fluent API nebo datové poznámky</span><span class="sxs-lookup"><span data-stu-id="f2bf3-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="f2bf3-133">Typy entit se ve výchozím nastavení konfigurují pomocí rozhraní API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="f2bf3-134">Zadejte `-DataAnnotations` (PMC) nebo `--data-annotations` (CLI), abyste místo toho mohli používat datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="f2bf3-135">Například při použití rozhraní Fluent API dojde k následujícímu generování uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="f2bf3-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="f2bf3-136">Když použijete datové poznámky, vytvoří se toto uživatelské rozhraní:</span><span class="sxs-lookup"><span data-stu-id="f2bf3-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="f2bf3-137">Název DbContext</span><span class="sxs-lookup"><span data-stu-id="f2bf3-137">DbContext name</span></span>

<span data-ttu-id="f2bf3-138">Název třídy DbContext ve vygenerovaném *obsahu* bude ve výchozím nastavení názvem databáze s příponou.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="f2bf3-139">Chcete-li určit jinou hodnotu, použijte `-Context` v PMC a `--context` v rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="f2bf3-140">Adresáře a obory názvů</span><span class="sxs-lookup"><span data-stu-id="f2bf3-140">Directories and namespaces</span></span>

<span data-ttu-id="f2bf3-141">Třídy entit a třída DbContext jsou vygenerované do kořenového adresáře projektu a používají výchozí obor názvů projektu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="f2bf3-142">Můžete zadat adresář, ve kterém jsou třídy vygenerované pomocí `-OutputDir` (PMC) nebo `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="f2bf3-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="f2bf3-143">Obor názvů bude kořenový obor názvů a názvy všech podadresářů v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="f2bf3-144">Můžete také použít `-ContextDir` (PMC) a `--context-dir` (CLI) k vytvoření uživatelského rozhraní třídy DbContext do samostatného adresáře z tříd typu entity.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="f2bf3-145">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="f2bf3-145">How it works</span></span>

<span data-ttu-id="f2bf3-146">Zpětná analýza začíná čtením schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="f2bf3-147">Čte informace o tabulkách, sloupcích, omezeních a indexech.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="f2bf3-148">V dalším kroku se pomocí informací o schématu vytvoří model EF Core.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="f2bf3-149">Tabulky slouží k vytváření typů entit. sloupce slouží k vytváření vlastností; a k vytváření relací se používají cizí klíče.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="f2bf3-150">Nakonec se model používá ke generování kódu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="f2bf3-151">Odpovídající třídy typů entit, rozhraní Fluent API a datové poznámky jsou vygenerované z důvodu opětovného vytvoření stejného modelu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="limitations"></a><span data-ttu-id="f2bf3-152">Omezení</span><span class="sxs-lookup"><span data-stu-id="f2bf3-152">Limitations</span></span>

* <span data-ttu-id="f2bf3-153">Ne vše o modelu lze znázornit pomocí schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="f2bf3-154">Například informace o [**hierarchiích dědičnosti**](../modeling/inheritance.md), [**vlastněných typech**](../modeling/owned-entities.md)a [**rozdělení tabulky**](../modeling/table-splitting.md) nejsou k dispozici ve schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="f2bf3-155">Z tohoto důvodu tyto konstrukce nebudou nikdy zpětně analyzovány.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-155">Because of this, these constructs will never be reverse engineered.</span></span>
* <span data-ttu-id="f2bf3-156">Kromě toho poskytovatel EF Core nemusí podporovat **některé typy sloupců** .</span><span class="sxs-lookup"><span data-stu-id="f2bf3-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="f2bf3-157">Tyto sloupce nebudou zahrnuty do modelu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-157">These columns won't be included in the model.</span></span>
* <span data-ttu-id="f2bf3-158">V EF Coreovém modelu můžete definovat [**tokeny souběžnosti**](../modeling/concurrency.md), aby se uživatelé nemohli současně aktualizovat stejnou entitu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="f2bf3-159">Některé databáze mají speciální typ, který představuje tento typ sloupce (například rowversion v SQL Server). v takovém případě můžeme tyto informace zpětně analyzovat; jiné tokeny souběžnosti však nebudou zpětně analyzovány.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>
* <span data-ttu-id="f2bf3-160">[Funkce C# typu odkazu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types) není v současné době v rekonstrukci podporována C# : EF Core vždy generuje kód, který předpokládá, že funkce je zakázána.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-160">[The C# 8 nullable reference type feature](/dotnet/csharp/tutorials/nullable-reference-types) is currently unsupported in reverse engineering: EF Core always generates C# code that assumes the feature is disabled.</span></span> <span data-ttu-id="f2bf3-161">Například textové sloupce s možnou hodnotou null budou vygenerované jako vlastnost s typem `string`, ne `string?`, s rozhraním API Fluent nebo pomocí datových poznámek, které slouží ke konfiguraci, jestli je vlastnost požadovaná nebo ne.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-161">For example, nullable text columns will be scaffolded as a property with type `string` , not `string?`, with either the Fluent API or Data Annotations used to configure whether a property is required or not.</span></span> <span data-ttu-id="f2bf3-162">Můžete upravit generovaný kód a nahradit je poznámkami, které C# mají hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-162">You can edit the scaffolded code and replace these with C# nullability annotations.</span></span> <span data-ttu-id="f2bf3-163">Podpora generování uživatelského rozhraní pro typy odkazů s možnou hodnotou null je sledována pomocí [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)problému.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-163">Scaffolding support for nullable reference types is tracked by issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="f2bf3-164">Přizpůsobení modelu</span><span class="sxs-lookup"><span data-stu-id="f2bf3-164">Customizing the model</span></span>

<span data-ttu-id="f2bf3-165">Kód vygenerovaný EF Core je váš kód.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-165">The code generated by EF Core is your code.</span></span> <span data-ttu-id="f2bf3-166">Nebojte se změnit.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-166">Feel free to change it.</span></span> <span data-ttu-id="f2bf3-167">Bude znovu vygenerována pouze v případě, že znovu budete provádět zpětnou analýzu stejného modelu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-167">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="f2bf3-168">Generovaný kód reprezentuje *jeden* model, který se dá použít pro přístup k databázi, ale není to *jediný* model, který se dá použít.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-168">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="f2bf3-169">Přizpůsobte třídy typu entity a třídu DbContext tak, aby vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-169">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="f2bf3-170">Například se můžete rozhodnout přejmenovat typy a vlastnosti, zavést Hierarchie dědičnosti nebo rozdělit tabulku do více entit.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-170">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="f2bf3-171">Z modelu můžete také odebrat nejedinečné indexy, nepoužívané sekvence a navigační vlastnosti, volitelné skalární vlastnosti a názvy omezení.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-171">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="f2bf3-172">Můžete také přidat další konstruktory, metody, vlastnosti atd.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-172">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="f2bf3-173">použití jiné částečné třídy v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-173">using another partial class in a separate file.</span></span> <span data-ttu-id="f2bf3-174">Tento přístup funguje i v případě, že máte v úmyslu provést zpětnou analýzu modelu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-174">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="f2bf3-175">Aktualizace modelu</span><span class="sxs-lookup"><span data-stu-id="f2bf3-175">Updating the model</span></span>

<span data-ttu-id="f2bf3-176">Po provedení změn v databázi bude pravděpodobně nutné aktualizovat model EF Core, aby odrážel tyto změny.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-176">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="f2bf3-177">Pokud se databáze změní na jednoduchá, může být nejjednodušší pouze ručně provést změny v modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-177">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="f2bf3-178">Například přejmenování tabulky nebo sloupce, odebrání sloupce nebo aktualizace typu sloupce jsou triviální změny v kódu.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-178">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="f2bf3-179">Důležitější změny se ale nedají snadno provést ručně.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-179">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="f2bf3-180">Jedním z běžných pracovních postupů je provést zpětnou analýzu modelu z databáze znovu pomocí `-Force` (PMC) nebo `--force` (CLI) a přepsat existující model aktualizovaným.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-180">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="f2bf3-181">Další běžně vyžadovaná funkce je možnost aktualizovat model z databáze a přitom zachovat přizpůsobení jako přejmenování, hierarchií typů atd. Průběh této funkce můžete sledovat pomocí [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) problému.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-181">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="f2bf3-182">Pokud znovu provedete zpětnou analýzu modelu z databáze, všechny změny, které jste provedli v souborech, budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="f2bf3-182">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
