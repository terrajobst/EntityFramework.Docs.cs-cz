---
title: Migrace s více projekty – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997984"
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
