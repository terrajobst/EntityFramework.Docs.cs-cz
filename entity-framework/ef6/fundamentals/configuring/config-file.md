---
title: Nastavení konfiguračního souboru – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417971"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="edd22-102">Nastavení konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="edd22-102">Configuration File Settings</span></span>
<span data-ttu-id="edd22-103">Entity Framework umožňuje zadat řadu nastavení z konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="edd22-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="edd22-104">V obecném EF se řídí principem konvence pro konfiguraci: všechna nastavení popisovaná v tomto příspěvku mají výchozí chování, stačí se starat o změnu nastavení jenom v případě, že výchozí hodnota už nevyhovuje vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="edd22-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="edd22-105">Alternativa na základě kódu</span><span class="sxs-lookup"><span data-stu-id="edd22-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="edd22-106">Všechna tato nastavení lze použít také pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="edd22-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="edd22-107">Počínaje EF6 jsme zavedli [konfiguraci na základě kódu](code-based.md), která poskytuje centrální způsob použití konfigurace z kódu.</span><span class="sxs-lookup"><span data-stu-id="edd22-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="edd22-108">Před EF6 může být konfigurace stále použita z kódu, ale je nutné použít různá rozhraní API ke konfiguraci různých oblastí.</span><span class="sxs-lookup"><span data-stu-id="edd22-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="edd22-109">Možnost konfiguračního souboru umožňuje, aby tato nastavení byla během nasazení snadno měněna bez aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="edd22-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="edd22-110">Konfigurační oddíl Entity Framework</span><span class="sxs-lookup"><span data-stu-id="edd22-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="edd22-111">Počínaje verzí EF 4.1 můžete nastavit inicializátor databáze pro kontext pomocí oddílu **appSettings** konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="edd22-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="edd22-112">V EF 4,3 jsme zavedli oddíl Custom **entityFramework** pro zpracování nového nastavení.</span><span class="sxs-lookup"><span data-stu-id="edd22-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="edd22-113">Entity Framework stále rozpozná Inicializátory databáze nastavené ve starém formátu, ale doporučujeme přesunout do nového formátu, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="edd22-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="edd22-114">Oddíl **entityFramework** byl automaticky přidán do konfiguračního souboru projektu při instalaci balíčku NuGet entityFramework.</span><span class="sxs-lookup"><span data-stu-id="edd22-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a><span data-ttu-id="edd22-115">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="edd22-115">Connection Strings</span></span>  

<span data-ttu-id="edd22-116">[Tato stránka](~/ef6/fundamentals/configuring/connection-strings.md) obsahuje další podrobnosti o tom, jak Entity Framework Určuje databázi, která se má použít, včetně připojovacích řetězců v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="edd22-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="edd22-117">Připojovací řetězce přecházejí do standardního elementu **connectionStrings** a nevyžadují oddíl **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="edd22-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="edd22-118">Modely založené na Code First používají normální připojovací řetězce ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="edd22-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="edd22-119">Příklad:</span><span class="sxs-lookup"><span data-stu-id="edd22-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="edd22-120">Modely založené na návrháři EF používají speciální připojovací řetězce EF.</span><span class="sxs-lookup"><span data-stu-id="edd22-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="edd22-121">Příklad:</span><span class="sxs-lookup"><span data-stu-id="edd22-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="edd22-122">Typ konfigurace založený na kódu (EF6 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="edd22-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="edd22-123">Počínaje EF6 můžete určit DbConfiguration pro EF, který se použije pro [konfiguraci na základě kódu](code-based.md) v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="edd22-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="edd22-124">Ve většině případů nemusíte toto nastavení zadávat, protože EF bude automaticky zjišťovat vaše DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="edd22-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="edd22-125">Podrobnosti o tom, kdy možná budete muset zadat DbConfiguration v konfiguračním souboru, najdete v oddílu **přesunutí DbConfiguration** v [konfiguraci založené na kódu](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="edd22-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="edd22-126">Chcete-li nastavit typ DbConfiguration, zadejte název kvalifikovaného typu sestavení v elementu **codeConfigurationType** .</span><span class="sxs-lookup"><span data-stu-id="edd22-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="edd22-127">Kvalifikovaný název sestavení je kvalifikovaný název oboru názvů následovaný čárkou a pak sestavení, ve kterém se nachází typ.</span><span class="sxs-lookup"><span data-stu-id="edd22-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="edd22-128">Volitelně můžete také zadat verzi sestavení, jazykovou verzi a token veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="edd22-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="edd22-129">Zprostředkovatelé databáze EF (EF6 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="edd22-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="edd22-130">Před EF6 Entity Framework se museli v rámci základního poskytovatele ADO.NET zahrnout konkrétní části poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="edd22-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="edd22-131">Počínaje verzí EF6 se teď součásti specifické pro EF spravují a registrují samostatně.</span><span class="sxs-lookup"><span data-stu-id="edd22-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="edd22-132">Normálně nebudete muset registrovat poskytovatele sami.</span><span class="sxs-lookup"><span data-stu-id="edd22-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="edd22-133">To se obvykle provede poskytovatelem, když ho nainstalujete.</span><span class="sxs-lookup"><span data-stu-id="edd22-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="edd22-134">Poskytovatelé jsou zaregistrovaní zahrnutím elementu **Provider** do podřízené části **poskytovatelé** oddílu **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="edd22-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="edd22-135">Existují dva povinné atributy pro položku zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="edd22-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="edd22-136">**invariantní** identifikuje základního poskytovatele ADO.NET, který je cílem tohoto poskytovatele EF.</span><span class="sxs-lookup"><span data-stu-id="edd22-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="edd22-137">**Type** je kvalifikovaný název typu sestavení pro implementaci zprostředkovatele EF.</span><span class="sxs-lookup"><span data-stu-id="edd22-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="edd22-138">Kvalifikovaný název sestavení je kvalifikovaný název oboru názvů následovaný čárkou a pak sestavení, ve kterém se nachází typ.</span><span class="sxs-lookup"><span data-stu-id="edd22-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="edd22-139">Volitelně můžete také zadat verzi sestavení, jazykovou verzi a token veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="edd22-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="edd22-140">Příkladem je položka vytvořená k registraci výchozího poskytovatele SQL Server při instalaci Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="edd22-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="edd22-141">Zachycení (EF 6.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="edd22-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="edd22-142">Počínaje verzí EF 6.1 můžete zachycení zaregistrovat v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="edd22-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="edd22-143">Zachycení umožňují spustit další logiku, když EF provede určité operace, jako je například provádění databázových dotazů, otevírání připojení atd.</span><span class="sxs-lookup"><span data-stu-id="edd22-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="edd22-144">Zachycení jsou zaregistrovaná zahrnutím elementu pro **zachycování** do podřízené části **entityFramework** v oddílu pro **zachycení** .</span><span class="sxs-lookup"><span data-stu-id="edd22-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="edd22-145">Následující konfigurace například registruje integrovaný zachytávací **DatabaseLogger** , který zaznamená všechny databázové operace do konzoly.</span><span class="sxs-lookup"><span data-stu-id="edd22-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="edd22-146">Protokolování operací databáze do souboru (EF 6.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="edd22-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="edd22-147">Registrace zachycení prostřednictvím konfiguračního souboru je obzvláště užitečná v případě, že chcete přidat protokolování do existující aplikace, které vám pomůžou s laděním problému.</span><span class="sxs-lookup"><span data-stu-id="edd22-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="edd22-148">**DatabaseLogger** podporuje protokolování do souboru zadáním názvu souboru jako parametru konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="edd22-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="edd22-149">Ve výchozím nastavení to způsobí, že soubor protokolu bude při každém spuštění aplikace přepsán novým souborem.</span><span class="sxs-lookup"><span data-stu-id="edd22-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="edd22-150">Chcete-li místo toho připojit k souboru protokolu, pokud již existuje, použijte něco podobného:</span><span class="sxs-lookup"><span data-stu-id="edd22-150">To instead append to the log file if it already exists use something like:</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="edd22-151">Další informace o **DatabaseLogger** a registrech zachycení najdete v blogovém příspěvku [EF 6,1: Zapnutí protokolování bez opětovné kompilace](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="edd22-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="edd22-152">Code First výchozí objekt pro vytváření připojení</span><span class="sxs-lookup"><span data-stu-id="edd22-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="edd22-153">Konfigurační oddíl umožňuje určit výchozí objekt pro vytváření připojení, který Code First použít k vyhledání databáze pro použití v kontextu.</span><span class="sxs-lookup"><span data-stu-id="edd22-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="edd22-154">Výchozí objekt pro vytváření připojení se používá jenom v případě, že se do konfiguračního souboru pro kontext nepřidal žádný připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="edd22-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="edd22-155">Při instalaci balíčku NuGet NuGet byl zaregistrován výchozí objekt pro vytváření připojení, který odkazuje buď na SQL Express, nebo na LocalDB, v závislosti na tom, který z nich jste nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="edd22-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="edd22-156">Chcete-li nastavit objekt pro vytváření připojení, zadejte v elementu **defaultConnectionFactory** název kvalifikovaného typu sestavení.</span><span class="sxs-lookup"><span data-stu-id="edd22-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="edd22-157">Kvalifikovaný název sestavení je kvalifikovaný název oboru názvů následovaný čárkou a pak sestavení, ve kterém se nachází typ.</span><span class="sxs-lookup"><span data-stu-id="edd22-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="edd22-158">Volitelně můžete také zadat verzi sestavení, jazykovou verzi a token veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="edd22-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="edd22-159">Tady je příklad nastavení vlastního výchozího objektu pro vytváření připojení:</span><span class="sxs-lookup"><span data-stu-id="edd22-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="edd22-160">Výše uvedený příklad vyžaduje, aby vlastní továrna měla konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="edd22-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="edd22-161">V případě potřeby můžete zadat parametry konstruktoru pomocí elementu **Parameters** .</span><span class="sxs-lookup"><span data-stu-id="edd22-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="edd22-162">Například SqlCeConnectionFactory, který je součástí Entity Framework, vyžaduje, abyste zadali neutrální název poskytovatele do konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="edd22-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="edd22-163">Neutrální název poskytovatele určuje verzi SQL Compact, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="edd22-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="edd22-164">Následující konfigurace způsobí, že ve výchozím nastavení používají kontexty SQL Compact verze 4,0.</span><span class="sxs-lookup"><span data-stu-id="edd22-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="edd22-165">Pokud nenastavíte výchozí objekt pro vytváření připojení, Code First používá SqlConnectionFactory, přejděte na `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="edd22-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="edd22-166">SqlConnectionFactory má také konstruktor, který umožňuje přepsat části připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="edd22-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="edd22-167">Pokud chcete použít instanci SQL Server jinou než `.\SQLEXPRESS` můžete tento konstruktor použít k nastavení serveru.</span><span class="sxs-lookup"><span data-stu-id="edd22-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="edd22-168">Následující konfigurace způsobí, že Code First používat **MyDatabaseServer** pro kontexty, které nemají explicitní sadu připojovacích řetězců.</span><span class="sxs-lookup"><span data-stu-id="edd22-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="edd22-169">Ve výchozím nastavení se předpokládá, že argumenty konstruktoru jsou typu String.</span><span class="sxs-lookup"><span data-stu-id="edd22-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="edd22-170">Můžete použít atribut Type pro změnu.</span><span class="sxs-lookup"><span data-stu-id="edd22-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="edd22-171">Inicializátory databáze</span><span class="sxs-lookup"><span data-stu-id="edd22-171">Database Initializers</span></span>  

<span data-ttu-id="edd22-172">Inicializátory databáze jsou nakonfigurovány pro jednotlivé kontexty.</span><span class="sxs-lookup"><span data-stu-id="edd22-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="edd22-173">Lze je nastavit v konfiguračním souboru pomocí elementu **Context** .</span><span class="sxs-lookup"><span data-stu-id="edd22-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="edd22-174">Tento prvek používá pro identifikaci konfigurovaného kontextu sestavení kvalifikovaný název.</span><span class="sxs-lookup"><span data-stu-id="edd22-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="edd22-175">Ve výchozím nastavení jsou Code First kontexty nakonfigurovány pro použití inicializátoru metodu createdatabaseifnotexists.</span><span class="sxs-lookup"><span data-stu-id="edd22-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="edd22-176">V elementu **kontextu** je atribut **disableDatabaseInitialization** , který lze použít k zakázání inicializace databáze.</span><span class="sxs-lookup"><span data-stu-id="edd22-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="edd22-177">Například následující konfigurace zakáže inicializaci databáze pro kontext blogů. BlogContext definovaný v MyAssembly. dll.</span><span class="sxs-lookup"><span data-stu-id="edd22-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="edd22-178">Pomocí elementu **databaseInitializer** můžete nastavit vlastní inicializátor.</span><span class="sxs-lookup"><span data-stu-id="edd22-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="edd22-179">Parametry konstruktoru používají stejnou syntaxi jako výchozí objekty pro vytváření připojení.</span><span class="sxs-lookup"><span data-stu-id="edd22-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

<span data-ttu-id="edd22-180">Můžete nakonfigurovat jeden z generických inicializátorů databáze, které jsou součástí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="edd22-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="edd22-181">Atribut **Type** používá formát .NET Framework pro obecné typy.</span><span class="sxs-lookup"><span data-stu-id="edd22-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="edd22-182">Například pokud používáte Migrace Code First, můžete nakonfigurovat databázi, která se má migrovat automaticky pomocí inicializátoru `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>`.</span><span class="sxs-lookup"><span data-stu-id="edd22-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
