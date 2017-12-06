---
title: "Konzola správce balíčků (Visual Studio) – EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
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
| ------------------------- | --------------------------- |
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

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| ***-Název*** \<řetězec >              | Název migrace.                                                      |
| <nobr>-OutputDir \<řetězec ></nobr>  | Adresář (a podklíčů – obor názvů) používat. Cesty se vztahují k adresáři projektu. Výchozí hodnota je "Migrace". |

> [!NOTE]
> Parametry v **tučné** jsou povinné a ty, které v *italics* jsou poziční.

### <a name="drop-database"></a>Vyřaďte databázi

Zahodí databáze.

Parametry:

|          |                                                          |
| -------- | -------------------------------------------------------- |
| -WhatIf  | Zobrazit databázi, kterou by být vyřazen, ale nemáte vyřadit. |

### <a name="get-dbcontext"></a>Get-DbContext

Získá informace o typu DbContext.

### <a name="remove-migration"></a>Odebrat migrace

Odebere poslední migrace.

Parametry:

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| -Force | Nekontrolovat Pokud chcete zobrazit, pokud se migrace použil k databázi. |

### <a name="scaffold-dbcontext"></a>DbContext vygenerované uživatelské rozhraní

Scaffold a typy DbContext a entity pro databázi.

Parametry:

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <nobr>***-Připojení*** \<řetězec ></nobr> | Připojovací řetězec k databázi.                                    |
| ***-Poskytovatel*** \<řetězec >                | Zprostředkovatel, který se má používat. (Např.) Microsoft.EntityFrameworkCore.SqlServer)       |
| -OutputDir \<řetězec >                     | Adresář, který chcete umístit soubory do. Cesty se vztahují k adresáři projektu. |
| -Kontext- \<řetězec >                       | Název DbContext ke generování.                                    |
| -Schémata \<řetězec [] >                     | Schémata tabulkami k vygenerování typy entit pro.                       |
| -Tabulky \<řetězec [] >                      | S tabulkami k vygenerování typy entit pro.                                  |
| -DataAnnotations                         | Pomocí atributů (kde je to možné), nakonfigurujte model. Pokud tento parametr vynechán, použije se rozhraní fluent API. |
| -UseDatabaseNames                        | Používejte názvy tabulek a sloupců přímo z databáze.                    |
| -Force                                   | Přepište existující soubory.                                                 |

### <a name="script-migration"></a>Skript migrace

Generuje skriptu SQL z migrace.

Parametry:

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| *-Z* \<řetězec > | Počáteční migrace. Výchozí hodnota je 0 (počáteční databáze).      |
| *-Na* \<řetězec >   | Koncová migrace. Výchozí hodnoty na poslední migrace.              |
| -Idempotent       | Generovat skript, který lze použít v databázi v žádné migrace. |
| -Výstup \<řetězec > | Soubor k zápisu výsledek, který má.                                   |

> [!TIP]
> Komu, z, a podporují karta rozšíření výstupní parametry.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <nobr>*-Migrace* \<řetězec ></nobr> | Cílová migrace. Pokud je to "0", budou vráceny všechny migrace. Výchozí hodnoty na poslední migrace. |

> [!TIP]
> Parametr migrace podporuje karta rozšíření.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
