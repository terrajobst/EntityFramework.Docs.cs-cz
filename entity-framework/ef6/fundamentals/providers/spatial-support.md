---
title: Podpora poskytovatele pro prostorové typy – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416337"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="c698e-102">Podpora poskytovatelů prostorových typů</span><span class="sxs-lookup"><span data-stu-id="c698e-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="c698e-103">Entity Framework podporuje práci s prostorovými daty prostřednictvím tříd DbGeography nebo DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="c698e-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="c698e-104">Tyto třídy spoléhají na funkce specifické pro databázi, které nabízí poskytovatel Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c698e-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="c698e-105">Ne všichni zprostředkovatelé podporují prostorová data a ty, které mohou mít další požadavky, jako je instalace sestavení prostorových typů.</span><span class="sxs-lookup"><span data-stu-id="c698e-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="c698e-106">Další informace o podpoře poskytovatele pro prostorové typy jsou uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="c698e-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="c698e-107">Další informace o tom, jak používat prostorové typy v aplikaci, najdete ve dvou návodech, jeden pro Code First, druhý pro Database First nebo Model First:</span><span class="sxs-lookup"><span data-stu-id="c698e-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="c698e-108">Typy prostorových dat v Code First</span><span class="sxs-lookup"><span data-stu-id="c698e-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="c698e-109">Typy prostorových dat v Návrháři EF</span><span class="sxs-lookup"><span data-stu-id="c698e-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="c698e-110">Verze EF podporující prostorové typy</span><span class="sxs-lookup"><span data-stu-id="c698e-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="c698e-111">Podpora prostorových typů byla představena v EF5.</span><span class="sxs-lookup"><span data-stu-id="c698e-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="c698e-112">Nicméně v prostorových typech EF5 jsou podporovány pouze v případě, že je aplikace cílena a spuštěna v rozhraní .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="c698e-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="c698e-113">Počínaje EF6 prostorovými typy jsou podporovány pro aplikace zaměřené na rozhraní .NET 4 i .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="c698e-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="c698e-114">Zprostředkovatelé EF podporující prostorové typy</span><span class="sxs-lookup"><span data-stu-id="c698e-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="c698e-115">EF5</span><span class="sxs-lookup"><span data-stu-id="c698e-115">EF5</span></span>  

<span data-ttu-id="c698e-116">Poskytovatelé Entity Framework pro EF5, o kterých víte, že podporují prostorové typy:</span><span class="sxs-lookup"><span data-stu-id="c698e-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="c698e-117">Poskytovatel Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="c698e-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="c698e-118">Tento poskytovatel se dodává jako součást EF5.</span><span class="sxs-lookup"><span data-stu-id="c698e-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="c698e-119">Tento zprostředkovatel závisí na některých dalších knihovnách nízké úrovně, které můžou být potřeba nainstalovat – podrobnosti najdete níže.</span><span class="sxs-lookup"><span data-stu-id="c698e-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="c698e-120">Devart dotConnect pro Oracle</span><span class="sxs-lookup"><span data-stu-id="c698e-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="c698e-121">Toto je zprostředkovatel třetí strany z Devart.</span><span class="sxs-lookup"><span data-stu-id="c698e-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="c698e-122">Pokud znáte poskytovatele EF5, který podporuje prostorové typy, kontaktujte prosím kontakt a my ho budeme moct přidat do tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="c698e-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="c698e-123">EF6</span><span class="sxs-lookup"><span data-stu-id="c698e-123">EF6</span></span>  

<span data-ttu-id="c698e-124">Poskytovatelé Entity Framework pro EF6, o kterých víte, že podporují prostorové typy:</span><span class="sxs-lookup"><span data-stu-id="c698e-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="c698e-125">Poskytovatel Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="c698e-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="c698e-126">Tento poskytovatel se dodává jako součást EF6.</span><span class="sxs-lookup"><span data-stu-id="c698e-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="c698e-127">Tento zprostředkovatel závisí na některých dalších knihovnách nízké úrovně, které můžou být potřeba nainstalovat – podrobnosti najdete níže.</span><span class="sxs-lookup"><span data-stu-id="c698e-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="c698e-128">Devart dotConnect pro Oracle</span><span class="sxs-lookup"><span data-stu-id="c698e-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="c698e-129">Toto je zprostředkovatel třetí strany z Devart.</span><span class="sxs-lookup"><span data-stu-id="c698e-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="c698e-130">Pokud znáte poskytovatele EF6, který podporuje prostorové typy, kontaktujte prosím kontakt a my ho budeme moct přidat do tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="c698e-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="c698e-131">Předpoklady pro prostorové typy s Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="c698e-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="c698e-132">SQL Server prostorová podpora závisí na typech SQL Server SqlGeography a SqlGeometry specifických pro nižší úroveň.</span><span class="sxs-lookup"><span data-stu-id="c698e-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="c698e-133">Tyto typy jsou živé v sestavení Microsoft. SqlServer. Types. dll a toto sestavení se nedodává jako součást EF nebo jako součást .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c698e-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="c698e-134">Když je nainstalována aplikace Visual Studio, často také nainstaluje verzi SQL Server a bude obsahovat instalaci Microsoft. SqlServer. Types. dll.</span><span class="sxs-lookup"><span data-stu-id="c698e-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="c698e-135">Pokud v počítači, kde chcete použít prostorové typy, není nainstalováno SQL Server, nebo pokud byly z instalace SQL Server vyloučeny prostorové typy, pak je budete muset nainstalovat ručně.</span><span class="sxs-lookup"><span data-stu-id="c698e-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="c698e-136">Typy lze nainstalovat pomocí `SQLSysClrTypes.msi`, který je součástí sady Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="c698e-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="c698e-137">Prostorové typy jsou SQL Server specifické pro verzi, proto doporučujeme [Vyhledat "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) na webu Microsoft Download Center a pak vybrat a stáhnout možnost, která odpovídá verzi SQL Server, kterou budete používat.</span><span class="sxs-lookup"><span data-stu-id="c698e-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
