---
title: Pomocí migrate.exe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 6e9880523bbcf2fe55390a447241e59723a0967f
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490212"
---
# <a name="using-migrateexe"></a>Pomocí migrate.exe
Migrace Code First lze použít k aktualizaci databáze ze sady visual Studio, ale mohou být provedeny také prostřednictvím migrate.exe nástroj příkazového řádku. Tato stránka vám poskytne rychlý přehled o tom, jak používat migrate.exe k provedení migrace ve srovnání s databází.

> [!NOTE]
> Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře. Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.

## <a name="copy-migrateexe"></a>Zkopírujte migrate.exe

Při instalaci rozhraní Entity Framework pomocí nástroje NuGet migrate.exe bude ve složce nástroje staženého balíčku. V &lt;složky projektu&gt;\\balíčky\\objektu EntityFramework.&lt; verze&gt;\\nástroje

Jakmile budete mít migrate.exe musíte zkopírovat do umístění sestavení, která obsahuje vaše migrace.

Pokud vaše aplikace cílí na rozhraní .NET 4, a ne 4.5, pak budete muset zkopírovat **Redirect.config** na místo jako kontejneru a přejmenujte jej **migrate.exe.config**. Je to tak, aby migrate.exe získá správnou vazbu přesměrování být schopna najít sestavení rozhraní Entity Framework.

| .NET 4.5                                   | ROZHRANÍ .NET 4.0                                   |
|:-------------------------------------------|:-------------------------------------------|
| ![Soubory rozhraní .NET 4.5](~/ef6/media/net45files.png)  | ![Soubory rozhraní .NET 4.0](~/ef6/media/net40files.png)  |

> [!NOTE]
> Migrate.exe nepodporuje x64 sestavení.

Po přesunutí migrate.exe do správné složky, pak byste měli použít k provedení migrace na databázi. Všechno, co je navrženy nástroj je provedení migrace. Nelze generovat migrace nebo vytvořit skript SQL.

## <a name="see-options"></a>Zobrazit možnosti

``` console
Migrate.exe /?
```

Výše se zobrazí na stránce nápovědy přidružené k tento nástroj, Všimněte si, že je potřeba mít EntityFramework.dll ve stejném umístění, které běží migrate.exe v pořadí, aby to fungovalo.

## <a name="migrate-to-the-latest-migration"></a>Migrace na nejnovější migrace

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Při spouštění migrate.exe pouze povinný parametr je sestavení, což je sestavení obsahující migrace, které se pokoušíte spustit, ale použije všechny konvence na základě nastavení, pokud nezadáte konfigurační soubor.

## <a name="migrate-to-a-specific-migration"></a>Migrace na konkrétní migrace

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Pokud chcete spustit migrace až po konkrétní migrace, můžete zadat název migrace. Tento kód spustí všechny předchozí migrace podle potřeby až na migraci zadán.

## <a name="specify-working-directory"></a>Zadejte pracovní adresář

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Pokud je sestavení má závislosti nebo načte soubory vzhledem k pracovní adresář je potřeba nastavit startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Zadejte konfiguraci migrace použít

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Pokud máte více tříd konfigurace migrace, třídy, které dědí z DbMigrationConfiguration, musíte určit, který se má použít pro toto spuštění. Tento parametr je zadán tím, že poskytuje volitelný druhý parametr bez přepínače jako výše.

## <a name="provide-connection-string"></a>Zadejte připojovací řetězec

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Pokud chcete zadat připojovací řetězec příkazového řádku je nutné také zadat název zprostředkovatele. Bez zadání názvu zprostředkovatele rozhraní způsobí výjimku.

## <a name="common-problems"></a>Běžné problémy

| Chybová zpráva                                                                                                                                                                                                                                                                                                                      | Řešení                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Neošetřená výjimka: System.IO.FileLoadException: Nelze načíst soubor nebo sestavení ' objektu EntityFramework, verze = 5.0.0.0, jazyková verze = neutral, PublicKeyToken = b77a5c561934e089 "nebo některá z jeho závislostí. Definice manifestu sestavení neodpovídá odkazu na sestavení. (Výjimka z HRESULT: 0x80131040)         | To obvykle znamená, že je spuštěná aplikace rozhraní .NET 4 bez Redirect.config souboru. Budete muset zkopírovat Redirect.config do stejného umístění jako migrate.exe a přejmenujte jej na migrate.exe.config.                                                                                       |
| Neošetřená výjimka: System.IO.FileLoadException: Nelze načíst soubor nebo sestavení ' objektu EntityFramework, verze = 4.4.0.0, jazyková verze = neutral, PublicKeyToken = b77a5c561934e089 "nebo některá z jeho závislostí. Definice manifestu sestavení neodpovídá odkazu na sestavení. (Výjimka z HRESULT: 0x80131040)          | Tato výjimka znamená, že používáte .NET 4.5 aplikaci Redirect.config zkopírovat do umístění migrate.exe. Pokud je vaše aplikace .NET 4.5 není potřeba mít konfigurační soubor s přesměrováními uvnitř. Odstraňte soubor migrate.exe.config.                                    |
| Chyba: Nepovedlo se aktualizovat databázi tak, aby odpovídaly aktuální model, protože existují čekající změny a je zakázáno automatické migrace. Zápisu změny čekající na vyřízení modelu migrace na základě kódu nebo povolit automatickou migraci. Nastavte DbMigrationsConfiguration.AutomaticMigrationsEnabled na true pro povolení automatické migrace. | K této chybě dochází, pokud běží migrace, pokud jste ještě nevytvořili migrace vypořádat se se změnami provedenými na model a databáze se neshoduje s modelem. Přidání vlastnosti do třídy modelu pak spustíte migrate.exe bez vytváření migrace pro upgrade databáze je příkladem. |
| Chyba: Typ není rozpoznán pro člena ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, verze = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ".                                                                                                                                       | Tuto chybu může způsobovat zadání nesprávné spouštěcí adresář. Tato hodnota musí být umístění migrate.exe                                                                                                                                                                                      |
| Neošetřená výjimka: System.NullReferenceException: odkaz není nastaven na instanci objektu na objekt. <br/>   na System.Data.Entity.Migrations.Console.Program.Main (String [] args)                                                                                                                                             | Příčinou může být bez zadání požadovaný parametr pro scénář, který používáte. Například zadání připojovacího řetězce bez zadání názvu zprostředkovatele rozhraní.                                                                                                                        |
| Chyba: v sestavení 'ClassLibrary1' byl nalezen více než jeden typ konfigurace migrace. Zadejte název ten, který má použít.                                                                                                                                                                                                  | Jako chyba uvádí, je v daném sestavení více než jednu třídu konfigurace. Chcete-li určit, která bude použita, je potřeba použít přepínač /configurationType.                                                                                                                                           |
| Chyba: Nepovedlo se načíst soubor nebo sestavení "&lt;assemblyName&gt;' nebo některou z jeho závislostí. Sestavení daného názvu nebo základ kódu byl neplatný. (Výjimka z HRESULT: 0x80131047)                                                                                                                                                    | To může být způsobeno nesprávně určující název sestavení nebo nemají                                                                                                                                                                                                                          |
| Chyba: Nepovedlo se načíst soubor nebo sestavení "&lt;assemblyName&gt;' nebo některou z jeho závislostí. Došlo pokusu o načtení programu v nesprávném formátu.                                                                                                                                                                          | To se stane, když se pokoušíte spustit migrate.exe x x64 aplikace. EF 5.0 a níže budou fungovat jenom na x86.                                                                                                                                                                                |
