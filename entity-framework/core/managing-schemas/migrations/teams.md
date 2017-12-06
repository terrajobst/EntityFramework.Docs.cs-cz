---
title: "Migrace v prostředích Team - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Migrace v prostředích Team
===============================
Při práci s migrací v prostředích team, věnujte zvláštní pozornost snímku souboru modelu. Tento soubor lze zjistit, pokud migrace vaší teammate slučuje řádně s vaším z Pokud potřebujete vyřešit konflikt znovu vytvořením migrace před její sdílení.

<a name="merging"></a>sloučení
-------
Při slučování migrace z ostatními členy týmu, může dojít v konfliktu v snímku souboru modelu. Pokud jsou obě změny nesouvisejícími, sloučení je jednoduchá a dvě migrace mohou existovat vedle sebe. Například může dojít ke konfliktu sloučení v konfiguraci typu entity zákazníka, které vypadá takto:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Vzhledem k tomu, že obě tyto vlastnosti musí existovat v posledním modelu, dokončete sloučení přidáním obě vlastnosti. V mnoha případech může váš systém správy verzí automaticky sloučení tyto změny za vás.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

V těchto případech migrace a vaše teammate migrace jsou navzájem nezávislé. Vzhledem k tomu, že buď z nich může být použije první, nemusíte provádět žádné další změny migrace před sdílením se svým týmem.

<a name="resolving-conflicts"></a>Řešení konfliktů
-------------------
Někdy dojde true konflikt při slučování snímků model modelu. Například můžete a vaše teammate může každý přejmenovaná stejnou vlastnost.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Pokud narazíte na tento druh konflikt vyřešte znovu vytvořením migrace. Postupujte takto:

1. Abort – sloučení a vrácení zpět do pracovního adresáře před sloučení
2. Odebrat migrace (ale zachovat změny modelu)
3. Sloučit změny vašeho teammate do pracovní adresář
4. Znovu přidejte migrace

Po této, můžete použít dvě migrace ve správném pořadí. Jejich migrace se použije první, přejmenování sloupce, který se *Alias*, následně migrace přejmenuje jeho *uživatelské jméno*.

Migrace je možné bezpečně sdílet s zbytek týmu.
