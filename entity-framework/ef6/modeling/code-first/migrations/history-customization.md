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
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="43a63-102">Přizpůsobení tabulky historie migrace</span><span class="sxs-lookup"><span data-stu-id="43a63-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="43a63-103">**EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="43a63-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="43a63-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="43a63-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="43a63-105">V tomto článku se dozvíte, jak používat Migrace Code First v základních scénářích.</span><span class="sxs-lookup"><span data-stu-id="43a63-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="43a63-106">Pokud to neuděláte, budete muset před pokračováním přečíst [migrace Code First](~/ef6/modeling/code-first/migrations/index.md) .</span><span class="sxs-lookup"><span data-stu-id="43a63-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="43a63-107">Co je tabulka historie migrace?</span><span class="sxs-lookup"><span data-stu-id="43a63-107">What is Migrations History Table?</span></span>

<span data-ttu-id="43a63-108">Tabulka historie migrace je tabulka, kterou používá Migrace Code First k ukládání podrobností o migraci použitých do databáze.</span><span class="sxs-lookup"><span data-stu-id="43a63-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="43a63-109">Ve výchozím nastavení je název tabulky v databázi \_\_MigrationHistory a vytvoří se při použití první migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="43a63-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="43a63-110">V Entity Framework 5 Tato tabulka byla systémovou tabulkou, pokud aplikace používala databázi Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="43a63-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="43a63-111">Tato změna se změnila v Entity Framework 6, ale tabulka historie migrace již neoznačuje systémovou tabulku.</span><span class="sxs-lookup"><span data-stu-id="43a63-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="43a63-112">Proč přizpůsobit tabulku historie migrace?</span><span class="sxs-lookup"><span data-stu-id="43a63-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="43a63-113">Tabulka historie migrace by se měla používat výhradně Migrace Code First a ruční změna může způsobit přerušení migrace.</span><span class="sxs-lookup"><span data-stu-id="43a63-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="43a63-114">V některých případech však není výchozí konfigurace vhodná a tabulka musí být upravena, např.:</span><span class="sxs-lookup"><span data-stu-id="43a63-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="43a63-115">Aby bylo možné povolit poskytovatele migrace služby<sup>Vzdálená plocha</sup> 3, musíte změnit názvy a/nebo omezující vlastnosti sloupců.</span><span class="sxs-lookup"><span data-stu-id="43a63-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="43a63-116">Chcete změnit název tabulky</span><span class="sxs-lookup"><span data-stu-id="43a63-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="43a63-117">Pro \_\_tabulka MigrationHistory je nutné použít jiné než výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="43a63-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="43a63-118">Je nutné uložit další data pro danou verzi kontextu, a proto je nutné přidat do tabulky další sloupec.</span><span class="sxs-lookup"><span data-stu-id="43a63-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="43a63-119">Slova preventivních hodnot</span><span class="sxs-lookup"><span data-stu-id="43a63-119">Words of precaution</span></span>

<span data-ttu-id="43a63-120">Změna tabulky historie migrace je účinná, ale musíte být opatrní, abyste ji nenepřehánějtei.</span><span class="sxs-lookup"><span data-stu-id="43a63-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="43a63-121">Modul runtime EF aktuálně nekontroluje, jestli je tabulka historie migrace přizpůsobená s modulem runtime kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="43a63-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="43a63-122">Pokud to tak není, vaše aplikace může být přerušena za běhu nebo se chová v nepředvídatelných způsobech.</span><span class="sxs-lookup"><span data-stu-id="43a63-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="43a63-123">To je ještě důležitější, pokud použijete pro každou databázi více kontextů, v takovém případě více kontextů může používat stejnou tabulku historie migrace k ukládání informací o migraci.</span><span class="sxs-lookup"><span data-stu-id="43a63-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="43a63-124">Jak přizpůsobit tabulku historie migrace?</span><span class="sxs-lookup"><span data-stu-id="43a63-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="43a63-125">Než začnete, musíte mít jistotu, že tabulku historie migrace můžete přizpůsobit jenom předtím, než použijete první migraci.</span><span class="sxs-lookup"><span data-stu-id="43a63-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="43a63-126">Nyní do kódu.</span><span class="sxs-lookup"><span data-stu-id="43a63-126">Now, to the code.</span></span>

<span data-ttu-id="43a63-127">Nejprve bude nutné vytvořit třídu odvozenou z třídy System. data. entity. migrations. History. HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="43a63-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="43a63-128">Třída HistoryContext je odvozená od třídy DbContext, takže konfigurace tabulky historie migrace je velmi podobná konfiguraci modelů EF pomocí rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="43a63-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="43a63-129">Stačí přepsat metodu OnModelCreating a použít rozhraní Fluent API ke konfiguraci tabulky.</span><span class="sxs-lookup"><span data-stu-id="43a63-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="43a63-130">Obvykle když nakonfigurujete modely EF, nemusíte volat základ. OnModelCreating () z přepsané metody OnModelCreating, protože DbContext. OnModelCreating () má prázdné tělo.</span><span class="sxs-lookup"><span data-stu-id="43a63-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="43a63-131">Nejedná se o případ, kdy se konfiguruje tabulka historie migrace.</span><span class="sxs-lookup"><span data-stu-id="43a63-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="43a63-132">V tomto případě je první věc, kterou je třeba provést v přepsání OnModelCreating (), ve skutečnosti volat základ. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="43a63-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="43a63-133">Tím se nakonfiguruje tabulka historie migrace ve výchozím způsobu, jakým se následně přizpůsobuje přepsání metody.</span><span class="sxs-lookup"><span data-stu-id="43a63-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="43a63-134">Řekněme, že chcete přejmenovat tabulku historie migrace a umístit ji do vlastního schématu s názvem admin.</span><span class="sxs-lookup"><span data-stu-id="43a63-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="43a63-135">Kromě vašeho DBA byste chtěli Přejmenovat sloupec MigrationId na ID migrace\_.</span><span class="sxs-lookup"><span data-stu-id="43a63-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="43a63-136"> Můžete to dosáhnout vytvořením následující třídy odvozené z HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="43a63-136"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="43a63-137">Až bude vaše vlastní HistoryContext připravená, je potřeba, abyste si ho zaregistrovali pomocí [konfigurace založené na kódu](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="43a63-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="43a63-138">To je poměrně mnoho.</span><span class="sxs-lookup"><span data-stu-id="43a63-138">That’s pretty much it.</span></span> <span data-ttu-id="43a63-139">Teď můžete přejít do konzoly Správce balíčků, povolit – migrace, přidat migraci a nakonec aktualizovat – databáze.</span><span class="sxs-lookup"><span data-stu-id="43a63-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="43a63-140">To by mělo mít za následek přidání do databáze historie migrace v závislosti na podrobnostech, které jste zadali v odvozené třídě HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="43a63-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![databáze](~/ef6/media/database.png)
