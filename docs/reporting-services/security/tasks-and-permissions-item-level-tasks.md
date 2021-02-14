---
description: Attività e autorizzazioni - Attività a livello di elemento
title: Attività a livello di elemento | Microsoft Docs
ms.date: 02/04/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- item-level tasks [Reporting Services]
ms.assetid: fdeb7bc3-167a-4342-84e3-32e3faa1fa39
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 82101577abe86914aff2ac1d5296d7dc176a60b0
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570435"
---
# <a name="tasks-and-permissions---item-level-tasks"></a>Attività e autorizzazioni - Attività a livello di elemento
  
  [!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]
  
  Un'attività a livello di elemento è una raccolta di autorizzazioni correlate a un report, una cartella, un modello di report, una risorsa o un'origine dati condivisa. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] include anche attività a livello di sistema applicabili all'intero sito del server di report. Per altre informazioni, vedere [Attività a livello di sistema](../../reporting-services/security/tasks-and-permissions-system-level-tasks.md). Per ulteriori informazioni sulle attività e le autorizzazioni in generale, vedere [Tasks and Permissions](../../reporting-services/security/tasks-and-permissions.md).  
  
> [!NOTE]  
>  Se si gestiscono queste attività a livello di programmazione, è necessario utilizzare metodi che supportano attività a livello di elemento. Per altre informazioni, vedere <xref:ReportService2010.ReportingService2010.ListTasks%2A> e <xref:ReportService2010.ReportingService2010.ListRoles%2A>.  
  
## <a name="permissions-in-item-level-tasks"></a>Autorizzazioni delle attività a livello di elemento  
 Nella tabella seguente vengono elencate le attività a livello di elemento e le relative autorizzazioni e gli elementi ai quali si applicano tali autorizzazioni. L'elenco delle autorizzazioni è puramente informativo e ha lo scopo di offrire una descrizione più precisa delle funzionalità disponibili tramite ogni attività.  
  
 I set di dati condivisi utilizzano lo stesso set di autorizzazioni dei report. Le parti del report utilizzano lo stesso set di autorizzazioni delle risorse.  
  
|Attività|Applicabile all'elemento|Autorizzazioni|  
|----------|---------------------|-----------------|  
|Aggiungere commenti sui report<br />(SSRS 2017 e versioni successive, Server di report di Power BI)|Report|Lettura di proprietà<br /><br /> Crea Commenti<br /><br /> Elimina commenti<br /><br /> Leggi Commenti<br /><br /> Aggiornare i commenti|  
|Utilizzo di report|Report|Lettura di contenuto<br /><br /> Lettura delle definizioni dei report<br /><br /> Lettura di proprietà|  
|Utilizzo di report|Set di dati condivisi|Lettura di contenuto<br /><br /> Lettura delle definizioni dei report<br /><br /> Lettura di proprietà|  
|Creazione di report collegati|Report|Creazione di collegamenti<br /><br /> Lettura di proprietà|  
|Gestione di tutte le sottoscrizioni|Report|Lettura di proprietà<br /><br /> Lettura di qualsiasi sottoscrizione<br /><br /> Creazione di qualsiasi sottoscrizione<br /><br /> Eliminazione di qualsiasi sottoscrizione<br /><br /> Aggiornamento di qualsiasi sottoscrizione|  
|Gestisci commenti<br />(SSRS 2017 e versioni successive, Server di report di Power BI)|Report|Lettura di proprietà<br /><br />Elimina tutti i commenti|  
|Gestire le origini dati|Cartelle|Creazione di origini dei dati|  
|Gestire le origini dati|Origini dati|Aggiornamento di proprietà<br /><br /> Eliminazione del contenuto di aggiornamento<br /><br /> Lettura di proprietà|  
|Gestione di cartelle|Cartelle|Creazione cartella<br /><br /> Eliminazione delle proprietà di aggiornamento<br /><br /> Lettura di proprietà|  
|Gestione di sottoscrizioni individuali|Report|Lettura di proprietà<br /><br /> Creazione di sottoscrizioni<br /><br /> Eliminazione di sottoscrizioni<br /><br /> Lettura di sottoscrizioni<br /><br /> Aggiornamento di sottoscrizioni|  
|Gestire i modelli|Cartelle|Creazione di un modello|  
|Gestire i modelli|Modelli|Lettura di proprietà<br /><br /> Lettura di contenuto<br /><br /> Eliminazione del contenuto di aggiornamento<br /><br /> Lettura di origini dei dati<br /><br /> Aggiornamento di origini dei dati<br /><br /> Lettura dei criteri di autorizzazione degli elementi del modello<br /><br /> Aggiornamento dei criteri di autorizzazione degli elementi del modello<br /><br /> Eliminazione delle proprietà di aggiornamento|  
|Gestione della cronologia dei report|Report|Lettura di proprietà<br /><br /> Creazione della cronologia dei report<br /><br /> Eliminazione della cronologia dei report<br /><br /> Esecuzione dei criteri di lettura<br /><br /> Aggiornamento di criteri<br /><br /> Visualizzazione della cronologia dei report|  
|Gestione di report|Cartelle|Creazione di report<br /><br /> applicabile anche alla creazione di set di dati condivisi|  
|Gestione di report|Report|Lettura di proprietà<br /><br /> Eliminazione delle proprietà di aggiornamento<br /><br /> Aggiornamento di parametri<br /><br /> Lettura di origini dei dati<br /><br /> Aggiornamento di origini dei dati<br /><br /> Lettura delle definizioni dei report<br /><br /> Aggiornamento delle definizioni dei report<br /><br /> Esecuzione dei criteri di lettura<br /><br /> Aggiornamento di criteri|  
|Gestione di report|Set di dati condivisi|Lettura di proprietà<br /><br /> Eliminazione delle proprietà di aggiornamento<br /><br /> Aggiornamento di parametri<br /><br /> Lettura di origini dei dati<br /><br /> Aggiornamento di origini dei dati<br /><br /> Lettura delle definizioni dei report<br /><br /> Aggiornamento delle definizioni dei report<br /><br /> Esecuzione dei criteri di lettura<br /><br /> Aggiornamento di criteri|  
|Gestione di risorse|Cartelle|Creare la risorsa|  
|Gestione di risorse|Risorse|Aggiornamento di proprietà<br /><br /> Eliminazione del contenuto di aggiornamento<br /><br /> Lettura di proprietà|  
|Gestione di risorse|Parti del report|Aggiornamento di proprietà<br /><br /> Eliminazione del contenuto di aggiornamento<br /><br /> Lettura di proprietà|  
|Impostazione della sicurezza per singoli elementi|Report, risorse, origini dei dati, set di dati condivisi, cartelle|Lettura di criteri di sicurezza - Aggiornamento di criteri di sicurezza|  
|Visualizzazione di origini dei dati|Origini dati|Lettura di contenuto<br /><br /> Lettura di proprietà|  
|Visualizzazione di cartelle|Cartelle|Lettura di proprietà<br /><br /> Esecuzione e visualizzazione<br /><br /> Visualizzazione della cronologia dei report|  
|Visualizzazione di modelli|Modelli di report|Lettura di proprietà<br /><br /> Lettura di contenuto<br /><br /> Lettura di origini dei dati|  
|Visualizzare i report|Report|Lettura di contenuto<br /><br /> Lettura di proprietà|  
|Visualizzare i report|Set di dati condivisi|Lettura di contenuto<br /><br /> Lettura di proprietà|  
|Visualizzare le risorse|Risorse|Lettura di contenuto<br /><br /> Lettura di proprietà|  
|Visualizzare le risorse|Parti del report|Lettura di contenuto<br /><br /> Lettura di proprietà|  
  
## <a name="see-also"></a>Vedere anche  
 [Concessione di autorizzazioni in un server di report in modalità nativa](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
