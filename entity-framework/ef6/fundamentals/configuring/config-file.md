---
title: Nastavení konfiguračního souboru – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886558"
---
# <a name="configuration-file-settings"></a>Nastavení konfiguračního souboru
Entity Framework umožňuje zadat řadu nastavení z konfiguračního souboru. V obecném EF se řídí principem konvence pro konfiguraci: všechna nastavení popisovaná v tomto příspěvku mají výchozí chování, stačí se starat o změnu nastavení jenom v případě, že výchozí hodnota už nevyhovuje vašim požadavkům.  

## <a name="a-code-based-alternative"></a>Alternativa na základě kódu  

Všechna tato nastavení lze použít také pomocí kódu. Počínaje EF6 jsme zavedli [konfiguraci na základě kódu](code-based.md), která poskytuje centrální způsob použití konfigurace z kódu. Před EF6 může být konfigurace stále použita z kódu, ale je nutné použít různá rozhraní API ke konfiguraci různých oblastí. Možnost konfiguračního souboru umožňuje, aby tato nastavení byla během nasazení snadno měněna bez aktualizace kódu.

## <a name="the-entity-framework-configuration-section"></a>Konfigurační oddíl Entity Framework  

Počínaje verzí EF 4.1 můžete nastavit inicializátor databáze pro kontext pomocí oddílu **appSettings** konfiguračního souboru. V EF 4,3 jsme zavedli oddíl Custom **entityFramework** pro zpracování nového nastavení. Entity Framework stále rozpozná Inicializátory databáze nastavené ve starém formátu, ale doporučujeme přesunout do nového formátu, pokud je to možné.

Oddíl **entityFramework** byl automaticky přidán do konfiguračního souboru projektu při instalaci balíčku NuGet entityFramework.  

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

## <a name="connection-strings"></a>Připojovací řetězce  

[Tato stránka](~/ef6/fundamentals/configuring/connection-strings.md) obsahuje další podrobnosti o tom, jak Entity Framework Určuje databázi, která se má použít, včetně připojovacích řetězců v konfiguračním souboru.  

Připojovací řetězce přecházejí do standardního elementu connectionStrings a nevyžadují oddíl **entityFramework** .  

Modely založené na Code First používají normální připojovací řetězce ADO.NET. Příklad:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Modely založené na návrháři EF používají speciální připojovací řetězce EF. Příklad:  

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

## <a name="code-based-configuration-type-ef6-onwards"></a>Typ konfigurace založený na kódu (EF6 a vyšší)  

Počínaje EF6 můžete určit DbConfiguration pro EF, který se použije pro [konfiguraci na základě kódu](code-based.md) v aplikaci. Ve většině případů nemusíte toto nastavení zadávat, protože EF bude automaticky zjišťovat vaše DbConfiguration. Podrobnosti o tom, kdy možná budete muset zadat DbConfiguration v konfiguračním souboru, najdete v oddílu **přesunutí DbConfiguration** v [konfiguraci založené na kódu](code-based.md).  

Chcete-li nastavit typ DbConfiguration, zadejte název kvalifikovaného typu sestavení v elementu **codeConfigurationType** .  

> [!NOTE]
> Kvalifikovaný název sestavení je kvalifikovaný název oboru názvů následovaný čárkou a pak sestavení, ve kterém se nachází typ. Volitelně můžete také zadat verzi sestavení, jazykovou verzi a token veřejného klíče.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Zprostředkovatelé databáze EF (EF6 a vyšší)  

Před EF6 Entity Framework se museli v rámci základního poskytovatele ADO.NET zahrnout konkrétní části poskytovatele databáze. Počínaje verzí EF6 se teď součásti specifické pro EF spravují a registrují samostatně.  

Normálně nebudete muset registrovat poskytovatele sami. To se obvykle provede poskytovatelem, když ho nainstalujete.  

Poskytovatelé jsou zaregistrovaní zahrnutím elementu **Provider** do podřízené části poskytovatelé oddílu **entityFramework** . Existují dva povinné atributy pro položku zprostředkovatele:  

- invariantní identifikuje základního poskytovatele ADO.NET, který je cílem tohoto poskytovatele EF.  
- **Type** je kvalifikovaný název typu sestavení pro implementaci zprostředkovatele EF.  

> [!NOTE]
> Kvalifikovaný název sestavení je kvalifikovaný název oboru názvů následovaný čárkou a pak sestavení, ve kterém se nachází typ. Volitelně můžete také zadat verzi sestavení, jazykovou verzi a token veřejného klíče.  

Příkladem je položka vytvořená k registraci výchozího poskytovatele SQL Server při instalaci Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Zachycení (EF 6.1 a vyšší)  

Počínaje verzí EF 6.1 můžete zachycení zaregistrovat v konfiguračním souboru. Zachycení umožňují spustit další logiku, když EF provede určité operace, jako je například provádění databázových dotazů, otevírání připojení atd.  

Zachycení jsou zaregistrovaná zahrnutím elementu pro **zachycování** do podřízené části **entityFramework** v oddílu pro **zachycení** . Následující konfigurace například registruje integrovaný zachytávací **DatabaseLogger** , který zaznamená všechny databázové operace do konzoly.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Protokolování operací databáze do souboru (EF 6.1 a vyšší)  

Registrace zachycení prostřednictvím konfiguračního souboru je obzvláště užitečná v případě, že chcete přidat protokolování do existující aplikace, které vám pomůžou s laděním problému. **DatabaseLogger** podporuje protokolování do souboru zadáním názvu souboru jako parametru konstruktoru.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Ve výchozím nastavení to způsobí, že soubor protokolu bude při každém spuštění aplikace přepsán novým souborem. Chcete-li místo toho připojit k souboru protokolu, pokud již existuje, použijte něco podobného:  

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

Další informace o **DatabaseLogger** a registrech zachycení najdete v blogovém příspěvku [EF 6,1: Zapínání protokolování bez opětovné](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)kompilace.  

## <a name="code-first-default-connection-factory"></a>Code First výchozí objekt pro vytváření připojení  

Konfigurační oddíl umožňuje určit výchozí objekt pro vytváření připojení, který Code First použít k vyhledání databáze pro použití v kontextu. Výchozí objekt pro vytváření připojení se používá jenom v případě, že se do konfiguračního souboru pro kontext nepřidal žádný připojovací řetězec.  

Při instalaci balíčku NuGet NuGet byl zaregistrován výchozí objekt pro vytváření připojení, který odkazuje buď na SQL Express, nebo na LocalDB, v závislosti na tom, který z nich jste nainstalovali.  

Chcete-li nastavit objekt pro vytváření připojení, zadejte v elementu **defaultConnectionFactory** název kvalifikovaného typu sestavení.  

> [!NOTE]
> Kvalifikovaný název sestavení je kvalifikovaný název oboru názvů následovaný čárkou a pak sestavení, ve kterém se nachází typ. Volitelně můžete také zadat verzi sestavení, jazykovou verzi a token veřejného klíče.  

Tady je příklad nastavení vlastního výchozího objektu pro vytváření připojení:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Výše uvedený příklad vyžaduje, aby vlastní továrna měla konstruktor bez parametrů. V případě potřeby můžete zadat parametry konstruktoru pomocí elementu **Parameters** .  

Například SqlCeConnectionFactory, který je součástí Entity Framework, vyžaduje, abyste zadali neutrální název poskytovatele do konstruktoru. Neutrální název poskytovatele určuje verzi SQL Compact, kterou chcete použít. Následující konfigurace způsobí, že ve výchozím nastavení používají kontexty SQL Compact verze 4,0.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Pokud nenastavíte výchozí objekt pro vytváření připojení, Code First používá SqlConnectionFactory a odkazuje na `.\SQLEXPRESS`. SqlConnectionFactory má také konstruktor, který umožňuje přepsat části připojovacího řetězce. Pokud chcete použít jinou instanci SQL Server, než `.\SQLEXPRESS` můžete použít tento konstruktor k nastavení serveru.  

Následující konfigurace způsobí, že Code First používat **MyDatabaseServer** pro kontexty, které nemají explicitní sadu připojovacích řetězců.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Ve výchozím nastavení se předpokládá, že argumenty konstruktoru jsou typu String. Můžete použít atribut Type pro změnu.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicializátory databáze  

Inicializátory databáze jsou nakonfigurovány pro jednotlivé kontexty. Lze je nastavit v konfiguračním souboru pomocí elementu **Context** . Tento prvek používá pro identifikaci konfigurovaného kontextu sestavení kvalifikovaný název.  

Ve výchozím nastavení jsou Code First kontexty nakonfigurovány pro použití inicializátoru metodu createdatabaseifnotexists. V elementu **kontextu** je atribut **disableDatabaseInitialization** , který lze použít k zakázání inicializace databáze.  

Například následující konfigurace zakáže inicializaci databáze pro kontext blogů. BlogContext definovaný v MyAssembly. dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Pomocí elementu **databaseInitializer** můžete nastavit vlastní inicializátor.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Parametry konstruktoru používají stejnou syntaxi jako výchozí objekty pro vytváření připojení.  

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

Můžete nakonfigurovat jeden z generických inicializátorů databáze, které jsou součástí Entity Framework. Atribut **Type** používá formát .NET Framework pro obecné typy.  

Například pokud používáte migrace Code First, můžete nakonfigurovat databázi, která se má automaticky migrovat pomocí `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializátoru.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
