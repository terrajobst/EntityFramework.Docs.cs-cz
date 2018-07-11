---
title: Upgrade z EF Core 1.0 RC2 na RTM – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949054"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Upgrade z EF Core 1.0 RC2 na RTM

Tento článek obsahuje pokyny pro přesouvání aplikace sestavené s balíčky RC2 nastavená na 1.0.0 RTM.

## <a name="package-versions"></a>Verze balíčku

Názvy balíčků na nejvyšší úrovni, která by obvykle instalují do aplikace neliší RC2 a verze RTM.

**Je třeba nainstalované balíčky s upgradem na verzi RTM verze:**

* Balíčky runtime (například `Microsoft.EntityFrameworkCore.SqlServer`) změnil z `1.0.0-rc2-final` k `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Balíčku změnil z `1.0.0-preview1-final` k `1.0.0-preview2-final`. Všimněte si, že nástroje je stále předběžné verze.

## <a name="existing-migrations-may-need-maxlength-added"></a>Existující migrace může být nutné přidat maxLength

Ve verzi RC2, definice sloupců v migraci vypadal `table.Column<string>(nullable: true)` a délka sloupce byla vyhledána v některá metadata uložíme v kódu na migraci. Ve verzi RTM, délka je teď součástí automaticky generovaný kód `table.Column<string>(maxLength: 450, nullable: true)`.

Všechny existující migrace, které byly automaticky před použitím RTM nebude mít `maxLength` zadaný argument. To znamená, že se použije maximální délka podporována databází (`nvarchar(max)` na SQL serveru). To může být u některých sloupců, ale sloupce, které jsou součástí klíče, cizí klíč nebo index musí být aktualizováno, aby zahrnovalo maximální délku. Podle konvence je 450 maximální počet znaků použitý pro klíče, cizí klíče a indexovaných sloupců. Pokud jste nakonfigurovali explicitně délku v modelu, měli byste použít delší místo.

**ASP.NET Identity**

Tato změna ovlivní projekty, které používají ASP.NET Identity a byly vytvořeny z předprodukčním-RTM šablony projektu. Šablona projektu zahrnuje migraci použít k vytvoření databáze. Tato migrace je nutné upravit k určení maximální délce `256` pro tyto sloupce.

*  **AspNetRoles**

    * Název

    * NormalizedName

*  **AspNetUsers**

   * E-mailu

   * NormalizedEmail

   * NormalizedUserName

   * UserName

K provedení této změny bude dojít k následující výjimce při použití u počáteční migraci databáze.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Odebere "importy" v souboru project.json

Pokud se zaměřujete na .NET Core RC2, je potřeba přidat `imports` k souboru project.json jako dočasným řešením pro některý ze EF Core závislosti, které nepodporují .NET Standard. Ty nyní je možné odebrat.

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
> Od verze 1.0 RTM, [.NET Core SDK](https://www.microsoft.com/net/download/core) už nepodporuje `project.json` nebo vývoj aplikací .NET Core pomocí sady Visual Studio 2015. Doporučujeme [migrace z project.json na csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Pokud používáte Visual Studio, doporučujeme je upgradovat na [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UPW: Přidat přesměrování vazby

Došlo k pokusu o spuštění EF příkazy na univerzální platformu Windows (UPW) výsledky projekty s následující chybou:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Budete muset ručně přidat přesměrování vazby do projektu UPW. Vytvořte soubor s názvem `App.config` v projektu kořenová složka a přidání přesměrování verze sestavení správná.

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
