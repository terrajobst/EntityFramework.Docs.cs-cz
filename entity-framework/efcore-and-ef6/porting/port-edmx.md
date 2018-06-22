---
title: Portování ze EF6 na jádro EF – portování Model na základě EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812687"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portování Model na základě EDMX EF6 na jádro EF

Základní EF nepodporuje formát souboru EDMX pro modely. Nejlepší možnost k portu tyto modely je chcete generovat nový model založené na kódu z databáze pro vaši aplikaci.

## <a name="install-ef-core-nuget-packages"></a>Instalace balíčků NuGet EF jádra

Nainstalujte `Microsoft.EntityFrameworkCore.Tools` balíček NuGet.

## <a name="regenerate-the-model"></a>Znovu vygenerovat modelu

Nyní můžete funkci zpětné analýzy pro vytvoření modelu na základě existující databáze.

Spusťte následující příkaz v konzole Správce balíčků (Nástroje –> Správce balíčků NuGet –> Konzola správce balíčků). V tématu [Konzola správce balíčků (Visual Studio)](../../core/miscellaneous/cli/powershell.md) možnosti příkazu k automatickému vygenerování podmnožinu tabulek atd.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Zde je třeba příkaz chcete vygenerovat model z databáze blogu ve vaší instanci SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Odebrat EF6 modelu

EF6 model by nyní odebrat z vaší aplikace.

Je v pořádku nechte balíček EF6 NuGet (EntityFramework) nainstalované, tak, jak EF jádra a EF6 můžou být použité-souběžného ve stejné aplikaci. Ale pokud nejsou hodláte použít EF6 v všech oblastech vaší aplikace, pak odinstalování balíčku vám pomůže poskytnout chyby při kompilaci na části kódu, které vyžadují pozornost.

## <a name="update-your-code"></a>Aktualizace kódu

V tomto okamžiku bude stačit adresování chyby při kompilaci a prostudovali kód pro případ, že změny chování mezi EF6 a EF základní ovlivní.

## <a name="test-the-port"></a>Testování port

Právě, protože aplikace zkompiluje, neznamená, že je úspěšně přesně do EF jádra. Musíte se k testování všech oblastí aplikace k zajištění, že žádná ze změn chování negativní dopad vaší aplikace.

> [!TIP]
> V tématu [Začínáme s EF základní na ASP.NET Core s existující databázi](xref:core/get-started/aspnetcore/existing-db) Další informace o tom, jak pracovat s existující databázi, 
