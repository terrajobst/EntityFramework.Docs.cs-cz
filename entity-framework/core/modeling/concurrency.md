---
title: "Tokeny souběžnosti - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="2ebc9-102">Tokeny souběžnosti</span><span class="sxs-lookup"><span data-stu-id="2ebc9-102">Concurrency Tokens</span></span>

<span data-ttu-id="2ebc9-103">Pokud vlastnost je nakonfigurovaný jako token souběžnosti pak EF zkontroluje, že žádný jiný uživatel změnil tuto hodnotu v databázi při ukládání změn do záznamů.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="2ebc9-104">EF používá optimistickou metodu souběžného vzor, znamená se bude předpokládat, že hodnota nebyla změněna a pokuste se ukládat data, ale výjimku, pokud zjistí, že byla hodnota změněna.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="2ebc9-105">Například může chceme konfigurovat `LastName` na `Person` být token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="2ebc9-106">To znamená, že pokud jeden uživatel se pokusí uložit některé změny `Person`, ale došlo ke změně jiného uživatele `LastName` pak bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="2ebc9-107">To může být žádoucí, aby vaše aplikace může vyzvat uživatele k Ujistěte se, že tento záznam stále představuje stejná skutečná osoba před uložením jejich změny.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="2ebc9-108">Tato stránka dokumenty postup konfigurace tokenů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="2ebc9-109">V tématu [zpracování souběžnosti](../saving/concurrency.md) příklady, jak použít optimistickou metodu souběžného v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="2ebc9-110">Jak fungují tokenů souběžnosti v EF</span><span class="sxs-lookup"><span data-stu-id="2ebc9-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="2ebc9-111">Úložiště dat můžete vynutit tokenů souběžnosti kontrolou, že všechny záznam bude aktualizován nebo odstraněn stále má stejnou hodnotu pro concurrency token, který byl přiřazen při kontext původně načtení dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="2ebc9-112">Například relačních databází dosáhnout zahrnutím token souběžnosti v `WHERE` klauzule libovolného `UPDATE` nebo `DELETE` příkazy a kontrola počet řádků, které situace měla vliv na.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="2ebc9-113">Pokud token souběžnosti stále shoduje se bude aktualizovat jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="2ebc9-114">Pokud se změnila hodnota v databázi, jsou aktualizovány žádné řádky.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="2ebc9-115">Konvence</span><span class="sxs-lookup"><span data-stu-id="2ebc9-115">Conventions</span></span>

<span data-ttu-id="2ebc9-116">Podle konvence jsou nakonfigurovány jako tokenů souběžnosti nikdy vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2ebc9-117">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="2ebc9-117">Data Annotations</span></span>

<span data-ttu-id="2ebc9-118">Můžete konfigurovat vlastnosti jako token souběžnosti datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="2ebc9-119">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="2ebc9-119">Fluent API</span></span>

<span data-ttu-id="2ebc9-120">Rozhraní Fluent API můžete konfigurovat vlastnosti jako token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="2ebc9-121">Časové razítko či řádku verze</span><span class="sxs-lookup"><span data-stu-id="2ebc9-121">Timestamp/row version</span></span>

<span data-ttu-id="2ebc9-122">Časové razítko je vlastnost, kde nová hodnota je generován databázi pokaždé, když se přidají nebo aktualizují řádek.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="2ebc9-123">Vlastnost je také považovány za token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="2ebc9-124">Tím se zajistí, že obdržíte výjimku, pokud jiné změnil řádek, který se pokoušíte aktualizovat, protože dotaz pro data.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="2ebc9-125">Jak to se dá dosáhnout závisí používaný zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="2ebc9-126">Pro systém SQL Server, se obvykle používá časové razítko na *byte []* vlastnosti, která bude instalační program jako *ROWVERSION* sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="2ebc9-127">Konvence</span><span class="sxs-lookup"><span data-stu-id="2ebc9-127">Conventions</span></span>

<span data-ttu-id="2ebc9-128">Podle konvence jsou vlastnosti nikdy nakonfigurovány jako časová razítka.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="2ebc9-129">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="2ebc9-129">Data Annotations</span></span>

<span data-ttu-id="2ebc9-130">Můžete konfigurovat vlastnosti podobě časového razítka datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="2ebc9-131">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="2ebc9-131">Fluent API</span></span>

<span data-ttu-id="2ebc9-132">Rozhraní Fluent API můžete konfigurovat vlastnosti podobě časového razítka.</span><span class="sxs-lookup"><span data-stu-id="2ebc9-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
