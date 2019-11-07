---
title: Použití samostatného projektu migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0c08855db77470d28e23f9ef1d147497dfcdff83
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655554"
---
# <a name="using-a-separate-migrations-project"></a>Použití samostatného projektu migrace

Můžete chtít ukládat migrace v jiném sestavení, než jaké obsahuje vaše `DbContext`. Tuto strategii můžete také použít k údržbě několika skupin migrace, například jednoho pro vývoj a další pro upgrady typu Release-to-Release.

Postup...

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

## <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
