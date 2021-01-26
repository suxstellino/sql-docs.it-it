---
title: 'Creazione guidata Gruppo di disponibilità: Specificare le opzioni del gruppo di disponibilità'
description: Descrive le opzioni disponibili nella pagina "Specifica nome del gruppo di disponibilità" della Creazione guidata Gruppo di disponibilità in SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.specifyagname.f1
- sql13.swb.adddatabasewizard.specifyagname.f1
ms.assetid: dcb6374d-becb-4c6c-b88c-5a8273f8aa38
author: cawrites
ms.author: chadam
ms.openlocfilehash: f5171195bb3d275eb170330565cd9f90531dc0c6
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783402"
---
# <a name="specify-availability-group-options-page-for-an-always-on-availability-group"></a>Pagina Specificare le opzioni del gruppo di disponibilità per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento vengono descritte le opzioni della pagina **Specifica nome del gruppo di disponibilità** . Questo argomento viene utilizzato dalla [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] e dalla [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] di [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="specify-availability-group-options"></a><a name="PageOptions"></a> Pagina Specificare le opzioni del gruppo di disponibilità  
 **Nome del gruppo di disponibilità**  
 Specificare il nome del gruppo di disponibilità. Per un nuovo gruppo di disponibilità, specificare un identificatore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] univoco in tutti i gruppi di disponibilità nel cluster WSFC. La lunghezza massima consentita per il nome del gruppo di disponibilità è 128 caratteri.  

 **Tipo di cluster** Successivamente, specificare il tipo di cluster. I tipi di cluster possibili dipendono dalla versione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e dal sistema operativo. Selezionarne uno dall'elenco seguente:

   * **Windows Server Failover Clustering**
   
      Usarlo quando il gruppo di disponibilità è ospitato in istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che appartengono a un cluster di failover di Windows Server per il ripristino di emergenza e la disponibilità elevata. Si applica a tutte le versioni supportate di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 

   * **EXTERNAL**
      
      Usarlo quando il gruppo di disponibilità è ospitato in un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gestita da una tecnologia di cluster esterna per la disponibilità elevata e il ripristino di emergenza, ad esempio Pacemaker in Linux. Si applica a [!INCLUDE[sssqlv14](../../../includes/sssqlv14-md.md)] e versioni successive.

   * **NONE**
      
      Usarlo quando il gruppo di disponibilità è ospitato in un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che non è gestita da una tecnologia di cluster per la scalabilità in lettura e il bilanciamento del carico. Si applica a [!INCLUDE[sssqlv14](../../../includes/sssqlv14-md.md)] e versioni successive. 
 
   **Rilevamento dell'integrità a livello di database** Selezionare questa casella per abilitare l'opzione di rilevamento di integrità a livello di database (DB_FAILOVER) per il gruppo di disponibilità. Il rilevamento dell'integrità del database avvisa quando un database non è più nello stato online, quando si verifica un errore, e attiverà il failover automatico del gruppo di disponibilità. Vedere [Opzione di failover del rilevamento di integrità del database AlwaysOn in SQL Server](sql-server-always-on-database-health-detection-failover-option.md).


Pagina Selezione database (Creazione guidata Gruppo di disponibilità/Aggiungi database)  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> Attività correlate  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi database a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
