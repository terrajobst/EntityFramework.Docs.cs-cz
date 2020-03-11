---
title: Použití samostatného projektu migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416813"
---
# <a name="using-a-separate-migrations-project"></a>Použití samostatného projektu migrace

Můžete chtít ukládat migrace v jiném sestavení, než jaké obsahuje vaše `DbContext`. Tuto strategii můžete také použít k údržbě několika skupin migrace, například jednoho pro vývoj a další pro upgrady typu Release-to-Release.

Požadovaná akce...

1. Vytvořte novou knihovnu tříd.

2. Přidejte odkaz na své DbContext sestavení.

3. Přesuňte migrace a soubory snímků modelu do knihovny tříd.
   > [!TIP]
   > Pokud nemáte žádná existující migrace, vygenerujte ji v projektu obsahujícím DbContext a pak ji přesuňte.
   > To je důležité, protože pokud migrační sestavení neobsahují existující migraci, příkaz Add-Migration nebude schopen najít DbContext.

4. Nakonfigurujte sestavení migrace:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Přidejte odkaz na vaše sestavení migrace ze sestavení po spuštění.
   * Pokud to způsobí cyklickou závislost, aktualizujte výstupní cestu knihovny tříd:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Pokud jste provedli vše správně, měli byste být schopni přidat do projektu nové migrace.

## <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
