---
title: Zlepšení výkonu při spuštění pomocí NGen-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: c9b5f8a06add9133d30955e3cc97a92e9b189bdf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182684"
---
# <a name="improving-startup-performance-with-ngen"></a>Zlepšení výkonu při spuštění pomocí NGen
> [!NOTE]
> **EF6 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 6. Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.  

.NET Framework podporuje generování nativních imagí pro spravované aplikace a knihovny jako způsob, jak rychleji spustit aplikace a také v některých případech používá méně paměti. Nativní bitové kopie jsou vytvořeny přečtením sestavení spravovaného kódu do souborů obsahujících nativní instrukce pro počítače před provedením aplikace, čímž dojde k uvolnění kompilátoru JIT (just-in-time) .NET JIT, aby bylo možné generovat nativní instrukce na adrese modul runtime aplikace  

Před verzí 6 byly základní knihovny modulu runtime EF součástí .NET Framework a nativní bitové kopie byly pro ně automaticky vygenerovány. Počínaje verzí 6 se celý modul runtime EF spojil do balíčku NuGet EntityFramework. K získání podobných výsledků je teď potřeba vygenerovat nativní Image pomocí nástroje příkazového řádku NGen. exe.  

Empirické pozorování ukazují, že nativní bitové kopie sestavení modulu runtime EF mohou v době spuštění aplikace vyjmout 1 až 3 sekundy.  

## <a name="how-to-use-ngenexe"></a>Jak používat NGen. exe  

Nejzákladnější funkce nástroje NGen. exe je "instalovat" (tj. vytvářet a uchovávat na disku) nativní bitové kopie pro sestavení a všechny jeho přímé závislosti. Tady je postup, jak to dosáhnout:  

1. Otevřete okno příkazového řádku jako správce.  
2. Změňte aktuální pracovní adresář na umístění sestavení, pro které chcete generovat nativní bitové kopie:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. V závislosti na operačním systému a konfiguraci aplikace možná budete muset vygenerovat nativní bitové kopie pro 32 bitovou architekturu, 64 bitovou architekturu nebo pro obojí.  

    Pro 32 bitového běhu:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Pro 64 bitového běhu:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Generování nativních imagí pro špatnou architekturu je velice Obvyklá chyba. V případě pochybností můžete jednoduše generovat nativní bitové kopie pro všechny architektury, které se vztahují k operačnímu systému nainstalovanému v počítači.  

NGen. exe také podporuje další funkce, jako je například odinstalace a zobrazení nainstalovaných nativních bitových kopií, zařazování do fronty generování více imagí atd. Další podrobnosti o použití naleznete v dokumentaci k nástroji [Ngen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Kdy použít NGen. exe  

Při rozhodování o tom, která sestavení pro generování nativních imagí v aplikaci na základě EF verze 6 nebo vyšší, byste měli vzít v úvahu následující možnosti:  

- **Sestavení modulu runtime pro hlavní EF, EntityFramework. dll**: Typická aplikace založená na EF spustí z tohoto sestavení značný objem kódu při spuštění nebo při prvním přístupu k databázi. V důsledku toho vytváření nativních imagí tohoto sestavení bude mít za následek největší nárůst výkonu při spuštění.  
- **Jakékoli sestavení poskytovatele EF používané vaší aplikací**: Čas spuštění může také mírně těžit z generování nativních bitových kopií. Například pokud aplikace používá poskytovatele EF pro SQL Server budete chtít vygenerovat nativní bitovou kopii pro EntityFramework. SqlServer. dll.  
- **Sestavení vaší aplikace a další závislosti**: [Dokumentace Ngen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx) obsahuje obecná kritéria pro výběr sestavení, která mají generovat nativní bitové kopie, a dopad nativních imagí na zabezpečení, pokročilých možností, jako jsou například "pevné vazby", scénáře, jako je například použití nativních imagí v ladění a scénáře profilace atd.  

> [!TIP]
> Nezapomeňte pečlivě změřit dopad použití nativních imagí na výkon při spuštění i na celkový výkon aplikace a porovnejte je s aktuálními požadavky. I když nativní bitové kopie budou obecně pomáhat zlepšit výkon při spuštění a v některých případech snižuje využití paměti, ne všechny scénáře budou mít stejnou výhodu. Například při spuštění ustáleného stavu (to znamená, že jakmile se všechny metody používané aplikací vyvolaly nejméně jednou) kód generovaný kompilátorem JIT může ve skutečnosti vracet poněkud lepší výkon než nativní bitové kopie.  

## <a name="using-ngenexe-in-a-development-machine"></a>Použití nástroje NGen. exe ve vývojovém počítači  

Během vývoje kompilátor JIT .NET nabízí nejlepší celkové kompromisy pro kód, který se často mění. Generování nativních bitových kopií pro zkompilované závislosti, jako je sestavení modulu runtime EF, může přispět k urychlení vývoje a testování tím, že se na začátku každého spuštění zajímá několik sekund.  

Vhodným místem pro vyhledání sestavení modulu runtime EF je umístění balíčku NuGet pro toto řešení. Například pro aplikaci, která používá EF 6.0.2 s SQL Server a cílí na rozhraní .NET 4,5 nebo vyšší, můžete v okně příkazového řádku zadat následující příkaz (Nezapomeňte ho otevřít jako správce):  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Tím se využívá skutečnost, že instalace nativních imagí pro poskytovatele EF pro SQL Server bude také standardně instalovat nativní bitové kopie pro hlavní sestavení modulu runtime EF. To funguje, protože NGen. exe dokáže detekovat, že EntityFramework. dll je přímá závislost sestavení EntityFramework. SqlServer. dll nacházející se ve stejném adresáři.  

## <a name="creating-native-images-during-setup"></a>Vytváření nativních imagí během instalace  

Sada nástrojů WiX podporuje zařazování do fronty generování nativních bitových kopií pro spravovaná sestavení během instalace, jak je vysvětleno v tomto [Průvodci](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Další možností je vytvořit vlastní úlohu instalace, která spustí příkaz NGen. exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Ověřuje se, jestli se pro EF používá nativní image.  

Můžete ověřit, zda konkrétní aplikace používá nativní sestavení, hledáním načtených sestavení, která mají příponu ". ni. dll" nebo ". ni. exe". Nativní bitová kopie pro hlavní sestavení modulu runtime EF se například nazývá EntityFramework. ni. dll. Snadný způsob, jak zkontrolovat načtená sestavení .NET procesu, je použít [Průzkumníka procesů](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Další věci, o kterých je třeba vědět  

**Vytvoření nativní bitové kopie sestavení by nemělo být zaměněno s registrací sestavení v globální [mezipaměti sestavení (GAC)](https://msdn.microsoft.com/library/yf1d93sz.aspx)** . Nástroj NGen. exe umožňuje vytvářet bitové kopie sestavení, která nejsou v globální mezipaměti sestavení (GAC), a ve skutečnosti může více aplikací, které používají určitou verzi EF, sdílet stejnou nativní bitovou kopii. I když systém Windows 8 může automaticky vytvářet nativní bitové kopie pro sestavení umístěná v mezipaměti GAC, modul runtime EF je optimalizován pro nasazení společně s vaší aplikací a nedoporučujeme ho registrovat v mezipaměti GAC, protože má negativní dopad na rozlišení sestavení a Údržba aplikací mezi další aspekty.  
