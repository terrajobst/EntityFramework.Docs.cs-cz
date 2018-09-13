---
title: Nezpracované dotazy SQL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 6b00648939ccedffeed09b4e1d6e8d70fa262a36
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490581"
---
# <a name="raw-sql-queries"></a>Nezpracované dotazy SQL
Entity Framework umožňuje dotazování pomocí jazyka LINQ pomocí tříd entit. Mohou však nastat situace, které chcete spouštět dotazy s využitím nezpracovaná SQL přímo na databázi. To zahrnuje volání uložené procedury, které mohou být užitečné pro Code First modely, které se aktuálně nepodporují mapování na uložené procedury. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

## <a name="writing-sql-queries-for-entities"></a>Psaní dotazů SQL pro entity  

Metoda SqlQuery na DbSet umožňuje neupraveného dotazu SQL má být proveden zápis, který vrátí instance entity. Vrácených objektů bude sledovat podle kontextu, stejně jako by se použily, pokud byly vrácené dotazu LINQ. Příklad:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Všimněte si, že stejně jako u dotazů LINQ dotaz není udělat, dokud jsou uvedené výsledky – v předchozím příkladu to provádí pomocí volání ToList.  

Pokaždé, když se nezpracované dotazy SQL jsou určeny pro dva důvody měli věnovat pozornost. Nejprve by měly být napsány dotaz k zajištění, že vrátí jenom entity, které jsou v zásadě požadovaného typu. Například při použití funkce, jako je dědičnost je snadné napsat dotaz, který bude vytvářet entity, které jsou chybného typu CLR.  

Za druhé některé typy neupraveného dotazu SQL vystavit potenciální ohrožení zabezpečení, zejména útoky prostřednictvím injektáže SQL. Ujistěte se, že správným způsobem pro ochranu před těmito útoky použít parametry v dotazu.  

### <a name="loading-entities-from-stored-procedures"></a>Načítání entit z uložených procedur  

DbSet.SqlQuery můžete použít k načtení entity z výsledků uloženou proceduru. Například následující kód volá vlastníka. GetBlogs procedury v databázi:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Můžete také předat parametry uložené procedury pomocí následující syntaxe:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Psaní dotazů SQL pro typy bez entit  

Jazyka SQL vracející instance libovolného typu, včetně primitivní typy, je možné vytvořit pomocí metody SqlQuery ve třídě databáze. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Výsledky vrácené SqlQuery v databázi se nikdy sledovat pomocí kontextu i v případě, objekty jsou instancemi typu entity.  

## <a name="sending-raw-commands-to-the-database"></a>Odesílání nezpracovaná příkazů do databáze  

Příkazy bez dotazu je odeslat do databáze pomocí metody ExecuteSqlCommand v databázi. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Všimněte si, že jsou všechny změny dat v databázi pomocí ExecuteSqlCommand neprůhledné kontextu dokud entity jsou načteny nebo znovu načíst z databáze.  

### <a name="output-parameters"></a>Výstupní parametry  

Pokud se používají výstupních parametrů, jejich hodnoty nebudou dostupné, dokud nebude mít zcela načíst výsledky. Je to z důvodu základní chování DbDataReader naleznete v tématu [načítání dat pomocí čtečky dat](http://go.microsoft.com/fwlink/?LinkID=398589) další podrobnosti.  
