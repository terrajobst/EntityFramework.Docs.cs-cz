---
title: Přizpůsobení tabulky historie migrace - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
caps.latest.revision: 3
ms.openlocfilehash: 3805d99be6d37d037096f5c5fb69fd30197c1e91
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914037"
---
# <a name="customizing-the-migrations-history-table"></a>Přizpůsobení tabulky historie migrace
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.

> [!NOTE]
> Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře. Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.

## <a name="what-is-migrations-history-table"></a>Co je tabulka historie migrace?

Migrace historie tabulka je tabulka používá k ukládání podrobností o migracích použita pro databázi pomocí migrace Code First. Ve výchozím nastavení je název tabulky v databázi \_ \_MigrationHistory a je vytvořen při použití první migrace databáze. V Entity Framework 5 Tato tabulka byla systémové tabulky, pokud aplikace používá databáze Microsoft Sql Server. To se změnilo v Entity Framework 6 ale a v tabulce historie migrace už není označený systémové tabulky.

## <a name="why-customize-migrations-history-table"></a>Proč přizpůsobení tabulky historie migrace?

Tabulky historie migrace by měl být používány pouze ve migrace Code First a ručně ji změnit může dojít k narušení migrace. Ale někdy není vhodný výchozí konfigurace a tabulce potřebuje k přizpůsobení, například:

-   Je potřeba změnit názvy a/nebo omezující vlastnosti pro sloupce, které chcete povolit 3<sup>VP</sup> poskytovatele migrace
-   Chcete změnit název tabulky
-   Budete muset použít jiné než výchozí schéma pro \_ \_MigrationHistory tabulky
-   Potřebujete ukládat další data pro danou verzi kontextu a proto budete muset přidat další sloupce do tabulky

## <a name="words-of-precaution"></a>Slova preventivní opatření

Změna tabulky historie migrace je efektivní, ale musíte mít na paměti není nepřehánějte to. Modul runtime EF aktuálně nekontroluje, jestli v tabulce historie přizpůsobená migrace je kompatibilní s modulem runtime. Pokud není aplikace může přerušit za běhu nebo se chovat nepředvídatelně. To je ještě více důležité, pokud používáte více kontexty na databázi v takovém případě několika kontextech můžete použít stejné tabulky historie migrace k ukládání informací o migraci.

## <a name="how-to-customize-migrations-history-table"></a>Postup přizpůsobení tabulky historie migrace?

Než začnete, je potřeba vědět, lze přizpůsobit v tabulce historie migrace pouze před použitím první migraci. Nyní s kódem.

Nejprve musíte vytvořit třídu odvozenou z třídy System.Data.Entity.Migrations.History.HistoryContext. HistoryContext třída je odvozena od třídy DbContext, tak v tabulce historie migrace konfigurace je velmi podobné konfigurace EF modely s rozhraním API fluent. Stačí přepsat metodu OnModelCreating a konfigurace tabulky pomocí rozhraní API fluent.

>[!NOTE]
> Při konfiguraci EF modely není obvykle potřeba volat základní. OnModelCreating() z přepsané metody OnModelCreating vzhledem k tomu, DbContext.OnModelCreating() má prázdný text. Toto není tento případ, při konfiguraci migrace tabulky historie. V tomto případě první věc, kterou provedete v přepsání OnModelCreating() je ve skutečnosti volat základní. OnModelCreating(). Tím nakonfigurujete v tabulce historie migrace výchozí způsobem, který můžete upravit v přepsání metody.

Řekněme, že chcete přejmenovat tabulku historie migrace a vložit ho do vlastního schématu nazývá "admin". Kromě toho vaše DBA byste o ni můžete přejmenovat sloupec MigrationId migrace\_ID.  Může dosáhnout vytvořením následující třídy odvozené od HistoryContext:

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

Jakmile vlastní HistoryContext připravený budete muset udělat EF upozornit když si zaregistrujete ho přes [konfigurace založená na kódu](http://msdn.com/data/jj680699):

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

Je to podstatě. Teď můžete přejít do konzoly Správce balíčků, povolení migrace, migrace přidat a nakonec aktualizaci databáze. Výsledkem by měl být přidávání do databáze tabulky historie migrace nakonfigurovaný podle podrobnostmi, které jste zadali ve vaší HistoryContext odvozené třídy.

![Databáze](~/ef6/media/database.png)
