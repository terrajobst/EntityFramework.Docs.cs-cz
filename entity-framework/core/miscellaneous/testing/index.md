---
title: Test komponent pomocí rozhraní Entity Framework - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054205"
---
# <a name="testing"></a>Testování

Můžete otestovat komponent pomocí něco, co blíží připojení k databázi skutečné bez režie skutečné databáze vstupně-výstupních operací.

Existují dvě hlavní možnosti tohoto postupu:
 * [Režim v paměti SQLite](sqlite.md) umožňuje psát efektivní testů proti zprostředkovatele, který se chová jako relační databáze.
 * [Zprostředkovatel InMemory](in-memory.md) je lightweight zprostředkovatele, který má minimální závislosti, ale není vždy chovat jako relační databáze.
