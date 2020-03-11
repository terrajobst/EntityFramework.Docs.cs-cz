---
title: Podpora poskytovatele pro prostorové typy – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416337"
---
# <a name="provider-support-for-spatial-types"></a>Podpora poskytovatelů prostorových typů
Entity Framework podporuje práci s prostorovými daty prostřednictvím tříd DbGeography nebo DbGeometry. Tyto třídy spoléhají na funkce specifické pro databázi, které nabízí poskytovatel Entity Framework. Ne všichni zprostředkovatelé podporují prostorová data a ty, které mohou mít další požadavky, jako je instalace sestavení prostorových typů. Další informace o podpoře poskytovatele pro prostorové typy jsou uvedené níže.  

Další informace o tom, jak používat prostorové typy v aplikaci, najdete ve dvou návodech, jeden pro Code First, druhý pro Database First nebo Model First:  

- [Typy prostorových dat v Code First](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Typy prostorových dat v Návrháři EF](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Verze EF podporující prostorové typy  

Podpora prostorových typů byla představena v EF5. Nicméně v prostorových typech EF5 jsou podporovány pouze v případě, že je aplikace cílena a spuštěna v rozhraní .NET 4,5.  

Počínaje EF6 prostorovými typy jsou podporovány pro aplikace zaměřené na rozhraní .NET 4 i .NET 4,5.  

## <a name="ef-providers-that-support-spatial-types"></a>Zprostředkovatelé EF podporující prostorové typy  

### <a name="ef5"></a>EF5  

Poskytovatelé Entity Framework pro EF5, o kterých víte, že podporují prostorové typy:  

- Poskytovatel Microsoft SQL Server  
    - Tento poskytovatel se dodává jako součást EF5.  
    - Tento zprostředkovatel závisí na některých dalších knihovnách nízké úrovně, které můžou být potřeba nainstalovat – podrobnosti najdete níže.  
- [Devart dotConnect pro Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Toto je zprostředkovatel třetí strany z Devart.  

Pokud znáte poskytovatele EF5, který podporuje prostorové typy, kontaktujte prosím kontakt a my ho budeme moct přidat do tohoto seznamu.  

### <a name="ef6"></a>EF6  

Poskytovatelé Entity Framework pro EF6, o kterých víte, že podporují prostorové typy:  

- Poskytovatel Microsoft SQL Server  
    - Tento poskytovatel se dodává jako součást EF6.  
    - Tento zprostředkovatel závisí na některých dalších knihovnách nízké úrovně, které můžou být potřeba nainstalovat – podrobnosti najdete níže.  
- [Devart dotConnect pro Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Toto je zprostředkovatel třetí strany z Devart.  

Pokud znáte poskytovatele EF6, který podporuje prostorové typy, kontaktujte prosím kontakt a my ho budeme moct přidat do tohoto seznamu.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Předpoklady pro prostorové typy s Microsoft SQL Server  

SQL Server prostorová podpora závisí na typech SQL Server SqlGeography a SqlGeometry specifických pro nižší úroveň. Tyto typy jsou živé v sestavení Microsoft. SqlServer. Types. dll a toto sestavení se nedodává jako součást EF nebo jako součást .NET Framework.  

Když je nainstalována aplikace Visual Studio, často také nainstaluje verzi SQL Server a bude obsahovat instalaci Microsoft. SqlServer. Types. dll.  

Pokud v počítači, kde chcete použít prostorové typy, není nainstalováno SQL Server, nebo pokud byly z instalace SQL Server vyloučeny prostorové typy, pak je budete muset nainstalovat ručně. Typy lze nainstalovat pomocí `SQLSysClrTypes.msi`, který je součástí sady Microsoft SQL Server Feature Pack. Prostorové typy jsou SQL Server specifické pro verzi, proto doporučujeme [Vyhledat "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) na webu Microsoft Download Center a pak vybrat a stáhnout možnost, která odpovídá verzi SQL Server, kterou budete používat.
