---
title: Přenos z EF6 do EF Core-portový model založený na EDMX – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416921"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Přenos EF6 modelu založeného na EDMX do EF Core

EF Core nepodporuje formát souboru EDMX pro modely. Nejlepší možností pro portování těchto modelů je vygenerování nového modelu založeného na kódu z databáze pro vaši aplikaci.

## <a name="install-ef-core-nuget-packages"></a>Nainstalovat EF Core balíčky NuGet

Nainstalujte balíček NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Znovu vygenerovat model

Nyní můžete pomocí funkce zpětná analýza vytvořit model založený na vaší stávající databázi.

Spusťte následující příkaz v konzole správce balíčků (nástroje – > Správce balíčků NuGet – > konzolu Správce balíčků). V tématu [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) jsou uvedeny možnosti příkazu k vytvoření uživatelského rozhraní podmnožiny tabulek atd.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Tady je příklad příkazu pro vytvoření uživatelského rozhraní modelu z blogovací databáze na instanci SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Odebrat model EF6

Nyní byste z aplikace odebrali EF6 model.

Je také dobré opustit balíček NuGet EF6 (EntityFramework), protože EF Core a EF6 lze použít vedle sebe ve stejné aplikaci. Pokud ale nehodláte používat EF6 v žádné oblasti aplikace, pak se při odinstalaci balíčku pomůže Chyba kompilace v částech kódu, které vyžadují pozornost.

## <a name="update-your-code"></a>Aktualizace kódu

V tomto okamžiku se jedná o řešení chyb při kompilaci a revizi kódu, abyste viděli, jestli se to bude mít vliv na změny chování mezi EF6 a EF Core.

## <a name="test-the-port"></a>Otestování portu

Vzhledem k tomu, že se vaše aplikace zkompiluje, neznamená to, že je úspěšně přepravovaná na EF Core. Budete muset otestovat všechny oblasti aplikace, aby se zajistilo, že žádná z změn chování nemá negativně vliv na vaši aplikaci.
