---
title: Šablony pro generování kódu návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418676"
---
# <a name="designer-code-generation-templates"></a>Šablony pro generování kódu návrháře
Při vytváření modelu pomocí Entity Framework Designer vaše třídy a odvozený kontext jsou automaticky generovány za vás. Kromě výchozího generování kódu poskytujeme také řadu šablon, které lze použít k přizpůsobení kódu, který se vygeneroval. Tyto šablony jsou k dispozici jako textové šablony T4, které vám umožní přizpůsobit šablony v případě potřeby.

Kód, který je generován ve výchozím nastavení, závisí na verzi sady Visual Studio, ve které vytvoříte model:

-   Modely vytvořené v aplikaci Visual Studio 2012 & 2013 budou generovat jednoduché třídy entit POCO a kontext, který je odvozený od zjednodušeného DbContext.
-   Modely vytvořené v aplikaci Visual Studio 2010 budou generovat třídy entit, které jsou odvozeny z objektů EntityObject a kontextu, který je odvozen z objektu ObjectContext.
  > [!NOTE]    
  > Po přidání modelu doporučujeme přepnout na šablonu generátoru DbContext.

Tato stránka se zabývá dostupnými šablonami a pak obsahuje pokyny k přidání šablony do modelu.

## <a name="available-templates"></a>Dostupné šablony

Následující šablony poskytuje tým Entity Framework:

### <a name="dbcontext-generator"></a>Generátor DbContext

Tato šablona vytvoří jednoduché třídy entit POCO a kontext, který je odvozen z DbContext pomocí EF6.
Tato šablona se doporučuje, pokud nemáte důvod, abyste použili jednu z dalších šablon uvedených níže.
Také šablona pro generování kódu, která se zobrazí ve výchozím nastavení, pokud používáte poslední verze sady Visual Studio (Visual Studio 2013 a vyšší): při vytváření nového modelu se tato šablona používá ve výchozím nastavení a soubory T4 (. TT) jsou vnořené do souboru. edmx.

#### <a name="older-versions-of-visual-studio"></a>Starší verze sady Visual Studio
- **Visual Studio 2012:** Chcete-li získat šablony **DbContextGenerator EF 6. x** , budete muset nainstalovat nejnovější **Entity Framework Tools pro Visual Studio** – Další informace najdete na stránce [získat Entity Framework](~/ef6/fundamentals/install.md) .
- **Visual Studio 2010:** Šablony **DbContextGenerator EF 6. x** nejsou pro Visual Studio 2010 k dispozici.

#### <a name="dbcontext-generator-for-ef-5x"></a>Generátor DbContext pro EF 5. x

Pokud používáte starší verzi balíčku NuGet EntityFramework (jednu s hlavní verzí 5), budete muset použít šablonu **generátoru DbContext EF 5. x** .

Pokud používáte Visual Studio 2013 nebo 2012 Tato šablona je již nainstalována.

Pokud používáte Visual Studio 2010, bude při přidávání šablony pro stažení z galerie sady Visual Studio nutné vybrat **online** kartu. Alternativně můžete nainstalovat šablonu přímo z galerie sady Visual Studio předem. Vzhledem k tomu, že šablony jsou zahrnuty v novějších verzích sady Visual Studio, verze v galerii lze nainstalovat pouze do sady Visual Studio 2010.

- [DbContext generátor EF 5. x proC#](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [DbContext generátor EF 5. x pro C# weby](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [Generátor EF 5. x DbContext pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [DbContext generátor EF 5. x pro weby VB.NET](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Generátor DbContext pro EF 4. x

Pokud používáte starší verzi balíčku NuGet EntityFramework (jednu s hlavní verzí 4), budete muset použít šablonu **generátoru DbContext EF 4. x** . Tato možnost je k dispozici na kartě **online** při přidávání šablony, nebo můžete šablonu nainstalovat přímo z galerie sady Visual Studio předem.

- [Generátor DbContext EF 4. x proC#](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [Generátor DbContext EF 4. x pro C# weby](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [Generátor DbContext EF 4. x pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [EF 4. x DbContext Generator pro weby VB.NET](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>Generátor objektů EntityObject

Tato šablona vygeneruje třídy entit, které jsou odvozeny z objektů EntityObject a kontext, který je odvozen z objektu ObjectContext.

> [!NOTE]
> Zvažte použití generátoru DbContext.

Pro nové aplikace je teď doporučená šablona DbContext Generator. Generátor DbContext využívá jednodušší rozhraní DbContext API. Generátor objektů EntityObject je nadále k dispozici pro podporu stávajících aplikací.

**Visual Studio 2010, 2012 &amp; 2013**

Při přidávání šablony pro stažení z galerie sady Visual Studio bude nutné vybrat **online** kartu. Alternativně můžete nainstalovat šablonu přímo z galerie sady Visual Studio předem.

- [EF 6. x objektů EntityObject generátor proC#](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Objektů EntityObject generátor EF 6. x pro C# weby](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [EF 6. x objektů EntityObject Generator pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [EF 6. x objektů EntityObject Generator pro weby VB.NET](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Generátor objektů EntityObject pro EF 5. x**


Pokud používáte Visual Studio 2012 nebo 2013, bude při přidávání šablony pro stažení z galerie sady Visual Studio nutné vybrat **online** kartu. Alternativně můžete nainstalovat šablonu přímo z galerie sady Visual Studio předem. Vzhledem k tomu, že šablony jsou součástí sady Visual Studio 2010, verze v galerii lze nainstalovat pouze do sady Visual Studio 2012 &amp; 2013.

- [Objektů EntityObject generátor EF 5. x proC#](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Objektů EntityObject generátor EF 5. x pro C# weby](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [Generátor EF 5. x objektů EntityObject pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [Objektů EntityObject generátor EF 5. x pro weby VB.NET](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Pokud chcete pouze generování kódu ObjectContext, aniž byste museli upravovat šablonu, můžete [se vrátit k objektů EntityObject generování kódu](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Pokud používáte aplikaci Visual Studio 2010, tato šablona je již nainstalována. Pokud vytvoříte nový model v aplikaci Visual Studio 2010 Tato šablona se používá ve výchozím nastavení, ale soubory. TT nejsou zahrnuty do projektu. Pokud chcete šablonu přizpůsobit, budete ji muset přidat do projektu.

### <a name="self-tracking-entities-ste-generator"></a>Generátor entit (STE) pro samoobslužné sledování

Tato šablona vygeneruje samostatné třídy entit a kontext, který je odvozen z objektu ObjectContext. V aplikaci EF je kontext zodpovědný za sledování změn v entitách. Ve scénářích N-vrstvých ale nemusí být kontext k dispozici na úrovni, která tyto entity mění. Entity s možností sledování vám pomůžou sledovat změny v libovolné vrstvě. Další informace najdete v tématu [entity pro sledování sebe](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Šablona STE se nedoporučuje.

V nových aplikacích už nedoporučujeme používat šablonu STE, ale bude dostupná i nadále, aby podporovala stávající aplikace. Další možnosti, které doporučujeme u N-vrstvých scénářů, najdete v článku věnovaném [odpojeným entitám](~/ef6/fundamentals/disconnected-entities/index.md) .

> [!NOTE]
> V šabloně STE není k dispozici žádná verze EF 6. x.


> [!NOTE]
> Neexistuje žádná Visual Studio 2013 verze šablony STE.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Pokud používáte Visual Studio 2012, bude při přidávání šablony pro stažení z galerie sady Visual Studio nutné vybrat **online** kartu. Alternativně můžete nainstalovat šablonu přímo z galerie sady Visual Studio předem. Vzhledem k tomu, že šablony jsou součástí sady Visual Studio 2010, verze v galerii lze nainstalovat pouze do sady Visual Studio 2012.

- [STE generátor EF 5. x proC#](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [STE generátor EF 5. x pro C# weby](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [Generátor EF 5. x STE pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [STE generátor EF 5. x pro weby VB.NET](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010 * *

Pokud používáte aplikaci Visual Studio 2010, tato šablona je již nainstalována.

### <a name="poco-entity-generator"></a>Generátor entit POCO

Tato šablona vytvoří třídy entit POCO a kontext, který je odvozen z objektu ObjectContext.

> [!NOTE]
> Zvažte použití generátoru DbContext.

Generátor DbContext je teď doporučovanou šablonou pro generování tříd POCO v nových aplikacích. Generátor DbContext využívá nové rozhraní DbContext API a může generovat jednodušší třídy POCO. Generátor entit POCO je nadále k dispozici pro podporu stávajících aplikací.

> [!NOTE]
> V šabloně STE není k dispozici žádná verze EF 5. x nebo EF 6. x.

> [!NOTE]
> Neexistuje žádná Visual Studio 2013 verze šablony POCO.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Při přidávání šablony pro stažení z galerie sady Visual Studio bude nutné vybrat **online** kartu. Alternativně můžete nainstalovat šablonu přímo z galerie sady Visual Studio předem.

- [Generátor POCO EF 4. x proC#](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Generátor POCO EF 4. x pro C# weby](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [Generátor POCO EF 4. x pro VB.NET](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [EF 4. x POCO Generator pro weby VB.NET](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>Co jsou šablony webů

Šablony "weby" (například **EF 5. x DbContext Generator pro jazyky C\#** ) slouží k použití v projektech webů vytvořených prostřednictvím webu **založeného na novém&gt;&gt; souborů...** . Tyto aplikace se liší od webových aplikací, které jsou vytvořené pomocí **&gt; projektu New-&gt;...** , které používají standardní šablony. Poskytujeme samostatné šablony, protože systém šablony položky v aplikaci Visual Studio je vyžaduje.

## <a name="using-a-template"></a>Použití šablony

Chcete-li začít používat šablonu generování kódu, klikněte pravým tlačítkem myši na prázdné místo na návrhové ploše v Návrháři EF a vyberte možnost **Přidat položku generování kódu...**

![Přidat položku kód pro obecné](~/ef6/media/add-code-gen-item.png)

Pokud jste již nainstalovali šablonu, kterou chcete použít (nebo byla součástí sady Visual Studio), bude k dispozici v části **kód** nebo **data** v nabídce vlevo.

![Instalováno](~/ef6/media/installed.png)

Pokud šablonu ještě nemáte nainstalovanou, v levé nabídce vyberte **online** a vyhledejte požadovanou šablonu.

![Hledání](~/ef6/media/search.png) 

Pokud používáte Visual Studio 2012, nové soubory. TT budou vnořené do souboru. edmx. *

> [!NOTE]
> Pro modely vytvořené v aplikaci Visual Studio 2012 budete muset odstranit šablony používané pro výchozí generování kódu, jinak budete mít vygenerované duplicitní třídy a kontextu. Výchozí soubory jsou **&lt;název modelu&gt;. TT** a **&lt;název modelu&gt;. Context.TT**. 

![Šablony VS2012](~/ef6/media/vs2012-templates.png)

Pokud používáte Visual Studio 2010, soubory TT se přidávají přímo do vašeho projektu.  

![Šablony VS2010](~/ef6/media/vs2010-templates.png)
