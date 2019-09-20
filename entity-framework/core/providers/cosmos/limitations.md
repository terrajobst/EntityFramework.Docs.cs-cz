---
title: Omezení poskytovatele Azure Cosmos DB – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150807"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Omezení poskytovatele EF Core Azure Cosmos DB

Poskytovatel Cosmos má několik omezení. Mnohé z těchto omezení jsou výsledkem omezení v podkladovém databázovém stroji Cosmos a nejsou specifické pro EF. Ale většina se [ještě neimplementovala](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Dočasná omezení

- I v případě, že existuje pouze jeden typ entity bez dědění dědičnosti na kontejner, má stále vlastnost diskriminátor.
- Typy entit s klíči oddílů v některých scénářích nefungují správně.
- `Include`volání nejsou podporovaná.
- `Join`volání nejsou podporovaná.

## <a name="azure-cosmos-db-sdk-limitations"></a>Omezení Azure Cosmos DB SDK

- Jsou k dispozici pouze asynchronní metody.

> [!WARNING]
> Vzhledem k tomu, že nejsou k dispozici žádné verze synchronizace metod nízké úrovně EF Core spoléhá na, jsou aktuálně implementovány `.Wait()` odpovídající funkce voláním vráceného. `Task` To znamená, že použití metod `SaveChanges`jako nebo `ToList` namísto jejich asynchronních protějšků by mohlo vést k zablokování aplikace

## <a name="azure-cosmos-db-limitations"></a>Omezení Azure Cosmos DB

Můžete si prohlédnout úplný přehled [Azure Cosmos DB podporovaných funkcí](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), jedná se o nejdůležitější rozdíly oproti relační databázi:

- Transakce iniciované klientem nejsou podporovány.
- Některé dotazy mezi oddíly nejsou v závislosti na závislých operátorech podporovány nebo mnohem pomalejší.