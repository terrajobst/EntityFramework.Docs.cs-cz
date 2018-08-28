---
title: Zlepšuje výkon při spouštění pomocí technologie NGen – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 324769ca6ee95e1576cf03d20e569f8b24778119
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998332"
---
# <a name="improving-startup-performance-with-ngen"></a>Zlepšuje výkon při spouštění pomocí technologie NGen
> [!NOTE]
> **EF6 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny informace neplatí.  

Rozhraní .NET Framework podporuje generování nativních bitových kopií pro spravované aplikace a knihovny jako způsob, jak pomoci rychleji spouštět aplikace a také v některých případech použijte méně paměti. Nativní bitové kopie jsou vytvořeny pomocí překladu sestavení spravovaného kódu do souborů před spuštěním aplikace, který obsahuje nativních strojové instrukce homogenního .NET JIT (Just-In-Time) kompilátorem by bylo nutné generovat nativní pokynů na adrese Doba spuštění aplikace.  

Starší než verze 6 knihovny EF runtime core byly součástí rozhraní .NET Framework a nativní bitové kopie byly automaticky generovány pro ně. Od verze 6 celý modul runtime EF byl sloučen do balíčku NuGet objektu EntityFramework. Nativní bitové kopie do tohoto okamžiku se vygenerovaly pomocí příkazového řádku nástroje NGen.exe získat podobné výsledky.  

Empirických pozorování ukazují, že můžete vyjmout nativní bitové kopie sestavení modulu runtime EF mezi 1 a 3 sekund dobu spuštění aplikace.  

## <a name="how-to-use-ngenexe"></a>Jak používat NGen.exe  

Většina základních funkcí nástroj NGen.exe je "instalaci" (to znamená, že pro vytvoření a uložení na disk) nativních bitových kopií pro sestavení a všechny její závislosti s přímým přístupem. Tady je způsob, který můžete dosáhnout:  

1. Otevřete okno příkazového řádku jako správce  
2. Změňte aktuální pracovní adresář na umístění sestavení, kterou chcete vygenerovat nativních bitových kopií pro:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. V závislosti na operačním systému a konfigurace aplikace může potřebovat ke generování nativních bitových kopií pro 32bitovou architekturu 64bitovou architekturu nebo oba systémy.  

    Pro 32bitové spustit:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Pro 64bitový spustit:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Generování nativních bitových kopií pro chybný architekturu je velmi běžné chyby. Pokud máte pochybnosti můžete jednoduše generování nativních bitových kopií pro všechny architektury, které platí pro operační systém nainstalovaný na počítači.  

NGen.exe podporuje také další funkce, jako je například odinstalace a zobrazení nainstalovaných nativní bitové kopie, služby Řízení front generování více imagí, atd. Podrobnosti o použití najdete [NGen.exe dokumentaci](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Kdy použít NGen.exe  

Pokud jde o rozhodnout, která sestavení pro generování nativních bitových kopií pro aplikace založené na EF verze 6 nebo novější, je třeba zvážit následující možnosti:  

- **Hlavní EF sestavení modulu runtime, EntityFramework.dll**: Typická aplikace EF na základě spustí značné množství kódu z tohoto sestavení při spuštění nebo na jeho první přístup k databázi. Vytvoření nativní bitové kopie tohoto sestavení. v důsledku toho vytvoří největší nárůst výkon při spuštění.  
- **Jakékoli sestavení zprostředkovatele na EF používaný vaší aplikací**: spuštění také využívají výhod mírně generování nativních bitových kopií z nich. Například pokud aplikace používá zprostředkovatele EF pro SQL Server můžete ke generování nativních bitových kopií pro EntityFramework.SqlServer.dll.  
- **Sestavení vaší aplikace a další závislosti**: [NGen.exe dokumentaci](https://msdn.microsoft.com/library/6t9t5wcf.aspx) popisuje obecná kritéria pro výběr sestavení pro generování nativních bitových kopií pro a dopad na zabezpečení, nativní bitové kopie Rozšířené možnosti, jako například "Pevná vazba" scénáře, jako je třeba použití nativních bitových kopií při ladění a profilování scénáře atd.  

> [!TIP]
> Ujistěte se, že pečlivě měřit dopad na výkon při spouštění a celkového výkonu aplikace pomocí nativních bitových kopií a porovnání skutečné požadavky. Zatímco nativní bitové kopie obvykle pomůže zvýšit výkon spuštění a v některých případech omezit využití paměti, bude rovnoměrně přínosem pro všechny scénáře. Například na stálé spuštění (po všechny metody, které používá aplikaci alespoň jednou vyvolání) kód generovaný kompilátorem JIT může ve skutečnosti přinést mírně vyšší výkon než nativní bitové kopie.  

## <a name="using-ngenexe-in-a-development-machine"></a>Pomocí NGen.exe ve vývojovém počítači  

Při vývoji v .NET JIT kompilátoru nabídne doporučené celkové kompromis pro kód, který se často mění. Generování nativních bitových kopií pro kompilované závislosti, jako jsou sestavení modulu runtime EF pomáhá urychlit vývoj a testování pomocí cutting pár sekund navýšení kapacity na začátku každé spuštění.  

Je vhodné místo, kde můžete najít sestavení modulu runtime EF umístění balíčku NuGet pro řešení. Pro aplikaci pomocí EF 6.0.2 s SQL serverem a cílit na rozhraní .NET 4.5 nebo vyšší můžete například zadat následující, v okně příkazového řádku (nezapomeňte ho spusťte jako správce):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Toto využívá skutečnost, že instalaci nativních bitových kopií pro EF zprostředkovatele pro SQL Server také ve výchozím nastavení nainstaluje nativních bitových kopií pro hlavní sestavení modulu runtime EF. Tento postup funguje, protože NGen.exe lze zjistit, že EntityFramework.dll přímou závislost sestavení EntityFramework.SqlServer.dll nachází ve stejném adresáři.  

## <a name="creating-native-images-during-setup"></a>Během instalace vytváření nativních obrázků  

Podporuje se sadou nástrojů WiX služby Řízení front generování nativních bitových kopií pro spravovaná sestavení během instalace, jak je popsáno v tomto [Příručka](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Další možností je vytvořit vlastní nastavení úkol, který spustíte příkaz NGen.exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Ověřuje se, že se nativní bitové kopie slouží pro EF  

Můžete ověřit, že konkrétní aplikace používá nativní sestavení tím, že hledají načtená sestavení, které mají příponu ". ni.dll"nebo". ni.exe". Například nativní bitovou kopii pro sestavení EF hlavní modul runtime zavolá se EntityFramework.ni.dll. Snadný způsob, jak kontrolovat načtená sestavení .NET procesu je použití [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Je dobré mít na paměti  

**Vytváření nativních bitových kopií sestavení, neměly by být zaměňovány s registrací v sestavení [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe umožňuje vytváření bitových kopií sestavení, které nejsou v mezipaměti GAC, a ve skutečnosti více aplikací, které používají určité verze EF můžou sdílet stejné nativních bitových kopií. Když Windows 8 může automaticky vytvářet nativní bitové kopie pro sestavení, které jsou umístěny v mezipaměti GAC, modul runtime EF je optimalizována pro se nasadí společně s vaší aplikací a nedoporučujeme registrace v mezipaměti GAC, jak to nemá negativní vliv na sestavení řešení a Údržba aplikací mezi další aspekty.  
