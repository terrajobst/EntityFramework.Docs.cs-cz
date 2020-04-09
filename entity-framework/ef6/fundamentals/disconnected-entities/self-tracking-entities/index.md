---
title: Samosledovací entity - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419523"
---
# <a name="self-tracking-entities"></a>Samosledovací entity

> [!IMPORTANT]
> Už nedoporučujeme používat šablonu self-tracking-entities. Bude i nadále k dispozici pouze pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte jiné alternativy, jako jsou [sledovatelné entity](https://trackableentities.github.io/), což je technologie podobná samoobslužným sledovacím entitám, která je aktivněji vyvinuta komunitou, nebo psaní vlastního kódu pomocí nízkoúrovňových nastavení api pro sledování změn.

V aplikaci založené na rozhraní entity kontext je zodpovědný za sledování změn v objektech. Potom použít SaveChanges metoda zachovat změny v databázi. Při práci s n-vrstvé aplikace, entity objekty jsou obvykle odpojeny od kontextu a musíte se rozhodnout, jak sledovat změny a sestavy těchto změn zpět do kontextu. Samoobslužné sledovací entity vám mohou pomoci sledovat změny v libovolné vrstvě a pak tyto změny přehrát do kontextu, který se má uložit.  

MTE pouze v případě, že kontext není k dispozici na vrstvě, kde jsou provedeny změny grafu objektu. Pokud je k dispozici kontext, není nutné používat stinné služby, protože kontext se postará o sledování změn.  

Tato položka šablony generuje dva soubory TT (textová šablona):  

- Soubor ** \<tt\>modelu** generuje typy entit a pomocnou třídu, která obsahuje logiku sledování změn, která se používá entity samočinného sledování a metody rozšíření, které umožňují nastavení stavu na entitách samočinného sledování.  
- ** \<Název\>modelu . Context.tt** soubor generuje odvozený kontext a třídu rozšíření, která obsahuje metody **ApplyChanges** pro třídy **ObjectContext** a **ObjectSet.** Tyto metody prozkoumat informace o sledování změn, která je obsažena v grafu samosledování entit odvodit sadu operací, které musí být provedeny uložit změny v databázi.  

## <a name="get-started"></a>Začínáme  

Chcete-li začít, navštivte [stránku samosledování entit návod.](walkthrough.md)  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Funkční aspekty při práci s entitami samočinného sledování  
> [!IMPORTANT]
> Už nedoporučujeme používat šablonu self-tracking-entities. Bude i nadále k dispozici pouze pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte jiné alternativy, jako jsou [sledovatelné entity](https://trackableentities.github.io/), což je technologie podobná samoobslužným sledovacím entitám, která je aktivněji vyvinuta komunitou, nebo psaní vlastního kódu pomocí nízkoúrovňových nastavení api pro sledování změn.

Při práci s entitami samočinného sledování zvažte následující:  

- Ujistěte se, že váš klientský projekt obsahuje odkaz na sestavení obsahující typy entit. Pokud přidáte pouze odkaz na službu do klientského projektu, klientský projekt bude používat typy proxy WCF a nikoli skutečné typy vlastních sledovacích entit. To znamená, že nezískáte funkce automatického oznámení, které spravují sledování entit v klientovi. Pokud záměrně nechcete zahrnout typy entit, budete muset ručně nastavit informace o sledování změn na straně klienta pro změny, které mají být odeslány zpět do služby.  
- Volání operace služby by měla být bezstavová a vytvořit novou instanci kontextu objektu. Doporučujeme také vytvořit kontext objektu v **using** bloku.  
- Při odeslání grafu, který byl změněn na straně klienta do služby a pak chcete pokračovat v práci se stejným grafem na straně klienta, je nutné ručně iteraci prostřednictvím grafu a volání **AcceptChanges** metoda na každý objekt obnovit změnu tracker.  

    > Pokud objekty v grafu obsahují vlastnosti s hodnotami generovanými databázemi (například hodnoty identity nebo souběžnosti), entity Framework nahradí hodnoty těchto vlastností hodnotami generovanými databázemi po volání metody **SaveChanges.** Můžete implementovat operaci služby vrátit uložené objekty nebo seznam generovaných hodnot vlastností pro objekty zpět klientovi. Klient by pak musel nahradit instance objektu nebo hodnoty vlastností objektu hodnotami nebo hodnotami vlastností vrácenými z operace služby.  
- Sloučení grafů z více požadavků na služby může zavést objekty s duplicitními hodnotami klíče ve výsledném grafu. Entity Framework neodebere objekty s duplicitními klíči při volání **ApplyChanges** metoda, ale místo toho vyvolá výjimku. Chcete-li se vyhnout grafy s duplicitní hodnoty klíče postupujte podle jednoho ze vzorků popsaných v následujícím blogu: [Self-tracking entity: ApplyChanges a duplicitní entity](https://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409).  
- Změníte-li vztah mezi objekty nastavením vlastnosti cizího klíče, je vlastnost reference navigace nastavena na hodnotu null a není synchronizována s příslušnou hlavní entitou v klientovi. Po grafu je připojen k kontextu objektu (například po volání **ApplyChanges** metoda), vlastnosti cizího klíče a navigační vlastnosti jsou synchronizovány.  

    > Není-li referenční navigační vlastnost synchronizována s příslušným objektem zabezpečení může být problém, pokud jste zadali kaskádové odstranění ve vztahu cizího klíče. Pokud odstraníte objekt zabezpečení, odstranění nebude rozšířeno na závislé objekty. Pokud máte kaskádové odstranění zadané, použijte navigační vlastnosti změnit vztahy namísto nastavení vlastnosti cizího klíče.  
- Samoobslužné sledování entity nejsou povoleny k provádění opožděné načítání.  
- Binární serializace a serializace ASP.NET objekty správy stavu není podporována entity samočinného sledování. Šablonu však můžete přizpůsobit tak, aby byla podpora binární serializace. Další informace naleznete [v tématu Použití binární serializace a ViewState s entity samočinného sledování](https://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Aspekty zabezpečení  

Při práci s entitami samosledování je třeba vzít v úvahu následující aspekty zabezpečení:  

- Služba by neměla důvěřovat požadavkům na načtení nebo aktualizaci dat z nedůvěryhodného klienta nebo prostřednictvím nedůvěryhodného kanálu. Klient musí být ověřen: měl by být použit zabezpečený kanál nebo obálka se zprávou. Požadavky klientů na aktualizaci nebo načtení dat musí být ověřeny, aby bylo zajištěno, že odpovídají očekávaným a legitimním změnám pro daný scénář.  
- Nepoužívejte citlivé informace jako klíče entit (například čísla sociálního pojištění). To zmírňuje možnost neúmyslnéserializace citlivých informací v grafech entity samočinného sledování klientovi, který není plně důvěryhodný. S nezávislými přidruženími může být klientovi odeslán také původní klíč entity, který souvisí s entitou, která je serializována.  
- Chcete-li se vyhnout šíření zpráv výjimek, které obsahují citlivá data na úroveň klienta, volání **ApplyChanges** a **SaveChanges** na úrovni serveru by měla být zabalena v kódu zpracování výjimek.  
