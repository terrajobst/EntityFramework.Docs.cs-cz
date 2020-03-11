---
title: Zpracování chyb potvrzení transakce – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417368"
---
# <a name="handling-transaction-commit-failures"></a>Zpracování chyb potvrzení transakce
> [!NOTE]
> **EF 6.1 a vyšší pouze** – funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6,1. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

V rámci 6,1 jsme zavedli novou funkci odolnosti připojení pro EF: schopnost detekovat a obnovovat automaticky v případě, že při přechodném selhání připojení dojde k ovlivnění potvrzení transakcí. Úplné podrobnosti o tomto scénáři se nejlépe popisují v příspěvku blogu [SQL Database možnosti připojení a problému idempotence](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Ve shrnutí je scénář, že když je vyvolána výjimka během potvrzení transakce, existují dvě možné příčiny:  

1. Potvrzení transakce na serveru selhalo.
2. Potvrzení transakce na serveru bylo úspěšné, ale potíže s připojením zabránily klientovi v navázání oznámení o úspěšnosti.  

Když se první situace stane aplikací nebo uživatel může operaci zopakovat, ale když dojde k druhé situaci, je třeba se vyhnout opakování a aplikace by se mohla automaticky obnovit. Důvodem je to, že bez možnosti rozpoznat, co byl skutečným důvodem, že se během potvrzení nahlásila výjimka, aplikace nemůže zvolit správný průběh akce. Nová funkce v EF 6,1 umožňuje EF pořídit databázi v případě úspěchu transakce a provedení správného postupu transparentního postupu.  

## <a name="using-the-feature"></a>Používání funkce  

Aby bylo možné povolit funkci, kterou potřebujete, uveďte volání [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) v konstruktoru svého **DbConfiguration**. Pokud neznáte **DbConfiguration**, přečtěte si téma [Konfigurace na základě kódu](~/ef6/fundamentals/configuring/code-based.md). Tato funkce se dá použít v kombinaci s automatickými pokusy, které jsme zavedli v EF6, což vám pomůže v situaci, kdy se transakci ve skutečnosti nepodařilo zapsat na serveru z důvodu přechodného selhání:  

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

## <a name="how-transactions-are-tracked"></a>Jak jsou sledovány transakce  

Když je funkce povolená, EF automaticky přidá novou tabulku do databáze s názvem **__Transactions**. V této tabulce je vložen nový řádek pokaždé, když je transakce vytvořena pomocí EF a tento řádek je kontrolován pro existenci, pokud dojde k selhání transakce během zápisu.  

I když EF dovede k vyřazení řádků z tabulky, když už nejsou potřeba, může tabulka růst, jestli se aplikace ukončí předčasně a z tohoto důvodu možná budete muset tabulku v některých případech vyprázdnit ručně.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Postup zpracování selhání potvrzení s předchozími verzemi

Před EF 6,1 neexistuje mechanismus pro zpracování selhání potvrzení v produktu EF. Existuje několik způsobů, jak v této situaci řešit, které je možné použít pro předchozí verze EF6:  

* Možnost 1 – nedělat nic  

  Pravděpodobnost selhání připojení během potvrzení transakce je nízká, takže může být přijatelné, aby vaše aplikace v případě, že k této situaci skutečně dojde, nedošlo k chybě.  

* Možnost 2 – obnovení stavu pomocí databáze  

  1. Zahodit aktuální DbContext  
  2. Vytvořte novou DbContext a obnovte stav aplikace z databáze.  
  3. Informujte uživatele, že poslední operace nemusí být úspěšně dokončená.  

* Možnost 3 – ruční sledování transakce  

  1. Přidejte nesledovanou tabulku do databáze použité ke sledování stavu transakcí.  
  2. Vloží řádek do tabulky na začátku každé transakce.  
  3. Pokud během potvrzení dojde k chybě připojení, vyhledejte přítomnost odpovídajícího řádku v databázi.  
     - Pokud je řádek k dispozici, pokračujte normálně, protože transakce byla úspěšně potvrzena.  
     - Pokud řádek chybí, použijte k opakování aktuální operace strategii spouštění.  
  4. Pokud je potvrzení úspěšné, odstraňte odpovídající řádek, abyste se vyhnuli nárůstu tabulky.  

[Tento Blogový příspěvek](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) obsahuje vzorový kód pro dosažení tohoto SQL Azure.  
