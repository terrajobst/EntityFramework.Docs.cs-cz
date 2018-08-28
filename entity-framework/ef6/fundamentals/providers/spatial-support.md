---
title: Podpora zprostředkovatele pro typy prostorových – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 07eeecb5f5e3e3eab8548c4c7c0ed55c5ffb4f31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998284"
---
# <a name="provider-support-for-spatial-types"></a>Podpora zprostředkovatele pro prostorové typy
Entity Framework podporuje práci s prostorovými daty formátu prostřednictvím DbGeography nebo DbGeometry tříd. Tyto třídy závisí na konkrétních databází funkce nabízené poskytovateli rozhraní Entity Framework. Ne všichni poskytovatelé podporu prostorových dat a ty, které se mají další požadavky, jako je například instalace sestavení prostorových typů. Další informace o podporu zprostředkovatele pro prostorové typy jsou uvedeny níže.  

Dva postupy, jeden pro Code First, druhá pro první databázi nebo modelu první najdete další informace o tom, jak pomocí prostorové typy v aplikaci:  

- [Prostorové datové typy v kódu nejprve](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Typy prostorových dat v EF designeru](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Verze EF, které podporují prostorové typy  

Podpora pro typy prostorových byla zavedena v EF5. Nicméně v EF5 prostorové typy jsou podporovány pouze pokud aplikace cílí a běží na rozhraní .NET 4.5.  

Počínaje EF6 prostorové typy jsou podporovány pro aplikace cílené na rozhraní .NET 4 a .NET 4.5.  

## <a name="ef-providers-that-support-spatial-types"></a>EF poskytovatelé, které podporují prostorové typy  

### <a name="ef5"></a>EF5  

Zprostředkovatelé rozhraní Entity Framework pro EF5, který jsme víme, že podpora prostorové typy jsou:  

- Zprostředkovatel Microsoft SQL Server  
    - Tento zprostředkovatel je dodáván jako součást EF5.  
    - Tento zprostředkovatel závisí na některé další knihovny nízké úrovně, které může být nutné nainstalovat – podrobnosti najdete níže.  
- [Devart dotConnect pro Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Toto je zprostředkovatele třetí strany z Devart.  

Pokud znáte EF5 poskytovatele, který podporuje prostorové typy pak prosím získejte v kontaktu a budeme rádi přidat do tohoto seznamu.  

### <a name="ef6"></a>EF6  

Zprostředkovatelé rozhraní Entity Framework pro EF6, který jsme víme, že podpora prostorové typy jsou:  

- Zprostředkovatel Microsoft SQL Server  
    - Tento zprostředkovatel je dodáván jako součást EF6.  
    - Tento zprostředkovatel závisí na některé další knihovny nízké úrovně, které může být nutné nainstalovat – podrobnosti najdete níže.  
- [Devart dotConnect pro Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Toto je zprostředkovatele třetí strany z Devart.  

Pokud znáte EF6 poskytovatele, který podporuje prostorové typy pak prosím získejte v kontaktu a budeme rádi přidat do tohoto seznamu.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Požadavky na prostorové typy s Microsoft SQL Server  

Prostorové podpory systému SQL Server závisí na typech nízké úrovně, SQL Server – konkrétní SqlGeography a SqlGeometry. Tyto typy v sestavení Microsoft.SqlServer.Types.dll za provozu a toto sestavení není dodán jako součást EF nebo jako součást rozhraní .NET Framework.  

Při instalaci sady Visual Studio často také nainstaluje verzi systému SQL Server a bude se jednat o instalaci Microsoft.SqlServer.Types.dll.  

Pokud SQL Server není nainstalován na počítači, ve které chcete použít prostorové typy nebo typy prostorových byly vyloučeny z instalace systému SQL Server, je potřeba je nainstalovat ručně. Typy lze nainstalovat pomocí `SQLSysClrTypes.msi`, která je součástí sady Microsoft SQL Server Feature Pack. Prostorové typy jsou specifické pro verzi, SQL Server, takže doporučujeme [vyhledejte "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) webu Microsoft Download Center, pak vyberte a stáhněte si možnost, která odpovídá verzi systému SQL Server budete používat.
