---
title: Relace – EF designeru - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490648"
---
# <a name="relationships---ef-designer"></a>Relace – EF designeru
> [!NOTE]
> Tato stránka obsahuje informace o nastavení relace v modelu pomocí EF designeru. Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md).

Přidružení definovat vztahy mezi typy entit v modelu. Toto téma ukazuje, jak mapovat přidružení se Návrhář Entity Framework (EF designeru). Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.

![EF designeru](~/ef6/media/efdesigner.png)

> [!NOTE]
> Při vytváření konceptuální model, upozornění na nenamapované entit a přidružení může zobrazit v seznamu chyb. Tato upozornění můžete ignorovat, protože až zvolíte, aby se vygenerovala databáze z modelu, chyby zmizí.

## <a name="associations-overview"></a>Přehled přidružení

Při návrhu modelu pomocí EF designeru soubor .edmx představuje model. V souboru .edmx **přidružení** element definuje vztah mezi dvěma typy entit. Asociace musíte zadat typy entit, které jsou zahrnuty v relaci a možný počet typů entit na každém konci vztahu, který je označován jako násobnost. Násobnost zakončení přidružení může mít hodnotu jedna (1), žádný nebo jeden (0..1) nebo mnoho (\*). Tato informace zadána v dva podřízené **End** elementy.

Za běhu instance typu entity na jednom konci asociace je přístupný prostřednictvím vlastnosti navigace nebo cizí klíče (Pokud se rozhodnete vystavit cizí klíče v entitách). S vystavený cizí klíče, je vztah mezi entitami spravované pomocí **elementu ReferentialConstraint** – element (podřízený prvek **přidružení** element). Doporučujeme vždy vystavit cizí klíče u relací v entitách.

> [!NOTE]
> V m (\*:\*) cizí klíče nelze přidat do entity. V \*:\* vztah, informací o přidružení se spravuje pomocí nezávislý objekt.

Informace o prvcích CSDL (**elementu ReferentialConstraint**, **přidružení**atd) najdete v článku [specifikace CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Vytvářet a odstraňovat přidružení

Vytváření přidružení k EF designeru aktualizace modelu obsahu souboru .edmx. Po vytvoření asociace, musíte vytvořit mapování pro přidružení (popsáno dále v tomto tématu).

> [!NOTE]
> V této části se předpokládá, že jste už přidali entity, které chcete vytvořit přidružení mezi do modelu.

### <a name="to-create-an-association"></a>Vytvoření přidružení

1.  Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, přejděte na **přidat nový**a vyberte **přidružení...** .
2.  Zadejte nastavení pro přidružení v **Přidat přidružení** dialogového okna.

    ![Přidat přidružení](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Můžete také bez přidání navigační vlastnosti nebo vlastnosti cizího klíče k entitám stranách asociace zrušením ** navigační vlastnost ** a ** přidat vlastnosti cizího klíče &lt;názvu typu entity&gt; Entity ** Zaškrtávací políčka. Pokud chcete přidat pouze jednu vlastnost navigace, přidružení bude pouze v jednom směru umožňujících přechody. Pokud chcete přidat žádné navigační vlastnosti, musíte zvolit přidání vlastnosti cizího klíče pro přístup k entitám stranách asociace.
    
3.  Klikněte na tlačítko **OK**.

### <a name="to-delete-an-association"></a>Odstranit spojení

Chcete-li odstranit přidružení proveďte jednu z následujících akcí:

-   Klikněte pravým tlačítkem na asociace v EF designeru ploše a vyberte možnost **odstranit**.

- OR –

-   Vyberte jeden nebo více přidružení a stiskněte klávesu DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Zahrnují vlastnosti cizího klíče v entitách (referenční omezení)

Doporučujeme vždy vystavit cizí klíče u relací v entitách. Entity Framework používá referenční omezení k určení, že vlastnost funguje jako cizí klíč pro relaci.

Pokud jste zaškrtli možnost ***přidat vlastnosti cizího klíče &lt;názvu typu entity&gt; Entity*** zaškrtávací políčko při vytvoření relace, pro vás byl přidán tento referenční omezení.

Použijete-li přidat nebo upravit referenční omezení EF designeru, EF designeru přidá nebo upraví **elementu ReferentialConstraint** prvek CSDL obsah souboru .edmx.

-   Dvakrát klikněte na panel přidružení, na kterou chcete upravit.
    **Referenční omezení** zobrazí se dialogové okno.
-   Z **hlavní** rozevíracího seznamu vyberte hlavní entity v referenčním omezení.
    Klíčové vlastnosti entity jsou přidány do **klíč objektu** seznam v dialogovém okně.
-   Z **závislé** rozevíracího seznamu vyberte závislé entity v referenčním omezení.
-   Pro každý klíč instančního objektu, který má závislé klíč, vyberte z rozevíracích seznamů v odpovídajícím klíčem závislé **závislé klíč** sloupce.

    ![Omezení REF](~/ef6/media/refconstraint.png)

-   Klikněte na tlačítko **OK**.

## <a name="create-and-edit-association-mappings"></a>Vytvořte a upravte mapování přidružení

Můžete určit, jak mapuje přidružení k databázi **podrobnosti mapování** okno EF designeru.

> [!NOTE]
> Můžete namapovat pouze podrobnosti pro přidružení, které nemají zadaný referenční omezení. Pokud je zadáno referenční omezení vlastností cizího klíče je zahrnuta v entitě a podrobnosti o mapování můžete použít entity na sloupec cizího klíče mapuje na ovládací prvek.

### <a name="create-an-association-mapping"></a>Vytvoření mapování přidružení

-   Klikněte pravým tlačítkem na přidružení v návrhové ploše a vyberte **mapování tabulky,**.
    Zobrazí se přidružení mapování v **podrobnosti mapování** okna.
-   Klikněte na tlačítko **přidat tabulku nebo zobrazení**.
    Se zobrazí rozevírací seznam obsahující všechny tabulky v modelu úložiště.
-   Vyberte tabulku, do kterého se namapuje přidružení.
    **Podrobnosti mapování** okně se zobrazí oba konce přidružení a klíčové vlastnosti pro typ entity v každé **End**.
-   Pro každou vlastnost klíče, klikněte na tlačítko **sloupec** pole a vyberte sloupec, do kterého se namapuje vlastnost.

    ![Podrobnosti mapování 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Upravit mapování přidružení

-   Klikněte pravým tlačítkem na přidružení v návrhové ploše a vyberte **mapování tabulky,**.
    Zobrazí se přidružení mapování v **podrobnosti mapování** okna.
-   Klikněte na tlačítko **mapuje &lt;název tabulky&gt;**.
    Se zobrazí rozevírací seznam obsahující všechny tabulky v modelu úložiště.
-   Vyberte tabulku, do kterého se namapuje přidružení.
    **Podrobnosti mapování** okně se zobrazí oba konce přidružení a klíčové vlastnosti typu entity na každém konci.
-   Pro každou vlastnost klíče, klikněte na tlačítko **sloupec** pole a vyberte sloupec, do kterého se namapuje vlastnost.

## <a name="edit-and-delete-navigation-properties"></a>Upravit a odstranit navigační vlastnosti

Navigační vlastnosti jsou místní vlastnosti, které se používají pro vyhledání entity na konci asociace v modelu. Navigační vlastnosti lze vytvořit při vytvoření přidružení mezi dvěma typy entit.

#### <a name="to-edit-navigation-properties"></a>Chcete-li upravit vlastnosti navigace

-   Vyberte vlastnost navigace na povrchu EF designeru.
    Informace o vlastnosti navigace se zobrazí v sadě Visual Studio **vlastnosti** okna.
-   Změňte nastavení vlastnosti **vlastnosti** okna.

#### <a name="to-delete-navigation-properties"></a>Chcete-li odstranit navigační vlastnosti

-   Pokud cizí klíče nejsou přístupná na typy entit v konceptuálním modelu, odstranění navigační vlastnost, aby odpovídající přidružení pouze v jednom směru umožňujících přechody nebo není umožňujících přechody vůbec.
-   Klikněte pravým tlačítkem na vlastnost navigace v EF designeru ploše a vyberte možnost **odstranit**.
