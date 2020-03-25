---
title: Porovnávače hodnot – EF Core
description: Použití porovnávače hodnot k řízení, jak EF Core porovnávají hodnoty vlastností
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: 9dfed7b7ef8163f4f5c94a0c81c510807c53c13d
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80148269"
---
# <a name="value-comparers"></a>Porovnávače hodnot

> [!NOTE]  
> Tato funkce je v EF Core 3,0 novinkou.

> [!TIP]  
> Kód v tomto dokumentu najdete na GitHubu jako [spustitelný Sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).

## <a name="background"></a>Pozadí

EF Core musí porovnat hodnoty vlastností, když:

* Zjištění, zda byla vlastnost změněna v rámci [zjišťování změn aktualizací](xref:core/saving/basic)
* Určení, zda jsou dvě hodnoty klíče stejné při řešení vztahů 

To se zpracovává automaticky pro běžné primitivní typy, jako je int, bool, DateTime atd.

U složitějších typů je třeba provést volby jako způsob porovnání.
Například pole bajtů může být porovnáno:

* Podle odkazu je to, že rozdíl je zjištěn pouze v případě, že je použito nové bajtové pole
* Důkladným porovnáním, aby se zjistila mutace bajtů v poli

Ve výchozím nastavení EF Core používá první z těchto přístupů pro pole neklíčového bajtu.
To znamená, že jsou porovnány pouze odkazy a byla zjištěna změna pouze v případě, že je existující bajtové pole nahrazeno novým.
Toto je narušené rozhodnutí, které při provádění metody SaveChanges vylučuje hloubkové porovnání mnoha velkých bajtových polí.
Ale běžný scénář nahrazení, vyslovení a obrázku s jiným obrázkem, je zpracováván způsobem.

Na druhé straně odkazování by nefungovalo, pokud se pole bajtů používá k reprezentaci binárních klíčů.
Je velmi pravděpodobné, že vlastnost FK je nastavena na _stejnou instanci_ jako vlastnost PK, pro kterou je třeba ji porovnat.
Proto EF Core používá hloubková porovnání pro pole bajtů fungující jako klíče.
Je pravděpodobné, že by se dosáhlo velkého výkonu, protože binární klíče jsou obvykle krátké.

### <a name="snapshots"></a>Snímky

Důkladné porovnání na proměnlivých typech znamená, že EF Core potřebuje možnost vytvořit hlubokou "hodnotu" snímku hodnoty vlastnosti.
Pouze kopírování odkazu by vedlo k tomu, že by se použila aktuální hodnota i snímek, protože se jedná _o stejný objekt_.
Proto pokud jsou na proměnlivých typech použity hloubková porovnání, jsou také požadovány důkladné snapshotting.

## <a name="properties-with-value-converters"></a>Vlastnosti s převaděči hodnot

V případě výše EF Core má podpora nativního mapování pro pole bajtů a tak může automaticky zvolit příslušné výchozí hodnoty.
Pokud je však vlastnost mapována pomocí [konvertoru hodnot](xref:core/modeling/value-conversions), EF Core nemůže vždy určit příslušné porovnání, které se má použít.
Místo toho EF Core vždy používá výchozí porovnání rovnosti definované typem vlastnosti.
To je často správné, ale může být potřeba je přepsat při mapování složitějších typů.

### <a name="simple-immutable-classes"></a>Jednoduché neměnné třídy

Zvažte, že vlastnost používá konvertor hodnot k namapování jednoduché, neměnné třídy.

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

Vlastnosti tohoto typu nevyžadují speciální porovnání nebo snímky z těchto důvodů:
* Rovnost je přepsaná, aby se různé instance správně porovnaly.
* Typ je neproměnlivý, takže není možné pořídit hodnotu snímku.

Takže v tomto případě je výchozí chování EF Core v pořádku.

### <a name="simple-immutable-structs"></a>Jednoduché neměnné struktury

Mapování jednoduchých struktur je také jednoduché a nevyžaduje žádné speciální porovnávače ani snapshotting.

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

EF Core obsahuje integrovanou podporu pro generování kompilovaných kopírování členů porovnání vlastností struktury.
To znamená, že struktury nemusí mít u EF přepsat rovnost, ale přesto se můžete rozhodnout k tomu, aby to bylo možné provést [z jiných důvodů](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).
Speciální snapshotting není potřeba, protože struktury jsou neměnné a pořád se kopírování členů zkopírovali.
(To platí také pro proměnlivé struktury, ale [měly by být obecně zabráněno proměnlivým strukturám](/dotnet/csharp/write-safe-efficient-code).)

### <a name="mutable-classes"></a>Měnitelné třídy

Doporučuje se používat neměnné typy (třídy nebo struktury) s převaděči hodnot, pokud je to možné.
To je obvykle efektivnější a má sémantiku čištění, než použití měnitelného typu.

Nicméně, je-li to však řečeno, je běžné používat vlastnosti typů, které aplikace nelze změnit.
Například mapování vlastnosti obsahující seznam čísel: 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

[Třída`List<T>`](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):
* Má referenční rovnost; dva seznamy obsahující stejné hodnoty jsou považovány za odlišné.
* Je proměnlivá; hodnoty v seznamu lze přidat a odebrat.

Typický převod hodnoty na vlastnost seznamu může převést seznam na JSON a z něj:

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

To pak vyžaduje nastavení `ValueComparer<T>` na vlastnost pro vynucení EF Core použití správného porovnání s tímto převodem:

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> Rozhraní API pro sestavování modelů ("Fluent") pro nastavení porovnávací hodnoty ještě není implementované.
> Místo toho kód výše volá SetValueComparer na nižší úrovni IMutableProperty vystavené tvůrcem jako metadata.

Konstruktor `ValueComparer<T>` přijímá tři výrazy:
* Výraz pro kontrolu kvality
* Výraz pro generování kódu hash
* Výraz pro vytvoření snímku hodnoty  

V tomto případě je porovnání provedeno pomocí kontroly, zda jsou sekvence čísel stejné.

Podobně je kód hash sestaven z stejné sekvence.
(Všimněte si, že se jedná o kód hash přes proměnlivé hodnoty, a proto může [způsobit problémy](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).
Neměnné místo, pokud je to možné.)

Snímek se vytvoří klonováním seznamu pomocí ToList –.
To je možné znovu jenom v případě, že se budou používat seznamy.
Neměnné místo, pokud je to možné. 

> [!NOTE]  
> Převaděče hodnot a porovnávače se vytvářejí pomocí výrazů místo jednoduchých delegátů.
> Je to proto, že EF vloží tyto výrazy do mnohem složitějšího stromu výrazů, který je poté zkompilován do delegáta Shaper entity.
> V koncepčním případě je to podobné jako vkládání kompilátoru.
> Například jednoduchý převod může být pouze zkompilovaný v přetypování, nikoli volání jiné metody pro provedení převodu.    

### <a name="key-comparers"></a>Porovnávače klíčů

Oddíl na pozadí obsahuje informace o tom, proč porovnávání klíčů může vyžadovat speciální sémantiku.
Nezapomeňte vytvořit porovnávací metodu, která je vhodná pro klíče při jejich nastavování na primární vlastnost, objekt zabezpečení nebo cizí klíč.

Použijte [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) ve výjimečných případech, kde se pro stejnou vlastnost vyžaduje odlišná sémantika.

> [!NOTE]  
> SetStructuralComparer je zastaralá v EF Core 5,0.
> Místo toho použijte SetKeyValueComparer.

### <a name="overriding-defaults"></a>Přepisování výchozích hodnot

V některých případech nemusí být výchozí porovnání používané EF Core vhodné.
Například mutace z bajtových polí nejsou ve výchozím nastavení zjištěny v EF Core.
To lze přepsat nastavením jiné porovnávací metody na vlastnost: 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

EF Core nyní porovná sekvence bajtů a bude proto detekovat mutace bajtových polí.
