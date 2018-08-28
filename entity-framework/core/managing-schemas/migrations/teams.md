---
title: Migrace v prostředích Team – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997692"
---
<a name="migrations-in-team-environments"></a>Migrace v prostředích Team
===============================
Při práci s migrací v týmu prostředí je třeba věnujte zvláštní pozornost snímku souboru modelu. Tento soubor můžete říct, pokud se migrace vašich programujete sloučí čistě s vaším nebo pokud je potřeba vyřešit konflikt tak, že znovu vytvoříte migraci před jejich sdílením.

<a name="merging"></a>sloučení
-------
Při sloučení migrace z vašeho týmu, může zobrazit je v konfliktu v modelu snímku souboru. Pokud jsou obě změny nesouvisející, sloučení je jednoduché a dvě migrace mohou existovat vedle sebe. Například může získat konfliktu při slučování v konfiguraci typu entity zákazník, který vypadá takto:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Protože obě tyto vlastnosti musí existovat v finálního modelu, sloučit tak, že přidáte obě vlastnosti. V mnoha případech může váš systém správy verzí automaticky sloučit tyto změny za vás.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

V těchto případech se migrace a migrace vašich programujete nezávisle na sobě navzájem. Protože některou z nich můžete uplatnit v první, není nutné provádět žádné další změny migraci před jejich sdílením se svým týmem.

<a name="resolving-conflicts"></a>Řešení konfliktů
-------------------
Někdy narazíte na true konfliktu při slučování snímků model modelu. Například vám a vaší programujete může každý přejmenovali stejnou vlastnost.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Pokud narazíte na tento druh konflikt vyřešte opětovné vytvoření migrace. Postupujte podle těchto kroků:

1. Zrušit sloučení a vrácení zpět do pracovního adresáře ještě před sloučením
2. Odebrat migrace (ale zachovat změny modelu)
3. Vaše programujete změny sloučíte do pracovního adresáře
4. Migrace je znovu přidat

Po této, můžete použít dvě migrace ve správném pořadí. Jejich migrace se použije první, přejmenování sloupce za účelem *Alias*, po tomto datu migrace přejmenuje na *uživatelské jméno*.

Migrace je bezpečně sdílet s ostatními členy týmu.
