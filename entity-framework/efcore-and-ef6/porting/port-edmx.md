---
title: Přenos z EF6 na EF Core – portování modelu založeného na EDMX – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416921"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Přenesení modelu založeného na EF6 EDMX na ef core

EF Core nepodporuje formát souboru EDMX pro modely. Nejlepší možností pro přenos těchto modelů je generovat nový model založený na kódu z databáze pro vaši aplikaci.

## <a name="install-ef-core-nuget-packages"></a>Instalace balíčků EF Core NuGet

Nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček NuGet.

## <a name="regenerate-the-model"></a>Regenerovat model

Nyní můžete použít funkci zpětné analýzy k vytvoření modelu na základě existující databáze.

Spusťte následující příkaz v konzole Správce balíčků (Nástroje –> Správce balíčků NuGet –> konzoli správce balíčků). Viz [Console Správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pro možnosti příkazu pro vytvoření uživatelského okna podmnožinu tabulek atd.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Například zde je příkaz k generování uživatelského terminálu modelu z databáze blogování na instanci SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Odebrat model EF6

Nyní byste odebrat model EF6 z aplikace.

Je v pořádku ponechat balíček EF6 NuGet (EntityFramework) nainstalovaný, protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci. Pokud však nemáte v úmyslu používat EF6 v žádné oblasti aplikace, pak odinstalace balíčku pomůže poskytnout chyby kompilace na části kódu, které vyžadují pozornost.

## <a name="update-your-code"></a>Aktualizace kódu

V tomto okamžiku je to otázka adresování chyb kompilace a revize kódu, abyste zjistili, zda se změní chování mezi EF6 a EF Core.

## <a name="test-the-port"></a>Testování portu

Jen proto, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenesena na EF Core. Budete muset otestovat všechny oblasti aplikace, abyste zajistili, že žádná ze změn chování nemá nepříznivý vliv na vaši aplikaci.
