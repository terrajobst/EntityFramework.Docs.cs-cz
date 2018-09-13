---
title: Vlastní sledování entity - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3575977ceabe7d93ac48d5fac253eac1341e2353
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489697"
---
# <a name="self-tracking-entities"></a>Vlastní sledování entit

> [!IMPORTANT]
> Už doporučujeme použít šablonu samoobslužného tracking entity. Pouze bude nadále být k dispozici pro podporu stávající aplikace. Pokud aplikace potřebuje pracovat s grafy odpojených entit, zvažte další možnosti, jako [organizovaným entity](http://trackableentities.github.io/), což je technologie, podobně jako u samoobslužného-Tracking-entit, které je více aktivně vyvíjen Komunita, nebo psaní vlastního kódu pomocí nízké úrovně rozhraní API pro sledování změn.

V aplikaci založené na rozhraní Entity Framework je zodpovědná za sledování změn ve vašich objektů kontextu. Pak použijete metodu SaveChanges a zachová tak změny do databáze. Při práci s N-vrstvé aplikace, jsou obvykle objekty entity odpojen od kontextu a musíte rozhodnout, jak sledovat změny a sestavy tyto změny zpět do kontextu. Vlastní sledování entity (STE) vám může pomoct ve všech úrovních sledovat změny a pak znovu přehrát tyto změny do kontextu, který se má uložit.  

Použijte STE jenom v případě, že kontextu není k dispozici na na úrovni kde dojde ke změně do grafu objektu. Pokud kontext je k dispozici, není nutné používat STE, protože kontext se postará o sledování změn.  

Tato položka šablony vytvoří dva soubory .tt (textové šablony):  

- **\<Název modelu\>.tt** generuje soubor typy entit a pomocná třída, která obsahuje logiku sledování změn, která používá vlastní sledování entit a metody rozšíření, které umožní nastavit stav na místním sledování entity.  
- **\<Název modelu\>. Context.tt** generuje soubor odvozené kontextu a rozšíření třídy, která obsahuje **applychanges –** metody **ObjectContext** a **ObjectSet** třídy. Tyto metody prozkoumejte informace sledování změn, které je obsažen v grafu svým sledování entity k odvození sadu operací, které je nutné provést k uložení změn v databázi.  

## <a name="get-started"></a>Začínáme  

Abyste mohli začít, navštivte [Self-Tracking entity návod](walkthrough.md) stránky.  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Funkční aspekty při práci se svým sledováním entity  
> [!IMPORTANT]
> Už doporučujeme použít šablonu samoobslužného tracking entity. Pouze bude nadále být k dispozici pro podporu stávající aplikace. Pokud aplikace potřebuje pracovat s grafy odpojených entit, zvažte další možnosti, jako [organizovaným entity](http://trackableentities.github.io/), což je technologie, podobně jako u samoobslužného-Tracking-entit, které je více aktivně vyvíjen Komunita, nebo psaní vlastního kódu pomocí nízké úrovně rozhraní API pro sledování změn.

Při práci se službou samoobslužné sledování entit, zvažte následující:  

- Ujistěte se, že klientský projekt obsahuje odkaz na sestavení obsahující typy entit. Pokud chcete přidat pouze odkaz na službu do projektu klienta, klientský projekt bude používat typ proxy WCF a ne skutečné svým sledování typy entit. To znamená, že nezískáte funkce automatizované oznámení, které spravují sledování entity na straně klienta. Pokud úmyslně nechcete zahrnout typy entit, budete muset ručně nastavit informace o sledování změn na straně klienta pro změny k odeslání zpět do služby.  
- Volání operace služby by měly být bezstavové a vytvořte novou instanci objektu kontextu. Doporučujeme také, že vytvoříte objekt kontextu v **pomocí** bloku.  
- Při odesílání grafu, který byl upraven na straně klienta ke službě a potom v úmyslu pokračovat v práci s do stejného grafu na straně klienta, budete muset ručně iteraci v rámci grafu a volání **metoda AcceptChanges** metodu pro každý objekt Resetujte sledování změn.  

    > Pokud objekty v grafu obsahují vlastnosti databáze vygenerovala hodnotami (například identity nebo souběžnosti hodnot), Entity Framework nahradí hodnoty těchto vlastností s hodnotami databáze vygenerovala po **SaveChanges** metoda je volána. Můžete implementovat vaše operace služby vrátit uložené objekty nebo seznam hodnot vygenerované vlastnosti objektů zpět do klienta. Klient potom by bylo potřeba nahradit instancí objektů nebo hodnot vlastností objektu objekty nebo hodnoty vlastností vrácená z operace služby.  
- Sloučení grafů z více žádostí o službu může představovat objektů s duplicitními hodnotami klíče ve výsledném grafu. Entity Framework neodebere objekty s duplicitní klíče, při volání **applychanges –** metody, ale místo toho dojde k výjimce. Abyste se vyhnuli nutnosti grafy s duplicitní hodnoty klíče odpovídat jednomu ze vzorů je popsáno v následujícím blogu: [Self-Tracking entity: applychanges – a duplicitní entity](http://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409).  
- Při změně vztahů mezi objekty tak, že nastavíte vlastnost cizího klíče je odkaz navigační vlastnost nastavena na hodnotu null a není synchronizovaná na příslušnou entitu instančního objektu na straně klienta. Po grafu je připojit ke kontextu objektu (například po volání **applychanges –** metoda), vlastnosti cizího klíče a vlastnosti navigace se synchronizují.  

    > Nemá odkaz na vlastnost navigace synchronizované s odpovídající objekt zabezpečení můžou být problémy, pokud jste zadali kaskádové odstranění na relace cizího klíče. Při odstranění objektu zabezpečení, odstranění se nerozšíří do závislé objekty. Pokud máte kaskádové odstranění zadané pomocí navigačních vlastností můžete změnit vztahy namísto nastavování vlastností cizího klíče.  
- K provádění opožděné načtení nejsou povolené samoobslužné sledování entity.  
- Binární serializace a serializace za účelem objekty stavu správy technologie ASP.NET není podporován místním sledování entity. Můžete však přizpůsobit šablonu k přidání podpory pro binární serializace. Další informace najdete v tématu [pomocí binární serializace a stav zobrazení s entitami Self-Tracking](http://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Důležité informace o zabezpečení  

Při práci se službou samoobslužné sledování entity musí vzít v úvahu následující aspekty zabezpečení:  

- Žádosti o získání nebo aktualizaci dat z nedůvěryhodné klienta nebo prostřednictvím kanálu nedůvěryhodné by neměl s důvěrou používají služby. Klient musí být ověřené: zabezpečeného kanálu nebo zprávy obálky by měla sloužit. Požadavky klientů na aktualizaci nebo načtení dat musí být ověřené na Ujistěte se, že jsou v souladu s očekávaným způsobem a je legitimní změny této situaci.  
- Vyhněte se použití citlivých informací jako klíče entity (například čísla sociálního pojištění). To snižuje možnost neúmyslně serializace citlivých informací v místním sledování grafů entity klientovi, který není plně důvěryhodné. Původní klíč entity, která souvisí s tím, který je serializována nezávislé přidružení, může být odesláno také klienta.  
- Aby se zabránilo šíření zprávy o výjimkách, které obsahují citlivá data do vrstva klienta volání **applychanges –** a **SaveChanges** na serveru vrstvy by měl být uzavřen ve zpracování výjimek kódu.  
