---
title: Entity pro sledování sebe – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419523"
---
# <a name="self-tracking-entities"></a>Entity pro sledování sebe

> [!IMPORTANT]
> Nedoporučujeme používat šablonu samoobslužné sledování – entity. Bude dál k dispozici jenom pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte další alternativy, jako jsou například [sledované entity](https://trackableentities.github.io/), což je technologie podobná entitám s vlastním sledováním, které jsou aktivně vyvíjené komunitou, nebo psaním vlastního kódu s využitím rozhraní API pro sledování změn nízké úrovně.

V aplikaci založené na Entity Framework je kontext zodpovědný za sledování změn v objektech. Pak použijte metodu SaveChanges k uchování změn v databázi. Při práci s N-vrstvými aplikacemi jsou objekty entit obvykle odpojeny od kontextu a je nutné se rozhodnout, jak sledovat změny a ohlásit tyto změny zpět do kontextu. Entity s možností sledování (ste) vám mohou pomáhat sledovat změny v libovolné vrstvě a následně tyto změny přehrát do kontextu, který má být uložen.  

Ste použijte pouze v případě, že kontext není k dispozici na úrovni, kde jsou provedeny změny v grafu objektu. Pokud je kontext k dispozici, není nutné používat ste, protože kontext se bude postarat o sledování změn.  

Tato položka šablony generuje dvě soubory. TT (textová šablona):  

- **Název\<modelu\>soubor. TT** vygeneruje typy entit a pomocnou třídu, která obsahuje logiku sledování změn, která je používána entitami s trasováním, a metodami rozšíření, které umožňují stav nastavení na entitách pro sledování samy sebe.  
- **Název\<ho modelu\>. Soubor Context.tt** vygeneruje odvozený kontext a třídu rozšíření, která obsahuje metody **ApplyChanges –** pro třídy **ObjectContext** a **ObjectSet** . Tyto metody prozkoumají informace o sledování změn, které jsou obsaženy v grafu entit s trasováním, a odvodit tak sadu operací, které je nutné provést k uložení změn v databázi.  

## <a name="get-started"></a>Začínáme  

Začněte tím, že přejdete na stránku s [návodem k samoobslužným entitám](walkthrough.md) .  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Funkční hlediska při práci s entitami pro sledování sebe  
> [!IMPORTANT]
> Nedoporučujeme používat šablonu samoobslužné sledování – entity. Bude dál k dispozici jenom pro podporu stávajících aplikací. Pokud vaše aplikace vyžaduje práci s odpojenými grafy entit, zvažte další alternativy, jako jsou například [sledované entity](https://trackableentities.github.io/), což je technologie podobná entitám s vlastním sledováním, které jsou aktivně vyvíjené komunitou, nebo psaním vlastního kódu s využitím rozhraní API pro sledování změn nízké úrovně.

Při práci s entitami pro sledování samy zvažte následující:  

- Ujistěte se, že váš klientský projekt obsahuje odkaz na sestavení obsahující typy entit. Pokud přidáte do klientského projektu pouze odkaz na službu, projekt klienta bude používat typy proxy serverů WCF a nikoli skutečné typy entit s vlastním sledováním. To znamená, že nebudete dostávat funkce automatizovaného oznamování, které spravují sledování entit na klientovi. Pokud záměrně nechcete zahrnout typy entit, budete muset ručně nastavit informace o sledování změn v klientovi, aby se změny poslaly zpátky do služby.  
- Volání operace služby by měla být Bezstavová a vytvoří novou instanci kontextu objektu. Doporučujeme také vytvořit kontext objektu v bloku **using** .  
- Když odešlete graf, který byl změněn v klientovi, do služby a pak v úmyslu pokračovat v práci se stejným grafem na klientovi, je nutné ručně iterovat v grafu a volat metodu **AcceptChanges** u každého objektu pro resetování sledování změn.  

    > Pokud objekty v grafu obsahují vlastnosti s hodnotami generovanými databází (například s hodnotami identity nebo souběžnosti), Entity Framework nahradí hodnoty těchto vlastností hodnotami generovanými databází po volání metody **SaveChanges** . Můžete implementovat operaci služby pro vrácení uložených objektů nebo seznam vygenerovaných hodnot vlastností pro objekty zpátky do klienta. Klient by pak musel nahradit hodnoty instancí objektů nebo vlastností objektu hodnotami objektů nebo vlastností vrácenými z operace služby.  
- Sloučení grafů z více žádostí o služby může do výsledného grafu zavést objekty s duplicitními hodnotami klíčů. Entity Framework neodebere objekty s duplicitními klíči při volání metody **ApplyChanges –** , ale místo toho vyvolá výjimku. Abyste se vyhnuli zobrazování grafů s duplicitními hodnotami klíčů, postupujte podle jednoho ze vzorů popsaných v následujícím blogu: [entity pro sledování sebe: ApplyChanges – a duplicitní entity](https://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409).  
- Když změníte relaci mezi objekty nastavením vlastnosti cizího klíče, navigační vlastnost odkazu je nastavena na hodnotu null a není synchronizována s příslušnou objektovou entitou v klientovi. Po připojení grafu ke kontextu objektu (například po volání metody **ApplyChanges –** ) jsou synchronizovány vlastnosti cizího klíče a navigační vlastnosti.  

    > Pokud jste u vztahu cizího klíče zadali kaskádové odstranění, nemusíte mít synchronizovaný navigační vlastnost s příslušným objektem zabezpečení. Odstraníte-li objekt zabezpečení, nebude tato akce rozšířena do závislých objektů. Pokud jste zadali kaskádové odstranění, použijte navigační vlastnosti pro změnu relací místo nastavení vlastnosti cizího klíče.  
- Entity pro sledování samy nejsou povoleny pro provádění opožděného načítání.  
- Binární serializace a serializace do objektů správy stavu ASP.NET není podporována entitami pro sledování sebe. Můžete však přizpůsobit šablonu, aby bylo možné přidat podporu binární serializace. Další informace najdete v tématu [použití binární serializace a vlastnosti ViewState s entitami, které se sledují sami](https://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Aspekty zabezpečení  

Při práci s entitami pro sledování samy by se měly vzít v úvahu následující bezpečnostní opatření:  

- Služba by neměla důvěřovat požadavkům na načtení nebo aktualizaci dat z nedůvěryhodného klienta nebo prostřednictvím nedůvěryhodného kanálu. Klient musí být ověřen: je třeba použít zabezpečený kanál nebo obálku zprávy. Žádosti klientů o aktualizaci nebo načtení dat musí být ověřené, aby bylo zajištěno, že budou splňovat očekávané a legitimní změny v daném scénáři.  
- Vyhněte se použití citlivých informací jako klíčů entit (například čísel sociálního zabezpečení). To snižuje možnost neúmyslného serializace citlivých informací v grafech entit s možností sledování samy na klienta, který není plně důvěryhodný. U nezávislých přidružení může klient také odeslat původní klíč entity, která souvisí se serializovanou serializací.  
- Aby nedošlo k šíření výjimek, které obsahují citlivá data na vrstvu klienta, volání **ApplyChanges –** a **SaveChanges** na úrovni serveru by mělo být zabaleno do kódu zpracování výjimek.  
