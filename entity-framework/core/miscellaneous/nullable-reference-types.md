---
title: Práce s typy odkazů s možnou hodnotou null – EF Core
author: roji
ms.date: 9/9/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: 1aa49f1ed518dfe4c13976cef8e868f58d12d5a3
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204415"
---
# <a name="working-with-nullable-reference-types"></a>Práce s typy odkazů s možnou hodnotou null

C#8 zavádí novou funkci, která se nazývá [typ odkazu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types), což umožňuje odkazovat na typy odkazů, které označují, zda jsou pro ně platné hodnoty null nebo ne. Pokud s touto funkcí začínáte, doporučujeme, abyste s ní obeznámeni tím, že si přečtete C# dokumentaci.

Tato stránka představuje podporu EF Core podporují typy odkazů s možnou hodnotou null a popisuje osvědčené postupy pro práci s nimi.

## <a name="required-and-optional-properties"></a>Povinné a volitelné vlastnosti

Hlavní dokumentace k vyžadovaným a volitelným vlastnostem a jejich interakci s odkazem s možnou hodnotou null je stránka [požadovaná a volitelná vlastnost](xref:core/modeling/required-optional) . Doporučujeme, abyste si nejdřív načetli tuto stránku.

> [!NOTE]
> Cvičení opatrnosti při povolování typů odkazů s možnou hodnotou null u existujícího projektu: vlastnosti referenčního typu, které byly dříve nakonfigurovány jako volitelné, budou nyní konfigurovány jako povinné, pokud nejsou explicitně opatřeny poznámkami k Nullable. Při správě schématu relační databáze to může způsobit, že se budou vygenerovat migrace, které změní hodnotu vlastnosti Database na sloupec.

## <a name="dbcontext-and-dbset"></a>DbContext a Negenerickými

Pokud jsou povoleny typy odkazů s možnou C# hodnotou null, kompilátor vygeneruje upozornění pro jakoukoli neinicializovaná vlastnost, která neumožňuje hodnotu null, protože by obsahovala hodnotu null. V důsledku toho se při běžném postupu definování nenullable `DbSet` v kontextu nyní vygeneruje upozornění. EF Core však vždy inicializuje všechny `DbSet` vlastnosti u typů odvozených od DbContext, takže je zaručeno, že nikdy nebudete mít hodnotu null, a to ani v případě, že to kompilátor neví. Proto se doporučuje zachovat `DbSet` vlastnosti, které neumožňují hodnotu null – umožňuje přístup k nim bez kontrol hodnot null – a pro tichá upozornění kompilátoru explicitním nastavením na hodnotu null s použitím operátoru null-striktní (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Vlastnosti a inicializace bez hodnoty null

Upozornění kompilátoru pro neinicializované typy odkazů, které neumožňují hodnotu null, jsou také problémem pro běžné vlastnosti vašich typů entit. V našem příkladu jsme se těmto upozorněním vyhnuli pomocí [vazby konstruktoru](xref:core/modeling/constructors), funkce, která funguje dokonale s vlastnostmi bez hodnoty null, a zajišťuje tak jejich vždycky inicializaci. V některých scénářích však vazba konstruktoru není možnost: navigační vlastnosti lze například inicializovat tímto způsobem.

Požadované navigační vlastnosti představují další potíže: i když pro daný objekt zabezpečení bude vždy existovat závislá hodnota, může nebo nemusí být načtena konkrétním dotazem v závislosti na potřebách v tomto okamžiku v programu ([Viz různé vzory pro načítání dat](xref:core/querying/related-data)). Ve stejnou dobu je nežádoucí tyto vlastnosti nastavit na hodnotu null, protože by to vyžadovalo, aby se všem přístupům kontrolovaly hodnoty null, i když jsou požadovány.

Jedním ze způsobů, jak s těmito scénáři pracovat, je mít vlastnost nepovolující hodnotu null s [polem](xref:core/modeling/backing-field), které je k dispozici jako null:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=12-17)]

Vzhledem k tomu, že navigační vlastnost nemůže mít hodnotu null, je nakonfigurovaná požadovaná navigace. a pokud je navigace správně načtená, bude závislý přístup prostřednictvím vlastnosti. Pokud je však vlastnost k dispozici bez předchozího správného načtení související entity, je vyvolána událost InvalidOperationException, protože kontrakt rozhraní API byl nesprávně použit.

Terser alternativou je možné jednoduše inicializovat vlastnost na hodnotu null s použitím operátoru null-striktní (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

Skutečná hodnota null nebude nikdy zaznamenána s výjimkou důsledků programátorské chyby, například přístup k vlastnosti navigace, aniž by bylo možné související entitu předem správně načíst.

> [!NOTE]
> Navigace v kolekci, která obsahuje odkazy na více souvisejících entit, by neměla vždy mít hodnotu null. Prázdná kolekce znamená, že žádné související entity neexistují, ale samotný seznam by nikdy neměl mít hodnotu null.

## <a name="navigating-and-including-nullable-relationships"></a>Navigace a zahrnutí vztahů s možnou hodnotou null

Při použití volitelných vztahů je možné narazit na upozornění kompilátoru, kde by mohla být skutečná výjimka nulového odkazu nemožná. Při překladu a spouštění dotazů LINQ EF Core zaručuje, že pokud existující související entita neexistuje, bude se místo vyvolání jednoduše ignorovat jakákoli navigace. Nicméně kompilátor neví o této EF Core záruky a vytváří upozornění, jako kdyby byl dotaz LINQ proveden v paměti s LINQ to Objects. V důsledku toho je nutné použít operátor null-striktní (!) k informování kompilátoru, že skutečná hodnota null není možná:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

K podobnému problému dochází při zahrnutí více úrovní vztahů mezi volitelnými navigacemi:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

Pokud sami zjistíte, že se jedná o spoustu dat, a tyto typy entit jsou v EF Corech dotazech převážně v podstatě (nebo výhradně), zvažte, že vlastnosti navigace neumožňují hodnotu null a nakonfigurujete je jako volitelné prostřednictvím rozhraní Fluent API nebo datových poznámek. Tím se odeberou všechna upozornění kompilátoru a zůstane relace volitelná; Pokud jsou však vaše entity procházeny mimo EF Core, můžete sledovat hodnoty null, i když jsou vlastnosti poznámy jako neumožňující hodnotu null.

## <a name="scaffolding"></a>Generování uživatelského rozhraní

[Funkce C# referenčního typu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types) není v současné době v zpětné analýze podporovaná: EF Core vždy generuje C# kód, který předpokládá, že funkce je vypnuta. Například textové sloupce s možnou hodnotou null budou vygenerované jako vlastnost s typem `string` , ne `string?`, pomocí rozhraní Fluent API nebo datových poznámek, které slouží ke konfiguraci, jestli je vlastnost požadovaná nebo ne. Můžete upravit generovaný kód a nahradit je poznámkami, které C# mají hodnotu null. Podpora generování uživatelského rozhraní pro typy odkazů s možnou hodnotou null je sledována pomocí [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)problému.