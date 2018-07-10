---
title: Práce s DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912046"
---
# <a name="working-with-dbcontext"></a>Práce s DbContext

Pokud chcete používat rozhraní Entity Framework k dotazování, vkládání, aktualizace a odstranění dat pomocí objektů .NET, musíte nejprve [vytvořit Model](~/ef6/modeling/index.md) který mapuje entit a vztahů, které jsou definovány v modelu s tabulkami v databázi.

Jakmile model aplikace komunikuje s hlavní třídou je `System.Data.Entity.DbContext` (často označované jako třídy kontextu). Můžete použít DbContext přidružené k modelu:
- Zápis a spouštění dotazů   
- Sloučit výsledky dotazu jako objekty entity
- Sledovat změny provedené na tyto objekty
- Zachovat změny objektu zpět na databázi
- Vytvoření vazby objektů v paměti pro ovládací prvky uživatelského rozhraní

Tato stránka poskytuje pokyny o tom, jak spravovat třídy kontextu.  

## <a name="defining-a-dbcontext-derived-class"></a>Definování DbContext odvozené třídy  

Doporučeným způsobem, jak pracovat s kontextem je definovat třídu, která je odvozena od položky DbContext a zpřístupní vlastnosti DbSet, které představují kolekce zadané entity v kontextu. Pokud pracujete s EF designeru, vygeneruje se pro vás kontextu. Pokud pracujete s Code First, obvykle napíšete kontextu sami.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Jakmile budete mít kontext, by pro dotazování, přidejte (pomocí `Add` nebo `Attach` metody) nebo odstranění (pomocí `Remove`) entity v kontextu prostřednictvím těchto vlastností. Přístup k `DbSet` vlastnost u objektu context představují výchozí dotaz, který vrátí všechny entity zadaného typu. Mějte na paměti, stačí přístupem k vlastnosti nebude provádět dotaz. Dotaz je proveden při:  

- Ve výčtu `foreach` (C#) nebo `For Each` – příkaz (Visual Basic).  
- Kolekce operací je uveden jako `ToArray`, `ToDictionary`, nebo `ToList`.  
- Operátory LINQ, jako například `First` nebo `Any` jsou určené v nejkrajnější část dotazu.  
- Jeden z následujících metod volá: `Load` rozšiřující metoda `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, a `DbSet<T>.Find`, pokud zadaný klíč nebyl nalezen již načteny v kontextu.  

## <a name="lifetime"></a>Doba platnosti  

Doba platnosti kontextu začíná, když instance je vytvořena a končí, když je odstraněn nebo uvolňování instance. Použití **pomocí** Pokud chcete, aby všechny prostředky, které řídí kontextu má být na konci bloku. Při použití **pomocí**, kompilátor automaticky vytvoří blok try/finally a volá funkci dispose v **nakonec** bloku.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Tady jsou některé obecné pokyny při rozhodování o životnost kontextu:  

- Při práci s webovými aplikacemi, používala kontextu instance pro každý požadavek.  
- Při práci s Windows Presentation Foundation (WPF) nebo formulářů Windows, pomocí místní instance na formulář. Díky tomu můžete použít funkci řešení change tracking poskytuje daného kontextu.  
- Pokud tato instance kontextu vytvoří kontejner vkládání závislostí, je obvykle odpovědnost kontejneru k uvolnění kontextu.
- Pokud kontextu se vytvoří v kódu aplikace, nezapomeňte k uvolnění kontextu, pokud se už nevyžaduje.  
- Při práci s kontextem dlouhotrvající zvažte následující:  
    - Jak načíst další objekty a jejich odkazů do paměti, může rychle zvýšit spotřebu paměti kontextu. To může způsobit problémy s výkonem.  
    - Kontext není bezpečná pro vlákno, proto by neměl být sdíleny napříč více vláken současně provádějící práce na něm.
    - Pokud výjimka způsobí, že se kontext, který má být ve stavu Neopravitelná, může ukončit celou aplikaci.  
    - Pravděpodobnost, že narazíte na problémy s souběžnosti zvýšení nárůstu mezery mezi dobou, kdy se data dotazovat a aktualizovat.  

## <a name="connections"></a>Připojení  

Ve výchozím kontextu spravuje připojení k databázi. Kontext se otevře a zavře připojení podle potřeby. Například kontextu otevře připojení k provedení dotazu a potom jej zavře připojení, mají po zpracování všech sadách výsledků dotazu.  

Existují případy, kdy chcete mít větší kontrolu nad po připojení k otevření a zavření. Například při práci s SQL serverem Compact, často doporučujeme udržovat samostatnou otevřít připojení k databázi po dobu životnosti aplikace pro zvýšení výkonu. Tento proces můžete spravovat ručně pomocí `Connection` vlastnost.  
