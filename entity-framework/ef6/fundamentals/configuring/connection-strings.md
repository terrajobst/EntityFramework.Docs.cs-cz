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
# <a name="connection-strings-and-models"></a>Připojovací řetězce a modely
V tomto tématu se dozvíte, jak Entity Framework zjistit, které databázové připojení se má použít, a jak ho můžete změnit. V tomto tématu jsou uvedeny modely vytvořené pomocí Code First a Návrhář EF.  

Obvykle aplikace Entity Framework používá třídu odvozenou z DbContext. Tato odvozená třída zavolá jeden z konstruktorů základní třídy DbContext, který bude řídit:  

- Jak se bude kontext připojovat k databázi – to znamená, jak se našel nebo používá připojovací řetězec  
- Zda bude kontext používat výpočet modelu pomocí Code First nebo načtení modelu vytvořeného pomocí návrháře EF  
- Další rozšířené možnosti  

Následující fragmenty znázorňují některé způsoby, jak lze použít konstruktory DbContext.  

## <a name="use-code-first-with-connection-by-convention"></a>Použití Code First s připojením podle konvence  

Pokud jste v aplikaci neudělali žádnou jinou konfiguraci, pak volání konstruktoru bez parametrů na DbContext způsobí, že DbContext se spustí v režimu Code First s připojením k databázi vytvořeným pomocí konvence. Příklad:  

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

V tomto příkladu DbContext používá kvalifikovaný název oboru názvů vaší odvozené třídy kontextu – demo. EF. BloggingContext – jako název databáze a vytváří připojovací řetězec pro tuto databázi pomocí SQL Express nebo LocalDB. Pokud jsou obě nainstalovány, bude použit SQL Express.  

Visual Studio 2010 obsahuje SQL Express ve výchozím nastavení a Visual Studio 2012 a novější zahrnuje LocalDB. Během instalace zkontroluje balíček NuGet EntityFramework, který databázový server je k dispozici. Balíček NuGet pak aktualizuje konfigurační soubor nastavením výchozího databázového serveru, který Code First používá při vytváření připojení podle konvence. Pokud je spuštěn SQL Express, bude použit. Pokud SQL Express není k dispozici, LocalDB se místo toho zaregistruje jako výchozí. V konfiguračním souboru nejsou provedeny žádné změny, pokud již obsahuje nastavení pro výchozí objekt pro vytváření připojení.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Použít Code First s připojením podle konvence a zadaného názvu databáze  

Pokud jste v aplikaci neudělali žádnou jinou konfiguraci, pak voláním konstruktoru String v DbContext s názvem databáze, který chcete použít, způsobí, že DbContext se spustí v režimu Code First s připojením k databázi vytvořeným konvencí do databáze nástroje. Tento název. Příklad:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

V tomto příkladu DbContext používá jako název databáze "BloggingDatabase" a vytvoří připojovací řetězec pro tuto databázi pomocí SQL Express (nainstalovaného se sadou Visual Studio 2010) nebo LocalDB (instaluje se s Visual Studiem 2012). Pokud jsou obě nainstalovány, bude použit SQL Express.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Použít Code First s připojovacím řetězcem v souboru App. config/Web. config  

Do souboru App. config nebo Web. config se můžete rozhodnout připojovací řetězec. Příklad:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Toto je jednoduchý způsob, jak říct, že DbContext používat databázový server jiný než SQL Express nebo LocalDB – výše uvedený příklad určuje databázi edice SQL Server Compact.  

Pokud se název připojovacího řetězce shoduje s názvem vašeho kontextu (buď s kvalifikací oboru názvů nebo bez něj), bude nalezen pomocí DbContext, pokud je použit konstruktor bez parametrů. Pokud se název připojovacího řetězce liší od názvu vašeho kontextu, pak můžete DbContext použít toto připojení v režimu Code First předáním názvu připojovacího řetězce konstruktoru DbContext. Příklad:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternativně můžete použít formát "název =\<připojovací řetězec název\>" pro řetězec předaný konstruktoru DbContext. Příklad:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Tento formulář vám umožní explicitně, abyste očekávali, že se připojovací řetězec nachází v konfiguračním souboru. Pokud nebyl nalezen připojovací řetězec se zadaným názvem, bude vyvolána výjimka.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Databáze/Model First s připojovacím řetězcem v souboru App. config/Web. config  

Modely vytvořené pomocí návrháře EF se liší od Code First v tom, že váš model již existuje a není generován z kódu při spuštění aplikace. Model obvykle existuje v projektu jako soubor EDMX.  

Návrhář přidá připojovací řetězec EF do souboru App. config nebo Web. config. Tento připojovací řetězec je speciální v tom, že obsahuje informace o tom, jak najít informace v souboru EDMX. Příklad:  

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

Návrhář EF také vygeneruje kód, který instruuje DbContext k použití tohoto připojení předáním názvu připojovacího řetězce konstruktoru DbContext. Příklad:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext ví, že načte existující model (místo použití Code First k jeho výpočtu z kódu), protože připojovací řetězec je připojovací řetězec EF, který obsahuje podrobnosti o modelu, který se má použít.  

## <a name="other-dbcontext-constructor-options"></a>Další možnosti konstruktoru DbContext  

Třída DbContext obsahuje další konstruktory a vzory použití, které umožňují několik pokročilejších scénářů. Některé z těchto akcí:  

- Třídu DbModelBuilder můžete použít k sestavení modelu Code First bez vytváření instancí instance DbContext. Výsledkem tohoto je objekt DbModel. Až budete připraveni vytvořit instanci DbContext, můžete tento objekt DbModel předat jednomu z konstruktorů DbContext.  
- Můžete předat úplný připojovací řetězec k DbContext místo pouze databáze nebo názvu připojovacího řetězce. Ve výchozím nastavení se tento připojovací řetězec používá společně se zprostředkovatelem System. data. SqlClient; To lze změnit nastavením jiné implementace IConnectionFactory na kontext. Database. DefaultConnectionFactory.  
- Existující objekt DbConnection můžete použít tak, že ho předáte konstruktoru DbContext. Pokud je objekt připojení instancí EntityConnection, použije se místo výpočtu modelu pomocí Code First model zadaný v připojení. Pokud je objekt instancí nějakého jiného typu, například SqlConnection, pak ho kontext použije pro režim Code First.  
- Můžete předat existující ObjectContext konstruktoru DbContext a vytvořit DbContext pro zabalení stávajícího kontextu. Tato možnost se dá použít pro existující aplikace, které používají ObjectContext, ale které chtějí využít DbContext v některých částech aplikace.  
