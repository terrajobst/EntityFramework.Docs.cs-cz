---
title: Relace – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655676"
---
# <a name="relationships"></a>Relace

Vztah určuje, jak vzájemně souvisí dvě entity. V relační databázi to představuje omezení cizího klíče.

> [!NOTE]  
> Většina ukázek v tomto článku používá relaci 1: n k předvedení konceptů. Příklady relací 1:1 a m:n najdete v části [Další vzory vztahů](#other-relationship-patterns) na konci článku. "

## <a name="definition-of-terms"></a>Definice pojmů

K popisu vztahů se používá určitý počet výrazů.

* **Závislá entita:** Toto je entita, která obsahuje vlastnosti cizího klíče. V relaci se někdy označuje jako "podřízený".

* **Hlavní entita:** Toto je entita, která obsahuje vlastnosti primárního a alternativního klíče. V relaci se někdy označuje jako "nadřazený".

* **Cizí klíč:** Vlastnosti v závislé entitě, která se používá k uložení hodnot vlastnosti hlavního klíče, se kterou entita souvisí

* **Hlavní klíč:** Vlastnosti, které jedinečně identifikují hlavní entitu. Může se jednat o primární klíč nebo alternativní klíč.

* **Navigační vlastnost:** Vlastnost definovaná na objektu zabezpečení nebo závislé entitě, která obsahuje odkazy na související entity (y).

  * **Navigační vlastnost kolekce:** Navigační vlastnost, která obsahuje odkazy na mnoho souvisejících entit.

  * **Referenční navigační vlastnost:** Navigační vlastnost, která obsahuje odkaz na jednu související entitu.

  * **Inverzní navigační vlastnost:** Při diskusi na konkrétní navigační vlastnost se tento termín odkazuje na vlastnost navigace na druhém konci relace.

Následující výpis kódu ukazuje vztah 1: n mezi `Blog` a `Post`

* `Post` je závislá entita.

* `Blog` je hlavní entitou.

* `Post.BlogId` je cizí klíč.

* `Blog.BlogId` je hlavní klíč (v tomto případě je to primární klíč, nikoli alternativní klíč).

* `Post.Blog` je navigační vlastnost odkazu

* `Blog.Posts` je navigační vlastnost kolekce

* `Post.Blog` je inverzní navigační vlastnost `Blog.Posts` (a naopak).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konvence

Podle konvence se vytvoří relace, když je u typu zjištěna vlastnost navigace. Vlastnost je považována za navigační vlastnost, pokud typ, na který odkazuje, nemůže být namapován jako skalární typ aktuálním poskytovatelem databáze.

> [!NOTE]  
> Relace zjištěné podle konvence budou vždycky cílit na primární klíč hlavní entity. Aby bylo možné cílit na alternativní klíč, je nutné provést další konfiguraci pomocí rozhraní Fluent API.

### <a name="fully-defined-relationships"></a>Plně definované relace

Nejběžnějším vzorem relací je mít vlastnosti navigace definované na obou koncích relace a vlastnost cizího klíče definované v třídě závislé entity.

* Pokud je mezi dvěma typy nalezen pár vlastností navigace, budou nakonfigurovány jako inverzní navigační vlastnosti stejné relace.

* Pokud závislá entita obsahuje vlastnost s názvem `<primary key property name>`, `<navigation property name><primary key property name>`nebo `<principal entity name><primary key property name>` pak bude nakonfigurována jako cizí klíč.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Pokud je mezi dvěma typy definováno více vlastností navigace (tj. více než jeden odlišný pár navigace, které navzájem odkazují), nebudou vytvořeny žádné vztahy podle konvence a bude nutné je ručně nakonfigurovat, abyste identifikovali, jak párování vlastností navigace nahoru.

### <a name="no-foreign-key-property"></a>Žádná vlastnost cizího klíče

I když se doporučuje mít vlastnost cizího klíče definovanou ve třídě závislé entity, není to nutné. Pokud se nenalezne žádná vlastnost cizího klíče, zavede se vlastnost stínového cizího klíče s názvem `<navigation property name><principal key property name>` (Další informace najdete v tématu [vlastnosti stínu](shadow-properties.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Jednoduchá navigační vlastnost

Zahrnutím pouze jedné navigační vlastnosti (bez invertované navigace a žádné vlastnosti cizího klíče) není dostatečná relace definovaná v konvenci. Můžete mít také jednu navigační vlastnost a vlastnost cizího klíče.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Kaskádové odstranění

Podle konvence se kaskádové odstranění nastaví na *kaskády* pro požadované relace a *ClientSetNull* pro volitelné vztahy. *Cascade* znamená, že jsou také odstraněny závislé entity. *ClientSetNull* znamená, že závislé entity, které nejsou načteny do paměti, zůstanou beze změny a je nutné je ručně odstranit nebo aktualizovat, aby odkazovaly na platnou objektovou entitu. V případě entit, které jsou načteny do paměti, se EF Core pokusí nastavit vlastnosti cizího klíče na hodnotu null.

Rozdíl mezi požadovanými a volitelnými vztahy najdete v části [vyžadované a volitelné relace](#required-and-optional-relationships) .

Další podrobnosti o různých chováních při odstraňování a výchozích hodnotách, které používá konvence, najdete v tématu [kaskádová odstranění](../saving/cascade-delete.md) .

## <a name="data-annotations"></a>Datové poznámky

Existují dva datové poznámky, které lze použít ke konfiguraci relací, `[ForeignKey]` a `[InverseProperty]`. Jsou k dispozici v oboru názvů `System.ComponentModel.DataAnnotations.Schema`.

### <a name="foreignkey"></a>Klíč ForeignKey

Pomocí datových poznámek můžete nakonfigurovat, která vlastnost má být použita jako vlastnost cizího klíče pro danou relaci. To se obvykle provádí v případě, že není zjištěna vlastnost cizího klíče podle konvence.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> Anotaci `[ForeignKey]` lze umístit buď do vlastnosti navigace v relaci. Nepotřebujete přejít na navigační vlastnost v třídě závislé entity.

### <a name="inverseproperty"></a>[InverseProperty]

Pomocí datových poznámek můžete nakonfigurovat, jak se mají spárovat vlastnosti navigace na závislých a hlavních entitách. To se obvykle provádí, pokud existuje více než jedna dvojice vlastností navigace mezi dvěma typy entit.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Pokud chcete nakonfigurovat relaci v rozhraní Fluent API, začněte tím, že určíte navigační vlastnosti, které tvoří relaci. `HasOne` nebo `HasMany` identifikuje navigační vlastnost u typu entity, na které začínáte konfigurovat. Pak řetězení volání `WithOne` nebo `WithMany` k identifikaci invertované navigace. `HasOne`/`WithOne` se používají pro referenční vlastnosti navigace a `HasMany`/`WithMany` se používají pro navigační vlastnosti kolekce.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Jednoduchá navigační vlastnost

Pokud máte pouze jednu navigační vlastnost, existují přetížení `WithOne` a `WithMany`bez parametrů. To znamená, že na konci relace existuje koncepční odkaz nebo kolekce, ale ve třídě entity není obsažena žádná vlastnost navigace.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Cizí klíč

Rozhraní Fluent API můžete použít ke konfiguraci, které vlastnosti by se měly používat jako vlastnost cizího klíče pro danou relaci.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

Následující výpis kódu ukazuje, jak nakonfigurovat složený cizí klíč.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Pomocí přetížení řetězců `HasForeignKey(...)` můžete nakonfigurovat vlastnost Shadow jako cizí klíč (Další informace najdete v tématu [vlastnosti stínu](shadow-properties.md) ). Do modelu doporučujeme explicitně přidat vlastnost Shadow, než ji použijete jako cizí klíč (jak je vidět níže).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Bez navigační vlastnosti

Nemusíte nutně zadávat navigační vlastnost. Můžete jednoduše zadat cizí klíč na jednu stranu relace.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Hlavní klíč

Pokud chcete, aby cizí klíč odkazoval na jinou vlastnost než na primární klíč, můžete pro relaci nakonfigurovat vlastnost klíče zabezpečení pomocí rozhraní Fluent API. Vlastnost, kterou nakonfigurujete jako hlavní klíč, se automaticky nastaví jako alternativní klíč (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

Následující výpis kódu ukazuje, jak nakonfigurovat složený klíč zabezpečení.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> Pořadí, ve kterém určíte vlastnosti hlavního klíče, musí odpovídat pořadí, ve kterém jsou zadané pro cizí klíč.

### <a name="required-and-optional-relationships"></a>Povinné a volitelné relace

Pomocí rozhraní Fluent API můžete nakonfigurovat, jestli je relace povinná nebo volitelná. Nakonec tato možnost určuje, jestli je vlastnost cizího klíče povinná nebo volitelná. To je nejužitečnější při použití cizího klíče stavu stínové kopie. Máte-li ve třídě entity vlastnost cizího klíče, je vyžadována požadovaná vlastnost vztahu na základě toho, zda je vlastnost cizího klíče povinná nebo volitelná (Další informace naleznete v části [povinné a volitelné vlastnosti](required-optional.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a>Kaskádové odstranění

Rozhraní Fluent API můžete použít ke konfiguraci chování kaskádového odstranění pro daný vztah explicitně.

Podrobné informace o jednotlivých možnostech najdete v části [kaskádová odstranění](../saving/cascade-delete.md) v části ukládání dat.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Další vzory vztahů

### <a name="one-to-one"></a>Jeden k jednomu

Jedna až jedna relace má referenční navigační vlastnost na obou stranách. Dodržují stejné konvence jako relace 1: n, ale do vlastnosti cizího klíče se zavedl jedinečný index, který zaručí, že k jednotlivým objektům zabezpečení souvisí jenom jedna závislá hodnota.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> EF vybere jednu z entit, které budou závislé na závislosti na schopnosti detekovat vlastnost cizího klíče. Pokud je jako závislá vybraná nesprávná entita, můžete ji opravit pomocí rozhraní Fluent API.

Když konfigurujete relaci s rozhraním API Fluent, použijete metody `HasOne` a `WithOne`.

Při konfiguraci cizího klíče musíte zadat typ závislé entity – Všimněte si obecného parametru poskytnutého `HasForeignKey` v níže uvedeném seznamu. V relaci 1: n je jasné, že entita s referenční navigací je závislá a ta, která je v kolekci, je objektem zabezpečení. Nejedná se ale o relaci 1:1, takže je potřeba explicitně definovat.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Mnoho k mnoha

Relace m:n bez třídy entity představující tabulku JOIN se ještě nepodporují. Můžete však znázornit relaci n:n zahrnutím třídy entity pro tabulku JOIN a mapováním dvou samostatných vztahů 1: n.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
