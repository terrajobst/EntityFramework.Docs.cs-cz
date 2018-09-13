---
title: Práce s odpojené entity - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: beb3847ce507a2112ac0d396a2023c7c4e2fca7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489931"
---
# <a name="working-with-disconnected-entities"></a>Práce s odpojené entity
V aplikaci založené na rozhraní Entity Framework je zodpovědná za vyhledávání podpory změny se použily sledované entity třídy kontextu. Volá metodu SaveChanges nevyřeší změny sledované podle kontextu do databáze. Při práci s n vrstvé aplikace, objekty entity jsou obvykle upraveny odpojené od kontextu, a musíte rozhodnout, jak sledovat změny a sestavy tyto změny zpět do kontextu. Toto téma popisuje různé možnosti, které jsou k dispozici, pokud používá nástroj Entity Framework s entitami.   

## <a name="web-service-frameworks"></a>Webové služby platformy

Webové služby technologie obvykle podporují vzory, které lze použít k uchování změn na jednotlivých objektů odpojené. Například rozhraní ASP.NET Web API vám umožní na akce kontroleru kódu, které může zahrnovat volání do EF a zachová tak změny provedené na objekt v databázi. Ve skutečnosti webového rozhraní API nástroje v sadě Visual Studio usnadňuje generování uživatelského rozhraní kontroler Web API z vašeho modelu Entity Framework 6. Další informace najdete v tématu [pomocí rozhraní Web API s Entity Framework 6](https://docs.microsoft.com/en-us/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

V minulosti byly několik dalších webových služeb technologií, která nabízí integraci s Entity Framework, jako je třeba [služeb WCF Data Services](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) a [RIA Services](https://docs.microsoft.com/en-us/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>Rozhraní API nízké úrovně EF

Pokud nechcete použít existující n vrstvého řešení, nebo pokud chcete přizpůsobit, co se stane v akci kontroleru služby webového rozhraní API, Entity Framework poskytuje rozhraní API, která umožní uplatnit změny provedené u odpojených úrovně. Další informace najdete v tématu [přidat, připojit a entity stavu](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Vlastní sledování entit  

Sledování změn pro libovolné grafy entit odpojené od kontextu EF je obtížné problém. Generování kódu entity Self-Tracking šablona byl jeden ze pokusy o jeho řešení. Tato šablona vygeneruje tříd entit, které obsahují logiku ke sledování změny provedené u odpojených úrovně jako stav v entitách, sami. Generuje sadu rozšiřujících metod na tyto změny použít na kontext se taky.

Tuto šablonu lze použít s modely vytvořené pomocí EF designeru, ale nelze použít s Code First modely. Další informace najdete v tématu [Self-Tracking entity](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Už doporučujeme použít šablonu samoobslužného tracking entity. Pouze bude nadále být k dispozici pro podporu stávající aplikace. Pokud aplikace potřebuje pracovat s grafy odpojených entit, zvažte další možnosti, jako [organizovaným entity](http://trackableentities.github.io/), což je technologie, podobně jako u samoobslužného-Tracking-entit, které je více aktivně vyvíjen Komunita, nebo psaní vlastního kódu pomocí nízké úrovně rozhraní API pro sledování změn.
