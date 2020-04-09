---
title: Co je nového v EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417515"
---
# <a name="new-features-in-ef-core-11"></a>Nové funkce v EF Core 1.1

## <a name="modeling"></a>Modelování

### <a name="field-mapping"></a>Mapování pole

Umožňuje nakonfigurovat záložní pole pro vlastnost. To může být užitečné pro vlastnosti jen pro čtení nebo data, která má Get/Set metody spíše než vlastnost.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapování na tabulky optimalizované pro paměť na serveru SQL Server

Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť. Při použití EF Core vytvořit a udržovat databázi na základě `Database.EnsureCreated()`modelu (buď s migrací nebo ), bude vytvořena tabulka optimalizovaná pro paměť pro tyto entity.

## <a name="change-tracking"></a>Sledování změn

### <a name="additional-change-tracking-apis-from-ef6"></a>Další api pro sledování změn z EF6

Jako `Reload`je `GetModifiedProperties` `GetDatabaseValues` , , atd.

## <a name="query"></a>Dotaz

### <a name="explicit-loading"></a>Explicitní načítání

Umožňuje aktivovat plnění vlastnosti navigace na entitu, která byla dříve načtena z databáze.

### <a name="dbsetfind"></a>DbSet.Najít

Poskytuje snadný způsob, jak načíst entitu na základě její hodnoty primárního klíče.

## <a name="other"></a>Ostatní

### <a name="connection-resiliency"></a>Odolnost připojení

Automaticky opakuje neúspěšné příkazy databáze. To je užitečné zejména při připojení k SQL Azure, kde jsou běžné přechodné chyby.

### <a name="simplified-service-replacement"></a>Zjednodušená náhrada služby

Usnadňuje nahrazení interních služeb, které používá EF.
