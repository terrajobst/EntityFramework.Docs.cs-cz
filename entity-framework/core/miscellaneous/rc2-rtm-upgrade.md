---
title: Upgrade z EF Core 1.0 RC2 na RTM – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998316"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="f2833-102">Upgrade z EF Core 1.0 RC2 na RTM</span><span class="sxs-lookup"><span data-stu-id="f2833-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="f2833-103">Tento článek obsahuje pokyny pro přesouvání aplikace sestavené s balíčky RC2 nastavená na 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="f2833-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="f2833-104">Verze balíčku</span><span class="sxs-lookup"><span data-stu-id="f2833-104">Package Versions</span></span>

<span data-ttu-id="f2833-105">Názvy balíčků na nejvyšší úrovni, která by obvykle instalují do aplikace neliší RC2 a verze RTM.</span><span class="sxs-lookup"><span data-stu-id="f2833-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="f2833-106">**Je třeba nainstalované balíčky s upgradem na verzi RTM verze:**</span><span class="sxs-lookup"><span data-stu-id="f2833-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="f2833-107">Balíčky runtime (například `Microsoft.EntityFrameworkCore.SqlServer`) změnil z `1.0.0-rc2-final` k `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="f2833-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="f2833-108">`Microsoft.EntityFrameworkCore.Tools` Balíčku změnil z `1.0.0-preview1-final` k `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="f2833-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="f2833-109">Všimněte si, že nástroje je stále předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="f2833-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="f2833-110">Existující migrace může být nutné přidat maxLength</span><span class="sxs-lookup"><span data-stu-id="f2833-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="f2833-111">Ve verzi RC2, definice sloupců v migraci vypadal `table.Column<string>(nullable: true)` a délka sloupce byla vyhledána v některá metadata uložíme v kódu na migraci.</span><span class="sxs-lookup"><span data-stu-id="f2833-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="f2833-112">Ve verzi RTM, délka je teď součástí automaticky generovaný kód `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="f2833-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="f2833-113">Všechny existující migrace, které byly automaticky před použitím RTM nebude mít `maxLength` zadaný argument.</span><span class="sxs-lookup"><span data-stu-id="f2833-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="f2833-114">To znamená, že se použije maximální délka podporována databází (`nvarchar(max)` na SQL serveru).</span><span class="sxs-lookup"><span data-stu-id="f2833-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="f2833-115">To může být u některých sloupců, ale sloupce, které jsou součástí klíče, cizí klíč nebo index musí být aktualizováno, aby zahrnovalo maximální délku.</span><span class="sxs-lookup"><span data-stu-id="f2833-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="f2833-116">Podle konvence je 450 maximální počet znaků použitý pro klíče, cizí klíče a indexovaných sloupců.</span><span class="sxs-lookup"><span data-stu-id="f2833-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="f2833-117">Pokud jste nakonfigurovali explicitně délku v modelu, měli byste použít delší místo.</span><span class="sxs-lookup"><span data-stu-id="f2833-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="f2833-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="f2833-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="f2833-119">Tato změna ovlivní projekty, které používají ASP.NET Identity a byly vytvořeny z předprodukčním-RTM šablony projektu.</span><span class="sxs-lookup"><span data-stu-id="f2833-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="f2833-120">Šablona projektu zahrnuje migraci použít k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="f2833-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="f2833-121">Tato migrace je nutné upravit k určení maximální délce `256` pro tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="f2833-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="f2833-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="f2833-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="f2833-123">Název</span><span class="sxs-lookup"><span data-stu-id="f2833-123">Name</span></span>

    * <span data-ttu-id="f2833-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="f2833-124">NormalizedName</span></span>

*  <span data-ttu-id="f2833-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="f2833-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="f2833-126">E-mailu</span><span class="sxs-lookup"><span data-stu-id="f2833-126">Email</span></span>

   * <span data-ttu-id="f2833-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="f2833-127">NormalizedEmail</span></span>

   * <span data-ttu-id="f2833-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="f2833-128">NormalizedUserName</span></span>

   * <span data-ttu-id="f2833-129">UserName</span><span class="sxs-lookup"><span data-stu-id="f2833-129">UserName</span></span>

<span data-ttu-id="f2833-130">K provedení této změny bude dojít k následující výjimce při použití u počáteční migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="f2833-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="f2833-131">.NET core: Odebere "importy" v souboru project.json</span><span class="sxs-lookup"><span data-stu-id="f2833-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="f2833-132">Pokud se zaměřujete na .NET Core RC2, je potřeba přidat `imports` k souboru project.json jako dočasným řešením pro některý ze EF Core závislosti, které nepodporují .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f2833-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="f2833-133">Ty nyní je možné odebrat.</span><span class="sxs-lookup"><span data-stu-id="f2833-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="f2833-134">Od verze 1.0 RTM, [.NET Core SDK](https://www.microsoft.com/net/download/core) už nepodporuje `project.json` nebo vývoj aplikací .NET Core pomocí sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="f2833-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="f2833-135">Doporučujeme [migrace z project.json na csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="f2833-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="f2833-136">Pokud používáte Visual Studio, doporučujeme je upgradovat na [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f2833-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="f2833-137">UPW: Přidat přesměrování vazby</span><span class="sxs-lookup"><span data-stu-id="f2833-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="f2833-138">Došlo k pokusu o spuštění EF příkazy na univerzální platformu Windows (UPW) výsledky projekty s následující chybou:</span><span class="sxs-lookup"><span data-stu-id="f2833-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="f2833-139">Budete muset ručně přidat přesměrování vazby do projektu UPW.</span><span class="sxs-lookup"><span data-stu-id="f2833-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="f2833-140">Vytvořte soubor s názvem `App.config` v projektu kořenová složka a přidání přesměrování verze sestavení správná.</span><span class="sxs-lookup"><span data-stu-id="f2833-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
