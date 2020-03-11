---
title: Vytváření DbContext při návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416740"
---
# <a name="design-time-dbcontext-creation"></a>Vytváření DbContext v době návrhu

Některé příkazy EF Core nástrojů (například příkazy [migrace][1] ) vyžadují, aby se v době návrhu vytvořila odvozená `DbContext` instance, aby se shromáždily podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze. Ve většině případů je žádoucí, aby vytvořený `DbContext` byl nakonfigurován podobným způsobem, jak by byl [nakonfigurován v době běhu][2].

Existují různé způsoby, jak se nástroje snaží vytvořit `DbContext`:

## <a name="from-application-services"></a>Z aplikačních služeb

Pokud váš projekt po spuštění používá [webového hostitele ASP.NET Core][3] nebo [obecného hostitele .NET Core][4], nástroj se pokusí získat objekt DbContext od poskytovatele služeb aplikace.

Nástroje se nejprve pokusí získat poskytovatele služby vyvoláním `Program.CreateHostBuilder()`, voláním `Build()`a následným přístupem k vlastnosti `Services`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> Při vytváření nové aplikace ASP.NET Core je tento zavěšení součástí standardně.

`DbContext` sám sebe a všechny závislosti ve svém konstruktoru musí být registrovány jako služby v poskytovateli služeb aplikace. To lze snadno dosáhnout vytvořením [konstruktoru na `DbContext`, který převezme instanci `DbContextOptions<TContext>` jako argument][5] a pomocí [metody`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Použití konstruktoru bez parametrů

Pokud DbContext nelze získat od poskytovatele služby Application Service, nástroje hledají odvozený typ `DbContext` v rámci projektu. Pak se pokusí vytvořit instanci pomocí konstruktoru bez parametrů. To může být výchozí konstruktor, pokud je `DbContext` nakonfigurován pomocí metody [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>Z továrny v době návrhu

Můžete také sdělit nástroje, jak vytvořit DbContext implementací rozhraní `IDesignTimeDbContextFactory<TContext>`: Pokud třída implementující toto rozhraní je nalezena v jednom projektu jako odvozená `DbContext` nebo v projektu po spuštění aplikace, nástroje vycházejí z jiných způsobů vytváření DbContext a místo toho používají továrnu v době návrhu.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> Parametr `args` se v tuto chvíli nepoužívá. Existuje [problém][8] sledující možnost zadat argumenty pro dobu návrhu z nástrojů.

Továrna v době návrhu může být obzvláště užitečná, pokud je třeba nakonfigurovat DbContext jinak než v době návrhu, a to v případě, že konstruktor `DbContext` přebírá další parametry nejsou registrovány v DI, pokud nepoužíváte hodnotu DI, nebo z nějakého důvodu nemusíte mít `BuildWebHost` metodu ve třídě `Main` vaší ASP.NET Core aplikace.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
