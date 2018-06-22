---
title: Co je nového v EF základní 1.1 - EF jádra
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054286"
---
# <a name="new-features-in-ef-core-11"></a>Nové funkce v EF základní 1.1

## <a name="modelling"></a>Modelování
### <a name="field-mapping"></a>Mapování polí
Umožňuje nakonfigurovat základní pole pro vlastnost. To může být užitečné pro vlastnosti jen pro čtení nebo data, která má metody Get/Set než vlastnost.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapování pro paměťově optimalizované tabulky v systému SQL Server
Můžete určit, zda je v tabulce, entita je namapovaná na paměťově optimalizované. Při použití EF jádra pro vytváření a údržbu databáze založené na modelu (buď pomocí migrace nebo `Database.EnsureCreated()`), vytvoří se paměťově optimalizované tabulky pro tyto entity.

## <a name="change-tracking"></a>sledování změn
### <a name="additional-change-tracking-apis-from-ef6"></a>Sledování rozhraní API z EF6 dalších změn.
Například `Reload`, `GetModifiedProperties`, `GetDatabaseValues` atd.

## <a name="query"></a>Dotazy
### <a name="explicit-loading"></a>Explicitní načítání
Umožňuje aktivovat naplnění navigační vlastnost u entity, která byla dříve načtena z databáze.
### <a name="dbsetfind"></a>DbSet.Find
Poskytuje snadný způsob, jak načíst podle jeho hodnotu primárního klíče entity.

## <a name="other"></a>Ostatní
### <a name="connection-resiliency"></a>Odolnost připojení
Opakované pokusy byly neúspěšné automaticky příkazy databáze. To je obzvláště užitečná při připojení k Azure SQL, které jsou společné přechodných chyb.
### <a name="simplified-service-replacement"></a>Zjednodušená služba nahrazení
Usnadňuje nahradit interních služeb, které používá EF.
