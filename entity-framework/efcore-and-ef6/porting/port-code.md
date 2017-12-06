---
title: "Portování ze EF6 na jádro EF – portování Model založené na kódu"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portování Model založené na kódu EF6 na jádro EF

Pokud jste si přečetli všechna upozornění a jste připraveni k portu, potom tady jsou některé pokyny, které vám pomůže začít.

## <a name="install-ef-core-nuget-packages"></a>Instalace balíčků NuGet EF jádra

Pokud chcete použít EF jádra, nainstalujte balíček NuGet pro zprostředkovatele databáze, kterou chcete použít. Například pokud je cílem systém SQL Server, by instalaci `Microsoft.EntityFrameworkCore.SqlServer`. V tématu [zprostředkovatelů databáze](../../core/providers/index.md) podrobnosti.

Pokud máte v úmyslu použít migrace, pak musíte taky nainstalovat `Microsoft.EntityFrameworkCore.Tools` balíčku.

Je v pořádku nechte balíček EF6 NuGet (EntityFramework) nainstalované, tak, jak EF jádra a EF6 můžou být použité-souběžného ve stejné aplikaci. Ale pokud nejsou hodláte použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže poskytnout chyby při kompilaci na části kódu, které vyžadují pozornost.

## <a name="swap-namespaces"></a>Swap – obory názvů

Většina rozhraní API, které používáte ve EF6 jsou v `System.Data.Entity` obor názvů (a související dílčí-obory názvů). První změna kódu je do odkládacího souboru `Microsoft.EntityFrameworkCore` oboru názvů. By obvykle začínat souboru kódu odvozené kontextu a pak vycházejí z ní, vyřešte chyby kompilace, kdy k nim dojde.

## <a name="context-configuration-connection-etc"></a>Kontext konfigurace (připojení atd.)

Jak je popsáno v [zkontrolujte EF základní bude práci pro vaše aplikace](ensure-requirements.md), EF základní má méně magic kolem zjišťování pro připojení k databázi. Je nutné přepsat `OnConfiguring` metoda v odvozené kontextu a použití rozhraní API konkrétní zprostředkovatele databáze k nastavení připojení k databázi.

Většina aplikací EF6 připojovací řetězec uložit v aplikacích `App/Web.config` souboru. V EF Core, číst, tento připojovací řetězec pomocí `ConfigurationManager` rozhraní API. Budete muset přidat odkaz na `System.Configuration` sestavení rozhraní, abyste mohli používat toto rozhraní API.

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

V tomto okamžiku je řádu adresování chyby při kompilaci a kontrola kódu, které chcete zobrazit, pokud změny chování ovlivní můžete.

## <a name="existing-migrations"></a>Existující migrace

Ve skutečnosti není k dispozici vhodný způsob k portu existující EF6 migrací EF jádra.

Pokud je to možné je nejvhodnější předpokládají, že byly použity všechny předchozí migrace z EF6 k databázi a potom spustit migraci schématu z tohoto bodu pomocí EF jádra. Chcete-li to provést, použijte `Add-Migration` příkaz pro přidání migrace po modelu je přesně do EF jádra. By pak odeberte všechny kód z `Up` a `Down` metody vygenerované migrace. Následné migrace se porovná do modelu, při které počáteční migrace se vygeneroval.

## <a name="test-the-port"></a>Testování port

Právě, protože aplikace zkompiluje, neznamená, že je úspěšně přesně do EF jádra. Musíte se k testování všech oblastí aplikace k zajištění, že žádná ze změn chování negativní dopad vaší aplikace.
