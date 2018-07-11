---
title: Ukládání dat – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949392"
---
# <a name="saving-data"></a>Ukládání dat

Každá instance kontextu má `ChangeTracker` , která zodpovídá za udržování přehledu o změny, které musí být zapsána do databáze. Při provádění změn do instance třídy entity, tyto změny se zaznamenávají do `ChangeTracker` a následně zapsána do databáze při volání `SaveChanges`. Poskytovatel databáze je zodpovědný za převod změny do konkrétních databází operations (například `INSERT`, `UPDATE`, a `DELETE` příkazy pro relační databáze).
