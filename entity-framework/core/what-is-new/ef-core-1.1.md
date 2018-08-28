---
title: Novinky v EF Core 1.1 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995432"
---
# <a name="new-features-in-ef-core-11"></a>Novinky v EF Core 1.1

## <a name="modelling"></a>Modelování
### <a name="field-mapping"></a>Mapování polí
Umožňuje nakonfigurovat pomocné pole pro vlastnost. To může být užitečné pro vlastnosti jen pro čtení nebo data, která má metody Get/Set, nikoli vlastnosti.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapování na paměťově optimalizovaných tabulek v SQL serveru
Můžete určit, že je tabulka entita je namapována na paměťově optimalizovaná. Když pomocí EF Core pro vytváření a údržbu databáze na základě modelu (pomocí migrace nebo `Database.EnsureCreated()`), paměťově optimalizované tabulky se vytvoří pro tyto entity.

## <a name="change-tracking"></a>Sledování změn
### <a name="additional-change-tracking-apis-from-ef6"></a>Další řešení change tracking rozhraní API z EF6
Například `Reload`, `GetModifiedProperties`, `GetDatabaseValues` atd.

## <a name="query"></a>Dotazy
### <a name="explicit-loading"></a>Explicitní načtení
Umožňuje aktivovat naplnění navigační vlastnost s entitou, která byla dříve načtena z databáze.
### <a name="dbsetfind"></a>DbSet.Find
Poskytuje snadný způsob, jak načíst podle jeho hodnotu primárního klíče entity.

## <a name="other"></a>Ostatní
### <a name="connection-resiliency"></a>Odolnost připojení
Automatické opakované pokusy byly neúspěšné databázových příkazů. To je užitečné hlavně při připojení k SQL Azure, kde jsou běžné přechodná selhání.
### <a name="simplified-service-replacement"></a>Zjednodušená služba nahrazení
Usnadňuje nahradit interních služeb, které EF používá.
