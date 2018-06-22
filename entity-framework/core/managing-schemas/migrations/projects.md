---
title: Migrace s více projekty - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161224"
---
<a name="using-a-separate-project"></a>Pomocí samostatného projektu
========================
Můžete chtít uložit vaše migrace v jiném sestavení než jeden obsahující vaše `DbContext`. Můžete také tuto strategii zachování více sad migrace, například, jeden pro vývoj a druhý pro upgrade verze verze.

Postup...

1. Vytvořte nové knihovny tříd.

2. Přidáte odkaz na sestavení vaší DbContext.

3. Přesuňte soubory snímků modelu a migrace do knihovny tříd.
   * Pokud nepřidali jste žádné, přidat do projektu DbContext a přesunout ho.

4. Konfigurace sestavení migrace:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Přidáte odkaz na vaše migrace sestavení ze sestavení po spuštění.
   * Pokud to způsobuje cyklickou závislost, aktualizujte výstupní cesta knihovny tříd:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Pokud jste to udělali všechno správně, byste měli přidávat nové migrace do projektu.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
