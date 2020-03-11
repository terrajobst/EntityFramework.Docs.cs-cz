---
title: Relace – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418243"
---
# <a name="relationships---ef-designer"></a>Vztahy – Návrhář EF
> [!NOTE]
> Tato stránka poskytuje informace o nastavení vztahů v modelu pomocí návrháře EF. Obecné informace o relacích v EF a o tom, jak přistupovat k datům a pracovat s nimi pomocí vztahů, najdete v tématu [relace & vlastnosti navigace](~/ef6/fundamentals/relationships.md).

Přidružení definují vztahy mezi typy entit v modelu. Toto téma ukazuje, jak mapovat asociace pomocí Entity Framework Designer (EF Designer). Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.

![Návrhář EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> Při sestavování koncepčního modelu se může zobrazit upozornění týkající se nemapovaných entit a přidružení v Seznam chyb. Tato upozornění můžete ignorovat, protože po výběru databáze z modelu budou tyto chyby pryč.

## <a name="associations-overview"></a>Přehled přidružení

Při návrhu modelu pomocí návrháře EF představuje soubor. edmx váš model. V souboru EDMX definuje element **Association** vztah mezi dvěma typy entit. Přidružení musí určovat typy entit, které jsou součástí relace, a možný počet typů entit na každém konci relace, který se označuje jako násobnost. Násobnost elementu end přidružení může mít hodnotu jedna (1), 0 nebo 1 (0.. 1) nebo mnoho (\*). Tyto informace jsou zadány ve dvou podřízených elementech **End** .

V době běhu lze k instancím typu entity na jednom konci přidružit prostřednictvím vlastností navigace nebo cizích klíčů (Pokud se rozhodnete vystavit cizí klíče ve svých entitách). Při zpřístupnění cizích klíčů je vztah mezi entitami spravován pomocí elementu **elementu ReferentialConstraint** (podřízený element elementu **Association** ). Doporučuje se, abyste vždycky vystavili cizí klíče pro vztahy ve svých entitách.

> [!NOTE]
> V m:n (\*:\*) nemůžete přidávat cizí klíče k entitám. Ve vztahu \*:\* jsou informace o přidružení spravovány pomocí nezávislého objektu.

Informace o prvcích CSDL (**elementu ReferentialConstraint**, **Association**atd.) najdete ve [specifikaci CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Vytváření a odstraňování přidružení

Vytvořením přidružení s návrhářem EF se aktualizuje obsah modelu souboru. edmx. Po vytvoření přidružení je nutné vytvořit mapování pro přidružení (popsáno dále v tomto tématu).

> [!NOTE]
> V této části se předpokládá, že už jste přidali entity, pro které chcete vytvořit přidružení mezi modelem.

### <a name="to-create-an-association"></a>Vytvoření přidružení

1.  Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, přejděte na **Přidat nový**a vyberte **asociace...** .
2.  Do dialogového okna **Přidat přidružení** zadejte nastavení pro přidružení.

    ![Přidat přidružení](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Můžete se rozhodnout, že do entit na koncích přidružení nepřidáte vlastnosti navigace nebo cizí klíč, a to tak, že zrušíte **navigační vlastnost **a **přidáte vlastnosti cizího klíče do &lt;název typu entity&gt; **zaškrtávací políčka entit. Pokud přidáte jenom jednu navigační vlastnost, bude možné toto přidružení procházet jenom v jednom směru. Pokud nepřidáte žádné navigační vlastnosti, musíte se rozhodnout přidat vlastnosti cizího klíče, aby bylo možné přistupovat k entitám na koncích přidružení.
    
3.  Klikněte na tlačítko **OK**.

### <a name="to-delete-an-association"></a>Odstranění přidružení

Chcete-li odstranit přidružení, proveďte jednu z následujících akcí:

-   Klikněte pravým tlačítkem na přidružení na povrchu návrháře EF a vyberte **Odstranit**.

- Ani

-   Vyberte jedno nebo více přidružení a stiskněte klávesu DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Zahrnutí vlastností cizího klíče do entit (referenční omezení)

Doporučuje se, abyste vždycky vystavili cizí klíče pro vztahy ve svých entitách. Entity Framework používá referenční omezení k identifikaci, že vlastnost slouží jako cizí klíč pro relaci.

Pokud jste zaškrtli políčko ***Přidat vlastnosti cizího klíče do pole &lt;název typu entity&gt; entitu*** při vytváření relace, toto referenční omezení bylo přidáno za vás.

Když použijete návrháře EF k přidání nebo úpravě referenčního omezení, Návrhář EF přidá nebo upraví element **elementu referentialconstraint** v obsahu CSDL souboru. edmx.

-   Dvakrát klikněte na přidružení, které chcete upravit.
    Zobrazí se dialogové okno **referenční omezení** .
-   V rozevíracím seznamu  **zabezpečení** vyberte entitu zabezpečení v referenčním omezení.
    Klíčové vlastnosti entity se přidají do seznamu **hlavních klíčů** v dialogovém okně.
-   V rozevíracím seznamu **závislé** vyberte entitu Dependent v referenčním omezení.
-   V případě každého hlavního klíče, který má závislý klíč, vyberte odpovídající klíč závislý v rozevíracích seznamech ve sloupci **závislého klíče** .

    ![Omezení ref](~/ef6/media/refconstraint.png)

-   Klikněte na tlačítko **OK**.

## <a name="create-and-edit-association-mappings"></a>Vytvoření a úprava mapování přidružení

Můžete určit způsob, jakým se přidružení mapuje k databázi v okně **Podrobnosti mapování** v Návrháři EF.

> [!NOTE]
> Můžete mapovat pouze podrobnosti o přidruženích, u kterých není zadáno referenční omezení. Pokud je zadáno referenční omezení, je v entitě obsažena vlastnost cizího klíče a můžete použít Podrobnosti mapování pro entitu k určení, ke kterému sloupci se cizí klíč mapuje.

### <a name="create-an-association-mapping"></a>Vytvoření mapování přidružení

-   Klikněte pravým tlačítkem na přidružení na návrhové ploše a vyberte **mapování tabulky**.
    V okně **Podrobnosti mapování** se zobrazí mapování přidružení.
-   Klikněte na **Přidat tabulku nebo zobrazení**.
    Zobrazí se rozevírací seznam, který obsahuje všechny tabulky v modelu úložiště.
-   Vyberte tabulku, do které bude mapování asociace namapováno.
    Okno **Podrobnosti mapování** zobrazuje obě konce přidružení a klíčové vlastnosti pro typ entity na každém **konci**.
-   Pro každou klíčovou vlastnost klikněte na pole **sloupec** a vyberte sloupec, ke kterému bude namapována vlastnost.

    ![Podrobnosti mapování 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Úprava mapování přidružení

-   Klikněte pravým tlačítkem na přidružení na návrhové ploše a vyberte **mapování tabulky**.
    V okně **Podrobnosti mapování** se zobrazí mapování přidružení.
-   Klikněte na tlačítko **mapy &lt;název tabulky&gt;** .
    Zobrazí se rozevírací seznam, který obsahuje všechny tabulky v modelu úložiště.
-   Vyberte tabulku, do které bude mapování asociace namapováno.
    Okno **Podrobnosti mapování** zobrazuje obě konce přidružení a klíčové vlastnosti pro typ entity na každém konci.
-   Pro každou klíčovou vlastnost klikněte na pole **sloupec** a vyberte sloupec, ke kterému bude namapována vlastnost.

## <a name="edit-and-delete-navigation-properties"></a>Upravit a odstranit navigační vlastnosti

Navigační vlastnosti jsou vlastnosti zástupce, které slouží k vyhledání entit na koncích přidružení v modelu. Navigační vlastnosti lze vytvořit při vytváření přidružení mezi dvěma typy entit.

#### <a name="to-edit-navigation-properties"></a>Úprava vlastností navigace

-   Vyberte navigační vlastnost na povrchu EF návrháře.
    Informace o vlastnosti navigace se zobrazí v okně **vlastnosti** sady Visual Studio .
-   Změňte nastavení vlastnosti v okně **vlastnosti** .

#### <a name="to-delete-navigation-properties"></a>Odstranění vlastností navigace

-   Pokud nejsou cizí klíče vystaveny na typech entit v koncepčním modelu, odstraněním navigační vlastnosti může být v jednom směru provázáno odpovídající přidružení.
-   Klikněte pravým tlačítkem na navigační vlastnost na povrchu návrháře EF a vyberte **Odstranit**.
