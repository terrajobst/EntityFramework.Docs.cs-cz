---
title: Referenční dokumentace k nástrojům pro EF Core (konzola správce balíčků) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181336"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Referenční informace o nástrojích Entity Framework Core Tools – konzola správce balíčků v aplikaci Visual Studio

Nástroje konzoly Správce balíčků (PMC) pro Entity Framework Core prováděly úlohy vývoje v době návrhu. Například vytvářejí [migrace](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), aplikují migrace a generují kód pro model založený na stávající databázi. Příkazy se spouští uvnitř sady Visual Studio pomocí [konzoly Správce balíčků](/nuget/tools/package-manager-console). Tyto nástroje fungují v projektech .NET Framework i .NET Core.

Pokud nepoužíváte aplikaci Visual Studio, doporučujeme místo toho použít [EF Core nástroje příkazového řádku](dotnet.md) . Nástroje rozhraní příkazového řádku jsou různé platformy a spouštějí se v příkazovém řádku.

## <a name="installing-the-tools"></a>Instalace nástrojů

Postupy pro instalaci a aktualizaci nástrojů se liší od ASP.NET Core 2.1 + a starších verzí nebo jiných typů projektů.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core verze 2,1 a novější

Nástroje jsou automaticky zahrnuty v projektu ASP.NET Core 2.1 +, protože balíček `Microsoft.EntityFrameworkCore.Tools` je součástí [Microsoft. AspNetCore. app Metapackage](/aspnet/core/fundamentals/metapackage-app).

Proto nemusíte nic dělat k instalaci nástrojů, ale budete muset:
* Obnovte balíčky před použitím nástrojů v novém projektu.
* Nainstalujte balíček pro aktualizaci nástrojů na novější verzi.

Abyste se ujistili, že budete získávat nejnovější verze nástrojů, doporučujeme, abyste provedli také následující kroky:

* Upravte soubor *. csproj* a přidejte řádek, který určuje nejnovější verzi balíčku [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) . Například soubor *. csproj* může zahrnovat `ItemGroup`, který vypadá takto:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aktualizujte nástroje, když se zobrazí zpráva podobná následujícímu příkladu:

> Verze EF Core Tools "2.1.1-RTM-30846" je starší než modul runtime "2.1.3-RTM-32065". Aktualizujte nástroje na nejnovější funkce a opravy chyb.

Aktualizace nástrojů:
* Nainstalujte nejnovější .NET Core SDK.
* Aktualizujte si Visual Studio na nejnovější verzi.
* Upravte soubor *. csproj* tak, aby zahrnoval odkaz na balíček na nejnovější balíček nástrojů, jak je uvedeno výše.

### <a name="other-versions-and-project-types"></a>Další verze a typy projektů

Nástroje konzoly Správce balíčků nainstalujete spuštěním následujícího příkazu v **konzole správce balíčků**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Aktualizujte nástroje spuštěním následujícího příkazu v **konzole správce balíčků**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Ověření instalace

Spuštěním tohoto příkazu ověřte, zda jsou nástroje nainstalovány:

``` powershell
Get-Help about_EntityFrameworkCore
```

Výstup vypadá takto (neříká vám to, jakou verzi nástrojů používáte):

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Používání nástrojů

Před použitím těchto nástrojů:
* Pochopte rozdíl mezi cílovým a spouštěným projektem.
* Naučte se používat nástroje s .NET Standard knihoven tříd.
* Pro ASP.NET Core projekty nastavte prostředí.

### <a name="target-and-startup-project"></a>Cílový a spouštěný projekt

Příkazy odkazují na *projekt* a na *spouštěný projekt*.

* *Projekt* je také označován jako *cílový projekt* , protože je tam, kde příkazy přidávají nebo odebírají soubory. Ve výchozím nastavení je **výchozí projekt** vybraný v **konzole správce balíčků** cílový projekt. Můžete určit jiný projekt jako cílový projekt pomocí možnosti <nobr>`--project`</nobr> .

* Spouštěný *projekt* je ten, který nástroje sestavují a spouštějí. Nástroje musí spustit kód aplikace v době návrhu, aby získali informace o projektu, jako je například připojovací řetězec databáze a konfigurace modelu. Ve výchozím nastavení je **projekt po spuštění** v **Průzkumník řešení** spouštěný projekt. Můžete určit jiný projekt jako spouštěný projekt pomocí možnosti <nobr>`--startup-project`</nobr> .

Spouštěcí projekt a cílový projekt jsou často stejný projekt. Typický scénář, kde jsou samostatné projekty, je:

* EF Core kontext a třídy entit jsou v knihovně tříd .NET Core.
* Aplikace konzoly .NET Core nebo webová aplikace odkazuje na knihovnu tříd.

Je také možné [umístit kód migrace do knihovny tříd odděleně od EF Coreho kontextu](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Další cílová rozhraní

Nástroje konzoly Správce balíčků fungují v projektech .NET Core nebo .NET Framework. Aplikace s modelem EF Core v knihovně tříd .NET Standard nemusí mít projekt .NET Core nebo .NET Framework. Například to platí pro Xamarin a Univerzální platforma Windows aplikace. V takových případech můžete vytvořit projekt konzolové aplikace .NET Core nebo .NET Framework, jehož jediným účelem je jednat jako projekt po spuštění pro nástroje. Projekt může být fiktivní projekt bez skutečného kódu &mdash; je potřeba jenom poskytnout cíl pro nástroje.

Proč je vyžadován fiktivní projekt? Jak bylo zmíněno dříve, nástroje musí spustit kód aplikace v době návrhu. K tomu je potřeba použít modul runtime .NET Core nebo .NET Framework. Když je model EF Core v projektu cíleném na rozhraní .NET Core nebo .NET Framework, EF Core nástroje vypůjčí modul runtime z projektu. Nemůžou to dělat, pokud je model EF Core v knihovně tříd .NET Standard. .NET Standard není skutečná implementace rozhraní .NET; je to specifikace sady rozhraní API, které musí implementace rozhraní .NET podporovat. Proto .NET Standard není dostačující pro EF Core nástroje pro spouštění kódu aplikace. Fiktivní projekt, který vytvoříte pro použití jako spouštěný projekt, poskytuje konkrétní cílovou platformu, do které mohou nástroje načíst .NET Standard knihovny tříd.

### <a name="aspnet-core-environment"></a>ASP.NET Core prostředí

Chcete-li určit prostředí pro ASP.NET Core projekty, nastavte **obálku ENV: ASPNETCORE_ENVIRONMENT** před spuštěním příkazů.

## <a name="common-parameters"></a>Společné parametry

V následující tabulce jsou uvedeny parametry, které jsou společné pro všechny EF Core příkazy:

| Parametr                 | Popis                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Context @no__t – > 0String        | Třída `DbContext`, která se má použít. Pouze název třídy nebo plně kvalifikovaný obory názvů.  Pokud je tento parametr vynechán, EF Core najde kontextovou třídu. Pokud existuje více kontextových tříd, je tento parametr povinný. |
| -Project @no__t – 0String >        | Cílový projekt. Pokud je tento parametr vynechán, použije se jako cílový projekt **výchozí projekt** pro **konzolu Správce balíčků** .                                                                             |
| -StartupProject \<String > | Spouštěný projekt. Pokud je tento parametr vynechán, použije se jako cílový projekt **spouštěcí projekt** ve **vlastnostech řešení** .                                                                                 |
| – Verbose                  | Zobrazit podrobný výstup.                                                                                                                                                                                                 |

Pokud chcete zobrazit informace o nápovědě k příkazu, použijte příkaz `Get-Help` prostředí PowerShell.

> [!TIP]
> Kontext, projekt a parametry StartupProject podporují rozšíření karty.

## <a name="add-migration"></a>Přidání – migrace

Přidá novou migraci.

Parametry:

| Parametr                         | Popis                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| @no__t -0-Name \<String > <nobr>       | Název migrace. Toto je poziční parametr a je povinný.                                              |
| <nobr>-OutputDir \<String ></nobr> | Adresář (a dílčí obor názvů), který se má použít. Cesty jsou relativní vzhledem k cílovému adresáři projektu. Ve výchozím nastavení se jedná o "migrace". |

## <a name="drop-database"></a>Vyřadit z databáze

Zruší databázi.

Parametry:

| Parametr | Popis                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Zobrazit, která databáze bude vyřazena, ale neodstraňovat ji. |

## <a name="get-dbcontext"></a>Get-DbContext

Získá informace o typu `DbContext`.

## <a name="remove-migration"></a>Odebrání – migrace

Odebere poslední migraci (vrátí zpět změny kódu, které byly provedeny pro migraci).

Parametry:

| Parametr | Popis                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Obnovte migraci (vraťte zpět změny, které byly pro databázi aplikovány). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Generuje kód pro `DbContext` a typy entit pro databázi. Aby `Scaffold-DbContext` generovat typ entity, musí mít databázová tabulka primární klíč.

Parametry:

| Parametr                          | Popis                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection @no__t – > 1String</nobr> | Připojovací řetězec k databázi. Pro projekty ASP.NET Core 2. x může být hodnota *Name = \<Name připojovacího řetězce >* . V takovém případě název pochází ze zdrojů konfigurace, které jsou nastaveny pro projekt. Toto je poziční parametr a je povinný. |
| <nobr>-Provider @no__t – > 1String</nobr>   | Poskytovatel, který se má použít. Obvykle se jedná o název balíčku NuGet, například: `Microsoft.EntityFrameworkCore.SqlServer`. Toto je poziční parametr a je povinný.                                                                                           |
| -OutputDir \<String >               | Adresář, do kterého se mají umístit soubory Cesty jsou relativní vzhledem k adresáři projektu.                                                                                                                                                                                             |
| -ContextDir \<String >              | Adresář, do kterého se má umístit soubor `DbContext` Cesty jsou relativní vzhledem k adresáři projektu.                                                                                                                                                                              |
| -Context @no__t – > 0String                 | Název třídy `DbContext`, která má být vygenerována.                                                                                                                                                                                                                          |
| -Schemas \<String [] >               | Schémata tabulek, pro které se mají generovat typy entit Pokud je tento parametr vynechán, jsou zahrnuty všechna schémata.                                                                                                                                                             |
| -Tables \<String [] >                | Tabulky, pro které se mají generovat typy entit Pokud je tento parametr vynechán, jsou zahrnuty všechny tabulky.                                                                                                                                                                         |
| – Dataanotace                   | Použijte atributy ke konfiguraci modelu (Pokud je to možné). Pokud tento parametr vynecháte, použije se jenom rozhraní API Fluent.                                                                                                                                                      |
| -UseDatabaseNames                  | Názvy tabulek a sloupců používejte přesně tak, jak se zobrazí v databázi. Pokud je tento parametr vynechán, jsou názvy databází změněny, aby lépe odpovídaly konvencím stylu C# názvu.                                                                                       |
| -Force                             | Přepsat existující soubory.                                                                                                                                                                                                                                               |

Příklad:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Příkladem je, že generování pouze tabulek vybralo pouze tabulky a vytvoří kontext v samostatné složce se zadaným názvem:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Migrace skriptu

Vygeneruje skript SQL, který aplikuje všechny změny z jedné vybrané migrace na jinou vybranou migraci.

Parametry:

| Parametr                | Popis                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *– Od* @no__t – 1String >        | Spouští se migrace. Migrace může být identifikována podle názvu nebo podle ID. Číslo 0 je zvláštní případ, který znamená *před první migrací*. Výchozí hodnota je 0.                                                              |
| *-To* \<String >          | Koncová migrace. Výchozí hodnota je poslední migrace.                                                                                                                                                                      |
| <nobr>– Idempotentní</nobr> | Vygenerujte skript, který se dá použít v databázi při libovolné migraci.                                                                                                                                                         |
| -Output @no__t – 0String >        | Soubor, do kterého se má zapisovat výsledek Pokud je tento parametr vynechán, vytvoří se soubor s vygenerovaným názvem ve stejné složce, ve které jsou vytvořeny běhové soubory aplikace, například: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* . |

> [!TIP]
> Parametry do, z a výstup podporují rozšíření na kartě.

Následující příklad vytvoří skript pro migraci InitialCreate pomocí názvu migrace.

```powershell
Script-Migration -To InitialCreate
```

Následující příklad vytvoří skript pro všechny migrace po migraci InitialCreate pomocí ID migrace.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Aktualizace – databáze

Aktualizuje databázi na poslední migraci nebo na určenou migraci.

| Parametr                           | Popis                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr> *-@No__t migrace* – 2String ></nobr> | Cílová migrace Migrace může být identifikována podle názvu nebo podle ID. Číslo 0 je zvláštní případ, který znamená *před první migrací* a způsobí, že se všechny migrace vrátí zpět. Pokud není zadaná žádná migrace, příkaz se nastaví jako výchozí pro poslední migraci. |

> [!TIP]
> Parametr Migration podporuje rozšíření karty.

Následující příklad vrátí všechny migrace.

```powershell
Update-Database -Migration 0
```

V následujících příkladech je databáze aktualizována na určenou migraci. První používá název migrace a druhý používá ID migrace:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Další zdroje

* [Migrace](xref:core/managing-schemas/migrations/index)
* [Zpětná analýza](xref:core/managing-schemas/scaffolding)
