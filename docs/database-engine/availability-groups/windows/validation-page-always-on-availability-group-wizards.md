---
title: 'Creazione guidata Gruppo di disponibilità: Pagina Convalida'
description: Questo argomento descrive le opzioni disponibili nella pagina Convalida della creazione guidata gruppo di disponibilità Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.validation.f1
- sql13.swb.adddatabasewizard.validation.f1
- sql13.swb.newagwizard.validation.f1
helpviewer_keywords:
- ', listeners'
ms.assetid: c8971556-240c-491a-bc86-9cc72f71a3dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: b4d05d10c54806b7ffeec607ffc1b50e27bfe5a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355089"
---
# <a name="validation-page-always-on-availability-group-wizards"></a>Pagina Convalida (procedure guidate gruppi di disponibilità Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
  Questo argomento della Guida descrive le opzioni della pagina **Convalida** . Questo argomento si applica alla [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)], alla [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)], alla [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Utilizzare questa pagina per verificare che l'ambiente supporta tutte le scelte di configurazione effettuate nelle pagine precedenti della procedura guidata.  
  
##  <a name="validation-page-options"></a><a name="PageOptions"></a> Opzioni della pagina Convalida  
 **Risultati della convalida del gruppo di disponibilità.**  
 In questa griglia vengono visualizzati i risultati di ogni passaggio di convalida completato. Le colonne della griglia sono le seguenti:  
  
 **Nome**  
 Visualizza una frase che descrive un passaggio specifico.  
  
 **Risultato**  
 Visualizza uno dei seguenti testi di collegamenti ipertestuali. Per ulteriori informazioni sul risultato del passaggio di convalida specificato, fare clic sul collegamento ipertestuale.  
  
|Risultato|Descrizione|  
|------------|-----------------|  
|**Error (Errore) (Error (Errore)e)**|Indica che il passaggio di convalida non è riuscito. Fare clic sul collegamento per visualizzare il messaggio di errore.|  
|**Ignorato**|Indica che il passaggio di convalida è stato ignorato perché non è necessario in base alle selezioni. Fare clic sul collegamento per visualizzare il motivo per cui un passaggio è stato ignorato.|  
|**Success**|Indica che il passaggio di convalida è riuscito.|  
|**Warning**|Indica un potenziale problema con la configurazione del gruppo di disponibilità.  Fare clic sul collegamento per visualizzare il messaggio di avviso.|  
  
 **Ripeti convalida**  
 Fare clic per ripetere i passaggi di convalida se si apporta una modifica al di fuori della procedura guidata in risposta a un errore di convalida.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi database a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
