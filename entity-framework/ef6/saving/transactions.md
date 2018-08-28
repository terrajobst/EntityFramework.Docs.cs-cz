---
title: Práce s transakcí - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 20b63c88c41c10b5a69660d5027097c647c7eedd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997548"
---
# <a name="working-with-transactions"></a>Práce s transakcí
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Tento dokument popisuje použití transakcí v EF6, včetně vylepšení, které jsme přidali od EF5 usnadňují práci s transakcí.  

## <a name="what-ef-does-by-default"></a>EF nemá ve výchozím nastavení  

Ve všech verzích rozhraní Entity Framework při každém spuštění **SaveChanges()** vložit, aktualizovat nebo odstranit v databázi rozhraní framework, zabalit tuto operaci v transakci. Tato transakce trvá pouze na dobu nutnou k provedení operace a pak je dokončí. Při spouštění jiná taková operace se spustí novou transakci.  

Počínaje EF6 **Database.ExecuteSqlCommand()** ve výchozím nastavení se zabalení příkazu v transakci Pokud jeden není již k dispozici. Existují přetížení této metody, které umožňují toto chování přepsat, pokud chcete. Také v EF6 provádění uložené procedury, které jsou součástí modelu prostřednictvím rozhraní API, jako **ObjectContext.ExecuteFunction()** dělá to samé (s tím rozdílem, že výchozí chování nelze v tuto chvíli přepsat).  

V obou případech je úroveň izolace transakce libovolnou úroveň izolace poskytovatele databáze bude považovat za jeho výchozí nastavení. Ve výchozím nastavení například na SQL serveru jde READ COMMITTED.  

Entity Framework nezalamuje dotazy v rámci transakce.  

Toto výchozí chování je vhodná pro velké množství uživatelů a pokud to není nutné udělat nic jiného v EF6; stejně jako vždy stačí napište požadovaný kód.  

Ale někteří uživatelé vyžadují větší kontrolu nad jejich transakcí – tento proces je popsán v následujících částech.  

## <a name="how-the-apis-work"></a>Jak fungují rozhraní API  

Před EF6 Entity Framework insisted při otevření připojení k databázi samotného (došlo k výjimce byl předán připojení, které už je otevřený). Protože transakce lze spustit pouze na otevření připojení, to znamená, že jediný způsob, jak uživatel zabalit několik operací do jedné transakce se má používat [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) nebo použijte  **ObjectContext.Connection** vlastnosti a volání start **Open()** a **BeginTransaction()** přímo na vrácený **EntityConnection** objekt. Kromě toho volání rozhraní API, které kontaktovat databáze by selhat, pokud transakce byl spuštěn na základní připojení k databázi sami.  

> [!NOTE]
> Omezení pouze přijímat připojení uzavřené byla odebrána v Entity Framework 6. Podrobnosti najdete v tématu [správu připojení](~/ef6/fundamentals/connection-management.md).  

Počínaje EF6 rozhraní framework teď poskytuje:  

1. **Database.BeginTransaction()** : jednodušší způsob pro uživatele ke spuštění a dokončení transakcí sami v rámci existující DbContext – povolení několika operací a nelze jej zkombinovat v rámci jedné transakce a proto všechny potvrzené nebo všechny Vrátí zpět jako jeden. Také umožňuje uživateli snadněji určit úroveň izolace transakce.  
2. **Database.UseTransaction()** : umožňuje používat transakce, která byla spuštěna mimo rozhraní Entity Framework uvolněn objekt DbContext.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Kombinování několik operací do jedné transakce v rámci stejného kontextu  

**Database.BeginTransaction()** má dva přepisy – znak, který se má explicitní [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) a druhou, která nepřijímá žádné argumenty a používá výchozí IsolationLevel z podkladového zprostředkovatele databáze. Vrátí obě přepsání **DbContextTransaction** objekt, který poskytuje **Commit()** a **Rollback()** metody, které provádějí potvrzení změn a vrácení zpět na příslušné úložiště transakce.  

**DbContextTransaction** má být uvolněn, jakmile byla potvrzena nebo vrácena zpět. Snadný způsob, jak dosáhnout jednoho se **using(...) {...}** syntaxe, která automaticky zavolá **Dispose()** při using blokovat dokončení:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> Od transakce vyžaduje, že základní úložiště připojení je otevřeno. Proto volání Database.BeginTransaction() otevře připojení, pokud již není otevřen. Pokud DbContextTransaction otevřít připojení pak dojde k uzavření ho při volání Dispose().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Předání kontextu existující transakce  

Někdy chcete transakce, které se ještě zvětšuje, v oboru a která zahrnuje operace ve stejné databázi, ale mimo EF úplně. K tomu musíte otevřít připojení a spuštění transakce a pak dali pokyn EF a) pro použití připojení k databázi už otevřený a (b) můžete využít existující transakce na toto připojení.  

K tomu musíte definovat a použijte konstruktor na vaší třídy kontextu, který dědí z jednoho z konstruktorů DbContext, což trvat i) existující parametr připojení a ii) na contextOwnsConnection logická.  

> [!NOTE]
> Příznak contextOwnsConnection musí být nastavena na hodnotu NEPRAVDA, pokud je volána v tomto scénáři. Je to důležité proto informuje Entity Framework, že se po dokončení se s ním neměli zavírat připojení (například. Viz řádek 4 níže):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Kromě toho musí začínat transakce sami (včetně IsolationLevel, pokud chcete, aby se zabránilo výchozí nastavení) a umožní Entity Framework, že je již spuštěna na připojení k existující transakce (viz řádek 33 níže).  

Potom můžete libovolně u provádění operací databáze přímo na objekt SqlConnection samotné nebo objekt dbcontext. Všechny tyto operace jsou spuštěny v rámci jedné transakce. Můžete převzít odpovědnost pro potvrzení nebo vrácení transakce a volání Dispose() na ni, stejně jako pro zavření a rušení připojení databáze. Příklad:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a>Odstraňování transakce

Můžete předat hodnotu null Database.UseTransaction() zrušte Entity Framework znalost aktuální transakce. Entity Framework bude ani potvrzení ani vrácení stávajících transakcí při tomto, proto používejte opatrně a pouze v případě, že jste si jisti, to je, co chcete udělat.  

### <a name="errors-in-usetransaction"></a>Chyby v transakci

Zobrazí se výjimka z Database.UseTransaction() Pokud předáte transakce při:  
- Entity Framework již má existující transakce  
- Entity Framework je již zpracovávána v rámci objekt TransactionScope  
- V transakci předaný objekt připojení má hodnotu null. To znamená, že transakce není přidružena připojení – to je obvykle známkou toho, že této transakce již byla dokončena  
- Objekt připojení v předán transakce neodpovídá připojení rozhraní Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Použití transakcí s dalšími funkcemi  

Tato část podrobně popisuje, jak výše uvedené transakce interakci s:  

- Odolnost připojení  
- Asynchronní metody  
- Transakce TransactionScope  

### <a name="connection-resiliency"></a>Odolnost připojení  

Novou funkci odolnosti proti chybám připojení nefunguje s uživatelem iniciované transakce. Podrobnosti najdete v tématu [omezení strategie opakování pokusu o spuštění](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).  

### <a name="asynchronous-programming"></a>Asynchronní programování  

Přístup uvedených v předchozích částech potřebuje žádné další možnosti nebo nastavení pro práci s [asynchronního dotazu a uložit metody](~/ef6/fundamentals/async.md
). Ale mějte na paměti, že v závislosti na tom, co dělat v rámci asynchronní metody může být výsledkem dlouhotrvajících transakcí – které pak může způsobit zablokování nebo blokování, což je vhodná pro výkon celkové aplikace.  

### <a name="transactionscope-transactions"></a>Transakce TransactionScope  

Před EF6 doporučený způsob poskytnutí větší rozsah transakce byla použití objektu TransactionScope:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

Připojení SqlConnection a Entity Framework jak použije okolí transakce TransactionScope a proto se měly potvrdit společně.  

Od verze rozhraní .NET 4.5.1 objekt TransactionScope byl aktualizován na také pracovat prostřednictvím použití asynchronních metod [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) výčtu:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

Stále existují určitá omezení přístupu TransactionScope:  

- Vyžaduje .NET 4.5.1 nebo novější pro práci s asynchronní metody.  
- Ve scénářích cloudové jej nelze použít, pokud jste si jisti, budete mít jeden a pouze jeden připojení (cloudové scénáře nepodporují distribuované transakce).  
- Ji nelze kombinovat s přístupem Database.UseTransaction() z předchozí části.  
- Výjimky vyvolá-li vydávat žádné DDL a nepovolili distribuované transakce ve službě MSDTC.  

Výhody TransactionScope přístupu:  

- Se automaticky upgraduje místní transakce na distribuovanou transakci Pokud provedete víc než jedno připojení k dané databázi nebo připojení k jedné databáze v kombinaci s připojení k jiné databázi v rámci jedné transakce (Poznámka: je zapotřebí Služba MSDTC službu nakonfigurované tak, aby distribuovaných transakcí, aby to fungovalo).  
- Usnadnění psaní kódu. Pokud dáváte přednost transakci okolí a zpracovávány implicitně na pozadí, spíše než explicitně v části ovládací prvek pak TransactionScope přístup může vyhovovat je lepší.  

Stručně řečeno, nové Database.BeginTransaction() a rozhraní API Database.UseTransaction() výše přístup objekt TransactionScope byl již není nezbytné pro většinu uživatelů. Pokud budete nadále používat TransactionScope pak mějte na paměti z výše uvedených omezení. Doporučujeme použít přístup popsaných v předchozích částech místo, kde je to možné.  
