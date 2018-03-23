---
title: Upgrade z EF základní 1.0 RC2 na RTM – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Upgrade na verzi RTM EF Core 1.0 RC2

Tento článek obsahuje pokyny pro přesouvání aplikace vytvořené s nástroji RC2 balíčky, do kterých 1.0.0 RTM.

## <a name="package-versions"></a>Verze balíčku

Názvy balíčky nejvyšší úrovně, které by obvykle nainstalovat do aplikace mezi RC2 a RTM nezměnila.

**Je třeba upgradovat na verze RTM nainstalované balíčky:**

* Modul runtime balíčky (například `Microsoft.EntityFrameworkCore.SqlServer`) se změnil z `1.0.0-rc2-final` k `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Balíčku se změnil z `1.0.0-preview1-final` k `1.0.0-preview2-final`. Upozorňujeme, že nástrojů se stále předběžné verze.

## <a name="existing-migrations-may-need-maxlength-added"></a>Existující migrace může být nutné přidat maxLength

V RC2, definice sloupců v migraci hledá jako `table.Column<string>(nullable: true)` a délka sloupce byla vyhledávány v některá metadata ukládáme v kódu migrace. Ve verzi RTM, délka je nyní zahrnutá v automaticky generovaný kód `table.Column<string>(maxLength: 450, nullable: true)`.

Nebude mít žádné existující migrace, které byly před použitím RTM generované uživatelské rozhraní `maxLength` byl zadán neplatný argument. To znamená, že se použije maximální délka podporovaná databáze (`nvarchar(max)` na serveru SQL Server). To může být vhodná pro některé sloupce, ale sloupce, které jsou součástí klíč, cizí klíč nebo index musí být aktualizováno, aby zahrnovalo maximální délku. Podle konvence je 450 maximální délku použít pro klíče, cizí klíče a indexované sloupce. Pokud jste nakonfigurovali explicitně délkou v modelu, měli byste použít dlouhou místo.

**ASP.NET Identity**

Tato změna ovlivňuje projekty, které používají ASP.NET Identity a byly vytvořeny z po předem-RTM šablona projektu. Šablona projektu zahrnuje migraci použít k vytvoření databáze. Tato migrace musí upravit, chcete-li určit maximální délku `256` pro následující sloupce.

*  **AspNetRoles**

    * Název

    * NormalizedName

*  **AspNetUsers**

   * E-mailu

   * NormalizedEmail

   * NormalizedUserName

   * UserName

K provedení této změny bude neuděláte následující výjimka při počáteční migrace se použije k databázi.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Odeberte "importy" v souboru project.json

Pokud byly cílem .NET Core RC2, je potřeba přidat `imports` do souboru project.json jako dočasné řešení pro některé základní EF závislosti nejsou podporovány .NET Standard. Tyto nyní může být odebrán.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> Od verze 1.0 RTM, [.NET Core SDK](https://www.microsoft.com/net/download/core) již nepodporuje `project.json` nebo vývoj aplikací .NET Core pomocí sady Visual Studio 2015. Doporučujeme [migrovat ze souboru project.json csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Pokud používáte Visual Studio, doporučujeme upgradu na [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Přidejte přesměrování vazby

Došlo k pokusu o spuštění EF příkazy na výsledky projekty univerzální platformu Windows (UWP) s následující chybou:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Budete muset ručně přidejte přesměrování vazby do projektu UPW. Vytvořte soubor s názvem `App.config` v projektu kořenová složka a přidejte přesměrování verzí sestavení správný.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
