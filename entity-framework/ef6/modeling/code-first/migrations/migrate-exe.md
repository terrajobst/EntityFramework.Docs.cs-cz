---
title: Použití souboru migrovat. exe – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 34866ddbbf81f090a064af148a612dd354ae9401
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824808"
---
# <a name="using-migrateexe"></a>Použití souboru migrovat. exe
Migrace Code First lze použít k aktualizaci databáze v rámci sady Visual Studio, ale lze ji provést také pomocí nástroje příkazového řádku migrovat. exe. Tato stránka poskytuje rychlý přehled o tom, jak pomocí nástroje Migration. exe provádět migrace v databázi.

> [!NOTE]
> V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích. Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="copy-migrateexe"></a>Kopírování souboru migrace. exe

Při instalaci Entity Framework pomocí nástroje NuGet migrace. exe se nachází ve složce Tools staženého balíčku. V &lt;složka projektu&gt;\\balíčky\\EntityFramework.&lt;verze&gt;\\Tools

Po dokončení migrace. exe je třeba ho zkopírovat do umístění sestavení, které obsahuje vaše migrace.

Pokud je vaše aplikace cílena na rozhraní .NET 4, a ne 4,5, pak budete muset zkopírovat soubor **Redirect. config** do umístění a přejmenovat ho **. exe. config**. To znamená, že při migraci. exe získá správné přesměrování vazby, aby bylo možné najít Entity Framework sestavení.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Soubory .NET 4,5](~/ef6/media/net45files.png) | ![Soubory .NET 4,0](~/ef6/media/net40files.png) |

> [!NOTE]
> migrace. exe nepodporuje sestavení x64.

Po přesunutí souboru Migration. exe do správné složky byste ho měli být schopni použít ke spouštění migrace z databáze. Tento nástroj je navržený tak, aby prováděl migrace. Nemůže generovat migrace ani vytvořit skript SQL.

## <a name="see-options"></a>Zobrazit možnosti

``` console
Migrate.exe /?
```

Výše uvedený postup zobrazí stránku s nápovědu přidruženou k tomuto nástroji. aby to fungovalo, budete muset mít EntityFramework. dll ve stejném umístění, ve kterém spouštíte příkaz migrovat. exe.

## <a name="migrate-to-the-latest-migration"></a>Migrace na nejnovější migraci

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile="..\\web.config"
```

Při spuštění souboru Migration. exe jediným povinným parametrem je sestavení, které je sestavením obsahující migrace, které se pokoušíte spustit, ale bude používat všechna nastavení založená na konvencích, pokud neurčíte konfigurační soubor.

## <a name="migrate-to-a-specific-migration"></a>Migrace na konkrétní migraci

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /targetMigration="AddTitle"
```

Pokud chcete spustit migrace až do konkrétní migrace, můžete zadat název migrace. Tato akce spustí všechny předchozí migrace podle potřeby, dokud se nezadá migrace.

## <a name="specify-working-directory"></a>Zadat pracovní adresář

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /startupDirectory="c:\\MyApp"
```

Pokud sestavení obsahuje závislosti nebo čte soubory vzhledem k pracovnímu adresáři, budete muset nastavit startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Zadejte konfiguraci migrace, která se má použít.

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile="..\\web.config"
```

Pokud máte více tříd konfigurace migrace, třídy, které dědí z DbMigrationConfiguration, je nutné zadat, který má být použit pro toto spuštění. To je určeno zadáním volitelného druhého parametru bez přepínače, jak je uvedeno výše.

## <a name="provide-connection-string"></a>Zadat připojovací řetězec

``` console
Migrate.exe BlogDemo.dll /connectionString="Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI" /connectionProviderName="System.Data.SqlClient"
```

Pokud chcete zadat připojovací řetězec z příkazového řádku, musíte zadat také název poskytovatele. Pokud neurčíte název poskytovatele, vyvolá se výjimka.

## <a name="common-problems"></a>Běžné problémy

| Chybová zpráva                                                                                                                                                                                                                                                                                                                      | Řešení                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Neošetřená výjimka: System. IO. FileLoadException: nelze načíst soubor nebo sestavení EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 nebo jedna z jejích závislostí. Nalezená definice manifestu sestavení neodpovídá odkazu na sestavení. (Výjimka z HRESULT: 0x80131040)         | To obvykle znamená, že máte spuštěnou aplikaci .NET 4 bez souboru Redirect. config. Musíte zkopírovat soubor Redirect. config do stejného umístění jako soubor migrovat. exe a přejmenovat ho na soubor migrovat. exe. config.                                                                                       |
| Neošetřená výjimka: System. IO. FileLoadException: nelze načíst soubor nebo sestavení EntityFramework, Version = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 nebo jedna z jejích závislostí. Nalezená definice manifestu sestavení neodpovídá odkazu na sestavení. (Výjimka z HRESULT: 0x80131040)          | Tato výjimka znamená, že máte spuštěnou aplikaci .NET 4,5 s příponou Redirect. config zkopírovanou do umístění migrace. exe. Pokud je vaše aplikace .NET 4,5, nemusíte mít konfigurační soubor s přesměrováním v. Odstraňte soubor migrovat. exe. config.                                    |
| Chyba: databázi se nepovedlo aktualizovat tak, aby odpovídala aktuálnímu modelu, protože existují probíhající změny a Automatická migrace je zakázaná. Napíšete probíhající změny modelu do migrace založené na kódu nebo povolte automatickou migraci. Chcete-li povolit automatickou migraci, nastavte DbMigrationsConfiguration. AutomaticMigrationsEnabled na hodnotu true. | K této chybě dochází, pokud jste migraci nevytvořili, když jste nevytvořili migraci, aby se projevily změny provedené v modelu, a databáze neodpovídá modelu. Když se do třídy modelu přidá vlastnost a pak se spustí soubor Migration. exe bez vytvoření migrace pro upgrade databáze, je to příklad. |
| Chyba: typ není přeložen pro člena ' System. data. entity. migrations. Design. ToolingFacade + UpdateRunner, EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 '.                                                                                                                                       | Tato chyba může být způsobena zadáním nesprávného spouštěcího adresáře. Musí se jednat o umístění souboru migrace. exe.                                                                                                                                                                                      |
| Neošetřená výjimka: System. NullReferenceException: odkaz na objekt není nastaven na instanci objektu. <br/>   v System. data. entity. migrations. Console. program. Main (String [] args)                                                                                                                                             | To může být způsobeno tím, že není zadán požadovaný parametr pro nějaký scénář, který používáte. Například zadání připojovacího řetězce bez zadání názvu poskytovatele.                                                                                                                        |
| Chyba: v sestavení ' ClassLibrary1 ' byl nalezen více než jeden typ konfigurace migrace. Zadejte název, který se má použít.                                                                                                                                                                                                  | V případě chybových stavů je v daném sestavení více než jedna třída konfigurace. K určení, které chcete použít, je nutné použít přepínač/configurationType.                                                                                                                                           |
| Chyba: nelze načíst soubor nebo sestavení&lt;assemblyName&gt;nebo jedna z jeho závislostí. Zadaný název sestavení nebo základ kódu byl neplatný. (Výjimka z HRESULT: 0x80131047)                                                                                                                                                    | To může být způsobeno nesprávným zadáním názvu sestavení nebo nemusíte mít                                                                                                                                                                                                                          |
| Chyba: nelze načíst soubor nebo sestavení&lt;assemblyName&gt;nebo jedna z jeho závislostí. Byl proveden pokus o načtení programu v nesprávném formátu.                                                                                                                                                                          | K tomu dochází, pokud se pokoušíte spustit příkaz migrovat. exe proti aplikaci x64. EF 5,0 a nižší budou fungovat jenom v x86.                                                                                                                                                                                |
