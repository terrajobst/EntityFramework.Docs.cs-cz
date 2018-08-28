---
title: Migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996519"
---
<a name="migrations"></a>Migrace
==========
Migrace poskytují způsob, jak postupně provést změny schématu databáze, kterou chcete udržovat synchronizované s EF Core model při zachování stávajících dat v databázi.

<a name="creating-the-database"></a>Vytvoření databáze
---------------------
Po [definované počáteční modelu][1], je čas vytvořit databázi. Chcete-li to provést, přidejte počáteční migraci.
Nainstalujte [EF Core Tools] [ 2] a spusťte příslušný příkaz.

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

V dalším kroku použití migrace k databázi a vytvoření schématu.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Přidání další migraci
------------------------
Po provedení změn do modelu EF Core, bude synchronizován schéma databáze. Abyste je připojili k aktuálním stavu, přidejte další migraci. Název migrace můžete použít jako zprávu potvrzení v systému správy verzí. Například, pokud byly provedeny změny uložte zapracováním připomínek zákazníků produktů, můžu rozhodnout něco jako *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po migraci je automaticky, zkontrolujte přesnost a přidejte jakékoli další operace nutné k použití správně. Migrace může například obsahovat následující operace:

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
> Přidání nové migrace upozorní, když automaticky operace, která může způsobit ztrátu dat (např. vyřazení sloupce). Nezapomeňte si zejména Zkontrolujte tyto migrace přesnost.

Použití migrace k databázi pomocí příslušný příkaz.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Odebírá se migrace
--------------------
Někdy přidejte migraci a Uvědomte si, že budete muset udělat dodatečné změny do modelu EF Core před každým jejím použitím.
Pokud chcete odebrat posledního migrace, použijte tento příkaz.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po odebrání, můžete provést další změny a potom ho znovu přidejte.

<a name="reverting-a-migration"></a>Vrácení migrace zpět
---------------------
Pokud jste již nainstalovali migrace (nebo několik migrací) do databáze, ale potřeba ji vzít zpět, slouží k použití migrace, ale zadejte název, který chcete vrátit zpět k migraci stejný příkaz.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Prázdný migrace
----------------
Někdy je užitečné, chcete-li přidat migrace bez jakýchkoli změn modelu. Přidání nové migrace vytvoří v tomto případě některý z prázdné. Můžete přizpůsobit, tato migrace provádět operace, které přímo nesouvisejí modelu EF Core.
Některé věci, které můžete chtít spravovat tímto způsobem jsou:

* Fulltextové vyhledávání
* Funkce
* Uložené procedury
* Aktivační procedury
* Zobrazení
* Atd.

<a name="generating-a-sql-script"></a>Generování skriptu SQL
-----------------------
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

<a name="applying-migrations-at-runtime"></a>Použití migrace za běhu
------------------------------
Některé aplikace může být vhodné pro použití migrace za běhu při spuštění nebo při prvním spuštění. K tomu `Migrate()` metody.

Upozornění: Tento přístup se nehodí pro každého. I když je skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat více robustní strategii nasazení, jako je generování skriptů SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Nevolejte `EnsureCreated()` před `Migrate()`. `EnsureCreated()` obchází migrace k vytvoření schématu, což způsobí, že `Migrate()` selhání.

> [!NOTE]
> Tato metoda vytvoří nahoře `IMigrator` služby, které lze použít pro pokročilejší scénáře. Použití `DbContext.GetService<IMigrator>()` k němu přistupovat.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
