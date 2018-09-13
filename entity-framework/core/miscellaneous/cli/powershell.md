---
title: Konzola správce balíčků (Visual Studio) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490847"
---
<a name="ef-core-package-manager-console-tools"></a>EF Core Package Manageru konzoly nástroje
=====================================
Nástroje EF Core Package Manageru konzoly (PMC) spuštění v aplikaci Visual Studio pomocí Nugetu [Konzola správce balíčků][2].
Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.

> [!TIP]
> Bez použití sady Visual Studio? [Nástroje příkazového řádku EF Core] [ 1] jsou různé platformy a spusťte v příkazovém řádku.

<a name="installing-the-tools"></a>Instalace nástrojů
--------------------
Instaluje se balíček Microsoft.EntityFrameworkCore.Tools NuGet nainstalujte nástroje konzoly Správce balíčků EF Core.
Můžete jej nainstalovat spuštěním následujícího příkazu v [Konzola správce balíčků][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Pokud vše funguje správně, je třeba spustit tento příkaz:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Pokud váš spouštěný projekt cílí na .NET Standard [zacílení podporované architektury] [ 3] před použitím nástroje.

> [!IMPORTANT]
> Pokud používáte **Universal Windows** nebo **Xamarin**, přesuňte váš kód EF do knihovny tříd .NET Standard a [zacílení podporované architektury] [ 3] před použitím nástroje. Zadejte jako spouštěný projekt knihovny tříd.

<a name="using-the-tools"></a>Pomocí nástrojů
---------------
Při každém vyvolání příkazu, existují dva projekty zahrnuté:

Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána). Výchozí nastavení na cílový projekt **výchozí projekt** vybraný v konzole Správce balíčků, ale můžete také zadat pomocí parametru - projektu.

Projekt po spuštění je emulován nástroji při spuštění kódu projektu. Použije se výchozí jeden **nastavit jako spouštěný projekt** v Průzkumníku řešení. Lze také určit pomocí parametru - výchozí projekt.

Společné parametry:

|                           |                             |
|:--------------------------|:----------------------------|
| -Kontextu \<řetězec >        | Chcete-li použít uvolněn objekt DbContext.       |
| -Projektu \<řetězec >        | Projekt pro použití.         |
| – Výchozí projekt \<řetězec > | Použít spouštěný projekt. |
| -Verbose                  | Zobrazit podrobný výstup.        |

Chcete-li zobrazit nápovědu informace o příkazu, pomocí Powershellu `Get-Help` příkazu.

> [!TIP]
> Parametry kontextu, projektu a výchozí projekt podporují rozšíření tab.

> [!TIP]
> Nastavte **env:ASPNETCORE_ENVIRONMENT** před spuštěním zadat prostředí ASP.NET Core.

<a name="commands"></a>Příkazy
--------

### <a name="add-migration"></a>Přidat migrace

Přidá novou migraci.

Parametry:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Název*** \<řetězec >             | Název migrace.                                                                                       |
| <nobr>-OutputDir \<řetězec ></nobr> | Adresáři (a v podřízeném oboru názvů) používat. Cesty jsou relativní vzhledem k adresáři projektu. Výchozí hodnota je "Migrace". |

> [!NOTE]
> Parametry v **tučné** jsou povinné a těch, které jsou v *Kurzíva* jsou poziční.

### <a name="drop-database"></a>Odstranění databáze

Zahodí databáze.

Parametry:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Zobrazit, které databáze bude vyřazena, ale ji. |

### <a name="get-dbcontext"></a>Get-DbContext

Získá informace o typu DbContext.

### <a name="remove-migration"></a>Remove migrace

Odebere poslední migrace.

Parametry:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Vrácení migrace, pokud byl použit k databázi. |

### <a name="scaffold-dbcontext"></a>Vygenerované uživatelské rozhraní DbContext

Nástroj scaffold kontext databáze a entity typy pro databázi.

Parametry:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Connection*** \<řetězec ></nobr> | Připojovací řetězec k databázi.                                                           |
| ***-Poskytovatel*** \<řetězec >                | Zprostředkovatel k použití. (například Microsoft.EntityFrameworkCore.SqlServer)                      |
| -OutputDir \<řetězec >                     | Adresář, který se má vložit soubory. Cesty jsou relativní vzhledem k adresáři projektu.                      |
| -ContextDir \<řetězec >                    | Adresář, který se má vložit DbContext. Cesty jsou relativní vzhledem k adresáři projektu.             |
| -Kontextu \<řetězec >                       | Název DbContext ke generování.                                                           |
| -Schémata \<String [] >                     | Schémata tabulek ke generování typů entit pro.                                              |
| -Tabulky \<String [] >                      | Tabulky pro typy entit pro generování.                                                         |
| -DataAnnotations                         | Atributy lze použijte ke konfiguraci modelu (Pokud je to možné). Pokud tento parametr vynechán, použije se pouze rozhraní fluent API. |
| -UseDatabaseNames                        | Použijte názvy tabulek a sloupců přímo z databáze.                                           |
| -Force                                   | Přepište existující soubory.                                                                        |

### <a name="script-migration"></a>Skript migrace

Vygeneruje skript SQL z migrace.

Parametry:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-Od* \<řetězec > | Počáteční migraci. Výchozí hodnota je 0 (výchozí databáze).      |
| *-Na* \<řetězec >   | Dokončení migrace. Výchozí hodnota je poslední migrace.              |
| -Idempotentní       | Generovat skript, který jde použít na databáze v libovolné migrace. |
| -Výstupní \<řetězec > | Soubor pro zápis výsledek.                                   |

> [!TIP]
> Na, z, a podpora rozšíření tab výstupní parametry.

### <a name="update-database"></a>Aktualizace databáze

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*– Migrace* \<řetězec ></nobr> | Cíl migrace. Pokud je "0", se vrátí zpět všechny migrace. Výchozí hodnota je poslední migrace. |

> [!TIP]
> Parametr migrace podporuje rozšíření tab.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
