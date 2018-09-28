---
title: Nastavení konfiguračního souboru - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: faba4e406b9f26f5bed6149f75c59da362d84692
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415780"
---
# <a name="configuration-file-settings"></a>Nastavení konfiguračního souboru
Entity Framework umožňuje celou řadu nastavení zadané v konfiguračním souboru. Obecně EF následuje pravidlo "úmluva nad konfigurací": všechna nastavení popisovaná v tomto příspěvku upravit výchozí chování, stačí si dělat starosti se nastavení změní, když výchozí již splňuje vaše požadavky.  

## <a name="a-code-based-alternative"></a>Alternativní založený na kódu  

Všechna tato nastavení lze také použít, pomocí kódu. Počínaje EF6 jsme představili [konfigurace založená na kódu](code-based.md), který nabízí centrální způsob použití konfigurace z kódu. Před EF6 můžete přesto budou použity konfigurace z kódu, ale budete muset použít různá rozhraní API ke konfiguraci různých oblastí. Možnost konfigurace souboru umožňuje tato nastavení se snadno změnit během nasazování bez aktualizace kódu.

## <a name="the-entity-framework-configuration-section"></a>Konfigurační oddíl Entity Framework  

Počínaje EF4.1 nastavit inicializátor databáze pro místní používání **appSettings** oddílu konfiguračního souboru. V EF 4.3 jsme představili vlastní **entityFramework** části ke zpracování nového nastavení. Entity Framework stále rozpozná sada s použitím starý formát inicializátory databáze, ale doporučujeme, abyste přechod na nový formát, kde je to možné.

**EntityFramework** části bylo automaticky přidáno do konfiguračního souboru projektu, při instalaci balíčku NuGet objektu EntityFramework.  

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

[Tato stránka](~/ef6/fundamentals/configuring/connection-strings.md) obsahuje další podrobnosti o tom, jak Entity Framework rozhodne databáze, kterou chcete použít, včetně připojovacích řetězců v konfiguračním souboru.  

Připojovací řetězce přejděte ve standardu **connectionStrings** elementu a nevyžadují, aby **entityFramework** části.  

Modely kódu nejprve na základě použít normální připojovací řetězce ADO.NET. Příklad:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF designeru na základě modelů použít speciální EF připojovací řetězce. Příklad:  

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

## <a name="code-based-configuration-type-ef6-onwards"></a>Typ konfigurace založená na kódu (ef6 nebo novější)  

Počínaje EF6, můžete zadat DbConfiguration pro EF pro [konfigurace založená na kódu](code-based.md) ve vaší aplikaci. Ve většině případů není nutné určit toto nastavení jako EF automaticky zjistí vaše DbConfiguration. Naleznete v části Podrobnosti o Pokud je nutné zadat DbConfiguration v konfiguračním souboru **přesunutí DbConfiguration** část [konfigurace založená na kódu](code-based.md).  

Nastavte typ DbConfiguration, zadáte názvem typu kvalifikovaného sestavení v **codeConfigurationType** elementu.  

> [!NOTE]
> Kvalifikovaný název sestavení je název kvalifikovaný obor názvů, za nímž následuje čárka, pak sestavení, který se typ nachází v. Volitelně můžete také určit sestavení verze, jazykovou verzi a token veřejného klíče.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Poskytovatelé databází EF (ef6 nebo novější)  

Před EF6 musely být zahrnut jako součást poskytovatele ADO.NET core specifická pro Entity Framework součástí poskytovatele databáze. Počínaje EF6, konkrétní části EF jsou teď spravované a zaregistrován samostatně.  

Obvykle nebudete muset zaregistrovat poskytovatele. To se obvykle provádí zprostředkovatel při instalaci.  

Poskytovatelé jsou registrovány zahrnutím **poskytovatele** element v rámci **poskytovatelů** podřízené část **objektu entityFramework** části. Existují dvě povinné atributy pro položka zprostředkovatele:  

- **invariantName** identifikuje základní poskytovatele ADO.NET, že tento EF poskytovatele cíle  
- **typ** je názvem typu kvalifikovaného sestavení EF implementace poskytovatele  

> [!NOTE]
> Kvalifikovaný název sestavení je název kvalifikovaný obor názvů, za nímž následuje čárka, pak sestavení, který se typ nachází v. Volitelně můžete také určit sestavení verze, jazykovou verzi a token veřejného klíče.  

Jako příklad uvádíme položky vytvořené při instalaci rozhraní Entity Framework zaregistrovat výchozího zprostředkovatele SQL Server.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Sběrače (EF6.1 a vyšší)  

Počínaje EF6.1 můžete zaregistrovat sběrače v konfiguračním souboru. Sběrače umožňují provozovat další logiku, když EF provede některé operace, jako je například provádění dotazů databáze, otevřením připojení atd.  

Sběrače zaregistrováni zahrnutím **zachycování** element v rámci **sběrače** podřízené část **entityFramework** oddílu. Například následující konfigurace zaregistruje předdefinované **DatabaseLogger** zachycování, které budou protokolovat všechny databázové operace do konzoly.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Operace databáze protokolování do souboru (EF6.1 a vyšší)  

Registrace sběrače prostřednictvím konfiguračního souboru je zvlášť užitečné, když chcete přidat do stávající aplikace k ladění problému protokolování. **DatabaseLogger** podporuje protokolování do souboru zadáním názvu souboru jako parametr konstruktoru.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Ve výchozím nastavení to způsobí, že soubor protokolu možné přepsat pomocí nového souboru při každém spuštění aplikace. Místo toho připojit k protokolu soubor již existuje-li použít něco jako:  

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

Další informace o **DatabaseLogger** a zápis sběrače, najdete v blogovém příspěvku [EF 6.1: zapnutí protokolování bez opětovné kompilace](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Kód prvního výchozí připojení objektu pro vytváření  

Konfigurační oddíl umožňuje určit výchozí objekt factory připojení, Code First by měl použít k vyhledání databáze pro kontext. Výchozí objekt pro vytváření připojení se používá pouze při přidání žádný připojovací řetězec do konfiguračního souboru pro kontext.  

Pokud jste nainstalovali balíček EF NuGet byl zaregistrován výchozí objekt factory připojení, která odkazuje na SQL Express nebo LocalDB, podle toho, která jste nainstalovali.  

Pokud chcete nastavit objekt pro vytváření připojení, zadáte názvem typu kvalifikovaného sestavení v **defaultConnectionFactory** elementu.  

> [!NOTE]
> Kvalifikovaný název sestavení je název kvalifikovaný obor názvů, za nímž následuje čárka, pak sestavení, který se typ nachází v. Volitelně můžete také určit sestavení verze, jazykovou verzi a token veřejného klíče.  

Tady je příklad nastavení vlastní výchozí objekt factory připojení:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Výše uvedený příklad vyžaduje vlastní objekt pro vytváření mít konstruktor bez parametrů. V případě potřeby můžete zadat parametry konstruktoru pomocí **parametry** elementu.  

SqlCeConnectionFactory, který je součástí rozhraní Entity Framework, například vyžaduje, kde zadáte výchozí název zprostředkovatele do konstruktoru. Výchozí název zprostředkovatele identifikuje verzi SQL Compact chcete použít. Kontexty používat verzi SQL Compact 4.0 ve výchozím nastavení způsobí, že následující konfiguraci.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Pokud nemají nastavený výchozí objekt factory připojení, Code First používá SqlConnectionFactory, přejdete na `.\SQLEXPRESS`. SqlConnectionFactory také má konstruktor, který umožňuje potlačit částí připojovacího řetězce. Pokud chcete použít instanci systému SQL Server jiných než `.\SQLEXPRESS` nastavení serveru můžete použít tento konstruktor.  

Následující konfigurace způsobí, že Code First pro použití **MyDatabaseServer** kontextů, které nemají explicitní připojovacího řetězce služby nastaven.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Ve výchozím nastavení předpokládá se, že jsou argumenty konstruktoru typu String. Chcete-li změnit to můžete použít atribut typu.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicializátory databáze  

Inicializátory databáze se konfigurují na základě podle kontextu. To můžete udělat v konfiguračním souboru pomocí **kontextu** elementu. Tento element kvalifikovaný název sestavení používá k identifikaci kontextu, která se právě nastavuje.  

Ve výchozím nastavení Code First kontexty umožňují použít inicializátor CreateDatabaseIfNotExists. Je **disableDatabaseInitialization** atribut na **kontextu** element, který slouží k zakázání inicializaci databáze.  

Například následující konfigurace zakáže inicializace databází pro kontext Blogging.BlogContext definované v MyAssembly.dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Můžete použít **databaseInitializer** prvek, který chcete nastavit vlastní inicializátor.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Parametry konstruktoru používají stejnou syntaxi jako objekty pro vytváření připojení výchozí.  

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

Můžete nakonfigurovat jeden z inicializátory generickou databázi, které jsou zahrnuty v rozhraní Entity Framework. **Typ** atribut používá formát rozhraní .NET Framework pro obecné typy.  

Pokud jsou pomocí migrace Code First, je třeba nakonfigurovat databáze, kterou chcete použít k migraci automaticky `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializátor.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
