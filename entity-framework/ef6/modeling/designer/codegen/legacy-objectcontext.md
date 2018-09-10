---
title: Návrat k objektu ObjectContext v Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: e90af3e973c71e2ce872e3edc24aafc1b2ccce0f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250332"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Návrat k objektu ObjectContext v Návrháři Entity Framework
V předchozí verzi rozhraní Entity Framework model vytvářené pomocí návrháře EF vygenerují kontextu, který je odvozen od objektu ObjectContext a tříd entit, které jsou odvozeny z EntityObject.

Počínaje EF4.1 doporučujeme přechodem na šablony generování kódu, který generuje kontextu odvozování z tříd entit DbContext a POCO.

V sadě Visual Studio 2012 získáte DbContext kód generován dle výchozího nastavení pro všechny nové modely vytvořené pomocí EF designeru. Stávající modely bude dál generovat kód ObjectContext na základě, pokud se nerozhodnete přepínat kontext databáze na základě generátor kódu.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Návratu ke generování kódu pro ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Zakázat generování kódu DbContext

Generování odvozené třídy DbContext a POCO zařizuje služba dva soubory .tt ve vašem projektu, pokud rozšiřujete souboru EDMX, v Průzkumníku řešení zobrazí se tyto soubory. Odstraňte oba tyto soubory z projektu.

![Obecné soubory kódu](~/ef6/media/codegenfiles.png)

Pokud používáte VB.NET bude nutné vybrat **zobrazit všechny soubory** tlačítko, abyste viděli vnořené soubory.

![Zobrazit všechny soubory](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Znovu povolit generování kódu pro ObjectContext

Otevřete model v EF designeru klikněte pravým tlačítkem na prázdnou část návrhové ploše a vyberte **vlastnosti**.

V okně změnit vlastnosti **strategie generování kódu** z **žádný** k **výchozí**.

![Strategie obecné kódu](~/ef6/media/codegenstrategy.png)
