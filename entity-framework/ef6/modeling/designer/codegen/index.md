---
title: Šablony generování kódu návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78418676"
---
# <a name="designer-code-generation-templates"></a>Šablony generování kódu návrháře
Při vytváření modelu pomocí návrháře entity framework u a odvozené kontextu jsou automaticky generovány pro vás. Kromě výchozí generování kódu poskytujeme také řadu šablon, které lze použít k přizpůsobení kódu, který se generuje. Tyto šablony jsou k dispozici jako textové šablony T4, což umožňuje přizpůsobit šablony v případě potřeby.

Kód, který se vygeneruje ve výchozím nastavení, závisí na tom, ve které verzi sady Visual Studio vytvoříte model:

-   Modely vytvořené v sadě Visual Studio 2012 & 2013 budou generovat jednoduché třídy entit POCO a kontext, který je odvozen ze zjednodušeného dbcontext.
-   Modely vytvořené v sadě Visual Studio 2010 bude generovat entity třídy, které jsou odvozeny z EntityObject a kontext, který je odvozen z ObjectContext.
  > [!NOTE]    
  > Doporučujeme přepnout na šablonu Generátoru DbContext po přidání modelu.

Tato stránka popisuje dostupné šablony a potom obsahuje pokyny pro přidání šablony do modelu.

## <a name="available-templates"></a>Dostupné šablony

Tým entity Framework poskytuje následující šablony:

### <a name="dbcontext-generator"></a>Generátor kontextu DbContext

Tato šablona bude generovat jednoduché třídy entit POCO a kontext, který je odvozen z DbContext pomocí EF6.
Toto je doporučená šablona, pokud nemáte důvod použít jednu z dalších šablon uvedených níže.
Je to také šablona generování kódu, kterou získáte ve výchozím nastavení, pokud používáte nejnovější verze sady Visual Studio (Visual Studio 2013 a dále): Při vytváření nového modelu se tato šablona používá ve výchozím nastavení a soubory T4 (.tt) jsou vnořeny pod souborem .edmx.

#### <a name="older-versions-of-visual-studio"></a>Starší verze sady Visual Studio
- **Visual Studio 2012:** Chcete-li získat **EF 6.x DbContextGenerator** šablony, budete muset nainstalovat nejnovější **entity framework nástroje pro Visual Studio** - viz získat entity [framework](~/ef6/fundamentals/install.md) stránku další informace.
- **Visual Studio 2010:** Šablony **EF 6.x DbContextGenerator** nejsou k dispozici pro Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Generátor dbcontext pro EF 5.x

Pokud používáte starší verzi entityFramework NuGet balíček (jeden s hlavní verzí 5) budete muset použít **EF 5.x DbContext Generator** šablony.

Pokud používáte Visual Studio 2013 nebo 2012, tato šablona je již nainstalovaná.

Pokud používáte Visual Studio 2010, budete muset vybrat **online** kartu při přidávání šablony ke stažení z Galerie Visual Studio. Alternativně můžete nainstalovat šablonu přímo z Galerie Sady Visual Studio předem. Vzhledem k tomu, že šablony jsou zahrnuty v novějších verzích sady Visual Studio verze v galerii lze nainstalovat pouze na Visual Studio 2010.

- [EF 5.x DbContext generátor pro C #](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [GENERÁTOR EF 5.x DbContext pro weby jazyka C#](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [EF 5.x DbContext generátor pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [Generátor ef 5.x DbContext pro VB.NET webových stránek](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Generátor dbcontext pro EF 4.x

Pokud používáte starší verzi entityFramework NuGet balíček (jeden s hlavní verzí 4) budete muset použít **EF 4.x DbContext Generator** šablony. To najdete na kartě **Online** při přidávání šablony nebo můžete šablonu nainstalovat přímo z galerie Sady Visual Studio.

- [EF 4.x DbContext generátor pro C #](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [EF 4.x DbContext Generátor pro weby Jazyka C#](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [EF 4.x DbContext generátor pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [EF 4.x DbContext Generátor pro VB.NET webových stránek](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>Generátor objektů entity

Tato šablona bude generovat entity třídy, které jsou odvozeny z EntityObject a kontext, který je odvozen z ObjectContext.

> [!NOTE]
> Zvažte použití generátoru DbContext

DbContext Generátor je nyní doporučená šablona pro nové aplikace. Generátor DbContext využívá jednodušší dbcontext rozhraní API. Generátor entityobject je i nadále k dispozici pro podporu existujících aplikací.

**Visual Studio 2010, 2012 &amp; 2013**

Při přidávání šablony budete muset vybrat kartu **Online** a stáhnout ji z Galerie sady Visual Studio. Alternativně můžete nainstalovat šablonu přímo z Galerie Sady Visual Studio předem.

- [EF 6.x EntityObject Generátor pro C #](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Generátor objektů Entity 6.x pro weby jazyka C#](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [GENERÁTOR Objektů Entity 6.x pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [Generátor objektů Entity 6.x pro VB.NET webových stránek](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Generátor entityobject pro EF 5.x**


Pokud používáte Visual Studio 2012 nebo 2013, budete muset vybrat **online** kartu při přidávání šablony ke stažení z Galerie Visual Studio. Alternativně můžete nainstalovat šablonu přímo z Galerie Sady Visual Studio předem. Vzhledem k tomu, že šablony jsou zahrnuty v sadě Visual Studio 2010 &amp; verze v galerii lze nainstalovat pouze na Visual Studio 2012 2013.

- [EF 5.x EntityObject Generátor pro C #](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Generátor objektů Entity 5.x pro weby jazyka C#](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [GENERÁTOR Objektů Entity EF 5.x pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [GENERÁTOR objektů Entity 5.x pro VB.NET webových stránek](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Pokud chcete pouze ObjectContext generování kódu bez nutnosti upravovat šablonu, můžete [se vrátit k EntityObject generování kódu](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Pokud používáte Visual Studio 2010, je tato šablona již nainstalována. Pokud vytvoříte nový model v sadě Visual Studio 2010, tato šablona se používá ve výchozím nastavení, ale soubory TT nejsou součástí projektu. Pokud chcete šablonu přizpůsobit, budete ji muset přidat do projektu.

### <a name="self-tracking-entities-ste-generator"></a>Generátor samosledovacích entit (STE)

Tato šablona bude generovat vlastní sledování entity třídy a kontext, který je odvozen z ObjectContext. V aplikaci EF kontext je zodpovědný za sledování změn v entitách. Však ve scénářích N-vrstvy kontextu nemusí být k dispozici na vrstvě, která upravuje entity. Samoobslužné sledovací entity vám pomohou sledovat změny v libovolné vrstvě. Další informace naleznete [v tématu Entity samočinného sledování](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Šablona STE se nedoporučuje

Již nedoporučujeme používat šablonu STE v nových aplikacích, je stále k dispozici pro podporu stávajících aplikací. Navštivte [odpojené entity článku](~/ef6/fundamentals/disconnected-entities/index.md) pro další možnosti, které doporučujeme pro scénáře N-tier.

> [!NOTE]
> Neexistuje žádná verze EF 6.x šablony STE.


> [!NOTE]
> Neexistuje žádná verze šablony STE pro Visual Studio 2013.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Pokud používáte Visual Studio 2012, budete muset při přidávání šablony vybrat kartu **Online** a stáhnout ji z Galerie Visual Studia. Alternativně můžete nainstalovat šablonu přímo z Galerie Sady Visual Studio předem. Vzhledem k tomu, že šablony jsou zahrnuty v sadě Visual Studio 2010 verze v galerii lze nainstalovat pouze na Visual Studio 2012.

- [EF 5.x STE generátor pro C #](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [GENERÁTOR EF 5.x STE pro weby jazyka C#](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [EF 5.x STE generátor pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [EF 5.x STE generátor pro VB.NET webových stránek](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010**

Pokud používáte Visual Studio 2010, je tato šablona již nainstalována.

### <a name="poco-entity-generator"></a>Generátor entit POCO

Tato šablona bude generovat třídy entit POCO a kontext, který je odvozen z ObjectContext

> [!NOTE]
> Zvažte použití generátoru DbContext

DbContext Generator je nyní doporučená šablona pro generování poco tříd v nových aplikacích. Generátor DbContext využívá nové rozhraní DBContext API a může generovat jednodušší třídy POCO. Generátor entit POCO je i nadále k dispozici pro podporu stávajících aplikací.

> [!NOTE]
> Neexistuje žádná verze EF 5.x nebo EF 6.x šablony STE.

> [!NOTE]
> Neexistuje žádná verze šablony POCO od Visual Studia 2013.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Při přidávání šablony budete muset vybrat kartu **Online** a stáhnout ji z Galerie sady Visual Studio. Alternativně můžete nainstalovat šablonu přímo z Galerie Sady Visual Studio předem.

- [EF 4.x GENERÁTOR POCO pro C #](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Generátor POCO EF 4.x pro weby jazyka C#](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [EF 4.x POCO generátor pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [Generátor POCO EF 4.x pro VB.NET webových stránek](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Jaké jsou šablony "Webové servery"

Šablony "Webové servery" (například **EF 5.x\# DbContext Generator for C Web Sites)** jsou určeny pro použití v projektech vytvořených prostřednictvím **souboru&gt; - Nový -&gt; Webový server...**. Ty se liší od webových aplikací vytvořených pomocí **souboru -&gt; Nový&gt; - Projekt...**, které používají standardní šablony. Poskytujeme samostatné šablony, protože systém šablon položek v sadě Visual Studio je vyžaduje.

## <a name="using-a-template"></a>Použití šablony

Chcete-li začít používat šablonu generování kódu, klepněte pravým tlačítkem myši na prázdné místo na návrhové ploše v návrháři EF a vyberte **přidat položku generování kódu...**.

![Přidat položku Gen kódu](~/ef6/media/add-code-gen-item.png)

Pokud jste již nainstalovali šablonu, kterou chcete použít (nebo byla zahrnuta v sadě Visual Studio), bude k dispozici v části **Kód** nebo **Data** v levé nabídce.

![Nainstalovaný](~/ef6/media/installed.png)

Pokud šablonu ještě nemáte nainstalovanou, vyberte v levé nabídce **online** a vyhledejte požadovanou šablonu.

![Search](~/ef6/media/search.png) 

Pokud používáte Visual Studio 2012, nové soubory TT budou vnořeny pod souborem .edmx.*

> [!NOTE]
> Pro modely vytvořené v sadě Visual Studio 2012 budete muset odstranit šablony používané pro výchozí generování kódu, jinak budete mít duplicitní třídy a kontext generovány. Výchozí soubory jsou ** &lt;&gt;název modelu .tt** a ** &lt;název&gt;modelu .context.tt**. 

![Šablony VS2012](~/ef6/media/vs2012-templates.png)

Pokud používáte Visual Studio 2010, tt soubory jsou přidány přímo do projektu.  

![Šablony VS2010](~/ef6/media/vs2010-templates.png)
