---
title: Výběr Entity Framework verze modulu Runtime EF návrháře modelů - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998239"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="9621f-102">Výběr Entity Framework verze modulu Runtime EF návrháře modelů</span><span class="sxs-lookup"><span data-stu-id="9621f-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="9621f-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9621f-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9621f-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="9621f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9621f-105">Počínaje EF6 na tomto obrázku byl přidán do EF designeru a umožňuje tak vybrat verzi modulu runtime, kterou chcete cílit při vytváření modelu.</span><span class="sxs-lookup"><span data-stu-id="9621f-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="9621f-106">Na obrazovce se objeví, když v projektu již není nainstalována nejnovější verze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9621f-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="9621f-107">Pokud už je nainstalovaná nejnovější verze budou používat jenom ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9621f-107">If the latest version is already installed it will just be used by default.</span></span>

![Obrazovka](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="9621f-109">Cílení EF6.x</span><span class="sxs-lookup"><span data-stu-id="9621f-109">Targeting EF6.x</span></span>

<span data-ttu-id="9621f-110">EF6 můžete vybrat z obrazovky zvolte si verzi přidat modul runtime EF6 do projektu.</span><span class="sxs-lookup"><span data-stu-id="9621f-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="9621f-111">Po přidání EF6, už nebudou zobrazovat tuto obrazovku v aktuálním projektu.</span><span class="sxs-lookup"><span data-stu-id="9621f-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="9621f-112">EF6 bude zakázáno, pokud už máte starší verzi EF nainstalovaný (protože je nelze cílit na více verzí modulu runtime ve stejném projektu).</span><span class="sxs-lookup"><span data-stu-id="9621f-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="9621f-113">Pokud tady není povolená možnost EF6, upgradujte váš projekt EF6 pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="9621f-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="9621f-114">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="9621f-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="9621f-115">Vyberte **aktualizace**</span><span class="sxs-lookup"><span data-stu-id="9621f-115">Select **Updates**</span></span>
3.  <span data-ttu-id="9621f-116">Vyberte **EntityFramework** (ujistěte se, že se bude aktualizovat na požadovanou verzi)</span><span class="sxs-lookup"><span data-stu-id="9621f-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="9621f-117">Klikněte na tlačítko **aktualizace**</span><span class="sxs-lookup"><span data-stu-id="9621f-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="9621f-118">Cílení EF5.x</span><span class="sxs-lookup"><span data-stu-id="9621f-118">Targeting EF5.x</span></span>

<span data-ttu-id="9621f-119">EF5 můžete vybrat z obrazovky zvolte si verzi modulu runtime EF5 přidejte do projektu.</span><span class="sxs-lookup"><span data-stu-id="9621f-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="9621f-120">Po přidání EF5, pořád uvidíte na obrazovce s EF6 možnost zakázána.</span><span class="sxs-lookup"><span data-stu-id="9621f-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="9621f-121">Pokud máte EF4.x verzi modulu runtime nainstalovány uvidíte, že tato verze EF uvedené v EF5 obrazovky spíše než.</span><span class="sxs-lookup"><span data-stu-id="9621f-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="9621f-122">V takovém případě můžete upgradovat na EF5 pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9621f-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="9621f-123">Vyberte **nástroje –&gt; Správce balíčků knihoven -&gt; Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="9621f-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="9621f-124">Spustit **Install-Package EntityFramework – verze 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="9621f-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="9621f-125">Cílení EF4.x</span><span class="sxs-lookup"><span data-stu-id="9621f-125">Targeting EF4.x</span></span>

<span data-ttu-id="9621f-126">Modul runtime EF4.x můžete nainstalovat do vašeho projektu pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="9621f-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="9621f-127">Vyberte **nástroje –&gt; Správce balíčků knihoven -&gt; Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="9621f-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="9621f-128">Spustit **EntityFramework Install-Package-verze 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="9621f-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
