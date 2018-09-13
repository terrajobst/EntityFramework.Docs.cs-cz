---
title: Případové studie pro Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490873"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Případové studie společnosti Microsoft pro Entity Framework
Případové studie na této stránce zvýrazněte několik skutečné produkční projekty, které jste použili Entity Framework.
> [!NOTE]
> Podrobné verzích tyto případové studie, už nejsou k dispozici na webu společnosti Microsoft. Proto jsme odebrali odkazy.

## <a name="epicor"></a>Epicor
Epicor je velkých softwarových globální společnost (s více než 400 vývojáři), který vyvíjí řešení plánování ERP (Enterprise Resource) pro společnosti, ve víc než 150 zemích.
Jejich produktem, Epicor 9, je založená na Service-Oriented architektury (SOA) pomocí rozhraní .NET Framework.
Potýkají s mnoha požadavky zákazníků o podporu pro Language Integrated Query (LINQ) a také chce snížit zatížení na back-end serverech SQL, tým se rozhodli upgradovat na Visual Studio 2010 a rozhraní .NET Framework 4.0.
Pomocí Entity Framework 4.0, studenti mohli k dosažení těchto cílů a zároveň výrazně zjednodušuje vývoj a údržbu.
Zejména rozhraní Entity Framework bohatou podporu T4 můžou úplnou kontrolu nad jejich generovaný kód a automaticky vytvářet v ukládání výkonu funkce, jako je předem kompilovaných dotazy a ukládání do mezipaměti.

> "Můžeme regulovat některé testy výkonu nedávno s existujícím kódem a jsme byli schopni snížit požadavky na systém SQL Server 90 procent.
To je z důvodu rozhraní ADO.NET Entity Framework 4." – Erik Johnsonem, viceprezident, produktový výzkum  

## <a name="veracity-solutions"></a>Pravdivosti řešení
Získá plánování akcí softwarového systému, která se má být obtížné udržovat a rozšířit nad dlouhodobě, používá pravdivosti řešení sady Visual Studio 2010 znovu zapsat jako výkonný a snadným ovládáním Rich Internet Application založená na technologii Silverlight 4.
Použití .NET RIA Services, studenti mohli rychle sestavit vrstvu služby jako nadstavby rozhraní Entity Framework, která zabránit zdvojení kódu a povolené pro běžné ověření a logika ověřování napříč úrovněmi.  

> "Jsme se prodávají na rozhraní Entity Framework při bylo poprvé dostupné a Entity Framework 4 ukázal být ještě lepší.
Vylepšené nástroje a je jednodušší pracovat s edmx soubory, které definují konceptuální model, úložiště modelu a mapování mezi tyto modely... S rozhraním Entity Framework, můžu získat této vrstvy přístupu k datům práce za den a sestavte ho si, jak se můžu obrátit.
Entity Framework je naše vrstvy přístupu k datům de facto stane; Nevím, proč kdokoli by to mohlo hodit." – Joe McBride, hlavní vývojář

## <a name="nec-display-solutions-of-america"></a>Společnost NEC zobrazení řešení amerických
Společnost NEC chtěli vstupují na trh pro digitální reklamu založené na místě s řešením mít prospěch inzerenty a vlastníci sítě a zvyšte svou vlastní výnosy.
K tomu, že spuštění pár webových aplikací, které automatizaci ručních procesů vyžaduje tradiční ad kampaně.
Lokality byly vytvořeny s použitím technologie ASP.NET, Silverlight 3, AJAX a služby WCF, spolu s Entity Framework v datové vrstvě přístupu ke komunikaci s SQL Server 2008.

> "S SQL serverem, jsme popisovač jsme může zajistit si tak propustnost, potřebujeme k poskytování inzerenty a sítím pomocí informací v reálném čase a spolehlivost pomáhá zajistit, že informace v naší nejdůležitější aplikace bude mít vždycky k dispozici"-Mike Corcoran Ředitel IT

## <a name="darwin-dimensions"></a>Dimenze Darwin
Pomocí technologie Microsoft široký rozsah, tým na adrese Darwin nedodá vytvořit Evolver - online avatar portál, příjemci můžou použít k vytvoření působivé, živoucí avatary pro použití hry, animace a sociální sítě stránky.
Produktivita výhody rozhraní Entity Framework a přijímání změn v komponenty, jako jsou Windows Workflow Foundation (WF) a Windows Server AppFabric (mezipaměť vysoce škálovatelných aplikací v paměti) bylo možné poskytovat úžasné produktu v 35 % menší tým dobu vývoje.
Přestože máte aktivovanou členové týmu rozdělit mezi více zemí, tým procesu agilního vývoje s týdenních vydáních.

 > "Snažíme nevytvářet technologie pro saké technologie. Jako při spuštění je důležité, že můžeme využívat technologie, která šetří čas a peníze.
 .NET je volba pro rychlé a cenově výhodný vývoj". – Zachary Olsen, architekt  

## <a name="silverware"></a>Stříbrné nádobí
S více než 15 letech zkušeností s vývojem POS (POS) řešení pro malé a střední restaurace skupiny vývojový tým na stříbrné nádobí nedodá k vylepšení svých produktů s více funkcemi na podnikové úrovni k upoutání pozornosti větší restaurace řetězců.
Pomocí nejnovější verze sady vývojových nástrojů od Microsoftu, studenti mohli vytvářet nové řešení čtyřikrát rychleji než dřív.
LINQ a Entity Framework snazší přesun z Crystal Reports do systému SQL Server 2008 a SQL Server Reporting Services (SSRS) pro svá úložiště dat a sestav, jako jsou klíčové nové funkce.

> "Správa počáteční datum je klíčem k úspěchu stříbrné nádobí – a to je důvod, proč jsme se rozhodli přijmout, SQL Reporting." -Nicholas Romanidis, ředitel IT / technické softwaru
