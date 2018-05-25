---
title: Ukládání dat – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="saving-data"></a>Ukládání dat

Každá instance kontextu má `ChangeTracker` zodpovídá pro udržování přehledu o změny, které musí být zapsána do databáze. Jak provedete změny instance třídy entity, se zaznamenávají tyto změny `ChangeTracker` a pak zapsána do databáze při volání `SaveChanges`. Zprostředkovatel databáze je zodpovědná za překladu změn v operacích specifických pro databázi (například `INSERT`, `UPDATE`, a `DELETE` příkazy pro relační databázi).
