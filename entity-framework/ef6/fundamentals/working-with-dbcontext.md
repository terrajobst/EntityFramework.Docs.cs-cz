---
title: Práce s DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416320"
---
# <a name="working-with-dbcontext"></a>Práce s DbContext

Aby bylo možné použít Entity Framework k dotazování, vkládání, aktualizaci a odstraňování dat pomocí objektů .NET, je nejprve nutné [vytvořit model](~/ef6/modeling/index.md) , který mapuje entity a vztahy, které jsou definovány v modelu, na tabulky v databázi.

Jakmile máte model, primární třída, se kterou komunikuje vaše aplikace, je `System.Data.Entity.DbContext` (často se označuje jako třída kontextu). Pomocí DbContext přidruženého k modelu můžete:
- Zápis a spouštění dotazů   
- Vyhodnotit výsledků dotazu jako objekty entit
- Sledování změn provedených u těchto objektů
- Zachovat změny objektů zpátky v databázi
- Svázání objektů v paměti s ovládacími prvky uživatelského rozhraní

Tato stránka obsahuje pokyny k tomu, jak spravovat třídu kontextu.  

## <a name="defining-a-dbcontext-derived-class"></a>Definování odvozené třídy DbContext  

Doporučeným způsobem, jak pracovat s kontextem, je definovat třídu, která je odvozena z DbContext a zpřístupňuje vlastnosti Negenerickými, které reprezentují kolekce zadaných entit v kontextu. Pokud pracujete s návrhářem EF, bude pro vás generován kontext. Pokud pracujete se Code First, budete obvykle psát kontext sami.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Jakmile budete mít kontext, měli byste dotazovat na, přidat (pomocí `Add` nebo `Attach` metody) nebo odebrat entity (pomocí `Remove`) v kontextu prostřednictvím těchto vlastností. Přístup k vlastnosti `DbSet` u objektu Context představuje počáteční dotaz, který vrátí všechny entity zadaného typu. Všimněte si, že při pouhém přístupu k vlastnosti se dotaz nespustí. Dotaz se spustí, když:  

- Je vyhodnoceno pomocí příkazu `foreach` (C#) nebo `For Each` (Visual Basic).  
- Je vyhodnocena operací shromažďování, například `ToArray`, `ToDictionary`nebo `ToList`.  
- V krajní části dotazu jsou uvedeny operátory LINQ, jako je například `First` nebo `Any`.  
- Je volána jedna z následujících metod: metoda rozšíření `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`a `DbSet<T>.Find`, pokud entita se zadaným klíčem nebyla v kontextu již načtena.  

## <a name="lifetime"></a>Životnost  

Životnost kontextu začíná při vytvoření instance a končí, když je instance buď vyřazena, nebo uvolňována paměti. Použijte k **použití** , pokud chcete, aby všechny prostředky, které kontext ovládá, byly odstraněny na konci bloku. Při použití metody **using**kompilátor automaticky vytvoří blok try/finally a zavolá Dispose v bloku **finally** .  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Tady jsou některé obecné pokyny pro rozhodování o životnosti kontextu:  

- Při práci s webovými aplikacemi použijte kontextovou instanci na požadavek.  
- Při práci s Windows Presentation Foundation (WPF) nebo model Windows Forms použijte kontextovou instanci na formulář. To vám umožní používat funkce sledování změn, které poskytuje kontext.  
- Je-li instance kontextu vytvořena pomocí kontejneru pro vkládání závislostí, obvykle je odpovědností kontejneru, aby odstranil kontext.
- Pokud je kontext vytvořen v kódu aplikace, nezapomeňte odstranit kontext, pokud již není požadován.  
- Při práci s dlouhodobě běžícím kontextem zvažte následující:  
    - Při načítání více objektů a jejich odkazů do paměti se může velmi rychle zvýšit spotřeba paměti daného kontextu. To může způsobit problémy s výkonem.  
    - Kontext není bezpečný pro přístup z více vláken, proto by neměl být sdílen napříč více vlákny, na kterých pracuje souběžně.
    - Pokud výjimka způsobí, že je kontext v neobnovitelné stavu, může dojít k ukončení celé aplikace.  
    - Šance na to, aby se při potížích s souběžnou přenosem zvýšila velikost, se zvyšuje jako mezera mezi okamžikem, kdy se data dotazují a aktualizují.  

## <a name="connections"></a>Připojení  

Ve výchozím nastavení kontext spravuje připojení k databázi. Kontext v případě potřeby otevře a zavře připojení. Například kontext otevírá připojení pro spuštění dotazu a pak ukončí připojení při zpracování všech sad výsledků.  

Existují případy, kdy chcete mít větší kontrolu nad tím, kdy se připojení otevírá a zavírá. Například při práci s SQL Server Compact se často doporučuje udržovat samostatné otevřené připojení k databázi po dobu života aplikace za účelem zvýšení výkonu. Tento proces můžete spravovat ručně pomocí vlastnosti `Connection`.  
