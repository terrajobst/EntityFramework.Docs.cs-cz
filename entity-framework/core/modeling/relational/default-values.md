---
title: Výchozí hodnoty – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655907"
---
# <a name="default-values"></a><span data-ttu-id="7552e-102">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="7552e-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="7552e-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="7552e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7552e-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="7552e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7552e-105">Výchozí hodnota sloupce je hodnota, která bude vložena při vložení nového řádku, ale pro sloupec není zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="7552e-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="7552e-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="7552e-106">Conventions</span></span>

<span data-ttu-id="7552e-107">Podle konvence není výchozí hodnota nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="7552e-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7552e-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7552e-108">Data Annotations</span></span>

<span data-ttu-id="7552e-109">Výchozí hodnotu nelze nastavit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="7552e-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7552e-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="7552e-110">Fluent API</span></span>

<span data-ttu-id="7552e-111">Rozhraní Fluent API můžete použít k určení výchozí hodnoty pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7552e-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="7552e-112">Můžete také zadat fragment SQL, který se použije k výpočtu výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7552e-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
