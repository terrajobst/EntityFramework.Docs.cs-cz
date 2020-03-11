---
title: Případové studie Entity Framework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417079"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Případové studie Microsoftu pro Entity Framework
Případové studie na této stránce zvýrazňují několik skutečných produkčních projektů, které jsou zaměstnané Entity Framework.
> [!NOTE]
> Podrobné verze těchto případových studií již nejsou k dispozici na webu společnosti Microsoft. Proto byly odkazy odebrány.

## <a name="epicor"></a>Epicor
Epicor je velká globální softwarová společnost (s více než 400 vývojáři), která vyvíjí řešení pro plánování podnikových zdrojů (ERP) pro společnosti ve více než 150 zemích.
Nejdůležitější produkt Epicor 9 je založený na architektuře SOA (Service-orientované) pomocí .NET Framework.
V rámci řady zákaznických žádostí o poskytnutí podpory jazyka LINQ (Language Integrated Query) a také pro omezení zatížení na svých back-end SQL serverech se tým rozhodl upgradovat na Visual Studio 2010 a .NET Framework 4,0.
Pomocí Entity Framework 4,0 byly schopné dosáhnout těchto cílů a také významně zjednodušit vývoj a údržbu.
Konkrétně podpora bohatých T4 Entity Framework jim umožní plně ovládat jejich vygenerovaný kód a automaticky vytvářet v funkcích pro ukládání výkonu, jako jsou předem kompilované dotazy a ukládání do mezipaměti.

> "Provedli jsme některé testy výkonu v poslední době se stávajícím kódem a nemohli jsme omezit požadavky na SQL Server o 90 procent.
Důvodem je, že ADO.NET Entity Framework 4. " – Erik Johnsonem, viceprezident, výzkumný produkt  

## <a name="veracity-solutions"></a>Řešení s pravdivostí
Získali jsme softwarový systém pro plánování událostí, který se bude obtížně udržovat a rozšířit na dlouhodobá řešení s využitím sady Visual Studio 2010 k jeho novému zápisu jako výkonné a snadno použitelné bohatý internetovou aplikaci postavenou na Silverlight 4.
Pomocí služeb .NET RIA byly schopné rychle vytvořit vrstvu služby nad Entity Framework, která se vyhnout duplicitám kódu a povolují se pro běžné ověřování a logiku ověřování napříč úrovněmi.  

> "Při prvním zavedení jsme prodali Entity Framework a Entity Framework 4 bylo prověřeno ještě lepší.
Vylepšení nástrojů a je snazší manipulovat se soubory. edmx, které definují koncepční model, model úložiště a mapování mezi těmito modely... Pomocí Entity Framework můžu získat tuto vrstvu přístupu k datům za den – a vytvořit ji při tom, jak mám.
Entity Framework je naše de facto Data Access Layer; Nevím, proč ji nikdo nepoužívá. " – Jan McBride, vedoucí vývojář

## <a name="nec-display-solutions-of-america"></a>Řešení pro zobrazení NEC v Americe
NEC chtěli přejít na trh s využitím digitálního reklamního inzerce pomocí řešení pro zvýhodnění inzerentů a vlastníků sítě a zvýšit svoje vlastní výnosy.
Za tímto účelem se spustí dvojice webových aplikací, které automatizují ruční procesy vyžadované v tradiční kampani služby AD.
Weby byly sestaveny pomocí ASP.NET, Silverlight 3, AJAX a WCF, spolu s Entity Framework ve vrstvě přístupu k datům, aby se nacházely SQL Server 2008.

> "Díky SQL Server jsme zjistili propustnost, kterou potřebujeme k poskytování inzerentů a sítí s informacemi v reálném čase a spolehlivosti, aby bylo zajištěno, že informace v klíčových aplikacích, které budou vždy k dispozici, budou mít vždycky k dispozici"-Jan Corcoran, Ředitelka IT

## <a name="darwin-dimensions"></a>Darwin – dimenze
Při použití široké škály technologií Microsoftu tým v Darwin nastavil pro vytváření a na portálu online avatary, které můžou zákazníci použít k vytváření působivých Lifelike avatary pro použití v hrách, animacích a stránkách sociálních sítí.
S výhodami pro produktivitu Entity Framework a načtením z komponent, jako je programovací model Windows Workflow Foundation (WF) a Windows Server AppFabric (vysoce škálovatelná mezipaměť aplikace v paměti), tým mohl dodat úžasné produkt 35% méně. čas vývoje.
Navzdory tomu, že se členové týmu rozdělují mezi více zemí, tým po procesu agilního vývoje s týdenními verzemi.

 > "Zkusíme, abychom nevytvořili technologii pro účely. Jako spuštění je klíčové, že využíváme technologii, která šetří čas a peníze.
 Rozhraní .NET bylo volbou pro rychlý a nákladově efektivní vývoj. " – Zachary Olsen, architekt  

## <a name="silverware"></a>Silverware
S více než 15 lety zkušeností při vývoji řešení pro malé a středních Restaurace v prodeji je vývojový tým na Silverware nastavený tak, aby vylepšil svůj produkt o více funkcí na podnikové úrovni, aby se daly přilákat větší řetězy restaurací.
Pomocí nejnovější verze vývojářských nástrojů od Microsoftu bylo možné vytvořit nové řešení čtyřikrát rychleji než dřív.
Klíčové nové funkce, jako LINQ a Entity Framework, usnadňují přesun ze sestav Crystal Reports do SQL Server 2008 a SQL Server Reporting Services (SSRS) pro potřeby jejich ukládání a vytváření sestav.

> "Efektivní správa dat je klíč k úspěchu SilverWare – a je to proto, že jsme se rozhodli přijmout SQL Reporting." – Nicholas Romanidis, ředitel oddělení IT/softwaru
