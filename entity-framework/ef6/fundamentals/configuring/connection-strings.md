---
title: Připojovací řetězce a modelů – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: dce414ea84f13235691abf0dcadef5c743d90f9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994956"
---
# <a name="connection-strings-and-models"></a>Připojovací řetězce a modelů
Toto téma popisuje, jak Entity Framework zjistí připojení k databázi, a jak ho změnit. Oba modely vytvořené pomocí Code First a EF designeru jsou popsané v tomto tématu.  

Obvykle aplikace Entity Framework používá třídy odvozené od položky DbContext. Tato odvozená třída bude volat jeden z konstruktorů na základní třídy DbContext do ovládacího prvku:  

- Jak se kontext připojení k databázi – to znamená, jak připojovací řetězec se nenašel nebo používané  
- Zda kontextu použije vypočítat model s použitím Code First nebo načtení modelu vytvářené pomocí návrháře EF  
- Další pokročilé možnosti  

Následující fragmenty ukazují některé ze způsobů, jak konstruktory kontext databáze. je možné použít.  

## <a name="use-code-first-with-connection-by-convention"></a>Použití Code First s připojením podle konvence  

Pokud jste neudělali žádné další konfiguraci ve vaší aplikaci, následným voláním konstruktoru bez parametrů na DbContext způsobí DbContext spouštění v režimu Code First pomocí připojení k databázi vytvoření konvencí. Příklad:  

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

V tomto příkladu DbContext používá obor názvů kvalifikovaný název vaší odvozené kontextu class—Demo.EF.BloggingContext—as název databáze a vytváří připojovací řetězec pro tuto databázi pomocí SQL Express nebo LocalDB. Pokud jsou oba nainstalovány, SQL Express se použije.  

Visual Studio 2010 ve výchozím nastavení a Visual Studio 2012 obsahuje SQL Express a dále zahrnuje LocalDB. Během instalace balíčku EntityFramework NuGet zkontroluje, které databázový server je k dispozici. Balíček NuGet pak aktualizuje konfigurační soubor tak, že nastavíte výchozí databázový server, který Code First používá při vytváření připojení podle konvence. Pokud je spuštěn systém SQL Express, bude použit. Pokud není k dispozici SQL Express LocalDB se bude zapsán jako výchozí místo. Do konfiguračního souboru jsou provedeny žádné změny, pokud již obsahuje nastavení pro výchozí objekt factory připojení.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Použití Code First s připojením podle konvence a zadaný název databáze  

Pokud jste neudělali žádné další konfiguraci ve vaší aplikaci, pak volání konstruktoru řetězec na kontext databáze s názvem databáze, kterou chcete použít způsobí DbContext spouštění v režimu Code First pomocí připojení k databázi vytvořené pomocí konvence na databázi Tento název. Příklad:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

V tomto příkladu DbContext používá "BloggingDatabase" jako název databáze a vytváří připojovací řetězec pro tuto databázi pomocí SQL Express (nainstalovaný sadou Visual Studio 2010) nebo LocalDB (nainstalované s Visual Studio 2012). Pokud jsou oba nainstalovány, SQL Express se použije.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Použít s připojovacím řetězcem v souboru app.config/web.config Code First  

Můžete vložit připojovací řetězec v souboru app.config nebo web.config. Příklad:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Toto je snadný způsob, jak zjistit kontext databáze. Chcete-li použít databázový server, než systém SQL Express nebo LocalDB – výše uvedený příklad určuje databázi systému SQL Server Compact Edition.  

Pokud název připojovacího řetězce odpovídá názvu váš kontext (s nebo bez kvalifikace názvů) pak ji bude nalezen podle DbContext při použití konstruktor bez parametrů. Pokud název připojovacího řetězce se liší od názvu kontextu poznáte kontext databáze. Chcete použít toto připojení v režimu Code First předáním název připojovacího řetězce do konstruktoru DbContext. Příklad:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternativně můžete použít formuláře "name =\<název připojovacího řetězce\>" pro řetězec předaný konstruktoru DbContext. Příklad:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Tento formulář umožňuje explicitní očekáváte, že připojovací řetězec k nalezena v konfiguračním souboru. Bude vyvolána výjimka, pokud nebyl nalezen připojovací řetězec s daným názvem.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Databáze a Model první připojovacím řetězcem v souboru app.config/web.config  

Modely vytvořené pomocí EF designeru se liší od Code First, že váš model již existuje a není generován z kódu při spuštění aplikace. Model obvykle existuje jako soubor EDMX ve vašem projektu.  

Návrhář přidá připojovacího řetězce služby EF do souboru app.config nebo web.config. Tento připojovací řetězec je speciální, protože obsahuje informace o tom, jak najít informace v souboru EDMX. Příklad:  

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

EF designeru také vygeneruje kód, který dává pokyn kontext databáze. Chcete použít toto připojení tím, že předáte název připojovacího řetězce do konstruktoru DbContext. Příklad:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext věděl, může se načíst existující model (nikoli výpočet z kódu pomocí Code First) vzhledem k tomu, že připojovací řetězec je řetězec připojení EF obsahující podrobnosti o použití modelu.  

## <a name="other-dbcontext-constructor-options"></a>Další možnosti konstruktor DbContext  

Třídy DbContext obsahuje další konstruktory a vzorce používání, které umožňují některé pokročilejší scénáře. Zde jsou některé z těchto:  

- Třída DbModelBuilder můžete použít k sestavení modelu Code First bez vytvoření instance instanci DbContext. Výsledek tohoto objektu je objekt DbModel. Můžete předat tento objekt DbModel do jednoho z konstruktorů kontext databáze. když budete chtít vytvořit instanci DbContext.  
- Úplný připojovací řetězec můžete předat DbContext namísto pouze název řetězce databázi nebo připojení. Ve výchozím nastavení tento připojovací řetězec se používá zprostředkovatele System.Data.SqlClient; To můžete změnit tak, že nastavíte na různé implementace IConnectionFactory do kontextu. Database.DefaultConnectionFactory.  
- Můžete použít existující objekt DbConnection předáním konstruktoru DbContext. Pokud je objekt připojení instance EntityConnection, bude model zadané v připojení používané spíše než výpočtu pomocí modelu kódu nejprve. Pokud je objekt instancí jiný typ – například SqlConnection – pak kontextu bude používat pro režim Code First.  
- Existující objekt ObjectContext může předat DbContext konstruktor pro vytvoření DbContext zabalení existujícího kontextu. To lze použít pro existující aplikace, které používají objekt ObjectContext, ale které chtějí využít nabídky DbContext v některé části aplikace.  
