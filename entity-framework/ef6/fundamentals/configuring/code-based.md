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
# <a name="code-based-configuration"></a>Konfigurace na základě kódu
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Konfiguraci pro aplikaci Entity Framework lze zadat v konfiguračním souboru (App. config/Web. config) nebo prostřednictvím kódu. Druhá je známá jako Konfigurace založená na kódu.  

Konfigurace v konfiguračním souboru je popsána v [samostatném článku](config-file.md). Konfigurační soubor má přednost před konfigurací založenou na kódu. Jinými slovy, pokud je možnost konfigurace nastavena v kódu i v konfiguračním souboru, bude použito nastavení v konfiguračním souboru.  

## <a name="using-dbconfiguration"></a>Použití DbConfiguration  

Konfigurace na základě kódu v EF6 a výše se dosahuje vytvořením podtříd třídy System. data. entity. config. DbConfiguration. Při DbConfigurationí podtříd by měly být dodrženy následující pokyny:  

- Pro svou aplikaci vytvořte pouze jednu třídu DbConfiguration. Tato třída určuje nastavení pro šířku domény pro aplikaci.  
- Třídu DbConfiguration umístěte do stejného sestavení jako třídu DbContext. (Pokud ho chcete změnit, přečtěte si část *přesunutí DbConfiguration* .)  
- Poskytněte třídu DbConfiguration veřejný konstruktor bez parametrů.  
- Nastavte možnosti konfigurace voláním chráněných metod DbConfiguration z tohoto konstruktoru.  

Podle těchto pokynů umožňuje EF zjistit a použít konfiguraci automaticky pomocí nástrojů, které potřebují přístup k vašemu modelu a při spuštění aplikace.  

## <a name="example"></a>Příklad  

Třída odvozená z DbConfiguration může vypadat takto:  

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

Tato třída nastaví EF, aby používala strategii spouštění SQL Azure – k automatickému opakování neúspěšných operací databáze a k použití místní databáze pro databáze, které jsou vytvořeny pomocí konvence z Code First.  

## <a name="moving-dbconfiguration"></a>Přesunutí DbConfiguration  

Existují případy, kdy není možné umístit třídu DbConfiguration do stejného sestavení jako třídu DbContext. Například můžete mít dvě třídy DbContext v různých sestaveních. Existují dvě možnosti, jak to zpracovat.  

První možností je použít konfigurační soubor k určení instance DbConfiguration, která se má použít. Pokud to chcete provést, nastavte atribut codeConfigurationType oddílu entityFramework. Příklad:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Hodnota codeConfigurationType musí být sestavení a kvalifikovaný název oboru názvů vaší třídy DbConfiguration.  

Druhou možností je umístit DbConfigurationTypeAttribute na svou kontextovou třídu. Příklad:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Hodnota předaná atributu může být buď váš typ DbConfiguration, jak je znázorněno výše, nebo sestavení a kvalifikovaný název typu řetězce pro obor názvů. Příklad:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Explicitní nastavení DbConfiguration  

V některých situacích se může stát, že se konfigurace bude potřebovat předtím, než se použije libovolný DbContext typ. Mezi příklady patří:  

- Použití DbModelBuilder k sestavení modelu bez kontextu  
- Použití některého jiného rozhraní Framework/Utility, který využívá DbContext, kde se tento kontext používá předtím, než se použije kontext aplikace.  

V takových situacích EF nemůže najít konfiguraci automaticky a místo toho je nutné provést jednu z následujících akcí:  

- V konfiguračním souboru nastavte typ DbConfiguration, jak je popsáno v části *posunutí DbConfiguration* výše.
- Při spuštění aplikace zavolat statickou metodu DbConfiguration. SetConfiguration  

## <a name="overriding-dbconfiguration"></a>Přepsání DbConfiguration  

Existují situace, kdy potřebujete přepsat konfigurační sadu v DbConfiguration. Obvykle to neprovádí vývojáři aplikací, ale spíše poskytovatelé třetích stran a moduly plug-in, které nemůžou používat odvozenou třídu DbConfiguration.  

V takovém případě EntityFramework umožňuje registraci obslužné rutiny události, která může upravit existující konfiguraci těsně před tím, než je uzamčena.  Také poskytuje cukrovou metodu specifickou pro nahrazení jakékoli služby vrácené lokátorem služby EF. To je způsob, jakým se má použít:  

- Při spuštění aplikace (před použitím EF) musí modul plug-in nebo poskytovatel zaregistrovat metodu obslužné rutiny události pro tuto událost. (Všimněte si, že k tomu musí dojít předtím, než aplikace použije EF.)  
- Obslužná rutina události provede volání ReplaceService pro každou službu, která musí být nahrazena.  

Například chcete-li nahradit IDbConnectionFactory a DbProviderService, zaregistrujte obslužnou rutinu podobným způsobem:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

V kódu nad MyProviderServices a MyConnectionFactory představuje vaše implementace služby.  

Můžete také přidat další obslužné rutiny závislostí a získat tak stejný efekt.  

Všimněte si, že můžete také zabalit DbProviderFactory tímto způsobem, ale v takovém případě bude ovlivněn pouze EF a nikoli použití DbProviderFactory mimo EF. Z tohoto důvodu budete pravděpodobně chtít dál zabalit DbProviderFactory jako dříve.  

Měli byste taky pamatovat na služby, které spouštíte externě v aplikaci – například při spuštění migrace z konzoly Správce balíčků. Při spuštění migrace z konzoly se pokusí najít vaše DbConfiguration. Nicméně bez ohledu na to, zda bude zabalené služby záviset na tom, kde obslužná rutina události zaregistruje. Pokud je zaregistrován jako součást konstrukce DbConfiguration, pak by měl být spuštěn kód a služba by měla být zabalena. Obvykle se nejedná o tento případ a to znamená, že nástroj nezíská zabalené služby.  
