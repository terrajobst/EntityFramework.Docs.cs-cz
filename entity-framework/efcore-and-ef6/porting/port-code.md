---
title: Přenos z EF6 do EF Core-portový model založený na kódu – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181219"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Přenos EF6 modelu založeného na kódu do EF Core

Pokud jste si přečetli všechna upozornění a jste připraveni na port, pak tady najdete pokyny, které vám pomůžou začít.

## <a name="install-ef-core-nuget-packages"></a>Nainstalovat EF Core balíčky NuGet

Chcete-li použít EF Core, nainstalujte balíček NuGet pro poskytovatele databáze, který chcete použít. Například při cílení na SQL Server byste měli nainstalovat `Microsoft.EntityFrameworkCore.SqlServer`. Podrobnosti najdete v tématu [poskytovatelé databáze](../../core/providers/index.md) .

Pokud plánujete použít migrace, měli byste také nainstalovat balíček `Microsoft.EntityFrameworkCore.Tools`.

Je také dobré opustit balíček NuGet EF6 (EntityFramework), protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci. Pokud ale nehodláte používat EF6 v žádné oblasti aplikace, pak se při odinstalaci balíčku pomůže Chyba kompilace v částech kódu, které vyžadují pozornost.

## <a name="swap-namespaces"></a>Prohození oborů názvů

Většina rozhraní API, která používáte v EF6, jsou v oboru názvů `System.Data.Entity` (a souvisejících dílčích oborech názvů). První změnou kódu je přepnutí na obor názvů `Microsoft.EntityFrameworkCore`. Obvykle byste začínaly s vaším odvozeným souborem kódu kontextu a pak tam dostanou problémy, které řeší chyby kompilace.

## <a name="context-configuration-connection-etc"></a>Konfigurace kontextu (připojení atd.)

Jak je popsáno v tématu [zajistěte, aby pro vaši aplikaci fungovala EF Core](ensure-requirements.md), EF Core má méně Magic k detekci databáze, ke které se chcete připojit. V odvozeném kontextu bude nutné přepsat metodu `OnConfiguring` a nastavit připojení k databázi pomocí rozhraní API pro konkrétního poskytovatele databáze.

Většina EF6ch aplikací ukládá připojovací řetězec do souborů aplikace `App/Web.config`. V EF Core si tento připojovací řetězec načetli pomocí rozhraní API `ConfigurationManager`. Aby bylo možné používat toto rozhraní API, možná budete muset přidat odkaz na sestavení rozhraní `System.Configuration`.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Aktualizace kódu

V tomto okamžiku se jedná o adresování chyb kompilace a revizi kódu, abyste viděli, zda vás ovlivní změny chování.

## <a name="existing-migrations"></a>Existující migrace

Neexistuje žádný vhodný způsob, jak přenést existující migrace EF6 do EF Core.

Je-li to možné, je vhodné předpokládat, že všechny předchozí migrace z EF6 byly použity v databázi, a pak začít migrovat schéma z tohoto bodu pomocí EF Core. K tomuto účelu byste použili příkaz `Add-Migration` k přidání migrace, jakmile je model předaný do EF Core. Veškerý kód byste pak odebrali z metod `Up` a `Down` v rámci vygenerované migrace. Další migrace se budou porovnávat s modelem, když se tato počáteční migrace vygenerovala z uživatelského rozhraní.

## <a name="test-the-port"></a>Otestování portu

Vzhledem k tomu, že se vaše aplikace zkompiluje, neznamená to, že je úspěšně přepravovaná na EF Core. Budete muset otestovat všechny oblasti aplikace, aby se zajistilo, že žádná z změn chování nemá negativně vliv na vaši aplikaci.
