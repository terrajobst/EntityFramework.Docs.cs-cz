---
title: Konfigurace založená na kódu - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 7c7da8992fd1b29f998e08c13d445c1d2d8cc2ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490815"
---
# <a name="code-based-configuration"></a><span data-ttu-id="4f64f-102">Konfigurace založená na kódu</span><span class="sxs-lookup"><span data-stu-id="4f64f-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="4f64f-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4f64f-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4f64f-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="4f64f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="4f64f-105">Konfigurace aplikace Entity Framework se dá nastavit v konfiguračním souboru (app.config/web.config) nebo prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="4f64f-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="4f64f-106">Ten se označuje jako konfigurace založená na kódu.</span><span class="sxs-lookup"><span data-stu-id="4f64f-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="4f64f-107">Konfigurace v konfiguračním souboru je popsána v tématu [samostatný článek](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="4f64f-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="4f64f-108">Konfigurační soubor má přednost před konfigurace založená na kódu.</span><span class="sxs-lookup"><span data-stu-id="4f64f-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="4f64f-109">Jinými slovy, pokud parametr konfigurace nastaven v obou kódu a konfiguračním souboru, pak nastavení v konfiguračním souboru se používá.</span><span class="sxs-lookup"><span data-stu-id="4f64f-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="4f64f-110">Pomocí DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="4f64f-110">Using DbConfiguration</span></span>  

<span data-ttu-id="4f64f-111">Konfigurace založená na kódu EF6 a vyšší je dosaženo vytvořením podtřída System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f64f-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="4f64f-112">Při vytvoření podtřídy DbConfiguration by měla dodržovat následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="4f64f-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="4f64f-113">Vytvořte pouze jednu třídu DbConfiguration pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f64f-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="4f64f-114">Tato třída určuje nastavení v celé doméně aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f64f-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="4f64f-115">Umístěte DbConfiguration třídu do stejného sestavení jako vaší třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="4f64f-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="4f64f-116">(Najdete v článku *přesunutí DbConfiguration* části, pokud chcete toto nastavení změnit.)</span><span class="sxs-lookup"><span data-stu-id="4f64f-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="4f64f-117">Poskytněte vaše třída DbConfiguration veřejný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="4f64f-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="4f64f-118">Nastavit možnosti konfigurace voláním chráněné metody DbConfiguration z v rámci tohoto konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="4f64f-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="4f64f-119">Dodržení těchto pokynů umožňuje EF mohli objevit a používat vaše konfigurace automaticky podle i nástroje, který potřebuje přístup k modelu a při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="4f64f-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="4f64f-120">Příklad</span><span class="sxs-lookup"><span data-stu-id="4f64f-120">Example</span></span>  

<span data-ttu-id="4f64f-121">Třídy odvozené od DbConfiguration může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="4f64f-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="4f64f-122">Tato třída EF nastaví automaticky opakovat operace databáze se nezdařilo - a použití místní databáze pro databáze, které jsou vytvořeny pomocí konvence z Code First pomocí strategie provádění SQL Azure –.</span><span class="sxs-lookup"><span data-stu-id="4f64f-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="4f64f-123">Přesunutí DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="4f64f-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="4f64f-124">Existují případy, kdy není možné umístit DbConfiguration třídu do stejného sestavení jako vaší třídy DbContext.</span><span class="sxs-lookup"><span data-stu-id="4f64f-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="4f64f-125">Například může mít dvě DbContext třídy každý v různých sestaveních.</span><span class="sxs-lookup"><span data-stu-id="4f64f-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="4f64f-126">Existují dvě možnosti pro tento.</span><span class="sxs-lookup"><span data-stu-id="4f64f-126">There are two options for handling this.</span></span>  

<span data-ttu-id="4f64f-127">První možností je použít konfigurační soubor pro zadání instance DbConfiguration používat.</span><span class="sxs-lookup"><span data-stu-id="4f64f-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="4f64f-128">Provedete to tak, nastavte atribut codeConfigurationType oddílu objektu entityFramework.</span><span class="sxs-lookup"><span data-stu-id="4f64f-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="4f64f-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4f64f-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="4f64f-130">Hodnota codeConfigurationType musí být sestavení a oboru názvů kvalifikovaný název třídy DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f64f-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="4f64f-131">Druhou možností je umístit DbConfigurationTypeAttribute na třídě kontextu.</span><span class="sxs-lookup"><span data-stu-id="4f64f-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="4f64f-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4f64f-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="4f64f-133">Hodnota předaná do atributu může být typ DbConfiguration – jak je znázorněno výše - nebo řetězec názvu typu kvalifikovaný pro sestavení a oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4f64f-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="4f64f-134">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4f64f-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="4f64f-135">Nastavení DbConfiguration explicitně</span><span class="sxs-lookup"><span data-stu-id="4f64f-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="4f64f-136">Existují určité situace, kdy může být potřeba konfigurace předtím, než se použil libovolný typ DbContext.</span><span class="sxs-lookup"><span data-stu-id="4f64f-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="4f64f-137">Příklady zahrnují:</span><span class="sxs-lookup"><span data-stu-id="4f64f-137">Examples of this include:</span></span>  

- <span data-ttu-id="4f64f-138">Použití DbModelBuilder k sestavení modelu bez kontextu</span><span class="sxs-lookup"><span data-stu-id="4f64f-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="4f64f-139">Pomocí některé jiné nástroj/framework kód, který využívá DbContext, kde se používá tento kontext předtím, než se používá kontext aplikace</span><span class="sxs-lookup"><span data-stu-id="4f64f-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="4f64f-140">V takových situacích je EF tak není možné zjistit konfiguraci automaticky a musí místo toho proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="4f64f-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="4f64f-141">Nastavit typ DbConfiguration v konfiguračním souboru, jak je popsáno v *přesunutí DbConfiguration* výše uvedené části</span><span class="sxs-lookup"><span data-stu-id="4f64f-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="4f64f-142">Zavolejte statickou metodu DbConfiguration.SetConfiguration během spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="4f64f-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="4f64f-143">Přepsání DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="4f64f-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="4f64f-144">Existují určité situace, kdy potřebujete přepsat konfiguraci nastavení v DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f64f-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="4f64f-145">Není to obvykle vývojáři aplikací, ale spíše poskytovatelů třetích stran a moduly plug-in, které nelze použít odvozenou třídu DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f64f-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="4f64f-146">V tomto případě umožňuje objektu EntityFramework obslužné rutiny události k registraci, který můžete upravit existující konfiguraci těsně před je uzamčen.</span><span class="sxs-lookup"><span data-stu-id="4f64f-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="4f64f-147">Také poskytuje metodu sugar speciálně pro nahrazení jakoukoli službu vrácený EF lokátoru služeb.</span><span class="sxs-lookup"><span data-stu-id="4f64f-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="4f64f-148">To je, jak je určena pro použití:</span><span class="sxs-lookup"><span data-stu-id="4f64f-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="4f64f-149">Při spuštění aplikace (před použitím EF) modul plug-in nebo zprostředkovatele měli zaregistrovat metodu obslužné rutiny události pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="4f64f-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="4f64f-150">(Všimněte si, že to musíte provést před vytvořením aplikace pomocí EF.)</span><span class="sxs-lookup"><span data-stu-id="4f64f-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="4f64f-151">Obslužná rutina události zavolá ReplaceService pro každou službu, která potřebuje vyměnit.</span><span class="sxs-lookup"><span data-stu-id="4f64f-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="4f64f-152">Například by repalce IDbConnectionFactory a DbProviderService zaregistrovat obslužnou rutinu podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4f64f-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="4f64f-153">V kódu nad MyProviderServices a MyConnectionFactory představují vaše implementace služby.</span><span class="sxs-lookup"><span data-stu-id="4f64f-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="4f64f-154">Můžete také přidat další závislosti obslužné rutiny pro docílíte stejného efektu.</span><span class="sxs-lookup"><span data-stu-id="4f64f-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="4f64f-155">Všimněte si, že můžete také zabalit DbProviderFactory tímto způsobem, ale udělat to ovlivní pouze EF a ne používá DbProviderFactory mimo EF.</span><span class="sxs-lookup"><span data-stu-id="4f64f-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="4f64f-156">Z tohoto důvodu budete pravděpodobně chtít zabalení DbProviderFactory máte před i nadále.</span><span class="sxs-lookup"><span data-stu-id="4f64f-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="4f64f-157">Je třeba také zachovat v paměti služby, které běží externě do vaší aplikace – například při spuštění migrace v konzole Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="4f64f-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="4f64f-158">Když spustíte migraci z konzoly, pokusí se najít vaše DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f64f-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="4f64f-159">Jestli se budou získávat zabalené služby však závisí, ve kterém obslužné rutiny událostí zaregistrované.</span><span class="sxs-lookup"><span data-stu-id="4f64f-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="4f64f-160">Pokud je registrován jako součást procesu vytváření vaší DbConfiguration by se měl spustit kód a služby by měl získat zabalena.</span><span class="sxs-lookup"><span data-stu-id="4f64f-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="4f64f-161">Obvykle to nebude tento případ a to znamená, že nástroje nedostali zabalené služby.</span><span class="sxs-lookup"><span data-stu-id="4f64f-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
