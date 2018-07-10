---
title: Návrat k objektu ObjectContext v Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912787"
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
