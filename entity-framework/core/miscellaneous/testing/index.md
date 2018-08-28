---
title: Testovat komponenty pomocí Entity Frameworku – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997834"
---
# <a name="testing"></a>Testování

Můžete testovat komponenty pomocí nějakého nástroje, které se blíží připojení k databázi skutečné bez režie skutečné databáze vstupně-výstupních operací.

Jak to udělat dvěma způsoby:
 * [Režimu in-memory SQLite](sqlite.md) díky kterému lze vytvářet efektivní testování poskytovatele, který se chová jako relační databáze.
 * [Zprostředkovatel InMemory](in-memory.md) je jednoduchý zprostředkovatel, který má minimální závislosti, ale vždy se nechová stejně jako relační databáze.
