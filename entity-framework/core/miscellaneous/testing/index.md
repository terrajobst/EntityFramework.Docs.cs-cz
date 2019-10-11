---
title: Testování komponent pomocí EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181314"
---
# <a name="testing-components-using-ef-core"></a>Testování součástí pomocí EF Core

Můžete chtít testovat komponenty pomocí něčeho, co se blíží skutečnému připojení k reálné databázi bez režie skutečných vstupně-výstupních operací databáze.

K dispozici jsou dvě hlavní možnosti:
 * [V režimu v paměti](sqlite.md) je možné zapisovat efektivní testy pro poskytovatele, který se chová jako relační databáze.
 * [Poskytovatel inMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale nemusí se vždy chovat jako relační databáze.
