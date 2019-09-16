---
title: Migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: ffa9a34f13ab29f0ba93f9fd1f469398630604ce
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005449"
---
<a name="migrations"></a>Migrace
==========

Datový model se během vývoje mění a nesynchronizuje se s databází. Databázi můžete vyřadit a nechat EF vytvořit nové, které odpovídají modelu, ale tento postup vede ke ztrátě dat. Funkce migrace v EF Core poskytuje způsob, jak přírůstkově aktualizovat schéma databáze, aby se zachovala synchronizace s datovým modelem aplikace a současně zachovává stávající data v databázi.

Migrace zahrnuje nástroje příkazového řádku a rozhraní API, které vám pomůžou s následujícími úlohami:

* [Vytvořte migraci](#create-a-migration). Vygeneruje kód, který může aktualizovat databázi pro synchronizaci se sadou změn modelu.
* [Aktualizujte databázi](#update-the-database). Použijte nedokončené migrace k aktualizaci schématu databáze.
* [Přizpůsobení kódu migrace](#customize-migration-code). Někdy je nutné vygenerovaný kód upravit nebo doplnit.
* [Odeberte migraci](#remove-a-migration). Odstraňte generovaný kód.
* [Vrácení migrace zpět](#revert-a-migration) Vrátí zpět změny databáze.
* [Vygenerujte skripty SQL](#generate-sql-scripts). Je možné, že budete potřebovat skript pro aktualizaci provozní databáze nebo pro řešení potíží s kódem migrace.
* [Použijte migrace za běhu](#apply-migrations-at-runtime). Pokud nejsou dostupné aktualizace a spouštění skriptů v době návrhu, je vhodné zavolat `Migrate()` metodu.

> [!TIP]
> Pokud je v jiném sestavení než projekt po spuštění, můžete explicitně zadat cílové a spouštěné projekty buď v [nástrojích konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell#target-and-startup-project) , nebo v [nástroji .NET Core CLI.](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project) `DbContext`

<a name="install-the-tools"></a>Instalace nástrojů
-----------------

Instalace [nástrojů příkazového řádku](xref:core/miscellaneous/cli/index):
* Pro Visual Studio doporučujeme [Nástroje konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell).
* Pro jiná vývojová prostředí vyberte [nástroje .NET Core CLI](xref:core/miscellaneous/cli/dotnet).

<a name="create-a-migration"></a>Vytvoření migrace
------------------

Po [Definování počátečního modelu](xref:core/modeling/index)je čas vytvořit databázi. Chcete-li přidat počáteční migraci, spusťte následující příkaz.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Do projektu se v adresáři **migrace** přidají tři soubory:

* **XXXXXXXXXXXXXX_InitialCreate. cs**– hlavní soubor migrace. Obsahuje operace nutné k použití migrace (v `Up()`nástroji) a k jejímu vrácení (v `Down()`).
* **XXXXXXXXXXXXXX_InitialCreate. Designer. cs**– soubor metadat migrace. Obsahuje informace, které používá EF.
* **MyContextModelSnapshot.cs**– snímek aktuálního modelu. Slouží k určení toho, co se změnilo při přidávání další migrace.

Časové razítko v názvu souboru pomáhá udržet seřazené chronologicky, takže se můžete podívat na průběh změn.

> [!TIP]
> Můžete přesunout soubory migrace a změnit jejich obor názvů. Nové migrace se vytvoří jako na stejné úrovni jako poslední migrace.

<a name="update-the-database"></a>Aktualizace databáze
-------------------

V dalším kroku použijte migraci na databázi a vytvořte schéma.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a>Přizpůsobení kódu migrace
------------------------

Po provedení změn v modelu EF Core nemusí být schéma databáze synchronizované. Pokud ho chcete uvést v aktuálním stavu, přidejte další migraci. Název migrace lze použít jako zprávu potvrzení v systému správy verzí. Například můžete zvolit název jako *AddProductReviews* , pokud je změna novou třídou entity pro recenze.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Jakmile se migrace vygeneruje z vygenerovaného kódu (kód vygenerovaný pro něj), zkontrolujte přesnost kódu a přidejte, odeberte nebo upravte všechny operace, které jsou nutné k jejich použití.

Například migrace může obsahovat následující operace:

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

I když tyto operace zajistí kompatibilitu schématu databáze, nezachovají se stávající názvy zákazníků. Chcete-li to lepší, přepište ho následujícím způsobem.

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
> Proces generování uživatelského rozhraní se upozorní na to, že výsledkem operace může být ztráta dat (například vyřazení sloupce). Pokud se zobrazí toto upozornění, nezapomeňte si projít kód migrace, aby se zajistila přesnost.

Pomocí příslušného příkazu použijte migraci na databázi.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a>Prázdné migrace

Někdy je vhodné přidat migraci bez provedení jakýchkoli změn modelu. V takovém případě přidání nové migrace vytvoří soubory kódu s prázdnými třídami. Tuto migraci můžete přizpůsobit tak, aby prováděla operace, které přímo nesouvisejí s modelem EF Core. Tímto způsobem můžete chtít spravovat tyto věci:

* Fulltextové vyhledávání
* Funkce
* Uložené procedury
* Aktivační procedury
* Zobrazení

<a name="remove-a-migration"></a>Odebrání migrace
------------------
Někdy můžete přidat migraci a zajistěte, abyste před použitím v modelu EF Core provedli další změny. K odebrání poslední migrace použijte tento příkaz.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Po odebrání migrace můžete provést další změny modelu a znovu ho přidat.

<a name="revert-a-migration"></a>Vrácení migrace zpět
------------------
Pokud jste již v databázi použili migraci (nebo několik migrací), ale potřebujete je vrátit zpět, můžete použít stejný příkaz pro použití migrace, ale zadejte název migrace, na kterou se chcete vrátit.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a>Generování skriptů SQL
--------------------
Při ladění migrací nebo jejich nasazení do provozní databáze je užitečné vygenerovat skript SQL. Skript pak může být dále revidován pro přesnost a vyladěný tak, aby vyhovoval potřebám provozní databáze. Skript se dá použít i ve spojení s technologií nasazení. Základní příkaz je následující.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Tento příkaz obsahuje několik možností.

Migrace **z** migrace by měla být poslední migrací použitou pro databázi před spuštěním skriptu. Pokud žádné migrace nepoužíváte, zadejte `0` (Toto je výchozí nastavení).

Migrace **do** je poslední migrace, která se použije pro databázi po spuštění skriptu. Tato hodnota se nastaví jako poslední migrace v projektu.

Skript **idempotentní** může být volitelně vygenerován. Tento skript aplikuje migrace jenom v případě, že se v databázi ještě nepoužily. To je užitečné, Pokud nevíte přesně, co poslední migrace použila v databázi, nebo pokud nasazujete do více databází, které mohou být při jiné migraci.

<a name="apply-migrations-at-runtime"></a>Použít migrace za běhu
---------------------------
Některé aplikace můžou chtít při spuštění nebo prvním spuštění použít migrace za běhu. `Migrate()` Použijte metodu.

Tato metoda sestaví na `IMigrator` službě, která se dá použít pro pokročilejší scénáře. Použijte `myDbContext.GetInfrastructure().GetService<IMigrator>()` pro přístup k němu.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * Tento přístup není pro všechny. I když je skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat robustnější strategii nasazení, jako je generování skriptů SQL.
> * Nevolejte `EnsureCreated()` před `Migrate()`. `EnsureCreated()`vynechá migrace k vytvoření schématu, což způsobí `Migrate()` selhání.

<a name="next-steps"></a>Další postup
----------

Další informace naleznete v tématu <xref:core/miscellaneous/cli/index>.
