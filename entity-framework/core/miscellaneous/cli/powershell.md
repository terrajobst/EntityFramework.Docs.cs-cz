---
title: Konzola správce balíčků (Visual Studio) – EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812557"
---
<a name="ef-core-package-manager-console-tools"></a>Nástroje konzoly Správce balíčků EF jádra
=====================================
Nástroje EF základní balíček správce konzoly (pomocí PMC) spustit v aplikaci Visual Studio pomocí NuGet [Konzola správce balíčků][2].
Tyto nástroje pro práci s projekty rozhraní .NET Framework a .NET Core.

> [!TIP]
> Není pomocí sady Visual Studio? [Nástroje příkazového řádku základní EF] [ 1] jsou napříč platformami a spusťte v příkazovém řádku.

<a name="installing-the-tools"></a>Instalace nástrojů
--------------------
Instalace balíčku Microsoft.EntityFrameworkCore.Tools NuGet nainstalujte nástroje konzoly Správce balíčků EF jádra.
Můžete ho nainstalovat spuštěním následujícího příkazu uvnitř [Konzola správce balíčků][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Pokud všechno fungovala správně, měli byste mít moci spustit tento příkaz:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Pokud je cílem vašeho projektu spuštění .NET Standard [cross cíl podporovaných prostředí] [ 3] před použitím nástroje.

> [!IMPORTANT]
> Pokud používáte **Universal Windows** nebo **Xamarin**, přesuňte EF kódu do .NET standardní knihovny tříd a [cross cíl podporovaných prostředí] [ 3] před použitím nástroje. Zadejte knihovny tříd jako spouštěný projekt.

<a name="using-the-tools"></a>Pomocí nástrojů
---------------
Při každém vyvolání příkazu, existují dva projekty související se situací:

Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat). Použije se výchozí hodnota cílový projekt **výchozí projekt** vybrali v konzole Správce balíčků, ale můžete také zadat pomocí parametru - projektu.

Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu. Výchozí hodnota jeden **nastavit jako spouštěný projekt** v Průzkumníku řešení. Je také lze pomocí parametru - StartupProject.

Společné parametry:

|                           |                             |
|:--------------------------|:----------------------------|
| -Kontext- \<řetězec >        | DbContext používat.       |
| -Projektu \<řetězec >        | Projekt, který používá.         |
| -StartupProject \<řetězec > | Spuštění projektu pro použití. |
| -Verbose                  | Zobrazit podrobný výstup.        |

Pokud chcete zobrazit nápovědu informace o příkazu, pomocí prostředí PowerShell na `Get-Help` příkaz.

> [!TIP]
> Parametry kontextu, projekt a StartupProject podporovat karta rozšíření.

> [!TIP]
> Nastavit **env:ASPNETCORE_ENVIRONMENT** před spuštěním k určení prostředí ASP.NET Core.

<a name="commands"></a>Příkazy
--------

### <a name="add-migration"></a>Přidat migrace

Přidá nové migrace.

Parametry:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Název*** \<řetězec >             | Název migrace.                                                                                       |
| <nobr>-OutputDir \<řetězec ></nobr> | Adresář (a podklíčů – obor názvů) používat. Cesty se vztahují k adresáři projektu. Výchozí hodnota je "Migrace". |

> [!NOTE]
> Parametry v **tučné** jsou povinné a ty, které v *italics* jsou poziční.

### <a name="drop-database"></a>Vyřaďte databázi

Zahodí databáze.

Parametry:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit. |

### <a name="get-dbcontext"></a>Get-DbContext

Získá informace o typu DbContext.

### <a name="remove-migration"></a>Odebrat migrace

Odebere poslední migrace.

Parametry:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Migrace vrátit, pokud nebyla použita k databázi. |

### <a name="scaffold-dbcontext"></a>DbContext vygenerované uživatelské rozhraní

Scaffold a typy DbContext a entity pro databázi.

Parametry:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Připojení*** \<řetězec ></nobr> | Připojovací řetězec k databázi.                                                           |
| ***-Poskytovatel*** \<řetězec >                | Zprostředkovatel, který se má používat. (Např.) Microsoft.EntityFrameworkCore.SqlServer)                              |
| -OutputDir \<řetězec >                     | Adresář, který chcete umístit soubory do. Cesty se vztahují k adresáři projektu.                      |
| -ContextDir \<řetězec >                    | Adresář, který chcete umístit do souboru DbContext. Cesty se vztahují k adresáři projektu.             |
| -Kontext- \<řetězec >                       | Název DbContext ke generování.                                                           |
| -Schémata \<řetězec [] >                     | Schémata tabulkami k vygenerování typy entit pro.                                              |
| -Tabulky \<řetězec [] >                      | S tabulkami k vygenerování typy entit pro.                                                         |
| -DataAnnotations                         | Pomocí atributů (kde je to možné), nakonfigurujte model. Pokud tento parametr vynechán, použije se rozhraní fluent API. |
| -UseDatabaseNames                        | Používejte názvy tabulek a sloupců přímo z databáze.                                           |
| -Force                                   | Přepište existující soubory.                                                                        |

### <a name="script-migration"></a>Skript migrace

Generuje skriptu SQL z migrace.

Parametry:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-Z* \<řetězec > | Počáteční migrace. Výchozí hodnota je 0 (počáteční databáze).      |
| *-Na* \<řetězec >   | Koncová migrace. Výchozí hodnoty na poslední migrace.              |
| -Idempotent       | Generovat skript, který lze použít v databázi v žádné migrace. |
| -Výstup \<řetězec > | Soubor k zápisu výsledek, který má.                                   |

> [!TIP]
> Komu, z, a podporují karta rozšíření výstupní parametry.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migrace* \<řetězec ></nobr> | Cílová migrace. Pokud je to "0", budou vráceny všechny migrace. Výchozí hodnoty na poslední migrace. |

> [!TIP]
> Parametr migrace podporuje karta rozšíření.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
