---
title: Přenesení z EF6 do EF Core – přenesení modelu na bázi EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997408"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Přenesení modelu na bázi EDMX EF6 do EF Core

EF Core nepodporuje formát souboru EDMX pro modely. Nejlepší možností k portu tyto modely, je nový model založený na kódu Generovat z databáze pro vaši aplikaci.

## <a name="install-ef-core-nuget-packages"></a>Instalace balíčků EF Core NuGet

Nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček NuGet.

## <a name="regenerate-the-model"></a>Znovu generovat model

Funkce zpětné analýzy nyní můžete vytvořit model založený na existující databázi.

Spuštěním následujícího příkazu v konzole Správce balíčků (Nástroje –> Správce balíčků NuGet –> Konzola správce balíčků). Zobrazit [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) pro parametry příkazu generovat uživatelské rozhraní pro podmnožinu tabulek atd.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Například tady je příkaz scaffold modelu z blogovací databáze na instanci serveru SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Odebrat EF6 model

EF6 model by teď odebrat z vaší aplikace.

Je v pořádku nechte instalaci balíčku EF6 NuGet (EntityFramework), jak EF Core a EF6 můžou být použité vedle sebe ve stejné aplikaci. Ale pokud nejsou máte v úmyslu použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže dojít k chybám kompilace na části kódu, který je potřeba věnovat pozornost.

## <a name="update-your-code"></a>Aktualizace kódu

V tomto okamžiku je otázkou adresování chyby při kompilaci a revize kódu a zjistěte, jestli změny chování mezi EF6 a EF Core ovlivní.

## <a name="test-the-port"></a>Port testu

To, že vaše aplikace zkompiluje, neznamená, že je úspěšně přenést do EF Core. Je potřeba otestovat všechny oblasti vaší aplikace k zajištění, že žádné změny chování mít nepříznivý vliv na aplikace.

> [!TIP]
> Zobrazit [Začínáme s EF Core v ASP.NET Core s existující databázi](xref:core/get-started/aspnetcore/existing-db) Další informace o tom, jak pracovat s existující databázi, 
