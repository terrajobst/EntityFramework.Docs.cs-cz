---
title: Práce s odpojenými entitami - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419526"
---
# <a name="working-with-disconnected-entities"></a>Práce s odpojenými entitami
V aplikaci založené na rozhraní entity je třída kontextu zodpovědná za zjišťování změn použitých na sledované entity. Volání SaveChanges metoda přetrvává změny sledované kontextu do databáze. Při práci s n-vrstvé aplikace, objekty entity jsou obvykle změněny při odpojení od kontextu a musíte se rozhodnout, jak sledovat změny a sestavy těchto změn zpět do kontextu. Toto téma popisuje různé možnosti, které jsou k dispozici při použití entity framework s odpojenými entitami.   

## <a name="web-service-frameworks"></a>Rozhraní webových služeb

Technologie webových služeb obvykle podporují vzory, které lze použít k zachování změn na jednotlivých odpojených objektech. Například ASP.NET webové rozhraní API umožňuje akce řadiče kódu, které mohou zahrnovat volání EF zachovat změny provedené v objektu v databázi. Nástroje webového rozhraní API v sadě Visual Studio ve skutečnosti usnadňují vytvoření uživatelského rozhraní webového rozhraní API z modelu Entity Framework 6. Další informace naleznete [v tématu použití webového rozhraní API s entity Framework 6](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

V minulosti existovalo několik dalších technologií webových služeb, které nabízely integraci s rozhraním Entity Framework, jako jsou [wcf datové služby](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) a [ria služby](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>Nízkoúrovňová EF API

Pokud nechcete používat existující n-vrstvé řešení nebo pokud chcete přizpůsobit, co se děje uvnitř akce řadiče ve službách webového rozhraní API, entity framework poskytuje rozhraní API, která umožňují použít změny provedené na odpojené vrstvě. Další informace naleznete v tématu [Přidání, Připojení a stav entity](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Entity samočinného sledování  

Sledování změn na libovolných grafech entit při odpojení od kontextu EF je pevný problém. Jedním z pokusů o jeho vyřešení byla šablona generování kódu entity samočinného sledování. Tato šablona generuje třídy entit, které obsahují logiku ke sledování změn provedených na odpojené vrstvě jako stav v samotných entitách. Sada rozšiřujících metod je také generována k použití těchto změn v kontextu.

Tuto šablonu lze použít s modely vytvořenými pomocí EF Designer, ale nelze použít s modely Code First. Další informace naleznete [v tématu Entity samočinného sledování](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Už nedoporučujeme používat šablonu self-tracking-entities. Bude i nadále k dispozici pouze pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte jiné alternativy, jako jsou [sledovatelné entity](https://trackableentities.github.io/), což je technologie podobná samoobslužným sledovacím entitám, která je aktivněji vyvinuta komunitou, nebo psaní vlastního kódu pomocí nízkoúrovňových nastavení api pro sledování změn.
