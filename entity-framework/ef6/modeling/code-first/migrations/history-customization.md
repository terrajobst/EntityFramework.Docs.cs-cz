---
title: Přizpůsobení tabulky historie migrace - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: ad07b1fe97b3dafe789c0315bd555f979fc412e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996758"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="d24db-102">Přizpůsobení tabulky historie migrace</span><span class="sxs-lookup"><span data-stu-id="d24db-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="d24db-103">**EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d24db-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="d24db-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="d24db-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="d24db-105">Tento článek předpokládá, že víte, jak pomocí migrace Code First v základní scénáře.</span><span class="sxs-lookup"><span data-stu-id="d24db-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="d24db-106">Pokud ne, pak budete muset přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d24db-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="d24db-107">Co je tabulka historie migrace?</span><span class="sxs-lookup"><span data-stu-id="d24db-107">What is Migrations History Table?</span></span>

<span data-ttu-id="d24db-108">Migrace historie tabulka je tabulka používá k ukládání podrobností o migracích použita pro databázi pomocí migrace Code First.</span><span class="sxs-lookup"><span data-stu-id="d24db-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="d24db-109">Ve výchozím nastavení je název tabulky v databázi \_ \_MigrationHistory a je vytvořen při použití první migrace databáze.</span><span class="sxs-lookup"><span data-stu-id="d24db-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="d24db-110">V Entity Framework 5 Tato tabulka byla systémové tabulky, pokud aplikace používá databáze Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="d24db-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="d24db-111">To se změnilo v Entity Framework 6 ale a v tabulce historie migrace už není označený systémové tabulky.</span><span class="sxs-lookup"><span data-stu-id="d24db-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="d24db-112">Proč přizpůsobení tabulky historie migrace?</span><span class="sxs-lookup"><span data-stu-id="d24db-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="d24db-113">Tabulky historie migrace by měl být používány pouze ve migrace Code First a ručně ji změnit může dojít k narušení migrace.</span><span class="sxs-lookup"><span data-stu-id="d24db-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="d24db-114">Ale někdy není vhodný výchozí konfigurace a tabulce potřebuje k přizpůsobení, například:</span><span class="sxs-lookup"><span data-stu-id="d24db-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="d24db-115">Je potřeba změnit názvy a/nebo omezující vlastnosti pro sloupce, které chcete povolit 3<sup>VP</sup> poskytovatele migrace</span><span class="sxs-lookup"><span data-stu-id="d24db-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="d24db-116">Chcete změnit název tabulky</span><span class="sxs-lookup"><span data-stu-id="d24db-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="d24db-117">Budete muset použít jiné než výchozí schéma pro \_ \_MigrationHistory tabulky</span><span class="sxs-lookup"><span data-stu-id="d24db-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="d24db-118">Potřebujete ukládat další data pro danou verzi kontextu a proto budete muset přidat další sloupce do tabulky</span><span class="sxs-lookup"><span data-stu-id="d24db-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="d24db-119">Slova preventivní opatření</span><span class="sxs-lookup"><span data-stu-id="d24db-119">Words of precaution</span></span>

<span data-ttu-id="d24db-120">Změna tabulky historie migrace je efektivní, ale musíte mít na paměti není nepřehánějte to.</span><span class="sxs-lookup"><span data-stu-id="d24db-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="d24db-121">Modul runtime EF aktuálně nekontroluje, jestli v tabulce historie přizpůsobená migrace je kompatibilní s modulem runtime.</span><span class="sxs-lookup"><span data-stu-id="d24db-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="d24db-122">Pokud není aplikace může přerušit za běhu nebo se chovat nepředvídatelně.</span><span class="sxs-lookup"><span data-stu-id="d24db-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="d24db-123">To je ještě více důležité, pokud používáte více kontexty na databázi v takovém případě několika kontextech můžete použít stejné tabulky historie migrace k ukládání informací o migraci.</span><span class="sxs-lookup"><span data-stu-id="d24db-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="d24db-124">Postup přizpůsobení tabulky historie migrace?</span><span class="sxs-lookup"><span data-stu-id="d24db-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="d24db-125">Než začnete, je potřeba vědět, lze přizpůsobit v tabulce historie migrace pouze před použitím první migraci.</span><span class="sxs-lookup"><span data-stu-id="d24db-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="d24db-126">Nyní s kódem.</span><span class="sxs-lookup"><span data-stu-id="d24db-126">Now, to the code.</span></span>

<span data-ttu-id="d24db-127">Nejprve musíte vytvořit třídu odvozenou z třídy System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="d24db-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="d24db-128">HistoryContext třída je odvozena od třídy DbContext, tak v tabulce historie migrace konfigurace je velmi podobné konfigurace EF modely s rozhraním API fluent.</span><span class="sxs-lookup"><span data-stu-id="d24db-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="d24db-129">Stačí přepsat metodu OnModelCreating a konfigurace tabulky pomocí rozhraní API fluent.</span><span class="sxs-lookup"><span data-stu-id="d24db-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="d24db-130">Při konfiguraci EF modely není obvykle potřeba volat základní. OnModelCreating() z přepsané metody OnModelCreating vzhledem k tomu, DbContext.OnModelCreating() má prázdný text.</span><span class="sxs-lookup"><span data-stu-id="d24db-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="d24db-131">Toto není tento případ, při konfiguraci migrace tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="d24db-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="d24db-132">V tomto případě první věc, kterou provedete v přepsání OnModelCreating() je ve skutečnosti volat základní. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="d24db-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="d24db-133">Tím nakonfigurujete v tabulce historie migrace výchozí způsobem, který můžete upravit v přepsání metody.</span><span class="sxs-lookup"><span data-stu-id="d24db-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="d24db-134">Řekněme, že chcete přejmenovat tabulku historie migrace a vložit ho do vlastního schématu nazývá "admin".</span><span class="sxs-lookup"><span data-stu-id="d24db-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="d24db-135">Kromě toho vaše DBA byste o ni můžete přejmenovat sloupec MigrationId migrace\_ID.</span><span class="sxs-lookup"><span data-stu-id="d24db-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="d24db-136">Může dosáhnout vytvořením následující třídy odvozené od HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="d24db-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="d24db-137">Jakmile vlastní HistoryContext připravený budete muset udělat EF upozornit když si zaregistrujete ho přes [konfigurace založená na kódu](http://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="d24db-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](http://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="d24db-138">Je to podstatě.</span><span class="sxs-lookup"><span data-stu-id="d24db-138">That’s pretty much it.</span></span> <span data-ttu-id="d24db-139">Teď můžete přejít do konzoly Správce balíčků, povolení migrace, migrace přidat a nakonec aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="d24db-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="d24db-140">Výsledkem by měl být přidávání do databáze tabulky historie migrace nakonfigurovaný podle podrobnostmi, které jste zadali ve vaší HistoryContext odvozené třídy.</span><span class="sxs-lookup"><span data-stu-id="d24db-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Databáze](~/ef6/media/database.png)
