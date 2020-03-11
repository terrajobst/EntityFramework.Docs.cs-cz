---
title: Výběr verze modulu runtime Entity Framework pro modely návrháře EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418145"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="17f1b-102">Výběr verze modulu runtime Entity Framework pro modely návrháře EF</span><span class="sxs-lookup"><span data-stu-id="17f1b-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="17f1b-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="17f1b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="17f1b-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="17f1b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="17f1b-105">Počínaje EF6 se do návrháře EF přidala následující obrazovka, která umožňuje vybrat verzi modulu runtime, pro kterou chcete při vytváření modelu cílit.</span><span class="sxs-lookup"><span data-stu-id="17f1b-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="17f1b-106">Obrazovka se zobrazí, pokud v projektu ještě není nainstalovaná nejnovější verze Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="17f1b-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="17f1b-107">Pokud je už nejnovější verze nainstalovaná, bude se používat jenom ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="17f1b-107">If the latest version is already installed it will just be used by default.</span></span>

![Obnovovací](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="17f1b-109">Cílení na EF6. x</span><span class="sxs-lookup"><span data-stu-id="17f1b-109">Targeting EF6.x</span></span>

<span data-ttu-id="17f1b-110">Můžete zvolit EF6 z obrazovky zvolit vaši verzi a přidat do projektu modul runtime EF6.</span><span class="sxs-lookup"><span data-stu-id="17f1b-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="17f1b-111">Po přidání EF6 se tato obrazovka v aktuálním projektu přestane zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="17f1b-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="17f1b-112">EF6 se zakáže, pokud už máte nainstalovanou starší verzi EF (vzhledem k tomu, že nemůžete cílit na více verzí modulu runtime ze stejného projektu).</span><span class="sxs-lookup"><span data-stu-id="17f1b-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="17f1b-113">Pokud zde není povolená možnost EF6, proveďte upgrade projektu na EF6 pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="17f1b-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="17f1b-114">V Průzkumník řešení klikněte pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="17f1b-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="17f1b-115">Vybrat **aktualizace**</span><span class="sxs-lookup"><span data-stu-id="17f1b-115">Select **Updates**</span></span>
3.  <span data-ttu-id="17f1b-116">Vyberte **EntityFramework** (Ujistěte se, že je bude aktualizovat na požadovanou verzi).</span><span class="sxs-lookup"><span data-stu-id="17f1b-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="17f1b-117">Kliknout na **aktualizovat**</span><span class="sxs-lookup"><span data-stu-id="17f1b-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="17f1b-118">Cílení na EF5. x</span><span class="sxs-lookup"><span data-stu-id="17f1b-118">Targeting EF5.x</span></span>

<span data-ttu-id="17f1b-119">Můžete zvolit EF5 z obrazovky zvolit vaši verzi a přidat do projektu modul runtime EF5.</span><span class="sxs-lookup"><span data-stu-id="17f1b-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="17f1b-120">Po přidání EF5 se stále zobrazuje obrazovka s možností EF6 zakázaná.</span><span class="sxs-lookup"><span data-stu-id="17f1b-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="17f1b-121">Pokud máte již nainstalovanou verzi EF4. x modulu runtime, uvidíte, že je verze EF uvedená na obrazovce, nikoli EF5.</span><span class="sxs-lookup"><span data-stu-id="17f1b-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="17f1b-122">V této situaci můžete upgradovat na EF5 pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="17f1b-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="17f1b-123">Výběr **nástrojů –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="17f1b-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="17f1b-124">Spustit **instalaci-Package EntityFramework-Version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="17f1b-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="17f1b-125">Cílení na EF4. x</span><span class="sxs-lookup"><span data-stu-id="17f1b-125">Targeting EF4.x</span></span>

<span data-ttu-id="17f1b-126">Do projektu můžete nainstalovat modul runtime EF4. x pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="17f1b-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="17f1b-127">Výběr **nástrojů –&gt; správce balíčků knihovny –&gt; konzolu Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="17f1b-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="17f1b-128">Spustit **instalaci-Package EntityFramework-Version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="17f1b-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
