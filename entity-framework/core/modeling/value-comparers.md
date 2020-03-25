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
# <a name="value-comparers"></a><span data-ttu-id="d4989-103">Porovnávače hodnot</span><span class="sxs-lookup"><span data-stu-id="d4989-103">Value Comparers</span></span>

> [!NOTE]  
> <span data-ttu-id="d4989-104">Tato funkce je v EF Core 3,0 novinkou.</span><span class="sxs-lookup"><span data-stu-id="d4989-104">This feature is new in EF Core 3.0.</span></span>

> [!TIP]  
> <span data-ttu-id="d4989-105">Kód v tomto dokumentu najdete na GitHubu jako [spustitelný Sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span><span class="sxs-lookup"><span data-stu-id="d4989-105">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span></span>

## <a name="background"></a><span data-ttu-id="d4989-106">Pozadí</span><span class="sxs-lookup"><span data-stu-id="d4989-106">Background</span></span>

<span data-ttu-id="d4989-107">EF Core musí porovnat hodnoty vlastností, když:</span><span class="sxs-lookup"><span data-stu-id="d4989-107">EF Core needs to compare property values when:</span></span>

* <span data-ttu-id="d4989-108">Zjištění, zda byla vlastnost změněna v rámci [zjišťování změn aktualizací](xref:core/saving/basic)</span><span class="sxs-lookup"><span data-stu-id="d4989-108">Determining whether a property has been changed as part of [detecting changes for updates](xref:core/saving/basic)</span></span>
* <span data-ttu-id="d4989-109">Určení, zda jsou dvě hodnoty klíče stejné při řešení vztahů</span><span class="sxs-lookup"><span data-stu-id="d4989-109">Determining whether two key values are the same when resolving relationships</span></span> 

<span data-ttu-id="d4989-110">To se zpracovává automaticky pro běžné primitivní typy, jako je int, bool, DateTime atd.</span><span class="sxs-lookup"><span data-stu-id="d4989-110">This is handled automatically for common primitive types such as int, bool, DateTime, etc.</span></span>

<span data-ttu-id="d4989-111">U složitějších typů je třeba provést volby jako způsob porovnání.</span><span class="sxs-lookup"><span data-stu-id="d4989-111">For more complex types, choices need to be made as to how to do the comparison.</span></span>
<span data-ttu-id="d4989-112">Například pole bajtů může být porovnáno:</span><span class="sxs-lookup"><span data-stu-id="d4989-112">For example, a byte array could be compared:</span></span>

* <span data-ttu-id="d4989-113">Podle odkazu je to, že rozdíl je zjištěn pouze v případě, že je použito nové bajtové pole</span><span class="sxs-lookup"><span data-stu-id="d4989-113">By reference, such that a difference is only detected if a new byte array is used</span></span>
* <span data-ttu-id="d4989-114">Důkladným porovnáním, aby se zjistila mutace bajtů v poli</span><span class="sxs-lookup"><span data-stu-id="d4989-114">By deep comparison, such that mutation of the bytes in the array is detected</span></span>

<span data-ttu-id="d4989-115">Ve výchozím nastavení EF Core používá první z těchto přístupů pro pole neklíčového bajtu.</span><span class="sxs-lookup"><span data-stu-id="d4989-115">By default, EF Core uses the first of these approaches for non-key byte arrays.</span></span>
<span data-ttu-id="d4989-116">To znamená, že jsou porovnány pouze odkazy a byla zjištěna změna pouze v případě, že je existující bajtové pole nahrazeno novým.</span><span class="sxs-lookup"><span data-stu-id="d4989-116">That is, only references are compared and a change is detected only when an existing byte array is replaced with a new one.</span></span>
<span data-ttu-id="d4989-117">Toto je narušené rozhodnutí, které při provádění metody SaveChanges vylučuje hloubkové porovnání mnoha velkých bajtových polí.</span><span class="sxs-lookup"><span data-stu-id="d4989-117">This is a pragmatic decision that avoids deep comparison of many large byte arrays when executing SaveChanges.</span></span>
<span data-ttu-id="d4989-118">Ale běžný scénář nahrazení, vyslovení a obrázku s jiným obrázkem, je zpracováván způsobem.</span><span class="sxs-lookup"><span data-stu-id="d4989-118">But the common scenario of replacing, say, an image with a different image is handled in a performant way.</span></span>

<span data-ttu-id="d4989-119">Na druhé straně odkazování by nefungovalo, pokud se pole bajtů používá k reprezentaci binárních klíčů.</span><span class="sxs-lookup"><span data-stu-id="d4989-119">On the other hand, reference equality would not work when byte arrays are used to represent binary keys.</span></span>
<span data-ttu-id="d4989-120">Je velmi pravděpodobné, že vlastnost FK je nastavena na _stejnou instanci_ jako vlastnost PK, pro kterou je třeba ji porovnat.</span><span class="sxs-lookup"><span data-stu-id="d4989-120">It's very unlikely that an FK property is set to the _same instance_ as a PK property to which it needs to be compared.</span></span>
<span data-ttu-id="d4989-121">Proto EF Core používá hloubková porovnání pro pole bajtů fungující jako klíče.</span><span class="sxs-lookup"><span data-stu-id="d4989-121">Therefore, EF Core uses deep comparisons for byte arrays acting as keys.</span></span>
<span data-ttu-id="d4989-122">Je pravděpodobné, že by se dosáhlo velkého výkonu, protože binární klíče jsou obvykle krátké.</span><span class="sxs-lookup"><span data-stu-id="d4989-122">This is unlikely to have a big performance hit since binary keys are usually short.</span></span>

### <a name="snapshots"></a><span data-ttu-id="d4989-123">Snímky</span><span class="sxs-lookup"><span data-stu-id="d4989-123">Snapshots</span></span>

<span data-ttu-id="d4989-124">Důkladné porovnání na proměnlivých typech znamená, že EF Core potřebuje možnost vytvořit hlubokou "hodnotu" snímku hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d4989-124">Deep comparisons on mutable types means that EF Core needs the ability to create a deep "snapshot" of the property value.</span></span>
<span data-ttu-id="d4989-125">Pouze kopírování odkazu by vedlo k tomu, že by se použila aktuální hodnota i snímek, protože se jedná _o stejný objekt_.</span><span class="sxs-lookup"><span data-stu-id="d4989-125">Just copying the reference instead would result in mutating both the current value and the snapshot, since they are _the same object_.</span></span>
<span data-ttu-id="d4989-126">Proto pokud jsou na proměnlivých typech použity hloubková porovnání, jsou také požadovány důkladné snapshotting.</span><span class="sxs-lookup"><span data-stu-id="d4989-126">Therefore, when deep comparisons are used on mutable types, deep snapshotting is also required.</span></span>

## <a name="properties-with-value-converters"></a><span data-ttu-id="d4989-127">Vlastnosti s převaděči hodnot</span><span class="sxs-lookup"><span data-stu-id="d4989-127">Properties with value converters</span></span>

<span data-ttu-id="d4989-128">V případě výše EF Core má podpora nativního mapování pro pole bajtů a tak může automaticky zvolit příslušné výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d4989-128">In the case above, EF Core has native mapping support for byte arrays and so can automatically choose appropriate defaults.</span></span>
<span data-ttu-id="d4989-129">Pokud je však vlastnost mapována pomocí [konvertoru hodnot](xref:core/modeling/value-conversions), EF Core nemůže vždy určit příslušné porovnání, které se má použít.</span><span class="sxs-lookup"><span data-stu-id="d4989-129">However, if the property is mapped through a [value converter](xref:core/modeling/value-conversions), then EF Core can't always determine the appropriate comparison to use.</span></span>
<span data-ttu-id="d4989-130">Místo toho EF Core vždy používá výchozí porovnání rovnosti definované typem vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d4989-130">Instead, EF Core always uses the default equality comparison defined by the type of the property.</span></span>
<span data-ttu-id="d4989-131">To je často správné, ale může být potřeba je přepsat při mapování složitějších typů.</span><span class="sxs-lookup"><span data-stu-id="d4989-131">This is often correct, but may need to be overridden when mapping more complex types.</span></span>

### <a name="simple-immutable-classes"></a><span data-ttu-id="d4989-132">Jednoduché neměnné třídy</span><span class="sxs-lookup"><span data-stu-id="d4989-132">Simple immutable classes</span></span>

<span data-ttu-id="d4989-133">Zvažte, že vlastnost používá konvertor hodnot k namapování jednoduché, neměnné třídy.</span><span class="sxs-lookup"><span data-stu-id="d4989-133">Consider a property the uses a value converter to map a simple, immutable class.</span></span>

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

<span data-ttu-id="d4989-134">Vlastnosti tohoto typu nevyžadují speciální porovnání nebo snímky z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="d4989-134">Properties of this type do not need special comparisons or snapshots because:</span></span>
* <span data-ttu-id="d4989-135">Rovnost je přepsaná, aby se různé instance správně porovnaly.</span><span class="sxs-lookup"><span data-stu-id="d4989-135">Equality is overridden so that different instances will compare correctly</span></span>
* <span data-ttu-id="d4989-136">Typ je neproměnlivý, takže není možné pořídit hodnotu snímku.</span><span class="sxs-lookup"><span data-stu-id="d4989-136">The type is immutable, so there is no chance of mutating a snapshot value</span></span>

<span data-ttu-id="d4989-137">Takže v tomto případě je výchozí chování EF Core v pořádku.</span><span class="sxs-lookup"><span data-stu-id="d4989-137">So in this case the default behavior of EF Core is fine as it is.</span></span>

### <a name="simple-immutable-structs"></a><span data-ttu-id="d4989-138">Jednoduché neměnné struktury</span><span class="sxs-lookup"><span data-stu-id="d4989-138">Simple immutable Structs</span></span>

<span data-ttu-id="d4989-139">Mapování jednoduchých struktur je také jednoduché a nevyžaduje žádné speciální porovnávače ani snapshotting.</span><span class="sxs-lookup"><span data-stu-id="d4989-139">The mapping for simple structs is also simple and requires no special comparers or snapshotting.</span></span>

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

<span data-ttu-id="d4989-140">EF Core obsahuje integrovanou podporu pro generování kompilovaných kopírování členů porovnání vlastností struktury.</span><span class="sxs-lookup"><span data-stu-id="d4989-140">EF Core has built-in support for generating compiled, memberwise comparisons of struct properties.</span></span>
<span data-ttu-id="d4989-141">To znamená, že struktury nemusí mít u EF přepsat rovnost, ale přesto se můžete rozhodnout k tomu, aby to bylo možné provést [z jiných důvodů](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span><span class="sxs-lookup"><span data-stu-id="d4989-141">This means structs don't need to have equality overridden for EF, but you may still choose to do this for [other reasons](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span></span>
<span data-ttu-id="d4989-142">Speciální snapshotting není potřeba, protože struktury jsou neměnné a pořád se kopírování členů zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="d4989-142">Also, special snapshotting is not needed since structs immutable and are always memberwise copied anyway.</span></span>
<span data-ttu-id="d4989-143">(To platí také pro proměnlivé struktury, ale [měly by být obecně zabráněno proměnlivým strukturám](/dotnet/csharp/write-safe-efficient-code).)</span><span class="sxs-lookup"><span data-stu-id="d4989-143">(This is also true for mutable structs, but [mutable structs should in general be avoided](/dotnet/csharp/write-safe-efficient-code).)</span></span>

### <a name="mutable-classes"></a><span data-ttu-id="d4989-144">Měnitelné třídy</span><span class="sxs-lookup"><span data-stu-id="d4989-144">Mutable classes</span></span>

<span data-ttu-id="d4989-145">Doporučuje se používat neměnné typy (třídy nebo struktury) s převaděči hodnot, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="d4989-145">It is recommended that you use immutable types (classes or structs) with value converters when possible.</span></span>
<span data-ttu-id="d4989-146">To je obvykle efektivnější a má sémantiku čištění, než použití měnitelného typu.</span><span class="sxs-lookup"><span data-stu-id="d4989-146">This is usually more efficient and has cleaner semantics than using a mutable type.</span></span>

<span data-ttu-id="d4989-147">Nicméně, je-li to však řečeno, je běžné používat vlastnosti typů, které aplikace nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="d4989-147">However, that being said, it is common to use properties of types that the application cannot change.</span></span>
<span data-ttu-id="d4989-148">Například mapování vlastnosti obsahující seznam čísel:</span><span class="sxs-lookup"><span data-stu-id="d4989-148">For example, mapping a property containing a list of numbers:</span></span> 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

<span data-ttu-id="d4989-149">[Třída`List<T>`](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span><span class="sxs-lookup"><span data-stu-id="d4989-149">The [`List<T>` class](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span></span>
* <span data-ttu-id="d4989-150">Má referenční rovnost; dva seznamy obsahující stejné hodnoty jsou považovány za odlišné.</span><span class="sxs-lookup"><span data-stu-id="d4989-150">Has reference equality; two lists containing the same values are treated as different.</span></span>
* <span data-ttu-id="d4989-151">Je proměnlivá; hodnoty v seznamu lze přidat a odebrat.</span><span class="sxs-lookup"><span data-stu-id="d4989-151">Is mutable; values in the list can be added and removed.</span></span>

<span data-ttu-id="d4989-152">Typický převod hodnoty na vlastnost seznamu může převést seznam na JSON a z něj:</span><span class="sxs-lookup"><span data-stu-id="d4989-152">A typical value conversion on a list property might convert the list to and from JSON:</span></span>

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

<span data-ttu-id="d4989-153">To pak vyžaduje nastavení `ValueComparer<T>` na vlastnost pro vynucení EF Core použití správného porovnání s tímto převodem:</span><span class="sxs-lookup"><span data-stu-id="d4989-153">This then requires setting a `ValueComparer<T>` on the property to force EF Core use correct comparisons with this conversion:</span></span>

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> <span data-ttu-id="d4989-154">Rozhraní API pro sestavování modelů ("Fluent") pro nastavení porovnávací hodnoty ještě není implementované.</span><span class="sxs-lookup"><span data-stu-id="d4989-154">The model builder ("fluent") API to set a value comparer has not yet been implemented.</span></span>
> <span data-ttu-id="d4989-155">Místo toho kód výše volá SetValueComparer na nižší úrovni IMutableProperty vystavené tvůrcem jako metadata.</span><span class="sxs-lookup"><span data-stu-id="d4989-155">Instead, the code above calls SetValueComparer on the lower-level IMutableProperty exposed by the builder as 'Metadata'.</span></span>

<span data-ttu-id="d4989-156">Konstruktor `ValueComparer<T>` přijímá tři výrazy:</span><span class="sxs-lookup"><span data-stu-id="d4989-156">The `ValueComparer<T>` constructor accepts three expressions:</span></span>
* <span data-ttu-id="d4989-157">Výraz pro kontrolu kvality</span><span class="sxs-lookup"><span data-stu-id="d4989-157">An expression for checking quality</span></span>
* <span data-ttu-id="d4989-158">Výraz pro generování kódu hash</span><span class="sxs-lookup"><span data-stu-id="d4989-158">An expression for generating a hash code</span></span>
* <span data-ttu-id="d4989-159">Výraz pro vytvoření snímku hodnoty</span><span class="sxs-lookup"><span data-stu-id="d4989-159">An expression to snapshot a value</span></span>  

<span data-ttu-id="d4989-160">V tomto případě je porovnání provedeno pomocí kontroly, zda jsou sekvence čísel stejné.</span><span class="sxs-lookup"><span data-stu-id="d4989-160">In this case the comparison is done by checking if the sequences of numbers are the same.</span></span>

<span data-ttu-id="d4989-161">Podobně je kód hash sestaven z stejné sekvence.</span><span class="sxs-lookup"><span data-stu-id="d4989-161">Likewise, the hash code is built from this same sequence.</span></span>
<span data-ttu-id="d4989-162">(Všimněte si, že se jedná o kód hash přes proměnlivé hodnoty, a proto může [způsobit problémy](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span><span class="sxs-lookup"><span data-stu-id="d4989-162">(Note that this is a hash code over mutable values and hence can [cause problems](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span></span>
<span data-ttu-id="d4989-163">Neměnné místo, pokud je to možné.)</span><span class="sxs-lookup"><span data-stu-id="d4989-163">Be immutable instead if you can.)</span></span>

<span data-ttu-id="d4989-164">Snímek se vytvoří klonováním seznamu pomocí ToList –.</span><span class="sxs-lookup"><span data-stu-id="d4989-164">The snapshot is created by cloning the list with ToList.</span></span>
<span data-ttu-id="d4989-165">To je možné znovu jenom v případě, že se budou používat seznamy.</span><span class="sxs-lookup"><span data-stu-id="d4989-165">Again, this is only needed if the lists are going to be mutated.</span></span>
<span data-ttu-id="d4989-166">Neměnné místo, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="d4989-166">Be immutable instead if you can.</span></span> 

> [!NOTE]  
> <span data-ttu-id="d4989-167">Převaděče hodnot a porovnávače se vytvářejí pomocí výrazů místo jednoduchých delegátů.</span><span class="sxs-lookup"><span data-stu-id="d4989-167">Value converters and comparers are constructed using expressions rather than simple delegates.</span></span>
> <span data-ttu-id="d4989-168">Je to proto, že EF vloží tyto výrazy do mnohem složitějšího stromu výrazů, který je poté zkompilován do delegáta Shaper entity.</span><span class="sxs-lookup"><span data-stu-id="d4989-168">This is because EF inserts these expressions into a much more complex expression tree that is then compiled into an entity shaper delegate.</span></span>
> <span data-ttu-id="d4989-169">V koncepčním případě je to podobné jako vkládání kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="d4989-169">Conceptually, this is similar to compiler inlining.</span></span>
> <span data-ttu-id="d4989-170">Například jednoduchý převod může být pouze zkompilovaný v přetypování, nikoli volání jiné metody pro provedení převodu.</span><span class="sxs-lookup"><span data-stu-id="d4989-170">For example, a simple conversion may just be a compiled in cast, rather than a call to another method to do the conversion.</span></span>    

### <a name="key-comparers"></a><span data-ttu-id="d4989-171">Porovnávače klíčů</span><span class="sxs-lookup"><span data-stu-id="d4989-171">Key comparers</span></span>

<span data-ttu-id="d4989-172">Oddíl na pozadí obsahuje informace o tom, proč porovnávání klíčů může vyžadovat speciální sémantiku.</span><span class="sxs-lookup"><span data-stu-id="d4989-172">The background section covers why key comparisons may require special semantics.</span></span>
<span data-ttu-id="d4989-173">Nezapomeňte vytvořit porovnávací metodu, která je vhodná pro klíče při jejich nastavování na primární vlastnost, objekt zabezpečení nebo cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="d4989-173">Make sure to create a comparer that is appropriate for keys when setting it on a primary, principal, or foreign key property.</span></span>

<span data-ttu-id="d4989-174">Použijte [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) ve výjimečných případech, kde se pro stejnou vlastnost vyžaduje odlišná sémantika.</span><span class="sxs-lookup"><span data-stu-id="d4989-174">Use [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) in the rare cases where different semantics is required on the same property.</span></span>

> [!NOTE]  
> <span data-ttu-id="d4989-175">SetStructuralComparer je zastaralá v EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="d4989-175">SetStructuralComparer has been obsoleted in EF Core 5.0.</span></span>
> <span data-ttu-id="d4989-176">Místo toho použijte SetKeyValueComparer.</span><span class="sxs-lookup"><span data-stu-id="d4989-176">Use SetKeyValueComparer instead.</span></span>

### <a name="overriding-defaults"></a><span data-ttu-id="d4989-177">Přepisování výchozích hodnot</span><span class="sxs-lookup"><span data-stu-id="d4989-177">Overriding defaults</span></span>

<span data-ttu-id="d4989-178">V některých případech nemusí být výchozí porovnání používané EF Core vhodné.</span><span class="sxs-lookup"><span data-stu-id="d4989-178">Sometimes the default comparison used by EF Core may not be appropriate.</span></span>
<span data-ttu-id="d4989-179">Například mutace z bajtových polí nejsou ve výchozím nastavení zjištěny v EF Core.</span><span class="sxs-lookup"><span data-stu-id="d4989-179">For example, mutation of byte arrays is not, by default, detected in EF Core.</span></span>
<span data-ttu-id="d4989-180">To lze přepsat nastavením jiné porovnávací metody na vlastnost:</span><span class="sxs-lookup"><span data-stu-id="d4989-180">This can be overridden by setting a different comparer on the property:</span></span> 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

<span data-ttu-id="d4989-181">EF Core nyní porovná sekvence bajtů a bude proto detekovat mutace bajtových polí.</span><span class="sxs-lookup"><span data-stu-id="d4989-181">EF Core will now compare byte sequences and will therefore detect byte array mutations.</span></span>
