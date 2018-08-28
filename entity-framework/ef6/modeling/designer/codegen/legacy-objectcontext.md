---
title: Návrat k objektu ObjectContext v Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: b52bfc36c97e1a3c7cd2d3716feb1ae48c68a56e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997309"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Návrat k objektu ObjectContext v Návrháři Entity Framework
V předchozí verzi rozhraní Entity Framework model vytvářené pomocí návrháře EF vygenerují kontextu, který je odvozen od objektu ObjectContext a tříd entit, které jsou odvozeny z EntityObject.

Počínaje EF4.1 doporučujeme přechodem na šablony generování kódu, který generuje kontextu odvozování z tříd entit DbContext a POCO.

V sadě Visual Studio 2012 získáte DbContext kód generován dle výchozího nastavení pro všechny nové modely vytvořené pomocí EF designeru. Stávající modely bude dál generovat kód ObjectContext na základě, pokud se nerozhodnete přepínat kontext databáze na základě generátor kódu.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Návratu ke generování kódu pro ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Zakázat generování kódu DbContext

Generování odvozené třídy DbContext a POCO zařizuje služba dva soubory .tt ve vašem projektu, pokud rozšiřujete souboru EDMX, v Průzkumníku řešení zobrazí se tyto soubory. Odstraňte oba tyto soubory z projektu.

![CodeGenFiles](~/ef6/media/codegenfiles.png)

Pokud používáte VB.NET bude nutné vybrat **zobrazit všechny soubory** tlačítko, abyste viděli vnořené soubory.

![ShowAllFiles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Znovu povolit generování kódu pro ObjectContext

Otevřete model v EF designeru klikněte pravým tlačítkem na prázdnou část návrhové ploše a vyberte **vlastnosti**.

V okně změnit vlastnosti **strategie generování kódu** z **žádný** k **výchozí**.

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)
