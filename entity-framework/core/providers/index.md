---
title: "Zprostředkovatelé databáze - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Zprostředkovatelé databáze

Entity Framework Core používá model zprostředkovatele umožňující EF, který se má použít pro přístup k mnoha různým databázím. Některé pojmy jsou společné pro většinu databází a jsou součástí primární EF základních komponent. Tyto koncepty patří vyjadřující dotazy v technologii LINQ, transakce a sledovacích změny objektů jednou jsou načteny z databáze. Některé pojmy jsou specifické pro konkrétního poskytovatele. Zprostředkovatel SQL Server například umožňuje nakonfigurovat paměťově optimalizované tabulky (funkce specifické pro systém SQL Server). Další koncepty jsou specifické pro třídu zprostředkovatelů. Například EF hlavními zprostředkovateli pro relační databáze sestavení na nejběžnější `Microsoft.EntityFrameworkCore.Relational` knihovny, která poskytuje rozhraní API pro konfiguraci mapování tabulky a sloupce, omezení cizích klíčů, atd.

EF hlavními zprostředkovateli jsou vytvořeny pomocí různých zdrojů. Ne všichni zprostředkovatelé se udržují v rámci projektu Entity Framework Core. Při určování zprostředkovatele třetí strany, ujistěte se, že jste vyhodnocení kvality, licencování, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.
