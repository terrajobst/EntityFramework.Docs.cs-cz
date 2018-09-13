---
title: Šablony generování kódu návrháře - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: 8479d4e76e6db43072c382792c69250ae032af62
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490174"
---
# <a name="designer-code-generation-templates"></a>Šablony generování kódu návrháře
Když vytvoříte model využívající Entity Framework Designer třídy a odvozené kontextu jsou automaticky generovány za vás. Kromě generování kódu výchozí poskytujeme také několik šablon, které můžete použít k přizpůsobení kód, který získá vygenerována. Tyto šablony jsou k dispozici jako textové šablony T4, což vám umožní přizpůsobit šablony v případě potřeby.

Kód, který získá vygenerována ve výchozím nastavení závisí na verzi sady Visual Studio vytvoříte v modelu:

-   Modelů vytvořených v sadě Visual Studio 2012 a 2013 vygeneruje jednoduchou POCO tříd entit a kontext, který je odvozen od zjednodušené uvolněn objekt DbContext.
-   Modelů vytvořených v sadě Visual Studio 2010 bude generovat tříd entit, které jsou odvozeny z EntityObject a kontext, který je odvozen od objektu ObjectContext.
  > [!NOTE]    
  > Doporučujeme přejít na šabloně generátor DbContext, po přidání vašeho modelu.

Tato stránka popisuje dostupné šablony a pokyny pro přidání šablony do modelu.

## <a name="available-templates"></a>Dostupné šablony

Následující šablony jsou poskytované týmem Entity Framework:

### <a name="dbcontext-generator"></a>Generátor DbContext

Tato šablona vygeneruje jednoduchou POCO tříd entit a kontext, který je odvozen od položky DbContext pomocí EF6.
Toto je doporučené šablony, pokud nemáte důvod použít jednu z jiných šablon uvedené níže.
Je také šablony generování kódu získáte ve výchozím nastavení, pokud používáte nejnovější verze sady Visual Studio (Visual Studio 2013 a vyšší): při vytváření nového modelu Tato šablona se používá ve výchozím nastavení a soubory T4 (.tt) jsou vnořené v souboru .edmx.

#### <a name="older-versions-of-visual-studio"></a>Starší verze sady Visual Studio
- **Visual Studio 2012:** zobrazíte **EF 6.x DbContextGenerator** budete muset nainstalovat nejnovější verzi šablony **Entity Framework Tools for Visual Studio** -najdete v článku [získat Entity Rozhraní Framework](~/ef6/fundamentals/install.md) stránky pro další informace.
- **Visual Studio 2010:** **EF 6.x DbContextGenerator** šablony nejsou k dispozici pro sadu Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Generátor DbContext pro EF 5.x

Pokud používáte starší verzi balíčku NuGet objektu EntityFramework, (jedna hlavní verze 5) je potřeba použít **EF 5.x DbContext generátor** šablony.

Pokud používáte Visual Studio 2013 nebo 2012 Tato šablona je již nainstalována.

Pokud používáte Visual Studio 2010 bude nutné vybrat **Online** kartě při přidávání šablonu, aby si ho stáhnout z Galerie sady Visual Studio. Případně můžete nainstalovat šablony přímo z Galerie sady Visual Studio předem. Vzhledem k tomu, že šablony jsou zahrnuty v pozdějších verzích sady Visual Studio verze v galerii lze nainstalovat pouze na Visual Studio 2010.

- [EF 5.x DbContext generátor pro jazyk C#](http://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [EF 5.x DbContext generátor jazyka C# webů](http://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [EF 5.x DbContext generátor pro VB.NET](http://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [EF 5.x DbContext generátor pro weby VB.NET](http://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Generátor DbContext pro EF 4.x

Pokud používáte starší verzi balíčku NuGet objektu EntityFramework, (jedna hlavní verze 4) je potřeba použít **EF 4.x DbContext generátor** šablony. To lze nalézt v **Online** kartě při přidávání šablony nebo šablony můžete nainstalovat přímo z Galerie sady Visual Studio předem.

- [EF 4.x DbContext generátor pro jazyk C#](http://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [EF 4.x DbContext generátor jazyka C# webů](http://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [EF 4.x DbContext generátor pro VB.NET](http://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [EF 4.x DbContext generátor pro weby VB.NET](http://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>Generátor EntityObject

Tato šablona vygeneruje tříd entit, které jsou odvozeny z EntityObject a kontext, který je odvozen od objektu ObjectContext.

> [!NOTE]
> Zvažte použití generátoru DbContext

Doporučené šablony pro nové aplikace je nyní generátor DbContext. Generátor DbContext využívá rozhraní API jednodušší DbContext. Generátor EntityObject i nadále k dispozici pro podporu stávající aplikace.

**Visual Studio 2010, 2012 &amp; 2013**

Bude nutné vybrat **Online** kartě při přidávání šablonu, aby si ho stáhnout z Galerie sady Visual Studio. Případně můžete nainstalovat šablony přímo z Galerie sady Visual Studio předem.

- [EF 6.x EntityObject generátor pro jazyk C#](http://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [EF 6.x EntityObject generátor jazyka C# webů](http://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [EF 6.x EntityObject generátor pro VB.NET](http://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [EF 6.x EntityObject generátor pro weby VB.NET](http://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Generátor EntityObject pro EF 5.x**


Pokud používáte Visual Studio 2012 nebo 2013 budete muset vybrat **Online** kartě při přidávání šablonu, aby si ho stáhnout z Galerie sady Visual Studio. Případně můžete nainstalovat šablony přímo z Galerie sady Visual Studio předem. Vzhledem k tomu, šablony jsou zahrnuty v sadě Visual Studio 2010 verze v galerii, lze nainstalovat pouze na Visual Studio 2012 &amp; 2013.

- [EF 5.x EntityObject generátor pro jazyk C#](http://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [EF 5.x EntityObject generátor jazyka C# webů](http://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [EF 5.x EntityObject generátor pro VB.NET](http://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [EF 5.x EntityObject generátor pro weby VB.NET](http://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Pokud chcete jenom ObjectContext generování kódu bez nutnosti k úpravě šablony můžete [vrátit k generování kódu EntityObject](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Pokud používáte Visual Studio 2010, tato šablona je již nainstalována. Pokud vytvoříte nový model v sadě Visual Studio 2010 Tato šablona se používá výchozí ale .tt, které soubory nejsou zahrnuty v projektu. Pokud budete chtít přizpůsobit šablonu, budete muset přidat do projektu.

### <a name="self-tracking-entities-ste-generator"></a>Vlastní sledování generátor entity (Vložit)

Tato šablona vygeneruje třídy Self-Tracking Entity a kontext, který je odvozen od objektu ObjectContext. V aplikaci EF kontext zodpovídá za sledování změn v entitách. V N-vrstvá scénáře, nemusí být k dispozici na na úrovni, která upravuje entity kontextu. Vlastní sledování entit můžete sledovat změny ve všech úrovních. Další informace najdete v tématu [Self-Tracking entity](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Vložit šablonu, ale nedoporučený krok

Doporučujeme už pomocí šablony vložit do nové aplikace, se nadále být k dispozici podpora stávajících aplikací. Přejděte [odpojené entity článku](~/ef6/fundamentals/disconnected-entities/index.md) další možnosti pro N-vrstvá scénářů doporučujeme.

> [!NOTE]
> Neexistuje žádná verze 6.x EF STE šablony.


> [!NOTE]
> Neexistuje žádná verze sady Visual Studio 2013 STE šablony.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Pokud používáte sadu Visual Studio 2012 je potřeba vybrat **Online** kartě při přidávání šablonu, aby si ho stáhnout z Galerie sady Visual Studio. Případně můžete nainstalovat šablony přímo z Galerie sady Visual Studio předem. Vzhledem k tomu, šablony jsou zahrnuty v sadě Visual Studio 2010 verze v galerii můžete nainstalovat jenom na Visual Studio 2012.

- [EF 5.x STE generátor pro jazyk C#](http://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [EF 5.x STE generátor jazyka C# webů](http://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [EF 5.x STE generátor pro VB.NET](http://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [EF 5.x STE generátor pro weby VB.NET](http://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010 **

Pokud používáte Visual Studio 2010, tato šablona je již nainstalována.

### <a name="poco-entity-generator"></a>Objektů POCO generátor Entity

Tato šablona vygeneruje POCO tříd entit a kontext, který je odvozen od objektu ObjectContext

> [!NOTE]
> Zvažte použití generátoru DbContext

Doporučené šablony pro generování tříd POCO v nové aplikace je nyní generátor DbContext. Generátor DbContext využívá výhod nového rozhraní API kontext databáze a může generovat tříd zjednodušení POCO. Generátor Entity objektů POCO i nadále k dispozici pro podporu stávající aplikace.

> [!NOTE]
> Neexistuje žádné EF 5.x nebo verze 6.x EF STE šablony.

> [!NOTE]
> Neexistuje žádná verze sady Visual Studio 2013 šablony POCO.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Bude nutné vybrat **Online** kartě při přidávání šablonu, aby si ho stáhnout z Galerie sady Visual Studio. Případně můžete nainstalovat šablony přímo z Galerie sady Visual Studio předem.

- [EF 4.x POCO generátor pro jazyk C#](http://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [EF 4.x POCO generátor jazyka C# webů](http://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [EF 4.x POCO generátor pro VB.NET](http://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [EF 4.x POCO generátor pro weby VB.NET](http://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Co jsou šablony "Webové servery"

Šablony "Webové servery" (například **EF 5.x DbContext generátor pro jazyk C\# weby**) jsou určeny pro použití ve webových projektech vytvořených prostřednictvím **soubor –&gt; New -&gt; webu...** . Ty se liší od webových aplikací vytvořených prostřednictvím **soubor –&gt; New -&gt; projektu...** , která může použít standardní šablony. Protože systém šablony položky v sadě Visual Studio, budeme poskytovat samostatné šablony.

## <a name="using-a-template"></a>Pomocí šablony

Pokud chcete začít používat šablony generování kódu, klikněte pravým tlačítkem na prázdné místo na návrhové ploše v EF designeru a vyberte **přidat položku generování kódu...** .

![Přidat položku Obecné kódu](~/ef6/media/add-code-gen-item.png)

Pokud jste již nainstalovali šablonu chcete použít (nebo je zahrnutý v sadě Visual Studio), pak bude k dispozici v části **kód** nebo **Data** části v levé nabídce.

![Nainstalovat](~/ef6/media/installed.png)

Pokud ještě nemáte nainstalované šablony, vyberte **Online** z nabídky vlevo a vyhledejte požadované šablony.

![Hledat](~/ef6/media/search.png) 

Pokud používáte sadu Visual Studio 2012, se nové soubory .tt vnoří pod file.* edmx

> [!NOTE]
> U modelů vytvořených v sadě Visual Studio 2012 je potřeba odstranit šablon použitých pro generování kódu výchozí v opačném případě budete mít duplicitní třídy a kontextu vygenerována. Výchozí soubory jsou  **&lt;název modelu&gt;.tt** a  **&lt;název modelu&gt;. context.tt**. 

![VS2012 šablony](~/ef6/media/vs2012-templates.png)

Pokud používáte Visual Studio 2010, se přidají soubory tt přímo do vašeho projektu.  

![VS2010 šablony](~/ef6/media/vs2010-templates.png)
