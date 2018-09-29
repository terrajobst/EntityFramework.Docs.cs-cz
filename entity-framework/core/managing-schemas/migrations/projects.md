---
title: Migrace s více projekty – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447141"
---
<a name="using-a-separate-project"></a>Použití samostatného projektu
========================
Můžete chtít ukládat vaše migrace v jiném sestavení než jeden obsahující vaše `DbContext`. Můžete také tuto strategii udržovat několik sad migrace, například, jeden pro vývoj a druhý pro upgrady verzí.

Postup...

1. Vytvořte novou knihovnu tříd.

2. Přidejte odkaz na sestavení DbContext.

3. Přesuňte soubory snímků modelu a migrace do knihovny tříd.
   > [!TIP]
   > Pokud máte k dispozici žádné existující migrace, vygenerujte ho projekt, který obsahuje objekt DbContext a přesuňte ho. To je důležité, protože pokud migrace sestavení neobsahuje existující migrace, příkazu Add-migrace nebude moci najít uvolněn objekt DbContext.

4. Konfigurace sestavení migrace:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Přidejte odkaz na sestavení migrace ze spuštění sestavení.
   * Pokud to způsobuje cyklickou závislost, aktualizujte výstupní cestu knihovny tříd:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Pokud jste to udělali všechno správně, byste měli možnost přidávat nové migrace do projektu.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
