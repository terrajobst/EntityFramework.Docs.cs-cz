---
title: Testovatelnost a Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 0ddf72ab46e2d67dc8a9cf75cbd40430352c5210
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490529"
---
# <a name="testability-and-entity-framework-40"></a>Testovatelnost a Entity Framework 4.0
Scott Allen

Publikováno: Květen 2010

## <a name="introduction"></a>Úvod

Tento dokument white paper popisuje a ukazuje, jak psát testovatelného kód pomocí technologie ADO.NET Entity Framework 4.0 a Visual Studio 2010. Tento dokument se nepokusí se zaměřit na konkrétní testovací metody, jako je návrh řízený testováním (TDD) nebo návrh na základě chování (BDD). Místo toho tento dokument se zaměřuje na tom, jak napsat kód, který používá rozhraní ADO.NET Entity Framework, ale zůstává snadno izolovat a testovat automatizovaně. Podíváme se na běžných vzorů návrhu, které usnadňují testování v datech scénáře přístupu a zjistit, jak použít tyto vzorce při použití rozhraní. Také podíváme na konkrétní funkce rozhraní Framework, pokud chcete zobrazit, jak můžete tyto funkce fungují v testovatelného kód.

## <a name="what-is-testable-code"></a>Co je Testovatelného kód?

Schopnost ověřit softwarového produktu pomocí automatizované testy jednotky nabízí řadu výhod žádoucí. Všichni ví, že dobré testy se sníží počet vady softwaru aplikace a zvýšení kvality vaší aplikace – ale s testy jednotek v místě dalece přesahuje právě hledání chyb.

Sadu testů jednotky dobré umožňuje vývojový tým ušetřit čas a udržíme si kontrolu softwaru, který vytvoří. Tým může provádět změny existující kód Refaktorovat, přepracování a změnu struktury softwaru splňovat nové požadavky a přidat nové komponenty do aplikace všechny zároveň budete vědět sadu testů můžete ověřit, chování aplikace. Testování jednotek je součást cyklu rychlé zpětné vazby a usnadněte změnu zachování udržovatelnosti softwaru jako zvýšení složitosti.

Testování částí se dodává s cenu, ale. Tým má investovat čas k vytváření a údržbě testů jednotek. Množství úsilí, abyste tyto testy můžete vytvářet přímo souvisí s **testovatelnosti** základní softwaru. Jak snadné je software k testování? Tým softwaru návrhu s testovatelností v úvahu se vytvořit efektivní testy rychleji, než tým pracovat bez možností intenzivního testování softwaru.

Microsoft proto navrhl ADO.NET Entity Framework 4.0 (EF4) s testovatelností v úvahu. To neznamená, že vývojáři bude psaní testů jednotek pro samotný kód rozhraní framework. Místo toho testovatelnosti cíle pro EF4 usnadňují vytváření testovatelného kód, který vytváří jako nadstavby rozhraní framework. Předtím, než se podíváme na konkrétní příklady, je vhodné pochopit kvality testovatelného kód.

### <a name="the-qualities-of-testable-code"></a>Kvality Testovatelného kód

Kód, který je snadno testovat bude mít vždy měly aspoň dva znaky. První, testovatelného kód je snadné **sledovat**. Zadaný některé sadu vstupů, by mělo být snadné sledovat výstup kódu. Například následující metodu testování je snadné vzhledem k tomu, že metoda přímo vrátí výsledek výpočtu.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Testování metodu je obtížné, pokud metoda zapíše vypočtená hodnota do soketu síťové, databázové tabulky nebo soubor jako v následujícím kódu. Test musí provést další práce k načtení hodnoty.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Za druhé, je snadné testovatelného kód **izolovat**. Použijeme následující pseudo kód jako chybný příklad možností intenzivního testování kódu.

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

Metoda je snadné sledovat – můžeme předat v zásadách pojištění a ověřte, že návratová hodnota odpovídá očekávaných výsledků. Ale pro testovací metodu potřebujeme mít databázi nainstalovanou s správné schéma a konfigurovat SMTP server pro případ, metoda se pokusí odeslat e-mailu.

Testování částí pouze chce, aby se k ověření logiky výpočtu uvnitř metody, ale test může selhat, protože e-mailový server je offline, nebo přesunout databázového serveru. Obě tyto chyby nesouvisí s chování, které chce, aby se test k ověření. Chování je obtížné izolovat.

Vývojářům, kteří snažit se psát testovatelného kód často snažit Udržovat oddělené oblasti zájmu v kódu, jsou zápisu. Výše uvedené metody by měly soustředit na obchodní výpočty a delegovat podrobnosti implementace databáze a e-mailu na jiné komponenty. Robert C. Martin označuje jako jednotné zásady odpovědnosti. Objekt by měl zapouzdření jednoho úzký odpovědnosti, jako je výpočet hodnoty zásad. Všechny ostatní databáze a oznámení práce by měl být odpovědnost některý jiný objekt. Kód napsaný tímto způsobem je jednodušší oddělit, protože se zaměřuje na jednu úlohu.

V rozhraní .NET máme abstrakce, musíme postupovat podle principu jednu zodpovědnost a dosáhnout izolace. Můžeme používat definice rozhraní a vynutit kód, který použije místo konkrétního typu implementujícího typ abstrakce rozhraní. Později v tomto dokumentu uvidíme, jak metoda jako chybný příklad uvedený výše může spolupracovat s rozhraní, které *vypadat* jako bude komunikovat s databáze. Během testu můžeme však nahradit fiktivní implementace, která nebude komunikovat s databáze, ale místo toho obsahuje data v paměti. Tuto fiktivní implementace izoluje kód z nesouvisejících potíže kód přístupu k datům nebo konfigurace databáze.

Existují další výhody izolace. Obchodní výpočet v metodě poslední by mělo trvat pouze několik milisekund, než ke spuštění, ale samotný test může spustit několik sekund jako směrování kódu kolem sítě a přednášky na různé servery. Testy jednotek by měl spustit rychlé usnadnit malé změny. Testy jednotek by měl také být opakovatelným a není nezdaří, protože nesouvisí se test komponenty došlo k problému. Psaní kódu, který je snadno sledovat a k izolování znamená, že vývojáři budou mít usnadní psaní testů pro kód, trávit méně času čekáním na testy ke spuštění a další důležité je, strávit míň času sledování chyby v kódu, které neexistují.

Snad můžete pochopit výhody testování a pochopit vlastnosti, které je třeba testovatelného kód. Nepovedlo se chystáte se vyřešit jak napsat kód, který funguje s EF4 k uložení dat do databáze přitom pozorovatelné a snadno izolovat, ale nejprve jsme vám zúžit našim hlavním cílem fattica možností intenzivního testování návrhů pro přístup k datům.

## <a name="design-patterns-for-data-persistence"></a>Způsoby návrhu pro trvalost dat

Obě chybný příkladů uvedených výše má příliš mnoho odpovědnosti. V prvním příkladu chybný došlo k provedení výpočtu *a* zápis do souboru. V druhém příkladu chybný museli číst data z databáze *a* provádět obchodní výpočet *a* odeslání e-mailu. Navržením menší metody, které oddělení připomínky a delegovat na ostatní součásti provedete skvělé pokroku v oblasti zápisu testovatelného kód. Cílem je vytvářet funkce vytváření akce v malém rozsahu a zaměřeně abstrakce.

Pokud jde o trvalost dat malé a jsou proto společné cílené abstrakce, které Těšíme se na jste bylo uvedeno jako vzory návrhu. Kniha Martina Fowlera vzory Enterprise Application Architecture byl první práce, která popisují tyto vzory v tisku. Předtím, než vám ukážeme, jak tyto technologie ADO.NET Entity Framework implementuje a funguje s tyto vzory poskytujeme stručný popis tyto vzory v následujících částech.

### <a name="the-repository-pattern"></a>Model úložiště

Fowler vám říká úložiště "zprostředkovává mezi doménou a data vrstev mapování rozhraní kolekce jako pro přístup k objektům domény". Cílem model úložiště je izolovat kódu z minutiae přístup k datům a jak jsme viděli starší izolace je požadovaná vlastnost pro testovatelnost.

Klíčem k izolaci je, jak úložiště poskytuje objekty pomocí rozhraní jako v kolekci. Logika, kterou píšete úložiště nemá představu, jak používat úložiště sloučit objekty, které požadujete. Úložiště může komunikovat s databází nebo objektů může vrátit pouze z kolekce v paměti. Všechno, co váš kód potřebuje vědět, je, že úložiště se zobrazí k udržení kolekce, a můžete načíst, přidat nebo odstranit objekty z kolekce.

V existujících aplikací .NET na konkrétní úložiště často dědí z obecného rozhraní vypadat asi takto:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Provedeme několik změn s definice rozhraní, když jsme EF4 poskytnout implementaci, ale základní princip zůstává stejná. Kód můžete použít konkrétní úložiště implementace tohoto rozhraní k načtení entity podle jeho hodnotu primárního klíče, k načtení kolekce entit na základě vyhodnocení predikátu, nebo jednoduše načíst všechny dostupné entity. Kód můžete také přidávat a odebírat entity prostřednictvím rozhraní úložiště.

S ohledem objekty IRepository zaměstnance, kód můžete provádět následující operace.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Protože kód používá rozhraní (IRepository zaměstnanců), abychom mohli kód poskytovat různé implementace rozhraní. Jedna implementace může být implementace se opírá o EF4 a uchování objektů do databáze Microsoft SQL Server. Jinou implementaci (jedna, které používáme během testování) může opírá zaměstnance seznam objektů v paměti. Rozhraní vám pomůže dosáhnout izolace v kódu.

Všimněte si, IRepository&lt;T&gt; rozhraní nevystavuje operace uložit. Jak jsme aktualizaci existujících objektů? Můžete narazit na IRepository definice, které zahrnují operace Uložit a implementace těchto úložišť muset okamžitě uchování objektu do databáze. V mnoha aplikacích nechceme však zachovat objekty jednotlivě. Místo toho chceme Oživte objekty, třeba z různých úložišť, upravit obchodních aktivit v rámci těchto objektů a pak zachovat všechny objekty v rámci jediné atomické operace. Naštěstí je vzor, podle kterého byl tento typ chování.

### <a name="the-unit-of-work-pattern"></a>Jednotka pracovní postup

Fowler vám říká, určitou jednotku práce se "udržovat seznam objektů ovlivněna obchodní transakce a koordinuje zápisu mimo změny a řešení problémů souběžnosti". Zodpovídá za jednotky práce a sledování změn objektů jsme Vdechněte život z úložiště a zachovat všechny změny, které jsme udělali na objekty při tom jednotku práce tím potvrdíte změny. Je také odpovědnost jednotku práce se nové objekty, které jsme přidali na všechna úložiště a vložit do databáze a také restartovat odstranění objektů.

Pokud jste někdy provedli veškerou práci s datovými sadami ADO.NET pak už budete znát jednotka pracovní postup. Datovými sadami ADO.NET měli možnost sledovat naše aktualizace, odstranění a vložení DataRow objektů a může (s použitím objektu TableAdapter) odsouhlaste naše změny do databáze. Ale datovou sadu objektů modelu podmnožinu podkladové databázi odpojené. Jednotka pracovní postup vykazuje stejné chování, ale s obchodních objektů a objektů domény, které jsou izolované od dat přístupový kód a bez ohledu databázi.

Abstrakce pro modelování jednotku práce v kódu .NET může vypadat nějak takto:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Zveřejněním úložiště odkazy z jednotky práce, kterou zajišťujeme jednu jednotku práce objektu má možnost sledovat všechny entity materializovaného během obchodní transakce. Implementace metody potvrzení pro skutečné pracovní jednotka je všechny kouzla sloučit změny v paměti s databází. 

Zadaný odkaz na IUnitOfWork, kód provést změny načíst z jednoho nebo více úložišť pro obchodní objekty a uložte všechny změny pomocí atomické operace potvrzení.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Vzor opožděné načtení

Opožděné načtení názvu fowler vám používá k popisu "objekt, který nebude obsahovat všechna data potřebujete, ale ví, jak získat". Transparentní opožděné načtení je důležité funkce mít při psaní kódu s možností intenzivního testování obchodní a práci s relační databáze. Jako příklad zvažte následující kód.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Jak se vyplní časové výkazy kolekce? Existují dva možné odpovědi. Jedna odpověď je, že zaměstnanec úložiště, když se zobrazí výzva k načtení zaměstnanec, vydá dotaz pro načtení zaměstnanec spolu s informace o přidružené čas kartě zaměstnance. V relačních databázích to obvykle vyžaduje dotazy s klauzulí JOIN a může vést k načtení více informací, než aplikace potřebuje. Co když aplikace potřebuje nikdy dotýkat vlastnost časové výkazy?

Druhá odpověď je načíst vlastnost časové výkazy "na vyžádání". Toto opožděné načtení je implicitní a pro obchodní logiku transparentní, protože kód se nevyvolá speciální rozhraní API se načíst informace o čas kartě. Kód předpokládá, že čas kartě informace jsou k dispozici v případě potřeby. Je součástí opožděné načtení, která v podstatě runtime zachycení volání metod některé magic. Prověřuje zachycovací kód je zodpovědná za mluvit k databázi a načítání informací o čas kartě a ponechání zdarma na obchodní logiku obchodní logiku. Tento opožděné načtení magic umožňuje obchodní kód samotné izolovat operace načítání dat a za následek více možností intenzivního testování kódu.

Nevýhodou opožděné načtení je, že když aplikace *nemá* potřebují informace o čas kartě kód se spustí další dotaz. To není žádný problém pro mnoho aplikací, ale výkon citlivých aplikací nebo aplikací počet objektů zaměstnance ve smyčce a spouštějícího dotaz k načtení času karty při každé iteraci smyčky (problém často označuje jako N + 1 dotaz problém), přetáhněte je opožděné načtení. V těchto scénářích aplikace může být vhodné například načíst informace o čas kartě nejefektivnějším způsobem je to možné.

Naštěstí uvidíme jak EF4 podporuje obě implicitní opožděné načtení a efektivní eager načte přesunout do další části a implementovat tyto vzory.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementace vzorů s rozhraním Entity Framework

Dobrou zprávou je, že jsou jednoduché implementace s EF4 všechny vzory návrhu, které jsme je popsáno v předchozí části. K předvedení budeme používat jednoduchou aplikaci ASP.NET MVC pro úpravy a zobrazí zaměstnancům a informace o jejich přidružených čas kartě. Začneme s použitím následujících "prostý staré CLR objektů" (POCOs). 

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

Tyto definice třídy mírně změní jako podíváme na různé přístupy a funkce EF4, ale cílem je zajistit těchto tříd jako trvalého ignorant (PÍ) co nejvíc. Objekt PI neví *jak*, nebo dokonce *Pokud*, stát, že obsahuje umístěným uvnitř databáze. Číslo PÍ a POCOs přejít těsná spolupráce s možností intenzivního testování softwaru. Objekty POCO přístup jsou méně omezené, flexibilnější a usnadňuje testování, protože můžete pracovat bez databáze k dispozici.

S POCOs na místě můžeme vytvořit Entity Data Model (EDM) v sadě Visual Studio (viz obrázek 1). EDM Nebudeme je používat ke generování kódu pro naše entity. Místo toho chcete používat entity, které jsme lovingly vytvořit ručně. Budeme používat jenom EDM pro generování naše schéma databáze a poskytněte metadata, která EF4 potřebuje k mapování objektů do databáze.

![EF test_01](~/ef6/media/eftest-01.jpg)

**Obrázek 1**

Poznámka: Pokud chcete vyvíjet první EDM model, je možné generovat čištění, POCO kód z modelu EDM. Můžete to provést pomocí rozšíření sady Visual Studio 2010 poskytované týmem programovatelnosti Data. Stáhnout rozšíření, spusťte Správce rozšíření v nabídce Nástroje v sadě Visual Studio, vyhledejte "POCO" (viz obrázek 2) online galerie šablon. Nejsou k dispozici pro EF několik šablon POCO. Další informace o použití šablony naleznete v části " [názorný postup: šablony objektů POCO pro Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![EF test_02](~/ef6/media/eftest-02.png)

**Obrázek 2**

Z tohoto POCO počáteční bod se podíváme na dva různé přístupy k testovatelného kód. První postup můžu volat EF přístup protože využívá abstrakce z rozhraní API Entity Framework pro implementaci jednotek práce a úložiště. Ve druhé metodě budeme vytvářet vlastní abstrakce vlastní úložiště a pak naleznete v tématu výhody a nevýhody obou těchto přístupů. Začneme prozkoumání EF přístup.  

### <a name="an-ef-centric-implementation"></a>Implementace zaměřenou na EF

Vezměte v úvahu následující akce kontroleru z projektu aplikace ASP.NET MVC. Akce načte objekt zaměstnance a vrátí výsledek, chcete-li zobrazit podrobné zobrazení jednotlivých zaměstnanců.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Je testovatelného kód? Nejsou k dispozici alespoň dva testy by potřebujeme si ověřit akce chování. Chtěli bychom nejprve, ověřte, že akce vrátí správné zobrazení – snadné testu. Bude také chceme napsat test ověření akce načte správné zaměstnanců, a rádi bychom to provést bez spuštění kódu pro dotazování databáze. Mějte na paměti, že chceme izolace testovaného kódu. Izolace zajistí, že není test nezdaří z důvodu chyby v kód přístupu k datům nebo konfigurace databáze. Pokud se test nezdaří, budeme vědět, že máme chyby v logice kontroleru a ne v některé součásti nižší úrovně systému.

Účelem izolace potřebujeme některé abstrakce, jako je rozhraní, které jsme uvedené výše pro úložiště a jednotek práce. Mějte na paměti, které zprostředkovává mezi objekty domény a datová vrstva mapování je určený použitému vzoru. V tomto scénáři EF4 *je* vrstvy data mapování a již poskytuje úložiště jako abstrakce s názvem IObjectSet&lt;T&gt; (z oboru názvů System.Data.Objects). Definice rozhraní vypadá takto.

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

IObjectSet&lt;T&gt; splňuje požadavky na úložiště, protože se podobá kolekci objektů (prostřednictvím typu IEnumerable&lt;T&gt;) a poskytuje metody pro přidání a odebrání objektů z simulované kolekce. Metody připojení a odpojení zpřístupňují další funkce EF4 rozhraní API. Chcete-li použít IObjectSet&lt;T&gt; jako rozhraní pro úložiště jsme jednotku práce abstrakce svázat dohromady úložiště potřebují.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Jeden konkrétní implementaci tohoto rozhraní bude komunikovat s SQL serverem a je snadné vytvořit horizontálních oddílů pomocí třídy objektu ObjectContext z EF4. Třídě ObjectContext je skutečná jednotka práce v rozhraní API EF4.

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

Přenesení IObjectSet&lt;T&gt; život je stejně jednoduché jako volání metody CreateObjectSet objektu ObjectContext. Na pozadí rozhraní použije metadata jsme součástí modelu EDM vytvoří konkrétní ObjectSet&lt;T&gt;. Budeme držet se vrátí IObjectSet&lt;T&gt; rozhraní, protože může pomoci zachovat testovatelnosti v klientském kódu.

Tento konkrétní implementaci je užitečná v produkčním prostředí, ale musíme zaměřit jak použijeme naše IUnitOfWork abstrakce usnadňuje testování.

### <a name="the-test-doubles"></a>Testu zdvojnásobí

K izolování akce kontroleru potřebujeme možnost přepínat mezi skutečné jednotku práce (se opírá o objektu ObjectContext) a test double nebo "falešnou" pracovní jednotka (provádění operací v paměti). Běžným přístupem k provedení tohoto typu přepínání nebudou moci kontroler MVC vytvořit instanci určitou jednotku práce, ale místo toho pass jednotku práce do kontroleru jako parametr konstruktoru.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Výše uvedený kód je příkladem vkládání závislostí. Nepovolit jsme řadič vytvoří jeho závislostí (jednotku práce) ale vložit závislost do kontroleru. V projektu aplikace MVC je běžné použití objekt pro vytváření vlastní kontroler v kombinaci s IOC (Inversion) ovládacího prvku kontejneru pro automatizaci vkládání závislostí. Tato témata jsou nad rámec tohoto článku, ale můžete si přečíst více podle následující odkazy na konci tohoto článku.

Falešné jednotka implementace pracovních, můžeme použít pro testování může vypadat nějak takto.

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

Všimněte si, že falešné jednotku práce zpřístupňuje vlastnost potvrzeny. Někdy je užitečné pro přidání funkcí do falešné třídy, která usnadňují testování. V tomto případě je snadné sledovat, pokud kód potvrdí určitou jednotku práce tak, že zkontrolujete vlastnost potvrzeny.

Budete potřebovat falešné IObjectSet&lt;T&gt; držet objekty pro zaměstnance a časové karty v paměti. Poskytujeme jedna implementace použití obecných typů.

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

Tento test double deleguje většinu práce na základní HashSet&lt;T&gt; objektu. Poznámka: Tento IObjectSet&lt;T&gt; vyžaduje, aby obecná omezení vynucení T jako třídu (typ odkazu) a také vynutí nám implementující rozhraní IQueryable&lt;T&gt;. Je snadné vytvořit kolekci v paměti se zobrazí jako položku IQueryable&lt;T&gt; pomocí standardního operátoru LINQ AsQueryable.

### <a name="the-tests"></a>Testy

Tradiční jednotkové testy budou používat jeden testovací třídy k uložení všech testů pro všechny akce v jeden kontroler MVC. Nám můžete napsat tyto testy nebo jakéhokoli typu testování částí pomocí paměti napodobenin jsme vytvořili. Ale pro účely tohoto článku, který jsme se vyhnete monolitické otestovat přístup pomocí třídy a místo toho skupiny testech a zaměřte se na konkrétní funkce.  Například "Vytvoření nového zaměstnance" může být funkce, které chceme otestovat, takže použijeme jednu testovací třídě ověření jednoho kontroleru akce slouží k vytváření nového zaměstnance.

Je běžné nastavení kódu, kterou potřebujeme pro tyto podrobné testovací třídy. Například vždy potřebujeme vytvořit naše úložiště v paměti a falešné jednotku práce. Potřebujeme taky instance kontroleru zaměstnance s falešnou jednotku práce vloženy. Tento společný kód instalace zveřejníme napříč testovacích tříd pomocí základní třídy.

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

"Místo objektu" používáme v základní třídě je jeden běžný vzor pro vytvoření testovací data. Místo objektu obsahuje metody pro vytváření objektů pro vytvoření instance entity testu pro použití napříč více testů komunikací.

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

Můžeme použít EmployeeControllerTestBase jako základní třída pro řadu zařízení test (viz obrázek 3). Každý testovací přípravek testovat konkrétní kontroler akce. Například jeden testovací přípravek se zaměří na testování akce vytvoření využitých požadavek HTTP GET (Chcete-li zobrazit zobrazení pro vytváření zaměstnanci) a jiné testovacího přípravku se zaměřuje na akce vytvoření použít v požadavku HTTP POST (abyste mohli informace získané při uživatel muset vytvořit zaměstnanci). Jednotlivé odvozené třídy je pouze za instalaci potřebných v jeho konkrétním kontextu a k poskytování kontrolních výrazů, třeba ověřit výsledky jeho kontextu konkrétní testovací.

![EF test_03](~/ef6/media/eftest-03.png)

**Obrázek 3**

Zásady vytváření a testování styl okomentovat není vyžadováno pro testovatelného kód – je jenom jedním z přístupů. Obrázek 4 ukazuje, že testy běžící v Resharper mozky Jet test runner modulu plug-in pro Visual Studio 2010.

![EF test_04](~/ef6/media/eftest-04.png)

**Obrázek 4**

Základní třída pro zpracování kódu sdílené nastavení jednotkové testy pro každou akci kontroleru jsou malé a usnadňují zápis. Testy se spustí rychle (protože jsme se provádění operací v paměti) a by neměla selhat z důvodu nesouvisejících infrastruktury nebo životního prostředí, (protože jednotky v rámci testu jsme jste izolovaný režim).

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

V těchto testech základní třída odvádí většinu práce Instalační program. Mějte na paměti, že konstruktor základní třídy vytvoří úložiště v paměti, falešné jednotku práce a instance třídy EmployeeController. Testovací třídy je odvozen z této základní třídy a se zaměřuje na konkrétních podrobnostech testování metody Create. V tomto případě specifika vaří "uspořádat, fungovat a kontrolní výraz" kroky, které se zobrazí v testování postup:

-   Vytvořte objekt Novýzaměstnanec pro simulaci příchozí data.
-   Vyvolání akce vytvoření EmployeeController a předejte mu Novýzaměstnanec.
-   Ověřte, že akce vytvoření vytváří očekávané výsledky (zaměstnance se zobrazí v úložišti).

Co jsme vytvořili umožňuje některé z akcí EmployeeController testování. Například při psaní testů pro akce indexu řadiče zaměstnance jsme může dědit ze základní třídy testu k navázání stejnou základní verzi pro naše testy. Základní třída znovu vytvoří úložiště v paměti, falešné jednotku práce a instance EmployeeController. Testy pro akce indexu stačí soustředit se na vyvolání akce Index a testování kvality modelu akce vrátí.

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

Testy jsme vytváření pomocí rozhraní fakes v paměti jsou orientované směrem k testování *stavu* softwaru. Například při testování chcete zkontrolovat stav úložiště po spuštění akce vytvoření – akce vytvoření úložiště uchování nového zaměstnance?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Dále podíváme na základě interakce testování. Testování na základě interakce se zeptá, pokud testovaný kód správné metody u našich objektů a předají správné parametry. Teď přejdeme na obálek jiné vzoru návrhu – opožděné načtení.

## <a name="eager-loading-and-lazy-loading"></a>Předběžné načítání a opožděné načtení

V určitém okamžiku v rozhraní ASP.NET MVC přidružené aplikace, kterou jsme možná budete chtít zobrazit informace o zaměstnance a zahrnout zaměstnance čas karty. Například můžeme mít čas kartě Souhrnná zobrazení, která zobrazuje název zaměstnance a celkový počet karet času v systému. Existuje několik strategií, které můžete provést k implementaci této funkce.

### <a name="projection"></a>Projekce

Jedním z přístupů snadno vytvořit souhrn je vytvořit vyhrazený pro informace, které chcete zobrazit v zobrazení modelu. V tomto scénáři může být model vypadat nějak takto.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Všimněte si, že EmployeeSummaryViewModel není entita – jinými slovy není něco, co chcete zachovat v databázi. Pouze budeme Tato třída slouží k nejprve přesunout data do zobrazení silného typu způsobem. Model zobrazení se podobá příchozí přenos dat (DTO) objektu protože neobsahuje žádné chování (žádné metody) – jenom vlastnosti. Vlastnosti bude obsahovat data, která potřebujeme k přesunutí. Je snadné vytvořit instanci tohoto zobrazení modelu pomocí LINQ na standardní projekce operátoru – vyberte operátor.

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

Existují dvě významné funkce do kódu uvedeného výše. Nejprve – kód je snadné testování, protože je stále snadno sledovat a izolaci. Vyberte operátor, který funguje stejně dobře s naší fakes v paměti stejně jako před skutečné jednotku práce.

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

Druhá významné funkce je, jak kód umožňuje EF4 ke generování jednoho efektivní dotazování shromažďovat informace o zaměstnance a čas kartě společně. Informace o zaměstnancích a informace o čas kartě jste načten do stejného objektu bez použití jakékoli speciální rozhraní API. Kód vyjádřena pouze informace, které vyžaduje použití standardní operátory LINQ, které využívají zdroje dat v paměti, jakož i vzdálené zdroje dat. Bylo možné převést stromy výrazů generovaných dotazů LINQ a C EF4\# kompilátoru do jednoho a efektivní dotazu T-SQL.

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

Existují jiné situace, když jsme nechcete pracovat pomocí zobrazení modelu nebo objekt DTO, ale o skutečné entity. Když jsme si vědomi, potřebujeme, aby zaměstnanec *a* zaměstnance čas karty, které se nám můžete například načíst související data v nenápadné a efektivním způsobem.

### <a name="explicit-eager-loading"></a>Explicitní předběžné načítání

Až budeme chtít například načíst informace o související entity, které potřebujeme některé mechanismus pro obchodní logiky (nebo v tomto scénáři logice kontroleru akce) vyjádřit svůj chce úložiště. EF4 ObjectQuery&lt;T&gt; třída definuje metodu zahrnout k určení související objekty, které chcete načíst během dotazu. Mějte na paměti objektu ObjectContext EF4 zpřístupňuje entity přes konkrétní ObjectSet&lt;T&gt; třída, která dědí z ObjectQuery&lt;T&gt;.  Kdybychom používali ObjectSet&lt;T&gt; odkazů v našich akce kontroleru nám napsat následující kód k určení nemůžou dočkat, až zatížení informace o čas kartě pro každý zaměstnanec.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Ale vzhledem k tomu, že se pokoušíme zachovat našeho kódu možností intenzivního testování jsme nevystavujete ObjectSet&lt;T&gt; z mimo skutečné jednotku práce třídy. Místo toho spoléháme IObjectSet&lt;T&gt; rozhraní, která se snadněji falšování, ale IObjectSet&lt;T&gt; nedefinuje metodu zahrnout. Výhodou LINQ je, že můžeme vytvořit vlastní operátor zahrnout.

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

Všimněte si, že tento operátor Include je definován jako rozšiřující metody pro položku IQueryable&lt;T&gt; místo IObjectSet&lt;T&gt;. Tento produkt nám nabízí možnost používat metodu s širší škálu možných typů, včetně IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;a objekt ObjectSet&lt;T&gt;. V případě, že základní sekvence není originální ObjectQuery EF4&lt;T&gt;, pak není nezpůsobily žádné potíže, provádí a zahrnout operátor je no-op. Pokud základní pořadí *je* ObjectQuery&lt;T&gt; (nebo odvozené z ObjectQuery&lt;T&gt;), pak bude EF4 najdete v našich požadavků pro další data a formulovali správné SQL dotaz.

Pomocí tohoto nového operátoru na místě můžete explicitně požadujeme nemůžou dočkat, až zatížení informace o čas kartě z úložiště.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Při spuštění proti skutečné ObjectContext, vytvoří kód následující jeden dotaz. Dotaz shromažďuje dostatek informací z databáze v jedné cesty k sloučit objekty zaměstnance a plně naplnit jejich časové výkazy vlastnost.

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

Dobré zprávy je, že zůstává plně testovatelného kód do metody akce. Nemusíme poskytovat všechny další funkce pro naše napodobeniny pro podporu operátor zahrnout. Chybná je, že jsme museli použít operátor zahrnout uvnitř chceme zajistit trvalost ignorant kód. Toto je typickým příkladem druhu kompromisy, které potřebujete k vyhodnocení při sestavování testovatelného kód. Existují situace, kdy je třeba dát trvalost obavy nevracení mimo abstrakce úložiště pro splnění cílů výkonu.

Alternativa k předběžné načítání je opožděné načtení. Děláme opožděné načtení znamená, že *není* naše obchodní kód explicitně oznamujeme požadavek přidružená data potřebovat. Místo toho používáme naše entity v aplikaci a pokud další data je potřeba Entity Framework se načtou data na vyžádání.

### <a name="lazy-loading"></a>Opožděné načtení

Je snadné Představte si situaci, kdy nevíme, co bude potřebovat dat je konkrétní obchodní logiky. Víme může logika potřebuje objekt zaměstnanců, ale společnost Microsoft může Nepodmíněný skok do různých pracovních cesty, kde některé z těchto cest vyžadují informace o čas kartě od zaměstnance a některé nikoli. Scénáře, jako jsou to jsou ideální pro implicitní opožděné načtení vzhledem k tomu, že data se zobrazí kouzelného na podle potřeby.

Opožděné načtení, označované také jako odložené načítání, umístěte některé požadavky na na našich objekty entity. POCOs s neznalosti true trvalosti by čelí všechny požadavky z vrstvy trvalosti, ale true trvalost neznalosti se v podstatě nedají dosáhnout.  Místo toho měříme neznalosti persistence v relativní stupňů. Bylo by unfortunate, pokud jsme museli dědit ze základní třídu orientované trvalého nebo použít specializované kolekce k dosažení opožděné načtení POCOs. Naštěstí EF4 má teď míň obtěžující řešení.

### <a name="virtually-undetectable"></a>Prakticky jiným způsobem nezjistitelné

Při použití objektů POCO, EF4 může dynamicky generovat runtime proxy pro entity. Tyto servery proxy nezaregistroval zabalení materializovaného POCOs a poskytují získat další služby tím, že zachytává každou vlastnost a nastavte operace k provedení další práce. Jeden konkrétní služby se opožděné načtení funkce, kterou jsme hledali. Jiná služba je mechanismus efektivního sledování změn, které můžete zaznamenat, když se program změní hodnoty vlastností entity. Seznam změn se používá v objektu ObjectContext během metodu SaveChanges zachovat všechny změny entity pomocí příkazů UPDATE.

Pro tyto servery proxy pro práci ale potřebují způsob, jak integrovat do vlastnosti get a množinové operace na entitu a servery proxy dosažení tohoto cíle, tak, že přepíšete virtuální členy. Proto pokud chceme zavést implicitní opožděné načtení a efektivního sledování změn musíme přejít zpět na naše POCO definice tříd a označit jako virtuální.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Nadále budeme moct říct, že entita zaměstnanců je většinou trvalost ignorant. Jediným požadavkem je určený virtuální členy a to nemá vliv na možnosti testování kódu. Nemusíme odvodit z jakékoli speciální základní třídy, nebo můžete použít speciální vyhrazené opožděné načtení kolekce. Jak ukazuje kód, všechny třídy implementace rozhraní ICollection&lt;T&gt; je k dispozici pro uložení související entity.

Je také jeden menší změnu, kterou musíme mít v naší jednotku práce. Opožděné načtení je *vypnout* ve výchozím nastavení při práci přímo s objektem ObjectContext. Existuje vlastnost, kterou jsme můžete nastavit na vlastnost ContextOptions pro povolení odložené načítání a my chceme umožnit opožděné načtení všude, kde nastavte tuto vlastnost uvnitř náš skutečný pracovní jednotka.

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

S implicitní opožděné načtení povoleno, kód aplikace můžete použít zaměstnance a zaměstnance související čas karty přitom blissfully vědět o práce potřebné pro EF načíst doplňující data.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Opožděné načtení usnadňuje zápis kódu aplikace a proxy magic kód zůstane zcela testovatelné. V paměti fakes jednotku práce můžete jednoduše předběžné načtení falešné entity se přidružená data v případě potřeby během testu.

V tuto chvíli jsme vám zapnout naši pozornost od vytváření úložišť pomocí IObjectSet&lt;T&gt; a podívejte se na abstrakce skrýt všechny projevy rozhraní trvalosti.

## <a name="custom-repositories"></a>Vlastní úložiště

Převedou jsme nejprve jednotek práce vzoru návrhu v tomto článku jsme zajistili část ukázkového kódu jak může vypadat jednotku práce. Umožňuje znovu k dispozici tento původního nápadu pomocí zaměstnance a zaměstnance čas kartě scénáře, který jsme pracovali s.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Hlavní rozdíl mezi tuto jednotku práce a jednotku práce jsme vytvořili v předchozí části je, jak tuto jednotku práce nepoužívá žádné abstrakce z rozhraní EF4 (neexistuje žádný IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; funguje také jako rozhraní úložiště, ale rozhraní API zveřejňuje nemusí dokonale odpovídaly potřebám naší aplikace. V této nadcházející přístup jsme bude představovat úložiště pomocí vlastní IRepository&lt;T&gt; abstrakce.

Mnoho vývojářů, kteří sledují založený na testování návrhu, návrh na základě chování a návrhu na základě domény metodologie raději IRepository&lt;T&gt; přístup z několika důvodů. První, IRepository&lt;T&gt; rozhraní představuje vrstvu "odolnou proti poškození". Jak popsal Eric Evans v knize jeho domény Driven Design vrstvu odolnou proti poškození udržuje kódu domény od infrastruktury rozhraní API, například trvalost rozhraní API. Za druhé můžou vývojáři vytvářet metody do úložiště, které splňují konkrétní požadavky aplikace (při zjištění při psaní testů). Například může často Potřebujeme najít jednu entitu pomocí hodnotou ID, takže můžeme přidat metodu FindById rozhraní úložiště.  Naše IRepository&lt;T&gt; definice bude vypadat nějak takto.

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

Všimněte si, že nám budete zase pomocí položku IQueryable&lt;T&gt; rozhraní ke zveřejnění kolekce entit. Položka IQueryable&lt;T&gt; umožňuje stromům výrazů LINQ tok do EF4 zprostředkovatele a poskytnout poskytovateli získat holistický pohled dotazu. Druhou možností je vrátit IEnumerable&lt;T&gt;, což znamená, že zprostředkovatele EF4 LINQ zobrazí pouze výrazy vytvořené v rámci služby úložiště. Jakékoli seskupení, řazení a projekce provedené mimo úložiště nebude obsahovat do příkazu SQL, které se odesílají do databáze, což může snížit výkon. Na druhé straně úložiště vrací pouze IEnumerable&lt;T&gt; výsledky se nikdy vás překvapí pomocí nového příkazu SQL. Oba přístupy bude fungovat a oba přístupy zůstanou testovatelné.

Je jednoduché zajistit jedna implementace položky IRepository&lt;T&gt; rozhraní pomocí obecných typů a rozhraní API EF4 objektu ObjectContext.

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

IRepository&lt;T&gt; přístup poskytuje některé další kontrolu nad naší dotazy protože klient má vyvolat metodu k získání k entitě. Uvnitř metody, kterou může zajišťuje další kontroly a operátory LINQ vynutit omezení použití. Všimněte si, že rozhraní má dvě omezení u obecného typu parametru. Prvním omezením je vyžadované ObjectSet změny třídy nevýhody chuti&lt;T&gt;, a druhé omezení vynutí naše entity k implementaci IEntity – abstrakce vytvořené pro aplikaci. Rozhraní IEntity vynutí entity, které mají čitelnou vlastnost Id, a pak můžete použít tuto vlastnost v metodě FindById. IEntity je definován následujícím kódem.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity může považovat za malý porušení trvalost neznalosti od našich entity jsou nutné k implementaci tohoto rozhraní. Mějte na paměti neznalosti trvalosti je o kompromisy a pro mnoho funkcí FindById převáží omezení stanovené rozhraní. Rozhraní nemá žádný vliv na testovatelnost.

Vytvoření instance živé IRepository&lt;T&gt; vyžaduje objektu ObjectContext EF4 konkrétní jednotka implementace pracovních měli spravovat instance.

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

Pomocí našeho vlastního úložiště není výrazně liší od používat úložiště založené na IObjectSet&lt;T&gt;. Místo použití operátory LINQ přímo na vlastnost, budeme muset nejdřív jednu vyvolávat metody v úložišti a zkopírovat položku IQueryable&lt;T&gt; odkaz.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Všimněte si, že bude fungovat na vlastní zahrnout operátor, který jsme dříve implementovali beze změny. V úložišti FindById metoda odebere duplicitní logiky z akce pokusu o načtení jedné entity.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Neexistuje žádný velký rozdíl možnosti testování dvě metody, které jsme jsme se zaměřili na. Poskytujeme může falešné implementace IRepository&lt;T&gt; konkrétních tříd se opírá o HashSet sestavením&lt;zaměstnance&gt; – stejně jako fungují v poslední části. Někteří vývojáři ale raději pomocí mock objektů a napodobení rozhraní objektu namísto sestavení fakes. Podíváme se na používání mocks testovat naše implementace a diskutovat o rozdílech mezi mocks a fakes v další části.

### <a name="testing-with-mocks"></a>Ve Visual Basicu s Mocks

Existují různé přístupy k vytváření jaké Martina Fowlera volání "test double". Test double (např. film stunt double) je objekt, který vytváříte "vytvoření několika" pro skutečné produkční objekty během testů. Úložiště v paměti, kterou jsme vytvořili jsou testu zdvojnásobí pro úložiště, které komunikují se SQL Server. Zaznamenali jsme na tom, jak používat tyto testu zdvojnásobí během testování částí kódu a rychlé spuštění testů.

Testu zdvojnásobí, kterou jsme vytvořili jste skutečný pracovní implementace. Na pozadí každé z nich ukládá konkrétní kolekci objektů, a budou přidat a odebrat objekty z této kolekce, protože budeme pracovat s úložišti během testu. Někteří vývojáři, jako je vytvářet jejich testu zdvojnásobí tímto způsobem – s skutečného kódu a pracovní implementace.  Tyto testovací zdvojnásobí jsou, čemu říkáme *napodobenin*. Mají implementace práci, ale nejsou dostatečně skutečné pro použití v produkčním prostředí. Falešné úložiště není ve skutečnosti zapisovat do databáze. Falešné server SMTP nebude ve skutečnosti odeslat e-mailovou zprávu v síti.

### <a name="mocks-versus-fakes"></a>Mocks oproti napodobenin

Existuje jiný typ double označované jako test *napodobení*. Napodobeniny mají pracovní implementace, jsou dostupné mocks žádnou implementaci. S pomocí mock objektu rozhraní můžeme vytvořit tyto mock objektů za běhu a použít je jako testu zdvojnásobí. V této části budeme používat open source napodobování framework Moq. Tady je jednoduchý příklad použití Moq dynamicky vytvořit test double pro úložiště zaměstnance.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Pro IRepository požádáme Moq&lt;zaměstnance&gt; implementace a vytvoří jednu dynamicky. Abychom se mohli do objektu implementace IRepository&lt;zaměstnance&gt; přístupem k vlastnosti objektu model&lt;T&gt; objektu. Je tento vnitřní objekt, který předáme do našich kontrolerů a nebude vědět, pokud je test double nebo skutečné úložiště. Stejným způsobem, jako jsme by volat metody pro objekt s skutečné implementaci jsme můžete vyvolávat metody v objektu.

Vás bude zajímat, mock úložiště bude tom, co jsme vyvolat metodu Add. Protože neexistuje žádná implementace za mock objektu, přidat nemá žádný účinek. Neexistuje žádná konkrétní kolekce na pozadí, jako jsme měli s fakes, kterou jsme napsali, tak se zahodí zaměstnance. Co o vrácené hodnotě FindById? V tomto případě mock objektu nepodporuje jediné, co můžete dělat, což je vrácená výchozí hodnotu. Protože jsme se vracející odkazový typ (zaměstnanci), vrácená hodnota je hodnota null.

Mocks může zvuk bezcenné. podstatné; Nicméně existují dvě další funkce mocks, které nebyly už jsme mluvili o. Nejprve Moq framework zaznamenává všechna volání mock objektu. Později v kódu můžete požádáme Moq Pokud kdokoli vyvolat metodu Add nebo pokud kdokoli volal metodu FindById. Podíváme se, jak později můžete použít tuto funkci "černé políčko" záznamu v testech.

Druhá skvělé funkce je, jak můžete použít Moq programovat mock objektu s *očekávání*. Očekávání informuje mock objektu, jak reagovat na daný zásahu. Například jsme naprogramovat očekávání do našich model a určit, k vrácení objektu zaměstnance, když uživatel vyvolá FindById. Rozhraní Moq používá rozhraní API instalační program a výrazy lambda programovat tyto očekávání.

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

V této ukázce požádáme Moq dynamicky vytvářet úložiště a pak budeme programovat úložiště s očekávání. Očekává se říká mock objektu pro vrácení nového objektu zaměstnance s hodnotou Id 5, když někdo volá metodu FindById předáním hodnoty 5. Tento test úspěšný, a jsme neměli muset sestavit úplnou implementaci pro falešné IRepository&lt;T&gt;.

Pojďme návštěvě testy, které jsme napsali dříve a opakovanou prací je, aby používaly mocks místo fakes. Stejně jako dříve, použijeme základní třídy nastavit společné kusy infrastrukturu, kterou potřebujeme pro všechny kontroleru testů.

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

Instalační kód zůstane většinou stejné. Namísto použití fakes, použijeme Moq k sestavení kompletních mock objektů. Základní třída uspořádá mock jednotky práce vrátit mock úložiště, když kód volá vlastnost zaměstnanci. Zbývající část mock nastavení bude probíhat uvnitř testu komunikací vyhrazený pro jednotlivé konkrétní scénáře. Například testovací přípravek, akce indexu bude nastavení mock úložiště vrátila seznam zaměstnanců při akci vyvolá metodu FindAll mock úložiště.

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

S výjimkou očekávání vypadat podobně jako testy, které jsme měli před testech. Ale možnost záznamu mock Framework jsme můžete přistupovat ke testy z různých úhlu. Podíváme se na tato nová Perspektiva v další části.

### <a name="state-versus-interaction-testing"></a>Stav oproti interakce testování

Existují různé techniky, které lze použít k testování softwaru pomocí mock objektů. Jedním z přístupů je používání stavu testování, což je, co jsme udělali v tomto dokumentu zatím. Stav na základě testování vytvoří kontrolní výrazy týkající se stavu softwaru. V rámci poslední sady testů vyvolá metodu akce v kontroleru jsme provedli kontrolní výraz o modelu, že se že má sestavit. Tady jsou některé příklady stavu testování:

-   Ověřte, že úložiště obsahuje nový objekt zaměstnance po vytvoření.
-   Ověřte, že model obsahuje seznam všech zaměstnanců po indexu.
-   Ověřte, že úložiště neobsahuje daný zaměstnance po odstranění.

Další možností, zobrazí se vám pomocí mock objektů je ověřit *interakce*. Při stavu na základě testování je tvrzení o stavu objektů, na základě interakce testování je tvrzení o interakci objekty. Příklad:

-   Ověřte, že kontroler vyvolá metodu Add v úložišti při vytvoření spustí.
-   Ověřte, že kontroler vyvolá metodu FindAll v úložišti, když spustí Index.
-   Ověřte, že kontroler vyvolá jednotka metoda Commit se práce se uložit změny, když provádí úpravy.

Interakce testování často vyžaduje méně testovací data, protože jsme nejsou vyvrtání v rámci kolekce a ověření počty. Například pokud víme, že akce Details vyvolá metodu FindById do úložiště se správnou hodnotu – potom akce pravděpodobně nepracuje správně. Tuto skutečnost můžete ověřit bez nastavování žádná data testu má vrátit z FindById.

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

Jediným nastavením požadovaným ve výše uvedené testovací přípravek se nastavení poskytuje základní třídy. Když jsme vyvolání akce kontroleru, zaznamená Moq interakce s mock úložiště. Pomocí ověřte API Moq, můžete požádáme Moq Pokud kontroler vyvolání FindById správnou hodnotu ID. Pokud kontroler nevyvolal metodu nebo vyvolat metodu s hodnotou neočekávaný parametr, ověřte, zda metoda vyvolá výjimku a test se nezdaří.

Tady je další příklad ověření, že akce vytvoření vyvolá potvrzení v aktuální jednotku práce.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Jeden nebezpečí s testováním interakce je tendence k za určení interakce. Možnost mock objektu, který chcete zaznamenat a ověřte, že všechny interakce s mock objektu neznamená test by měl pokusit ověřte všechny interakce s. Některé interakce se podrobnosti implementace a měli byste ověřit jenom interakce *požadované* splňovat aktuální test.

Volba mezi mocks nebo fakes do značné míry závisí na systému, testování a osobní (nebo skupiny) předvolby. Mock objektů může výrazně omezit množství kódu, je nutné implementovat testu zdvojnásobí, ale ne každý bude vyhovovat programování očekávání a ověření interakce.

## <a name="conclusions"></a>Závěry

V tomto dokumentu jsme jste jsme vám ukázali několik způsobů vytváření testovatelného kód při použití pro trvalost dat ADO.NET Entity Framework. Můžeme využít integrované abstrakce jako IObjectSet&lt;T&gt;, nebo vytvořte vlastní abstrakce jako IRepository&lt;T&gt;.  V obou případech se POCO podpory v ADO.NET Entity Framework 4.0 umožňuje uživatelům tato abstrakce zůstanou trvalé ignorant a s možností intenzivního testování. Další EF4 funkcí, jako jsou implicitní opožděné načtení umožňuje obchodních a aplikačních služeb kód fungovat bez starostí o podrobnosti relační datové úložiště. Nakonec jsou abstrakce, kterou jsme vytvořili snadno mock nebo falešný v rámci testů jednotek a používáme tyto testu zdvojnásobí zajistit rychlé spuštění, vysoce izolované a spolehlivé testy.

### <a name="additional-resources"></a>Další prostředky

-   Robert C. Martin, " [principu jednotnou zodpovědnost](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martina Fowlera [katalog způsobů](http://www.martinfowler.com/eaaCatalog/index.html) z *vzory Enterprise Application Architecture*
-   Griffin Caprio " [injektáž závislostí](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog programovatelnosti data, " [názorný postup: testu řízeného rozvoje s rozhraním Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Blog programovatelnosti data, " [pomocí úložiště a jednotky pracovních vzorů s Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels " [BDD ÚVOD](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Lázecký " [Představujeme počítač specifikace](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee " [BDD s použitím MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans " [návrhu na základě domény](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martina Fowlera " [Mocks nejsou zástupné procedury](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martina Fowlera " [testování Double](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller " [stavu a interakce testování](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>Životopis

Scott Allen je členem technický pracovník na Pluralsightu a Zakladatel OdeToCode.com. Během 15 let vývoje softwaru komerční Scott pracoval na řešení zahrnující všechno od zařízení se systémem embedded 8 bitů na vysoce škálovatelné webové aplikace ASP.NET. Scott můžete oslovit na svém blogu na OdeToCode nebo na Twitteru pod [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).
