---
title: Začínáme s Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 74ae347af3c386639631f28ccb2ddbe9f444953a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416313"
---
# <a name="get-started-with-entity-framework-6"></a>Začínáme s Entity Framework 6

Tato příručka obsahuje kolekci odkazů na vybrané články k dokumentaci, návody a videa, které vám pomůžou rychle začít.

## <a name="fundamentals"></a>Základy

* [Jak získat Entity Framework](~/ef6/fundamentals/install.md)

  V tomto článku se dozvíte, jak přidat Entity Framework k vašim aplikacím a pokud chcete použít návrháře EF, ujistěte se, že jste ho nainstalovali v aplikaci Visual Studio.

* [Vytvoření modelu: Code First, Návrhář EF a pracovní postupy EF](~/ef6/modeling/index.md)

  Dáváte přednost určení modelu EF pro psaní kódu nebo polí kreslení a řádků?
Chcete použít EF k mapování objektů na existující databázi, nebo chcete, aby byl EF vytvořen databáze přizpůsobená pro vaše objekty?
Tady získáte informace o dvou různých přístupech k použití EF6: EF Designer a Code First.
Ujistěte se, že provedete diskusi a sledujte rozdíl v videu.

* [Práce s DbContext](~/ef6/fundamentals/working-with-dbcontext.md)

  DbContext je první a nejdůležitější typ EF, který potřebujete zjistit, jak používat. Slouží jako hlavní panel pro databázové dotazy a sleduje změny, které provedete u objektů, aby je bylo možné uchovat zpátky do databáze.

* [Položit otázku](~/ef6/resources/get-help.md)

  Zjistěte, jak získat pomoc od expertů a přispět vlastní odpovědi do komunity.

* [Psaní příspěvků](https://github.com/aspnet/EntityFramework6/)

  Entity Framework 6 používá otevřený vývojový model. Přečtěte si naše úložiště GitHub a zjistěte, jak vám můžeme porozumět EFm.

## <a name="code-first-resources"></a>Prostředky Code First

  - [Code First k pracovnímu postupu existující databáze](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Code First k novému pracovnímu postupu databáze](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Mapování výčtů pomocí Code First](~/ef6/modeling/code-first/data-types/enums.md)
  - [Mapování prostorových typů pomocí Code First](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Vytváření vlastních konvencí Code First](~/ef6/modeling/code-first/conventions/custom.md)
  - [Použití Code First konfigurace Fluent s Visual Basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Migrace Code First](~/ef6/modeling/code-first/migrations/index.md)
  - [Migrace Code First v týmových prostředích](~/ef6/modeling/code-first/migrations/teams.md)
  - [Automatické migrace Code First](~/ef6/modeling/code-first/migrations/automatic.md) (už se nedoporučuje)

## <a name="ef-designer-resources"></a>Prostředky návrháře EF
  - [Pracovní postup Database First](~/ef6/modeling/designer/workflows/database-first.md)
  - [Pracovní postup Model First](~/ef6/modeling/designer/workflows/model-first.md)
  - [Výčty mapování](~/ef6/modeling/designer/data-types/enums.md)
  - [Mapování prostorových typů](~/ef6/modeling/designer/data-types/spatial.md)
  - [Mapování dědičnosti mezi tabulkami na hierarchii](~/ef6/modeling/designer/inheritance/tph.md)
  - [Mapování dědičnosti typů na základě tabulky](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Mapování uložených procedur pro aktualizace](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Mapování uložených procedur pro dotaz](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Dělení entity](~/ef6/modeling/designer/entity-splitting.md)
  - [Dělení tabulky](~/ef6/modeling/designer/table-splitting.md)
  - [Definování dotazu](~/ef6/modeling/designer/advanced/defining-query.md) (rozšířené)
  - [Funkce vracející tabulku](~/ef6/modeling/designer/advanced/tvfs.md) (rozšířené)

## <a name="other-resources"></a>Další prostředky
  - [Asynchronní dotazování a ukládání](~/ef6/fundamentals/async.md)
  - [Datová vazba s WinForms](~/ef6/fundamentals/databinding/winforms.md)
  - [Vázání dat pomocí WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Odpojení scénářů s entitami pro sledování sebe](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (už se nedoporučuje)
