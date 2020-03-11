---
title: Konfigurace založená na kódu – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417867"
---
# <a name="code-based-configuration"></a><span data-ttu-id="aec0d-102">Konfigurace na základě kódu</span><span class="sxs-lookup"><span data-stu-id="aec0d-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="aec0d-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="aec0d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="aec0d-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="aec0d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="aec0d-105">Konfiguraci pro aplikaci Entity Framework lze zadat v konfiguračním souboru (App. config/Web. config) nebo prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="aec0d-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="aec0d-106">Druhá je známá jako Konfigurace založená na kódu.</span><span class="sxs-lookup"><span data-stu-id="aec0d-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="aec0d-107">Konfigurace v konfiguračním souboru je popsána v [samostatném článku](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="aec0d-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="aec0d-108">Konfigurační soubor má přednost před konfigurací založenou na kódu.</span><span class="sxs-lookup"><span data-stu-id="aec0d-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="aec0d-109">Jinými slovy, pokud je možnost konfigurace nastavena v kódu i v konfiguračním souboru, bude použito nastavení v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="aec0d-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="aec0d-110">Použití DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="aec0d-110">Using DbConfiguration</span></span>  

<span data-ttu-id="aec0d-111">Konfigurace na základě kódu v EF6 a výše se dosahuje vytvořením podtříd třídy System. data. entity. config. DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="aec0d-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="aec0d-112">Při DbConfigurationí podtříd by měly být dodrženy následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="aec0d-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="aec0d-113">Pro svou aplikaci vytvořte pouze jednu třídu DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="aec0d-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="aec0d-114">Tato třída určuje nastavení pro šířku domény pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aec0d-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="aec0d-115">Třídu DbConfiguration umístěte do stejného sestavení jako třídu DbContext.</span><span class="sxs-lookup"><span data-stu-id="aec0d-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="aec0d-116">(Pokud ho chcete změnit, přečtěte si část *přesunutí DbConfiguration* .)</span><span class="sxs-lookup"><span data-stu-id="aec0d-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="aec0d-117">Poskytněte třídu DbConfiguration veřejný konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="aec0d-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="aec0d-118">Nastavte možnosti konfigurace voláním chráněných metod DbConfiguration z tohoto konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="aec0d-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="aec0d-119">Podle těchto pokynů umožňuje EF zjistit a použít konfiguraci automaticky pomocí nástrojů, které potřebují přístup k vašemu modelu a při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="aec0d-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="aec0d-120">Příklad</span><span class="sxs-lookup"><span data-stu-id="aec0d-120">Example</span></span>  

<span data-ttu-id="aec0d-121">Třída odvozená z DbConfiguration může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="aec0d-121">A class derived from DbConfiguration might look like this:</span></span>  

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
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="aec0d-122">Tato třída nastaví EF, aby používala strategii spouštění SQL Azure – k automatickému opakování neúspěšných operací databáze a k použití místní databáze pro databáze, které jsou vytvořeny pomocí konvence z Code First.</span><span class="sxs-lookup"><span data-stu-id="aec0d-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="aec0d-123">Přesunutí DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="aec0d-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="aec0d-124">Existují případy, kdy není možné umístit třídu DbConfiguration do stejného sestavení jako třídu DbContext.</span><span class="sxs-lookup"><span data-stu-id="aec0d-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="aec0d-125">Například můžete mít dvě třídy DbContext v různých sestaveních.</span><span class="sxs-lookup"><span data-stu-id="aec0d-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="aec0d-126">Existují dvě možnosti, jak to zpracovat.</span><span class="sxs-lookup"><span data-stu-id="aec0d-126">There are two options for handling this.</span></span>  

<span data-ttu-id="aec0d-127">První možností je použít konfigurační soubor k určení instance DbConfiguration, která se má použít.</span><span class="sxs-lookup"><span data-stu-id="aec0d-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="aec0d-128">Pokud to chcete provést, nastavte atribut codeConfigurationType oddílu entityFramework.</span><span class="sxs-lookup"><span data-stu-id="aec0d-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="aec0d-129">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aec0d-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="aec0d-130">Hodnota codeConfigurationType musí být sestavení a kvalifikovaný název oboru názvů vaší třídy DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="aec0d-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="aec0d-131">Druhou možností je umístit DbConfigurationTypeAttribute na svou kontextovou třídu.</span><span class="sxs-lookup"><span data-stu-id="aec0d-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="aec0d-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aec0d-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="aec0d-133">Hodnota předaná atributu může být buď váš typ DbConfiguration, jak je znázorněno výše, nebo sestavení a kvalifikovaný název typu řetězce pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="aec0d-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="aec0d-134">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aec0d-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="aec0d-135">Explicitní nastavení DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="aec0d-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="aec0d-136">V některých situacích se může stát, že se konfigurace bude potřebovat předtím, než se použije libovolný DbContext typ.</span><span class="sxs-lookup"><span data-stu-id="aec0d-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="aec0d-137">Mezi příklady patří:</span><span class="sxs-lookup"><span data-stu-id="aec0d-137">Examples of this include:</span></span>  

- <span data-ttu-id="aec0d-138">Použití DbModelBuilder k sestavení modelu bez kontextu</span><span class="sxs-lookup"><span data-stu-id="aec0d-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="aec0d-139">Použití některého jiného rozhraní Framework/Utility, který využívá DbContext, kde se tento kontext používá předtím, než se použije kontext aplikace.</span><span class="sxs-lookup"><span data-stu-id="aec0d-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="aec0d-140">V takových situacích EF nemůže najít konfiguraci automaticky a místo toho je nutné provést jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="aec0d-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="aec0d-141">V konfiguračním souboru nastavte typ DbConfiguration, jak je popsáno v části *posunutí DbConfiguration* výše.</span><span class="sxs-lookup"><span data-stu-id="aec0d-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="aec0d-142">Při spuštění aplikace zavolat statickou metodu DbConfiguration. SetConfiguration</span><span class="sxs-lookup"><span data-stu-id="aec0d-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="aec0d-143">Přepsání DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="aec0d-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="aec0d-144">Existují situace, kdy potřebujete přepsat konfigurační sadu v DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="aec0d-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="aec0d-145">Obvykle to neprovádí vývojáři aplikací, ale spíše poskytovatelé třetích stran a moduly plug-in, které nemůžou používat odvozenou třídu DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="aec0d-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="aec0d-146">V takovém případě EntityFramework umožňuje registraci obslužné rutiny události, která může upravit existující konfiguraci těsně před tím, než je uzamčena.</span><span class="sxs-lookup"><span data-stu-id="aec0d-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="aec0d-147">Také poskytuje cukrovou metodu specifickou pro nahrazení jakékoli služby vrácené lokátorem služby EF.</span><span class="sxs-lookup"><span data-stu-id="aec0d-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="aec0d-148">To je způsob, jakým se má použít:</span><span class="sxs-lookup"><span data-stu-id="aec0d-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="aec0d-149">Při spuštění aplikace (před použitím EF) musí modul plug-in nebo poskytovatel zaregistrovat metodu obslužné rutiny události pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="aec0d-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="aec0d-150">(Všimněte si, že k tomu musí dojít předtím, než aplikace použije EF.)</span><span class="sxs-lookup"><span data-stu-id="aec0d-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="aec0d-151">Obslužná rutina události provede volání ReplaceService pro každou službu, která musí být nahrazena.</span><span class="sxs-lookup"><span data-stu-id="aec0d-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="aec0d-152">Například chcete-li nahradit IDbConnectionFactory a DbProviderService, zaregistrujte obslužnou rutinu podobným způsobem:</span><span class="sxs-lookup"><span data-stu-id="aec0d-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="aec0d-153">V kódu nad MyProviderServices a MyConnectionFactory představuje vaše implementace služby.</span><span class="sxs-lookup"><span data-stu-id="aec0d-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="aec0d-154">Můžete také přidat další obslužné rutiny závislostí a získat tak stejný efekt.</span><span class="sxs-lookup"><span data-stu-id="aec0d-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="aec0d-155">Všimněte si, že můžete také zabalit DbProviderFactory tímto způsobem, ale v takovém případě bude ovlivněn pouze EF a nikoli použití DbProviderFactory mimo EF.</span><span class="sxs-lookup"><span data-stu-id="aec0d-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="aec0d-156">Z tohoto důvodu budete pravděpodobně chtít dál zabalit DbProviderFactory jako dříve.</span><span class="sxs-lookup"><span data-stu-id="aec0d-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="aec0d-157">Měli byste taky pamatovat na služby, které spouštíte externě v aplikaci – například při spuštění migrace z konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="aec0d-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="aec0d-158">Při spuštění migrace z konzoly se pokusí najít vaše DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="aec0d-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="aec0d-159">Nicméně bez ohledu na to, zda bude zabalené služby záviset na tom, kde obslužná rutina události zaregistruje.</span><span class="sxs-lookup"><span data-stu-id="aec0d-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="aec0d-160">Pokud je zaregistrován jako součást konstrukce DbConfiguration, pak by měl být spuštěn kód a služba by měla být zabalena.</span><span class="sxs-lookup"><span data-stu-id="aec0d-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="aec0d-161">Obvykle se nejedná o tento případ a to znamená, že nástroj nezíská zabalené služby.</span><span class="sxs-lookup"><span data-stu-id="aec0d-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
