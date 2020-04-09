---
title: Začínáme s rámcem entity 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 729dea2c474c35f638ccaf6673550f76e88e2667
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136270"
---
# <a name="get-started-with-entity-framework-6"></a>Začínáme s rozhraním entity 6

Tato příručka obsahuje kolekci odkazů na vybrané dokumenty, návody a videa, které vám pomohou rychle začít.

## <a name="fundamentals"></a>Základy

* [Jak získat Entity Framework](~/ef6/fundamentals/install.md)

  Zde se dozvíte, jak přidat entity framework do vašich aplikací a pokud chcete použít EF Designer, ujistěte se, že si to nainstalovat v sadě Visual Studio.

* [Vytvoření modelu: Nejprve kód, návrhář EF a pracovní postupy EF](~/ef6/modeling/index.md)

  Dáváte přednost zadání ef modelu psaní kódu nebo kreslení polí a čar?
Budete používat EF k mapování objektů na existující databázi nebo chcete EF vytvořit databázi přizpůsobenou vašim objektům?
Zde se dozvíte o dvou různých přístupech k použití EF6: EF Designer a Code First.
Ujistěte se, že budete sledovat diskusi a podívejte se na video o rozdílu.

* [Práce s DbContext](~/ef6/fundamentals/working-with-dbcontext.md)

  DbContext je první a nejdůležitější EF typ, který potřebujete naučit, jak používat. Slouží jako odpalovací rampa pro databázové dotazy a sleduje změny, které provedete v objektech, aby mohly být zachovány zpět do databáze.

* [Položit otázku](~/ef6/resources/get-help.md)

  Zjistěte, jak získat pomoc od odborníků a přispět vlastními odpověďmi komunitě.

* [Přispět](https://github.com/aspnet/EntityFramework6/)

  Entity Framework 6 používá otevřený vývojový model. Zjistěte, jak můžete ef ještě vylepšit návštěvou našeho úložiště GitHub.

## <a name="code-first-resources"></a>Kód první zdroje

  - [Kód nejprve do pracovního postupu existující databáze](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Kód nejprve do pracovního postupu nové databáze](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Mapování výčtů pomocí kódu jako první](~/ef6/modeling/code-first/data-types/enums.md)
  - [Mapování prostorových typů pomocí kódu jako první](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Psaní vlastní kód první konvence](~/ef6/modeling/code-first/conventions/custom.md)
  - [Použití první konfigurace code fluent s visual basicem](~/ef6/modeling/code-first/fluent/vb.md)
  - [První migrace kódu](~/ef6/modeling/code-first/migrations/index.md)
  - [Migrace první ho kódu v týmových prostředích](~/ef6/modeling/code-first/migrations/teams.md)
  - [Automatické migrace prvního kódu](~/ef6/modeling/code-first/migrations/automatic.md) (to se již nedoporučuje)

## <a name="ef-designer-resources"></a>Zdroje návrháře EF
  - [První pracovní postup databáze](~/ef6/modeling/designer/workflows/database-first.md)
  - [První pracovní postup modelu](~/ef6/modeling/designer/workflows/model-first.md)
  - [Mapování výčtů](~/ef6/modeling/designer/data-types/enums.md)
  - [Mapování prostorových typů](~/ef6/modeling/designer/data-types/spatial.md)
  - [Mapování dědičnosti podle hierarchie](~/ef6/modeling/designer/inheritance/tph.md)
  - [Mapování dědičnosti typu tabulka na typ](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Mapování uložených procedur pro aktualizace](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Mapování uložené procedury pro dotaz](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Dělení entity](~/ef6/modeling/designer/entity-splitting.md)
  - [Dělení tabulky](~/ef6/modeling/designer/table-splitting.md)
  - [Definování dotazu](~/ef6/modeling/designer/advanced/defining-query.md) (upřesnit)
  - [Funkce s hodnotou tabulky](~/ef6/modeling/designer/advanced/tvfs.md) (upřesnit)

## <a name="other-resources"></a>Další prostředky
  - [Asynchronní dotaz a uložení](~/ef6/fundamentals/async.md)
  - [Datová vazba s winforms](~/ef6/fundamentals/databinding/winforms.md)
  - [Datová vazba s WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Odpojené scénáře s entitami samočinného sledování](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (to se již nedoporučuje)
