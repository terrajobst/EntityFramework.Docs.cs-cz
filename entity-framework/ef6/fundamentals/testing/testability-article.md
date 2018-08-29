---
title: Testovatelnost a Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 17a9f09022531a81042979464de05fbbd2570759
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995226"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="a5953-102">Testovatelnost a Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="a5953-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="a5953-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="a5953-103">Scott Allen</span></span>

<span data-ttu-id="a5953-104">Publikováno: Květen 2010</span><span class="sxs-lookup"><span data-stu-id="a5953-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="a5953-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="a5953-105">Introduction</span></span>

<span data-ttu-id="a5953-106">Tento dokument white paper popisuje a ukazuje, jak psát testovatelného kód pomocí technologie ADO.NET Entity Framework 4.0 a Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a5953-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="a5953-107">Tento dokument se nepokusí se zaměřit na konkrétní testovací metody, jako je návrh řízený testováním (TDD) nebo návrh na základě chování (BDD).</span><span class="sxs-lookup"><span data-stu-id="a5953-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="a5953-108">Místo toho tento dokument se zaměřuje na tom, jak napsat kód, který používá rozhraní ADO.NET Entity Framework, ale zůstává snadno izolovat a testovat automatizovaně.</span><span class="sxs-lookup"><span data-stu-id="a5953-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="a5953-109">Podíváme se na běžných vzorů návrhu, které usnadňují testování v datech scénáře přístupu a zjistit, jak použít tyto vzorce při použití rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5953-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="a5953-110">Také podíváme na konkrétní funkce rozhraní Framework, pokud chcete zobrazit, jak můžete tyto funkce fungují v testovatelného kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="a5953-111">Co je Testovatelného kód?</span><span class="sxs-lookup"><span data-stu-id="a5953-111">What Is Testable Code?</span></span>

<span data-ttu-id="a5953-112">Schopnost ověřit softwarového produktu pomocí automatizované testy jednotky nabízí řadu výhod žádoucí.</span><span class="sxs-lookup"><span data-stu-id="a5953-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="a5953-113">Všichni ví, že dobré testy se sníží počet vady softwaru aplikace a zvýšení kvality vaší aplikace – ale s testy jednotek v místě dalece přesahuje právě hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="a5953-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="a5953-114">Sadu testů jednotky dobré umožňuje vývojový tým ušetřit čas a udržíme si kontrolu softwaru, který vytvoří.</span><span class="sxs-lookup"><span data-stu-id="a5953-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="a5953-115">Tým může provádět změny existující kód Refaktorovat, přepracování a změnu struktury softwaru splňovat nové požadavky a přidat nové komponenty do aplikace všechny zároveň budete vědět sadu testů můžete ověřit, chování aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5953-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="a5953-116">Testování jednotek je součást cyklu rychlé zpětné vazby a usnadněte změnu zachování udržovatelnosti softwaru jako zvýšení složitosti.</span><span class="sxs-lookup"><span data-stu-id="a5953-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="a5953-117">Testování částí se dodává s cenu, ale.</span><span class="sxs-lookup"><span data-stu-id="a5953-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="a5953-118">Tým má investovat čas k vytváření a údržbě testů jednotek.</span><span class="sxs-lookup"><span data-stu-id="a5953-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="a5953-119">Množství úsilí, abyste tyto testy můžete vytvářet přímo souvisí s **testovatelnosti** základní softwaru.</span><span class="sxs-lookup"><span data-stu-id="a5953-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="a5953-120">Jak snadné je software k testování?</span><span class="sxs-lookup"><span data-stu-id="a5953-120">How easy is the software to test?</span></span> <span data-ttu-id="a5953-121">Tým softwaru návrhu s testovatelností v úvahu se vytvořit efektivní testy rychleji, než tým pracovat bez možností intenzivního testování softwaru.</span><span class="sxs-lookup"><span data-stu-id="a5953-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="a5953-122">Microsoft proto navrhl ADO.NET Entity Framework 4.0 (EF4) s testovatelností v úvahu.</span><span class="sxs-lookup"><span data-stu-id="a5953-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="a5953-123">To neznamená, že vývojáři bude psaní testů jednotek pro samotný kód rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="a5953-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="a5953-124">Místo toho testovatelnosti cíle pro EF4 usnadňují vytváření testovatelného kód, který vytváří jako nadstavby rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="a5953-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="a5953-125">Předtím, než se podíváme na konkrétní příklady, je vhodné pochopit kvality testovatelného kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="a5953-126">Kvality Testovatelného kód</span><span class="sxs-lookup"><span data-stu-id="a5953-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="a5953-127">Kód, který je snadno testovat bude mít vždy měly aspoň dva znaky.</span><span class="sxs-lookup"><span data-stu-id="a5953-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="a5953-128">První, testovatelného kód je snadné **sledovat**.</span><span class="sxs-lookup"><span data-stu-id="a5953-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="a5953-129">Zadaný některé sadu vstupů, by mělo být snadné sledovat výstup kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="a5953-130">Například následující metodu testování je snadné vzhledem k tomu, že metoda přímo vrátí výsledek výpočtu.</span><span class="sxs-lookup"><span data-stu-id="a5953-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="a5953-131">Testování metodu je obtížné, pokud metoda zapíše vypočtená hodnota do soketu síťové, databázové tabulky nebo soubor jako v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="a5953-132">Test musí provést další práce k načtení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a5953-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="a5953-133">Za druhé, je snadné testovatelného kód **izolovat**.</span><span class="sxs-lookup"><span data-stu-id="a5953-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="a5953-134">Použijeme následující pseudo kód jako chybný příklad možností intenzivního testování kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="a5953-135">Metoda je snadné sledovat – můžeme předat v zásadách pojištění a ověřte, že návratová hodnota odpovídá očekávaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="a5953-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="a5953-136">Ale pro testovací metodu potřebujeme mít databázi nainstalovanou s správné schéma a konfigurovat SMTP server pro případ, metoda se pokusí odeslat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a5953-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="a5953-137">Testování částí pouze chce, aby se k ověření logiky výpočtu uvnitř metody, ale test může selhat, protože e-mailový server je offline, nebo přesunout databázového serveru.</span><span class="sxs-lookup"><span data-stu-id="a5953-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="a5953-138">Obě tyto chyby nesouvisí s chování, které chce, aby se test k ověření.</span><span class="sxs-lookup"><span data-stu-id="a5953-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="a5953-139">Chování je obtížné izolovat.</span><span class="sxs-lookup"><span data-stu-id="a5953-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="a5953-140">Vývojářům, kteří snažit se psát testovatelného kód často snažit Udržovat oddělené oblasti zájmu v kódu, jsou zápisu.</span><span class="sxs-lookup"><span data-stu-id="a5953-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="a5953-141">Výše uvedené metody by měly soustředit na obchodní výpočty a delegovat podrobnosti implementace databáze a e-mailu na jiné komponenty.</span><span class="sxs-lookup"><span data-stu-id="a5953-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="a5953-142">Robert C. Martin označuje jako jednotné zásady odpovědnosti.</span><span class="sxs-lookup"><span data-stu-id="a5953-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="a5953-143">Objekt by měl zapouzdření jednoho úzký odpovědnosti, jako je výpočet hodnoty zásad.</span><span class="sxs-lookup"><span data-stu-id="a5953-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="a5953-144">Všechny ostatní databáze a oznámení práce by měl být odpovědnost některý jiný objekt.</span><span class="sxs-lookup"><span data-stu-id="a5953-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="a5953-145">Kód napsaný tímto způsobem je jednodušší oddělit, protože se zaměřuje na jednu úlohu.</span><span class="sxs-lookup"><span data-stu-id="a5953-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="a5953-146">V rozhraní .NET máme abstrakce, musíme postupovat podle principu jednu zodpovědnost a dosáhnout izolace.</span><span class="sxs-lookup"><span data-stu-id="a5953-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="a5953-147">Můžeme používat definice rozhraní a vynutit kód, který použije místo konkrétního typu implementujícího typ abstrakce rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5953-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="a5953-148">Později v tomto dokumentu uvidíme, jak metoda jako chybný příklad uvedený výše může spolupracovat s rozhraní, které *vypadat* jako bude komunikovat s databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="a5953-149">Během testu můžeme však nahradit fiktivní implementace, která nebude komunikovat s databáze, ale místo toho obsahuje data v paměti.</span><span class="sxs-lookup"><span data-stu-id="a5953-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="a5953-150">Tuto fiktivní implementace izoluje kód z nesouvisejících potíže kód přístupu k datům nebo konfigurace databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="a5953-151">Existují další výhody izolace.</span><span class="sxs-lookup"><span data-stu-id="a5953-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="a5953-152">Obchodní výpočet v metodě poslední by mělo trvat pouze několik milisekund, než ke spuštění, ale samotný test může spustit několik sekund jako směrování kódu kolem sítě a přednášky na různé servery.</span><span class="sxs-lookup"><span data-stu-id="a5953-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="a5953-153">Testy jednotek by měl spustit rychlé usnadnit malé změny.</span><span class="sxs-lookup"><span data-stu-id="a5953-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="a5953-154">Testy jednotek by měl také být opakovatelným a není nezdaří, protože nesouvisí se test komponenty došlo k problému.</span><span class="sxs-lookup"><span data-stu-id="a5953-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="a5953-155">Psaní kódu, který je snadno sledovat a k izolování znamená, že vývojáři budou mít usnadní psaní testů pro kód, trávit méně času čekáním na testy ke spuštění a další důležité je, strávit míň času sledování chyby v kódu, které neexistují.</span><span class="sxs-lookup"><span data-stu-id="a5953-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="a5953-156">Snad můžete pochopit výhody testování a pochopit vlastnosti, které je třeba testovatelného kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="a5953-157">Nepovedlo se chystáte se vyřešit jak napsat kód, který funguje s EF4 k uložení dat do databáze přitom pozorovatelné a snadno izolovat, ale nejprve jsme vám zúžit našim hlavním cílem fattica možností intenzivního testování návrhů pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="a5953-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="a5953-158">Způsoby návrhu pro trvalost dat</span><span class="sxs-lookup"><span data-stu-id="a5953-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="a5953-159">Obě chybný příkladů uvedených výše má příliš mnoho odpovědnosti.</span><span class="sxs-lookup"><span data-stu-id="a5953-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="a5953-160">V prvním příkladu chybný došlo k provedení výpočtu *a* zápis do souboru.</span><span class="sxs-lookup"><span data-stu-id="a5953-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="a5953-161">V druhém příkladu chybný museli číst data z databáze *a* provádět obchodní výpočet *a* odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a5953-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="a5953-162">Navržením menší metody, které oddělení připomínky a delegovat na ostatní součásti provedete skvělé pokroku v oblasti zápisu testovatelného kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="a5953-163">Cílem je vytvářet funkce vytváření akce v malém rozsahu a zaměřeně abstrakce.</span><span class="sxs-lookup"><span data-stu-id="a5953-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="a5953-164">Pokud jde o trvalost dat malé a jsou proto společné cílené abstrakce, které Těšíme se na jste bylo uvedeno jako vzory návrhu.</span><span class="sxs-lookup"><span data-stu-id="a5953-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="a5953-165">Kniha Martina Fowlera vzory Enterprise Application Architecture byl první práce, která popisují tyto vzory v tisku.</span><span class="sxs-lookup"><span data-stu-id="a5953-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="a5953-166">Předtím, než vám ukážeme, jak tyto technologie ADO.NET Entity Framework implementuje a funguje s tyto vzory poskytujeme stručný popis tyto vzory v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="a5953-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="a5953-167">Model úložiště</span><span class="sxs-lookup"><span data-stu-id="a5953-167">The Repository Pattern</span></span>

<span data-ttu-id="a5953-168">Fowler vám říká úložiště "zprostředkovává mezi doménou a data vrstev mapování rozhraní kolekce jako pro přístup k objektům domény".</span><span class="sxs-lookup"><span data-stu-id="a5953-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="a5953-169">Cílem model úložiště je izolovat kódu z minutiae přístup k datům a jak jsme viděli starší izolace je požadovaná vlastnost pro testovatelnost.</span><span class="sxs-lookup"><span data-stu-id="a5953-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="a5953-170">Klíčem k izolaci je, jak úložiště poskytuje objekty pomocí rozhraní jako v kolekci.</span><span class="sxs-lookup"><span data-stu-id="a5953-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="a5953-171">Logika, kterou píšete úložiště nemá představu, jak používat úložiště sloučit objekty, které požadujete.</span><span class="sxs-lookup"><span data-stu-id="a5953-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="a5953-172">Úložiště může komunikovat s databází nebo objektů může vrátit pouze z kolekce v paměti.</span><span class="sxs-lookup"><span data-stu-id="a5953-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="a5953-173">Všechno, co váš kód potřebuje vědět, je, že úložiště se zobrazí k udržení kolekce, a můžete načíst, přidat nebo odstranit objekty z kolekce.</span><span class="sxs-lookup"><span data-stu-id="a5953-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="a5953-174">V existujících aplikací .NET na konkrétní úložiště často dědí z obecného rozhraní vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="a5953-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="a5953-175">Provedeme několik změn s definice rozhraní, když jsme EF4 poskytnout implementaci, ale základní princip zůstává stejná.</span><span class="sxs-lookup"><span data-stu-id="a5953-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="a5953-176">Kód můžete použít konkrétní úložiště implementace tohoto rozhraní k načtení entity podle jeho hodnotu primárního klíče, k načtení kolekce entit na základě vyhodnocení predikátu, nebo jednoduše načíst všechny dostupné entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="a5953-177">Kód můžete také přidávat a odebírat entity prostřednictvím rozhraní úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="a5953-178">S ohledem objekty IRepository zaměstnance, kód můžete provádět následující operace.</span><span class="sxs-lookup"><span data-stu-id="a5953-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="a5953-179">Protože kód používá rozhraní (IRepository zaměstnanců), abychom mohli kód poskytovat různé implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5953-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="a5953-180">Jedna implementace může být implementace se opírá o EF4 a uchování objektů do databáze Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a5953-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="a5953-181">Jinou implementaci (jedna, které používáme během testování) může opírá zaměstnance seznam objektů v paměti.</span><span class="sxs-lookup"><span data-stu-id="a5953-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="a5953-182">Rozhraní vám pomůže dosáhnout izolace v kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="a5953-183">Všimněte si, IRepository&lt;T&gt; rozhraní nevystavuje operace uložit.</span><span class="sxs-lookup"><span data-stu-id="a5953-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="a5953-184">Jak jsme aktualizaci existujících objektů?</span><span class="sxs-lookup"><span data-stu-id="a5953-184">How do we update existing objects?</span></span> <span data-ttu-id="a5953-185">Můžete narazit na IRepository definice, které zahrnují operace Uložit a implementace těchto úložišť muset okamžitě uchování objektu do databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="a5953-186">V mnoha aplikacích nechceme však zachovat objekty jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="a5953-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="a5953-187">Místo toho chceme Oživte objekty, třeba z různých úložišť, upravit obchodních aktivit v rámci těchto objektů a pak zachovat všechny objekty v rámci jediné atomické operace.</span><span class="sxs-lookup"><span data-stu-id="a5953-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="a5953-188">Naštěstí je vzor, podle kterého byl tento typ chování.</span><span class="sxs-lookup"><span data-stu-id="a5953-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="a5953-189">Jednotka pracovní postup</span><span class="sxs-lookup"><span data-stu-id="a5953-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="a5953-190">Fowler vám říká, určitou jednotku práce se "udržovat seznam objektů ovlivněna obchodní transakce a koordinuje zápisu mimo změny a řešení problémů souběžnosti".</span><span class="sxs-lookup"><span data-stu-id="a5953-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="a5953-191">Zodpovídá za jednotky práce a sledování změn objektů jsme Vdechněte život z úložiště a zachovat všechny změny, které jsme udělali na objekty při tom jednotku práce tím potvrdíte změny.</span><span class="sxs-lookup"><span data-stu-id="a5953-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="a5953-192">Je také odpovědnost jednotku práce se nové objekty, které jsme přidali na všechna úložiště a vložit do databáze a také restartovat odstranění objektů.</span><span class="sxs-lookup"><span data-stu-id="a5953-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="a5953-193">Pokud jste někdy provedli veškerou práci s datovými sadami ADO.NET pak už budete znát jednotka pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="a5953-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="a5953-194">Datovými sadami ADO.NET měli možnost sledovat naše aktualizace, odstranění a vložení DataRow objektů a může (s použitím objektu TableAdapter) odsouhlaste naše změny do databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="a5953-195">Ale datovou sadu objektů modelu podmnožinu podkladové databázi odpojené.</span><span class="sxs-lookup"><span data-stu-id="a5953-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="a5953-196">Jednotka pracovní postup vykazuje stejné chování, ale s obchodních objektů a objektů domény, které jsou izolované od dat přístupový kód a bez ohledu databázi.</span><span class="sxs-lookup"><span data-stu-id="a5953-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="a5953-197">Abstrakce pro modelování jednotku práce v kódu .NET může vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="a5953-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="a5953-198">Zveřejněním úložiště odkazy z jednotky práce, kterou zajišťujeme jednu jednotku práce objektu má možnost sledovat všechny entity materializovaného během obchodní transakce.</span><span class="sxs-lookup"><span data-stu-id="a5953-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="a5953-199">Implementace metody potvrzení pro skutečné pracovní jednotka je všechny kouzla sloučit změny v paměti s databází.</span><span class="sxs-lookup"><span data-stu-id="a5953-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="a5953-200">Zadaný odkaz na IUnitOfWork, kód provést změny načíst z jednoho nebo více úložišť pro obchodní objekty a uložte všechny změny pomocí atomické operace potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a5953-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="a5953-201">Vzor opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="a5953-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="a5953-202">Opožděné načtení názvu fowler vám používá k popisu "objekt, který nebude obsahovat všechna data potřebujete, ale ví, jak získat".</span><span class="sxs-lookup"><span data-stu-id="a5953-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="a5953-203">Transparentní opožděné načtení je důležité funkce mít při psaní kódu s možností intenzivního testování obchodní a práci s relační databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="a5953-204">Jako příklad zvažte následující kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="a5953-205">Jak se vyplní časové výkazy kolekce?</span><span class="sxs-lookup"><span data-stu-id="a5953-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="a5953-206">Existují dva možné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a5953-206">There are two possible answers.</span></span> <span data-ttu-id="a5953-207">Jedna odpověď je, že zaměstnanec úložiště, když se zobrazí výzva k načtení zaměstnanec, vydá dotaz pro načtení zaměstnanec spolu s informace o přidružené čas kartě zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="a5953-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="a5953-208">V relačních databázích to obvykle vyžaduje dotazy s klauzulí JOIN a může vést k načtení více informací, než aplikace potřebuje.</span><span class="sxs-lookup"><span data-stu-id="a5953-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="a5953-209">Co když aplikace potřebuje nikdy dotýkat vlastnost časové výkazy?</span><span class="sxs-lookup"><span data-stu-id="a5953-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="a5953-210">Druhá odpověď je načíst vlastnost časové výkazy "na vyžádání".</span><span class="sxs-lookup"><span data-stu-id="a5953-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="a5953-211">Toto opožděné načtení je implicitní a pro obchodní logiku transparentní, protože kód se nevyvolá speciální rozhraní API se načíst informace o čas kartě.</span><span class="sxs-lookup"><span data-stu-id="a5953-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="a5953-212">Kód předpokládá, že čas kartě informace jsou k dispozici v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a5953-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="a5953-213">Je součástí opožděné načtení, která v podstatě runtime zachycení volání metod některé magic.</span><span class="sxs-lookup"><span data-stu-id="a5953-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="a5953-214">Prověřuje zachycovací kód je zodpovědná za mluvit k databázi a načítání informací o čas kartě a ponechání zdarma na obchodní logiku obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="a5953-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="a5953-215">Tento opožděné načtení magic umožňuje obchodní kód samotné izolovat operace načítání dat a za následek více možností intenzivního testování kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="a5953-216">Nevýhodou opožděné načtení je, že když aplikace *nemá* potřebují informace o čas kartě kód se spustí další dotaz.</span><span class="sxs-lookup"><span data-stu-id="a5953-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="a5953-217">To není žádný problém pro mnoho aplikací, ale výkon citlivých aplikací nebo aplikací počet objektů zaměstnance ve smyčce a spouštějícího dotaz k načtení času karty při každé iteraci smyčky (problém často označuje jako N + 1 dotaz problém), přetáhněte je opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="a5953-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="a5953-218">V těchto scénářích aplikace může být vhodné například načíst informace o čas kartě nejefektivnějším způsobem je to možné.</span><span class="sxs-lookup"><span data-stu-id="a5953-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="a5953-219">Naštěstí uvidíme jak EF4 podporuje obě implicitní opožděné načtení a efektivní eager načte přesunout do další části a implementovat tyto vzory.</span><span class="sxs-lookup"><span data-stu-id="a5953-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="a5953-220">Implementace vzorů s rozhraním Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a5953-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="a5953-221">Dobrou zprávou je, že jsou jednoduché implementace s EF4 všechny vzory návrhu, které jsme je popsáno v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="a5953-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="a5953-222">K předvedení budeme používat jednoduchou aplikaci ASP.NET MVC pro úpravy a zobrazí zaměstnancům a informace o jejich přidružených čas kartě.</span><span class="sxs-lookup"><span data-stu-id="a5953-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="a5953-223">Začneme s použitím následujících "prostý staré CLR objektů" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="a5953-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="a5953-224">Tyto definice třídy mírně změní jako podíváme na různé přístupy a funkce EF4, ale cílem je zajistit těchto tříd jako trvalého ignorant (PÍ) co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="a5953-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="a5953-225">Objekt PI neví *jak*, nebo dokonce *Pokud*, stát, že obsahuje umístěným uvnitř databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="a5953-226">Číslo PÍ a POCOs přejít těsná spolupráce s možností intenzivního testování softwaru.</span><span class="sxs-lookup"><span data-stu-id="a5953-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="a5953-227">Objekty POCO přístup jsou méně omezené, flexibilnější a usnadňuje testování, protože můžete pracovat bez databáze k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a5953-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="a5953-228">S POCOs na místě můžeme vytvořit Entity Data Model (EDM) v sadě Visual Studio (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="a5953-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="a5953-229">EDM Nebudeme je používat ke generování kódu pro naše entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="a5953-230">Místo toho chcete používat entity, které jsme lovingly vytvořit ručně.</span><span class="sxs-lookup"><span data-stu-id="a5953-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="a5953-231">Budeme používat jenom EDM pro generování naše schéma databáze a poskytněte metadata, která EF4 potřebuje k mapování objektů do databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![eftest_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="a5953-233">**Obrázek 1**</span><span class="sxs-lookup"><span data-stu-id="a5953-233">**Figure 1**</span></span>

<span data-ttu-id="a5953-234">Poznámka: Pokud chcete vyvíjet první EDM model, je možné generovat čištění, POCO kód z modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="a5953-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="a5953-235">Můžete to provést pomocí rozšíření sady Visual Studio 2010 poskytované týmem programovatelnosti Data.</span><span class="sxs-lookup"><span data-stu-id="a5953-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="a5953-236">Stáhnout rozšíření, spusťte Správce rozšíření v nabídce Nástroje v sadě Visual Studio, vyhledejte "POCO" (viz obrázek 2) online galerie šablon.</span><span class="sxs-lookup"><span data-stu-id="a5953-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="a5953-237">Nejsou k dispozici pro EF několik šablon POCO.</span><span class="sxs-lookup"><span data-stu-id="a5953-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="a5953-238">Další informace o použití šablony naleznete v části " [názorný postup: šablony objektů POCO pro Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="a5953-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![eftest_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="a5953-240">**Obrázek 2**</span><span class="sxs-lookup"><span data-stu-id="a5953-240">**Figure 2**</span></span>

<span data-ttu-id="a5953-241">Z tohoto POCO počáteční bod se podíváme na dva různé přístupy k testovatelného kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="a5953-242">První postup můžu volat EF přístup protože využívá abstrakce z rozhraní API Entity Framework pro implementaci jednotek práce a úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="a5953-243">Ve druhé metodě budeme vytvářet vlastní abstrakce vlastní úložiště a pak naleznete v tématu výhody a nevýhody obou těchto přístupů.</span><span class="sxs-lookup"><span data-stu-id="a5953-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="a5953-244">Začneme prozkoumání EF přístup.</span><span class="sxs-lookup"><span data-stu-id="a5953-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="a5953-245">Implementace zaměřenou na EF</span><span class="sxs-lookup"><span data-stu-id="a5953-245">An EF Centric Implementation</span></span>

<span data-ttu-id="a5953-246">Vezměte v úvahu následující akce kontroleru z projektu aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a5953-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="a5953-247">Akce načte objekt zaměstnance a vrátí výsledek, chcete-li zobrazit podrobné zobrazení jednotlivých zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="a5953-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="a5953-248">Je testovatelného kód?</span><span class="sxs-lookup"><span data-stu-id="a5953-248">Is the code testable?</span></span> <span data-ttu-id="a5953-249">Nejsou k dispozici alespoň dva testy by potřebujeme si ověřit akce chování.</span><span class="sxs-lookup"><span data-stu-id="a5953-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="a5953-250">Chtěli bychom nejprve, ověřte, že akce vrátí správné zobrazení – snadné testu.</span><span class="sxs-lookup"><span data-stu-id="a5953-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="a5953-251">Bude také chceme napsat test ověření akce načte správné zaměstnanců, a rádi bychom to provést bez spuštění kódu pro dotazování databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="a5953-252">Mějte na paměti, že chceme izolace testovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="a5953-253">Izolace zajistí, že není test nezdaří z důvodu chyby v kód přístupu k datům nebo konfigurace databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="a5953-254">Pokud se test nezdaří, budeme vědět, že máme chyby v logice kontroleru a ne v některé součásti nižší úrovně systému.</span><span class="sxs-lookup"><span data-stu-id="a5953-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="a5953-255">Účelem izolace potřebujeme některé abstrakce, jako je rozhraní, které jsme uvedené výše pro úložiště a jednotek práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="a5953-256">Mějte na paměti, které zprostředkovává mezi objekty domény a datová vrstva mapování je určený použitému vzoru.</span><span class="sxs-lookup"><span data-stu-id="a5953-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="a5953-257">V tomto scénáři EF4 *je* vrstvy data mapování a již poskytuje úložiště jako abstrakce s názvem IObjectSet&lt;T&gt; (z oboru názvů System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="a5953-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="a5953-258">Definice rozhraní vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="a5953-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="a5953-259">IObjectSet&lt;T&gt; splňuje požadavky na úložiště, protože se podobá kolekci objektů (prostřednictvím typu IEnumerable&lt;T&gt;) a poskytuje metody pro přidání a odebrání objektů z simulované kolekce.</span><span class="sxs-lookup"><span data-stu-id="a5953-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="a5953-260">Metody připojení a odpojení zpřístupňují další funkce EF4 rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5953-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="a5953-261">Chcete-li použít IObjectSet&lt;T&gt; jako rozhraní pro úložiště jsme jednotku práce abstrakce svázat dohromady úložiště potřebují.</span><span class="sxs-lookup"><span data-stu-id="a5953-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="a5953-262">Jeden konkrétní implementaci tohoto rozhraní bude komunikovat s SQL serverem a je snadné vytvořit horizontálních oddílů pomocí třídy objektu ObjectContext z EF4.</span><span class="sxs-lookup"><span data-stu-id="a5953-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="a5953-263">Třídě ObjectContext je skutečná jednotka práce v rozhraní API EF4.</span><span class="sxs-lookup"><span data-stu-id="a5953-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="a5953-264">Přenesení IObjectSet&lt;T&gt; život je stejně jednoduché jako volání metody CreateObjectSet objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="a5953-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="a5953-265">Na pozadí rozhraní použije metadata jsme součástí modelu EDM vytvoří konkrétní ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="a5953-266">Budeme držet se vrátí IObjectSet&lt;T&gt; rozhraní, protože může pomoci zachovat testovatelnosti v klientském kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="a5953-267">Tento konkrétní implementaci je užitečná v produkčním prostředí, ale musíme zaměřit jak použijeme naše IUnitOfWork abstrakce usnadňuje testování.</span><span class="sxs-lookup"><span data-stu-id="a5953-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="a5953-268">Testu zdvojnásobí</span><span class="sxs-lookup"><span data-stu-id="a5953-268">The Test Doubles</span></span>

<span data-ttu-id="a5953-269">K izolování akce kontroleru potřebujeme možnost přepínat mezi skutečné jednotku práce (se opírá o objektu ObjectContext) a test double nebo "falešnou" pracovní jednotka (provádění operací v paměti).</span><span class="sxs-lookup"><span data-stu-id="a5953-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="a5953-270">Běžným přístupem k provedení tohoto typu přepínání nebudou moci kontroler MVC vytvořit instanci určitou jednotku práce, ale místo toho pass jednotku práce do kontroleru jako parametr konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="a5953-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="a5953-271">Výše uvedený kód je příkladem vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="a5953-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="a5953-272">Nepovolit jsme řadič vytvoří jeho závislostí (jednotku práce) ale vložit závislost do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="a5953-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="a5953-273">V projektu aplikace MVC je běžné použití objekt pro vytváření vlastní kontroler v kombinaci s IOC (Inversion) ovládacího prvku kontejneru pro automatizaci vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="a5953-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="a5953-274">Tato témata jsou nad rámec tohoto článku, ale můžete si přečíst více podle následující odkazy na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="a5953-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="a5953-275">Falešné jednotka implementace pracovních, můžeme použít pro testování může vypadat nějak takto.</span><span class="sxs-lookup"><span data-stu-id="a5953-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="a5953-276">Všimněte si, že falešné jednotku práce zpřístupňuje vlastnost potvrzeny.</span><span class="sxs-lookup"><span data-stu-id="a5953-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="a5953-277">Někdy je užitečné pro přidání funkcí do falešné třídy, která usnadňují testování.</span><span class="sxs-lookup"><span data-stu-id="a5953-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="a5953-278">V tomto případě je snadné sledovat, pokud kód potvrdí určitou jednotku práce tak, že zkontrolujete vlastnost potvrzeny.</span><span class="sxs-lookup"><span data-stu-id="a5953-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="a5953-279">Budete potřebovat falešné IObjectSet&lt;T&gt; držet objekty pro zaměstnance a časové karty v paměti.</span><span class="sxs-lookup"><span data-stu-id="a5953-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="a5953-280">Poskytujeme jedna implementace použití obecných typů.</span><span class="sxs-lookup"><span data-stu-id="a5953-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="a5953-281">Tento test double deleguje většinu práce na základní HashSet&lt;T&gt; objektu.</span><span class="sxs-lookup"><span data-stu-id="a5953-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="a5953-282">Poznámka: Tento IObjectSet&lt;T&gt; vyžaduje, aby obecná omezení vynucení T jako třídu (typ odkazu) a také vynutí nám implementující rozhraní IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="a5953-283">Je snadné vytvořit kolekci v paměti se zobrazí jako položku IQueryable&lt;T&gt; pomocí standardního operátoru LINQ AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="a5953-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="a5953-284">Testy</span><span class="sxs-lookup"><span data-stu-id="a5953-284">The Tests</span></span>

<span data-ttu-id="a5953-285">Tradiční jednotkové testy budou používat jeden testovací třídy k uložení všech testů pro všechny akce v jeden kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="a5953-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="a5953-286">Nám můžete napsat tyto testy nebo jakéhokoli typu testování částí pomocí paměti napodobenin jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a5953-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="a5953-287">Ale pro účely tohoto článku, který jsme se vyhnete monolitické otestovat přístup pomocí třídy a místo toho skupiny testech a zaměřte se na konkrétní funkce.</span><span class="sxs-lookup"><span data-stu-id="a5953-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="a5953-288">Například "Vytvoření nového zaměstnance" může být funkce, které chceme otestovat, takže použijeme jednu testovací třídě ověření jednoho kontroleru akce slouží k vytváření nového zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="a5953-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="a5953-289">Je běžné nastavení kódu, kterou potřebujeme pro tyto podrobné testovací třídy.</span><span class="sxs-lookup"><span data-stu-id="a5953-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="a5953-290">Například vždy potřebujeme vytvořit naše úložiště v paměti a falešné jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="a5953-291">Potřebujeme taky instance kontroleru zaměstnance s falešnou jednotku práce vloženy.</span><span class="sxs-lookup"><span data-stu-id="a5953-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="a5953-292">Tento společný kód instalace zveřejníme napříč testovacích tříd pomocí základní třídy.</span><span class="sxs-lookup"><span data-stu-id="a5953-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="a5953-293">"Místo objektu" používáme v základní třídě je jeden běžný vzor pro vytvoření testovací data.</span><span class="sxs-lookup"><span data-stu-id="a5953-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="a5953-294">Místo objektu obsahuje metody pro vytváření objektů pro vytvoření instance entity testu pro použití napříč více testů komunikací.</span><span class="sxs-lookup"><span data-stu-id="a5953-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="a5953-295">Můžeme použít EmployeeControllerTestBase jako základní třída pro řadu zařízení test (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="a5953-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="a5953-296">Každý testovací přípravek testovat konkrétní kontroler akce.</span><span class="sxs-lookup"><span data-stu-id="a5953-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="a5953-297">Například jeden testovací přípravek se zaměří na testování akce vytvoření využitých požadavek HTTP GET (Chcete-li zobrazit zobrazení pro vytváření zaměstnanci) a jiné testovacího přípravku se zaměřuje na akce vytvoření použít v požadavku HTTP POST (abyste mohli informace získané při uživatel muset vytvořit zaměstnanci).</span><span class="sxs-lookup"><span data-stu-id="a5953-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="a5953-298">Jednotlivé odvozené třídy je pouze za instalaci potřebných v jeho konkrétním kontextu a k poskytování kontrolních výrazů, třeba ověřit výsledky jeho kontextu konkrétní testovací.</span><span class="sxs-lookup"><span data-stu-id="a5953-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![eftest_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="a5953-300">**Obrázek 3**</span><span class="sxs-lookup"><span data-stu-id="a5953-300">**Figure 3**</span></span>

<span data-ttu-id="a5953-301">Zásady vytváření a testování styl okomentovat není vyžadováno pro testovatelného kód – je jenom jedním z přístupů.</span><span class="sxs-lookup"><span data-stu-id="a5953-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="a5953-302">Obrázek 4 ukazuje, že testy běžící v Resharper mozky Jet test runner modulu plug-in pro Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a5953-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![eftest_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="a5953-304">**Obrázek 4**</span><span class="sxs-lookup"><span data-stu-id="a5953-304">**Figure 4**</span></span>

<span data-ttu-id="a5953-305">Základní třída pro zpracování kódu sdílené nastavení jednotkové testy pro každou akci kontroleru jsou malé a usnadňují zápis.</span><span class="sxs-lookup"><span data-stu-id="a5953-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="a5953-306">Testy se spustí rychle (protože jsme se provádění operací v paměti) a by neměla selhat z důvodu nesouvisejících infrastruktury nebo životního prostředí, (protože jednotky v rámci testu jsme jste izolovaný režim).</span><span class="sxs-lookup"><span data-stu-id="a5953-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="a5953-307">V těchto testech základní třída odvádí většinu práce Instalační program.</span><span class="sxs-lookup"><span data-stu-id="a5953-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="a5953-308">Mějte na paměti, že konstruktor základní třídy vytvoří úložiště v paměti, falešné jednotku práce a instance třídy EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="a5953-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="a5953-309">Testovací třídy je odvozen z této základní třídy a se zaměřuje na konkrétních podrobnostech testování metody Create.</span><span class="sxs-lookup"><span data-stu-id="a5953-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="a5953-310">V tomto případě specifika vaří "uspořádat, fungovat a kontrolní výraz" kroky, které se zobrazí v testování postup:</span><span class="sxs-lookup"><span data-stu-id="a5953-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="a5953-311">Vytvořte objekt Novýzaměstnanec pro simulaci příchozí data.</span><span class="sxs-lookup"><span data-stu-id="a5953-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="a5953-312">Vyvolání akce vytvoření EmployeeController a předejte mu Novýzaměstnanec.</span><span class="sxs-lookup"><span data-stu-id="a5953-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="a5953-313">Ověřte, že akce vytvoření vytváří očekávané výsledky (zaměstnance se zobrazí v úložišti).</span><span class="sxs-lookup"><span data-stu-id="a5953-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="a5953-314">Co jsme vytvořili umožňuje některé z akcí EmployeeController testování.</span><span class="sxs-lookup"><span data-stu-id="a5953-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="a5953-315">Například při psaní testů pro akce indexu řadiče zaměstnance jsme může dědit ze základní třídy testu k navázání stejnou základní verzi pro naše testy.</span><span class="sxs-lookup"><span data-stu-id="a5953-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="a5953-316">Základní třída znovu vytvoří úložiště v paměti, falešné jednotku práce a instance EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="a5953-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="a5953-317">Testy pro akce indexu stačí soustředit se na vyvolání akce Index a testování kvality modelu akce vrátí.</span><span class="sxs-lookup"><span data-stu-id="a5953-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="a5953-318">Testy jsme vytváření pomocí rozhraní fakes v paměti jsou orientované směrem k testování *stavu* softwaru.</span><span class="sxs-lookup"><span data-stu-id="a5953-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="a5953-319">Například při testování chcete zkontrolovat stav úložiště po spuštění akce vytvoření – akce vytvoření úložiště uchování nového zaměstnance?</span><span class="sxs-lookup"><span data-stu-id="a5953-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="a5953-320">Dále podíváme na základě interakce testování.</span><span class="sxs-lookup"><span data-stu-id="a5953-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="a5953-321">Testování na základě interakce se zeptá, pokud testovaný kód správné metody u našich objektů a předají správné parametry.</span><span class="sxs-lookup"><span data-stu-id="a5953-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="a5953-322">Teď přejdeme na obálek jiné vzoru návrhu – opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="a5953-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="a5953-323">Předběžné načítání a opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="a5953-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="a5953-324">V určitém okamžiku v rozhraní ASP.NET MVC přidružené aplikace, kterou jsme možná budete chtít zobrazit informace o zaměstnance a zahrnout zaměstnance čas karty.</span><span class="sxs-lookup"><span data-stu-id="a5953-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="a5953-325">Například můžeme mít čas kartě Souhrnná zobrazení, která zobrazuje název zaměstnance a celkový počet karet času v systému.</span><span class="sxs-lookup"><span data-stu-id="a5953-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="a5953-326">Existuje několik strategií, které můžete provést k implementaci této funkce.</span><span class="sxs-lookup"><span data-stu-id="a5953-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="a5953-327">Projekce</span><span class="sxs-lookup"><span data-stu-id="a5953-327">Projection</span></span>

<span data-ttu-id="a5953-328">Jedním z přístupů snadno vytvořit souhrn je vytvořit vyhrazený pro informace, které chcete zobrazit v zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="a5953-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="a5953-329">V tomto scénáři může být model vypadat nějak takto.</span><span class="sxs-lookup"><span data-stu-id="a5953-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="a5953-330">Všimněte si, že EmployeeSummaryViewModel není entita – jinými slovy není něco, co chcete zachovat v databázi.</span><span class="sxs-lookup"><span data-stu-id="a5953-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="a5953-331">Pouze budeme Tato třída slouží k nejprve přesunout data do zobrazení silného typu způsobem.</span><span class="sxs-lookup"><span data-stu-id="a5953-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="a5953-332">Model zobrazení se podobá příchozí přenos dat (DTO) objektu protože neobsahuje žádné chování (žádné metody) – jenom vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a5953-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="a5953-333">Vlastnosti bude obsahovat data, která potřebujeme k přesunutí.</span><span class="sxs-lookup"><span data-stu-id="a5953-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="a5953-334">Je snadné vytvořit instanci tohoto zobrazení modelu pomocí LINQ na standardní projekce operátoru – vyberte operátor.</span><span class="sxs-lookup"><span data-stu-id="a5953-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="a5953-335">Existují dvě významné funkce do kódu uvedeného výše.</span><span class="sxs-lookup"><span data-stu-id="a5953-335">There are two notable features to the above code.</span></span> <span data-ttu-id="a5953-336">Nejprve – kód je snadné testování, protože je stále snadno sledovat a izolaci.</span><span class="sxs-lookup"><span data-stu-id="a5953-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="a5953-337">Vyberte operátor, který funguje stejně dobře s naší fakes v paměti stejně jako před skutečné jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="a5953-338">Druhá významné funkce je, jak kód umožňuje EF4 ke generování jednoho efektivní dotazování shromažďovat informace o zaměstnance a čas kartě společně.</span><span class="sxs-lookup"><span data-stu-id="a5953-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="a5953-339">Informace o zaměstnancích a informace o čas kartě jste načten do stejného objektu bez použití jakékoli speciální rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5953-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="a5953-340">Kód vyjádřena pouze informace, které vyžaduje použití standardní operátory LINQ, které využívají zdroje dat v paměti, jakož i vzdálené zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="a5953-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="a5953-341">Bylo možné převést stromy výrazů generovaných dotazů LINQ a C EF4\# kompilátoru do jednoho a efektivní dotazu T-SQL.</span><span class="sxs-lookup"><span data-stu-id="a5953-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="a5953-342">Existují jiné situace, když jsme nechcete pracovat pomocí zobrazení modelu nebo objekt DTO, ale o skutečné entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="a5953-343">Když jsme si vědomi, potřebujeme, aby zaměstnanec *a* zaměstnance čas karty, které se nám můžete například načíst související data v nenápadné a efektivním způsobem.</span><span class="sxs-lookup"><span data-stu-id="a5953-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="a5953-344">Explicitní předběžné načítání</span><span class="sxs-lookup"><span data-stu-id="a5953-344">Explicit Eager Loading</span></span>

<span data-ttu-id="a5953-345">Až budeme chtít například načíst informace o související entity, které potřebujeme některé mechanismus pro obchodní logiky (nebo v tomto scénáři logice kontroleru akce) vyjádřit svůj chce úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="a5953-346">EF4 ObjectQuery&lt;T&gt; třída definuje metodu zahrnout k určení související objekty, které chcete načíst během dotazu.</span><span class="sxs-lookup"><span data-stu-id="a5953-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="a5953-347">Mějte na paměti objektu ObjectContext EF4 zpřístupňuje entity přes konkrétní ObjectSet&lt;T&gt; třída, která dědí z ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="a5953-348">Kdybychom používali ObjectSet&lt;T&gt; odkazů v našich akce kontroleru nám napsat následující kód k určení nemůžou dočkat, až zatížení informace o čas kartě pro každý zaměstnanec.</span><span class="sxs-lookup"><span data-stu-id="a5953-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="a5953-349">Ale vzhledem k tomu, že se pokoušíme zachovat našeho kódu možností intenzivního testování jsme nevystavujete ObjectSet&lt;T&gt; z mimo skutečné jednotku práce třídy.</span><span class="sxs-lookup"><span data-stu-id="a5953-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="a5953-350">Místo toho spoléháme IObjectSet&lt;T&gt; rozhraní, která se snadněji falšování, ale IObjectSet&lt;T&gt; nedefinuje metodu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="a5953-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="a5953-351">Výhodou LINQ je, že můžeme vytvořit vlastní operátor zahrnout.</span><span class="sxs-lookup"><span data-stu-id="a5953-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="a5953-352">Všimněte si, že tento operátor Include je definován jako rozšiřující metody pro položku IQueryable&lt;T&gt; místo IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="a5953-353">Tento produkt nám nabízí možnost používat metodu s širší škálu možných typů, včetně IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;a objekt ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="a5953-354">V případě, že základní sekvence není originální ObjectQuery EF4&lt;T&gt;, pak není nezpůsobily žádné potíže, provádí a zahrnout operátor je no-op.</span><span class="sxs-lookup"><span data-stu-id="a5953-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="a5953-355">Pokud základní pořadí *je* ObjectQuery&lt;T&gt; (nebo odvozené z ObjectQuery&lt;T&gt;), pak bude EF4 najdete v našich požadavků pro další data a formulovali správné SQL dotaz.</span><span class="sxs-lookup"><span data-stu-id="a5953-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="a5953-356">Pomocí tohoto nového operátoru na místě můžete explicitně požadujeme nemůžou dočkat, až zatížení informace o čas kartě z úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="a5953-357">Při spuštění proti skutečné ObjectContext, vytvoří kód následující jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="a5953-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="a5953-358">Dotaz shromažďuje dostatek informací z databáze v jedné cesty k sloučit objekty zaměstnance a plně naplnit jejich časové výkazy vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a5953-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="a5953-359">Dobré zprávy je, že zůstává plně testovatelného kód do metody akce.</span><span class="sxs-lookup"><span data-stu-id="a5953-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="a5953-360">Nemusíme poskytovat všechny další funkce pro naše napodobeniny pro podporu operátor zahrnout.</span><span class="sxs-lookup"><span data-stu-id="a5953-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="a5953-361">Chybná je, že jsme museli použít operátor zahrnout uvnitř chceme zajistit trvalost ignorant kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="a5953-362">Toto je typickým příkladem druhu kompromisy, které potřebujete k vyhodnocení při sestavování testovatelného kód.</span><span class="sxs-lookup"><span data-stu-id="a5953-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="a5953-363">Existují situace, kdy je třeba dát trvalost obavy nevracení mimo abstrakce úložiště pro splnění cílů výkonu.</span><span class="sxs-lookup"><span data-stu-id="a5953-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="a5953-364">Alternativa k předběžné načítání je opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="a5953-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="a5953-365">Děláme opožděné načtení znamená, že *není* naše obchodní kód explicitně oznamujeme požadavek přidružená data potřebovat.</span><span class="sxs-lookup"><span data-stu-id="a5953-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="a5953-366">Místo toho používáme naše entity v aplikaci a pokud další data je potřeba Entity Framework se načtou data na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="a5953-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="a5953-367">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="a5953-367">Lazy Loading</span></span>

<span data-ttu-id="a5953-368">Je snadné Představte si situaci, kdy nevíme, co bude potřebovat dat je konkrétní obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="a5953-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="a5953-369">Víme může logika potřebuje objekt zaměstnanců, ale společnost Microsoft může Nepodmíněný skok do různých pracovních cesty, kde některé z těchto cest vyžadují informace o čas kartě od zaměstnance a některé nikoli.</span><span class="sxs-lookup"><span data-stu-id="a5953-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="a5953-370">Scénáře, jako jsou to jsou ideální pro implicitní opožděné načtení vzhledem k tomu, že data se zobrazí kouzelného na podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a5953-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="a5953-371">Opožděné načtení, označované také jako odložené načítání, umístěte některé požadavky na na našich objekty entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="a5953-372">POCOs s neznalosti true trvalosti by čelí všechny požadavky z vrstvy trvalosti, ale true trvalost neznalosti se v podstatě nedají dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="a5953-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="a5953-373">Místo toho měříme neznalosti persistence v relativní stupňů.</span><span class="sxs-lookup"><span data-stu-id="a5953-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="a5953-374">Bylo by unfortunate, pokud jsme museli dědit ze základní třídu orientované trvalého nebo použít specializované kolekce k dosažení opožděné načtení POCOs.</span><span class="sxs-lookup"><span data-stu-id="a5953-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="a5953-375">Naštěstí EF4 má teď míň obtěžující řešení.</span><span class="sxs-lookup"><span data-stu-id="a5953-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="a5953-376">Prakticky jiným způsobem nezjistitelné</span><span class="sxs-lookup"><span data-stu-id="a5953-376">Virtually Undetectable</span></span>

<span data-ttu-id="a5953-377">Při použití objektů POCO, EF4 může dynamicky generovat runtime proxy pro entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="a5953-378">Tyto servery proxy nezaregistroval zabalení materializovaného POCOs a poskytují získat další služby tím, že zachytává každou vlastnost a nastavte operace k provedení další práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="a5953-379">Jeden konkrétní služby se opožděné načtení funkce, kterou jsme hledali.</span><span class="sxs-lookup"><span data-stu-id="a5953-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="a5953-380">Jiná služba je mechanismus efektivního sledování změn, které můžete zaznamenat, když se program změní hodnoty vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="a5953-381">Seznam změn se používá v objektu ObjectContext během metodu SaveChanges zachovat všechny změny entity pomocí příkazů UPDATE.</span><span class="sxs-lookup"><span data-stu-id="a5953-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="a5953-382">Pro tyto servery proxy pro práci ale potřebují způsob, jak integrovat do vlastnosti get a množinové operace na entitu a servery proxy dosažení tohoto cíle, tak, že přepíšete virtuální členy.</span><span class="sxs-lookup"><span data-stu-id="a5953-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="a5953-383">Proto pokud chceme zavést implicitní opožděné načtení a efektivního sledování změn musíme přejít zpět na naše POCO definice tříd a označit jako virtuální.</span><span class="sxs-lookup"><span data-stu-id="a5953-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="a5953-384">Nadále budeme moct říct, že entita zaměstnanců je většinou trvalost ignorant.</span><span class="sxs-lookup"><span data-stu-id="a5953-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="a5953-385">Jediným požadavkem je určený virtuální členy a to nemá vliv na možnosti testování kódu.</span><span class="sxs-lookup"><span data-stu-id="a5953-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="a5953-386">Nemusíme odvodit z jakékoli speciální základní třídy, nebo můžete použít speciální vyhrazené opožděné načtení kolekce.</span><span class="sxs-lookup"><span data-stu-id="a5953-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="a5953-387">Jak ukazuje kód, všechny třídy implementace rozhraní ICollection&lt;T&gt; je k dispozici pro uložení související entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="a5953-388">Je také jeden menší změnu, kterou musíme mít v naší jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="a5953-389">Opožděné načtení je *vypnout* ve výchozím nastavení při práci přímo s objektem ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="a5953-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="a5953-390">Existuje vlastnost, kterou jsme můžete nastavit na vlastnost ContextOptions pro povolení odložené načítání a my chceme umožnit opožděné načtení všude, kde nastavte tuto vlastnost uvnitř náš skutečný pracovní jednotka.</span><span class="sxs-lookup"><span data-stu-id="a5953-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="a5953-391">S implicitní opožděné načtení povoleno, kód aplikace můžete použít zaměstnance a zaměstnance související čas karty přitom blissfully vědět o práce potřebné pro EF načíst doplňující data.</span><span class="sxs-lookup"><span data-stu-id="a5953-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="a5953-392">Opožděné načtení usnadňuje zápis kódu aplikace a proxy magic kód zůstane zcela testovatelné.</span><span class="sxs-lookup"><span data-stu-id="a5953-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="a5953-393">V paměti fakes jednotku práce můžete jednoduše předběžné načtení falešné entity se přidružená data v případě potřeby během testu.</span><span class="sxs-lookup"><span data-stu-id="a5953-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="a5953-394">V tuto chvíli jsme vám zapnout naši pozornost od vytváření úložišť pomocí IObjectSet&lt;T&gt; a podívejte se na abstrakce skrýt všechny projevy rozhraní trvalosti.</span><span class="sxs-lookup"><span data-stu-id="a5953-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="a5953-395">Vlastní úložiště</span><span class="sxs-lookup"><span data-stu-id="a5953-395">Custom Repositories</span></span>

<span data-ttu-id="a5953-396">Převedou jsme nejprve jednotek práce vzoru návrhu v tomto článku jsme zajistili část ukázkového kódu jak může vypadat jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="a5953-397">Umožňuje znovu k dispozici tento původního nápadu pomocí zaměstnance a zaměstnance čas kartě scénáře, který jsme pracovali s.</span><span class="sxs-lookup"><span data-stu-id="a5953-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="a5953-398">Hlavní rozdíl mezi tuto jednotku práce a jednotku práce jsme vytvořili v předchozí části je, jak tuto jednotku práce nepoužívá žádné abstrakce z rozhraní EF4 (neexistuje žádný IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="a5953-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="a5953-399">IObjectSet&lt;T&gt; funguje také jako rozhraní úložiště, ale rozhraní API zveřejňuje nemusí dokonale odpovídaly potřebám naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5953-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="a5953-400">V této nadcházející přístup jsme bude představovat úložiště pomocí vlastní IRepository&lt;T&gt; abstrakce.</span><span class="sxs-lookup"><span data-stu-id="a5953-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="a5953-401">Mnoho vývojářů, kteří sledují založený na testování návrhu, návrh na základě chování a návrhu na základě domény metodologie raději IRepository&lt;T&gt; přístup z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="a5953-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="a5953-402">První, IRepository&lt;T&gt; rozhraní představuje vrstvu "odolnou proti poškození".</span><span class="sxs-lookup"><span data-stu-id="a5953-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="a5953-403">Jak popsal Eric Evans v knize jeho domény Driven Design vrstvu odolnou proti poškození udržuje kódu domény od infrastruktury rozhraní API, například trvalost rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a5953-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="a5953-404">Za druhé můžou vývojáři vytvářet metody do úložiště, které splňují konkrétní požadavky aplikace (při zjištění při psaní testů).</span><span class="sxs-lookup"><span data-stu-id="a5953-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="a5953-405">Například může často Potřebujeme najít jednu entitu pomocí hodnotou ID, takže můžeme přidat metodu FindById rozhraní úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="a5953-406">Naše IRepository&lt;T&gt; definice bude vypadat nějak takto.</span><span class="sxs-lookup"><span data-stu-id="a5953-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="a5953-407">Všimněte si, že nám budete zase pomocí položku IQueryable&lt;T&gt; rozhraní ke zveřejnění kolekce entit.</span><span class="sxs-lookup"><span data-stu-id="a5953-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="a5953-408">Položka IQueryable&lt;T&gt; umožňuje stromům výrazů LINQ tok do EF4 zprostředkovatele a poskytnout poskytovateli získat holistický pohled dotazu.</span><span class="sxs-lookup"><span data-stu-id="a5953-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="a5953-409">Druhou možností je vrátit IEnumerable&lt;T&gt;, což znamená, že zprostředkovatele EF4 LINQ zobrazí pouze výrazy vytvořené v rámci služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="a5953-410">Jakékoli seskupení, řazení a projekce provedené mimo úložiště nebude obsahovat do příkazu SQL, které se odesílají do databáze, což může snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="a5953-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="a5953-411">Na druhé straně úložiště vrací pouze IEnumerable&lt;T&gt; výsledky se nikdy vás překvapí pomocí nového příkazu SQL.</span><span class="sxs-lookup"><span data-stu-id="a5953-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="a5953-412">Oba přístupy bude fungovat a oba přístupy zůstanou testovatelné.</span><span class="sxs-lookup"><span data-stu-id="a5953-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="a5953-413">Je jednoduché zajistit jedna implementace položky IRepository&lt;T&gt; rozhraní pomocí obecných typů a rozhraní API EF4 objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="a5953-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="a5953-414">IRepository&lt;T&gt; přístup poskytuje některé další kontrolu nad naší dotazy protože klient má vyvolat metodu k získání k entitě.</span><span class="sxs-lookup"><span data-stu-id="a5953-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="a5953-415">Uvnitř metody, kterou může zajišťuje další kontroly a operátory LINQ vynutit omezení použití.</span><span class="sxs-lookup"><span data-stu-id="a5953-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="a5953-416">Všimněte si, že rozhraní má dvě omezení u obecného typu parametru.</span><span class="sxs-lookup"><span data-stu-id="a5953-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="a5953-417">Prvním omezením je vyžadované ObjectSet změny třídy nevýhody chuti&lt;T&gt;, a druhé omezení vynutí naše entity k implementaci IEntity – abstrakce vytvořené pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a5953-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="a5953-418">Rozhraní IEntity vynutí entity, které mají čitelnou vlastnost Id, a pak můžete použít tuto vlastnost v metodě FindById.</span><span class="sxs-lookup"><span data-stu-id="a5953-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="a5953-419">IEntity je definován následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="a5953-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="a5953-420">IEntity může považovat za malý porušení trvalost neznalosti od našich entity jsou nutné k implementaci tohoto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5953-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="a5953-421">Mějte na paměti neznalosti trvalosti je o kompromisy a pro mnoho funkcí FindById převáží omezení stanovené rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a5953-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="a5953-422">Rozhraní nemá žádný vliv na testovatelnost.</span><span class="sxs-lookup"><span data-stu-id="a5953-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="a5953-423">Vytvoření instance živé IRepository&lt;T&gt; vyžaduje objektu ObjectContext EF4 konkrétní jednotka implementace pracovních měli spravovat instance.</span><span class="sxs-lookup"><span data-stu-id="a5953-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="a5953-424">Používání vlastního úložiště</span><span class="sxs-lookup"><span data-stu-id="a5953-424">Using the Custom Repository</span></span>

<span data-ttu-id="a5953-425">Pomocí našeho vlastního úložiště není výrazně liší od používat úložiště založené na IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="a5953-426">Místo použití operátory LINQ přímo na vlastnost, budeme muset nejdřív jednu vyvolávat metody v úložišti a zkopírovat položku IQueryable&lt;T&gt; odkaz.</span><span class="sxs-lookup"><span data-stu-id="a5953-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="a5953-427">Všimněte si, že bude fungovat na vlastní zahrnout operátor, který jsme dříve implementovali beze změny.</span><span class="sxs-lookup"><span data-stu-id="a5953-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="a5953-428">V úložišti FindById metoda odebere duplicitní logiky z akce pokusu o načtení jedné entity.</span><span class="sxs-lookup"><span data-stu-id="a5953-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="a5953-429">Neexistuje žádný velký rozdíl možnosti testování dvě metody, které jsme jsme se zaměřili na.</span><span class="sxs-lookup"><span data-stu-id="a5953-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="a5953-430">Poskytujeme může falešné implementace IRepository&lt;T&gt; konkrétních tříd se opírá o HashSet sestavením&lt;zaměstnance&gt; – stejně jako fungují v poslední části.</span><span class="sxs-lookup"><span data-stu-id="a5953-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="a5953-431">Někteří vývojáři ale raději pomocí mock objektů a napodobení rozhraní objektu namísto sestavení fakes.</span><span class="sxs-lookup"><span data-stu-id="a5953-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="a5953-432">Podíváme se na používání mocks testovat naše implementace a diskutovat o rozdílech mezi mocks a fakes v další části.</span><span class="sxs-lookup"><span data-stu-id="a5953-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="a5953-433">Ve Visual Basicu s Mocks</span><span class="sxs-lookup"><span data-stu-id="a5953-433">Testing with Mocks</span></span>

<span data-ttu-id="a5953-434">Existují různé přístupy k vytváření jaké Martina Fowlera volání "test double".</span><span class="sxs-lookup"><span data-stu-id="a5953-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="a5953-435">Test double (např. film stunt double) je objekt, který vytváříte "vytvoření několika" pro skutečné produkční objekty během testů.</span><span class="sxs-lookup"><span data-stu-id="a5953-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="a5953-436">Úložiště v paměti, kterou jsme vytvořili jsou testu zdvojnásobí pro úložiště, které komunikují se SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a5953-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="a5953-437">Zaznamenali jsme na tom, jak používat tyto testu zdvojnásobí během testování částí kódu a rychlé spuštění testů.</span><span class="sxs-lookup"><span data-stu-id="a5953-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="a5953-438">Testu zdvojnásobí, kterou jsme vytvořili jste skutečný pracovní implementace.</span><span class="sxs-lookup"><span data-stu-id="a5953-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="a5953-439">Na pozadí každé z nich ukládá konkrétní kolekci objektů, a budou přidat a odebrat objekty z této kolekce, protože budeme pracovat s úložišti během testu.</span><span class="sxs-lookup"><span data-stu-id="a5953-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="a5953-440">Někteří vývojáři, jako je vytvářet jejich testu zdvojnásobí tímto způsobem – s skutečného kódu a pracovní implementace.</span><span class="sxs-lookup"><span data-stu-id="a5953-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="a5953-441">Tyto testovací zdvojnásobí jsou, čemu říkáme *napodobenin*.</span><span class="sxs-lookup"><span data-stu-id="a5953-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="a5953-442">Mají implementace práci, ale nejsou dostatečně skutečné pro použití v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a5953-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="a5953-443">Falešné úložiště není ve skutečnosti zapisovat do databáze.</span><span class="sxs-lookup"><span data-stu-id="a5953-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="a5953-444">Falešné server SMTP nebude ve skutečnosti odeslat e-mailovou zprávu v síti.</span><span class="sxs-lookup"><span data-stu-id="a5953-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="a5953-445">Mocks oproti napodobenin</span><span class="sxs-lookup"><span data-stu-id="a5953-445">Mocks versus Fakes</span></span>

<span data-ttu-id="a5953-446">Existuje jiný typ double označované jako test *napodobení*.</span><span class="sxs-lookup"><span data-stu-id="a5953-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="a5953-447">Napodobeniny mají pracovní implementace, jsou dostupné mocks žádnou implementaci.</span><span class="sxs-lookup"><span data-stu-id="a5953-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="a5953-448">S pomocí mock objektu rozhraní můžeme vytvořit tyto mock objektů za běhu a použít je jako testu zdvojnásobí.</span><span class="sxs-lookup"><span data-stu-id="a5953-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="a5953-449">V této části budeme používat open source napodobování framework Moq.</span><span class="sxs-lookup"><span data-stu-id="a5953-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="a5953-450">Tady je jednoduchý příklad použití Moq dynamicky vytvořit test double pro úložiště zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="a5953-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="a5953-451">Pro IRepository požádáme Moq&lt;zaměstnance&gt; implementace a vytvoří jednu dynamicky.</span><span class="sxs-lookup"><span data-stu-id="a5953-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="a5953-452">Abychom se mohli do objektu implementace IRepository&lt;zaměstnance&gt; přístupem k vlastnosti objektu model&lt;T&gt; objektu.</span><span class="sxs-lookup"><span data-stu-id="a5953-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="a5953-453">Je tento vnitřní objekt, který předáme do našich kontrolerů a nebude vědět, pokud je test double nebo skutečné úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="a5953-454">Stejným způsobem, jako jsme by volat metody pro objekt s skutečné implementaci jsme můžete vyvolávat metody v objektu.</span><span class="sxs-lookup"><span data-stu-id="a5953-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="a5953-455">Vás bude zajímat, mock úložiště bude tom, co jsme vyvolat metodu Add.</span><span class="sxs-lookup"><span data-stu-id="a5953-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="a5953-456">Protože neexistuje žádná implementace za mock objektu, přidat nemá žádný účinek.</span><span class="sxs-lookup"><span data-stu-id="a5953-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="a5953-457">Neexistuje žádná konkrétní kolekce na pozadí, jako jsme měli s fakes, kterou jsme napsali, tak se zahodí zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="a5953-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="a5953-458">Co o vrácené hodnotě FindById?</span><span class="sxs-lookup"><span data-stu-id="a5953-458">What about the return value of FindById?</span></span> <span data-ttu-id="a5953-459">V tomto případě mock objektu nepodporuje jediné, co můžete dělat, což je vrácená výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a5953-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="a5953-460">Protože jsme se vracející odkazový typ (zaměstnanci), vrácená hodnota je hodnota null.</span><span class="sxs-lookup"><span data-stu-id="a5953-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="a5953-461">Mocks může zvuk bezcenné. podstatné; Nicméně existují dvě další funkce mocks, které nebyly už jsme mluvili o.</span><span class="sxs-lookup"><span data-stu-id="a5953-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="a5953-462">Nejprve Moq framework zaznamenává všechna volání mock objektu.</span><span class="sxs-lookup"><span data-stu-id="a5953-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="a5953-463">Později v kódu můžete požádáme Moq Pokud kdokoli vyvolat metodu Add nebo pokud kdokoli volal metodu FindById.</span><span class="sxs-lookup"><span data-stu-id="a5953-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="a5953-464">Podíváme se, jak později můžete použít tuto funkci "černé políčko" záznamu v testech.</span><span class="sxs-lookup"><span data-stu-id="a5953-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="a5953-465">Druhá skvělé funkce je, jak můžete použít Moq programovat mock objektu s *očekávání*.</span><span class="sxs-lookup"><span data-stu-id="a5953-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="a5953-466">Očekávání informuje mock objektu, jak reagovat na daný zásahu.</span><span class="sxs-lookup"><span data-stu-id="a5953-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="a5953-467">Například jsme naprogramovat očekávání do našich model a určit, k vrácení objektu zaměstnance, když uživatel vyvolá FindById.</span><span class="sxs-lookup"><span data-stu-id="a5953-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="a5953-468">Rozhraní Moq používá rozhraní API instalační program a výrazy lambda programovat tyto očekávání.</span><span class="sxs-lookup"><span data-stu-id="a5953-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="a5953-469">V této ukázce požádáme Moq dynamicky vytvářet úložiště a pak budeme programovat úložiště s očekávání.</span><span class="sxs-lookup"><span data-stu-id="a5953-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="a5953-470">Očekává se říká mock objektu pro vrácení nového objektu zaměstnance s hodnotou Id 5, když někdo volá metodu FindById předáním hodnoty 5.</span><span class="sxs-lookup"><span data-stu-id="a5953-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="a5953-471">Tento test úspěšný, a jsme neměli muset sestavit úplnou implementaci pro falešné IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="a5953-472">Pojďme návštěvě testy, které jsme napsali dříve a opakovanou prací je, aby používaly mocks místo fakes.</span><span class="sxs-lookup"><span data-stu-id="a5953-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="a5953-473">Stejně jako dříve, použijeme základní třídy nastavit společné kusy infrastrukturu, kterou potřebujeme pro všechny kontroleru testů.</span><span class="sxs-lookup"><span data-stu-id="a5953-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="a5953-474">Instalační kód zůstane většinou stejné.</span><span class="sxs-lookup"><span data-stu-id="a5953-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="a5953-475">Namísto použití fakes, použijeme Moq k sestavení kompletních mock objektů.</span><span class="sxs-lookup"><span data-stu-id="a5953-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="a5953-476">Základní třída uspořádá mock jednotky práce vrátit mock úložiště, když kód volá vlastnost zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="a5953-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="a5953-477">Zbývající část mock nastavení bude probíhat uvnitř testu komunikací vyhrazený pro jednotlivé konkrétní scénáře.</span><span class="sxs-lookup"><span data-stu-id="a5953-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="a5953-478">Například testovací přípravek, akce indexu bude nastavení mock úložiště vrátila seznam zaměstnanců při akci vyvolá metodu FindAll mock úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="a5953-479">S výjimkou očekávání vypadat podobně jako testy, které jsme měli před testech.</span><span class="sxs-lookup"><span data-stu-id="a5953-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="a5953-480">Ale možnost záznamu mock Framework jsme můžete přistupovat ke testy z různých úhlu.</span><span class="sxs-lookup"><span data-stu-id="a5953-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="a5953-481">Podíváme se na tato nová Perspektiva v další části.</span><span class="sxs-lookup"><span data-stu-id="a5953-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="a5953-482">Stav oproti interakce testování</span><span class="sxs-lookup"><span data-stu-id="a5953-482">State versus Interaction Testing</span></span>

<span data-ttu-id="a5953-483">Existují různé techniky, které lze použít k testování softwaru pomocí mock objektů.</span><span class="sxs-lookup"><span data-stu-id="a5953-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="a5953-484">Jedním z přístupů je používání stavu testování, což je, co jsme udělali v tomto dokumentu zatím.</span><span class="sxs-lookup"><span data-stu-id="a5953-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="a5953-485">Stav na základě testování vytvoří kontrolní výrazy týkající se stavu softwaru.</span><span class="sxs-lookup"><span data-stu-id="a5953-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="a5953-486">V rámci poslední sady testů vyvolá metodu akce v kontroleru jsme provedli kontrolní výraz o modelu, že se že má sestavit.</span><span class="sxs-lookup"><span data-stu-id="a5953-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="a5953-487">Tady jsou některé příklady stavu testování:</span><span class="sxs-lookup"><span data-stu-id="a5953-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="a5953-488">Ověřte, že úložiště obsahuje nový objekt zaměstnance po vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a5953-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="a5953-489">Ověřte, že model obsahuje seznam všech zaměstnanců po indexu.</span><span class="sxs-lookup"><span data-stu-id="a5953-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="a5953-490">Ověřte, že úložiště neobsahuje daný zaměstnance po odstranění.</span><span class="sxs-lookup"><span data-stu-id="a5953-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="a5953-491">Další možností, zobrazí se vám pomocí mock objektů je ověřit *interakce*.</span><span class="sxs-lookup"><span data-stu-id="a5953-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="a5953-492">Při stavu na základě testování je tvrzení o stavu objektů, na základě interakce testování je tvrzení o interakci objekty.</span><span class="sxs-lookup"><span data-stu-id="a5953-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="a5953-493">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a5953-493">For example:</span></span>

-   <span data-ttu-id="a5953-494">Ověřte, že kontroler vyvolá metodu Add v úložišti při vytvoření spustí.</span><span class="sxs-lookup"><span data-stu-id="a5953-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="a5953-495">Ověřte, že kontroler vyvolá metodu FindAll v úložišti, když spustí Index.</span><span class="sxs-lookup"><span data-stu-id="a5953-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="a5953-496">Ověřte, že kontroler vyvolá jednotka metoda Commit se práce se uložit změny, když provádí úpravy.</span><span class="sxs-lookup"><span data-stu-id="a5953-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="a5953-497">Interakce testování často vyžaduje méně testovací data, protože jsme nejsou vyvrtání v rámci kolekce a ověření počty.</span><span class="sxs-lookup"><span data-stu-id="a5953-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="a5953-498">Například pokud víme, že akce Details vyvolá metodu FindById do úložiště se správnou hodnotu – potom akce pravděpodobně nepracuje správně.</span><span class="sxs-lookup"><span data-stu-id="a5953-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="a5953-499">Tuto skutečnost můžete ověřit bez nastavování žádná data testu má vrátit z FindById.</span><span class="sxs-lookup"><span data-stu-id="a5953-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="a5953-500">Jediným nastavením požadovaným ve výše uvedené testovací přípravek se nastavení poskytuje základní třídy.</span><span class="sxs-lookup"><span data-stu-id="a5953-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="a5953-501">Když jsme vyvolání akce kontroleru, zaznamená Moq interakce s mock úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="a5953-502">Pomocí ověřte API Moq, můžete požádáme Moq Pokud kontroler vyvolání FindById správnou hodnotu ID.</span><span class="sxs-lookup"><span data-stu-id="a5953-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="a5953-503">Pokud kontroler nevyvolal metodu nebo vyvolat metodu s hodnotou neočekávaný parametr, ověřte, zda metoda vyvolá výjimku a test se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="a5953-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="a5953-504">Tady je další příklad ověření, že akce vytvoření vyvolá potvrzení v aktuální jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="a5953-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="a5953-505">Jeden nebezpečí s testováním interakce je tendence k za určení interakce.</span><span class="sxs-lookup"><span data-stu-id="a5953-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="a5953-506">Možnost mock objektu, který chcete zaznamenat a ověřte, že všechny interakce s mock objektu neznamená test by měl pokusit ověřte všechny interakce s.</span><span class="sxs-lookup"><span data-stu-id="a5953-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="a5953-507">Některé interakce se podrobnosti implementace a měli byste ověřit jenom interakce *požadované* splňovat aktuální test.</span><span class="sxs-lookup"><span data-stu-id="a5953-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="a5953-508">Volba mezi mocks nebo fakes do značné míry závisí na systému, testování a osobní (nebo skupiny) předvolby.</span><span class="sxs-lookup"><span data-stu-id="a5953-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="a5953-509">Mock objektů může výrazně omezit množství kódu, je nutné implementovat testu zdvojnásobí, ale ne každý bude vyhovovat programování očekávání a ověření interakce.</span><span class="sxs-lookup"><span data-stu-id="a5953-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="a5953-510">Závěry</span><span class="sxs-lookup"><span data-stu-id="a5953-510">Conclusions</span></span>

<span data-ttu-id="a5953-511">V tomto dokumentu jsme jste jsme vám ukázali několik způsobů vytváření testovatelného kód při použití pro trvalost dat ADO.NET Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a5953-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="a5953-512">Můžeme využít integrované abstrakce jako IObjectSet&lt;T&gt;, nebo vytvořte vlastní abstrakce jako IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="a5953-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="a5953-513">V obou případech se POCO podpory v ADO.NET Entity Framework 4.0 umožňuje uživatelům tato abstrakce zůstanou trvalé ignorant a s možností intenzivního testování.</span><span class="sxs-lookup"><span data-stu-id="a5953-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="a5953-514">Další EF4 funkcí, jako jsou implicitní opožděné načtení umožňuje obchodních a aplikačních služeb kód fungovat bez starostí o podrobnosti relační datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="a5953-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="a5953-515">Nakonec jsou abstrakce, kterou jsme vytvořili snadno mock nebo falešný v rámci testů jednotek a používáme tyto testu zdvojnásobí zajistit rychlé spuštění, vysoce izolované a spolehlivé testy.</span><span class="sxs-lookup"><span data-stu-id="a5953-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="a5953-516">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="a5953-516">Additional Resources</span></span>

-   <span data-ttu-id="a5953-517">Robert C. Martin, " [principu jednotnou zodpovědnost](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="a5953-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="a5953-518">Martina Fowlera [katalog způsobů](http://www.martinfowler.com/eaaCatalog/index.html) z *vzory Enterprise Application Architecture*</span><span class="sxs-lookup"><span data-stu-id="a5953-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="a5953-519">Griffin Caprio " [injektáž závislostí](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="a5953-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="a5953-520">Blog programovatelnosti data, " [názorný postup: testu řízeného rozvoje s rozhraním Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="a5953-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="a5953-521">Blog programovatelnosti data, " [pomocí úložiště a jednotky pracovních vzorů s Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="a5953-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="a5953-522">Dave Astels " [BDD ÚVOD](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="a5953-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="a5953-523">Aaron Lázecký " [Představujeme počítač specifikace](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="a5953-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="a5953-524">Eric Lee " [BDD s použitím MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="a5953-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="a5953-525">Eric Evans " [návrhu na základě domény](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="a5953-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="a5953-526">Martina Fowlera " [Mocks nejsou zástupné procedury](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="a5953-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="a5953-527">Martina Fowlera " [testování Double](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="a5953-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="a5953-528">Jeremy Miller " [stavu a interakce testování](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="a5953-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="a5953-529">Moq</span><span class="sxs-lookup"><span data-stu-id="a5953-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="a5953-530">Životopis</span><span class="sxs-lookup"><span data-stu-id="a5953-530">Biography</span></span>

<span data-ttu-id="a5953-531">Scott Allen je členem technický pracovník na Pluralsightu a Zakladatel OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="a5953-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="a5953-532">Během 15 let vývoje softwaru komerční Scott pracoval na řešení zahrnující všechno od zařízení se systémem embedded 8 bitů na vysoce škálovatelné webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5953-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="a5953-533">Scott můžete oslovit na svém blogu na OdeToCode nebo na Twitteru pod [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="a5953-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
