---
title: rsAccessedDenied - Errore di Reporting Services | Microsoft Docs
description: "Questa pagina di riferimento degli errori contiene informazioni su \"rsAccessedDenied\": Le autorizzazioni concesse all'utente 'mydomain\\myAccount' non sono sufficienti per eseguire questa operazione."
ms.date: 05/22/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- rsAccessDenied error
ms.assetid: 2f76b1bf-96a2-4755-b76b-84e933220efc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 20d213cac8f3d3413266dc410b0d66879e771619
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054184"
---
# <a name="rsaccesseddenied---reporting-services-error"></a>rsAccessedDenied - Errore di Reporting Services
  L'errore di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**rsAccessedDenied** si verifica quando un utente non dispone dell'autorizzazione per eseguire un'azione, ad esempio non ha un'assegnazione di ruolo che consenta di aprire un report o non ha aperto il browser con le autorizzazioni necessarie.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modalità nativa &#124; modalità SharePoint|  
  
- Se l'errore si è verificato durante l'accesso diretto al server di report tramite URL, verrà eseguito il mapping tra l'eccezione e un errore HTTP 401.  
  
- Se l'errore si è verificato durante l'uso del portale Web, l'eccezione è in genere associata a un errore HTTP 401 o a un'altra pagina di errore HTML definito.  
  
- Se si è verificato durante un'operazione, una sottoscrizione o un recapito pianificato, l'errore verrà indicato solo nel file di log del server di report.  
  
## <a name="details"></a>Dettagli  
  
|Dettaglio|Valore|  
|-|-|  
|**Nome prodotto**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**ID evento**|rsAccessedDenied|  
|**Origine evento**|Microsoft.ReportingServices.Diagnostics.Utilities.ErrorStrings|  
|**Componente**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|**Testo del messaggio**|Le autorizzazioni concesse all'utente 'mydomain\myAccount' non sono sufficienti per eseguire questa operazione. (rsAccessDenied) (ReportingServicesLibrary)|  
  
## <a name="user-action"></a>Azione utente  
 Le autorizzazioni che consentono di accedere al contenuto e alle operazioni del server di report vengono concesse tramite assegnazioni di ruolo. In una nuova installazione solo gli amministratori locali possono accedere a un server di report. Per concedere l'accesso ad altri utenti, l'amministratore locale deve creare un'assegnazione di ruolo che specifica un account di gruppo o un account utente di dominio, uno o più ruoli che definiscono le attività eseguibili dall'utente, nonché un ambito, ovvero in genere la cartella Home o il nodo radice della gerarchia di cartelle del server di report. È possibile usare il portale Web per creare assegnazioni di ruolo. Per altre informazioni, vedere [Assegnazioni di ruolo](../../reporting-services/security/role-assignments.md).  
  
 Questo errore viene causato anche dall'amministratore locale del server di report. Per altre informazioni, vedere [Configurare un server di report in modalità nativa per gli amministratori locali &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Assegnazioni di ruolo](../../reporting-services/security/role-assignments.md)  
 [Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
 [Ruoli e autorizzazioni &#40;Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md)  
  