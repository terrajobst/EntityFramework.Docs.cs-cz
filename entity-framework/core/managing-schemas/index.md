---
title: Správa databáze schémata - EF jádra
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054739"
---
# <a name="managing-database-schemas"></a>Správa schémat databáze
Základní EF poskytuje dva primární způsoby udržování schéma modelu a databázi EF základní synchronizace. Vybrat mezi nimi, rozhodnete, jestli váš model EF jádra nebo schéma databáze zdroj pravdivosti.

Pokud chcete, aby váš model EF základní jako zdroj pravdivosti, použijte [migrace][1]. Při provádění změn pro svůj model EF základní přírůstkově vztahuje se odpovídající změny schématu na vaší databáze, aby zůstala kompatibilní s EF základní modelu.

Použití [Reverse Engineering] [ 2] Pokud chcete, aby schéma databáze se stane zdroje pravdivosti. Tento přístup umožňuje vygenerovat konstruktoru DbContext a tříd entit, typ podle zpětnou analýzu svého schématu databáze do model EF jádra.

> [!NOTE]
> [Vytvořit a vyřadit rozhraní API] [ 3] můžete také vytvořit schéma databáze z modelu EF jádra. Jsou to ale především pro účely testování, při vytváření prototypu a další scénáře, kde vyřazením databáze je přijatelná.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
