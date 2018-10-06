---
title: Migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 5ae06a4342a556936dc44c5bf6622814eaad4733
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834744"
---
<a name="migrations"></a>Migrace
==========

Datový model se změní při vývoji a získá synchronizován s databází. Můžete vyřaďte databázi a umožní vytvořit nový, který odpovídá modelu EF, ale tento postup má za následek ztrátu dat. Funkce migrace v EF Core poskytuje způsob, jak přírůstkové aktualizace schématu databáze, aby udržovat synchronizované s datovým modelem aplikace při zachování stávajících dat v databázi.

Migrace zahrnuje nástroje pro příkazový řádek a rozhraní API, která vám pomůžou s následující úkoly:

* [Vytvoření migrace](#create-a-migration). Generovat kód, který může aktualizovat databázi pro synchronizaci se sadou změn modelu.
* [Aktualizovat databázi](#update-the-database). Použijte čekající migrace aktualizovat schéma databáze.
* [Migrace code přizpůsobit](#customize-migration-code). Někdy musí upravit, nebo doplnit generovaného kódu.
* [Odebrat migrace](#remove-a-migration). Odstranění vygenerovaného kódu.
* [Vrácení migrace](#revert-a-migration). Vrátit zpět změny databáze.
* [Generovat SQL skripty](#generate-sql-scripts). Může být nutné skript aktualizovat produkční databáze nebo řešení potíží s migrací kódu.
* [Použití migrace za běhu](#apply-migrations-at-runtime). Když aktualizace návrhu a spouštění skriptů nejsou nejlepší možnosti, zavolejte `Migrate()` metody.

<a name="install-the-tools"></a>Instalace nástrojů
-----------------

Nainstalujte [nástroje příkazového řádku](xref:core/miscellaneous/cli/index):
* Pro Visual Studio, doporučujeme, abyste [Konzola správce balíčků nástroje](xref:core/miscellaneous/cli/powershell).
* U jiných vývojových prostředích, zvolte [nástroje rozhraní příkazového řádku .NET Core](xref:core/miscellaneous/cli/dotnet).

<a name="create-a-migration"></a>Vytvoření migrace
------------------

Po [definované počáteční modelu](xref:core/modeling/index), je čas vytvořit databázi. Chcete-li přidat počáteční migraci spusťte následující příkaz.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Tři soubory jsou přidány do projektu v rámci **migrace** adresáře:

* **00000000000000_InitialCreate.cs**– migrace hlavní soubor. Obsahuje operace, která je nutné použít migraci (v `Up()`) a vrátit ji (v `Down()`).
* **00000000000000_InitialCreate.Designer.cs**– soubor metadat migrace. Obsahuje informace, které EF používá.
* **MyContextModelSnapshot.cs**– snímek aktuální model. Umožňuje určit, co se změnilo, při přidávání další migraci.

Časové razítko v názvu souboru udržet chronologicky seřadit, abyste viděli průběh změny.

> [!TIP]
> Můžete libovolně přesouvat soubory migrace a měnit jejich oboru názvů. Nová migrace jsou vytvořeny jako poslední migrace objektů stejné úrovně.

<a name="update-the-database"></a>Aktualizace databáze
-------------------

V dalším kroku použití migrace k databázi a vytvoření schématu.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a>Přizpůsobení migrace kódu
------------------------

Po provedení změn do modelu EF Core, může být schématu databáze synchronizované. Abyste je připojili k aktuálním stavu, přidejte další migraci. Název migrace můžete použít jako zprávu potvrzení v systému správy verzí. Například můžete zvolit název, například *AddProductReviews* Pokud změna je novou třídu entity u kontroly.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po migraci je vygenerovaná (kód generovaný pro něj), projděte si kód pro přesnost a přidat, odebrat nebo změnit všechny operace nutné k použití správně.

Migrace může například obsahovat následující operace:

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Přestože tyto operace provést schéma databáze kompatibilní, neuchovají se stávajícími názvy zákazníka. Chcete-li lépe, přepište ho následujícím způsobem.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> Proces migrace generování uživatelského rozhraní upozorní, když operace může způsobit ztrátu dat (např. vyřazení sloupce). Pokud zjistíte, že upozornění, zejména si přečtěte téma migrace kódu přesnost.

Použití migrace k databázi pomocí příslušný příkaz.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a>Prázdný migrace

Někdy je užitečné, chcete-li přidat migrace bez jakýchkoli změn modelu. V tomto případě přidáte novou migraci vytvoří soubory s kódem s prázdné třídy. Můžete přizpůsobit, tato migrace provádět operace, které přímo nesouvisejí modelu EF Core. Některé věci, které můžete chtít spravovat tímto způsobem jsou:

* Fulltextové vyhledávání
* Funkce
* Uložené procedury
* Aktivační procedury
* Zobrazení

<a name="remove-a-migration"></a>Odstranit migraci
------------------
Někdy přidejte migraci a Uvědomte si, že budete muset udělat dodatečné změny do modelu EF Core před každým jejím použitím. Pokud chcete odebrat posledního migrace, použijte tento příkaz.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po odebrání migrace, můžete provést další změny a potom ho znovu přidejte.

<a name="revert-a-migration"></a>Vrácení migrace
------------------
Pokud jste již nainstalovali migrace (nebo několik migrací) do databáze, ale potřeba ji vzít zpět, slouží k použití migrace, ale zadejte název, který chcete vrátit zpět k migraci stejný příkaz.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a>Generování skriptů SQL
--------------------
Při ladění vaše migrace nebo jejich nasazení do produkční databáze, je užitečné se vygenerovat skript SQL. Skript můžete být dále kontroluje přesnost a vyladění podle potřeb provozní databáze. Skript je také možné ve spojení s technologií nasazení. Základní příkaz vypadá takto.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Existuje několik možností pro tento příkaz.

**z** migrace by měla být použita pro databázi před spuštěním skriptu poslední migrace. Pokud byly použity žádné migrace, zadejte `0` (Toto je výchozí hodnota).

**k** migrace je poslední migrace, která bude použita pro databázi po spuštění skriptu. Výchozí hodnota poslední migrace ve vašem projektu.

**Idempotentní** skriptu můžete volitelně vygenerovat. Tento skript migrace platí pouze, pokud ještě nebyla použita k databázi. To je užitečné, pokud zatím nevíte přesně poslední migrace použita pro databázi byla nebo pokud provádíte nasazení do více databází, které lze jednotlivě za různé migrace.

<a name="apply-migrations-at-runtime"></a>Použití migrace za běhu
---------------------------
Některé aplikace může být vhodné pro použití migrace za běhu při spuštění nebo při prvním spuštění. K tomu `Migrate()` metody.

Tato metoda vytvoří nahoře `IMigrator` služby, které lze použít pro pokročilejší scénáře. Použití `DbContext.GetService<IMigrator>()` k němu přistupovat.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * Tento přístup se nehodí pro každého. I když je skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat více robustní strategii nasazení, jako je generování skriptů SQL.
> * Nevolejte `EnsureCreated()` před `Migrate()`. `EnsureCreated()` obchází migrace k vytvoření schématu, což způsobí, že `Migrate()` selhání.

<a name="next-steps"></a>Další kroky
----------

Další informace naleznete v tématu <xref:core/miscellaneous/cli/index>.