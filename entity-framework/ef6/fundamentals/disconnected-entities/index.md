---
title: Práce s odpojenými entitami – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419526"
---
# <a name="working-with-disconnected-entities"></a>Práce s odpojenými entitami
V aplikaci založené na Entity Framework je kontextová třída zodpovědná za zjištění změn použitých u sledovaných entit. Volání metody SaveChanges uchovává změny sledované kontextem databáze. Při práci s n-vrstvými aplikacemi jsou objekty entit obvykle měněny během odpojení od kontextu a Vy musíte rozhodnout, jak sledovat změny a ohlásit tyto změny zpět do kontextu. Toto téma popisuje různé možnosti, které jsou k dispozici při použití Entity Framework s odpojenými entitami.   

## <a name="web-service-frameworks"></a>Rozhraní webové služby

Technologie webových služeb obvykle podporují vzory, které lze použít k uchovávání změn v jednotlivých odpojených objektech. Například webové rozhraní API ASP.NET umožňuje kódu provádět akce kontroleru, které mohou zahrnovat volání do EF, aby trvaly změny objektu v databázi. Nástroje webového rozhraní API v aplikaci Visual Studio ve skutečnosti usnadňují vytvoření kontroleru webového rozhraní API z modelu Entity Framework 6. Další informace najdete v tématu [použití webového rozhraní API s Entity Framework 6](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

V minulosti existovalo několik technologií webových služeb, které nabízejí integraci s Entity Framework, jako jsou [WCF Data Services](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) a [RIA služby](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>Rozhraní API EF nízké úrovně

Pokud nechcete používat existující n-vrstvé řešení, nebo pokud chcete přizpůsobit, co se stane uvnitř akce kontroleru v rámci služby webového rozhraní API, Entity Framework poskytuje rozhraní API, která umožňují aplikovat změny provedené na odpojené úrovni. Další informace najdete v tématu věnovaném [Přidání, připojení a stavu entity](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Entity pro sledování sebe  

Sledování změn u libovolných grafů entit během odpojení od kontextu EF je pevný problém. Jedním z pokusů o jeho řešení byla šablona generování kódu entit s automatickým sledováním. Tato šablona generuje třídy entit, které obsahují logiku pro sledování změn provedených na odpojené úrovni jako stav v entitách samotných. Pro použití těchto změn v kontextu je vygenerována také sada rozšiřujících metod.

Tuto šablonu lze použít s modely vytvořenými pomocí návrháře EF, ale nelze je použít s Code Firstmi modely. Další informace najdete v tématu [entity pro sledování sebe](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Nedoporučujeme používat šablonu samoobslužné sledování – entity. Bude dál k dispozici jenom pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte další alternativy, jako jsou například [sledované entity](https://trackableentities.github.io/), což je technologie podobná entitám s vlastním sledováním, které jsou aktivně vyvíjené komunitou, nebo psaním vlastního kódu s využitím rozhraní API pro sledování změn nízké úrovně.
