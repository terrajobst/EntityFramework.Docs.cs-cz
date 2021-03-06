---
title: Migrace v týmových prostředích – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416768"
---
# <a name="migrations-in-team-environments"></a>Migrace v týmových prostředích

Při práci s migracemi v týmových prostředích věnujte zvláštní pozornost souboru snímku modelu. Tento soubor vám může sdělit, jestli vaše migrace společník bez problémů slučuje s vámi, nebo pokud potřebujete vyřešit konflikt tím, že ho před jeho sdílením znovu vytvoříte.

## <a name="merging"></a>sloučení

Při sloučení migrace z ostatními týmu můžete v souboru snímku modelu Zobrazit konflikty. Pokud se obě změny netýkají, je sloučení triviální a můžou dvě migrace koexistovat. Například může dojít ke konfliktu při sloučení v konfiguraci typu entity zákazníka, která vypadá takto:

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

Vzhledem k tomu, že obě tyto vlastnosti musí existovat v konečném modelu, dokončete sloučení přidáním obou vlastností. V mnoha případech může systém správy verzí tyto změny automaticky sloučit.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

V těchto případech je migrace a migrace společník vzájemně nezávislá. Vzhledem k tomu, že některé z nich je možné použít jako první, nemusíte provádět žádné další změny v migraci předtím, než je budete moct sdílet se svým týmem.

## <a name="resolving-conflicts"></a>Řešení konfliktů

Někdy při slučování modelu snímku modelu dochází ke skutečnému konfliktu. Například vaše společník může přejmenovat stejnou vlastnost.

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

Pokud se setkáte s tímto druhem konfliktu, vyřešte ho tím, že znovu vytvoříte migraci. Postupujte následovně:

1. Před sloučením přerušit sloučení a vrátit se zpátky do svého pracovního adresáře
2. Odebrání migrace (ale zachovat změny modelu)
3. Sloučit změny společník do pracovního adresáře
4. Opětovné přidání migrace

Po provedení této akce lze dvě migrace použít ve správném pořadí. Nejprve se použije migrace, aby se sloupec přejmenoval na *alias*, a potom se migrace přejmenuje na *uživatelské jméno*.

Vaši migraci můžete bezpečně sdílet se zbytkem týmu.
