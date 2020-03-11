---
title: Nezpracované dotazy SQL – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417093"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL
Entity Framework umožňuje dotazování pomocí LINQ s vašimi třídami entit. Nicméně mohou nastat situace, kdy budete chtít spouštět dotazy pomocí nezpracovaného SQL přímo proti databázi. To zahrnuje volání uložených procedur, které mohou být užitečné pro Code First modely, které aktuálně nepodporují mapování na uložené procedury. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

## <a name="writing-sql-queries-for-entities"></a>Zápis dotazů SQL pro entity  

Metoda SqlQuery v Negenerickými umožňuje napsat nezpracovaný dotaz SQL, který bude vracet instance entit. Vrácené objekty budou sledovány kontextem stejně, jako by byly vráceny dotazem LINQ. Příklad:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Všimněte si, že stejně jako u dotazů LINQ není dotaz proveden, dokud nejsou výčty výsledků – v předchozím příkladu je provedeno volání ToList –.  

Při každém zápisu nezpracovaných dotazů SQL ze dvou důvodů by měla být provedena péče. Nejprve by měl být vytvořen dotaz, aby bylo zajištěno, že bude vracet pouze entity, které jsou skutečně požadovaného typu. Například při použití funkcí, jako je dědičnost, je snadné napsat dotaz, který vytvoří entity, které mají nesprávný typ CLR.  

Za druhé, některé typy nezpracovaných dotazů SQL zveřejňují potenciální bezpečnostní rizika, zejména při útokech prostřednictvím injektáže SQL. Ujistěte se, že používáte parametry v dotazu správným způsobem ochrany proti takovým útokům.  

### <a name="loading-entities-from-stored-procedures"></a>Načítání entit z uložených procedur  

Pomocí Negenerickými. SqlQuery můžete načíst entity z výsledků uložené procedury. Například následující kód volá dbo. Procedura getbloges v databázi:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Můžete také předat parametry uložené proceduře pomocí následující syntaxe:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Zápis dotazů SQL pro typy bez entit  

Dotaz SQL vracející instance libovolného typu, včetně primitivních typů, lze vytvořit pomocí metody SqlQuery třídy Database. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Výsledky vrácené z SqlQuery v databázi nikdy nebudou sledovány kontextem, i když jsou objekty instance typu entity.  

## <a name="sending-raw-commands-to-the-database"></a>Posílání nezpracovaných příkazů do databáze  

Příkazy jiného typu než dotaz lze odeslat do databáze pomocí metody ExecuteSqlCommand v databázi. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Všimněte si, že všechny změny provedené v datech v databázi pomocí ExecuteSqlCommand jsou neprůhledné do kontextu, dokud se entity nenačte nebo znovu načtou z databáze.  

### <a name="output-parameters"></a>Výstupní parametry  

Pokud se používají výstupní parametry, jejich hodnoty nebudou k dispozici, dokud nebudou výsledky zcela načteny. Důvodem je základní chování DbDataReader. Další informace najdete v tématu [načtení dat pomocí objektu DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .  
