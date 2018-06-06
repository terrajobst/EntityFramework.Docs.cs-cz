---
title: Migrace - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754506"
---
<a name="migrations"></a>Migrace
==========
Migrace poskytují způsob, jak přírůstkově použít změny schématu k databázi a udržovat synchronizované se svým modelem EF základní současně v něm zachovat stávající data v databázi.

<a name="creating-the-database"></a>Vytvoření databáze
---------------------
Poté, co jste [definované počáteční modelu][1], je čas vytvořit databázi. Chcete-li to provést, přidejte počáteční migrace.
Nainstalujte [EF základní nástroje] [ 2] a spusťte příslušný příkaz.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Tři soubory budou přidány do projektu v části **migrace** directory:

* **00000000000000_InitialCreate.cs**– soubor hlavní migrace. Obsahuje nezbytné uplatňovat migrace operace (v `Up()`) a vraťte se zpátky (v `Down()`).
* **00000000000000_InitialCreate.Designer.cs**– soubor metadat migrace. Obsahuje informace o používané EF.
* **MyContextModelSnapshot.cs**– snímek aktuální model. Použít k určení, co se změnilo při přidávání pro další migraci.

Časové razítko v názvu souboru pomáhá udržovat seřadit časovém pořadí, abyste viděli průběh změny.

> [!TIP]
> Jste volné k přesunutí souborů migrace a změnit jejich obor názvů. Nové migrace jsou vytvořené jako poslední migrace na stejné úrovni.

Dále platí migrace pro databázi vytvořit schéma.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Přidání další migraci
------------------------
Po provedení změn pro svůj model EF jádra, bude synchronizován schématu databáze. A převeďte ho do aktuální, přidejte další migraci. Název migrace slouží jako potvrzení zprávy v systému správy verzí. Například pokud byly provedeny změny uložte zákazníka recenzemi produktů, I může zvolit něco podobného jako *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Po migraci je generované uživatelské rozhraní, zkontrolujte přesnost a přidejte jakékoli další operace vyžaduje správně používat. Migrace může například obsahovat následující operace:

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

Při těchto operací upravit schéma databáze kompatibilní, neuchovají se stávajícími názvy zákazníka. Chcete-li lépe, přepište ho následujícím způsobem.

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
> Přidání nové migrace upozorní, když je vygeneroval operace, které může způsobit ztrátu dat (např. vyřazení sloupce). Ujistěte se, že zejména Zkontrolujte tyto migrace přesnost.

Migrace se vztahují na databázi pomocí příslušný příkaz.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Odebrání migrace
--------------------
Někdy přidejte migrace a Uvědomte si, že budete muset udělat další změny modelu EF základní před každým jejím použitím.
Chcete-li odebrat poslední migrace, použijte tento příkaz.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po odebrání, můžete změnit další modelu a ji znovu přidejte.

<a name="reverting-a-migration"></a>Vrácení migrace
---------------------
Pokud jste již nainstalovali migrace (nebo několik migrace) do databáze, ale potřeba vraťte se zpátky, můžete stejný příkaz použijte migrace, ale zadejte název migrace, které chcete vrátit zpět.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Prázdný migrace
----------------
Někdy je užitečné pro přidání migrace bez jakýchkoli změn modelu. V takovém případě přidávání nové migrace vytvoří některého prázdný. Tato migrace k provádění operací, které přímo nesouvisejí model EF základní můžete přizpůsobit.
Některé věci, které můžete chtít spravovat tímto způsobem jsou:

* Fulltextové vyhledávání
* Funkce
* Uložené procedury
* Aktivační procedury
* zobrazení
* Atd.

<a name="generating-a-sql-script"></a>Generování skriptu SQL
-----------------------
Při ladění vaše migrace nebo jejich nasazení do provozní databáze, je užitečné pro generování skriptu SQL. Skript můžete mít další zkontrolovat přesnost a přizpůsobená podle potřeb z provozní databáze. Skript lze také použít ve spojení s technologií nasazení. Příkaz základní je následující.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Existuje několik možností pro tento příkaz.

**z** migrace by měl být poslední migrace do databáze použít před spuštěním skriptu. Pokud se neaplikovaly žádné migrace, zadejte `0` (Toto je výchozí hodnota).

**k** migrace je poslední migrace, která bude použita k databázi, po spuštění skriptu. Toto výchozí nastavení na poslední migrace ve vašem projektu.

**Idempotent** skript může volitelně generovat. Tento skript migrací platí pouze, pokud již nebyla použita k databázi. To je užitečné, pokud neznáte přesně co do databáze použít poslední migrace byla nebo Pokud nasazujete více databází, které může být v různých migrace.

<a name="applying-migrations-at-runtime"></a>Použití migrace za běhu
------------------------------
Některé aplikace může chtít použít migrace za běhu při spuštění nebo prvním spuštění. To provést pomocí `Migrate()` metoda.

Upozornění: Tento přístup není pro všechny uživatele. I když je skvělá pro aplikace s místní databází, bude vyžadovat většinu aplikací robustnější strategii nasazení, jako je generování skriptů SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Nemůžete volat `EnsureCreated()` před `Migrate()`. `EnsureCreated()` obchází migrace vytvořit schéma, což způsobí, že `Migrate()` selhání.

> [!NOTE]
> Tato metoda vytvoří na `IMigrator` služby, která lze použít pro pokročilejší scénáře. Použití `DbContext.GetService<IMigrator>()` k přístupu.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
