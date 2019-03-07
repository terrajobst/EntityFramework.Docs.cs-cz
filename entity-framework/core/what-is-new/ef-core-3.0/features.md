---
title: Novinky v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: cf0d2cf032b9aa319fe706aece5b1ea66a5d6251
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463388"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Novým funkcím zahrnutým v EF Core 3.0 (aktuálně ve verzi preview)

> [!IMPORTANT]
> Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.

Následující seznam obsahuje hlavní nové funkce pro EF Core 3.0.
Většina těchto funkcí nejsou zahrnuté v aktuální verzi preview, ale bude k dispozici během postupu směrem k RTM.

Důvodem je, že na začátku uvolnění se Zaměřujeme na implementaci plánované [rozbíjející změny v](xref:core/what-is-new/ef-core-3.0/breaking-changes).
Mnohé z těchto novinkách jsou vylepšení na EF Core na své vlastní.
Řada dalších vyžadovaných pro odblokování dalších vylepšení. 

Úplný seznam oprav chyb a vylepšení probíhá, zobrazí se [tento dotaz v našich sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Vylepšení LINQ 

[Sledování problému #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview.

LINQ umožňuje psát dotazy databáze, aniž byste museli opustit váš jazyk podle vlastní volby, využití výhod bohaté zadejte informace, které pomůžou technologie IntelliSense a kontrola typu v době kompilace.
Ale LINQ také umožňuje psát neomezený počet složité dotazy a, který byl vždy velkým problémem pro zprostředkovatele LINQ.
V prvních několika verzích EF Core jsme vyřešit, v části tím, jaké části dotazu může být přeloženy na SQL a tím, že zbývající části dotazu ke spuštění v paměti na straně klienta.
Toto spuštění na straně klienta může být žádoucí v některých situacích ale v mnoha jiných případech může vést neefektivní dotazy, které nejsou označené, dokud je aplikace nasazená do produkčního prostředí.
V EF Core 3.0 plánujeme změnit velký fungování naše implementace LINQ, a jak můžeme otestovat.
Cíle jsou k němu robustnější (třeba, aby se zabránilo přerušení dotazů v aktualizací), Povolit překlad více výrazů správně do databáze SQL, vygenerujte efektivní dotazy ve více případech a zabránit v přechodu nezjištěné neefektivní dotazy.

## <a name="cosmos-db-support"></a>Podpora služby cosmos DB 

[Sledování problému #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Tato funkce je zahrnutá v aktuální verzi preview, ale ještě není dokončena. 

Pracujeme na poskytovatele služby Cosmos DB pro jádro EF Core a umožňuje vývojářům znáte model programování na EF snadno cílit na jako databáze aplikace služby Azure Cosmos DB.
Cílem je, že některé z výhod Cosmos DB, jako jsou globální distribuce "always on" dostupnost, elastické škálovatelnosti a nízké latenci, ještě více přístupné pro vývojáře na platformě .NET.
Zprostředkovatel umožní většina EF Core funkcí, jako je sledování automatických změn, LINQ a převody hodnot, s využitím rozhraní SQL API ve službě Cosmos DB.
Začali jsme snaha před EF Core 2.2 a [jsme se rozhodli některé verze zprostředkovatele k dispozici ve verzi preview](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Nový plán má pokračovat ve vývoji poskytovatele spolu s EF Core 3.0. 

## <a name="c-80-support"></a>C#Podpora 8.0

[Sledování problému #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[sledování problému #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview.

Chceme našim zákazníkům využívat některé části [nové funkce v C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) , jako jsou asynchronní datové proudy (včetně `await foreach`) a typy s možnou hodnotou Null odkazů pomocí EF Core.

## <a name="reverse-engineering-of-database-views"></a>Zpětná analýza zobrazení databáze

[Sledování problému #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Tato funkce není součástí aktuální verzi preview.

[Typy dotazů](xref:core/modeling/query-types), zavedená v EF Core 2.1 a považován za typy entit bez klíčů v EF Core 3.0, představují data, která mohou číst z databáze, ale nejde aktualizovat.
Tato vlastnost je mezi nimi vlastně ideálně se hodí pro zobrazení databáze ve většině scénářů, abychom plánovat k automatizaci vytváření typů entit bez klíčů při zpětné analýze zobrazení databáze.

## <a name="property-bag-entities"></a>Vlastnosti kontejneru objektů a dat entit 

[Sledování problému #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) a [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview. 

Tato funkce je o povolení entity, které ukládají data v indexované vlastnosti namísto regulární vlastností a také o bude možné použít instance stejné třídy .NET (potenciálně něco jednoduchého jako `Dictionary<string, object>`) k reprezentaci entit různých typů ve stejném modelu EF Core.
Tato funkce je odrazový můstek k podpoře many-to-many relací bez připojení k entitě, což je jedna z nejžádanějších vylepšení pro jádro EF Core.

## <a name="ef-63-on-net-core"></a>EF 6.3 v rozhraní .NET Core 

[Sledování problému EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271)

Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview. 

Chápeme, že mnoho existujících aplikací pomocí předchozí verze EF a že přenesení do EF Core jenom, abyste mohli využívat výhod .NET Core může někdy vyžadovat značné úsilí.
Z tohoto důvodu jsme se přizpůsobení další verze EF 6 a spustit na .NET Core 3.0.
To usnadňuje přenos stávající aplikace s minimálními změnami provádíme.
Existuje budou představovat určitá omezení. Příklad:
- Bude vyžadovat noví zprostředkovatelé pro práci s jinými databázemi kromě podpory systému SQL Server na .NET Core
- Prostorový podporu systému SQL Server nebude povolen

Všimněte si také, že neexistují žádné nové plánované funkce pro EF 6 v tomto okamžiku.
