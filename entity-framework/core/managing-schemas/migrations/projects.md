---
title: Použití samostatného projektu migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812028"
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

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
