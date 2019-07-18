---
title: Práce s transakcemi – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306527"
---
# <a name="working-with-transactions"></a>Práce s transakcemi
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

Tento dokument popisuje použití transakcí v EF6, včetně vylepšení, která jsme přidali od EF5, aby bylo snadné pracovat s transakcemi.  

## <a name="what-ef-does-by-default"></a>Jaké má EF ve výchozím nastavení  

Ve všech verzích Entity Framework, kdykoli spustíte **SaveChanges ()** pro vložení, aktualizaci nebo odstranění databáze, rozhraní zalomí tuto operaci v transakci. Tato transakce trvá pouze dostatečně dlouhou dobu pro provedení operace a poté dokončí. Při provádění jiné takové operace se spustí nová transakce.  

Počínaje příkazem EF6 **Database. ExecuteSqlCommand ()** se ve výchozím nastavení zabalí příkaz do transakce, pokud ještě nikdo neexistoval. Existují přetížení této metody, které vám umožní toto chování přepsat, pokud chcete. Také v EF6 provádění uložených procedur obsažených v modelu prostřednictvím rozhraní API, jako je **ObjectContext. ExecuteFunction ()** , je stejné (s tím rozdílem, že výchozí chování nelze v okamžiku přepsat).  

V obou případech je úroveň izolace transakce libovolnou úrovní izolace, kterou poskytovatel databáze považuje za výchozí nastavení. Ve výchozím nastavení je to například u SQL Server se jedná o POTVRZENé čtení.  

Entity Framework nebalí dotazy v transakci.  

Tato výchozí funkce je vhodná pro velké množství uživatelů, a pokud není potřeba nic dělat jinak v EF6; Stačí napsat kód tak, jak jste to trvali.  

Někteří uživatelé však vyžadují větší kontrolu nad svými transakcemi – to je popsáno v následujících částech.  

## <a name="how-the-apis-work"></a>Jak fungují rozhraní API  

Před EF6 Entity Framework nastaly při otevírání samotného databázového připojení (vyvolalo výjimku, pokud bylo předáno připojení, které již bylo otevřeno). Vzhledem k tomu, že transakci lze spustit pouze v otevřeném připojení, což znamená, že jediný způsob, jakým může uživatel zabalit několik operací do jedné transakce, byl buď použit jako [objekt TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) , nebo použít vlastnost **ObjectContext. Connection** a spustit volání metody **Open ()** a **BeginTransaction ()** přímo na vráceném objektu **EntityConnection** . Kromě toho volání rozhraní API, která kontaktovala databázi, by nebylo úspěšné, pokud jste spustili transakci na podkladovém připojení databáze sami.  

> [!NOTE]
> Omezení pouze přijetí uzavřených připojení bylo v Entity Framework 6 odebráno. Podrobnosti najdete v tématu [Správa připojení](~/ef6/fundamentals/connection-management.md).  

Počínaje EF6 Framework nyní poskytuje:  

1. **Database.BeginTransaction()** : Jednodušší způsob, jak uživatel spustit a dokončit transakce samotné v rámci existující DbContext – umožňuje kombinovat několik operací v rámci stejné transakce a tedy všechny potvrzené nebo všechny provedené zpátky jako jeden. Umožňuje také uživateli snadněji zadat úroveň izolace transakce.  
2. **Database. UseTransaction ()** : umožňuje DbContext použít transakci, která byla spuštěna mimo Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Kombinování několika operací do jedné transakce v rámci stejného kontextu  

**Database. BeginTransaction ()** má dvě přepsání – jedna, která přijímá explicitní [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) a jednu, která nepřijímá žádné argumenty a používá výchozí IsolationLevel z podkladového poskytovatele databáze. Obě přepsání vrátí objekt **DbContextTransaction** , který poskytuje metody **potvrzení ()** a **vrácení zpět ()** , které provádějí potvrzení a vrácení zpět v podkladové transakci úložiště.  

**DbContextTransaction** má být uvolněna, jakmile bude potvrzena nebo vrácena zpět. Jedním ze způsobů, jak toho dosáhnout, je **použití (...). {...}** syntaxe, která bude automaticky volat **Dispose ()** při dokončení bloku using:  

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
            }
        }
    }
}
```  

> [!NOTE]
> Zahájení transakce vyžaduje otevření základního připojení úložiště. Proto voláním Database. BeginTransaction () otevře připojení, pokud ještě není otevřené. Pokud DbContextTransaction připojení otevřelo, zavře se, až se zavolá Dispose ().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Předání existující transakce do kontextu  

Někdy byste chtěli být transakce, která je ještě širší v oboru a která zahrnuje operace ve stejné databázi, ale mimo EF zcela. Aby to bylo možné, musíte otevřít připojení a spustit transakci sami, sdělit EF a) pro použití již otevřeného připojení k databázi a b) pro použití existující transakce na tomto připojení.  

Chcete-li to provést, je nutné definovat a použít konstruktor pro třídu Context, která dědí z jednoho z konstruktorů DbContext, který přebírá i stávající parametr připojení a II) contextOwnsConnectionou logickou hodnotu.  

> [!NOTE]
> Při volání v tomto scénáři musí být příznak contextOwnsConnection nastaven na hodnotu false. To je důležité, protože informují Entity Framework, že by nezavřeli připojení, když s ním bude provedeno (například na řádku 4 níže):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Kromě toho je nutné spustit transakci sami sami (včetně IsolationLevel, pokud chcete zabránit výchozímu nastavení) a nechat Entity Framework, že již existuje existující transakce, která již začala na připojení (viz řádek 33 níže).  

Pak můžete spouštět databázové operace buď přímo na samotném SqlConnection, nebo na DbContext. Všechny tyto operace jsou spouštěny v rámci jedné transakce. Přinesete zodpovědnost za potvrzení nebo vrácení transakce a pro volání metody Dispose () na ni a také pro uzavření a zrušení připojení k databázi. Příklad:  

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
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
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
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a>Vymazávání transakce

Můžete předat hodnotu null objektu Database. UseTransaction (), aby bylo jasné, že se Entity Framework aktuální transakce. Pokud to uděláte, Entity Framework nebude ani po provedení této akce potvrdit ani vrátit zpátky existující transakci, takže používejte s péčí a jenom pokud jste si jisti, co chcete udělat.  

### <a name="errors-in-usetransaction"></a>Chyby v UseTransaction

Výjimka z databáze. UseTransaction () se zobrazí, Pokud předáte transakci v těchto případech:  
- Entity Framework už má existující transakci.  
- Entity Framework už v rámci objektu TransactionScope funguje.  
- Objekt připojení v předané transakci má hodnotu null. To znamená, že transakce není přidružena k připojení – obvykle se jedná o znaménko, že tato transakce již byla dokončena.  
- Objekt připojení v předané transakci se neshoduje s připojením Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Používání transakcí s jinými funkcemi  

Tato část podrobně popisuje, jak výše uvedené transakce spolupracují:  

- Odolnost připojení  
- Asynchronní metody  
- Transakce TransactionScope  

### <a name="connection-resiliency"></a>Odolnost připojení  

Nová funkce odolnosti připojení nefunguje s transakcemi iniciované uživatelem. Podrobnosti najdete v tématu [opakování strategií provádění](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Asynchronní programování  

Přístup, který je popsaný v předchozích částech, nepotřebuje žádné další možnosti ani nastavení pro práci [s asynchronními dotazy a](~/ef6/fundamentals/async.md
)metodami úspory. Mějte ale na paměti, že v závislosti na tom, co v rámci asynchronních metod provedete, to může způsobit dlouhotrvající transakce – což může způsobit zablokování nebo blokování, které je pro výkon celé aplikace špatné.  

### <a name="transactionscope-transactions"></a>Transakce TransactionScope  

Před EF6 doporučeným způsobem poskytnutí větších transakcí rozsahu bylo použití objektu TransactionScope:  

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

SqlConnection a Entity Framework by používaly ambientní transakci TransactionScope a měla by se tedy považovat za společně.  

Počínaje rozhraním .NET 4.5.1 TransactionScope bylo aktualizováno, aby fungovalo také s asynchronními metodami prostřednictvím použití výčtu [nastavením TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :  

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

Existují i určitá omezení přístupu k objektu TransactionScope:  

- Pro práci s asynchronními metodami vyžaduje rozhraní .NET 4.5.1 nebo novější.  
- Nedá se použít v cloudových scénářích, pokud si nejste jistí, že máte jenom jedno připojení (cloudové scénáře nepodporují distribuované transakce).  
- Nejde kombinovat s přístupem k databázi. UseTransaction () v předchozích částech.  
- Vyvolá výjimky, pokud vydáte skript DDL a nepovolíte distribuované transakce prostřednictvím služby MSDTC.  

Výhody přístupu s objektem TransactionScope:  

- Pokud provedete více než jedno připojení k dané databázi nebo zkombinujete připojení k jedné databázi s připojením k jiné databázi v rámci stejné transakce, bude automaticky upgradována místní transakce na distribuovanou transakci (Poznámka: musíte mít Služba MSDTC nakonfigurovaná tak, aby umožňovala fungování distribuovaných transakcí.  
- Snadné kódování. Pokud upřednostňujete, aby transakce byla ambientní a řešena implicitně na pozadí místo explicitního řízení, může přístup k objektům TransactionScope lépe vyhovovat.  

V souhrnu s novými rozhraními API Database. BeginTransaction () a Database. UseTransaction () výše už není přístup k objektům TransactionScope nutný pro většinu uživatelů. Pokud budete pokračovat v používání objektu TransactionScope, pamatujte na výše uvedená omezení. Místo toho doporučujeme použít postup popsaný v předchozích částech.  
