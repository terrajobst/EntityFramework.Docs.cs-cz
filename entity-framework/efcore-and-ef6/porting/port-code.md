---
title: Přenos z EF6 na ef core – portování modelu založeného na kódu – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419635"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Přenesení modelu ef6 založeného na kódu na základní EF

Pokud jste si přečetli všechny upozornění a jste připraveni k portu, pak zde je několik pokynů, které vám pomohou začít.

## <a name="install-ef-core-nuget-packages"></a>Instalace balíčků EF Core NuGet

Chcete-li použít EF Core, nainstalujete balíček NuGet pro poskytovatele databáze, který chcete použít. Například při cílení sql server, `Microsoft.EntityFrameworkCore.SqlServer`byste nainstalovat . Podrobnosti najdete v [tématu Zprostředkovatelé databáze.](../../core/providers/index.md)

Pokud plánujete používat migrace, měli byste také `Microsoft.EntityFrameworkCore.Tools` nainstalovat balíček.

Je v pořádku ponechat balíček EF6 NuGet (EntityFramework) nainstalovaný, protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci. Pokud však nemáte v úmyslu používat EF6 v žádné oblasti aplikace, pak odinstalace balíčku pomůže poskytnout chyby kompilace na části kódu, které vyžadují pozornost.

## <a name="swap-namespaces"></a>Zaměnit obory názvů

Většina api, které používáte v `System.Data.Entity` EF6 jsou v oboru názvů (a související podobory). První změna kódu je přepnutí do oboru `Microsoft.EntityFrameworkCore` názvů. Obvykle byste začít s odvozené matné kód souboru a pak pracovat z toho, adresování chybkompilace, jak k nim dochází.

## <a name="context-configuration-connection-etc"></a>Kontextová konfigurace (připojení atd.)

Jak je popsáno v [části Ujistěte se, že EF Core bude fungovat pro vaši aplikaci](ensure-requirements.md), EF Core má méně magie kolem zjišťování databáze pro připojení. Budete muset přepsat metodu v `OnConfiguring` odvozeném kontextu a použít rozhraní API specifické pro poskytovatele databáze k nastavení připojení k databázi.

Většina aplikací EF6 ukládá připojovací řetězec do souboru aplikace. `App/Web.config` V EF Core, načtete `ConfigurationManager` tento připojovací řetězec pomocí rozhraní API. Možná budete muset přidat odkaz `System.Configuration` na sestavení rozhraní, abyste mohli používat toto rozhraní API.

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

V tomto okamžiku je to otázka adresování chyb kompilace a revize kódu, abyste zjistili, zda změny chování ovlivní vás.

## <a name="existing-migrations"></a>Stávající migrace

Neexistuje žádný skutečně proveditelný způsob, jak portovat stávající ef6 migrace ef6 na EF Core.

Pokud je to možné, je nejlepší předpokládat, že všechny předchozí migrace z EF6 byly použity do databáze a potom začít migraci schématu z tohoto bodu pomocí EF Core. Chcete-li to provést, `Add-Migration` pomocí příkazu přidat migraci, jakmile je model portován na EF Core. Potom by odebrat veškerý `Up` `Down` kód z a metody generování šavle migrace. Následné migrace budou porovnány s modelem, když byla počáteční migrace přeložena.

## <a name="test-the-port"></a>Testování portu

Jen proto, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenesena na EF Core. Budete muset otestovat všechny oblasti aplikace, abyste zajistili, že žádná ze změn chování nemá nepříznivý vliv na vaši aplikaci.
