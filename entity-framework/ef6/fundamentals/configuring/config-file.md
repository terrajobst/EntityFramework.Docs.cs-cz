---
title: Nastavení konfiguračního souboru - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490869"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="bd55f-102">Nastavení konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="bd55f-102">Configuration File Settings</span></span>
<span data-ttu-id="bd55f-103">Entity Framework umožňuje celou řadu nastavení zadané v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="bd55f-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="bd55f-104">Obecně EF následuje pravidlo "úmluva nad konfigurací": všechna nastavení popisovaná v tomto příspěvku upravit výchozí chování, stačí si dělat starosti se nastavení změní, když výchozí již splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="bd55f-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="bd55f-105">Alternativní založený na kódu</span><span class="sxs-lookup"><span data-stu-id="bd55f-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="bd55f-106">Všechna tato nastavení lze také použít, pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="bd55f-107">Počínaje EF6 jsme představili [konfigurace založená na kódu](code-based.md), který nabízí centrální způsob použití konfigurace z kódu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="bd55f-108">Před EF6 můžete přesto budou použity konfigurace z kódu, ale budete muset použít různá rozhraní API ke konfiguraci různých oblastí.</span><span class="sxs-lookup"><span data-stu-id="bd55f-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="bd55f-109">Možnost konfigurace souboru umožňuje tato nastavení se snadno změnit během nasazování bez aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="bd55f-110">Konfigurační oddíl Entity Framework</span><span class="sxs-lookup"><span data-stu-id="bd55f-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="bd55f-111">Počínaje EF4.1 nastavit inicializátor databáze pro místní používání **appSettings** oddílu konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="bd55f-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="bd55f-112">V EF 4.3 jsme představili vlastní **entityFramework** části ke zpracování nového nastavení.</span><span class="sxs-lookup"><span data-stu-id="bd55f-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="bd55f-113">Entity Framework stále rozpozná sada s použitím starý formát inicializátory databáze, ale doporučujeme, abyste přechod na nový formát, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="bd55f-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="bd55f-114">**EntityFramework** části bylo automaticky přidáno do konfiguračního souboru projektu, při instalaci balíčku NuGet objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="bd55f-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="bd55f-115">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="bd55f-115">Connection Strings</span></span>  

<span data-ttu-id="bd55f-116">[Tato stránka](~/ef6/fundamentals/configuring/connection-strings.md) obsahuje další podrobnosti o tom, jak Entity Framework rozhodne databáze, kterou chcete použít, včetně připojovacích řetězců v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="bd55f-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="bd55f-117">Připojovací řetězce přejděte ve standardu **connectionStrings** elementu a nevyžadují, aby **entityFramework** části.</span><span class="sxs-lookup"><span data-stu-id="bd55f-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="bd55f-118">Modely kódu nejprve na základě použít normální připojovací řetězce ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bd55f-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="bd55f-119">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd55f-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="bd55f-120">EF designeru na základě modelů použít speciální EF připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="bd55f-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="bd55f-121">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd55f-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="bd55f-122">Typ konfigurace založená na kódu (ef6 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="bd55f-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="bd55f-123">Počínaje EF6, můžete zadat DbConfiguration pro EF pro [konfigurace založená na kódu](code-based.md) ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd55f-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="bd55f-124">Ve většině případů není nutné určit toto nastavení jako EF automaticky zjistí vaše DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="bd55f-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="bd55f-125">Naleznete v části Podrobnosti o Pokud je nutné zadat DbConfiguration v konfiguračním souboru **přesunutí DbConfiguration** část [konfigurace založená na kódu](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="bd55f-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="bd55f-126">Nastavte typ DbConfiguration, zadáte názvem typu kvalifikovaného sestavení v **codeConfigurationType** elementu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="bd55f-127">Kvalifikovaný název sestavení je název kvalifikovaný obor názvů, za nímž následuje čárka, pak sestavení, který se typ nachází v.</span><span class="sxs-lookup"><span data-stu-id="bd55f-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="bd55f-128">Volitelně můžete také určit sestavení verze, jazykovou verzi a token veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="bd55f-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="bd55f-129">Poskytovatelé databází EF (ef6 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="bd55f-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="bd55f-130">Před EF6 musely být zahrnut jako součást poskytovatele ADO.NET core specifická pro Entity Framework součástí poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="bd55f-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="bd55f-131">Počínaje EF6, konkrétní části EF jsou teď spravované a zaregistrován samostatně.</span><span class="sxs-lookup"><span data-stu-id="bd55f-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="bd55f-132">Obvykle nebudete muset zaregistrovat poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="bd55f-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="bd55f-133">To se obvykle provádí zprostředkovatel při instalaci.</span><span class="sxs-lookup"><span data-stu-id="bd55f-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="bd55f-134">Poskytovatelé jsou registrovány zahrnutím **poskytovatele** element v rámci **poskytovatelů** podřízené část **objektu entityFramework** části.</span><span class="sxs-lookup"><span data-stu-id="bd55f-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="bd55f-135">Existují dvě povinné atributy pro položka zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="bd55f-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="bd55f-136">**invariantName** identifikuje základní poskytovatele ADO.NET, že tento EF poskytovatele cíle</span><span class="sxs-lookup"><span data-stu-id="bd55f-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="bd55f-137">**typ** je názvem typu kvalifikovaného sestavení EF implementace poskytovatele</span><span class="sxs-lookup"><span data-stu-id="bd55f-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="bd55f-138">Kvalifikovaný název sestavení je název kvalifikovaný obor názvů, za nímž následuje čárka, pak sestavení, který se typ nachází v.</span><span class="sxs-lookup"><span data-stu-id="bd55f-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="bd55f-139">Volitelně můžete také určit sestavení verze, jazykovou verzi a token veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="bd55f-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="bd55f-140">Jako příklad uvádíme položky vytvořené při instalaci rozhraní Entity Framework zaregistrovat výchozího zprostředkovatele SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bd55f-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="bd55f-141">Sběrače (EF6.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="bd55f-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="bd55f-142">Počínaje EF6.1 můžete zaregistrovat sběrače v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="bd55f-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="bd55f-143">Sběrače umožňují provozovat další logiku, když EF provede některé operace, jako je například provádění dotazů databáze, otevřením připojení atd.</span><span class="sxs-lookup"><span data-stu-id="bd55f-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="bd55f-144">Sběrače zaregistrováni zahrnutím **zachycování** element v rámci **sběrače** podřízené část **entityFramework** oddílu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="bd55f-145">Například následující konfigurace zaregistruje předdefinované **DatabaseLogger** zachycování, které budou protokolovat všechny databázové operace do konzoly.</span><span class="sxs-lookup"><span data-stu-id="bd55f-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="bd55f-146">Operace databáze protokolování do souboru (EF6.1 a vyšší)</span><span class="sxs-lookup"><span data-stu-id="bd55f-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="bd55f-147">Registrace sběrače prostřednictvím konfiguračního souboru je zvlášť užitečné, když chcete přidat do stávající aplikace k ladění problému protokolování.</span><span class="sxs-lookup"><span data-stu-id="bd55f-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="bd55f-148">**DatabaseLogger** podporuje protokolování do souboru zadáním názvu souboru jako parametr konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bd55f-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="bd55f-149">Ve výchozím nastavení to způsobí, že soubor protokolu možné přepsat pomocí nového souboru při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd55f-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="bd55f-150">Místo toho připojit k protokolu soubor již existuje-li použít něco jako:</span><span class="sxs-lookup"><span data-stu-id="bd55f-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="bd55f-151">Další informace o **DatabaseLogger** a zápis sběrače, najdete v blogovém příspěvku [EF 6.1: zapnutí protokolování bez opětovné kompilace](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="bd55f-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="bd55f-152">Kód prvního výchozí připojení objektu pro vytváření</span><span class="sxs-lookup"><span data-stu-id="bd55f-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="bd55f-153">Konfigurační oddíl umožňuje určit výchozí objekt factory připojení, Code First by měl použít k vyhledání databáze pro kontext.</span><span class="sxs-lookup"><span data-stu-id="bd55f-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="bd55f-154">Výchozí objekt pro vytváření připojení se používá pouze při přidání žádný připojovací řetězec do konfiguračního souboru pro kontext.</span><span class="sxs-lookup"><span data-stu-id="bd55f-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="bd55f-155">Pokud jste nainstalovali balíček EF NuGet byl zaregistrován výchozí objekt factory připojení, která odkazuje na SQL Express nebo LocalDB, podle toho, která jste nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="bd55f-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="bd55f-156">Pokud chcete nastavit objekt pro vytváření připojení, zadáte názvem typu kvalifikovaného sestavení v **deafultConnectionFactory** elementu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="bd55f-157">Kvalifikovaný název sestavení je název kvalifikovaný obor názvů, za nímž následuje čárka, pak sestavení, který se typ nachází v.</span><span class="sxs-lookup"><span data-stu-id="bd55f-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="bd55f-158">Volitelně můžete také určit sestavení verze, jazykovou verzi a token veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="bd55f-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="bd55f-159">Tady je příklad nastavení vlastní výchozí objekt factory připojení:</span><span class="sxs-lookup"><span data-stu-id="bd55f-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="bd55f-160">Výše uvedený příklad vyžaduje vlastní objekt pro vytváření mít konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="bd55f-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="bd55f-161">V případě potřeby můžete zadat parametry konstruktoru pomocí **parametry** elementu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="bd55f-162">SqlCeConnectionFactory, který je součástí rozhraní Entity Framework, například vyžaduje, kde zadáte výchozí název zprostředkovatele do konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="bd55f-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="bd55f-163">Výchozí název zprostředkovatele identifikuje verzi SQL Compact chcete použít.</span><span class="sxs-lookup"><span data-stu-id="bd55f-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="bd55f-164">Kontexty používat verzi SQL Compact 4.0 ve výchozím nastavení způsobí, že následující konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="bd55f-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="bd55f-165">Pokud nemají nastavený výchozí objekt factory připojení, Code First používá SqlConnectionFactory, přejdete na `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="bd55f-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="bd55f-166">SqlConnectionFactory také má konstruktor, který umožňuje potlačit částí připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="bd55f-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="bd55f-167">Pokud chcete použít instanci systému SQL Server jiných než `.\SQLEXPRESS` nastavení serveru můžete použít tento konstruktor.</span><span class="sxs-lookup"><span data-stu-id="bd55f-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="bd55f-168">Následující konfigurace způsobí, že Code First pro použití **MyDatabaseServer** kontextů, které nemají explicitní připojovacího řetězce služby nastaven.</span><span class="sxs-lookup"><span data-stu-id="bd55f-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="bd55f-169">Ve výchozím nastavení předpokládá se, že jsou argumenty konstruktoru typu String.</span><span class="sxs-lookup"><span data-stu-id="bd55f-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="bd55f-170">Chcete-li změnit to můžete použít atribut typu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="bd55f-171">Inicializátory databáze</span><span class="sxs-lookup"><span data-stu-id="bd55f-171">Database Initializers</span></span>  

<span data-ttu-id="bd55f-172">Inicializátory databáze se konfigurují na základě podle kontextu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="bd55f-173">To můžete udělat v konfiguračním souboru pomocí **kontextu** elementu.</span><span class="sxs-lookup"><span data-stu-id="bd55f-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="bd55f-174">Tento element kvalifikovaný název sestavení používá k identifikaci kontextu, která se právě nastavuje.</span><span class="sxs-lookup"><span data-stu-id="bd55f-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="bd55f-175">Ve výchozím nastavení Code First kontexty umožňují použít inicializátor CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="bd55f-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="bd55f-176">Je **disableDatabaseInitialization** atribut na **kontextu** element, který slouží k zakázání inicializaci databáze.</span><span class="sxs-lookup"><span data-stu-id="bd55f-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="bd55f-177">Například následující konfigurace zakáže inicializace databází pro kontext Blogging.BlogContext definované v MyAssembly.dll.</span><span class="sxs-lookup"><span data-stu-id="bd55f-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="bd55f-178">Můžete použít **databaseInitializer** prvek, který chcete nastavit vlastní inicializátor.</span><span class="sxs-lookup"><span data-stu-id="bd55f-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="bd55f-179">Parametry konstruktoru používají stejnou syntaxi jako objekty pro vytváření připojení výchozí.</span><span class="sxs-lookup"><span data-stu-id="bd55f-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="bd55f-180">Můžete nakonfigurovat jeden z inicializátory generickou databázi, které jsou zahrnuty v rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bd55f-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="bd55f-181">**Typ** atribut používá formát rozhraní .NET Framework pro obecné typy.</span><span class="sxs-lookup"><span data-stu-id="bd55f-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="bd55f-182">Pokud jsou pomocí migrace Code First, je třeba nakonfigurovat databáze, kterou chcete použít k migraci automaticky `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializátor.</span><span class="sxs-lookup"><span data-stu-id="bd55f-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
