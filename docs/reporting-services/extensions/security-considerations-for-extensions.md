---
title: Considerazioni sulla sicurezza per le estensioni | Microsoft Docs
description: Informazioni su criteri, condizioni e requisiti di sicurezza che determinano il modo in cui i server di report concedono le autorizzazioni per le estensioni di Reporting Services.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- security [Reporting Services], extensions
- extensions [Reporting Services], security
- permissions [Reporting Services], extensions
ms.assetid: 58cbdfeb-1105-4a7d-a3b8-b897ff95f367
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8994d24fe6beacfd9dc11417d529575760e7dff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065246"
---
# <a name="security-considerations-for-extensions"></a>Considerazioni sulla sicurezza per le estensioni
  È necessario che ogni applicazione destinata a CLR (Common Language Runtime) interagisca con il relativo sistema di sicurezza. L'applicazione viene valutata automaticamente al momento dell'esecuzione e a tale applicazione viene concesso da CLR un set di autorizzazioni. A seconda delle autorizzazioni ricevute, l'applicazione può continuare a essere eseguita oppure viene generata un'eccezione di sicurezza. I criteri e le impostazioni di sicurezza locali nei file di configurazione dei criteri di sicurezza per un server di report specifico definiscono le autorizzazioni per il codice ricevute da un assembly.  
  
 Prima di richiedere le autorizzazioni, è necessario essere a conoscenza delle risorse e delle operazioni protette che verranno utilizzate dal codice dell'estensione, nonché sapere quali autorizzazioni proteggono tali risorse e operazioni. È inoltre necessario tenere traccia di ogni risorsa alla quale accedono i vari metodi della libreria di classi chiamati dai componenti dell'estensione. Per ulteriori informazioni, vedere "Richiesta di autorizzazioni" nella Guida per gli sviluppatori di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Le estensioni distribuite in un server di report devono essere eseguite come completamente attendibili, vale a dire che l'estensione deve far parte di un gruppo di codice a cui sia concesso il set di autorizzazioni **FullTrust**. Questo significa inoltre che l'estensione può disporre di accesso a determinate operazioni e risorse del server disponibili tramite CLR, a seconda dell'utente autenticato per un report specifico. Per altre informazioni sui gruppi di codice e sulle estensioni, vedere [Sicurezza dall'accesso di codice in Reporting Services](../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md).  
  
> [!IMPORTANT]  
>  In [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene applicata la sicurezza di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] per tutte le estensioni.  
  
 Le condizioni seguenti riguardano la distribuzione delle estensioni di elaborazione dati, recapito, rendering e sicurezza in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
-   Solo l'amministratore locale dispone dell'autorizzazione per la distribuzione di un'estensione.  
  
-   Solo gli utenti con le autorizzazioni di lettura/scrittura appropriate possono modificare i file di configurazione per il componente di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che viene esteso.  
  
-   Solo gli utenti con privilegi dispongono dell'autorizzazione per la modifica dei file dei criteri di sicurezza e l'abilitazione della sicurezza da accesso di codice per un'estensione.  
  
 Per altre informazioni sulla sicurezza dall'accesso di codice in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Sviluppo sicuro &#40;Reporting Services&#41;](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
 Per ulteriori informazioni sulla sicurezza in [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vedere 'argomento relativo alla sicurezza di .NET Framework nella Guida per gli sviluppatori di [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
## <a name="initialization-of-extension-assemblies"></a>Inizializzazione degli assembly di estensioni  
 Quando le estensioni vengono caricate per la prima volta in memoria dal server di report, utilizzano le credenziali dell'account di servizio, in quanto alcuni assembly di estensioni richiedono autorizzazioni specifiche per l'accesso alle risorse di sistema, la lettura dei file di configurazione e il caricamento di altri assembly dipendenti. Dopo che un assembly è stato caricato e inizializzato, tuttavia, per tutte le chiamate successive agli assembly di estensioni vengono utilizzate le credenziali dell'account utente che ha attualmente effettuato l'accesso.  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../reporting-services/extensions/reporting-services-extensions.md)   
 [Libreria di estensioni di Reporting Services](../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
