---
title: Testování a Entity Framework 4,0 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181579"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="de784-102">Testování a Entity Framework 4,0</span><span class="sxs-lookup"><span data-stu-id="de784-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="de784-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="de784-103">Scott Allen</span></span>

<span data-ttu-id="de784-104">Publikováno: květen 2010</span><span class="sxs-lookup"><span data-stu-id="de784-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="de784-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="de784-105">Introduction</span></span>

<span data-ttu-id="de784-106">Tento dokument white paper popisuje a ukazuje, jak napsat kód testovatelné pomocí nástroje ADO.NET Entity Framework 4,0 a sady Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="de784-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="de784-107">Tento dokument se nepokouší soustředit na konkrétní metodologii testování, jako je například metoda založená na testu (TDD) nebo návrh na základě chování (BDD).</span><span class="sxs-lookup"><span data-stu-id="de784-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="de784-108">Místo toho se tento dokument zaměřuje na to, jak psát kód, který používá ADO.NET Entity Framework a zatím zůstává snadno izolovaný a testovaný a automatizovaný.</span><span class="sxs-lookup"><span data-stu-id="de784-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="de784-109">Podíváme se na běžné vzory návrhu, které usnadňují testování ve scénářích přístupu k datům, a dozvíte se, jak tyto vzory použít při použití architektury.</span><span class="sxs-lookup"><span data-stu-id="de784-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="de784-110">Také se podíváme na konkrétní funkce rozhraní, abyste viděli, jak tyto funkce můžou pracovat v testovatelné kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="de784-111">Co je testovatelné kód?</span><span class="sxs-lookup"><span data-stu-id="de784-111">What Is Testable Code?</span></span>

<span data-ttu-id="de784-112">Možnost ověřit část softwaru pomocí automatizovaných testů jednotek nabízí spoustu žádoucích výhod.</span><span class="sxs-lookup"><span data-stu-id="de784-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="de784-113">Každý ví, že dobré testy sníží počet softwarových vad v aplikaci a zvýší kvalitu aplikace, ale testy jednotek budou ještě mnohem mimo hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="de784-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="de784-114">Dobrá testovací sada umožňuje vývojovému týmu ušetřit čas a zůstat pod kontrolou softwaru, který vytváří.</span><span class="sxs-lookup"><span data-stu-id="de784-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="de784-115">Tým může provádět změny v existujícím kódu, Refaktorovat, přenavrhovat a restrukturovat software, aby splňoval nové požadavky, a současně přidat nové komponenty do aplikace a současně s vědomím, že testovací sada dokáže ověřit chování aplikace.</span><span class="sxs-lookup"><span data-stu-id="de784-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="de784-116">Testy jednotek jsou součástí cyklu rychlé zpětné vazby, která usnadňují změnu a zachovává udržovatelnost softwaru při zvyšování složitosti.</span><span class="sxs-lookup"><span data-stu-id="de784-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="de784-117">Testování částí se ale dodává s cenou.</span><span class="sxs-lookup"><span data-stu-id="de784-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="de784-118">Tým musí investovat čas na vytvoření a údržbu testování částí.</span><span class="sxs-lookup"><span data-stu-id="de784-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="de784-119">Množství úsilí potřebné k vytvoření těchto testů přímo souvisí s **testováním** základního softwaru.</span><span class="sxs-lookup"><span data-stu-id="de784-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="de784-120">Jak snadné je software k testování?</span><span class="sxs-lookup"><span data-stu-id="de784-120">How easy is the software to test?</span></span> <span data-ttu-id="de784-121">Tým, který navrhuje software s ohledem na testování, vytvoří efektivní testy rychleji než tým, který pracuje s testovatelné softwarem.</span><span class="sxs-lookup"><span data-stu-id="de784-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="de784-122">Společnost Microsoft navrhla ADO.NET Entity Framework 4,0 (EF4) s ohledem na testování.</span><span class="sxs-lookup"><span data-stu-id="de784-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="de784-123">To neznamená, že vývojáři budou psát testy jednotek proti samotnému kódu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="de784-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="de784-124">Místo toho cíle testování pro EF4 usnadňuje vytváření testovatelné kódu, který sestaví na rozhraní.</span><span class="sxs-lookup"><span data-stu-id="de784-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="de784-125">Předtím, než se podíváme na konkrétní příklady, je vhodné pochopit kvality testovatelné kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="de784-126">Vlastnosti testovatelné kódu</span><span class="sxs-lookup"><span data-stu-id="de784-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="de784-127">Kód, který se dá snadno testovat, se vždy zobrazí alespoň se dvěma vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="de784-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="de784-128">Nejprve je testovatelné kód snadno **sledovat**.</span><span class="sxs-lookup"><span data-stu-id="de784-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="de784-129">Pro určitou sadu vstupů by mělo být snadné sledovat výstup kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="de784-130">Například testování následující metody je snadné, protože metoda přímo vrací výsledek výpočtu.</span><span class="sxs-lookup"><span data-stu-id="de784-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="de784-131">Testování metody je obtížné, pokud metoda zapíše vypočítanou hodnotu do síťového soketu, databázové tabulky nebo souboru jako následující kód.</span><span class="sxs-lookup"><span data-stu-id="de784-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="de784-132">Test musí provést další práci, aby bylo možné načíst hodnotu.</span><span class="sxs-lookup"><span data-stu-id="de784-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="de784-133">V druhém případě se testovatelné kód snadno **izoluje**.</span><span class="sxs-lookup"><span data-stu-id="de784-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="de784-134">Pojďme použít následující pseudo kód jako špatný příklad kódu testovatelné.</span><span class="sxs-lookup"><span data-stu-id="de784-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="de784-135">Metodu je možné snadno sledovat – můžeme předat zásady pojištění a ověřit, zda vrácená hodnota odpovídá očekávanému výsledku.</span><span class="sxs-lookup"><span data-stu-id="de784-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="de784-136">K otestování této metody ale musíme mít nainstalovanou databázi se správným schématem a nakonfigurovat server SMTP pro případ, že se metoda pokusí odeslat e-mail.</span><span class="sxs-lookup"><span data-stu-id="de784-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="de784-137">Test jednotek chce pouze ověřit logiku výpočtu v rámci metody, ale test může selhat, protože e-mailový server je offline nebo proto, že databázový server byl přesunut.</span><span class="sxs-lookup"><span data-stu-id="de784-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="de784-138">Oba tyto chyby nesouvisejí s chováním, které test chce ověřit.</span><span class="sxs-lookup"><span data-stu-id="de784-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="de784-139">Chování je obtížné izolovat.</span><span class="sxs-lookup"><span data-stu-id="de784-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="de784-140">Vývojáři softwaru, kteří se snaží psát kód testovatelné, často usilují o udržení oddělení obav v kódu, který zapisuje.</span><span class="sxs-lookup"><span data-stu-id="de784-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="de784-141">Výše uvedená metoda by se měla soustředit na obchodní výpočty a podrobné informace o implementaci databáze a e-mailu pro ostatní komponenty.</span><span class="sxs-lookup"><span data-stu-id="de784-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="de784-142">Robert C. Martin volá tento princip zodpovědnosti.</span><span class="sxs-lookup"><span data-stu-id="de784-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="de784-143">Objekt by měl zapouzdřit jedinou, úzký zodpovědnost, jako je například výpočet hodnoty zásad.</span><span class="sxs-lookup"><span data-stu-id="de784-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="de784-144">Všechny ostatní práce databáze a oznámení by měly být zodpovědností nějakého jiného objektu.</span><span class="sxs-lookup"><span data-stu-id="de784-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="de784-145">Kód napsaný tímto způsobem je snazší izolovat, protože se zaměřuje na jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="de784-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="de784-146">V .NET máme abstrakce, které potřebujeme pro splnění principu jedné zodpovědnosti a zajištění izolace.</span><span class="sxs-lookup"><span data-stu-id="de784-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="de784-147">Můžeme použít definice rozhraní a vynutit, aby kód používal abstrakci rozhraní místo konkrétního typu.</span><span class="sxs-lookup"><span data-stu-id="de784-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="de784-148">Později v tomto dokumentu se dozvíte, jak metoda, jako je chybný příklad uvedený výše, může fungovat s rozhraními, která *vypadají* jako, budou komunikovat s databází.</span><span class="sxs-lookup"><span data-stu-id="de784-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="de784-149">V době testování však můžeme nahradit fiktivní implementaci, která nepracuje s databází, ale místo toho ukládá data do paměti.</span><span class="sxs-lookup"><span data-stu-id="de784-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="de784-150">Tato fiktivní implementace izoluje kód od nesouvisejících problémů v kódu pro přístup k datům nebo v konfiguraci databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="de784-151">Pro izolaci jsou k dispozici další výhody.</span><span class="sxs-lookup"><span data-stu-id="de784-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="de784-152">Obchodní kalkulace v poslední metodě by měla trvat jen několik milisekund, ale test sám může běžet několik sekund, než se kód rozhlásí kolem sítě a mluví na různé servery.</span><span class="sxs-lookup"><span data-stu-id="de784-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="de784-153">Testy jednotek by měly rychle běžet, aby se usnadnily malé změny.</span><span class="sxs-lookup"><span data-stu-id="de784-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="de784-154">Testy jednotek by také měly být opakované a neúspěšné, protože došlo k potížím s komponentou, která nesouvisí s testem.</span><span class="sxs-lookup"><span data-stu-id="de784-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="de784-155">Psaní kódu, který se snadno sleduje a izoluje znamená, že vývojáři budou mít snazší časová testování kódu, stráví méně času čekáním na provedení testů a důležitější je, že stráví méně času sledováním chyb, které neexistují.</span><span class="sxs-lookup"><span data-stu-id="de784-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="de784-156">Snad můžete vyhodnotit výhody testování a pochopit, jaké kvality se testovatelné kód projeví.</span><span class="sxs-lookup"><span data-stu-id="de784-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="de784-157">Chystáme se poznamenat, jak napsat kód, který spolupracuje s EF4, aby ušetřil data do databáze a bylo možné je snadno izolovat, ale nejdřív se zúžíme na diskuzi o tom, abychom probrali návrhy testovatelné pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="de784-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="de784-158">Vzory návrhu pro trvalost dat</span><span class="sxs-lookup"><span data-stu-id="de784-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="de784-159">Oba chybné příklady uvedené dříve obsahovaly příliš mnoho zodpovědností.</span><span class="sxs-lookup"><span data-stu-id="de784-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="de784-160">Prvním chybným příkladem bylo provedení výpočtu *a* zápis do souboru.</span><span class="sxs-lookup"><span data-stu-id="de784-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="de784-161">Druhý špatný příklad musel číst data z databáze *a* provádět obchodní výpočty *a* posílat e-maily.</span><span class="sxs-lookup"><span data-stu-id="de784-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="de784-162">Návrh menších metod, které oddělují obavy a přenesou zodpovědnost na jiné komponenty, zajistíte skvělou rozteč pro psaní testovatelné kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="de784-163">Cílem je sestavovat funkce z malých a prioritních abstrakcí.</span><span class="sxs-lookup"><span data-stu-id="de784-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="de784-164">V případě, že se dostanou k trvalému ukládání dat, hledají se i ty, které jsme hledali, jsou tak běžné, že jsou popsané jako vzory návrhu.</span><span class="sxs-lookup"><span data-stu-id="de784-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="de784-165">První práce s popisem těchto vzorů v tisku je Fowleraa na základě vzorů podnikové aplikace v knize Novák.</span><span class="sxs-lookup"><span data-stu-id="de784-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="de784-166">Stručný popis těchto vzorů v následujících částech vám ukážeme, jak tyto vzory ADO.NET Entity Framework implementuje a funguje s těmito vzory.</span><span class="sxs-lookup"><span data-stu-id="de784-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="de784-167">Vzor úložiště</span><span class="sxs-lookup"><span data-stu-id="de784-167">The Repository Pattern</span></span>

<span data-ttu-id="de784-168">Fowlera říká, že úložiště "vystupuje z vrstev mapování domény a dat pomocí rozhraní pro přístup k objektům domény".</span><span class="sxs-lookup"><span data-stu-id="de784-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="de784-169">Cílem vzoru úložiště je izolovat kód od minutiae přístupu k datům a jak jsme viděli předchozí izolaci, je pro účely testování nutná hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="de784-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="de784-170">Klíčem k izolaci je způsob, jakým úložiště zpřístupňuje objekty pomocí rozhraní typu kolekce.</span><span class="sxs-lookup"><span data-stu-id="de784-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="de784-171">Logika, kterou napíšete k použití úložiště, nemá žádný nápad, jak úložiště vyhodnotit objekty, které požadujete.</span><span class="sxs-lookup"><span data-stu-id="de784-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="de784-172">Úložiště může komunikovat s databází nebo může vrátit objekty z kolekce v paměti.</span><span class="sxs-lookup"><span data-stu-id="de784-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="de784-173">Veškerý váš kód musí znát, že se zdá, že úložiště uchovává kolekci a že objekty lze načíst, přidat a odstranit z kolekce.</span><span class="sxs-lookup"><span data-stu-id="de784-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="de784-174">V existujících aplikacích .NET konkrétní úložiště často dědí z obecného rozhraní podobného následujícímu:</span><span class="sxs-lookup"><span data-stu-id="de784-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="de784-175">V definici rozhraní provedeme několik změn, když poskytujeme implementaci pro EF4, ale základní koncept zůstává stejný.</span><span class="sxs-lookup"><span data-stu-id="de784-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="de784-176">Kód může použít konkrétní úložiště implementující toto rozhraní k načtení entity pomocí hodnoty primárního klíče, načtení kolekce entit na základě vyhodnocení predikátu nebo jednoduše načíst všechny dostupné entity.</span><span class="sxs-lookup"><span data-stu-id="de784-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="de784-177">Kód může také přidávat a odebírat entity přes rozhraní úložiště.</span><span class="sxs-lookup"><span data-stu-id="de784-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="de784-178">S ohledem na IRepository objektů zaměstnanců může kód provádět následující operace.</span><span class="sxs-lookup"><span data-stu-id="de784-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="de784-179">Vzhledem k tomu, že kód používá rozhraní (IRepository zaměstnance), můžeme kód poskytnout různým implementací rozhraní.</span><span class="sxs-lookup"><span data-stu-id="de784-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="de784-180">Jedna implementace může být implementace zajištěná EF4 a trvalých objektů do databáze Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="de784-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="de784-181">Odlišná implementace (jedna, kterou používáme během testování) může být zálohovaná seznamem objektů zaměstnanců v paměti.</span><span class="sxs-lookup"><span data-stu-id="de784-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="de784-182">Rozhraní vám pomůže dosáhnout izolace v kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="de784-183">Všimněte si, že rozhraní IRepository&lt;T&gt; nezveřejňuje operaci uložení.</span><span class="sxs-lookup"><span data-stu-id="de784-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="de784-184">Jak aktualizovat existující objekty?</span><span class="sxs-lookup"><span data-stu-id="de784-184">How do we update existing objects?</span></span> <span data-ttu-id="de784-185">Můžete se setkat s definicemi IRepository, které zahrnují operaci uložení, a implementace těchto úložišť budou muset do databáze okamžitě zachovat objekt.</span><span class="sxs-lookup"><span data-stu-id="de784-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="de784-186">V mnoha aplikacích ale nechceme objekty uchovávat individuálně.</span><span class="sxs-lookup"><span data-stu-id="de784-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="de784-187">Místo toho chceme přinášet objekty do života, třeba z různých úložišť, upravovat tyto objekty jako součást obchodní aktivity a pak uchovávat všechny objekty jako součást jedné atomické operace.</span><span class="sxs-lookup"><span data-stu-id="de784-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="de784-188">Naštěstí existuje vzor, který umožňuje tento typ chování.</span><span class="sxs-lookup"><span data-stu-id="de784-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="de784-189">Model pracovní jednotky</span><span class="sxs-lookup"><span data-stu-id="de784-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="de784-190">Fowlera říká, že jednotka práce bude "udržovat seznam objektů ovlivněných obchodní transakcí a koordinuje zápis ze změn a řešení problémů s souběžnou kofunkčností".</span><span class="sxs-lookup"><span data-stu-id="de784-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="de784-191">Je zodpovědností za jednotku práce sledovat změny objektů, které připravujeme z úložiště, a zachovat veškeré změny, které jsme v objektech provedli, když sdělíme, že je jednotka práce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="de784-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="de784-192">Je také zodpovědností za jednotku práce při přebírání nových objektů, které jsme přidali do všech úložišť, a vkládání objektů do databáze a také spravovat weby odstranění.</span><span class="sxs-lookup"><span data-stu-id="de784-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="de784-193">Pokud jste někdy dokončili práci s ADO.NETmi datovými sadami, už budete obeznámeni se vzorem pracovní jednotky.</span><span class="sxs-lookup"><span data-stu-id="de784-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="de784-194">ADO.NET datové sady měly schopnost sledovat naše aktualizace, odstranění a vložení objektů DataRow a může (pomocí TableAdapteru) sjednotit všechny změny v databázi.</span><span class="sxs-lookup"><span data-stu-id="de784-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="de784-195">Objekty DataSet ale modelují odpojenou podmnožinu podkladové databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="de784-196">Model pracovní jednotky vykazuje stejné chování, ale funguje s obchodními objekty a doménovými objekty, které jsou izolovány od kódu přístupu k datům a nevědí o databázi.</span><span class="sxs-lookup"><span data-stu-id="de784-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="de784-197">Abstrakce pro modelování jednotky práce v kódu .NET může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="de784-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="de784-198">Zveřejněním odkazů na úložiště z pracovní jednotky můžeme zajistit, aby jedna jednotka pracovního objektu mohla sledovat všechny entity vyhodnocené během obchodní transakce.</span><span class="sxs-lookup"><span data-stu-id="de784-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="de784-199">Implementace metody Commit pro skutečnou pracovní jednotku má za následek to, že všechny Magic se budou sjednotit o sloučení změn v paměti s databází.</span><span class="sxs-lookup"><span data-stu-id="de784-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="de784-200">S ohledem na odkaz IUnitOfWork může kód provádět změny obchodních objektů načtených z jednoho nebo více úložišť a uložit všechny změny pomocí operace atomické potvrzení.</span><span class="sxs-lookup"><span data-stu-id="de784-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="de784-201">Vzor opožděného zatížení</span><span class="sxs-lookup"><span data-stu-id="de784-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="de784-202">Fowlera používá k popisu "objektu, který neobsahuje všechna data, která potřebujete, ale ví, jak ho získat", používá název opožděného načítání.</span><span class="sxs-lookup"><span data-stu-id="de784-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="de784-203">Transparentní opožděné načítání je důležitou funkcí při psaní testovatelné obchodního kódu a práci s relační databází.</span><span class="sxs-lookup"><span data-stu-id="de784-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="de784-204">Zvažte například následující kód.</span><span class="sxs-lookup"><span data-stu-id="de784-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="de784-205">Jak se vyplní kolekce TimeCards?</span><span class="sxs-lookup"><span data-stu-id="de784-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="de784-206">K dispozici jsou dvě možné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="de784-206">There are two possible answers.</span></span> <span data-ttu-id="de784-207">Jednou z odpovědí je, že úložiště zaměstnanců, když se zobrazí výzva k načtení zaměstnance, vydá dotaz, který získá jak zaměstnance, tak i informace o časové kartě daného zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="de784-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="de784-208">V relačních databázích to obecně vyžaduje dotaz s klauzulí JOIN a může mít za následek načítání dalších informací, než kolik potřebuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="de784-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="de784-209">Co když aplikace nikdy nepotřebuje dotykové ovládání vlastnosti TimeCards?</span><span class="sxs-lookup"><span data-stu-id="de784-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="de784-210">Druhou odpovědí je načíst vlastnost TimeCards na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="de784-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="de784-211">Toto opožděné načítání je implicitní a transparentní pro obchodní logiku, protože kód nevyvolává speciální rozhraní API pro načtení informací o časové kartě.</span><span class="sxs-lookup"><span data-stu-id="de784-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="de784-212">Kód předpokládá, že informace o časové kartě jsou v případě potřeby k dispozici.</span><span class="sxs-lookup"><span data-stu-id="de784-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="de784-213">U opožděného načítání je zapojeno některé Magic, které obvykle zahrnuje zachycení metod při volání za běhu.</span><span class="sxs-lookup"><span data-stu-id="de784-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="de784-214">Kód pro zachycení je zodpovědný za komunikaci s databází a načítání informací o časových kartách, zatímco obchodní logika zůstane bezplatná obchodní logikou.</span><span class="sxs-lookup"><span data-stu-id="de784-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="de784-215">Toto opožděné zatížení Magic umožňuje obchodnímu kódu izolovat sám sebe od operací načítání dat a výsledkem je další testovatelné kód.</span><span class="sxs-lookup"><span data-stu-id="de784-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="de784-216">Nevýhodou opožděného zatížení je, že *když aplikace potřebuje* informace o časové kartě, kód provede další dotaz.</span><span class="sxs-lookup"><span data-stu-id="de784-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="de784-217">Nejedná se o problém s mnoha aplikacemi, ale pro aplikace a aplikace s výkonem, které se přecházejí prostřednictvím řady objektů zaměstnanců a spouští dotaz k načtení časových karet během každé iterace smyčky (problém se často označuje jako N + 1. problém s dotazem), opožděné načítání je přetahování.</span><span class="sxs-lookup"><span data-stu-id="de784-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="de784-218">V těchto scénářích může aplikace chtít eagerly získat informace o časové kartě, a to nejúčinnějším způsobem.</span><span class="sxs-lookup"><span data-stu-id="de784-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="de784-219">Naštěstí vám ukážeme, jak EF4 podporuje implicitní opožděné načítání a efektivní Eager načítání při přesunu do další části a implementace těchto vzorů.</span><span class="sxs-lookup"><span data-stu-id="de784-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="de784-220">Implementace vzorů s Entity Framework</span><span class="sxs-lookup"><span data-stu-id="de784-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="de784-221">Dobrá zpráva je, že všechny vzory návrhu, které jsme popsali v poslední části, jsou pro implementaci pomocí EF4 jednoduché.</span><span class="sxs-lookup"><span data-stu-id="de784-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="de784-222">Abychom předvedli použití jednoduché aplikace ASP.NET MVC k úpravám a zobrazení zaměstnanců a jejich přidružených informací o časových kartách.</span><span class="sxs-lookup"><span data-stu-id="de784-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="de784-223">Začneme pomocí následujících "jednoduchých starých objektů CLR" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="de784-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="de784-224">Tyto definice tříd se mírně změní, protože zkoumáme různé přístupy a funkce EF4, ale záměr je zachovat tyto třídy jako trvalost (PI), jak je to možné.</span><span class="sxs-lookup"><span data-stu-id="de784-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="de784-225">Objekt PI neví, *jak*, nebo i *když*se jedná o stav, který se nachází v databázi.</span><span class="sxs-lookup"><span data-stu-id="de784-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="de784-226">PI a POCOs se dostanou rukou s testovatelné softwarem.</span><span class="sxs-lookup"><span data-stu-id="de784-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="de784-227">Objekty využívající přístup k POCO jsou méně omezené, pružnější a lépe se testují, protože mohou fungovat bez přítomnosti databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="de784-228">POCOs je možné v aplikaci Visual Studio vytvořit pomocí model EDM (Entity Data Model) (EDM) (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="de784-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="de784-229">K vygenerování kódu pro naše entity nebudeme používat EDM.</span><span class="sxs-lookup"><span data-stu-id="de784-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="de784-230">Místo toho chceme, abychom používali entity, které jsme lovingly, na základě ruky.</span><span class="sxs-lookup"><span data-stu-id="de784-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="de784-231">K vygenerování našeho schématu databáze použijeme jenom model EDM a poskytneme metadata, která EF4 potřebuje k namapování objektů do databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="de784-233">**Obrázek 1**</span><span class="sxs-lookup"><span data-stu-id="de784-233">**Figure 1**</span></span>

<span data-ttu-id="de784-234">Poznámka: Pokud chcete model EDM nejprve vyvinout, je možné vygenerovat čistý POCO kód z modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="de784-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="de784-235">Můžete to provést pomocí rozšíření sady Visual Studio 2010, které poskytuje tým programovatelnosti dat.</span><span class="sxs-lookup"><span data-stu-id="de784-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="de784-236">Chcete-li stáhnout rozšíření, spusťte Správce rozšíření z nabídky nástroje v aplikaci Visual Studio a vyhledejte online galerii šablon pro "POCO" (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="de784-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="de784-237">Pro EF je k dispozici několik šablon POCO.</span><span class="sxs-lookup"><span data-stu-id="de784-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="de784-238">Další informace o použití šablony najdete v části " [Návod: Šablona POCO pro Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="de784-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="de784-240">**Obrázek 2**</span><span class="sxs-lookup"><span data-stu-id="de784-240">**Figure 2**</span></span>

<span data-ttu-id="de784-241">Z tohoto počátečního bodu POCO budeme prozkoumat dva různé přístupy k testovatelné kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="de784-242">První přístup, který se nazývá přístup EF, protože využívá abstrakce z rozhraní Entity Framework API k implementaci jednotek práce a úložišť.</span><span class="sxs-lookup"><span data-stu-id="de784-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="de784-243">Ve druhém postupu vytvoříme naše vlastní abstrakce úložiště a pak uvidíte výhody a nevýhody jednotlivých přístupů.</span><span class="sxs-lookup"><span data-stu-id="de784-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="de784-244">Začneme prozkoumáním přístupu EF.</span><span class="sxs-lookup"><span data-stu-id="de784-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="de784-245">Implementace orientované na EF</span><span class="sxs-lookup"><span data-stu-id="de784-245">An EF Centric Implementation</span></span>

<span data-ttu-id="de784-246">Vezměte v úvahu následující akci kontroleru z projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="de784-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="de784-247">Akce načte objekt Employee a vrátí výsledek pro zobrazení podrobného zobrazení zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="de784-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="de784-248">Je kód testovatelné?</span><span class="sxs-lookup"><span data-stu-id="de784-248">Is the code testable?</span></span> <span data-ttu-id="de784-249">K dispozici jsou alespoň dva testy, které potřebujeme k ověření chování akce.</span><span class="sxs-lookup"><span data-stu-id="de784-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="de784-250">Nejdříve byste chtěli ověřit, že akce vrátí správné zobrazení – jednoduchý test.</span><span class="sxs-lookup"><span data-stu-id="de784-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="de784-251">Chtěli bychom také napsat test, abychom ověřili, že akce načte správného zaměstnance, a chceme to udělat bez spuštění kódu pro dotazování databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="de784-252">Nezapomeňte izolovat testovaný kód.</span><span class="sxs-lookup"><span data-stu-id="de784-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="de784-253">Při izolaci se ověří, že se test nezdařil z důvodu chyby v kódu pro přístup k datům nebo v konfiguraci databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="de784-254">Pokud se test nezdaří, poznáte, že došlo k chybě v logice kontroleru a ne v některé součásti systému nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="de784-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="de784-255">Aby se zajistila izolace, budete potřebovat několik abstrakcí, jako jsou rozhraní, které jsme dříve předložili pro úložiště a jednotky práce.</span><span class="sxs-lookup"><span data-stu-id="de784-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="de784-256">Zapamatujte si, že vzor úložiště je navržený tak, aby vypravil mezi doménovými objekty a vrstvou mapování dat.</span><span class="sxs-lookup"><span data-stu-id="de784-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="de784-257">V tomto scénáři EF4 *je* vrstva mapování dat a již poskytuje abstrakci podobnou úložišti s názvem IObjectSet&lt;t&gt; (z oboru názvů System. data. Objects).</span><span class="sxs-lookup"><span data-stu-id="de784-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="de784-258">Definice rozhraní vypadá následovně.</span><span class="sxs-lookup"><span data-stu-id="de784-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="de784-259">IObjectSet&lt;T&gt; splňuje požadavky pro úložiště, protože se podobá kolekci objektů (přes IEnumerable&lt;T&gt;) a poskytuje metody pro přidání a odebrání objektů z simulované kolekce.</span><span class="sxs-lookup"><span data-stu-id="de784-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="de784-260">Metody připojení a odpojení zpřístupňují další možnosti rozhraní EF4 API.</span><span class="sxs-lookup"><span data-stu-id="de784-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="de784-261">Pokud chcete používat IObjectSet&lt;T&gt; jako rozhraní pro úložiště, potřebujeme k vytváření vazeb úložišť určitou jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="de784-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="de784-262">Jedna konkrétní implementace tohoto rozhraní se bude mluvit SQL Server a je snadné ho vytvořit pomocí třídy ObjectContext z EF4.</span><span class="sxs-lookup"><span data-stu-id="de784-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="de784-263">Třída ObjectContext je skutečnou jednotkou práce v rozhraní EF4 API.</span><span class="sxs-lookup"><span data-stu-id="de784-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="de784-264">Zavedení IObjectSet&lt;T&gt; je stejně snadné jako volání metody metody CreateObjectSet objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="de784-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="de784-265">Na pozadí Framework použije metadata, která jsme poskytli v modelu EDM, k vytvoření konkrétního&lt;u ObjectSet&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="de784-266">Zavedeme se vrácením rozhraní IObjectSet&lt;T&gt;, protože pomůže zachovat testování v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="de784-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="de784-267">Tato konkrétní implementace je užitečná v produkčním prostředí, ale musíme se zaměřit na to, jak budeme používat naši abstrakci IUnitOfWork k usnadnění testování.</span><span class="sxs-lookup"><span data-stu-id="de784-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="de784-268">Dvojitá přesnost testu</span><span class="sxs-lookup"><span data-stu-id="de784-268">The Test Doubles</span></span>

<span data-ttu-id="de784-269">K izolaci akce kontroleru potřebujeme, aby bylo možné přepínat mezi skutečnou jednotkou práce (pomocí objektu ObjectContext) a dvojitou zkušební jednotkou nebo "falešnou" jednotkou práce (provádění operací v paměti).</span><span class="sxs-lookup"><span data-stu-id="de784-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="de784-270">Běžným přístupem k provedení tohoto typu přepínání je neumožnit, aby řadič MVC vytvořil instanci pracovní jednotky, ale místo toho předávala jednotku práce do kontroleru jako parametr konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="de784-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="de784-271">Výše uvedený kód je příkladem vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="de784-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="de784-272">Nepovolujeme, aby kontroler vytvořil závislost (pracovní jednotka), ale vložil závislost do kontroleru.</span><span class="sxs-lookup"><span data-stu-id="de784-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="de784-273">V projektu MVC se běžně používá vlastní továrna kontroleru v kombinaci s kontejnerem inverze správy (IoC) pro automatizaci vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="de784-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="de784-274">Tato témata jsou nad rámec tohoto článku, ale další informace najdete na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="de784-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="de784-275">Napodobeninná jednotka práce, kterou můžeme použít pro testování, může vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="de784-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="de784-276">Všimněte si, že falešná jednotka práce zveřejňuje svěřenou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="de784-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="de784-277">Někdy je užitečné přidat funkce do falešné třídy, která usnadňuje testování.</span><span class="sxs-lookup"><span data-stu-id="de784-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="de784-278">V tomto případě je možné snadno sledovat, jestli kód potvrdí pracovní jednotku, a to tak, že zkontroluje vlastnost potvrzená.</span><span class="sxs-lookup"><span data-stu-id="de784-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="de784-279">Pro uchování objektů Employee a docházky v paměti budeme také potřebovat falešně IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="de784-280">Jednu implementaci můžeme poskytnout pomocí generických typů.</span><span class="sxs-lookup"><span data-stu-id="de784-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="de784-281">Tento test zdvojnásobí svoji práci na základní objekt HashSet –&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="de784-282">Všimněte si, že IObjectSet&lt;T&gt; vyžaduje obecné omezení vynucující T jako třídu (odkazový typ), a také nám vynutí implementaci IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="de784-283">Je snadné vytvořit kolekci v paměti jako sadu IQueryable&lt;T&gt; pomocí standardního operátoru LINQ AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="de784-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="de784-284">Testy</span><span class="sxs-lookup"><span data-stu-id="de784-284">The Tests</span></span>

<span data-ttu-id="de784-285">Tradiční testy jednotek budou používat jednu testovací třídu pro všechny akce v jednom kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="de784-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="de784-286">Tyto testy můžeme napsat nebo libovolný typ testu jednotek pomocí paměti, kterou jsme vytvořili v paměti.</span><span class="sxs-lookup"><span data-stu-id="de784-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="de784-287">Pro účely tohoto článku se ale vyhneme přístupu ke třídě monolitické test a místo toho seskupme naše testy, abyste se mohli soustředit na konkrétní funkčnost.</span><span class="sxs-lookup"><span data-stu-id="de784-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="de784-288">  Například "vytvořit nového zaměstnance" může být funkce, kterou chceme testovat, takže použijeme jednu testovací třídu pro ověření, že je jediná akce kontroleru zodpovědná za vytvoření nového zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="de784-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="de784-289">K dispozici je několik běžných instalačních kódů, které potřebujeme pro všechny tyto jemně odstupňované testovací třídy.</span><span class="sxs-lookup"><span data-stu-id="de784-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="de784-290">Například musíme vždy vytvořit naše úložiště v paměti a falešné pracovní jednotky.</span><span class="sxs-lookup"><span data-stu-id="de784-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="de784-291">Potřebujeme také instanci řadiče zaměstnanců s vloženou falešnou jednotkou práce.</span><span class="sxs-lookup"><span data-stu-id="de784-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="de784-292">Tento kód společné instalace budeme sdílet napříč testovacími třídami pomocí základní třídy.</span><span class="sxs-lookup"><span data-stu-id="de784-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="de784-293">"Matka" objektu, který používáme v základní třídě, je jedním z běžných vzorů pro vytváření testovacích dat.</span><span class="sxs-lookup"><span data-stu-id="de784-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="de784-294">Matka objektu obsahuje metody pro vytváření instancí testovacích entit pro použití v několika testovacích přípravcích.</span><span class="sxs-lookup"><span data-stu-id="de784-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="de784-295">EmployeeControllerTestBase můžeme použít jako základní třídu pro určitý počet zkušebních přípravek (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="de784-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="de784-296">Každý testovací armatura provede test konkrétní akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="de784-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="de784-297">Například jeden testovací přípravek se soustředí na testování akce vytvoření používané během žádosti HTTP GET (zobrazení pro vytvoření zaměstnance) a jiný přípravek se zaměří na akci vytvoření použitou v požadavku HTTP POST (k získání informací odeslaných uživatel, který má vytvořit zaměstnance).</span><span class="sxs-lookup"><span data-stu-id="de784-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="de784-298">Každá odvozená třída je pouze odpovědná za nastavení potřebná v jeho konkrétním kontextu a k poskytnutí kontrolních výrazů potřebných k ověření výsledků pro svůj konkrétní testovací kontext.</span><span class="sxs-lookup"><span data-stu-id="de784-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="de784-300">**Obrázek 3**</span><span class="sxs-lookup"><span data-stu-id="de784-300">**Figure 3**</span></span>

<span data-ttu-id="de784-301">Zásady vytváření názvů a styl testu, které jsou zde uvedeny, nejsou vyžadovány pro testovatelné kód – jedná se pouze o jediný přístup.</span><span class="sxs-lookup"><span data-stu-id="de784-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="de784-302">Obrázek 4 znázorňuje testy spuštěné v reostřejším modulu plug-in strojového testu jet pro Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="de784-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="de784-304">**Obrázek 4**</span><span class="sxs-lookup"><span data-stu-id="de784-304">**Figure 4**</span></span>

<span data-ttu-id="de784-305">U základní třídy pro zpracování kódu sdíleného nastavení jsou testy jednotek každé akce kontroleru malé a snadno se zapisují.</span><span class="sxs-lookup"><span data-stu-id="de784-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="de784-306">Testy se spustí rychle (vzhledem k tomu, že provádíme operace v paměti) a nemělo by dojít k selhání z důvodu nesouvisející infrastruktury nebo životního prostředí (protože jsme jednotku v rámci testu izolovanou).</span><span class="sxs-lookup"><span data-stu-id="de784-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="de784-307">V těchto testech základní třída provádí většinu práce při instalaci.</span><span class="sxs-lookup"><span data-stu-id="de784-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="de784-308">Zapamatujte si, že konstruktor základní třídy vytvoří úložiště v paměti, falešnou pracovní jednotku a instanci třídy EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="de784-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="de784-309">Testovací třída je odvozena z této základní třídy a zaměřuje se na konkrétní testování metody Create.</span><span class="sxs-lookup"><span data-stu-id="de784-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="de784-310">V takovém případě se konkrétní informace povaří do kroků "uspořádat, ACT a Assert", které se zobrazí v jakémkoli postupu testování částí:</span><span class="sxs-lookup"><span data-stu-id="de784-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="de784-311">Vytvořte objekt Novýzaměstnanec pro simulaci příchozích dat.</span><span class="sxs-lookup"><span data-stu-id="de784-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="de784-312">Vyvolejte akci vytvoření EmployeeController a předejte ji do Novýzaměstnanec.</span><span class="sxs-lookup"><span data-stu-id="de784-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="de784-313">Ověřte, že akce vytvořit generuje očekávané výsledky (zaměstnanec se zobrazí v úložišti).</span><span class="sxs-lookup"><span data-stu-id="de784-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="de784-314">To, co jsme vytvořili, nám umožňuje testovat jakékoli EmployeeController akce.</span><span class="sxs-lookup"><span data-stu-id="de784-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="de784-315">Například když zapisujeme testy pro akci indexu řadiče zaměstnanců, můžeme dědit z testovací základní třídy a vytvořit stejné základní nastavení pro naše testy.</span><span class="sxs-lookup"><span data-stu-id="de784-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="de784-316">Znovu vytvoří základní třídu úložiště v paměti, falešnou pracovní jednotku a instanci EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="de784-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="de784-317">Testy pro akci indexu se musí soustředit jenom na vyvolání akce indexu a testování kvality modelu, který akce vrátí.</span><span class="sxs-lookup"><span data-stu-id="de784-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="de784-318">Testy, které vytváříme s napodobeniny v paměti, jsou orientované k testování *stavu* softwaru.</span><span class="sxs-lookup"><span data-stu-id="de784-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="de784-319">Když například otestujete akci vytvoření, chceme po provedení akce vytvoření zkontrolovat stav úložiště – úložiště bude obsahovat nového zaměstnance?</span><span class="sxs-lookup"><span data-stu-id="de784-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="de784-320">Později se podíváme na testování na základě interakce.</span><span class="sxs-lookup"><span data-stu-id="de784-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="de784-321">Testování založené na interakcích se zeptá, jestli testovaný kód vyvolal správné metody pro naše objekty a předal správné parametry.</span><span class="sxs-lookup"><span data-stu-id="de784-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="de784-322">Prozatím se podíváme na pokrytí dalšího vzoru návrhu – opožděné zatížení.</span><span class="sxs-lookup"><span data-stu-id="de784-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="de784-323">Eager načítání a opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="de784-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="de784-324">V určitém okamžiku webové aplikace ASP.NET MVC můžeme chtít zobrazit informace o zaměstnanci a zahrnout do něj přidružené časové karty zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="de784-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="de784-325">Můžete mít například souhrnné zobrazení časových karet, které zobrazuje jméno zaměstnance a celkový počet časových karet v systému.</span><span class="sxs-lookup"><span data-stu-id="de784-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="de784-326">Je možné provést několik přístupů k implementaci této funkce.</span><span class="sxs-lookup"><span data-stu-id="de784-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="de784-327">Projekce</span><span class="sxs-lookup"><span data-stu-id="de784-327">Projection</span></span>

<span data-ttu-id="de784-328">Jedním ze způsobů, jak vytvořit souhrn, je vytvořit model vyhrazený pro informace, které chceme zobrazit v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="de784-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="de784-329">V tomto scénáři by model vypadal jako následující.</span><span class="sxs-lookup"><span data-stu-id="de784-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="de784-330">Všimněte si, že EmployeeSummaryViewModel není entita – jinými slovy, že v databázi nechcete uchovávat.</span><span class="sxs-lookup"><span data-stu-id="de784-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="de784-331">Tuto třídu budeme používat jenom k náhodnému rozmístění dat do zobrazení v rámci silně typovaného způsobu.</span><span class="sxs-lookup"><span data-stu-id="de784-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="de784-332">Model zobrazení je jako objekt pro přenos dat (DTO), protože neobsahuje žádné chování (žádné metody) – pouze vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="de784-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="de784-333">Vlastnosti budou obsahovat data, která je potřeba přesunout.</span><span class="sxs-lookup"><span data-stu-id="de784-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="de784-334">Je snadné vytvořit instanci tohoto modelu zobrazení pomocí operátoru standardní projekce LINQ – operátor SELECT.</span><span class="sxs-lookup"><span data-stu-id="de784-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="de784-335">Výše uvedený kód obsahuje dvě významné funkce.</span><span class="sxs-lookup"><span data-stu-id="de784-335">There are two notable features to the above code.</span></span> <span data-ttu-id="de784-336">First – kód je snadno testován, protože je stále snadné ho sledovat a izolovat.</span><span class="sxs-lookup"><span data-stu-id="de784-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="de784-337">Operátor Select funguje stejně jako i v případě, že se jedná o falešné místo v paměti, protože se jedná o skutečnou pracovní jednotku.</span><span class="sxs-lookup"><span data-stu-id="de784-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="de784-338">Druhá významná funkce je způsob, jakým kód umožňuje EF4 generovat jediný efektivní dotaz pro sestavování informací o zaměstnanci a časové kartě společně.</span><span class="sxs-lookup"><span data-stu-id="de784-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="de784-339">Do stejného objektu jsme načetli informace o zaměstnancích a časové kartě, aniž byste museli používat žádná speciální rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="de784-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="de784-340">Kód vyjadřuje pouze informace, které vyžaduje, pomocí standardních operátorů LINQ, které pracují se zdroji dat v paměti a také se vzdálenými zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="de784-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="de784-341">EF4 byl schopný přeložit stromy výrazů generované dotazy LINQ a kompilátorem C\# do jediného a efektivního dotazu T-SQL.</span><span class="sxs-lookup"><span data-stu-id="de784-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="de784-342">Existují jiné časy, kdy nechci pracovat s modelem zobrazení nebo objektem DTO, ale s reálnými entitami.</span><span class="sxs-lookup"><span data-stu-id="de784-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="de784-343">Když ví, že potřebujeme zaměstnance *a* časové karty zaměstnanců, můžeme eagerly načíst související data nenápadně a efektivně.</span><span class="sxs-lookup"><span data-stu-id="de784-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="de784-344">Explicitní načítání Eager</span><span class="sxs-lookup"><span data-stu-id="de784-344">Explicit Eager Loading</span></span>

<span data-ttu-id="de784-345">Když chceme eagerly informace o entitách souvisejících s načtením, potřebujeme nějaký mechanismus pro obchodní logiku (nebo v tomto scénáři logiku akční logiky), abyste vyjádřili přání k úložišti.</span><span class="sxs-lookup"><span data-stu-id="de784-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="de784-346">Třída EF4 ObjectQuery&lt;T&gt; definuje metodu include pro určení souvisejících objektů, které mají být načteny během dotazu.</span><span class="sxs-lookup"><span data-stu-id="de784-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="de784-347">Nezapomeňte, že EF4 ObjectContext zpřístupňuje entity prostřednictvím konkrétního&gt; třídy ObjectSet&lt;T, která dědí z ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="de784-348">  Pokud jsme používali odkazy na ObjectSet&lt;T&gt; v naší akci kontroleru, mohli jsme napsat následující kód, který určí Eager načtení informací o časové kartě pro každého zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="de784-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="de784-349">Nicméně vzhledem k tomu, že se snažíme zachovat náš kód testovatelné, Nezveřejňujeme&gt; ObjectSet&lt;T, než je skutečná jednotka třídy Work.</span><span class="sxs-lookup"><span data-stu-id="de784-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="de784-350">Místo toho spoléháme na rozhraní&gt; IObjectSet&lt;T, které je snazší, ale IObjectSet&lt;T&gt; nedefinuje metodu include.</span><span class="sxs-lookup"><span data-stu-id="de784-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="de784-351">Krásy jazyka LINQ je, že můžeme vytvořit vlastní operátor include.</span><span class="sxs-lookup"><span data-stu-id="de784-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="de784-352">Všimněte si, že tento operátor include je definován jako metoda rozšíření pro IQueryable&lt;T&gt; namísto IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="de784-353">Díky tomu je možné využít metodu s širší škálou možných typů, včetně IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;a ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="de784-354">V případě, že se podkladová sekvence nejedná o originální EF4 ObjectQuery&lt;T&gt;, nedošlo k žádnému poškození a operátor include je no-op.</span><span class="sxs-lookup"><span data-stu-id="de784-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="de784-355">Pokud *je* podkladová sekvence ObjectQuery&lt;t&gt; (nebo je odvozená z ObjectQuery&lt;t&gt;), pak EF4 uvidí náš požadavek na další data a formuluje správný dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="de784-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="de784-356">Díky tomuto novému operátorovi můžeme explicitně vyžádat Eager načtení informací o časové kartě z úložiště.</span><span class="sxs-lookup"><span data-stu-id="de784-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="de784-357">Při spuštění proti reálnému objektu ObjectContext vytvoří kód následující jeden dotaz.</span><span class="sxs-lookup"><span data-stu-id="de784-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="de784-358">Dotaz shromažďuje dostatek informací z databáze v jedné cestě, aby vyhodnotit objekty zaměstnanců a plně naplnily jejich vlastnost TimeCards.</span><span class="sxs-lookup"><span data-stu-id="de784-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="de784-359">Skvělé novinky je kód v rámci metody Action zůstane plně testovatelné.</span><span class="sxs-lookup"><span data-stu-id="de784-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="de784-360">Nemusíme poskytovat žádné další funkce pro naše napodobeniny, aby podporovaly operátor include.</span><span class="sxs-lookup"><span data-stu-id="de784-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="de784-361">Chybné zprávy: museli jsme použít operátor include uvnitř kódu, který jsme chtěli zachovat pro ignorování.</span><span class="sxs-lookup"><span data-stu-id="de784-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="de784-362">Toto je základní příklad typu kompromisů, které budete muset vyhodnotit při sestavování kódu testovatelné.</span><span class="sxs-lookup"><span data-stu-id="de784-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="de784-363">K dispozici jsou situace, kdy potřebujete mít obavy z nevracení paměti mimo abstrakci úložiště pro splnění cílů výkonu.</span><span class="sxs-lookup"><span data-stu-id="de784-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="de784-364">Alternativa k načtení Eager je opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="de784-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="de784-365">Opožděné načítání znamená, *že nepotřebujeme* náš podnikový kód, aby explicitně informoval požadavek na přidružená data.</span><span class="sxs-lookup"><span data-stu-id="de784-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="de784-366">Místo toho používáme entity v aplikaci a pokud jsou potřeba další data Entity Framework načte data na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="de784-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="de784-367">Opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="de784-367">Lazy Loading</span></span>

<span data-ttu-id="de784-368">Můžete si snadno představit situaci, kdy nevíte, jaká data bude potřebovat obchodní logika.</span><span class="sxs-lookup"><span data-stu-id="de784-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="de784-369">Je možné, že logika potřebuje objekt Employee, ale můžeme se rozvětvit na různé cesty spuštění, kde některé z těchto cest vyžadují informace o časové kartě od zaměstnance a některé ne.</span><span class="sxs-lookup"><span data-stu-id="de784-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="de784-370">Podobné scénáře jsou ideální pro implicitní opožděné načítání, protože data se zobrazí podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="de784-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="de784-371">Opožděné načítání, označované také jako odložené načítání, umísťuje některé požadavky na naše objekty entity.</span><span class="sxs-lookup"><span data-stu-id="de784-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="de784-372">POCOs s true Persistence ignorování by nedokázala dosáhnout žádného požadavku z trvalé vrstvy, ale ignorování pravdivého přetrvávání je prakticky nemožné.</span><span class="sxs-lookup"><span data-stu-id="de784-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="de784-373">  Místo toho měřím ignorování Persistence ve relativních stupních.</span><span class="sxs-lookup"><span data-stu-id="de784-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="de784-374">By se unfortunate, pokud potřebujeme dědit ze základní třídy s trvalými stálostmi, nebo použít specializovanou kolekci k dosažení opožděného načítání v POCOs.</span><span class="sxs-lookup"><span data-stu-id="de784-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="de784-375">Naštěstí EF4 má méně rušivé řešení.</span><span class="sxs-lookup"><span data-stu-id="de784-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="de784-376">Prakticky nezjistitelné</span><span class="sxs-lookup"><span data-stu-id="de784-376">Virtually Undetectable</span></span>

<span data-ttu-id="de784-377">Při použití objektů POCO může EF4 dynamicky generovat běhové proxy objekty pro entity.</span><span class="sxs-lookup"><span data-stu-id="de784-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="de784-378">Tyto proxy objekty neviditelně balí materializované POCOs a poskytují další služby zachycením jednotlivých vlastností operace get a set pro provedení další práce.</span><span class="sxs-lookup"><span data-stu-id="de784-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="de784-379">Jednou takovou službou je funkce opožděného načítání, kterou hledáte.</span><span class="sxs-lookup"><span data-stu-id="de784-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="de784-380">Další služba je účinný mechanismus pro sledování změn, který může nahrávat, když program změní hodnoty vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="de784-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="de784-381">Seznam změn používá rozhraní ObjectContext během metody SaveChanges k trvalému uchování všech upravených entit pomocí příkazů UPDATE.</span><span class="sxs-lookup"><span data-stu-id="de784-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="de784-382">Aby mohly tyto proxy fungovat, ale potřebují způsob, jak se připojit k operacím Get a set u entity, a proxy objekty dosáhnou tohoto cíle přepsáním virtuálních členů.</span><span class="sxs-lookup"><span data-stu-id="de784-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="de784-383">Proto, pokud chceme mít implicitní opožděné načítání a efektivní sledování změn, musíme přejít zpět na naše definice tříd POCO a označit vlastnosti jako virtuální.</span><span class="sxs-lookup"><span data-stu-id="de784-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="de784-384">Přesto můžeme říci, že entita zaměstnance je většinou ignorována.</span><span class="sxs-lookup"><span data-stu-id="de784-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="de784-385">Jediným požadavkem je použít virtuální členy a nemá vliv na testování kódu.</span><span class="sxs-lookup"><span data-stu-id="de784-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="de784-386">Nepotřebujeme odvozovat ze žádné zvláštní základní třídy ani ani použít speciální kolekci vyhrazenou pro opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="de784-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="de784-387">Jak kód ukazuje, je k dispozici jakákoli třída implementující rozhraní ICollection&lt;T&gt; pro ukládání souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="de784-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="de784-388">V naší pracovní jednotce je také potřeba udělat jednu menší změnu.</span><span class="sxs-lookup"><span data-stu-id="de784-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="de784-389">Opožděné načítání je ve výchozím nastavení *vypnuté* , když pracujete přímo s objektem ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="de784-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="de784-390">Pro vlastnost předané možnosti ContextOptions můžete nastavit vlastnost, která umožňuje odložené načítání. tuto vlastnost můžeme nastavit v naší skutečné pracovní jednotce, pokud chceme povolit opožděné načítání všude.</span><span class="sxs-lookup"><span data-stu-id="de784-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="de784-391">Pokud je povoleno implicitní opožděné načítání, kód aplikace může použít zaměstnance a přidružené časové karty zaměstnance a zbývající blissfully nevědomosti práce požadované pro EF načíst další data.</span><span class="sxs-lookup"><span data-stu-id="de784-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="de784-392">Opožděné načítání usnadňuje psaní kódu aplikace a u proxy Magic kód zůstane zcela testovatelné.</span><span class="sxs-lookup"><span data-stu-id="de784-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="de784-393">Napodobenina pracovní jednotky v paměti může jednoduše předčítat falešné entity s přidruženými daty v případě potřeby během testu.</span><span class="sxs-lookup"><span data-stu-id="de784-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="de784-394">V tuto chvíli provedeme pozornost vytvářením úložišť pomocí IObjectSet&lt;T&gt; a podíváme se na abstrakce, abyste skryli všechny známky rozhraní Persistence.</span><span class="sxs-lookup"><span data-stu-id="de784-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="de784-395">Vlastní úložiště</span><span class="sxs-lookup"><span data-stu-id="de784-395">Custom Repositories</span></span>

<span data-ttu-id="de784-396">Když jsme nejdřív v tomto článku předložili pracovní vzor pracovní jednotky, poskytli jsme vám nějaký vzorový kód, který může být v pracovní jednotce vypadat.</span><span class="sxs-lookup"><span data-stu-id="de784-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="de784-397">Pojďme tento původní nápad znovu prezentovat pomocí scénáře zaměstnance a zaměstnance s časovými kartami, se kterými jsme pracovali.</span><span class="sxs-lookup"><span data-stu-id="de784-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="de784-398">Hlavním rozdílem mezi touto jednotkou práce a částí práce, kterou jsme vytvořili v poslední části, je, jak tato jednotka práce nepoužívá žádné abstrakce z EF4 Frameworku (&gt;&lt;T).</span><span class="sxs-lookup"><span data-stu-id="de784-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="de784-399">IObjectSet&lt;T&gt; funguje dobře jako rozhraní úložiště, ale rozhraní API, které zpřístupňuje, nemusí dokonale sjednotit požadavky naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="de784-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="de784-400">V tomto nadcházejícím přístupu budeme zastupovat úložiště pomocí vlastní IRepository&lt;T&gt; abstrakce.</span><span class="sxs-lookup"><span data-stu-id="de784-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="de784-401">Mnoho vývojářů, kteří následují návrhem řízený návrh, návrhem řízený chování a návrhem metodologie řízených doménami, dávají přednost IRepository&lt;T&gt; přístup z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="de784-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="de784-402">Nejprve rozhraní IRepository&lt;T&gt; představuje vrstvu "anti-poškození".</span><span class="sxs-lookup"><span data-stu-id="de784-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="de784-403">Jak je popsáno v Eric Evans v rámci své pracovní knihy založené na doméně, vrstva odolná proti poškození udržuje kód vaší domény od rozhraní API infrastruktury, jako je trvalá rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="de784-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="de784-404">V druhé době můžou vývojáři sestavovat metody do úložiště, které splňuje přesné potřeby aplikace (jak bylo zjištěno při psaní testů).</span><span class="sxs-lookup"><span data-stu-id="de784-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="de784-405">Například může být často potřeba vyhledat jednu entitu pomocí hodnoty ID, abychom mohli do rozhraní úložiště přidat metodu FindById.</span><span class="sxs-lookup"><span data-stu-id="de784-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="de784-406">  Naše IRepository&lt;T&gt; definice bude vypadat jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="de784-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="de784-407">Všimněte si, že jsme zpátky na použití rozhraní IQueryable&lt;T&gt; k vystavování kolekcí entit.</span><span class="sxs-lookup"><span data-stu-id="de784-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="de784-408">IQueryable&lt;T&gt; umožňuje stromům výrazů LINQ přesměrovat do poskytovatele EF4 a poskytnout poskytovateli holistický zobrazení dotazu.</span><span class="sxs-lookup"><span data-stu-id="de784-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="de784-409">Druhou možností je vrátit rozhraní IEnumerable&lt;T&gt;, což znamená, že poskytovatel LINQ EF4 uvidí jenom výrazy sestavené uvnitř úložiště.</span><span class="sxs-lookup"><span data-stu-id="de784-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="de784-410">Jakékoli seskupení, řazení a projekce provedené mimo úložiště se neskládají do příkazu SQL, který se odesílá do databáze, což může snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="de784-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="de784-411">Na druhé straně úložiště vracející jenom IEnumerable&lt;T&gt; výsledky nikdy nevrátí nový příkaz SQL.</span><span class="sxs-lookup"><span data-stu-id="de784-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="de784-412">Oba přístupy budou fungovat a oba přístupy zůstanou testovatelné.</span><span class="sxs-lookup"><span data-stu-id="de784-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="de784-413">Je jednoduché poskytnout jednu implementaci rozhraní IRepository&lt;T&gt; pomocí obecných typů a rozhraní EF4 ObjectContext API.</span><span class="sxs-lookup"><span data-stu-id="de784-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="de784-414">Přístup IRepository&lt;T&gt; nám poskytuje další kontrolu nad našimi dotazy, protože klient musí vyvolat metodu pro získání entity.</span><span class="sxs-lookup"><span data-stu-id="de784-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="de784-415">Uvnitř metody můžeme poskytnout další kontroly a operátory LINQ k prosazování omezení aplikace.</span><span class="sxs-lookup"><span data-stu-id="de784-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="de784-416">Všimněte si, že rozhraní má dvě omezení parametru obecného typu.</span><span class="sxs-lookup"><span data-stu-id="de784-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="de784-417">Prvním omezením je nevýhoda třídy chuti, kterou vyžaduje ObjectSet&lt;T&gt;. druhé omezení vynutí, aby naši entity implementovaly IEntity – abstrakce vytvořená pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="de784-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="de784-418">Rozhraní IEntity vynutí, aby entity měly vlastnost čitelného ID a tuto vlastnost pak můžeme použít v metodě FindById.</span><span class="sxs-lookup"><span data-stu-id="de784-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="de784-419">IEntity je definován s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="de784-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="de784-420">IEntity může být považováno za malé porušení trvalosti, protože pro implementaci tohoto rozhraní jsou vyžadovány naše entity.</span><span class="sxs-lookup"><span data-stu-id="de784-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="de784-421">Pamatujte na to, že ignorování přetrvává v souvislosti s kompromisy a pro mnoho funkcí FindById převažuje omezení uložené rozhraním.</span><span class="sxs-lookup"><span data-stu-id="de784-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="de784-422">Rozhraní nemá žádný vliv na testování.</span><span class="sxs-lookup"><span data-stu-id="de784-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="de784-423">Vytvoření instance živého IRepository&lt;T&gt; vyžaduje rozhraní ObjectContext EF4, takže konkrétní jednotka práce by měla spravovat vytváření instancí.</span><span class="sxs-lookup"><span data-stu-id="de784-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="de784-424">Používání vlastního úložiště</span><span class="sxs-lookup"><span data-stu-id="de784-424">Using the Custom Repository</span></span>

<span data-ttu-id="de784-425">Použití našeho vlastního úložiště se významně neliší od použití úložiště založeného na IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="de784-426">Namísto použití operátorů LINQ přímo na vlastnost bude nejprve nutné vyvolat jednu z metod úložiště, aby bylo možné získat odkaz IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="de784-427">Všimněte si, že vlastní operátor include, který jsme dřív implementovali, bude fungovat beze změny.</span><span class="sxs-lookup"><span data-stu-id="de784-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="de784-428">Metoda FindById úložiště odstraní duplikovánou logiku z akcí, které se pokoušejí načíst jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="de784-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="de784-429">Neexistují žádné významné rozdíly v testování dvou přístupů, které jsme prozkoumali.</span><span class="sxs-lookup"><span data-stu-id="de784-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="de784-430">Pro IRepositoryy, které jsou založené na HashSet –&lt;zaměstnanců&gt; – stejně jako to, co jsme provedli v poslední části, můžeme poskytnout falešné implementace&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="de784-431">Někteří vývojáři však dávají přednost použití objektů typu Object a objektové architektury objektů namísto sestavování napodobenin.</span><span class="sxs-lookup"><span data-stu-id="de784-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="de784-432">Podíváme se na použití makety k otestování naší implementace a diskuze o rozdílech mezi modely a napodobeninami v další části.</span><span class="sxs-lookup"><span data-stu-id="de784-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="de784-433">Testování s použitím makety</span><span class="sxs-lookup"><span data-stu-id="de784-433">Testing with Mocks</span></span>

<span data-ttu-id="de784-434">Existují různé přístupy k vytváření toho, co Fowlera Novák volá "test Double".</span><span class="sxs-lookup"><span data-stu-id="de784-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="de784-435">Test Double (například Stunt double) je objekt, který sestavíte jako "podstavec" pro reálné a produkční objekty během testů.</span><span class="sxs-lookup"><span data-stu-id="de784-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="de784-436">Úložiště v paměti, které jsme vytvořili, jsou pro úložiště, která komunikují SQL Server, testována dvojitá.</span><span class="sxs-lookup"><span data-stu-id="de784-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="de784-437">Zjistili jsme, jak používat tyto testy-Double při testování částí k izolaci kódu a udržování testů v rychlém provozu.</span><span class="sxs-lookup"><span data-stu-id="de784-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="de784-438">Test podvoje, že jsme vytvořili reálné a funkční implementace.</span><span class="sxs-lookup"><span data-stu-id="de784-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="de784-439">Na pozadí každý z nich ukládá konkrétní kolekci objektů a přidá a odebere objekty z této kolekce při manipulaci s úložištěm během testu.</span><span class="sxs-lookup"><span data-stu-id="de784-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="de784-440">Někteří vývojáři, jako je sestavení testu, tento způsob zdvojnásobí – s reálným kódem a pracovními implementacemi.</span><span class="sxs-lookup"><span data-stu-id="de784-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="de784-441">  Tyto dva testy jsou povolány jako *falešné*.</span><span class="sxs-lookup"><span data-stu-id="de784-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="de784-442">Mají funkční implementace, ale nejsou dostatečně reálné pro použití v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="de784-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="de784-443">Falešné úložiště ve skutečnosti nezapisuje do databáze.</span><span class="sxs-lookup"><span data-stu-id="de784-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="de784-444">Falešný server SMTP ve skutečnosti neposílá e-mailovou zprávu přes síť.</span><span class="sxs-lookup"><span data-stu-id="de784-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="de784-445">Modely versus napodobeniny</span><span class="sxs-lookup"><span data-stu-id="de784-445">Mocks versus Fakes</span></span>

<span data-ttu-id="de784-446">K dispozici je jiný typ *testu, známý jako objekt typu*.</span><span class="sxs-lookup"><span data-stu-id="de784-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="de784-447">I když napodobeniny mají funkční implementace, jsou modely dodávány bez implementace.</span><span class="sxs-lookup"><span data-stu-id="de784-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="de784-448">V případě, že je k dispozici maketa objektového rozhraní, sestavíme tyto objekty v době běhu a použijeme je jako test dvojitých.</span><span class="sxs-lookup"><span data-stu-id="de784-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="de784-449">V této části budeme používat Open Source napodobnou rozhraní MOQ.</span><span class="sxs-lookup"><span data-stu-id="de784-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="de784-450">Tady je jednoduchý příklad použití MOQ k dynamickému vytváření testovacího zdvojnásobení pro úložiště zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="de784-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="de784-451">Požádáme MOQ o IRepository implementaci&gt;&lt;zaměstnanců a sestavíme ji dynamicky.</span><span class="sxs-lookup"><span data-stu-id="de784-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="de784-452">K objektu, který implementuje IRepository&lt;&gt; zaměstnanců, se můžeme dostat přístupem k vlastnosti objektu objektu makety&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="de784-453">Jedná se o vnitřní objekt, který můžeme předat do našich řadičů a nedokáže zjistit, jestli se jedná o dvojitý test nebo reálné úložiště.</span><span class="sxs-lookup"><span data-stu-id="de784-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="de784-454">Můžeme vyvolat metody objektu stejně jako bychom vyvolali metody u objektu s skutečnou implementací.</span><span class="sxs-lookup"><span data-stu-id="de784-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="de784-455">Je nutné zajímat, co bude v případě, kdy vyvolá metodu Add, vyvoláno.</span><span class="sxs-lookup"><span data-stu-id="de784-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="de784-456">Vzhledem k tomu, že není k dispozici žádná implementace za objektem makety, nelze přidat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="de784-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="de784-457">Na pozadí se nevyskytuje žádná konkrétní kolekce, jako bychom měli napsané falešné, takže se zaměstnanec zahodí.</span><span class="sxs-lookup"><span data-stu-id="de784-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="de784-458">Co je návratová hodnota FindById?</span><span class="sxs-lookup"><span data-stu-id="de784-458">What about the return value of FindById?</span></span> <span data-ttu-id="de784-459">V takovém případě objekt makety dělá pouze to, co může udělat, což vrátí výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="de784-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="de784-460">Vzhledem k tomu, že vracíme typ odkazu (zaměstnanec), vrácená hodnota je hodnota null.</span><span class="sxs-lookup"><span data-stu-id="de784-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="de784-461">Napodobení může bezcenné zvuk; Existují však dvě další funkce popsaných prvků, o kterých jsme mluvilii.</span><span class="sxs-lookup"><span data-stu-id="de784-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="de784-462">Nejprve rozhraní MOQ zaznamená všechna volání vytvořená na objektu.</span><span class="sxs-lookup"><span data-stu-id="de784-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="de784-463">Později v kódu můžeme požádat o MOQ, pokud kdokoli vyvolal metodu Add nebo když někdo vyvolal metodu FindById.</span><span class="sxs-lookup"><span data-stu-id="de784-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="de784-464">Později uvidíte, jak můžeme použít tuto "černou" funkci záznamu v testech.</span><span class="sxs-lookup"><span data-stu-id="de784-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="de784-465">Druhá skvělé funkce je způsob, jak můžeme použít MOQ k naprogramování objektu makety s *očekáváními*.</span><span class="sxs-lookup"><span data-stu-id="de784-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="de784-466">Očekává se, že objekt Form, jak reagovat na danou interakci.</span><span class="sxs-lookup"><span data-stu-id="de784-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="de784-467">Například můžeme naprogramovat očekávání do našeho objektu a sdělit mu, aby vrátila objekt Employee, když někdo vyvolá FindById.</span><span class="sxs-lookup"><span data-stu-id="de784-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="de784-468">MOQ Framework používá rozhraní API pro instalaci a lambda výrazy k naprogramování těchto očekávání.</span><span class="sxs-lookup"><span data-stu-id="de784-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="de784-469">V této ukázce požádáme MOQ, aby dynamicky sestavil úložiště a pak jsme úložiště naprogramoval na očekávanou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="de784-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="de784-470">Očekává se, že objekt Form vrátí objekt pro nový zaměstnanec s hodnotou ID 5, pokud někdo vyvolá metodu FindById, která předává hodnotu 5.</span><span class="sxs-lookup"><span data-stu-id="de784-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="de784-471">Tento test projde a nemuseli jsme sestavovat plnou implementaci falešného IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="de784-472">Pojďme znovu přejít k dříve popsaným testům a znovu je použít k použití návrhů namísto napodobenin.</span><span class="sxs-lookup"><span data-stu-id="de784-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="de784-473">Stejně jako dřív, použijeme základní třídu k nastavení společných částí infrastruktury, které potřebujeme pro všechny testy řadiče.</span><span class="sxs-lookup"><span data-stu-id="de784-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="de784-474">Instalační kód zůstává většinou stejný.</span><span class="sxs-lookup"><span data-stu-id="de784-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="de784-475">Místo použití falešných objektů použijeme MOQ k vytvoření objektů typu Object.</span><span class="sxs-lookup"><span data-stu-id="de784-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="de784-476">Základní třída uspořádá v případě, že se má v případě, že kód vyvolá vlastnost Employees, použít podrobnější úložiště.</span><span class="sxs-lookup"><span data-stu-id="de784-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="de784-477">Zbytek napodobení se provede v rámci testovacích zařízení vyhrazených pro každý konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="de784-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="de784-478">Například test přípravek pro akci index nastaví napodobné úložiště tak, aby vracelo seznam zaměstnanců, když akce vyvolá metodu FindAll ve vzorovém úložišti.</span><span class="sxs-lookup"><span data-stu-id="de784-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="de784-479">S výjimkou toho, že naše testy vypadají podobně jako testy, které jsme předtím používali.</span><span class="sxs-lookup"><span data-stu-id="de784-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="de784-480">Nicméně s možností záznamu z napodobné architektury se můžeme přiblíží k testování z jiného úhlu.</span><span class="sxs-lookup"><span data-stu-id="de784-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="de784-481">V další části se podíváme na tuto novou perspektivu.</span><span class="sxs-lookup"><span data-stu-id="de784-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="de784-482">Stav versus testování interakce</span><span class="sxs-lookup"><span data-stu-id="de784-482">State versus Interaction Testing</span></span>

<span data-ttu-id="de784-483">Existují různé techniky, které můžete použít k otestování softwaru s využitím objektů.</span><span class="sxs-lookup"><span data-stu-id="de784-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="de784-484">Jednou z možností je použít testování založené na stavu, které jsme v tomto dokumentu udělali.</span><span class="sxs-lookup"><span data-stu-id="de784-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="de784-485">Testování na základě stavu poskytuje kontrolní výrazy týkající se stavu softwaru.</span><span class="sxs-lookup"><span data-stu-id="de784-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="de784-486">V posledním testu vyvolali metodu Action na řadiči a provedli jste kontrolní výraz modelu, který by měl sestavit.</span><span class="sxs-lookup"><span data-stu-id="de784-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="de784-487">Tady jsou některé další příklady stavu testování:</span><span class="sxs-lookup"><span data-stu-id="de784-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="de784-488">Ověřte, že úložiště obsahuje nový objekt Employee po provedení vytvoření.</span><span class="sxs-lookup"><span data-stu-id="de784-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="de784-489">Ověřte, že model obsahuje seznam všech zaměstnanců po spuštění indexu.</span><span class="sxs-lookup"><span data-stu-id="de784-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="de784-490">Ověřte, že úložiště neobsahuje daného zaměstnance, když se spustí odstranění.</span><span class="sxs-lookup"><span data-stu-id="de784-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="de784-491">Dalším postupem, který se zobrazí u objektů s přípravou, je ověření *interakcí*.</span><span class="sxs-lookup"><span data-stu-id="de784-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="de784-492">Zatímco testování na základě stavu poskytuje kontrolní výrazy týkající se stavu objektů, testování na základě interakce poskytuje kontrolní výrazy týkající se způsobu interakce objektů.</span><span class="sxs-lookup"><span data-stu-id="de784-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="de784-493">Příklad:</span><span class="sxs-lookup"><span data-stu-id="de784-493">For example:</span></span>

-   <span data-ttu-id="de784-494">Ověřte, že řadič vyvolá metodu Add úložiště při vytváření.</span><span class="sxs-lookup"><span data-stu-id="de784-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="de784-495">Ověřte, že řadič vyvolá metodu FindAll úložiště, když se spustí index.</span><span class="sxs-lookup"><span data-stu-id="de784-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="de784-496">Ověřte, že řadič vyvolá metodu potvrzení pracovní jednotky, aby se změny uložily při spuštění úpravy.</span><span class="sxs-lookup"><span data-stu-id="de784-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="de784-497">Testování interakce často vyžaduje méně testovacích dat, protože nejsme poking v kolekcích a Ověřujeme počty.</span><span class="sxs-lookup"><span data-stu-id="de784-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="de784-498">Pokud například ví, že akce vyvolá metodu FindById úložiště se správnou hodnotou, pak se tato akce pravděpodobně chová správně.</span><span class="sxs-lookup"><span data-stu-id="de784-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="de784-499">Toto chování můžeme ověřit bez nastavování všech testovacích dat, která se mají vrátit z FindById.</span><span class="sxs-lookup"><span data-stu-id="de784-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="de784-500">Jediným nastavením požadovaným v rámci výše uvedeného testovacího přípravku je nastavení poskytované základní třídou.</span><span class="sxs-lookup"><span data-stu-id="de784-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="de784-501">Když vyvoláme akci kontroleru, MOQ zaznamená interakce s modelovým úložištěm.</span><span class="sxs-lookup"><span data-stu-id="de784-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="de784-502">Pomocí rozhraní MOQ API pro ověření, můžeme požádat MOQ, pokud řadič vyvolal FindById se správnou hodnotou ID.</span><span class="sxs-lookup"><span data-stu-id="de784-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="de784-503">Pokud řadič nevolal metodu nebo vyvolal metodu s neočekávanou hodnotou parametru, metoda Verify vyvolá výjimku a test se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="de784-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="de784-504">Tady je další příklad, jak ověřit, že akce vytvořit vyvolá potvrzení pro aktuální pracovní jednotku.</span><span class="sxs-lookup"><span data-stu-id="de784-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="de784-505">Jedním z nebezpečí při testování interakcí je, že se má v rámci určení interakcí.</span><span class="sxs-lookup"><span data-stu-id="de784-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="de784-506">Schopnost objektu makety zaznamenat a ověřit každou interakci s objektem pro zápis neznamená, že test by měl zkusit ověřit každou interakci.</span><span class="sxs-lookup"><span data-stu-id="de784-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="de784-507">Některé interakce jsou podrobné informace o implementaci a měli byste pouze ověřit interakce *požadované* pro splnění aktuálního testu.</span><span class="sxs-lookup"><span data-stu-id="de784-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="de784-508">Volba mezi maketami nebo napodobeninami, které jsou v podstatě, závisí na systému, který testujete, a na vašich osobních (nebo týmových) preferencích.</span><span class="sxs-lookup"><span data-stu-id="de784-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="de784-509">Objekty typu Object můžou významně snížit množství kódu, který potřebujete k implementaci zdvojnásobení testu, ale ne všichni mají pohodlné programování a ověřují interakce.</span><span class="sxs-lookup"><span data-stu-id="de784-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="de784-510">Závěry</span><span class="sxs-lookup"><span data-stu-id="de784-510">Conclusions</span></span>

<span data-ttu-id="de784-511">V tomto dokumentu jsme ukázali několik přístupů k vytváření kódu testovatelné při použití ADO.NET Entity Framework pro trvalost dat.</span><span class="sxs-lookup"><span data-stu-id="de784-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="de784-512">Můžeme využít předdefinované abstrakce jako IObjectSet&lt;T&gt;nebo vytvořit vlastní abstrakce, jako je IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="de784-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="de784-513">  V obou případech podpora POCO v ADO.NET Entity Framework 4,0 umožňuje spotřebitelům těchto abstrakcí zůstat trvale ignorovatelné a vysoce testovatelné.</span><span class="sxs-lookup"><span data-stu-id="de784-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="de784-514">Další funkce EF4, jako je implicitní opožděné načítání, umožňují pracovat s kódem obchodních a aplikačních služeb, aniž by se museli starat o podrobnosti o relačním úložišti dat.</span><span class="sxs-lookup"><span data-stu-id="de784-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="de784-515">A nakonec se abstrakce, které vytvoříme, snadno napodobují nebo jsou falešné v rámci testů jednotek a můžeme použít tyto podvojky testu k zajištění rychlých spuštěných, vysoce izolovaných a spolehlivých testů.</span><span class="sxs-lookup"><span data-stu-id="de784-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="de784-516">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="de784-516">Additional Resources</span></span>

-   <span data-ttu-id="de784-517">Robert C. Martin, " [Princip jedné zodpovědnosti](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="de784-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="de784-518">Martin Fowlera, [katalog vzorů](https://www.martinfowler.com/eaaCatalog/index.html) ze *vzorů architektury podnikové aplikace*</span><span class="sxs-lookup"><span data-stu-id="de784-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="de784-519">Griffin Caprio, " [vkládání závislostí](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="de784-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="de784-520">Blog o programovatelnosti dat " [Návod: Vývoj řízený testováním pomocí Entity Framework 4,0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="de784-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="de784-521">Blog o programovatelnosti dat, " [používání úložiště a pracovní jednotky vzorů s Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="de784-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="de784-522">Aaron Jensen, " [představení specifikací počítačů](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="de784-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="de784-523">Eric Novák, " [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="de784-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="de784-524">Eric Evans, " [návrh založený na doméně](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="de784-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="de784-525">Martin Fowlera, " [napodobeniny nejsou](https://martinfowler.com/articles/mocksArentStubs.html)zástupné kódy"</span><span class="sxs-lookup"><span data-stu-id="de784-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="de784-526">Martin Fowlera, " [test Double](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="de784-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="de784-527">Moq</span><span class="sxs-lookup"><span data-stu-id="de784-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="de784-528">Biografie</span><span class="sxs-lookup"><span data-stu-id="de784-528">Biography</span></span>

<span data-ttu-id="de784-529">Scott Allen je členem technického personálu v Pluralsight a zakladatel OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="de784-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="de784-530">V 15 letech komerčního vývojového softwaru se Scott pracoval na řešeních, která jsou na všech počítačích se systémem, která obsahují 8bitové integrované zařízení, aby vysoce škálovatelné webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de784-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="de784-531">Na svém blogu můžete přistihnout na OdeToCode nebo na Twitteru na [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="de784-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
