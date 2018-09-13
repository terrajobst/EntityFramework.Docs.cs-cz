---
title: Podpora zprostředkovatele pro typy prostorových – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: ffd22222f59a541d8135d3738d37a7e8f5dc5d7c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489749"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="aad48-102">Podpora zprostředkovatele pro prostorové typy</span><span class="sxs-lookup"><span data-stu-id="aad48-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="aad48-103">Entity Framework podporuje práci s prostorovými daty formátu prostřednictvím DbGeography nebo DbGeometry tříd.</span><span class="sxs-lookup"><span data-stu-id="aad48-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="aad48-104">Tyto třídy závisí na konkrétních databází funkce nabízené poskytovateli rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aad48-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="aad48-105">Ne všichni poskytovatelé podporu prostorových dat a ty, které se mají další požadavky, jako je například instalace sestavení prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="aad48-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="aad48-106">Další informace o podporu zprostředkovatele pro prostorové typy jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="aad48-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="aad48-107">Dva postupy, jeden pro Code First, druhá pro první databázi nebo modelu první najdete další informace o tom, jak pomocí prostorové typy v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="aad48-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="aad48-108">Prostorové datové typy v kódu nejprve</span><span class="sxs-lookup"><span data-stu-id="aad48-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="aad48-109">Typy prostorových dat v EF designeru</span><span class="sxs-lookup"><span data-stu-id="aad48-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="aad48-110">Verze EF, které podporují prostorové typy</span><span class="sxs-lookup"><span data-stu-id="aad48-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="aad48-111">Podpora pro typy prostorových byla zavedena v EF5.</span><span class="sxs-lookup"><span data-stu-id="aad48-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="aad48-112">Nicméně v EF5 prostorové typy jsou podporovány pouze pokud aplikace cílí a běží na rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="aad48-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="aad48-113">Počínaje EF6 prostorové typy jsou podporovány pro aplikace cílené na rozhraní .NET 4 a .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="aad48-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="aad48-114">EF poskytovatelé, které podporují prostorové typy</span><span class="sxs-lookup"><span data-stu-id="aad48-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="aad48-115">EF5</span><span class="sxs-lookup"><span data-stu-id="aad48-115">EF5</span></span>  

<span data-ttu-id="aad48-116">Zprostředkovatelé rozhraní Entity Framework pro EF5, který jsme víme, že podpora prostorové typy jsou:</span><span class="sxs-lookup"><span data-stu-id="aad48-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="aad48-117">Zprostředkovatel Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="aad48-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="aad48-118">Tento zprostředkovatel je dodáván jako součást EF5.</span><span class="sxs-lookup"><span data-stu-id="aad48-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="aad48-119">Tento zprostředkovatel závisí na některé další knihovny nízké úrovně, které může být nutné nainstalovat – podrobnosti najdete níže.</span><span class="sxs-lookup"><span data-stu-id="aad48-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="aad48-120">Devart dotConnect pro Oracle</span><span class="sxs-lookup"><span data-stu-id="aad48-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="aad48-121">Toto je zprostředkovatele třetí strany z Devart.</span><span class="sxs-lookup"><span data-stu-id="aad48-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="aad48-122">Pokud znáte EF5 poskytovatele, který podporuje prostorové typy pak prosím získejte v kontaktu a budeme rádi přidat do tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="aad48-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="aad48-123">EF6</span><span class="sxs-lookup"><span data-stu-id="aad48-123">EF6</span></span>  

<span data-ttu-id="aad48-124">Zprostředkovatelé rozhraní Entity Framework pro EF6, který jsme víme, že podpora prostorové typy jsou:</span><span class="sxs-lookup"><span data-stu-id="aad48-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="aad48-125">Zprostředkovatel Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="aad48-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="aad48-126">Tento zprostředkovatel je dodáván jako součást EF6.</span><span class="sxs-lookup"><span data-stu-id="aad48-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="aad48-127">Tento zprostředkovatel závisí na některé další knihovny nízké úrovně, které může být nutné nainstalovat – podrobnosti najdete níže.</span><span class="sxs-lookup"><span data-stu-id="aad48-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="aad48-128">Devart dotConnect pro Oracle</span><span class="sxs-lookup"><span data-stu-id="aad48-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="aad48-129">Toto je zprostředkovatele třetí strany z Devart.</span><span class="sxs-lookup"><span data-stu-id="aad48-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="aad48-130">Pokud znáte EF6 poskytovatele, který podporuje prostorové typy pak prosím získejte v kontaktu a budeme rádi přidat do tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="aad48-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="aad48-131">Požadavky na prostorové typy s Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="aad48-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="aad48-132">Prostorové podpory systému SQL Server závisí na typech nízké úrovně, SQL Server – konkrétní SqlGeography a SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="aad48-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="aad48-133">Tyto typy v sestavení Microsoft.SqlServer.Types.dll za provozu a toto sestavení není dodán jako součást EF nebo jako součást rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="aad48-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="aad48-134">Při instalaci sady Visual Studio často také nainstaluje verzi systému SQL Server a bude se jednat o instalaci Microsoft.SqlServer.Types.dll.</span><span class="sxs-lookup"><span data-stu-id="aad48-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="aad48-135">Pokud SQL Server není nainstalován na počítači, ve které chcete použít prostorové typy nebo typy prostorových byly vyloučeny z instalace systému SQL Server, je potřeba je nainstalovat ručně.</span><span class="sxs-lookup"><span data-stu-id="aad48-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="aad48-136">Typy lze nainstalovat pomocí `SQLSysClrTypes.msi`, která je součástí sady Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="aad48-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="aad48-137">Prostorové typy jsou specifické pro verzi, SQL Server, takže doporučujeme [vyhledejte "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) webu Microsoft Download Center, pak vyberte a stáhněte si možnost, která odpovídá verzi systému SQL Server budete používat.</span><span class="sxs-lookup"><span data-stu-id="aad48-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
