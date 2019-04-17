---
title: Konfigurace založená na kódu - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619282"
---
# <a name="code-based-configuration"></a>Konfigurace založená na kódu
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Konfigurace aplikace Entity Framework se dá nastavit v konfiguračním souboru (app.config/web.config) nebo prostřednictvím kódu. Ten se označuje jako konfigurace založená na kódu.  

Konfigurace v konfiguračním souboru je popsána v tématu [samostatný článek](config-file.md). Konfigurační soubor má přednost před konfigurace založená na kódu. Jinými slovy, pokud parametr konfigurace nastaven v obou kódu a konfiguračním souboru, pak nastavení v konfiguračním souboru se používá.  

## <a name="using-dbconfiguration"></a>Pomocí DbConfiguration  

Konfigurace založená na kódu EF6 a vyšší je dosaženo vytvořením podtřída System.Data.Entity.Config.DbConfiguration. Při vytvoření podtřídy DbConfiguration by měla dodržovat následující pokyny:  

- Vytvořte pouze jednu třídu DbConfiguration pro vaši aplikaci. Tato třída určuje nastavení v celé doméně aplikace.  
- Umístěte DbConfiguration třídu do stejného sestavení jako vaší třídy DbContext. (Najdete v článku *přesunutí DbConfiguration* části, pokud chcete toto nastavení změnit.)  
- Poskytněte vaše třída DbConfiguration veřejný konstruktor bez parametrů.  
- Nastavit možnosti konfigurace voláním chráněné metody DbConfiguration z v rámci tohoto konstruktoru.  

Dodržení těchto pokynů umožňuje EF mohli objevit a používat vaše konfigurace automaticky podle i nástroje, který potřebuje přístup k modelu a při spuštění aplikace.  

## <a name="example"></a>Příklad  

Třídy odvozené od DbConfiguration může vypadat takto:  

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

Tato třída EF nastaví automaticky opakovat operace databáze se nezdařilo - a použití místní databáze pro databáze, které jsou vytvořeny pomocí konvence z Code First pomocí strategie provádění SQL Azure –.  

## <a name="moving-dbconfiguration"></a>Přesunutí DbConfiguration  

Existují případy, kdy není možné umístit DbConfiguration třídu do stejného sestavení jako vaší třídy DbContext. Například může mít dvě DbContext třídy každý v různých sestaveních. Existují dvě možnosti pro tento.  

První možností je použít konfigurační soubor pro zadání instance DbConfiguration používat. Provedete to tak, nastavte atribut codeConfigurationType oddílu objektu entityFramework. Příklad:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Hodnota codeConfigurationType musí být sestavení a oboru názvů kvalifikovaný název třídy DbConfiguration.  

Druhou možností je umístit DbConfigurationTypeAttribute na třídě kontextu. Příklad:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Hodnota předaná do atributu může být typ DbConfiguration – jak je znázorněno výše - nebo řetězec názvu typu kvalifikovaný pro sestavení a oboru názvů. Příklad:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Nastavení DbConfiguration explicitně  

Existují určité situace, kdy může být potřeba konfigurace předtím, než se použil libovolný typ DbContext. Příklady zahrnují:  

- Použití DbModelBuilder k sestavení modelu bez kontextu  
- Pomocí některé jiné nástroj/framework kód, který využívá DbContext, kde se používá tento kontext předtím, než se používá kontext aplikace  

V takových situacích je EF tak není možné zjistit konfiguraci automaticky a musí místo toho proveďte jednu z následujících akcí:  

- Nastavit typ DbConfiguration v konfiguračním souboru, jak je popsáno v *přesunutí DbConfiguration* výše uvedené části
- Zavolejte statickou metodu DbConfiguration.SetConfiguration během spuštění aplikace  

## <a name="overriding-dbconfiguration"></a>Přepsání DbConfiguration  

Existují určité situace, kdy potřebujete přepsat konfiguraci nastavení v DbConfiguration. Není to obvykle vývojáři aplikací, ale spíše poskytovatelů třetích stran a moduly plug-in, které nelze použít odvozenou třídu DbConfiguration.  

V tomto případě umožňuje objektu EntityFramework obslužné rutiny události k registraci, který můžete upravit existující konfiguraci těsně před je uzamčen.  Také poskytuje metodu sugar speciálně pro nahrazení jakoukoli službu vrácený EF lokátoru služeb. To je, jak je určena pro použití:  

- Při spuštění aplikace (před použitím EF) modul plug-in nebo zprostředkovatele měli zaregistrovat metodu obslužné rutiny události pro tuto událost. (Všimněte si, že to musíte provést před vytvořením aplikace pomocí EF.)  
- Obslužná rutina události zavolá ReplaceService pro každou službu, která potřebuje vyměnit.  

Například chcete-li nahradit IDbConnectionFactory a DbProviderService by zaregistrovat obslužnou rutinu podobný následujícímu:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

V kódu nad MyProviderServices a MyConnectionFactory představují vaše implementace služby.  

Můžete také přidat další závislosti obslužné rutiny pro docílíte stejného efektu.  

Všimněte si, že můžete také zabalit DbProviderFactory tímto způsobem, ale udělat to ovlivní pouze EF a ne používá DbProviderFactory mimo EF. Z tohoto důvodu budete pravděpodobně chtít zabalení DbProviderFactory máte před i nadále.  

Je třeba také zachovat v paměti služby, které běží externě do vaší aplikace – například při spuštění migrace v konzole Správce balíčků. Když spustíte migraci z konzoly, pokusí se najít vaše DbConfiguration. Jestli se budou získávat zabalené služby však závisí, ve kterém obslužné rutiny událostí zaregistrované. Pokud je registrován jako součást procesu vytváření vaší DbConfiguration by se měl spustit kód a služby by měl získat zabalena. Obvykle to nebude tento případ a to znamená, že nástroje nedostali zabalené služby.  
