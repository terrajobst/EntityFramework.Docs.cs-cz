---
title: Testování součástí pomocí EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197489"
---
# <a name="testing-components-using-ef-core"></a>Testování součástí pomocí EF Core

Můžete chtít testovat komponenty pomocí něčeho, co se blíží skutečnému připojení k reálné databázi bez režie skutečných vstupně-výstupních operací databáze.

K dispozici jsou dvě hlavní možnosti:
 * [V režimu v paměti](sqlite.md) je možné zapisovat efektivní testy pro poskytovatele, který se chová jako relační databáze.
 * [Poskytovatel inMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale nemusí se vždy chovat jako relační databáze.
