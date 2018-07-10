---
title: Připojovací řetězce a modelů – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: afb13998a6482b3e7a7e250892854ab7599f109d
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912067"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="1ea2f-102">Připojovací řetězce a modelů</span><span class="sxs-lookup"><span data-stu-id="1ea2f-102">Connection strings and models</span></span>
<span data-ttu-id="1ea2f-103">Toto téma popisuje, jak Entity Framework zjistí připojení k databázi, a jak ho změnit.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="1ea2f-104">Oba modely vytvořené pomocí Code First a EF designeru jsou popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="1ea2f-105">Obvykle aplikace Entity Framework používá třídy odvozené od položky DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="1ea2f-106">Tato odvozená třída bude volat jeden z konstruktorů na základní třídy DbContext do ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="1ea2f-107">Jak kontextu se budou připojovat k database—i.e. jak připojovací řetězec se nenašel nebo používané</span><span class="sxs-lookup"><span data-stu-id="1ea2f-107">How the context will connect to a database—i.e. how a connection string is found/used</span></span>  
- <span data-ttu-id="1ea2f-108">Zda kontextu použije vypočítat model s použitím Code First nebo načtení modelu vytvářené pomocí návrháře EF</span><span class="sxs-lookup"><span data-stu-id="1ea2f-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="1ea2f-109">Další pokročilé možnosti</span><span class="sxs-lookup"><span data-stu-id="1ea2f-109">Additional advanced options</span></span>  

<span data-ttu-id="1ea2f-110">Následující fragmenty ukazují některé ze způsobů, jak konstruktory kontext databáze. je možné použít.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="1ea2f-111">Použití Code First s připojením podle konvence</span><span class="sxs-lookup"><span data-stu-id="1ea2f-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="1ea2f-112">Pokud jste neudělali žádné další konfiguraci ve vaší aplikaci, následným voláním konstruktoru bez parametrů na DbContext způsobí DbContext spouštění v režimu Code First pomocí připojení k databázi vytvoření konvencí.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="1ea2f-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="1ea2f-114">V tomto příkladu DbContext používá obor názvů kvalifikovaný název vaší odvozené kontextu class—Demo.EF.BloggingContext—as název databáze a vytváří připojovací řetězec pro tuto databázi pomocí SQL Express nebo LocalDB.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="1ea2f-115">Pokud jsou oba nainstalovány, SQL Express se použije.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="1ea2f-116">Visual Studio 2010 ve výchozím nastavení a Visual Studio 2012 obsahuje SQL Express a dále zahrnuje LocalDB.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="1ea2f-117">Během instalace balíčku EntityFramework NuGet zkontroluje, které databázový server je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="1ea2f-118">Balíček NuGet pak aktualizuje konfigurační soubor tak, že nastavíte výchozí databázový server, který Code First používá při vytváření připojení podle konvence.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="1ea2f-119">Pokud je spuštěn systém SQL Express, bude použit.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="1ea2f-120">Pokud není k dispozici SQL Express LocalDB se bude zapsán jako výchozí místo.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="1ea2f-121">Do konfiguračního souboru jsou provedeny žádné změny, pokud již obsahuje nastavení pro výchozí objekt factory připojení.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="1ea2f-122">Použití Code First s připojením podle konvence a zadaný název databáze</span><span class="sxs-lookup"><span data-stu-id="1ea2f-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="1ea2f-123">Pokud jste neudělali žádné další konfiguraci ve vaší aplikaci, pak volání konstruktoru řetězec na kontext databáze s názvem databáze, kterou chcete použít způsobí DbContext spouštění v režimu Code First pomocí připojení k databázi vytvořené pomocí konvence na databázi Tento název.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="1ea2f-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="1ea2f-125">V tomto příkladu DbContext používá "BloggingDatabase" jako název databáze a vytváří připojovací řetězec pro tuto databázi pomocí SQL Express (nainstalovaný sadou Visual Studio 2010) nebo LocalDB (nainstalované s Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="1ea2f-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="1ea2f-126">Pokud jsou oba nainstalovány, SQL Express se použije.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="1ea2f-127">Použít s připojovacím řetězcem v souboru app.config/web.config Code First</span><span class="sxs-lookup"><span data-stu-id="1ea2f-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="1ea2f-128">Můžete vložit připojovací řetězec v souboru app.config nebo web.config.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="1ea2f-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="1ea2f-130">Toto je snadný způsob, jak zjistit kontext databáze. Chcete-li použít databázový server, než systém SQL Express nebo LocalDB – výše uvedený příklad určuje databázi systému SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="1ea2f-131">Pokud název připojovacího řetězce odpovídá názvu váš kontext (s nebo bez kvalifikace názvů) pak ji bude nalezen podle DbContext při použití konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="1ea2f-132">Pokud název připojovacího řetězce se liší od názvu kontextu poznáte kontext databáze. Chcete použít toto připojení v režimu Code First předáním název připojovacího řetězce do konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="1ea2f-133">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="1ea2f-134">Alternativně můžete použít formuláře "name =\<název připojovacího řetězce\>" pro řetězec předaný konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="1ea2f-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="1ea2f-136">Tento formulář umožňuje explicitní očekáváte, že připojovací řetězec k nalezena v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="1ea2f-137">Bude vyvolána výjimka, pokud nebyl nalezen připojovací řetězec s daným názvem.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="1ea2f-138">Databáze a Model první připojovacím řetězcem v souboru app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="1ea2f-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="1ea2f-139">Modely vytvořené pomocí EF designeru se liší od Code First, že váš model již existuje a není generován z kódu při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="1ea2f-140">Model obvykle existuje jako soubor EDMX ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="1ea2f-141">Návrhář přidá připojovacího řetězce služby EF do souboru app.config nebo web.config.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="1ea2f-142">Tento připojovací řetězec je speciální, protože obsahuje informace o tom, jak najít informace v souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="1ea2f-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="1ea2f-144">EF designeru také vygeneruje kód, který dává pokyn kontext databáze. Chcete použít toto připojení tím, že předáte název připojovacího řetězce do konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="1ea2f-145">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="1ea2f-146">DbContext věděl, může se načíst existující model (nikoli výpočet z kódu pomocí Code First) vzhledem k tomu, že připojovací řetězec je řetězec připojení EF obsahující podrobnosti o použití modelu.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="1ea2f-147">Další možnosti konstruktor DbContext</span><span class="sxs-lookup"><span data-stu-id="1ea2f-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="1ea2f-148">Třídy DbContext obsahuje další konstruktory a vzorce používání, které umožňují některé pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="1ea2f-149">Zde jsou některé z těchto:</span><span class="sxs-lookup"><span data-stu-id="1ea2f-149">Some of these are:</span></span>  

- <span data-ttu-id="1ea2f-150">Třída DbModelBuilder můžete použít k sestavení modelu Code First bez vytvoření instance instanci DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="1ea2f-151">Výsledek tohoto objektu je objekt DbModel.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="1ea2f-152">Můžete předat tento objekt DbModel do jednoho z konstruktorů kontext databáze. když budete chtít vytvořit instanci DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="1ea2f-153">Úplný připojovací řetězec můžete předat DbContext namísto pouze název řetězce databázi nebo připojení.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="1ea2f-154">Ve výchozím nastavení tento připojovací řetězec se používá zprostředkovatele System.Data.SqlClient; To můžete změnit tak, že nastavíte na různé implementace IConnectionFactory do kontextu. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="1ea2f-155">Můžete použít existující objekt DbConnection předáním konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="1ea2f-156">Pokud je objekt připojení instance EntityConnection, bude model zadané v připojení používané spíše než výpočtu pomocí modelu kódu nejprve.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="1ea2f-157">Pokud je objekt instancí jiný typ – například SqlConnection – pak kontextu bude používat pro režim Code First.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="1ea2f-158">Existující objekt ObjectContext může předat DbContext konstruktor pro vytvoření DbContext zabalení existujícího kontextu.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="1ea2f-159">To lze použít pro existující aplikace, které používají objekt ObjectContext, ale které chtějí využít nabídky DbContext v některé části aplikace.</span><span class="sxs-lookup"><span data-stu-id="1ea2f-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
