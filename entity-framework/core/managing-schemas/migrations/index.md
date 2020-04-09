---
title: Migrace – jádro EF
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136201"
---
# <a name="migrations"></a>Migrace

Datový model se během vývoje změní a synchronizuje se s databází. Můžete přetažení databáze a nechat EF vytvořit nový, který odpovídá modelu, ale tento postup má za následek ztrátu dat. Funkce migrace v EF Core poskytuje způsob, jak postupně aktualizovat schéma databáze, aby bylo synchronizováno s datovým modelem aplikace při zachování existujících dat v databázi.

Migrace zahrnují nástroje příkazového řádku a api, které pomáhají s následujícími úkoly:

* [Vytvořte migraci](#create-a-migration). Vygenerujte kód, který může aktualizovat databázi a synchronizovat ji se sadou změn modelu.
* [Aktualizujte databázi](#update-the-database). Chcete-li aktualizovat schéma databáze, použijte čekající migrace.
* [Přizpůsobení kódu migrace](#customize-migration-code). Někdy je třeba upravit nebo doplnit generovaný kód.
* [Odebrání migrace](#remove-a-migration). Odstraňte generovaný kód.
* [Vrátit migraci](#revert-a-migration). Vrátit zpět změny databáze.
* [Generovat skripty SQL](#generate-sql-scripts). Možná budete potřebovat skript k aktualizaci produkční databáze nebo k řešení potíží s kódem migrace.
* [Použití migrace za běhu](#apply-migrations-at-runtime). Pokud aktualizace návrhu a spuštěné skripty nejsou nejlepší `Migrate()` mise, zavolejte metodu.

> [!TIP]
> Pokud `DbContext` je v jiném sestavení než projekt spuštění, můžete explicitně zadat cílové a spouštěcí projekty v [nástrojích konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell#target-and-startup-project) nebo [v nástrojích rozhraní .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).

## <a name="install-the-tools"></a>Instalace nástrojů

Nainstalujte [nástroje příkazového řádku](xref:core/miscellaneous/cli/index):

* Pro Visual Studio doporučujeme [nástroje konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell).
* Pro ostatní vývojová prostředí zvolte [nástroje rozhraní .NET Core CLI](xref:core/miscellaneous/cli/dotnet).

## <a name="create-a-migration"></a>Vytvoření migrace

Po definování [počátečního modelu](xref:core/modeling/index)je čas vytvořit databázi. Chcete-li přidat počáteční migraci, spusťte následující příkaz.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

V adresáři **Migrace** jsou do projektu přidány tři soubory:

* **XXXXXXXXXXXXXX_InitialCreate.cs**--Hlavní soubor migrace. Obsahuje operace nezbytné k použití `Up()`migrace (v ) a `Down()`vrátit ji (v ).
* **XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--Soubor metadat migrace. Obsahuje informace používané EF.
* **MyContextModelSnapshot.cs**--Snímek aktuálního modelu. Slouží k určení, co se změnilo při přidávání další migrace.

Časové razítko v názvu souboru pomáhá udržet je uspořádány chronologicky, takže můžete vidět průběh změn.

> [!TIP]
> Můžete přesunout soubory migrace a změnit jejich obor názvů. Nové migrace jsou vytvořeny jako na stejné úrovni poslední migrace.

## <a name="update-the-database"></a>Aktualizace databáze

Dále použijte migraci do databáze k vytvoření schématu.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a>Přizpůsobení kódu migrace

Po provedení změn modelu EF Core je schéma databáze pravděpodobně nesynchronizované. Chcete-li ji aktualizovat, přidejte další migraci. Název migrace lze použít jako zprávu o potvrzení v systému správy verzí. Můžete například zvolit název jako *AddProductReviews,* pokud je změna novou třídou entity pro recenze.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

Jakmile je migrace šetrné k ráže (kód pro ni vygenerovaný), zkontrolujte kód pro přesnost a přidat, odebrat nebo upravit všechny operace potřebné k použití správně.

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

Zatímco tyto operace, aby schéma databáze kompatibilní, nezachovávají existující názvy zákazníků. Chcete-li, aby to bylo lepší, přepište to takto.

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
> Proces migrace lešení varuje, když operace může mít za následek ztrátu dat (jako je uvolnění sloupce). Pokud se zobrazí toto upozornění, nezapomeňte zkontrolovat kód migrace pro přesnost.

Použijte migraci do databáze pomocí příslušného příkazu.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a>Prázdné migrace

Někdy je užitečné přidat migraci bez provedení změn modelu. V tomto případě přidání nové migrace vytvoří soubory kódu s prázdnými třídami. Tuto migraci můžete přizpůsobit tak, aby prováděla operace, které přímo nesouvisejí s modelem EF Core. Některé věci, které byste mohli chtít spravovat tímto způsobem, jsou:

* Fulltextové vyhledávání
* Functions
* Uložené procedury
* Aktivační události
* Zobrazení

## <a name="remove-a-migration"></a>Odebrání migrace

Někdy přidáte migraci a uvědomíte si, že před použitím je třeba provést další změny modelu EF Core. Chcete-li odebrat poslední migraci, použijte tento příkaz.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Remove-Migration
```

***

Po odebrání migrace můžete provést další změny modelu a znovu jej přidat.

## <a name="revert-a-migration"></a>Vrácení migrace

Pokud jste již migraci (nebo několik migrací) do databáze použili, ale potřebujete ji vrátit, můžete použít stejný příkaz k aplikování migrace, ale zadejte název migrace, ke které chcete přejít zpět.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a>Generovat skripty SQL

Při ladění migrace nebo jejich nasazení do produkční databáze, je užitečné generovat skript SQL. Skript pak může být dále přezkoumána pro přesnost a vyladěny tak, aby vyhovovaly potřebám produkční databáze. Skript lze také použít ve spojení s technologií nasazení. Základní příkaz je následující.

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a>Základní použití
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a>S From (na implicitní)
Tím se vygeneruje skript SQL z této migrace na nejnovější migraci.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>S od a do
Tím se vygeneruje `from` skript SQL `to` z migrace na zadanou migraci.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Můžete `from` použít, který je novější `to` než pro generování skriptu vrácení zpět. *Vezměte prosím na vědomí potenciální scénáře ztráty dat.*

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

#### <a name="basic-usage"></a>Základní použití
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a>S From (na implicitní)
Tím se vygeneruje skript SQL z této migrace na nejnovější migraci.
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>S od a do
Tím se vygeneruje `from` skript SQL `to` z migrace na zadanou migraci.
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Můžete `from` použít, který je novější `to` než pro generování skriptu vrácení zpět. *Vezměte prosím na vědomí potenciální scénáře ztráty dat.*

***

Tento příkaz má několik možností.

**Z** migrace by měla být poslední migrace použitá v databázi před spuštěním skriptu. Pokud nebyly použity žádné `0` migrace, zadejte (toto je výchozí).

Migrace **na** je poslední migrace, která bude použita v databázi po spuštění skriptu. To to to to toto nastavení má být poslední migrace v projektu.

**Idempotentní** skript může být volitelně generován. Tento skript použije migraci pouze v případě, že ještě nebyly použity v databázi. To je užitečné, pokud přesně nevíte, jaká byla poslední migrace použitá v databázi, nebo pokud nasazujete do více databází, které mohou být při jiné migraci.

## <a name="apply-migrations-at-runtime"></a>Použití migrace za běhu

Některé aplikace mohou chtít použít migrace za běhu při spuštění nebo prvním spuštění. Proveďte to `Migrate()` metodou.

Tato metoda vychází z `IMigrator` nad služby, které lze použít pro pokročilejší scénáře. Používá `myDbContext.GetInfrastructure().GetService<IMigrator>()` se k přístupu.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * Tento přístup není pro každého. I když je to skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat robustnější strategii nasazení, jako je generování skriptů SQL.
> * Nevolejte `EnsureCreated()` dříve `Migrate()`. `EnsureCreated()`obchází migrace k vytvoření schématu, `Migrate()` který způsobí selhání.

## <a name="next-steps"></a>Další kroky

Další informace naleznete v tématu <xref:core/miscellaneous/cli/index>.
