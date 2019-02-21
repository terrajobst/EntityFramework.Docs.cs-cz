---
title: Odolnost připojení – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 6d8cf117dfd94524a53e10bb4a23c2a44c4c8e7b
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459169"
---
# <a name="connection-resiliency"></a>Odolnost připojení

Odolnost připojení automaticky opakuje neúspěšné databázových příkazů. Funkci lze použít s libovolnou databází zadáním "strategie provádění", která zapouzdří logiku potřebnou pro zjišťování chyb a opakování příkazů. EF Core poskytovatelé může poskytovat přizpůsobené jejich konkrétní databázi podmínky při selhání a zásady opakování optimální strategií provádění.

Jako příklad zprostředkovatele SQL Server obsahuje strategie provádění, která je specificky přizpůsobit, aby SQL Server (včetně SQL Azure). Zná typy výjimek, které umožňují opakovaný pokus a má účelné výchozí hodnoty pro maximální počet opakovaných pokusů, zpoždění mezi opakovanými pokusy apod.

Strategie provádění je zadaný při konfiguraci možnosti pro váš kontext. Toto je obvykle v `OnConfiguring` metoda odvozené kontextu:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

nebo v `Startup.cs` pro aplikace ASP.NET Core:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>Strategie provádění vlastní

Je mechanismus pro registraci strategie vlastní provádění vlastní, pokud chcete změnit výchozí hodnoty.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Strategie provádění a transakce

Strategie provádění, která automaticky opakovat při selhání musí být schopný k přehrání každou operaci v bloku opakování, které se nedaří. Při opakování jsou povolené, bude každá operace, které můžete provádět prostřednictvím EF Core vyvolaly provozu. To znamená, že každý dotaz a každé volání `SaveChanges()` bude zopakován jako celek, pokud dojde k přechodnému selhání.

Nicméně pokud váš kód zahájí transakci pomocí `BeginTransaction()` definujete vlastní skupinu operací, které je potřeba za jednotku považuje a všechno, co je uvnitř transakce bude nutné je znovu přehrát musí dojít k selhání. Při pokusu provést při použití strategie provádění obdržíte výjimku podobný tomuto:

> InvalidOperationException: Strategie provádění nakonfigurované "SqlServerRetryingExecutionStrategy" nepodporuje transakce iniciované uživatelem. Použití strategie provádění vrácený "DbContext.Database.CreateExecutionStrategy()" provádět všechny operace v transakci jako vyvolaly jednotky.

Řešením je vyvolat s všechno, co představuje delegáta, který je nutné spustit ručně strategie provádění. Pokud dojde k přechodnému selhání, strategie provádění znovu vyvolá delegáta.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

Tento přístup můžete použít také s okolí transakce.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Chyba při potvrzení transakce a problém idempotence

Obecně platí Pokud dojde k selhání připojení aktuální transakce je vrácena zpět. Nicméně pokud připojení se ukončí, když je transakce vrácení potvrzené výsledný stav transakce není znám. Najdete v tomto [blogový příspěvek](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) další podrobnosti.

Ve výchozím nastavení, strategie provádění opakování operace, jako když transakce byla vrácena zpět, ale pokud se nejedná o případ výsledkem bude výjimky Pokud nový stav databáze není kompatibilní nebo může vést k **poškození dat** Pokud operace není závislý na konkrétní stav, například při vkládání nový řádek s automaticky generované hodnoty klíče.

Chcete-li to vyřešit několika způsoby.

### <a name="option-1---do-almost-nothing"></a>Možnost 1 - proveďte (téměř) nic

Pravděpodobnost selhání připojení během zápisu transakce tak může být přijatelný pro vaše aplikace právě selhat, pokud dojde k tomuto stavu dochází.

Musíte ale Vyhněte se použití klíče generované úložištěm, aby se zajistilo, že místo přidání duplicitní řádek je vyvolána výjimka. Zvažte použití klientem generovaná hodnota GUID nebo generátor hodnot na straně klienta.

### <a name="option-2---rebuild-application-state"></a>Možnost 2 - stavu opětovné sestavení aplikace

1. Zrušit aktuální `DbContext`.
2. Vytvořte nový `DbContext` a obnovení stavu aplikace z databáze.
3. Informujte uživatele, že poslední operaci možná nebyly úspěšně dokončeny.

### <a name="option-3---add-state-verification"></a>Možnost 3 – Přidání ověření stavu

Pro většinu operací, které mění stav databáze je možné přidat kód, který kontroluje, zda byla úspěšná. Poskytuje metody rozšíření pro lepší pochopení – EF `IExecutionStrategy.ExecuteInTransaction`.

Tato metoda začíná a potvrzení transakce a přijímá také funkci `verifySucceeded` parametr, který je voláno, když dojde k přechodné chybě během zápisu transakce.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Tady `SaveChanges` vyvolání `acceptAllChangesOnSuccess` nastavena na `false` chcete vyhnout změně stavu `Blog` entitu, kterou chcete `Unchanged` Pokud `SaveChanges` proběhne úspěšně. Díky tomu opakování stejné operace potvrzení nezdaří a transakce je vrácena zpět.

### <a name="option-4---manually-track-the-transaction"></a>Možnost 4 - manuálně sledovat transakce

Pokud chcete použít klíče generované úložištěm nebo potřebujete obecný způsob zpracování selhání potvrzení změn, které nejsou závislé na operace prováděné každou transakci, může přiřadit ID, které je zaškrtnuté políčko, pokud se potvrzení nezdaří.

1. Přidáte tabulku do databáze, která slouží ke sledování stavu transakce.
2. Vložte řádek do tabulky na začátku každé transakci.
3. Pokud se nepovede při potvrzení změn, zkontrolujte přítomnost odpovídající řádek v databázi.
4. Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, aby se zabránilo růst v tabulce.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Ujistěte se, že má kontext použitý pro ověření strategie provádění definován tak, že připojení je pravděpodobně znovu během ověřování selže, pokud došlo k selhání během zápisu transakce.
