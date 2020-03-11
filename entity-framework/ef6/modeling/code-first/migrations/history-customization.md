---
title: Přizpůsobení tabulky historie migrace – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418977"
---
# <a name="customizing-the-migrations-history-table"></a>Přizpůsobení tabulky historie migrace
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.

> [!NOTE]
> V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích. Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="what-is-migrations-history-table"></a>Co je tabulka historie migrace?

Tabulka historie migrace je tabulka, kterou používá Migrace Code First k ukládání podrobností o migraci použitých do databáze. Ve výchozím nastavení je název tabulky v databázi \_\_MigrationHistory a vytvoří se při použití první migrace do databáze. V Entity Framework 5 Tato tabulka byla systémovou tabulkou, pokud aplikace používala databázi Microsoft SQL Server. Tato změna se změnila v Entity Framework 6, ale tabulka historie migrace již neoznačuje systémovou tabulku.

## <a name="why-customize-migrations-history-table"></a>Proč přizpůsobit tabulku historie migrace?

Tabulka historie migrace by se měla používat výhradně Migrace Code First a ruční změna může způsobit přerušení migrace. V některých případech však není výchozí konfigurace vhodná a tabulka musí být upravena, např.:

-   Aby bylo možné povolit poskytovatele migrace služby<sup>Vzdálená plocha</sup> 3, musíte změnit názvy a/nebo omezující vlastnosti sloupců.
-   Chcete změnit název tabulky
-   Pro \_\_tabulka MigrationHistory je nutné použít jiné než výchozí schéma.
-   Je nutné uložit další data pro danou verzi kontextu, a proto je nutné přidat do tabulky další sloupec.

## <a name="words-of-precaution"></a>Slova preventivních hodnot

Změna tabulky historie migrace je účinná, ale musíte být opatrní, abyste ji nenepřehánějtei. Modul runtime EF aktuálně nekontroluje, jestli je tabulka historie migrace přizpůsobená s modulem runtime kompatibilní. Pokud to tak není, vaše aplikace může být přerušena za běhu nebo se chová v nepředvídatelných způsobech. To je ještě důležitější, pokud použijete pro každou databázi více kontextů, v takovém případě více kontextů může používat stejnou tabulku historie migrace k ukládání informací o migraci.

## <a name="how-to-customize-migrations-history-table"></a>Jak přizpůsobit tabulku historie migrace?

Než začnete, musíte mít jistotu, že tabulku historie migrace můžete přizpůsobit jenom předtím, než použijete první migraci. Nyní do kódu.

Nejprve bude nutné vytvořit třídu odvozenou z třídy System. data. entity. migrations. History. HistoryContext. Třída HistoryContext je odvozená od třídy DbContext, takže konfigurace tabulky historie migrace je velmi podobná konfiguraci modelů EF pomocí rozhraní Fluent API. Stačí přepsat metodu OnModelCreating a použít rozhraní Fluent API ke konfiguraci tabulky.

>[!NOTE]
> Obvykle když nakonfigurujete modely EF, nemusíte volat základ. OnModelCreating () z přepsané metody OnModelCreating, protože DbContext. OnModelCreating () má prázdné tělo. Nejedná se o případ, kdy se konfiguruje tabulka historie migrace. V tomto případě je první věc, kterou je třeba provést v přepsání OnModelCreating (), ve skutečnosti volat základ. OnModelCreating(). Tím se nakonfiguruje tabulka historie migrace ve výchozím způsobu, jakým se následně přizpůsobuje přepsání metody.

Řekněme, že chcete přejmenovat tabulku historie migrace a umístit ji do vlastního schématu s názvem admin. Kromě vašeho DBA byste chtěli Přejmenovat sloupec MigrationId na ID migrace\_.  Můžete to dosáhnout vytvořením následující třídy odvozené z HistoryContext:

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

Až bude vaše vlastní HistoryContext připravená, je potřeba, abyste si ho zaregistrovali pomocí [konfigurace založené na kódu](https://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

To je poměrně mnoho. Teď můžete přejít do konzoly Správce balíčků, povolit – migrace, přidat migraci a nakonec aktualizovat – databáze. To by mělo mít za následek přidání do databáze historie migrace v závislosti na podrobnostech, které jste zadali v odvozené třídě HistoryContext.

![databáze](~/ef6/media/database.png)
