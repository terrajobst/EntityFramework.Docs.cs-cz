---
title: Co je nového v EF Core 1,1-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656016"
---
# <a name="new-features-in-ef-core-11"></a>Nové funkce v EF Core 1,1

## <a name="modeling"></a>Modelování

### <a name="field-mapping"></a>Mapování polí

Umožňuje konfigurovat pole pro zálohování pro vlastnost. To může být užitečné pro vlastnosti, které jsou jen pro čtení, nebo data, která mají metody get/set spíše než vlastnost.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapování na paměťově optimalizované tabulky v SQL Server

Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť. Při použití EF Core k vytvoření a údržbě databáze založené na modelu (buď s migrací nebo `Database.EnsureCreated()`) se pro tyto entity vytvoří paměťově optimalizovaná tabulka.

## <a name="change-tracking"></a>Sledování změn

### <a name="additional-change-tracking-apis-from-ef6"></a>Další rozhraní API pro sledování změn z EF6

Například `Reload`, `GetModifiedProperties``GetDatabaseValues` atd.

## <a name="query"></a>Dotazy

### <a name="explicit-loading"></a>Explicitní načítání

Umožňuje aktivovat naplnění vlastnosti navigace u entity, která byla dříve načtena z databáze.

### <a name="dbsetfind"></a>Negenerickými. Find

Poskytuje snadný způsob, jak načíst entitu na základě její hodnoty primárního klíče.

## <a name="other"></a>Ostatní

### <a name="connection-resiliency"></a>Odolnost připojení

Automaticky opakuje neúspěšné příkazy databáze. To je užitečné hlavně v případě připojení k SQL Azure, kde jsou časté přechodné chyby.

### <a name="simplified-service-replacement"></a>Zjednodušené nahrazení služby

Usnadňuje nahrazení interních služeb, které používá EF.
