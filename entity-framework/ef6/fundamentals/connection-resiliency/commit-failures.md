---
title: Zpracování selhání potvrzení transakce - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: f912777104c2e925122c05046d4d65660de8b8a8
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250853"
---
# <a name="handling-transaction-commit-failures"></a>Zpracování selhání potvrzení transakce
> [!NOTE]
> **EF6.1 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.1. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Jako součást 6.1 Představujeme novou funkci odolnosti proti chybám připojení pro EF: schopnost detekovat a automatického obnovení po selhání přechodné připojení ovlivnit potvrzení potvrzení transakcí. Všechny podrobnosti scénáře jsou nejlépe popisuje tento blogový příspěvek [připojení k databázi SQL a problém idempotence](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Stručně řečeno tento scénář je, že když je výjimka vyvolána během zápisu transakce, existují dva možné příčiny:  

1. Potvrzení transakce nejde na serveru
2. Potvrzení transakce byla úspěšně dokončena na serveru, ale problém s připojením zabránila oznámením o úspěšné dosažení klienta  

Při první situace nastane u uživatele nebo aplikace můžete operaci opakovat, ale pokud druhá situace nastane, mělo by se vyhnout opakovaných pokusů a aplikace může automaticky obnovit. Na výzvu je, že kdybychom neměli možnost zjistit, co byl skutečné důvod nahlášení výjimky během zápisu, aplikace nejde zvolit, ta správná cesta akce. Nová funkce v EF 6.1 umožňuje EF zkontrolujte s databází, pokud transakce byla úspěšná a transparentně trvat ta správná cesta akce.  

## <a name="using-the-feature"></a>Pomocí funkce  

Chcete-li povolit tuto funkci musí obsahovat volání [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) v konstruktoru vaše **DbConfiguration**. Pokud nejste obeznámeni s **DbConfiguration**, naleznete v tématu [kódu na základě konfigurace](~/ef6/fundamentals/configuring/code-based.md). Tuto funkci můžete použít v kombinaci s Automatické opakované pokusy, kterou jsme představili v EF6, které pomáhají v situaci, ve kterém ve skutečnosti zápis transakce se nezdařil na serveru, protože došlo k přechodné chybě:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>Způsob sledování transakce  

Pokud je povolena funkce, EF automaticky přidá novou tabulku pro databázi s názvem **__Transactions**. V této tabulce je vložen nový řádek, pokaždé, když transakce je vytvořen pomocí EF a tento řádek je zaškrtnuté políčko existenci, pokud během zápisu dojde k selhání transakce.  

I když EF provede nezaručené vyřadit řádky z tabulky, když už nejsou potřeba, můžou růst v tabulce, pokud se aplikace ukončí předčasně a z tohoto důvodu možná bude nutné k vyprázdnění tabulek ručně v některých případech.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Jak řešit selhání potvrzení s předchozími verzemi

Před EF 6.1 není mechanismem pro zpracování selhání potvrzení do EF produktu. K práci s touto situací, který lze použít k předchozím verzím EF6 několika způsoby:  

* Možnost 1 - nedělat nic  

  Pravděpodobnost selhání připojení během zápisu transakce tak může být přijatelný pro vaše aplikace právě selhat, pokud dojde k tomuto stavu dochází.  

* Možnost 2 – databázi můžete obnovit stav  

  1. Zrušit aktuální kontext databáze.  
  2. Vytvořit nový kontext databáze a obnovení stavu aplikace z databáze  
  3. Informujte uživatele, že poslední operaci možná nebyly úspěšně dokončeny  

* Možnost 3 - manuálně sledovat transakce  

  1. Přidáte tabulku – sledovat na databáze, která slouží ke sledování stavu transakce.  
  2. Vložte řádek do tabulky na začátku každé transakci.  
  3. Pokud se nepovede při potvrzení změn, zkontrolujte přítomnost odpovídající řádek v databázi.  
     - Pokud se nachází na řádku, bude normálně pokračujte, protože transakce byla úspěšně zapsána  
     - Pokud chybí na řádku, použijte strategie provádění aktuální operaci zopakovat.  
  4. Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, aby se zabránilo růst v tabulce.  

[Tento příspěvek na blogu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) obsahuje ukázkový kód pro provedení to v SQL Azure.  
