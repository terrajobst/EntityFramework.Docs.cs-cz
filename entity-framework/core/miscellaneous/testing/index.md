---
title: Testování komponent pomocí EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416561"
---
# <a name="testing-components-using-ef-core"></a>Testování součástí pomocí EF Core

Můžete chtít testovat komponenty pomocí něčeho, co se blíží skutečnému připojení k reálné databázi bez režie skutečných vstupně-výstupních operací databáze.

K dispozici jsou dvě hlavní možnosti:

* [V režimu v paměti](sqlite.md) je možné zapisovat efektivní testy pro poskytovatele, který se chová jako relační databáze.
* [Poskytovatel inMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale nemusí se vždy chovat jako relační databáze.
