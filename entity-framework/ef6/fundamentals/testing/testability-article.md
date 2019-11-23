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
# <a name="testability-and-entity-framework-40"></a>Testování a Entity Framework 4,0
Scott Allen

Publikováno: květen 2010

## <a name="introduction"></a>Úvod

Tento dokument white paper popisuje a ukazuje, jak napsat kód testovatelné pomocí nástroje ADO.NET Entity Framework 4,0 a sady Visual Studio 2010. Tento dokument se nepokouší soustředit na konkrétní metodologii testování, jako je například metoda založená na testu (TDD) nebo návrh na základě chování (BDD). Místo toho se tento dokument zaměřuje na to, jak psát kód, který používá ADO.NET Entity Framework a zatím zůstává snadno izolovaný a testovaný a automatizovaný. Podíváme se na běžné vzory návrhu, které usnadňují testování ve scénářích přístupu k datům, a dozvíte se, jak tyto vzory použít při použití architektury. Také se podíváme na konkrétní funkce rozhraní, abyste viděli, jak tyto funkce můžou pracovat v testovatelné kódu.

## <a name="what-is-testable-code"></a>Co je testovatelné kód?

Možnost ověřit část softwaru pomocí automatizovaných testů jednotek nabízí spoustu žádoucích výhod. Každý ví, že dobré testy sníží počet softwarových vad v aplikaci a zvýší kvalitu aplikace, ale testy jednotek budou ještě mnohem mimo hledání chyb.

Dobrá testovací sada umožňuje vývojovému týmu ušetřit čas a zůstat pod kontrolou softwaru, který vytváří. Tým může provádět změny v existujícím kódu, Refaktorovat, přenavrhovat a restrukturovat software, aby splňoval nové požadavky, a současně přidat nové komponenty do aplikace a současně s vědomím, že testovací sada dokáže ověřit chování aplikace. Testy jednotek jsou součástí cyklu rychlé zpětné vazby, která usnadňují změnu a zachovává udržovatelnost softwaru při zvyšování složitosti.

Testování částí se ale dodává s cenou. Tým musí investovat čas na vytvoření a údržbu testování částí. Množství úsilí potřebné k vytvoření těchto testů přímo souvisí s **testováním** základního softwaru. Jak snadné je software k testování? Tým, který navrhuje software s ohledem na testování, vytvoří efektivní testy rychleji než tým, který pracuje s testovatelné softwarem.

Společnost Microsoft navrhla ADO.NET Entity Framework 4,0 (EF4) s ohledem na testování. To neznamená, že vývojáři budou psát testy jednotek proti samotnému kódu rozhraní. Místo toho cíle testování pro EF4 usnadňuje vytváření testovatelné kódu, který sestaví na rozhraní. Předtím, než se podíváme na konkrétní příklady, je vhodné pochopit kvality testovatelné kódu.

### <a name="the-qualities-of-testable-code"></a>Vlastnosti testovatelné kódu

Kód, který se dá snadno testovat, se vždy zobrazí alespoň se dvěma vlastnostmi. Nejprve je testovatelné kód snadno **sledovat**. Pro určitou sadu vstupů by mělo být snadné sledovat výstup kódu. Například testování následující metody je snadné, protože metoda přímo vrací výsledek výpočtu.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Testování metody je obtížné, pokud metoda zapíše vypočítanou hodnotu do síťového soketu, databázové tabulky nebo souboru jako následující kód. Test musí provést další práci, aby bylo možné načíst hodnotu.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

V druhém případě se testovatelné kód snadno **izoluje**. Pojďme použít následující pseudo kód jako špatný příklad kódu testovatelné.

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

Metodu je možné snadno sledovat – můžeme předat zásady pojištění a ověřit, zda vrácená hodnota odpovídá očekávanému výsledku. K otestování této metody ale musíme mít nainstalovanou databázi se správným schématem a nakonfigurovat server SMTP pro případ, že se metoda pokusí odeslat e-mail.

Test jednotek chce pouze ověřit logiku výpočtu v rámci metody, ale test může selhat, protože e-mailový server je offline nebo proto, že databázový server byl přesunut. Oba tyto chyby nesouvisejí s chováním, které test chce ověřit. Chování je obtížné izolovat.

Vývojáři softwaru, kteří se snaží psát kód testovatelné, často usilují o udržení oddělení obav v kódu, který zapisuje. Výše uvedená metoda by se měla soustředit na obchodní výpočty a podrobné informace o implementaci databáze a e-mailu pro ostatní komponenty. Robert C. Martin volá tento princip zodpovědnosti. Objekt by měl zapouzdřit jedinou, úzký zodpovědnost, jako je například výpočet hodnoty zásad. Všechny ostatní práce databáze a oznámení by měly být zodpovědností nějakého jiného objektu. Kód napsaný tímto způsobem je snazší izolovat, protože se zaměřuje na jeden úkol.

V .NET máme abstrakce, které potřebujeme pro splnění principu jedné zodpovědnosti a zajištění izolace. Můžeme použít definice rozhraní a vynutit, aby kód používal abstrakci rozhraní místo konkrétního typu. Později v tomto dokumentu se dozvíte, jak metoda, jako je chybný příklad uvedený výše, může fungovat s rozhraními, která *vypadají* jako, budou komunikovat s databází. V době testování však můžeme nahradit fiktivní implementaci, která nepracuje s databází, ale místo toho ukládá data do paměti. Tato fiktivní implementace izoluje kód od nesouvisejících problémů v kódu pro přístup k datům nebo v konfiguraci databáze.

Pro izolaci jsou k dispozici další výhody. Obchodní kalkulace v poslední metodě by měla trvat jen několik milisekund, ale test sám může běžet několik sekund, než se kód rozhlásí kolem sítě a mluví na různé servery. Testy jednotek by měly rychle běžet, aby se usnadnily malé změny. Testy jednotek by také měly být opakované a neúspěšné, protože došlo k potížím s komponentou, která nesouvisí s testem. Psaní kódu, který se snadno sleduje a izoluje znamená, že vývojáři budou mít snazší časová testování kódu, stráví méně času čekáním na provedení testů a důležitější je, že stráví méně času sledováním chyb, které neexistují.

Snad můžete vyhodnotit výhody testování a pochopit, jaké kvality se testovatelné kód projeví. Chystáme se poznamenat, jak napsat kód, který spolupracuje s EF4, aby ušetřil data do databáze a bylo možné je snadno izolovat, ale nejdřív se zúžíme na diskuzi o tom, abychom probrali návrhy testovatelné pro přístup k datům.

## <a name="design-patterns-for-data-persistence"></a>Vzory návrhu pro trvalost dat

Oba chybné příklady uvedené dříve obsahovaly příliš mnoho zodpovědností. Prvním chybným příkladem bylo provedení výpočtu *a* zápis do souboru. Druhý špatný příklad musel číst data z databáze *a* provádět obchodní výpočty *a* posílat e-maily. Návrh menších metod, které oddělují obavy a přenesou zodpovědnost na jiné komponenty, zajistíte skvělou rozteč pro psaní testovatelné kódu. Cílem je sestavovat funkce z malých a prioritních abstrakcí.

V případě, že se dostanou k trvalému ukládání dat, hledají se i ty, které jsme hledali, jsou tak běžné, že jsou popsané jako vzory návrhu. První práce s popisem těchto vzorů v tisku je Fowleraa na základě vzorů podnikové aplikace v knize Novák. Stručný popis těchto vzorů v následujících částech vám ukážeme, jak tyto vzory ADO.NET Entity Framework implementuje a funguje s těmito vzory.

### <a name="the-repository-pattern"></a>Vzor úložiště

Fowlera říká, že úložiště "vystupuje z vrstev mapování domény a dat pomocí rozhraní pro přístup k objektům domény". Cílem vzoru úložiště je izolovat kód od minutiae přístupu k datům a jak jsme viděli předchozí izolaci, je pro účely testování nutná hodnota vlastnosti.

Klíčem k izolaci je způsob, jakým úložiště zpřístupňuje objekty pomocí rozhraní typu kolekce. Logika, kterou napíšete k použití úložiště, nemá žádný nápad, jak úložiště vyhodnotit objekty, které požadujete. Úložiště může komunikovat s databází nebo může vrátit objekty z kolekce v paměti. Veškerý váš kód musí znát, že se zdá, že úložiště uchovává kolekci a že objekty lze načíst, přidat a odstranit z kolekce.

V existujících aplikacích .NET konkrétní úložiště často dědí z obecného rozhraní podobného následujícímu:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

V definici rozhraní provedeme několik změn, když poskytujeme implementaci pro EF4, ale základní koncept zůstává stejný. Kód může použít konkrétní úložiště implementující toto rozhraní k načtení entity pomocí hodnoty primárního klíče, načtení kolekce entit na základě vyhodnocení predikátu nebo jednoduše načíst všechny dostupné entity. Kód může také přidávat a odebírat entity přes rozhraní úložiště.

S ohledem na IRepository objektů zaměstnanců může kód provádět následující operace.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Vzhledem k tomu, že kód používá rozhraní (IRepository zaměstnance), můžeme kód poskytnout různým implementací rozhraní. Jedna implementace může být implementace zajištěná EF4 a trvalých objektů do databáze Microsoft SQL Server. Odlišná implementace (jedna, kterou používáme během testování) může být zálohovaná seznamem objektů zaměstnanců v paměti. Rozhraní vám pomůže dosáhnout izolace v kódu.

Všimněte si, že rozhraní IRepository&lt;T&gt; nezveřejňuje operaci uložení. Jak aktualizovat existující objekty? Můžete se setkat s definicemi IRepository, které zahrnují operaci uložení, a implementace těchto úložišť budou muset do databáze okamžitě zachovat objekt. V mnoha aplikacích ale nechceme objekty uchovávat individuálně. Místo toho chceme přinášet objekty do života, třeba z různých úložišť, upravovat tyto objekty jako součást obchodní aktivity a pak uchovávat všechny objekty jako součást jedné atomické operace. Naštěstí existuje vzor, který umožňuje tento typ chování.

### <a name="the-unit-of-work-pattern"></a>Model pracovní jednotky

Fowlera říká, že jednotka práce bude "udržovat seznam objektů ovlivněných obchodní transakcí a koordinuje zápis ze změn a řešení problémů s souběžnou kofunkčností". Je zodpovědností za jednotku práce sledovat změny objektů, které připravujeme z úložiště, a zachovat veškeré změny, které jsme v objektech provedli, když sdělíme, že je jednotka práce potvrzena. Je také zodpovědností za jednotku práce při přebírání nových objektů, které jsme přidali do všech úložišť, a vkládání objektů do databáze a také spravovat weby odstranění.

Pokud jste někdy dokončili práci s ADO.NETmi datovými sadami, už budete obeznámeni se vzorem pracovní jednotky. ADO.NET datové sady měly schopnost sledovat naše aktualizace, odstranění a vložení objektů DataRow a může (pomocí TableAdapteru) sjednotit všechny změny v databázi. Objekty DataSet ale modelují odpojenou podmnožinu podkladové databáze. Model pracovní jednotky vykazuje stejné chování, ale funguje s obchodními objekty a doménovými objekty, které jsou izolovány od kódu přístupu k datům a nevědí o databázi.

Abstrakce pro modelování jednotky práce v kódu .NET může vypadat takto:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Zveřejněním odkazů na úložiště z pracovní jednotky můžeme zajistit, aby jedna jednotka pracovního objektu mohla sledovat všechny entity vyhodnocené během obchodní transakce. Implementace metody Commit pro skutečnou pracovní jednotku má za následek to, že všechny Magic se budou sjednotit o sloučení změn v paměti s databází. 

S ohledem na odkaz IUnitOfWork může kód provádět změny obchodních objektů načtených z jednoho nebo více úložišť a uložit všechny změny pomocí operace atomické potvrzení.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Vzor opožděného zatížení

Fowlera používá k popisu "objektu, který neobsahuje všechna data, která potřebujete, ale ví, jak ho získat", používá název opožděného načítání. Transparentní opožděné načítání je důležitou funkcí při psaní testovatelné obchodního kódu a práci s relační databází. Zvažte například následující kód.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Jak se vyplní kolekce TimeCards? K dispozici jsou dvě možné odpovědi. Jednou z odpovědí je, že úložiště zaměstnanců, když se zobrazí výzva k načtení zaměstnance, vydá dotaz, který získá jak zaměstnance, tak i informace o časové kartě daného zaměstnance. V relačních databázích to obecně vyžaduje dotaz s klauzulí JOIN a může mít za následek načítání dalších informací, než kolik potřebuje aplikace. Co když aplikace nikdy nepotřebuje dotykové ovládání vlastnosti TimeCards?

Druhou odpovědí je načíst vlastnost TimeCards na vyžádání. Toto opožděné načítání je implicitní a transparentní pro obchodní logiku, protože kód nevyvolává speciální rozhraní API pro načtení informací o časové kartě. Kód předpokládá, že informace o časové kartě jsou v případě potřeby k dispozici. U opožděného načítání je zapojeno některé Magic, které obvykle zahrnuje zachycení metod při volání za běhu. Kód pro zachycení je zodpovědný za komunikaci s databází a načítání informací o časových kartách, zatímco obchodní logika zůstane bezplatná obchodní logikou. Toto opožděné zatížení Magic umožňuje obchodnímu kódu izolovat sám sebe od operací načítání dat a výsledkem je další testovatelné kód.

Nevýhodou opožděného zatížení je, že *když aplikace potřebuje* informace o časové kartě, kód provede další dotaz. Nejedná se o problém s mnoha aplikacemi, ale pro aplikace a aplikace s výkonem, které se přecházejí prostřednictvím řady objektů zaměstnanců a spouští dotaz k načtení časových karet během každé iterace smyčky (problém se často označuje jako N + 1. problém s dotazem), opožděné načítání je přetahování. V těchto scénářích může aplikace chtít eagerly získat informace o časové kartě, a to nejúčinnějším způsobem.

Naštěstí vám ukážeme, jak EF4 podporuje implicitní opožděné načítání a efektivní Eager načítání při přesunu do další části a implementace těchto vzorů.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementace vzorů s Entity Framework

Dobrá zpráva je, že všechny vzory návrhu, které jsme popsali v poslední části, jsou pro implementaci pomocí EF4 jednoduché. Abychom předvedli použití jednoduché aplikace ASP.NET MVC k úpravám a zobrazení zaměstnanců a jejich přidružených informací o časových kartách. Začneme pomocí následujících "jednoduchých starých objektů CLR" (POCOs). 

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

Tyto definice tříd se mírně změní, protože zkoumáme různé přístupy a funkce EF4, ale záměr je zachovat tyto třídy jako trvalost (PI), jak je to možné. Objekt PI neví, *jak*, nebo i *když*se jedná o stav, který se nachází v databázi. PI a POCOs se dostanou rukou s testovatelné softwarem. Objekty využívající přístup k POCO jsou méně omezené, pružnější a lépe se testují, protože mohou fungovat bez přítomnosti databáze.

POCOs je možné v aplikaci Visual Studio vytvořit pomocí model EDM (Entity Data Model) (EDM) (viz obrázek 1). K vygenerování kódu pro naše entity nebudeme používat EDM. Místo toho chceme, abychom používali entity, které jsme lovingly, na základě ruky. K vygenerování našeho schématu databáze použijeme jenom model EDM a poskytneme metadata, která EF4 potřebuje k namapování objektů do databáze.

![EF test_01](~/ef6/media/eftest-01.jpg)

**Obrázek 1**

Poznámka: Pokud chcete model EDM nejprve vyvinout, je možné vygenerovat čistý POCO kód z modelu EDM. Můžete to provést pomocí rozšíření sady Visual Studio 2010, které poskytuje tým programovatelnosti dat. Chcete-li stáhnout rozšíření, spusťte Správce rozšíření z nabídky nástroje v aplikaci Visual Studio a vyhledejte online galerii šablon pro "POCO" (viz obrázek 2). Pro EF je k dispozici několik šablon POCO. Další informace o použití šablony najdete v části " [Návod: Šablona POCO pro Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![EF test_02](~/ef6/media/eftest-02.png)

**Obrázek 2**

Z tohoto počátečního bodu POCO budeme prozkoumat dva různé přístupy k testovatelné kódu. První přístup, který se nazývá přístup EF, protože využívá abstrakce z rozhraní Entity Framework API k implementaci jednotek práce a úložišť. Ve druhém postupu vytvoříme naše vlastní abstrakce úložiště a pak uvidíte výhody a nevýhody jednotlivých přístupů. Začneme prozkoumáním přístupu EF.  

### <a name="an-ef-centric-implementation"></a>Implementace orientované na EF

Vezměte v úvahu následující akci kontroleru z projektu ASP.NET MVC. Akce načte objekt Employee a vrátí výsledek pro zobrazení podrobného zobrazení zaměstnance.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Je kód testovatelné? K dispozici jsou alespoň dva testy, které potřebujeme k ověření chování akce. Nejdříve byste chtěli ověřit, že akce vrátí správné zobrazení – jednoduchý test. Chtěli bychom také napsat test, abychom ověřili, že akce načte správného zaměstnance, a chceme to udělat bez spuštění kódu pro dotazování databáze. Nezapomeňte izolovat testovaný kód. Při izolaci se ověří, že se test nezdařil z důvodu chyby v kódu pro přístup k datům nebo v konfiguraci databáze. Pokud se test nezdaří, poznáte, že došlo k chybě v logice kontroleru a ne v některé součásti systému nižší úrovně.

Aby se zajistila izolace, budete potřebovat několik abstrakcí, jako jsou rozhraní, které jsme dříve předložili pro úložiště a jednotky práce. Zapamatujte si, že vzor úložiště je navržený tak, aby vypravil mezi doménovými objekty a vrstvou mapování dat. V tomto scénáři EF4 *je* vrstva mapování dat a již poskytuje abstrakci podobnou úložišti s názvem IObjectSet&lt;t&gt; (z oboru názvů System. data. Objects). Definice rozhraní vypadá následovně.

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

IObjectSet&lt;T&gt; splňuje požadavky pro úložiště, protože se podobá kolekci objektů (přes IEnumerable&lt;T&gt;) a poskytuje metody pro přidání a odebrání objektů z simulované kolekce. Metody připojení a odpojení zpřístupňují další možnosti rozhraní EF4 API. Pokud chcete používat IObjectSet&lt;T&gt; jako rozhraní pro úložiště, potřebujeme k vytváření vazeb úložišť určitou jednotku práce.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Jedna konkrétní implementace tohoto rozhraní se bude mluvit SQL Server a je snadné ho vytvořit pomocí třídy ObjectContext z EF4. Třída ObjectContext je skutečnou jednotkou práce v rozhraní EF4 API.

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

Zavedení IObjectSet&lt;T&gt; je stejně snadné jako volání metody metody CreateObjectSet objektu ObjectContext. Na pozadí Framework použije metadata, která jsme poskytli v modelu EDM, k vytvoření konkrétního&lt;u ObjectSet&gt;. Zavedeme se vrácením rozhraní IObjectSet&lt;T&gt;, protože pomůže zachovat testování v kódu klienta.

Tato konkrétní implementace je užitečná v produkčním prostředí, ale musíme se zaměřit na to, jak budeme používat naši abstrakci IUnitOfWork k usnadnění testování.

### <a name="the-test-doubles"></a>Dvojitá přesnost testu

K izolaci akce kontroleru potřebujeme, aby bylo možné přepínat mezi skutečnou jednotkou práce (pomocí objektu ObjectContext) a dvojitou zkušební jednotkou nebo "falešnou" jednotkou práce (provádění operací v paměti). Běžným přístupem k provedení tohoto typu přepínání je neumožnit, aby řadič MVC vytvořil instanci pracovní jednotky, ale místo toho předávala jednotku práce do kontroleru jako parametr konstruktoru.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Výše uvedený kód je příkladem vkládání závislostí. Nepovolujeme, aby kontroler vytvořil závislost (pracovní jednotka), ale vložil závislost do kontroleru. V projektu MVC se běžně používá vlastní továrna kontroleru v kombinaci s kontejnerem inverze správy (IoC) pro automatizaci vkládání závislostí. Tato témata jsou nad rámec tohoto článku, ale další informace najdete na konci tohoto článku.

Napodobeninná jednotka práce, kterou můžeme použít pro testování, může vypadat takto.

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

Všimněte si, že falešná jednotka práce zveřejňuje svěřenou vlastnost. Někdy je užitečné přidat funkce do falešné třídy, která usnadňuje testování. V tomto případě je možné snadno sledovat, jestli kód potvrdí pracovní jednotku, a to tak, že zkontroluje vlastnost potvrzená.

Pro uchování objektů Employee a docházky v paměti budeme také potřebovat falešně IObjectSet&lt;T&gt;. Jednu implementaci můžeme poskytnout pomocí generických typů.

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

Tento test zdvojnásobí svoji práci na základní objekt HashSet –&lt;T&gt;. Všimněte si, že IObjectSet&lt;T&gt; vyžaduje obecné omezení vynucující T jako třídu (odkazový typ), a také nám vynutí implementaci IQueryable&lt;T&gt;. Je snadné vytvořit kolekci v paměti jako sadu IQueryable&lt;T&gt; pomocí standardního operátoru LINQ AsQueryable.

### <a name="the-tests"></a>Testy

Tradiční testy jednotek budou používat jednu testovací třídu pro všechny akce v jednom kontroleru MVC. Tyto testy můžeme napsat nebo libovolný typ testu jednotek pomocí paměti, kterou jsme vytvořili v paměti. Pro účely tohoto článku se ale vyhneme přístupu ke třídě monolitické test a místo toho seskupme naše testy, abyste se mohli soustředit na konkrétní funkčnost.  Například "vytvořit nového zaměstnance" může být funkce, kterou chceme testovat, takže použijeme jednu testovací třídu pro ověření, že je jediná akce kontroleru zodpovědná za vytvoření nového zaměstnance.

K dispozici je několik běžných instalačních kódů, které potřebujeme pro všechny tyto jemně odstupňované testovací třídy. Například musíme vždy vytvořit naše úložiště v paměti a falešné pracovní jednotky. Potřebujeme také instanci řadiče zaměstnanců s vloženou falešnou jednotkou práce. Tento kód společné instalace budeme sdílet napříč testovacími třídami pomocí základní třídy.

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

"Matka" objektu, který používáme v základní třídě, je jedním z běžných vzorů pro vytváření testovacích dat. Matka objektu obsahuje metody pro vytváření instancí testovacích entit pro použití v několika testovacích přípravcích.

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

EmployeeControllerTestBase můžeme použít jako základní třídu pro určitý počet zkušebních přípravek (viz obrázek 3). Každý testovací armatura provede test konkrétní akce kontroleru. Například jeden testovací přípravek se soustředí na testování akce vytvoření používané během žádosti HTTP GET (zobrazení pro vytvoření zaměstnance) a jiný přípravek se zaměří na akci vytvoření použitou v požadavku HTTP POST (k získání informací odeslaných uživatel, který má vytvořit zaměstnance). Každá odvozená třída je pouze odpovědná za nastavení potřebná v jeho konkrétním kontextu a k poskytnutí kontrolních výrazů potřebných k ověření výsledků pro svůj konkrétní testovací kontext.

![EF test_03](~/ef6/media/eftest-03.png)

**Obrázek 3**

Zásady vytváření názvů a styl testu, které jsou zde uvedeny, nejsou vyžadovány pro testovatelné kód – jedná se pouze o jediný přístup. Obrázek 4 znázorňuje testy spuštěné v reostřejším modulu plug-in strojového testu jet pro Visual Studio 2010.

![EF test_04](~/ef6/media/eftest-04.png)

**Obrázek 4**

U základní třídy pro zpracování kódu sdíleného nastavení jsou testy jednotek každé akce kontroleru malé a snadno se zapisují. Testy se spustí rychle (vzhledem k tomu, že provádíme operace v paměti) a nemělo by dojít k selhání z důvodu nesouvisející infrastruktury nebo životního prostředí (protože jsme jednotku v rámci testu izolovanou).

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

V těchto testech základní třída provádí většinu práce při instalaci. Zapamatujte si, že konstruktor základní třídy vytvoří úložiště v paměti, falešnou pracovní jednotku a instanci třídy EmployeeController. Testovací třída je odvozena z této základní třídy a zaměřuje se na konkrétní testování metody Create. V takovém případě se konkrétní informace povaří do kroků "uspořádat, ACT a Assert", které se zobrazí v jakémkoli postupu testování částí:

-   Vytvořte objekt Novýzaměstnanec pro simulaci příchozích dat.
-   Vyvolejte akci vytvoření EmployeeController a předejte ji do Novýzaměstnanec.
-   Ověřte, že akce vytvořit generuje očekávané výsledky (zaměstnanec se zobrazí v úložišti).

To, co jsme vytvořili, nám umožňuje testovat jakékoli EmployeeController akce. Například když zapisujeme testy pro akci indexu řadiče zaměstnanců, můžeme dědit z testovací základní třídy a vytvořit stejné základní nastavení pro naše testy. Znovu vytvoří základní třídu úložiště v paměti, falešnou pracovní jednotku a instanci EmployeeController. Testy pro akci indexu se musí soustředit jenom na vyvolání akce indexu a testování kvality modelu, který akce vrátí.

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

Testy, které vytváříme s napodobeniny v paměti, jsou orientované k testování *stavu* softwaru. Když například otestujete akci vytvoření, chceme po provedení akce vytvoření zkontrolovat stav úložiště – úložiště bude obsahovat nového zaměstnance?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Později se podíváme na testování na základě interakce. Testování založené na interakcích se zeptá, jestli testovaný kód vyvolal správné metody pro naše objekty a předal správné parametry. Prozatím se podíváme na pokrytí dalšího vzoru návrhu – opožděné zatížení.

## <a name="eager-loading-and-lazy-loading"></a>Eager načítání a opožděné načítání

V určitém okamžiku webové aplikace ASP.NET MVC můžeme chtít zobrazit informace o zaměstnanci a zahrnout do něj přidružené časové karty zaměstnance. Můžete mít například souhrnné zobrazení časových karet, které zobrazuje jméno zaměstnance a celkový počet časových karet v systému. Je možné provést několik přístupů k implementaci této funkce.

### <a name="projection"></a>Projekce

Jedním ze způsobů, jak vytvořit souhrn, je vytvořit model vyhrazený pro informace, které chceme zobrazit v zobrazení. V tomto scénáři by model vypadal jako následující.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Všimněte si, že EmployeeSummaryViewModel není entita – jinými slovy, že v databázi nechcete uchovávat. Tuto třídu budeme používat jenom k náhodnému rozmístění dat do zobrazení v rámci silně typovaného způsobu. Model zobrazení je jako objekt pro přenos dat (DTO), protože neobsahuje žádné chování (žádné metody) – pouze vlastnosti. Vlastnosti budou obsahovat data, která je potřeba přesunout. Je snadné vytvořit instanci tohoto modelu zobrazení pomocí operátoru standardní projekce LINQ – operátor SELECT.

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

Výše uvedený kód obsahuje dvě významné funkce. First – kód je snadno testován, protože je stále snadné ho sledovat a izolovat. Operátor Select funguje stejně jako i v případě, že se jedná o falešné místo v paměti, protože se jedná o skutečnou pracovní jednotku.

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

Druhá významná funkce je způsob, jakým kód umožňuje EF4 generovat jediný efektivní dotaz pro sestavování informací o zaměstnanci a časové kartě společně. Do stejného objektu jsme načetli informace o zaměstnancích a časové kartě, aniž byste museli používat žádná speciální rozhraní API. Kód vyjadřuje pouze informace, které vyžaduje, pomocí standardních operátorů LINQ, které pracují se zdroji dat v paměti a také se vzdálenými zdroji dat. EF4 byl schopný přeložit stromy výrazů generované dotazy LINQ a kompilátorem C\# do jediného a efektivního dotazu T-SQL.

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

Existují jiné časy, kdy nechci pracovat s modelem zobrazení nebo objektem DTO, ale s reálnými entitami. Když ví, že potřebujeme zaměstnance *a* časové karty zaměstnanců, můžeme eagerly načíst související data nenápadně a efektivně.

### <a name="explicit-eager-loading"></a>Explicitní načítání Eager

Když chceme eagerly informace o entitách souvisejících s načtením, potřebujeme nějaký mechanismus pro obchodní logiku (nebo v tomto scénáři logiku akční logiky), abyste vyjádřili přání k úložišti. Třída EF4 ObjectQuery&lt;T&gt; definuje metodu include pro určení souvisejících objektů, které mají být načteny během dotazu. Nezapomeňte, že EF4 ObjectContext zpřístupňuje entity prostřednictvím konkrétního&gt; třídy ObjectSet&lt;T, která dědí z ObjectQuery&lt;T&gt;.  Pokud jsme používali odkazy na ObjectSet&lt;T&gt; v naší akci kontroleru, mohli jsme napsat následující kód, který určí Eager načtení informací o časové kartě pro každého zaměstnance.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Nicméně vzhledem k tomu, že se snažíme zachovat náš kód testovatelné, Nezveřejňujeme&gt; ObjectSet&lt;T, než je skutečná jednotka třídy Work. Místo toho spoléháme na rozhraní&gt; IObjectSet&lt;T, které je snazší, ale IObjectSet&lt;T&gt; nedefinuje metodu include. Krásy jazyka LINQ je, že můžeme vytvořit vlastní operátor include.

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

Všimněte si, že tento operátor include je definován jako metoda rozšíření pro IQueryable&lt;T&gt; namísto IObjectSet&lt;T&gt;. Díky tomu je možné využít metodu s širší škálou možných typů, včetně IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;a ObjectSet&lt;T&gt;. V případě, že se podkladová sekvence nejedná o originální EF4 ObjectQuery&lt;T&gt;, nedošlo k žádnému poškození a operátor include je no-op. Pokud *je* podkladová sekvence ObjectQuery&lt;t&gt; (nebo je odvozená z ObjectQuery&lt;t&gt;), pak EF4 uvidí náš požadavek na další data a formuluje správný dotaz SQL.

Díky tomuto novému operátorovi můžeme explicitně vyžádat Eager načtení informací o časové kartě z úložiště.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Při spuštění proti reálnému objektu ObjectContext vytvoří kód následující jeden dotaz. Dotaz shromažďuje dostatek informací z databáze v jedné cestě, aby vyhodnotit objekty zaměstnanců a plně naplnily jejich vlastnost TimeCards.

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

Skvělé novinky je kód v rámci metody Action zůstane plně testovatelné. Nemusíme poskytovat žádné další funkce pro naše napodobeniny, aby podporovaly operátor include. Chybné zprávy: museli jsme použít operátor include uvnitř kódu, který jsme chtěli zachovat pro ignorování. Toto je základní příklad typu kompromisů, které budete muset vyhodnotit při sestavování kódu testovatelné. K dispozici jsou situace, kdy potřebujete mít obavy z nevracení paměti mimo abstrakci úložiště pro splnění cílů výkonu.

Alternativa k načtení Eager je opožděné načítání. Opožděné načítání znamená, *že nepotřebujeme* náš podnikový kód, aby explicitně informoval požadavek na přidružená data. Místo toho používáme entity v aplikaci a pokud jsou potřeba další data Entity Framework načte data na vyžádání.

### <a name="lazy-loading"></a>Opožděné načítání

Můžete si snadno představit situaci, kdy nevíte, jaká data bude potřebovat obchodní logika. Je možné, že logika potřebuje objekt Employee, ale můžeme se rozvětvit na různé cesty spuštění, kde některé z těchto cest vyžadují informace o časové kartě od zaměstnance a některé ne. Podobné scénáře jsou ideální pro implicitní opožděné načítání, protože data se zobrazí podle potřeby.

Opožděné načítání, označované také jako odložené načítání, umísťuje některé požadavky na naše objekty entity. POCOs s true Persistence ignorování by nedokázala dosáhnout žádného požadavku z trvalé vrstvy, ale ignorování pravdivého přetrvávání je prakticky nemožné.  Místo toho měřím ignorování Persistence ve relativních stupních. By se unfortunate, pokud potřebujeme dědit ze základní třídy s trvalými stálostmi, nebo použít specializovanou kolekci k dosažení opožděného načítání v POCOs. Naštěstí EF4 má méně rušivé řešení.

### <a name="virtually-undetectable"></a>Prakticky nezjistitelné

Při použití objektů POCO může EF4 dynamicky generovat běhové proxy objekty pro entity. Tyto proxy objekty neviditelně balí materializované POCOs a poskytují další služby zachycením jednotlivých vlastností operace get a set pro provedení další práce. Jednou takovou službou je funkce opožděného načítání, kterou hledáte. Další služba je účinný mechanismus pro sledování změn, který může nahrávat, když program změní hodnoty vlastností entity. Seznam změn používá rozhraní ObjectContext během metody SaveChanges k trvalému uchování všech upravených entit pomocí příkazů UPDATE.

Aby mohly tyto proxy fungovat, ale potřebují způsob, jak se připojit k operacím Get a set u entity, a proxy objekty dosáhnou tohoto cíle přepsáním virtuálních členů. Proto, pokud chceme mít implicitní opožděné načítání a efektivní sledování změn, musíme přejít zpět na naše definice tříd POCO a označit vlastnosti jako virtuální.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Přesto můžeme říci, že entita zaměstnance je většinou ignorována. Jediným požadavkem je použít virtuální členy a nemá vliv na testování kódu. Nepotřebujeme odvozovat ze žádné zvláštní základní třídy ani ani použít speciální kolekci vyhrazenou pro opožděné načítání. Jak kód ukazuje, je k dispozici jakákoli třída implementující rozhraní ICollection&lt;T&gt; pro ukládání souvisejících entit.

V naší pracovní jednotce je také potřeba udělat jednu menší změnu. Opožděné načítání je ve výchozím nastavení *vypnuté* , když pracujete přímo s objektem ObjectContext. Pro vlastnost předané možnosti ContextOptions můžete nastavit vlastnost, která umožňuje odložené načítání. tuto vlastnost můžeme nastavit v naší skutečné pracovní jednotce, pokud chceme povolit opožděné načítání všude.

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

Pokud je povoleno implicitní opožděné načítání, kód aplikace může použít zaměstnance a přidružené časové karty zaměstnance a zbývající blissfully nevědomosti práce požadované pro EF načíst další data.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Opožděné načítání usnadňuje psaní kódu aplikace a u proxy Magic kód zůstane zcela testovatelné. Napodobenina pracovní jednotky v paměti může jednoduše předčítat falešné entity s přidruženými daty v případě potřeby během testu.

V tuto chvíli provedeme pozornost vytvářením úložišť pomocí IObjectSet&lt;T&gt; a podíváme se na abstrakce, abyste skryli všechny známky rozhraní Persistence.

## <a name="custom-repositories"></a>Vlastní úložiště

Když jsme nejdřív v tomto článku předložili pracovní vzor pracovní jednotky, poskytli jsme vám nějaký vzorový kód, který může být v pracovní jednotce vypadat. Pojďme tento původní nápad znovu prezentovat pomocí scénáře zaměstnance a zaměstnance s časovými kartami, se kterými jsme pracovali.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Hlavním rozdílem mezi touto jednotkou práce a částí práce, kterou jsme vytvořili v poslední části, je, jak tato jednotka práce nepoužívá žádné abstrakce z EF4 Frameworku (&gt;&lt;T). IObjectSet&lt;T&gt; funguje dobře jako rozhraní úložiště, ale rozhraní API, které zpřístupňuje, nemusí dokonale sjednotit požadavky naší aplikace. V tomto nadcházejícím přístupu budeme zastupovat úložiště pomocí vlastní IRepository&lt;T&gt; abstrakce.

Mnoho vývojářů, kteří následují návrhem řízený návrh, návrhem řízený chování a návrhem metodologie řízených doménami, dávají přednost IRepository&lt;T&gt; přístup z několika důvodů. Nejprve rozhraní IRepository&lt;T&gt; představuje vrstvu "anti-poškození". Jak je popsáno v Eric Evans v rámci své pracovní knihy založené na doméně, vrstva odolná proti poškození udržuje kód vaší domény od rozhraní API infrastruktury, jako je trvalá rozhraní API. V druhé době můžou vývojáři sestavovat metody do úložiště, které splňuje přesné potřeby aplikace (jak bylo zjištěno při psaní testů). Například může být často potřeba vyhledat jednu entitu pomocí hodnoty ID, abychom mohli do rozhraní úložiště přidat metodu FindById.  Naše IRepository&lt;T&gt; definice bude vypadat jako v následujícím příkladu.

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

Všimněte si, že jsme zpátky na použití rozhraní IQueryable&lt;T&gt; k vystavování kolekcí entit. IQueryable&lt;T&gt; umožňuje stromům výrazů LINQ přesměrovat do poskytovatele EF4 a poskytnout poskytovateli holistický zobrazení dotazu. Druhou možností je vrátit rozhraní IEnumerable&lt;T&gt;, což znamená, že poskytovatel LINQ EF4 uvidí jenom výrazy sestavené uvnitř úložiště. Jakékoli seskupení, řazení a projekce provedené mimo úložiště se neskládají do příkazu SQL, který se odesílá do databáze, což může snížit výkon. Na druhé straně úložiště vracející jenom IEnumerable&lt;T&gt; výsledky nikdy nevrátí nový příkaz SQL. Oba přístupy budou fungovat a oba přístupy zůstanou testovatelné.

Je jednoduché poskytnout jednu implementaci rozhraní IRepository&lt;T&gt; pomocí obecných typů a rozhraní EF4 ObjectContext API.

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

Přístup IRepository&lt;T&gt; nám poskytuje další kontrolu nad našimi dotazy, protože klient musí vyvolat metodu pro získání entity. Uvnitř metody můžeme poskytnout další kontroly a operátory LINQ k prosazování omezení aplikace. Všimněte si, že rozhraní má dvě omezení parametru obecného typu. Prvním omezením je nevýhoda třídy chuti, kterou vyžaduje ObjectSet&lt;T&gt;. druhé omezení vynutí, aby naši entity implementovaly IEntity – abstrakce vytvořená pro aplikaci. Rozhraní IEntity vynutí, aby entity měly vlastnost čitelného ID a tuto vlastnost pak můžeme použít v metodě FindById. IEntity je definován s následujícím kódem.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity může být považováno za malé porušení trvalosti, protože pro implementaci tohoto rozhraní jsou vyžadovány naše entity. Pamatujte na to, že ignorování přetrvává v souvislosti s kompromisy a pro mnoho funkcí FindById převažuje omezení uložené rozhraním. Rozhraní nemá žádný vliv na testování.

Vytvoření instance živého IRepository&lt;T&gt; vyžaduje rozhraní ObjectContext EF4, takže konkrétní jednotka práce by měla spravovat vytváření instancí.

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

### <a name="using-the-custom-repository"></a>Používání vlastního úložiště

Použití našeho vlastního úložiště se významně neliší od použití úložiště založeného na IObjectSet&lt;T&gt;. Namísto použití operátorů LINQ přímo na vlastnost bude nejprve nutné vyvolat jednu z metod úložiště, aby bylo možné získat odkaz IQueryable&lt;T&gt;.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Všimněte si, že vlastní operátor include, který jsme dřív implementovali, bude fungovat beze změny. Metoda FindById úložiště odstraní duplikovánou logiku z akcí, které se pokoušejí načíst jednu entitu.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Neexistují žádné významné rozdíly v testování dvou přístupů, které jsme prozkoumali. Pro IRepositoryy, které jsou založené na HashSet –&lt;zaměstnanců&gt; – stejně jako to, co jsme provedli v poslední části, můžeme poskytnout falešné implementace&lt;T&gt;. Někteří vývojáři však dávají přednost použití objektů typu Object a objektové architektury objektů namísto sestavování napodobenin. Podíváme se na použití makety k otestování naší implementace a diskuze o rozdílech mezi modely a napodobeninami v další části.

### <a name="testing-with-mocks"></a>Testování s použitím makety

Existují různé přístupy k vytváření toho, co Fowlera Novák volá "test Double". Test Double (například Stunt double) je objekt, který sestavíte jako "podstavec" pro reálné a produkční objekty během testů. Úložiště v paměti, které jsme vytvořili, jsou pro úložiště, která komunikují SQL Server, testována dvojitá. Zjistili jsme, jak používat tyto testy-Double při testování částí k izolaci kódu a udržování testů v rychlém provozu.

Test podvoje, že jsme vytvořili reálné a funkční implementace. Na pozadí každý z nich ukládá konkrétní kolekci objektů a přidá a odebere objekty z této kolekce při manipulaci s úložištěm během testu. Někteří vývojáři, jako je sestavení testu, tento způsob zdvojnásobí – s reálným kódem a pracovními implementacemi.  Tyto dva testy jsou povolány jako *falešné*. Mají funkční implementace, ale nejsou dostatečně reálné pro použití v produkčním prostředí. Falešné úložiště ve skutečnosti nezapisuje do databáze. Falešný server SMTP ve skutečnosti neposílá e-mailovou zprávu přes síť.

### <a name="mocks-versus-fakes"></a>Modely versus napodobeniny

K dispozici je jiný typ *testu, známý jako objekt typu*. I když napodobeniny mají funkční implementace, jsou modely dodávány bez implementace. V případě, že je k dispozici maketa objektového rozhraní, sestavíme tyto objekty v době běhu a použijeme je jako test dvojitých. V této části budeme používat Open Source napodobnou rozhraní MOQ. Tady je jednoduchý příklad použití MOQ k dynamickému vytváření testovacího zdvojnásobení pro úložiště zaměstnanců.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Požádáme MOQ o IRepository implementaci&gt;&lt;zaměstnanců a sestavíme ji dynamicky. K objektu, který implementuje IRepository&lt;&gt; zaměstnanců, se můžeme dostat přístupem k vlastnosti objektu objektu makety&lt;T&gt;. Jedná se o vnitřní objekt, který můžeme předat do našich řadičů a nedokáže zjistit, jestli se jedná o dvojitý test nebo reálné úložiště. Můžeme vyvolat metody objektu stejně jako bychom vyvolali metody u objektu s skutečnou implementací.

Je nutné zajímat, co bude v případě, kdy vyvolá metodu Add, vyvoláno. Vzhledem k tomu, že není k dispozici žádná implementace za objektem makety, nelze přidat žádnou akci. Na pozadí se nevyskytuje žádná konkrétní kolekce, jako bychom měli napsané falešné, takže se zaměstnanec zahodí. Co je návratová hodnota FindById? V takovém případě objekt makety dělá pouze to, co může udělat, což vrátí výchozí hodnotu. Vzhledem k tomu, že vracíme typ odkazu (zaměstnanec), vrácená hodnota je hodnota null.

Napodobení může bezcenné zvuk; Existují však dvě další funkce popsaných prvků, o kterých jsme mluvilii. Nejprve rozhraní MOQ zaznamená všechna volání vytvořená na objektu. Později v kódu můžeme požádat o MOQ, pokud kdokoli vyvolal metodu Add nebo když někdo vyvolal metodu FindById. Později uvidíte, jak můžeme použít tuto "černou" funkci záznamu v testech.

Druhá skvělé funkce je způsob, jak můžeme použít MOQ k naprogramování objektu makety s *očekáváními*. Očekává se, že objekt Form, jak reagovat na danou interakci. Například můžeme naprogramovat očekávání do našeho objektu a sdělit mu, aby vrátila objekt Employee, když někdo vyvolá FindById. MOQ Framework používá rozhraní API pro instalaci a lambda výrazy k naprogramování těchto očekávání.

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

V této ukázce požádáme MOQ, aby dynamicky sestavil úložiště a pak jsme úložiště naprogramoval na očekávanou hodnotu. Očekává se, že objekt Form vrátí objekt pro nový zaměstnanec s hodnotou ID 5, pokud někdo vyvolá metodu FindById, která předává hodnotu 5. Tento test projde a nemuseli jsme sestavovat plnou implementaci falešného IRepository&lt;T&gt;.

Pojďme znovu přejít k dříve popsaným testům a znovu je použít k použití návrhů namísto napodobenin. Stejně jako dřív, použijeme základní třídu k nastavení společných částí infrastruktury, které potřebujeme pro všechny testy řadiče.

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

Instalační kód zůstává většinou stejný. Místo použití falešných objektů použijeme MOQ k vytvoření objektů typu Object. Základní třída uspořádá v případě, že se má v případě, že kód vyvolá vlastnost Employees, použít podrobnější úložiště. Zbytek napodobení se provede v rámci testovacích zařízení vyhrazených pro každý konkrétní scénář. Například test přípravek pro akci index nastaví napodobné úložiště tak, aby vracelo seznam zaměstnanců, když akce vyvolá metodu FindAll ve vzorovém úložišti.

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

S výjimkou toho, že naše testy vypadají podobně jako testy, které jsme předtím používali. Nicméně s možností záznamu z napodobné architektury se můžeme přiblíží k testování z jiného úhlu. V další části se podíváme na tuto novou perspektivu.

### <a name="state-versus-interaction-testing"></a>Stav versus testování interakce

Existují různé techniky, které můžete použít k otestování softwaru s využitím objektů. Jednou z možností je použít testování založené na stavu, které jsme v tomto dokumentu udělali. Testování na základě stavu poskytuje kontrolní výrazy týkající se stavu softwaru. V posledním testu vyvolali metodu Action na řadiči a provedli jste kontrolní výraz modelu, který by měl sestavit. Tady jsou některé další příklady stavu testování:

-   Ověřte, že úložiště obsahuje nový objekt Employee po provedení vytvoření.
-   Ověřte, že model obsahuje seznam všech zaměstnanců po spuštění indexu.
-   Ověřte, že úložiště neobsahuje daného zaměstnance, když se spustí odstranění.

Dalším postupem, který se zobrazí u objektů s přípravou, je ověření *interakcí*. Zatímco testování na základě stavu poskytuje kontrolní výrazy týkající se stavu objektů, testování na základě interakce poskytuje kontrolní výrazy týkající se způsobu interakce objektů. Příklad:

-   Ověřte, že řadič vyvolá metodu Add úložiště při vytváření.
-   Ověřte, že řadič vyvolá metodu FindAll úložiště, když se spustí index.
-   Ověřte, že řadič vyvolá metodu potvrzení pracovní jednotky, aby se změny uložily při spuštění úpravy.

Testování interakce často vyžaduje méně testovacích dat, protože nejsme poking v kolekcích a Ověřujeme počty. Pokud například ví, že akce vyvolá metodu FindById úložiště se správnou hodnotou, pak se tato akce pravděpodobně chová správně. Toto chování můžeme ověřit bez nastavování všech testovacích dat, která se mají vrátit z FindById.

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

Jediným nastavením požadovaným v rámci výše uvedeného testovacího přípravku je nastavení poskytované základní třídou. Když vyvoláme akci kontroleru, MOQ zaznamená interakce s modelovým úložištěm. Pomocí rozhraní MOQ API pro ověření, můžeme požádat MOQ, pokud řadič vyvolal FindById se správnou hodnotou ID. Pokud řadič nevolal metodu nebo vyvolal metodu s neočekávanou hodnotou parametru, metoda Verify vyvolá výjimku a test se nezdaří.

Tady je další příklad, jak ověřit, že akce vytvořit vyvolá potvrzení pro aktuální pracovní jednotku.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Jedním z nebezpečí při testování interakcí je, že se má v rámci určení interakcí. Schopnost objektu makety zaznamenat a ověřit každou interakci s objektem pro zápis neznamená, že test by měl zkusit ověřit každou interakci. Některé interakce jsou podrobné informace o implementaci a měli byste pouze ověřit interakce *požadované* pro splnění aktuálního testu.

Volba mezi maketami nebo napodobeninami, které jsou v podstatě, závisí na systému, který testujete, a na vašich osobních (nebo týmových) preferencích. Objekty typu Object můžou významně snížit množství kódu, který potřebujete k implementaci zdvojnásobení testu, ale ne všichni mají pohodlné programování a ověřují interakce.

## <a name="conclusions"></a>Závěry

V tomto dokumentu jsme ukázali několik přístupů k vytváření kódu testovatelné při použití ADO.NET Entity Framework pro trvalost dat. Můžeme využít předdefinované abstrakce jako IObjectSet&lt;T&gt;nebo vytvořit vlastní abstrakce, jako je IRepository&lt;T&gt;.  V obou případech podpora POCO v ADO.NET Entity Framework 4,0 umožňuje spotřebitelům těchto abstrakcí zůstat trvale ignorovatelné a vysoce testovatelné. Další funkce EF4, jako je implicitní opožděné načítání, umožňují pracovat s kódem obchodních a aplikačních služeb, aniž by se museli starat o podrobnosti o relačním úložišti dat. A nakonec se abstrakce, které vytvoříme, snadno napodobují nebo jsou falešné v rámci testů jednotek a můžeme použít tyto podvojky testu k zajištění rychlých spuštěných, vysoce izolovaných a spolehlivých testů.

### <a name="additional-resources"></a>Další prostředky

-   Robert C. Martin, " [Princip jedné zodpovědnosti](https://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowlera, [katalog vzorů](https://www.martinfowler.com/eaaCatalog/index.html) ze *vzorů architektury podnikové aplikace*
-   Griffin Caprio, " [vkládání závislostí](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog o programovatelnosti dat " [Návod: Vývoj řízený testováním pomocí Entity Framework 4,0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Blog o programovatelnosti dat, " [používání úložiště a pracovní jednotky vzorů s Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Aaron Jensen, " [představení specifikací počítačů](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Novák, " [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [návrh založený na doméně](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowlera, " [napodobeniny nejsou](https://martinfowler.com/articles/mocksArentStubs.html)zástupné kódy"
-   Martin Fowlera, " [test Double](https://martinfowler.com/bliki/TestDouble.html)"
-   [Moq](https://code.google.com/p/moq/)

### <a name="biography"></a>Biografie

Scott Allen je členem technického personálu v Pluralsight a zakladatel OdeToCode.com. V 15 letech komerčního vývojového softwaru se Scott pracoval na řešeních, která jsou na všech počítačích se systémem, která obsahují 8bitové integrované zařízení, aby vysoce škálovatelné webové aplikace ASP.NET. Na svém blogu můžete přistihnout na OdeToCode nebo na Twitteru na [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).
