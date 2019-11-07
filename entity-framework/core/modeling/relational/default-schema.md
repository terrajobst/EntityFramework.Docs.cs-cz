---
title: Výchozí schéma – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655970"
---
# <a name="default-schema"></a><span data-ttu-id="3c9bf-102">Výchozí schéma</span><span class="sxs-lookup"><span data-stu-id="3c9bf-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="3c9bf-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="3c9bf-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3c9bf-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="3c9bf-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3c9bf-105">Výchozím schématem je schéma databáze, ve kterém budou objekty vytvořeny, pokud není pro daný objekt explicitně nakonfigurováno schéma.</span><span class="sxs-lookup"><span data-stu-id="3c9bf-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="3c9bf-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="3c9bf-106">Conventions</span></span>

<span data-ttu-id="3c9bf-107">Podle konvence poskytovatel databáze zvolí nejvhodnější výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="3c9bf-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="3c9bf-108">Například Microsoft SQL Server použije schéma `dbo` a SQLite nepoužije schéma (vzhledem k tomu, že schémata nejsou podporovaná v SQLite).</span><span class="sxs-lookup"><span data-stu-id="3c9bf-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3c9bf-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="3c9bf-109">Data Annotations</span></span>

<span data-ttu-id="3c9bf-110">Výchozí schéma nelze nastavit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="3c9bf-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3c9bf-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="3c9bf-111">Fluent API</span></span>

<span data-ttu-id="3c9bf-112">Rozhraní Fluent API můžete použít k určení výchozího schématu.</span><span class="sxs-lookup"><span data-stu-id="3c9bf-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
