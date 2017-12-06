---
title: "Základní EF Detached.EntityFramework - nástrojů a rozšíření-"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: DE1C3FEA-F618-4C11-932F-7C392A1B2C94
ms.technology: entity-framework-core
uid: core/extensions/entityframeworkcore-detached
ms.openlocfilehash: 93937eee07a9e1a0b94bd92140faec7d1753332f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="efdetachedentityframework-extension"></a>EFDetached.EntityFramework rozšíření

> [!NOTE]  
> Toto rozšíření se neuchovávají v rámci projektu Entity Framework Core. Až budete zvažovat rozšíření třetích stran, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.

Načítá a ukládá grafy celý odpojit entity (entita s jejich podřízených entit a seznamy). INSPIROVANÉ [GraphDiff](https://github.com/refactorthis/GraphDiff/). Cílem je také přidat simplificate některé moduly plug-in některé opakované úkoly, jako je auditování a stránkování.

V následujícím zdroji vám pomůže začít pracovat s Detached.EntityFramework
* [Úložiště Detached.EntityFramework GitHub](https://github.com/leonardoporro/Detached/)
