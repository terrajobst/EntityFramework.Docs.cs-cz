---
title: "Odolnost připojení - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a>Odolnost připojení

Odolnost připojení automaticky opakuje příkazy databáze se nezdařilo. Funkci lze použít s libovolnou databází zadáním "provádění strategie", který zapouzdřuje logice potřeba rozpoznala chyby a opakujte příkazy. Hlavními zprostředkovateli EF může poskytovat strategie provádění přizpůsobené jejich podmínky při selhání konkrétní databázi a zásady optimální opakování.

Jako příklad obsahuje zprostředkovatele SQL Server provádění strategie, který je specificky přizpůsobit, aby SQL Server (včetně SQL Azure). Je vědět typů výjimek, které můžete zkusit znovu a má rozumný výchozí hodnoty pro maximální počet opakovaných pokusů, zpoždění mezi opakování atd.

Při konfiguraci možností pro váš kontext zadaná strategie provádění. To je obvykle v `OnConfiguring` metoda vaší odvozené kontextu, nebo v `Startup.cs` pro aplikaci ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Strategie vlastního provádění

Je mechanismus k registraci strategie vlastní provádění vlastní, pokud chcete změnit výchozí hodnoty.

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

Provádění strategii, která automaticky opakovat při selhání musí být schopni přehrání jednotlivých operací v bloku opakování, který selže. Pokud jsou povolené opakování, každou operaci provádět prostřednictvím EF základní stane vlastní nevyřeší operaci, tj. každý dotaz a každé volání `SaveChanges()` bude zopakován jako jednotku, pokud dojde k přechodné chybě.

Ale pokud váš kód zahájí transakce pomocí `BeginTransaction()` definujete vlastní skupiny operací, které musí být považovány za jednotku, tj. všechno uvnitř transakce muset být přehrání musí dojít k chybě. Zobrazí se výjimku takto a pokusíte se to udělat, když používá provádění strategie.

> InvalidOperationException: V nakonfigurované strategii provádění 'SqlServerRetryingExecutionStrategy' nepodporuje transakce inicializované uživatelem. Použijte strategii provádění vrácený 'DbContext.Database.CreateExecutionStrategy()' provádět všechny operace v rámci transakce jako nevyřeší jednotku.

Řešení je ručně vyvolání strategie provádění s představující všechno delegáta, který je třeba provést. Pokud dojde k přechodné chybě, bude strategii provádění znovu vyvolání delegáta.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Chyba při potvrzení transakce a idempotenci problému

Obecně platí když dojde k selhání připojení aktuální transakce je vrácena zpět. Ale pokud připojení se ukončí při transakce vrácení potvrdí výsledná stav transakce neznámý. Toto [příspěvku na blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) další podrobnosti.

Ve výchozím nastavení, strategii provádění bude opakovat operaci jako kdyby byla transakce vrácena zpět, ale pokud se nejedná o tento případ tato akce způsobí výjimku Pokud nový stav databáze není kompatibilní, nebo může vést k **poškození dat** Pokud operace není závislý na konkrétní stav, například při vkládání nový řádek s automaticky generované hodnoty klíče.

Existuje několik způsobů, jak nakládat s to.

### <a name="option-1---do-almost-nothing"></a>Možnost 1 - proveďte (téměř) nic

Pravděpodobnost selhání připojení během zápisu transakce je nízký, může být přijatelné pro vaši aplikaci právě nezdaří, pokud k tomuto stavu ve skutečnosti dochází.

Ale budete muset Vyhněte se použití klíče generované úložištěm, aby se zajistilo, že místo přidávání duplicitní řádek je vyvolána výjimka. Zvažte použití klientem generovaná hodnota GUID nebo generátor hodnoty na straně klienta.

### <a name="option-2---rebuild-application-state"></a>Možnost 2 – stav aplikace opětovné sestavení

1. Zrušit aktuální `DbContext`.
2. Vytvořte novou `DbContext` a obnovení stavu aplikace z databáze.
3. Informujte uživatele, že poslední operace nemusí mít byla úspěšně dokončena.

### <a name="option-3---add-state-verification"></a>Možnost 3 – Přidání ověření stavu

Pro většinu operací, které změní stav databáze je možné přidat kód, který kontroluje, zda byla úspěšná. EF poskytuje metody rozšíření pro zjednodušení - `IExecutionStrategy.ExecuteInTransaction`.

Tato metoda začne a potvrdí transakce a také přijímá funkce při `verifySucceeded` parametru, která je volána, když dojde k přechodné chybě. během zápisu transakce.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Zde `SaveChanges` vyvolání `acceptAllChangesOnSuccess` nastavena na `false` chcete vyhnout změně stavu `Blog` entity, která má `Unchanged` Pokud `SaveChanges` úspěšné. To umožňuje opakovat stejné operace, pokud se potvrzení nezdaří a transakce je vrácena zpět.

### <a name="option-4---manually-track-the-transaction"></a>Možnost 4 - ručně sledovat transakce

Pokud potřebujete použít klíče generované úložištěm nebo potřebujete obecné tak zpracovávat chyby potvrzení, že nezávisí na operaci provést, může být přiřazen ID, které je zaškrtnuto, pokud se potvrzení nezdaří každou transakci.

1. Přidání tabulky do databáze použít ke sledování stavu transakce.
2. Vložte řádek do tabulky na začátku každou transakci.
3. Pokud se nepovede připojit během potvrzení, zkontrolujte přítomnost odpovídající řádek v databázi.
4. Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, aby se zabránilo růst v tabulce.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Zajistěte, aby kontextu použít pro ověření strategie provádění definován jako je připojení, pravděpodobně selžou znovu během ověřování, pokud se nezdařilo během zápisu transakce.
