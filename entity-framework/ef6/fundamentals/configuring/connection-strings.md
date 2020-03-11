---
title: Připojovací řetězce a modely – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419295"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="caabf-102">Připojovací řetězce a modely</span><span class="sxs-lookup"><span data-stu-id="caabf-102">Connection strings and models</span></span>
<span data-ttu-id="caabf-103">V tomto tématu se dozvíte, jak Entity Framework zjistit, které databázové připojení se má použít, a jak ho můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="caabf-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="caabf-104">V tomto tématu jsou uvedeny modely vytvořené pomocí Code First a Návrhář EF.</span><span class="sxs-lookup"><span data-stu-id="caabf-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="caabf-105">Obvykle aplikace Entity Framework používá třídu odvozenou z DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="caabf-106">Tato odvozená třída zavolá jeden z konstruktorů základní třídy DbContext, který bude řídit:</span><span class="sxs-lookup"><span data-stu-id="caabf-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="caabf-107">Jak se bude kontext připojovat k databázi – to znamená, jak se našel nebo používá připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="caabf-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="caabf-108">Zda bude kontext používat výpočet modelu pomocí Code First nebo načtení modelu vytvořeného pomocí návrháře EF</span><span class="sxs-lookup"><span data-stu-id="caabf-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="caabf-109">Další rozšířené možnosti</span><span class="sxs-lookup"><span data-stu-id="caabf-109">Additional advanced options</span></span>  

<span data-ttu-id="caabf-110">Následující fragmenty znázorňují některé způsoby, jak lze použít konstruktory DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="caabf-111">Použití Code First s připojením podle konvence</span><span class="sxs-lookup"><span data-stu-id="caabf-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="caabf-112">Pokud jste v aplikaci neudělali žádnou jinou konfiguraci, pak volání konstruktoru bez parametrů na DbContext způsobí, že DbContext se spustí v režimu Code First s připojením k databázi vytvořeným pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="caabf-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="caabf-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-113">For example:</span></span>  

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

<span data-ttu-id="caabf-114">V tomto příkladu DbContext používá kvalifikovaný název oboru názvů vaší odvozené třídy kontextu – demo. EF. BloggingContext – jako název databáze a vytváří připojovací řetězec pro tuto databázi pomocí SQL Express nebo LocalDB.</span><span class="sxs-lookup"><span data-stu-id="caabf-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="caabf-115">Pokud jsou obě nainstalovány, bude použit SQL Express.</span><span class="sxs-lookup"><span data-stu-id="caabf-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="caabf-116">Visual Studio 2010 obsahuje SQL Express ve výchozím nastavení a Visual Studio 2012 a novější zahrnuje LocalDB.</span><span class="sxs-lookup"><span data-stu-id="caabf-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="caabf-117">Během instalace zkontroluje balíček NuGet EntityFramework, který databázový server je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="caabf-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="caabf-118">Balíček NuGet pak aktualizuje konfigurační soubor nastavením výchozího databázového serveru, který Code First používá při vytváření připojení podle konvence.</span><span class="sxs-lookup"><span data-stu-id="caabf-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="caabf-119">Pokud je spuštěn SQL Express, bude použit.</span><span class="sxs-lookup"><span data-stu-id="caabf-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="caabf-120">Pokud SQL Express není k dispozici, LocalDB se místo toho zaregistruje jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="caabf-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="caabf-121">V konfiguračním souboru nejsou provedeny žádné změny, pokud již obsahuje nastavení pro výchozí objekt pro vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="caabf-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="caabf-122">Použít Code First s připojením podle konvence a zadaného názvu databáze</span><span class="sxs-lookup"><span data-stu-id="caabf-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="caabf-123">Pokud jste v aplikaci neudělali žádnou jinou konfiguraci, pak voláním konstruktoru String v DbContext s názvem databáze, který chcete použít, způsobí, že DbContext se spustí v režimu Code First s připojením k databázi vytvořeným konvencí do databáze nástroje. Tento název.</span><span class="sxs-lookup"><span data-stu-id="caabf-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="caabf-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="caabf-125">V tomto příkladu DbContext používá jako název databáze "BloggingDatabase" a vytvoří připojovací řetězec pro tuto databázi pomocí SQL Express (nainstalovaného se sadou Visual Studio 2010) nebo LocalDB (instaluje se s Visual Studiem 2012).</span><span class="sxs-lookup"><span data-stu-id="caabf-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="caabf-126">Pokud jsou obě nainstalovány, bude použit SQL Express.</span><span class="sxs-lookup"><span data-stu-id="caabf-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="caabf-127">Použít Code First s připojovacím řetězcem v souboru App. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="caabf-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="caabf-128">Do souboru App. config nebo Web. config se můžete rozhodnout připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="caabf-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="caabf-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="caabf-130">Toto je jednoduchý způsob, jak říct, že DbContext používat databázový server jiný než SQL Express nebo LocalDB – výše uvedený příklad určuje databázi edice SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="caabf-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="caabf-131">Pokud se název připojovacího řetězce shoduje s názvem vašeho kontextu (buď s kvalifikací oboru názvů nebo bez něj), bude nalezen pomocí DbContext, pokud je použit konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="caabf-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="caabf-132">Pokud se název připojovacího řetězce liší od názvu vašeho kontextu, pak můžete DbContext použít toto připojení v režimu Code First předáním názvu připojovacího řetězce konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="caabf-133">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="caabf-134">Alternativně můžete použít formát "název =\<připojovací řetězec název\>" pro řetězec předaný konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="caabf-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="caabf-136">Tento formulář vám umožní explicitně, abyste očekávali, že se připojovací řetězec nachází v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="caabf-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="caabf-137">Pokud nebyl nalezen připojovací řetězec se zadaným názvem, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="caabf-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="caabf-138">Databáze/Model First s připojovacím řetězcem v souboru App. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="caabf-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="caabf-139">Modely vytvořené pomocí návrháře EF se liší od Code First v tom, že váš model již existuje a není generován z kódu při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="caabf-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="caabf-140">Model obvykle existuje v projektu jako soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="caabf-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="caabf-141">Návrhář přidá připojovací řetězec EF do souboru App. config nebo Web. config.</span><span class="sxs-lookup"><span data-stu-id="caabf-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="caabf-142">Tento připojovací řetězec je speciální v tom, že obsahuje informace o tom, jak najít informace v souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="caabf-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="caabf-143">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-143">For example:</span></span>  

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

<span data-ttu-id="caabf-144">Návrhář EF také vygeneruje kód, který instruuje DbContext k použití tohoto připojení předáním názvu připojovacího řetězce konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="caabf-145">Příklad:</span><span class="sxs-lookup"><span data-stu-id="caabf-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="caabf-146">DbContext ví, že načte existující model (místo použití Code First k jeho výpočtu z kódu), protože připojovací řetězec je připojovací řetězec EF, který obsahuje podrobnosti o modelu, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="caabf-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="caabf-147">Další možnosti konstruktoru DbContext</span><span class="sxs-lookup"><span data-stu-id="caabf-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="caabf-148">Třída DbContext obsahuje další konstruktory a vzory použití, které umožňují několik pokročilejších scénářů.</span><span class="sxs-lookup"><span data-stu-id="caabf-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="caabf-149">Některé z těchto akcí:</span><span class="sxs-lookup"><span data-stu-id="caabf-149">Some of these are:</span></span>  

- <span data-ttu-id="caabf-150">Třídu DbModelBuilder můžete použít k sestavení modelu Code First bez vytváření instancí instance DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="caabf-151">Výsledkem tohoto je objekt DbModel.</span><span class="sxs-lookup"><span data-stu-id="caabf-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="caabf-152">Až budete připraveni vytvořit instanci DbContext, můžete tento objekt DbModel předat jednomu z konstruktorů DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="caabf-153">Můžete předat úplný připojovací řetězec k DbContext místo pouze databáze nebo názvu připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="caabf-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="caabf-154">Ve výchozím nastavení se tento připojovací řetězec používá společně se zprostředkovatelem System. data. SqlClient; To lze změnit nastavením jiné implementace IConnectionFactory na kontext. Database. DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="caabf-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="caabf-155">Existující objekt DbConnection můžete použít tak, že ho předáte konstruktoru DbContext.</span><span class="sxs-lookup"><span data-stu-id="caabf-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="caabf-156">Pokud je objekt připojení instancí EntityConnection, použije se místo výpočtu modelu pomocí Code First model zadaný v připojení.</span><span class="sxs-lookup"><span data-stu-id="caabf-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="caabf-157">Pokud je objekt instancí nějakého jiného typu, například SqlConnection, pak ho kontext použije pro režim Code First.</span><span class="sxs-lookup"><span data-stu-id="caabf-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="caabf-158">Můžete předat existující ObjectContext konstruktoru DbContext a vytvořit DbContext pro zabalení stávajícího kontextu.</span><span class="sxs-lookup"><span data-stu-id="caabf-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="caabf-159">Tato možnost se dá použít pro existující aplikace, které používají ObjectContext, ale které chtějí využít DbContext v některých částech aplikace.</span><span class="sxs-lookup"><span data-stu-id="caabf-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
