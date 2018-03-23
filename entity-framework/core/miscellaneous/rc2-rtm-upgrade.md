---
title: Upgrade z EF základní 1.0 RC2 na RTM – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="9d872-102">Upgrade na verzi RTM EF Core 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="9d872-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="9d872-103">Tento článek obsahuje pokyny pro přesouvání aplikace vytvořené s nástroji RC2 balíčky, do kterých 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="9d872-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="9d872-104">Verze balíčku</span><span class="sxs-lookup"><span data-stu-id="9d872-104">Package Versions</span></span>

<span data-ttu-id="9d872-105">Názvy balíčky nejvyšší úrovně, které by obvykle nainstalovat do aplikace mezi RC2 a RTM nezměnila.</span><span class="sxs-lookup"><span data-stu-id="9d872-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="9d872-106">**Je třeba upgradovat na verze RTM nainstalované balíčky:**</span><span class="sxs-lookup"><span data-stu-id="9d872-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="9d872-107">Modul runtime balíčky (například `Microsoft.EntityFrameworkCore.SqlServer`) se změnil z `1.0.0-rc2-final` k `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="9d872-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="9d872-108">`Microsoft.EntityFrameworkCore.Tools` Balíčku se změnil z `1.0.0-preview1-final` k `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="9d872-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="9d872-109">Upozorňujeme, že nástrojů se stále předběžné verze.</span><span class="sxs-lookup"><span data-stu-id="9d872-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="9d872-110">Existující migrace může být nutné přidat maxLength</span><span class="sxs-lookup"><span data-stu-id="9d872-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="9d872-111">V RC2, definice sloupců v migraci hledá jako `table.Column<string>(nullable: true)` a délka sloupce byla vyhledávány v některá metadata ukládáme v kódu migrace.</span><span class="sxs-lookup"><span data-stu-id="9d872-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="9d872-112">Ve verzi RTM, délka je nyní zahrnutá v automaticky generovaný kód `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="9d872-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="9d872-113">Nebude mít žádné existující migrace, které byly před použitím RTM generované uživatelské rozhraní `maxLength` byl zadán neplatný argument.</span><span class="sxs-lookup"><span data-stu-id="9d872-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="9d872-114">To znamená, že se použije maximální délka podporovaná databáze (`nvarchar(max)` na serveru SQL Server).</span><span class="sxs-lookup"><span data-stu-id="9d872-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="9d872-115">To může být vhodná pro některé sloupce, ale sloupce, které jsou součástí klíč, cizí klíč nebo index musí být aktualizováno, aby zahrnovalo maximální délku.</span><span class="sxs-lookup"><span data-stu-id="9d872-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="9d872-116">Podle konvence je 450 maximální délku použít pro klíče, cizí klíče a indexované sloupce.</span><span class="sxs-lookup"><span data-stu-id="9d872-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="9d872-117">Pokud jste nakonfigurovali explicitně délkou v modelu, měli byste použít dlouhou místo.</span><span class="sxs-lookup"><span data-stu-id="9d872-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="9d872-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="9d872-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="9d872-119">Tato změna ovlivňuje projekty, které používají ASP.NET Identity a byly vytvořeny z po předem-RTM šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="9d872-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="9d872-120">Šablona projektu zahrnuje migraci použít k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="9d872-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="9d872-121">Tato migrace musí upravit, chcete-li určit maximální délku `256` pro následující sloupce.</span><span class="sxs-lookup"><span data-stu-id="9d872-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="9d872-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="9d872-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="9d872-123">Název</span><span class="sxs-lookup"><span data-stu-id="9d872-123">Name</span></span>

    * <span data-ttu-id="9d872-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="9d872-124">NormalizedName</span></span>

*  <span data-ttu-id="9d872-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="9d872-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="9d872-126">E-mailu</span><span class="sxs-lookup"><span data-stu-id="9d872-126">Email</span></span>

   * <span data-ttu-id="9d872-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="9d872-127">NormalizedEmail</span></span>

   * <span data-ttu-id="9d872-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="9d872-128">NormalizedUserName</span></span>

   * <span data-ttu-id="9d872-129">UserName</span><span class="sxs-lookup"><span data-stu-id="9d872-129">UserName</span></span>

<span data-ttu-id="9d872-130">K provedení této změny bude neuděláte následující výjimka při počáteční migrace se použije k databázi.</span><span class="sxs-lookup"><span data-stu-id="9d872-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="9d872-131">.NET core: Odeberte "importy" v souboru project.json</span><span class="sxs-lookup"><span data-stu-id="9d872-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="9d872-132">Pokud byly cílem .NET Core RC2, je potřeba přidat `imports` do souboru project.json jako dočasné řešení pro některé základní EF závislosti nejsou podporovány .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9d872-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="9d872-133">Tyto nyní může být odebrán.</span><span class="sxs-lookup"><span data-stu-id="9d872-133">These can now be removed.</span></span>

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
> <span data-ttu-id="9d872-134">Od verze 1.0 RTM, [.NET Core SDK](https://www.microsoft.com/net/download/core) již nepodporuje `project.json` nebo vývoj aplikací .NET Core pomocí sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="9d872-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="9d872-135">Doporučujeme [migrovat ze souboru project.json csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="9d872-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="9d872-136">Pokud používáte Visual Studio, doporučujeme upgradu na [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="9d872-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="9d872-137">UWP: Přidejte přesměrování vazby</span><span class="sxs-lookup"><span data-stu-id="9d872-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="9d872-138">Došlo k pokusu o spuštění EF příkazy na výsledky projekty univerzální platformu Windows (UWP) s následující chybou:</span><span class="sxs-lookup"><span data-stu-id="9d872-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="9d872-139">Budete muset ručně přidejte přesměrování vazby do projektu UPW.</span><span class="sxs-lookup"><span data-stu-id="9d872-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="9d872-140">Vytvořte soubor s názvem `App.config` v projektu kořenová složka a přidejte přesměrování verzí sestavení správný.</span><span class="sxs-lookup"><span data-stu-id="9d872-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
