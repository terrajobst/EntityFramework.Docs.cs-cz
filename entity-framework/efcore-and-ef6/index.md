---
title: Porovnání EF Core a EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002753"
---
# <a name="compare-ef-core--ef6"></a>Porovnání EF Core a EF6

Existují dvě různé verze Entity Framework, Entity Framework Core a Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Rozhraní Entity Framework 6 (EF6) je technologie pokusů a otestovaná data přístup s mnoha let funkcí a ustálení. Ho prvního vydání v 2008, v rámci rozhraní .NET Framework 3.5 SP1 a Visual Studio 2008 SP1. Od verze EF4.1 je dodávána jako [balíček EntityFramework NuGet](https://www.nuget.org/packages/EntityFramework/) -aktuálně jeden z nejoblíbenější balíčky na NuGet.org.

EF6 nadále podporované produktu a bude dál zobrazovat oprav chyb a menší vylepšení nějakou dobu pochází.

## <a name="entity-framework-core"></a>Entity Framework Core

Základní Entity Framework (EF jader) je jednoduché, rozšiřitelný a napříč platformami verzi rozhraní Entity Framework. Základní EF představuje mnoho vylepšení a nové funkce ve srovnání s EF6. Ve stejnou dobu je základní EF nový kód základní a není vyspělá jako EF6.

Základní EF zajišťuje možnosti vývojáře z EF6 a většina nejvyšší úrovně rozhraní API, zůstane příliš, tak bude působí povědomý zaměstnance, kteří použili EF6 EF jádra. Ve stejnou dobu EF základní je sestavena úplně novou sadu základní součásti služby. To znamená, že základní EF nedědí automaticky všechny funkce z EF6. Když některé z těchto funkcí se zobrazí v budoucích verzích, nebude další funkce, ne tak často používaná implementovat v EF jádra.

Nový rozšiřitelný a lightweight základní má také povolené nám přidat některé funkce do EF jádra, která nebude implementována v EF6 (například alternativní klíče, dávková aktualizace a vyhodnocení smíšený klienta a databáze v dotazech LINQ).
