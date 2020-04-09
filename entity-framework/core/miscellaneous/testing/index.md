---
title: Testování součástí pomocí EF Core - EF Core
description: Různé přístupy k testování aplikací, které používají EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634254"
---
# <a name="testing-code-that-uses-ef-core"></a>Testovací kód, který používá EF Core

Testování kódu, který přistupuje k databázi, vyžaduje buď:
* Spouštění dotazů a aktualizací proti stejnému databázovému systému používanému v produkčním prostředí.
* Spouštění dotazů a aktualizací proti jiným snadněji spravovatelným databázovým systémem.
* Použití test zdvojnásobí nebo nějaký jiný mechanismus, aby se zabránilo použití databáze vůbec.

Tento dokument popisuje kompromisy zapojené do každé z těchto voleb a ukazuje, jak ef core lze použít s každým přístupem.  

## <a name="all-database-providers-are-not-equal"></a>Všichni poskytovatelé databáze nejsou rovni.

Je velmi důležité si uvědomit, že EF Core není navržen tak, aby abstraktní každý aspekt základního databázového systému.
Místo toho EF Core je společná sada vzorů a konceptů, které lze použít s libovolnýdatabázový systém.
Zprostředkovatelé databáze EF Core pak vrstva databázi specifické chování a funkce přes tento společný rámec.
To umožňuje každému databázovému systému dělat to, co umí nejlépe, a přitom zachovat shodnost, kde je to vhodné, s jinými databázovými systémy. 

V zásadě to znamená, že přepnutí zprostředkovatele databáze změní chování EF Core a aplikace nelze očekávat, že bude fungovat správně, pokud explicitně účty pro všechny rozdíly v chování.
To bylo řečeno, v mnoha případech to bude fungovat, protože tam je vysoký stupeň shodnosti mezi relačnídatabáze.
Tohle je dobré a špatné.
Dobré, protože přechod mezi databázemi může být poměrně snadné.
Špatné, protože může poskytnout falešný pocit zabezpečení, pokud aplikace není plně testována proti novému databázovému systému.  

## <a name="approach-1-production-database-system"></a>Přístup 1: Systém výrobních databází

Jak je popsáno v předchozí části, jediný způsob, jak se ujistit, že testujete, co běží v produkčním prostředí je použít stejný databázový systém.
Například pokud nasazená aplikace používá SQL Azure, pak testování by mělo být provedeno také proti SQL Azure.

Však s každý vývojář spustit testy proti SQL Azure při aktivní práci na kódu by bylo pomalé a nákladné.
To ilustruje hlavní kompromis v rámci těchto přístupů: kdy je vhodné odchýlit se od systému výrobních databází, aby se zlepšila účinnost zkoušek?

Naštěstí v tomto případě je odpověď poměrně snadná: použijte místní nebo místní SQL Server pro testování vývojářů.
SQL Azure a SQL Server jsou velmi podobné, takže testování proti SQL Server je obvykle přiměřené kompromis.
Jak bylo řečeno, je stále rozumné spustit testy proti SQL Azure sám před vstupem do produkčního prostředí.
 
### <a name="localdb"></a>Místní databáze 

Všechny hlavní databázové systémy mají nějakou formu "Developer Edition" pro místní testování.
SQL Server má také funkci nazvanou [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).
Hlavní výhodou LocalDb je, že se točí do instance databáze na vyžádání.
Tím se zabrání spuštění databázové služby v počítači i v případě, že nespouštějí testy.

LocalDb není bez jeho problémů:
* Nepodporuje vše, co [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) dělá.
* Není k dispozici na Linuxu.
* To může způsobit zpoždění při prvním spuštění testu jako služba se točil nahoru.

Osobně jsem nikdy nenašel to problém s databázovou službu běží na mém dev stroji a já bych obecně doporučujeme používat Developer Edition místo.
Nicméně, to může být vhodné pro některé lidi, zejména na méně výkonné dev stroje.  

## <a name="approach-2-sqlite"></a>Přístup 2: SQLite

EF Core testuje zprostředkovatele SERVERU SQL Server především spuštěním proti místní instanci serveru SQL Server.
Tyto testy spustit desítky tisíc dotazů během několika minut na rychlé mši.
To ukazuje, že použití reálného databázového systému může být performant řešení.
Je mýtus, že použití některých lehčí-váha databáze je jediný způsob, jak spustit testy rychle.

Jak již bylo řečeno, co když z nějakého důvodu nemůžete spustit testy proti něčemu, co se blíží vašemu produkčnímu databázovému systému?
Další nejlepší volbou je použít něco s podobnou funkčností.
To obvykle znamená další relační databáze, pro které [SQLite](https://sqlite.org/index.html) je zřejmá volba.

SQLite je dobrá volba, protože:
* Běží v procesu s vaší aplikací a tak má nízkou režii.
* Používá jednoduché, automaticky vytvořené soubory pro databáze, a proto nevyžaduje správu databáze.
* Má režim v paměti, který zabraňuje i vytvoření souboru.

Nezapomeňte však, že:
* Nevyhnutelnost SQLite nepodporuje vše, co váš produkční databázový systém dělá.
* SQLite se bude chovat jinak než produkční databázový systém pro některé dotazy.

Takže pokud používáte SQLite pro některé testování, ujistěte se, že také test proti skutečnédatabázový systém.

Viz [testování s SQLite](xref:core/miscellaneous/testing/sqlite) pro EF Core specifické pokyny. 

## <a name="approach-3-the-ef-core-in-memory-database"></a>Přístup 3: Ef Core v paměti databáze

EF Core je dodáván s databází v paměti, kterou používáme pro interní testování samotného EF Core.
Tato databáze obecně **není vhodná jako náhrada za testovací aplikace, které používají EF Core**. Konkrétně:
* Nejedná se o relační databázi
* Nepodporuje transakce
* Není optimalizován pro výkon

Nic z toho není velmi důležité při testování vnitřních souborů EF Core, protože ji používáme konkrétně tam, kde je databáze irelevantní pro test.
Na druhou stranu tyto věci mají tendenci být velmi důležité při testování aplikace, která používá EF Core.

## <a name="unit-testing"></a>Testování jednotek

Zvažte testování obchodní logiky, která může být nutné použít některá data z databáze, ale není ze své podstaty testování interakcí databáze.
Jednou z možností je použít [test double,](https://en.wikipedia.org/wiki/Test_double) jako je například falešný nebo falešný.

Pro interní testování EF Core používáme zkušební dvojinky.
Však nikdy se snažíme zesměšňovat DbContext nebo IQueryable.
Je to obtížné, těžkopádné a křehké.
**Nedělej to.**

Místo toho používáme databázi v paměti při testování částí něco, co používá DbContext.
V tomto případě je vhodné používat databázi v paměti, protože test není závislý na chování databáze.
Jen to nedělají k testování skutečných databázových dotazů nebo aktualizací.   

Viz [Testování s poskytovatelem v paměti](xref:core/miscellaneous/testing/in-memory) pro EF Core specifické pokyny pro použití databáze v paměti pro testování částí.
