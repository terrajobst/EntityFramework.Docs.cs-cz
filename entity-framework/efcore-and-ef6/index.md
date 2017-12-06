---
title: "Porovnání EF základní & EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF základní & EF6

Existují dvě verze Entity Framework, Entity Framework Core a Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Rozhraní Entity Framework 6 (EF6) je technologie pokusů a otestovaná data přístup s mnoha let funkcí a ustálení. Ho prvního vydání v 2008, v rámci rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1. Od verze EF4.1 je dodávána jako [balíček EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) -aktuálně nejoblíbenější balíček v NuGet.org.

EF6 nadále podporované produktu a bude dál zobrazovat oprav chyb a menší vylepšení nějakou dobu pochází.

## <a name="entity-framework-core"></a>Základní Entity Framework

Základní Entity Framework (EF jader) je jednoduché, rozšiřitelný a napříč platformami verzi rozhraní Entity Framework. Základní EF představuje mnoho vylepšení a nové funkce ve srovnání s EF6. Ve stejnou dobu je základní EF nový kód základní a není vyspělá jako EF6.

Základní EF zajišťuje možnosti vývojáře z EF6 a většina nejvyšší úrovně rozhraní API, zůstane příliš, tak bude působí povědomý zaměstnance, kteří použili EF6 EF jádra. Ve stejnou dobu EF základní je sestavena úplně novou sadu základní součásti služby. To znamená, že základní EF nedědí automaticky všechny funkce z EF6. Některé z těchto funkcí se zobrazí v budoucích verzích (například opožděného načítání a připojení odolnosti), dalších méně často používány, že funkce nebudou implementované v EF jádra.

Nový rozšiřitelný a lightweight základní má také povolené nám přidat některé funkce do EF jádra, která nebude implementována v EF6 (například alternativní klíče a vyhodnocení smíšený klienta a databáze v dotazech LINQ).
