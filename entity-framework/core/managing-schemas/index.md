---
title: Správa schémat databáze – základní ef
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416305"
---
# <a name="managing-database-schemas"></a>Správa schémat databází

EF Core poskytuje dva primární způsoby synchronizace modelu EF Core a schématu databáze. Chcete-li si vybrat mezi těmito dvěma, rozhodněte se, zda je zdrojem pravdy model EF Core nebo schéma databáze.

Pokud chcete, aby byl zdroj pravdy model EF Core, použijte [migrace][1]. Při provádění změn modelu EF Core tento přístup postupně použije odpovídající změny schématu do databáze tak, aby zůstalkompatibilní s modelem EF Core.

Pokud chcete, aby bylo zdrojem pravdy schéma databáze, použijte [reverzní inženýrství.][2] Tento přístup umožňuje sešité číslo DbContext a třídy typu entity pomocí zpětného inženýrství schéma databáze do modelu EF Core.

> [!NOTE]
> Vytvoření [a přetažení api][3] můžete také vytvořit schéma databáze z modelu EF Core. Jsou však především pro testování, prototypování a další scénáře, kde je přijatelné uvolnění databáze.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
