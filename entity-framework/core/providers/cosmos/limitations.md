---
title: Omezení poskytovatele Azure Cosmos DB – EF Core
description: Omezení poskytovatele Entity Framework Core Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417207"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Omezení poskytovatele EF Core Azure Cosmos DB

Poskytovatel Cosmos má několik omezení. Mnohé z těchto omezení jsou výsledkem omezení v podkladovém databázovém stroji Cosmos a nejsou specifické pro EF. Ale většina se [ještě neimplementovala](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Dočasná omezení

- I v případě, že existuje pouze jeden typ entity bez dědění dědičnosti na kontejner, má stále vlastnost diskriminátor.
- Typy entit s klíči oddílů v některých scénářích nefungují správně.
- `Include` volání nejsou podporovaná.
- `Join` volání nejsou podporovaná.

## <a name="azure-cosmos-db-sdk-limitations"></a>Omezení Azure Cosmos DB SDK

- Jsou k dispozici pouze asynchronní metody.

> [!WARNING]
> Vzhledem k tomu, že nejsou k dispozici žádné verze synchronizace metod nízké úrovně EF Core spoléhá na, jsou aktuálně implementovány odpovídající funkce voláním `.Wait()` na vrácené `Task`. To znamená, že použití metod jako `SaveChanges`nebo `ToList` namísto jejich asynchronních protějšků by mohlo vést k zablokování vaší aplikace

## <a name="azure-cosmos-db-limitations"></a>Omezení Azure Cosmos DB

Můžete si prohlédnout úplný přehled [Azure Cosmos DB podporovaných funkcí](/azure/cosmos-db/modeling-data), jedná se o nejdůležitější rozdíly oproti relační databázi:

- Transakce iniciované klientem nejsou podporovány.
- Některé dotazy mezi oddíly nejsou v závislosti na závislých operátorech podporovány nebo mnohem pomalejší.
