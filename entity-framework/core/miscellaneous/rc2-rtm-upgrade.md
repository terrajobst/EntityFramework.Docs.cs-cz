---
title: Upgrade z verze EF Core 1,0 RC2 na RTM-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 779caad7883d13684b389dab7515be44bc42e1ef
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655809"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="5feda-102">Upgrade z verze EF Core 1,0 RC2 na RTM</span><span class="sxs-lookup"><span data-stu-id="5feda-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="5feda-103">Tento článek poskytuje pokyny pro přesunutí aplikace vytvořené s balíčky RC2 do 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="5feda-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="5feda-104">Verze balíčku</span><span class="sxs-lookup"><span data-stu-id="5feda-104">Package Versions</span></span>

<span data-ttu-id="5feda-105">Názvy balíčků nejvyšší úrovně, které byste obvykle nainstalovali do aplikace, se mezi verzemi RC2 a RTM nezměnily.</span><span class="sxs-lookup"><span data-stu-id="5feda-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="5feda-106">**Je nutné upgradovat nainstalované balíčky na verzi RTM:**</span><span class="sxs-lookup"><span data-stu-id="5feda-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="5feda-107">Běhové balíčky (například `Microsoft.EntityFrameworkCore.SqlServer`) se změnily z `1.0.0-rc2-final` na `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="5feda-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="5feda-108">Balíček `Microsoft.EntityFrameworkCore.Tools` se změnil z `1.0.0-preview1-final` na `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="5feda-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="5feda-109">Všimněte si, že nástroj je stále předběžnou verzí.</span><span class="sxs-lookup"><span data-stu-id="5feda-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="5feda-110">Existující migrace můžou vyžadovat přidání maxLength.</span><span class="sxs-lookup"><span data-stu-id="5feda-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="5feda-111">V rozhraní RC2 se definice sloupce v migraci jako `table.Column<string>(nullable: true)` a délka sloupce vyhledala v některých metadatech, které ukládáme do kódu na pozadí migrace.</span><span class="sxs-lookup"><span data-stu-id="5feda-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="5feda-112">Ve verzi RTM je tato délka vložená do `table.Column<string>(maxLength: 450, nullable: true)`generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="5feda-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="5feda-113">U všech existujících migrací, které byly vygenerované před použitím RTM, nebude mít zadaný argument `maxLength`.</span><span class="sxs-lookup"><span data-stu-id="5feda-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="5feda-114">To znamená, že se použije maximální délka podporovaná databází (`nvarchar(max)` při SQL Server).</span><span class="sxs-lookup"><span data-stu-id="5feda-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="5feda-115">To může být pro některé sloupce jemné, ale sloupce, které jsou součástí klíče, cizího klíče nebo indexu, se musí aktualizovat tak, aby zahrnovaly maximální délku.</span><span class="sxs-lookup"><span data-stu-id="5feda-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="5feda-116">Podle konvence je 450 maximální délka používané pro klíče, cizí klíče a indexované sloupce.</span><span class="sxs-lookup"><span data-stu-id="5feda-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="5feda-117">Pokud jste v modelu explicitně nakonfigurovali délku, měli byste místo toho použít tuto délku.</span><span class="sxs-lookup"><span data-stu-id="5feda-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="5feda-118">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="5feda-118">ASP.NET Identity</span></span>

<span data-ttu-id="5feda-119">Tato změna ovlivní projekty, které používají ASP.NET Identity a byly vytvořeny ze šablony projektu před verzí RTM.</span><span class="sxs-lookup"><span data-stu-id="5feda-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="5feda-120">Šablona projektu zahrnuje migraci použitou k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="5feda-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="5feda-121">Tato migrace musí být upravena, aby určovala maximální délku `256` pro následující sloupce.</span><span class="sxs-lookup"><span data-stu-id="5feda-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

* <span data-ttu-id="5feda-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="5feda-122">**AspNetRoles**</span></span>
  * <span data-ttu-id="5feda-123">Name</span><span class="sxs-lookup"><span data-stu-id="5feda-123">Name</span></span>
  * <span data-ttu-id="5feda-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="5feda-124">NormalizedName</span></span>
* <span data-ttu-id="5feda-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="5feda-125">**AspNetUsers**</span></span>
  * <span data-ttu-id="5feda-126">E-mail</span><span class="sxs-lookup"><span data-stu-id="5feda-126">Email</span></span>
  * <span data-ttu-id="5feda-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="5feda-127">NormalizedEmail</span></span>
  * <span data-ttu-id="5feda-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="5feda-128">NormalizedUserName</span></span>
  * <span data-ttu-id="5feda-129">UserName</span><span class="sxs-lookup"><span data-stu-id="5feda-129">UserName</span></span>

<span data-ttu-id="5feda-130">Tato změna způsobí při použití prvotní migrace na databázi následující výjimku.</span><span class="sxs-lookup"><span data-stu-id="5feda-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

``` Console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="5feda-131">.NET Core: odebrání "Imports" v Project. JSON</span><span class="sxs-lookup"><span data-stu-id="5feda-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="5feda-132">Pokud jste se rozhodli .NET Core s verzí RC2, je nutné přidat `imports` do Project. JSON jako dočasné řešení pro některé závislosti EF Core, které nepodporují .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="5feda-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="5feda-133">Teď je můžete odebrat.</span><span class="sxs-lookup"><span data-stu-id="5feda-133">These can now be removed.</span></span>

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
> <span data-ttu-id="5feda-134">Od verze 1,0 RTM již [.NET Core SDK](https://www.microsoft.com/net/download/core) nadále nepodporují `project.json` ani vývoj aplikací .NET Core pomocí sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="5feda-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="5feda-135">Doporučujeme [migrovat z Project. JSON na csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="5feda-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="5feda-136">Pokud používáte aplikaci Visual Studio, doporučujeme upgradovat na [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="5feda-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="5feda-137">UWP: Přidání přesměrování vazby</span><span class="sxs-lookup"><span data-stu-id="5feda-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="5feda-138">Když se pokusíte spustit příkazy EF v projektech Univerzální platforma Windows (UWP), dojde k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="5feda-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

```output
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

<span data-ttu-id="5feda-139">Je nutné ručně přidat přesměrování vazby do projektu UWP.</span><span class="sxs-lookup"><span data-stu-id="5feda-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="5feda-140">V kořenové složce projektu vytvořte soubor s názvem `App.config` a přidejte přesměrování do správné verze sestavení.</span><span class="sxs-lookup"><span data-stu-id="5feda-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

```xml
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
