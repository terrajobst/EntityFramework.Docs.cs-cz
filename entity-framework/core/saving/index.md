---
title: Ukládání dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998177"
---
# <a name="saving-data"></a>Ukládání dat

Každá instance kontextu má `ChangeTracker` , která zodpovídá za udržování přehledu o změny, které musí být zapsána do databáze. Při provádění změn do instance třídy entity, tyto změny se zaznamenávají do `ChangeTracker` a následně zapsána do databáze při volání `SaveChanges`. Poskytovatel databáze je zodpovědný za převod změny do konkrétních databází operations (například `INSERT`, `UPDATE`, a `DELETE` příkazy pro relační databáze).
