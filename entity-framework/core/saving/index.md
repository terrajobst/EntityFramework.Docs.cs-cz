---
title: Ukládání dat – jádro EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417548"
---
# <a name="saving-data"></a>Ukládání dat

Každá instance kontextu `ChangeTracker` má, který je zodpovědný za sledování změn, které je třeba zapsat do databáze. Při provádění změn instancí tříd entity jsou tyto `ChangeTracker` změny zaznamenány v databázi `SaveChanges`a poté zapsány do databáze při volání . Poskytovatel databáze je zodpovědný za překlad změn do operací specifických `INSERT` `UPDATE`pro `DELETE` databázi (například , a příkazy pro relační databázi).
