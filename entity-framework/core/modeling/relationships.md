---
title: Relace – EF Core
description: Konfigurace vztahů mezi typy entit při použití Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6d68e813cec6c989e8e4cb848f8740489645c65c
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77051404"
---
# <a name="relationships"></a>Relace

Vztah určuje, jak vzájemně souvisí dvě entity. V relační databázi to představuje omezení cizího klíče.

> [!NOTE]  
> Většina ukázek v tomto článku používá relaci 1: n k předvedení konceptů. Příklady relací 1:1 a m:n najdete v části [Další vzory vztahů](#other-relationship-patterns) na konci článku. "

## <a name="definition-of-terms"></a>Definice pojmů

K popisu vztahů se používá určitý počet výrazů.

* **Závislá entita:** Toto je entita, která obsahuje vlastnosti cizího klíče. V relaci se někdy označuje jako "podřízený".

* **Hlavní entita:** Toto je entita, která obsahuje vlastnosti primárního nebo alternativního klíče. V relaci se někdy označuje jako "nadřazený".

* **Hlavní klíč:** Vlastnosti, které jednoznačně identifikují hlavní entitu Může se jednat o primární klíč nebo alternativní klíč.

* **Cizí klíč:** Vlastnosti v závislé entitě, které se používají k ukládání hodnot hlavních klíčů pro související entitu

* **Navigační vlastnost:** Vlastnost definovaná na objektu zabezpečení nebo závislé entitě, která odkazuje na související entitu.

  * **Navigační vlastnost kolekce:** Navigační vlastnost, která obsahuje odkazy na mnoho souvisejících entit.

  * **Referenční navigační vlastnost:** Navigační vlastnost, která obsahuje odkaz na jednu související entitu.

  * **Inverzní navigační vlastnost:** Při diskusi na konkrétní navigační vlastnost se tento termín odkazuje na vlastnost navigace na druhém konci relace.
  
* **Vztah s odkazem na sebe:** Vztah, ve kterém jsou typy závislých a hlavních entit stejné.

Následující kód ukazuje vztah 1: n mezi `Blog` a `Post`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* `Post` je závislá entita.

* `Blog` je hlavní entitou.

* `Blog.BlogId` je hlavní klíč (v tomto případě je to primární klíč, nikoli alternativní klíč).

* `Post.BlogId` je cizí klíč.

* `Post.Blog` je navigační vlastnost odkazu

* `Blog.Posts` je navigační vlastnost kolekce

* `Post.Blog` je inverzní navigační vlastnost `Blog.Posts` (a naopak).

## <a name="conventions"></a>Zásady

Ve výchozím nastavení se relace vytvoří, když je u typu zjištěna vlastnost navigace. Vlastnost je považována za navigační vlastnost, pokud typ, na který odkazuje, nemůže být namapován jako skalární typ aktuálním poskytovatelem databáze.

> [!NOTE]  
> Relace zjištěné podle konvence budou vždycky cílit na primární klíč hlavní entity. Aby bylo možné cílit na alternativní klíč, je nutné provést další konfiguraci pomocí rozhraní Fluent API.

### <a name="fully-defined-relationships"></a>Plně definované relace

Nejběžnějším vzorem relací je mít vlastnosti navigace definované na obou koncích relace a vlastnost cizího klíče definované v třídě závislé entity.

* Pokud je mezi dvěma typy nalezen pár vlastností navigace, budou nakonfigurovány jako inverzní navigační vlastnosti stejné relace.

* Pokud závislá entita obsahuje vlastnost s názvem, který odpovídá jednomu z těchto vzorů, pak se nakonfiguruje jako cizí klíč:
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

V tomto příkladu se pro konfiguraci relace použijí zvýrazněné vlastnosti.

> [!NOTE]
> Pokud je vlastnost primárním klíčem nebo je typu, který není kompatibilní s klíčovým objektem zabezpečení, nebude nakonfigurovaný jako cizí klíč.

> [!NOTE]
> Před EF Core 3,0 vlastnost s názvem přesně stejná jako vlastnost hlavního klíče [byla také shodná s cizím klíčem](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .

### <a name="no-foreign-key-property"></a>Žádná vlastnost cizího klíče

I když se doporučuje mít vlastnost cizího klíče definovanou ve třídě závislé entity, není to nutné. Pokud se nenalezne žádná vlastnost cizího klíče, zavede se [vlastnost stínového cizího klíče](shadow-properties.md) s názvem `<navigation property name><principal key property name>` nebo `<principal entity name><principal key property name>`, pokud se na závislém typu nenachází žádná navigace.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

V tomto příkladu je `BlogId` stín cizího klíče, protože před nedokončeným názvem navigace by byl redundantní.

> [!NOTE]
> Pokud vlastnost se stejným názvem již existuje, bude název vlastnosti stínové kopie zaregistrovaný číslem.

### <a name="single-navigation-property"></a>Jednoduchá navigační vlastnost

Zahrnutím pouze jedné navigační vlastnosti (bez invertované navigace a žádné vlastnosti cizího klíče) není dostatečná relace definovaná v konvenci. Můžete mít také jednu navigační vlastnost a vlastnost cizího klíče.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a>Omezení

Pokud je mezi dvěma typy definováno více vlastností navigace (to znamená více než pouze jedna dvojice navigačních oblastí, které odkazují na sebe), jsou relace reprezentované navigačními vlastnostmi dvojznačné. Budete je muset ručně nakonfigurovat, aby se vyřešila nejednoznačnost.

### <a name="cascade-delete"></a>Kaskádové odstranění

Podle konvence se kaskádové odstranění nastaví na *kaskády* pro požadované relace a *ClientSetNull* pro volitelné vztahy. *Cascade* znamená, že jsou také odstraněny závislé entity. *ClientSetNull* znamená, že závislé entity, které nejsou načteny do paměti, zůstanou beze změny a je nutné je ručně odstranit nebo aktualizovat, aby odkazovaly na platnou objektovou entitu. V případě entit, které jsou načteny do paměti, se EF Core pokusí nastavit vlastnosti cizího klíče na hodnotu null.

Rozdíl mezi požadovanými a volitelnými vztahy najdete v části [vyžadované a volitelné relace](#required-and-optional-relationships) .

Další podrobnosti o různých chováních při odstraňování a výchozích hodnotách, které používá konvence, najdete v tématu [kaskádová odstranění](../saving/cascade-delete.md) .

## <a name="manual-configuration"></a>Ruční konfigurace

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

Pokud chcete nakonfigurovat relaci v rozhraní Fluent API, začněte tím, že určíte navigační vlastnosti, které tvoří relaci. `HasOne` nebo `HasMany` identifikuje navigační vlastnost u typu entity, na které začínáte konfigurovat. Pak řetězení volání `WithOne` nebo `WithMany` k identifikaci invertované navigace. `HasOne`/`WithOne` se používají pro referenční vlastnosti navigace a `HasMany`/`WithMany` se používají pro navigační vlastnosti kolekce.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

Pomocí datových poznámek můžete nakonfigurovat, jak se mají spárovat vlastnosti navigace na závislých a hlavních entitách. To se obvykle provádí, pokud existuje více než jedna dvojice vlastností navigace mezi dvěma typy entit.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> Můžete použít pouze [povinné] u vlastností závislé entity, aby se ovlivnila požadovaná vlastnost vztahu. [Požadováno] při navigaci z hlavní entity je obvykle ignorováno, ale může způsobit, že entita bude závislá na sobě.

> [!NOTE]
> Poznámky k datům `[ForeignKey]` a `[InverseProperty]` jsou k dispozici v oboru názvů `System.ComponentModel.DataAnnotations.Schema`. `[Required]` je k dispozici v oboru názvů `System.ComponentModel.DataAnnotations`.

---

### <a name="single-navigation-property"></a>Jednoduchá navigační vlastnost

Pokud máte pouze jednu navigační vlastnost, existují přetížení `WithOne` a `WithMany`bez parametrů. To znamená, že na konci relace existuje koncepční odkaz nebo kolekce, ale ve třídě entity není obsažena žádná vlastnost navigace.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a>Cizí klíč

#### <a name="fluent-api-simple-keytabfluent-api-simple-key"></a>[Rozhraní Fluent API (jednoduchý klíč)](#tab/fluent-api-simple-key)

Pomocí rozhraní Fluent API můžete nakonfigurovat, která vlastnost se má použít jako vlastnost cizího klíče pro daný vztah:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-keytabfluent-api-composite-key"></a>[Rozhraní Fluent API (složený klíč)](#tab/fluent-api-composite-key)

Pomocí rozhraní Fluent API můžete nakonfigurovat, které vlastnosti se mají použít jako vlastnosti složeného cizího klíče pro danou relaci:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-keytabdata-annotations-simple-key"></a>[Datové poznámky (jednoduchý klíč)](#tab/data-annotations-simple-key)

Pomocí datových poznámek můžete nakonfigurovat, která vlastnost má být použita jako vlastnost cizího klíče pro danou relaci. To se obvykle provádí v případě, že vlastnost cizího klíče není zjištěna podle konvence:

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> Anotaci `[ForeignKey]` lze umístit buď do vlastnosti navigace v relaci. Nepotřebujete přejít na navigační vlastnost v třídě závislé entity.

> [!NOTE]
> Vlastnost zadaná pomocí `[ForeignKey]` pro vlastnost navigace nemusí existovat závislého typu. V tomto případě se pro vytvoření stínového cizího klíče použije zadaný název.

---

#### <a name="shadow-foreign-key"></a>Stínový cizí klíč

Pomocí přetížení řetězců `HasForeignKey(...)` můžete nakonfigurovat vlastnost Shadow jako cizí klíč (Další informace najdete v tématu [vlastnosti stínu](shadow-properties.md) ). Do modelu doporučujeme explicitně přidat vlastnost Shadow, než ji použijete jako cizí klíč (jak je vidět níže).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a>Název omezení cizího klíče

Podle konvence při cílení na relační databázi jsou omezení cizího klíče pojmenována FK_<dependent type name> _<principal type name>_ <foreign key property name>. Pro složené cizí klíče se <foreign key property name> jako podtržítko oddělený seznam názvů vlastností cizích klíčů.

Název omezení můžete také nakonfigurovat následujícím způsobem:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a>Bez navigační vlastnosti

Nemusíte nutně zadávat navigační vlastnost. Můžete jednoduše zadat cizí klíč na jednu stranu relace.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a>Hlavní klíč

Pokud chcete, aby cizí klíč odkazoval na jinou vlastnost než na primární klíč, můžete pro relaci nakonfigurovat vlastnost klíče zabezpečení pomocí rozhraní Fluent API. Vlastnost, kterou nakonfigurujete jako hlavní klíč, se automaticky nastaví jako [alternativní klíč](alternate-keys.md).

#### <a name="simple-keytabsimple-key"></a>[Jednoduchý klíč](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-keytabcomposite-key"></a>[Složený klíč](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> Pořadí, ve kterém určíte vlastnosti hlavního klíče, musí odpovídat pořadí, ve kterém jsou zadané pro cizí klíč.

---

### <a name="required-and-optional-relationships"></a>Povinné a volitelné relace

Pomocí rozhraní Fluent API můžete nakonfigurovat, jestli je relace povinná nebo volitelná. Nakonec tato možnost určuje, jestli je vlastnost cizího klíče povinná nebo volitelná. To je nejužitečnější při použití cizího klíče stavu stínové kopie. Máte-li ve třídě entity vlastnost cizího klíče, je vyžadována požadovaná vlastnost vztahu na základě toho, zda je vlastnost cizího klíče povinná nebo volitelná (Další informace naleznete v části [povinné a volitelné vlastnosti](required-optional.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> Volání `IsRequired(false)` také nastaví vlastnost cizího klíče jako volitelnou, pokud není konfigurována jinak.

### <a name="cascade-delete"></a>Kaskádové odstranění

Rozhraní Fluent API můžete použít ke konfiguraci chování kaskádového odstranění pro daný vztah explicitně.

Podrobné informace o jednotlivých možnostech najdete v části [kaskádové odstranění](../saving/cascade-delete.md) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a>Další vzory vztahů

### <a name="one-to-one"></a>Jeden k jednomu

Jedna až jedna relace má referenční navigační vlastnost na obou stranách. Dodržují stejné konvence jako relace 1: n, ale do vlastnosti cizího klíče se zavedl jedinečný index, který zaručí, že k jednotlivým objektům zabezpečení souvisí jenom jedna závislá hodnota.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> EF vybere jednu z entit, které budou závislé na závislosti na schopnosti detekovat vlastnost cizího klíče. Pokud je jako závislá vybraná nesprávná entita, můžete ji opravit pomocí rozhraní Fluent API.

Když konfigurujete relaci s rozhraním API Fluent, použijete metody `HasOne` a `WithOne`.

Při konfiguraci cizího klíče musíte zadat typ závislé entity – Všimněte si obecného parametru poskytnutého `HasForeignKey` v níže uvedeném seznamu. V relaci 1: n je jasné, že entita s referenční navigací je závislá a ta, která je v kolekci, je objektem zabezpečení. Nejedná se ale o relaci 1:1, takže je potřeba explicitně definovat.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Many-to-many

Relace m:n bez třídy entity představující tabulku JOIN se ještě nepodporují. Můžete však znázornit relaci n:n zahrnutím třídy entity pro tabulku JOIN a mapováním dvou samostatných vztahů 1: n.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
