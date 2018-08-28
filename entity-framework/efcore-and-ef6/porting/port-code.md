---
title: Přenesení z EF6 do EF Core – přenesení modelu na úrovni kódu
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997046"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Přenesení modelu na základě kódu EF6 do EF Core

Pokud jste si přečetli všechna upozornění a jste připraveni k portu, pak tady jsou některé pokyny, které vám pomůžou začít.

## <a name="install-ef-core-nuget-packages"></a>Instalace balíčků EF Core NuGet

Pokud chcete použít EF Core, nainstalujte balíček NuGet pro poskytovatele databáze, kterou chcete použít. Například při cílení na systém SQL Server, nainstalujte `Microsoft.EntityFrameworkCore.SqlServer`. Zobrazit [poskytovatelé databází](../../core/providers/index.md) podrobnosti.

Pokud máte v úmyslu použít migrace, pak byste měli nainstalovat také `Microsoft.EntityFrameworkCore.Tools` balíčku.

Je v pořádku nechte instalaci balíčku EF6 NuGet (EntityFramework), jak EF Core a EF6 můžou být použité vedle sebe ve stejné aplikaci. Ale pokud nejsou máte v úmyslu použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže dojít k chybám kompilace na části kódu, který je potřeba věnovat pozornost.

## <a name="swap-namespaces"></a>Obory názvů prohození

Většina rozhraní API, které můžete použít EF6 jsou v `System.Data.Entity` obor názvů (a související sub obory názvů). První změny kódu, je do odkládacího souboru `Microsoft.EntityFrameworkCore` oboru názvů. By obvykle začínat odvozené kontextu souboru s kódem a pak vycházejí z něj, vyřešení chyb kompilace, když k nim dojde.

## <a name="context-configuration-connection-etc"></a>Konfigurace kontextu (připojení atd.)

Jak je popsáno v [zajistit EF Core bude práce pro aplikace](ensure-requirements.md), EF Core má méně magic kolem zjišťování pro připojení k databázi. Je potřeba přepsat `OnConfiguring` metoda v odvozené kontextu a použití rozhraní API konkrétní poskytovatele databáze nastavit připojení k databázi.

Většina aplikací EF6 uložit připojovací řetězec v aplikacích `App/Web.config` souboru. V EF Core čtete tento připojovací řetězec pomocí `ConfigurationManager` rozhraní API. Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní framework bude moct pomocí tohoto rozhraní API.

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

V tomto okamžiku je otázkou vyřešení chyb kompilace a revizi kódu zobrazíte, pokud změny chování ovlivní vás.

## <a name="existing-migrations"></a>Migrace existující

Ve skutečnosti není k dispozici vhodná způsob, jak přenést existující migrace EF6 do EF Core.

Pokud je to možné doporučujeme předpokládají, že se použily všechny předchozí přenesení z EF6 do databáze a pak zahájení migrace schématu od bodu, pomocí EF Core. Chcete-li to provést, použijte `Add-Migration` příkaz pro přidání migrace po modelu se přenáší na EF Core. Potom odeberte všechen kód z `Up` a `Down` metody vygenerované migrace. Následné migrace při porovnání modelu při této počáteční migrace se automaticky.

## <a name="test-the-port"></a>Port testu

To, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenést do EF Core. Je potřeba otestovat všechny oblasti vaší aplikace k zajištění, že žádné změny chování mít nepříznivý vliv na aplikace.
