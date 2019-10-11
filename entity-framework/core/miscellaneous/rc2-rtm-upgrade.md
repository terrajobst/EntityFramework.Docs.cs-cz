---
title: Upgrade z verze EF Core 1,0 RC2 na RTM-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181364"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Upgrade z verze EF Core 1,0 RC2 na RTM

Tento článek poskytuje pokyny pro přesunutí aplikace vytvořené s balíčky RC2 do 1.0.0 RTM.

## <a name="package-versions"></a>Verze balíčku

Názvy balíčků nejvyšší úrovně, které byste obvykle nainstalovali do aplikace, se mezi verzemi RC2 a RTM nezměnily.

**Je nutné upgradovat nainstalované balíčky na verzi RTM:**

* Běhové balíčky (například `Microsoft.EntityFrameworkCore.SqlServer`) se změnily z `1.0.0-rc2-final` na `1.0.0`.

* Balíček `Microsoft.EntityFrameworkCore.Tools` se změnil z `1.0.0-preview1-final` na `1.0.0-preview2-final`. Všimněte si, že nástroj je stále předběžnou verzí.

## <a name="existing-migrations-may-need-maxlength-added"></a>Existující migrace můžou vyžadovat přidání maxLength.

V rámci RC2 se definice sloupce v migraci jako `table.Column<string>(nullable: true)` a délka sloupce vyhledala v některých metadatech, které ukládáme do kódu na pozadí migrace. Ve verzi RTM je tato délka obsažená ve vygenerovaném kódu `table.Column<string>(maxLength: 450, nullable: true)`.

U všech existujících migrací, které byly vygenerované před použitím RTM, nebude mít zadaný argument `maxLength`. To znamená, že se použije maximální délka podporovaná databází (`nvarchar(max)` v SQL Server). To může být pro některé sloupce jemné, ale sloupce, které jsou součástí klíče, cizího klíče nebo indexu, se musí aktualizovat tak, aby zahrnovaly maximální délku. Podle konvence je 450 maximální délka používané pro klíče, cizí klíče a indexované sloupce. Pokud jste v modelu explicitně nakonfigurovali délku, měli byste místo toho použít tuto délku.

**ASP.NET Identity**

Tato změna ovlivní projekty, které používají ASP.NET Identity a byly vytvořeny ze šablony projektu před verzí RTM. Šablona projektu zahrnuje migraci použitou k vytvoření databáze. Tato migrace musí být upravena, aby určovala maximální délku `256` pro následující sloupce.

*  **AspNetRoles**

    * Name

    * NormalizedName

*  **AspNetUsers**

   * Email

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Tato změna způsobí při použití prvotní migrace na databázi následující výjimku.

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: Odebrat importy v Project. JSON

Pokud jste se rozhodli .NET Core s verzí RC2, je nutné přidat `imports` do Project. JSON jako dočasné řešení pro některé závislosti EF Core, které nepodporují .NET Standard. Teď je můžete odebrat.

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
> Od verze 1,0 RTM již [.NET Core SDK](https://www.microsoft.com/net/download/core) nadále nepodporují `project.json` ani vývoj aplikací .NET Core pomocí sady Visual Studio 2015. Doporučujeme [migrovat z Project. JSON na csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Pokud používáte aplikaci Visual Studio, doporučujeme upgradovat na [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>PODPORUJÍ Přidat přesměrování vazby

Když se pokusíte spustit příkazy EF v projektech Univerzální platforma Windows (UWP), dojde k následující chybě:

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

Je nutné ručně přidat přesměrování vazby do projektu UWP. V kořenové složce projektu vytvořte soubor s názvem `App.config` a přidejte přesměrování do správné verze sestavení.

```xml
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
