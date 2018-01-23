---
title: "Načítají se související Data – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a>Načítání související Data

Entity Framework Core umožňuje používat navigační vlastnosti v modelu se načíst související entity. Existují tři obecné vzory O/RM používají k zatížení související data.
* **Přes načítání** znamená, že související data načtená z databáze jako součást počáteční dotazu.
* **Explicitní načítání** znamená, že související data se explicitně načíst z databáze později.
* **Opožděného načítání** znamená, že související transparentně načtení dat z databáze při přístupu k navigační vlastnost. Opožděného načítání ještě není možné s EF jádra.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="eager-loading"></a>přes načítání

Můžete použít `Include` metoda k určení související data mají být zahrnuty do výsledků dotazu. V následujícím příkladu, bude mít blogy, které jsou vráceny ve výsledcích jejich `Posts` vlastnost naplněný související příspěvky.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core bude automaticky opravu up navigačních vlastností pro ostatní entity, které byly dříve načteny do instance kontextu. Takže i v případě, že není výslovně zahrnout data pro navigační vlastnost, vlastnost pořád naplněný, pokud některé nebo všechny související entity byly dříve načteny.


Související data z více vztahů můžete zahrnout jeden dotaz.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Více úrovní

Podrobnostem přispívají vztahů s zahrnují více úrovní související data pomocí `ThenInclude` metoda. Následující příklad načte všechny blogy, jejich související příspěvky a Autor každé post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Aktuální verze sady Visual Studio nabízí možnosti dokončení nesprávný kód a mohou způsobit správné výrazy označen příznakem chyby syntaxe při použití `ThenInclude` metoda po navigační vlastnost kolekce. Toto je příznakem IntelliSense chyb sledovány v https://github.com/dotnet/roslyn/issues/8237. Je bezpečné tyto chyby syntaxe nesprávné ignorovat, dokud je správný kód a mohou být zkompilovány úspěšně. 

Můžete zřetězit více volání `ThenInclude` Chcete-li pokračovat, včetně další úrovně související data.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Můžete sloučit všechny tohoto zahrnout související data z více úrovní a více kořenových certifikačních autorit ve stejném dotazu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Můžete zahrnout více entit v relaci pro jednu z entity, které bude zahrnut. Například při dotazování `Blog`s, zahrnete `Posts` a chcete současně obsahovat `Author` a `Tags` z `Posts`. K tomuto účelu, budete muset zadat jednotlivé obsahovat cestu spouštění v kořenovém adresáři. Například `Blog -> Posts -> Author` a `Blog -> Posts -> Tags`. To neznamená, že budete mít redundantní spojení, ve většině případů, které budou konsolidovat EF spojení při generování SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Ignorovat zahrnuje

Pokud změníte dotaz tak, že už vrátí instance typu entity, které začne dotaz s, se ignorují operátory zahrnout.

V následujícím příkladu jsou na základě operátory zahrnout `Blog`, ale pak `Select` operátor se používá ke změně dotaz vrátit instanci anonymního typu. V takovém případě operátory zahrnout nemají žádný vliv.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Ve výchozím nastavení, bude EF základní zaprotokolovat upozornění při zahrnují operátory jsou ignorovány. V tématu [protokolování](../miscellaneous/logging.md) Další informace o zobrazení výstupu protokolování. Chování lze změnit, pokud zahrnout operátor je ignorován throw nebo nic nestane. To se provádí při nastavování možnosti pro váš kontext – obvykle ve `DbContext.OnConfiguring`, nebo v `Startup.cs` Pokud používáte ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>explicitní načítání

> [!NOTE]  
> Tato funkce byla zavedená v EF základní 1.1.

Můžete explicitně načíst navigační vlastnost prostřednictvím `DbContext.Entry(...)` rozhraní API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Můžete také explicitně načíst navigační vlastnost spuštěním další dotaz, který vrací související entity. Pokud je povoleno sledování změn, pak při načítání entitu, EF základní bude automaticky nastavit navigační vlastnosti nově načíst entitiy odkazovat na všechny entity již načten a vlastnosti navigace entit již načten k odkazování na nově načíst entity.

### <a name="querying-related-entities"></a>Dotazování entit v relaci

Můžete také získat LINQ dotazu, který reprezentuje obsah navigační vlastnosti.

To umožňuje provádět akce, jako je například spuštění agregační operátor přes související entity bez jejich načtení do paměti.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Můžete také filtrovat, které entit v relaci jsou načtena do paměti.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>opožděného načítání

Opožděného načítání ještě nepodporuje EF jádra. Můžete zobrazit [opožděného načítání položky na našem nevyřízených položek](https://github.com/aspnet/EntityFramework/issues/3797) sledovat tuto funkci.

## <a name="related-data-and-serialization"></a>Související data a serializace

Protože základní EF bude automaticky opravu up navigační vlastnosti, můžete skončili s cykly v grafu objektu. Například načítání blog a nesouvisí způsobí příspěvcích na blogu objekt, který odkazuje na kolekci zpráv. Každý z těchto příspěvcích bude mít odkaz na blogu.

Některé architektury serializace neumožňují takové cykly. Například Json.NET vyvolá následující výjimky, pokud je encoutered cyklus.

> Newtonsoft.Json.JsonSerializationException: Vlastní odkazující na smyčky zjištěna vlastnost 'Blog' typu 'MyApplication.Models.Blog'.

Pokud používáte ASP.NET Core, můžete nakonfigurovat Json.NET ignorovat cykly, které najde v grafu objektů. To se provádí v `ConfigureServices(...)` metoda v `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
