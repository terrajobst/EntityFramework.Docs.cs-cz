---
title: Ukládání dat – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417548"
---
# <a name="saving-data"></a>Ukládání dat

Každá instance kontextu má `ChangeTracker` zodpovědný za sledování změn, které je nutné zapsat do databáze. Při provádění změn instancí tříd entit jsou tyto změny zaznamenávány do `ChangeTracker` a následně zapsány do databáze při volání `SaveChanges`. Poskytovatel databáze zodpovídá za překlady změn do operací specifických pro databázi (například `INSERT`, `UPDATE`a `DELETE` příkazy pro relační databázi).
