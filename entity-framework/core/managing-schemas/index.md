---
title: Správa schémat databáze – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655649"
---
# <a name="managing-database-schemas"></a>Správa schémat databází

EF Core poskytuje dva hlavní způsoby udržování modelu EF Core a schématu databáze v synchronizaci. Pokud si chcete vybrat mezi těmito dvěma možnostmi, rozhodněte se, jestli je model EF Core nebo schéma databáze zdrojem pravdy.

Pokud chcete, aby byl model EF Core zdrojem pravdy, použijte [migrace][1]. Při provádění změn v modelu EF Core tento postup přírůstkově aplikuje odpovídající změny schématu do vaší databáze tak, aby zůstal kompatibilní s modelem EF Core.

Pokud chcete, aby bylo vaše schéma databáze zdrojem pravdy, použijte [zpětný technický vývoj][2] . Tento přístup vám umožní vytvořit si třídu typu DbContext a třídy typů entit, a to tak, že vaše schéma databáze vrátí zpět do modelu EF Core.

> [!NOTE]
> [Rozhraní API pro vytváření a vyřazení][3] můžou také vytvořit schéma databáze z modelu EF Core. Jsou ale primárně určené pro testování, vytváření prototypů a další scénáře, kdy je odstranění databáze přijatelné.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
