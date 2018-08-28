---
title: Správa schémat databází – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994382"
---
# <a name="managing-database-schemas"></a>Správa schémat databází
EF Core poskytuje dvěma základními způsoby udržování EF Core modelu a databázové schéma synchronizace. K výběru mezi těmito dvěma, rozhodněte, zda modelu EF Core nebo schéma databáze je zdroj informací.

Pokud chcete modelu EF Core zdroj pravdivých informací, použijte [migrace][1]. Při provádění změn do modelu EF Core, postupně vztahuje se odpovídající změny schématu na vaši databázi, aby zůstala kompatibilní s modelem EF Core.

Použití [Reverse Engineering] [ 2] Pokud chcete schéma databáze se zdroji pravdivých informací. Tento přístup umožňuje generování uživatelského rozhraní DbContext a třídy typu entity ve zpětné analýze schématu databáze do modelu EF Core.

> [!NOTE]
> [Vytvoření a přemístění rozhraní API] [ 3] můžete také vytvořit schéma databáze z modelu EF Core. Nicméně jsou primárně pro testování, vytváření prototypů a další scénáře, kde vyřazením databáze je přijatelné.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
