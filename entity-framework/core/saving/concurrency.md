---
title: "Concurrency – zpracování EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a><span data-ttu-id="d52cc-102">Zpracování souběžnosti</span><span class="sxs-lookup"><span data-stu-id="d52cc-102">Handling Concurrency</span></span>

<span data-ttu-id="d52cc-103">Pokud vlastnost je nakonfigurovaný jako token souběžnosti pak EF zkontroluje, že žádný jiný uživatel změnil tuto hodnotu v databázi při ukládání změn do záznamů.</span><span class="sxs-lookup"><span data-stu-id="d52cc-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span>

> [!TIP]  
> <span data-ttu-id="d52cc-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d52cc-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

## <a name="how-concurrency-handling-works-in-ef-core"></a><span data-ttu-id="d52cc-105">Jak funguje zpracování souběžnosti v EF jádra</span><span class="sxs-lookup"><span data-stu-id="d52cc-105">How concurrency handling works in EF Core</span></span>

<span data-ttu-id="d52cc-106">Podrobný popis toho, jak funguje zpracování souběžnosti v Entity Framework Core, najdete v části [tokenů souběžnosti](../modeling/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="d52cc-106">For a detailed description of how concurrency handling works in Entity Framework Core, see [Concurrency Tokens](../modeling/concurrency.md).</span></span>

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="d52cc-107">Řešení konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="d52cc-107">Resolving concurrency conflicts</span></span>

<span data-ttu-id="d52cc-108">Řešení konfliktů souběžnosti zahrnuje použití algoritmu sloučit čekajících změn z aktuálního uživatele se změny provedené v databázi.</span><span class="sxs-lookup"><span data-stu-id="d52cc-108">Resolving a concurrency conflict involves using an algorithm to merge the pending changes from the current user with the changes made in the database.</span></span> <span data-ttu-id="d52cc-109">Přesný postup budou lišit v závislosti na vaší aplikace, ale běžný postup je zobrazí hodnoty pro uživatele a potom kliknul rozhodnout správné hodnoty, které mají být uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="d52cc-109">The exact approach will vary based on your application, but a common approach is to display the values to the user and have them decide the correct values to be stored in the database.</span></span>

<span data-ttu-id="d52cc-110">**Existují tři sady hodnot, které vám pomůžou vyřešit konflikt souběžnosti.**</span><span class="sxs-lookup"><span data-stu-id="d52cc-110">**There are three sets of values available to help resolve a concurrency conflict.**</span></span>

* <span data-ttu-id="d52cc-111">**Aktuální hodnoty** jsou hodnoty, které aplikace při pokusu o zápis do databáze.</span><span class="sxs-lookup"><span data-stu-id="d52cc-111">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="d52cc-112">**Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze, před jakoukoli úpravou byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="d52cc-112">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="d52cc-113">**Databáze hodnoty** jsou hodnoty, které jsou aktuálně uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="d52cc-113">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="d52cc-114">Pro zpracování konflikt souběžnosti, catch `DbUpdateConcurrencyException` během `SaveChanges()`, použijte `DbUpdateConcurrencyException.Entries` připravit novou sadu změn pro ovlivněné entity, a poté opakujte `SaveChanges()` operaci.</span><span class="sxs-lookup"><span data-stu-id="d52cc-114">To handle a concurrency conflict, catch a `DbUpdateConcurrencyException` during `SaveChanges()`, use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities, and then retry the `SaveChanges()` operation.</span></span>

<span data-ttu-id="d52cc-115">V následujícím příkladu `Person.FirstName` a `Person.LastName` se instalační program jako token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="d52cc-115">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency token.</span></span> <span data-ttu-id="d52cc-116">Došlo `// TODO:` komentář do umístění, kde by mělo zahrnovat určitou logiku aplikace zvolit hodnota, která má být uložen do databáze.</span><span class="sxs-lookup"><span data-stu-id="d52cc-116">There is a `// TODO:` comment in the location where you would include application specific logic to choose the value to be saved to the database.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
