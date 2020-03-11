---
title: Odolnost připojení – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416639"
---
# <a name="connection-resiliency"></a>Odolnost připojení

Odolnost připojení automaticky opakuje neúspěšné příkazy databáze. Funkci lze použít s libovolnou databází poskytnutím "strategie provádění", která zapouzdřuje logiku potřebnou k detekci selhání a opakování příkazů. Poskytovatelé EF Core můžou poskytovat strategie spouštění přizpůsobené konkrétním stavům selhání databáze a optimálním zásadám opakování.

Například poskytovatel SQL Server zahrnuje strategii provádění, která je určena speciálně pro SQL Server (včetně SQL Azure). Informace o typech výjimek, které se dají opakovat, a má rozumné výchozí hodnoty pro maximální počet opakování, zpoždění mezi opakovanými pokusy atd.

Při konfiguraci možností pro váš kontext je určena strategie provádění. Obvykle se jedná o metodu `OnConfiguring` vašeho odvozeného kontextu:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

nebo v `Startup.cs` pro aplikaci ASP.NET Core:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>Vlastní strategie provádění

Existuje mechanismus, jak zaregistrovat vlastní strategii spouštění, pokud chcete změnit některou z výchozích hodnot.

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

Strategie provádění, která se automaticky opakuje při selhání, musí být schopná přehrát všechny operace v bloku opakování, který se nezdaří. Když jsou povoleny opakované pokusy, každá operace, kterou provedete prostřednictvím EF Core, se stal svou vlastní operací vyvolaly. To znamená, že každý dotaz a každé volání `SaveChanges()` se bude opakovat jako jednotka, pokud dojde k přechodnému selhání.

Nicméně pokud váš kód inicializuje transakci pomocí `BeginTransaction()` definujete vlastní skupinu operací, které je třeba považovat za jednotku, a všechno, co se v transakci musí přehrát, dojde k selhání. Pokud se pokusíte provést tuto operaci při použití strategie provádění, obdržíte výjimku podobnou následující:

> InvalidOperationException: nakonfigurovaná strategie provádění SqlServerRetryingExecutionStrategy nepodporuje transakce iniciované uživatelem. K provedení všech operací v transakci jako jednotky vyvolaly použijte strategii spuštění vrácenou funkcí DbContext. Database. CreateExecutionStrategy ().

Řešením je ručně vyvolat strategii spouštění s delegátem, který představuje všechno, co je třeba provést. Pokud dojde k přechodnému selhání, strategie provádění znovu vyvolá delegáta.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

Tento přístup lze také použít s ambientních transakcí.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Selhání potvrzení transakce a problém idempotence

Obecně platí, že pokud dojde k selhání připojení, aktuální transakce se vrátí zpět. Pokud je ale připojení vyřazeno, zatímco probíhá potvrzení transakce, výsledný stav transakce není známý. 

Ve výchozím nastavení strategie spouštění zopakuje operaci, jako kdyby byla transakce vrácena zpět, ale pokud to ale není, bude výsledkem výjimka, pokud je stav nového databáze nekompatibilní nebo může způsobit **poškození dat** , pokud se operace nespoléhá na určitý stav, například při vložení nového řádku s automaticky generovanými hodnotami klíče.

Existuje několik způsobů, jak s nimi pracovat.

### <a name="option-1---do-almost-nothing"></a>Možnost 1 – do (téměř) nic

Pravděpodobnost selhání připojení během potvrzení transakce je nízká, takže může být přijatelné, aby vaše aplikace v případě, že k této situaci skutečně dojde, nedošlo k chybě.

Je však nutné se vyhnout použití klíčů generovaných úložištěm, aby bylo zajištěno, že bude vyvolána výjimka namísto přidání duplicitního řádku. Zvažte použití hodnoty GUID generované klientem nebo generátoru hodnot na straně klienta.

### <a name="option-2---rebuild-application-state"></a>Možnost 2 – opětovné sestavení stavu aplikace

1. Zahodí aktuální `DbContext`.
2. Vytvořte novou `DbContext` a obnovte stav aplikace z databáze.
3. Informujte uživatele, že poslední operace nemusí být úspěšně dokončena.

### <a name="option-3---add-state-verification"></a>Možnost 3 – Přidání ověření stavu

Pro většinu operací, které mění stav databáze je možné přidat kód, který kontroluje, zda bylo úspěšné. EF poskytuje metodu rozšíření, která usnadňuje `IExecutionStrategy.ExecuteInTransaction`.

Tato metoda začíná a potvrdí transakci a také přijímá funkci v parametru `verifySucceeded`, který je vyvolán, když dojde k přechodné chybě během zápisu transakce.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Zde je `SaveChanges` vyvolána s `acceptAllChangesOnSuccess` nastavenou na `false`, aby nedošlo ke změně stavu `Blog` entity na `Unchanged`, pokud `SaveChanges` úspěšné. To umožňuje opakovat stejnou operaci, pokud se potvrzení nezdařilo a transakce je vrácena zpět.

### <a name="option-4---manually-track-the-transaction"></a>Možnost 4 – ruční sledování transakce

Pokud potřebujete použít klíče generované úložištěm nebo potřebujete obecný způsob zpracování selhání potvrzení, které není závislé na operaci provedené jednotlivými transakce, může být přiřazeno ID, které je zaškrtnuto, když se potvrzení nezdaří.

1. Přidejte tabulku do databáze použité ke sledování stavu transakcí.
2. Vloží řádek do tabulky na začátku každé transakce.
3. Pokud během potvrzení dojde k chybě připojení, vyhledejte přítomnost odpovídajícího řádku v databázi.
4. Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, abyste se vyhnuli nárůstu tabulky.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Ujistěte se, že kontext použitý pro ověření má strategii spuštění definovanou v případě, že během ověřování dojde k chybě při potvrzení transakce.
