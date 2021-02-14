---
title: Aggiungere un database a un gruppo di disponibilità con la "Creazione guidata Gruppo di disponibilità"
description: Aggiungere un database a un gruppo di disponibilità Always On usando la "Creazione guidata Gruppo di disponibilità" in SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.adddatabasewizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], databases
ms.assetid: 81e5e36d-735d-4731-8017-2654673abb88
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0147846a3f7ec66295895812ac828a12c99d5632
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340907"
---
# <a name="add-a-database-to-an-always-on-availability-group-with-the-availability-group-wizard"></a>Aggiungere un database a un gruppo di disponibilità Always On con la "Creazione guidata Gruppo di disponibilità"
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Usare la procedura guidata Aggiungi database a gruppo di disponibilità per aggiungere uno o più database a un gruppo di disponibilità Always On esistente.  
  
> [!NOTE]  
>  Per informazioni sull'uso di [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell per aggiungere un database, vedere [Aggiungere un database a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
 Se non è mai stato aggiunto un database a un gruppo di disponibilità, vedere la sezione "Database di disponibilità" in [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites-restrictions-and-recommendations"></a><a name="Prerequisites"></a> Prerequisiti, restrizioni e raccomandazioni  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria corrente.  
  
-   **Prerequisiti per l'utilizzo della sincronizzazione dati iniziale completa**  
  
    -   Tutti i percorsi di file dei database devono essere identici in ogni istanza del server in cui è ospitata una replica per il gruppo di disponibilità.  
  
    -   In un'istanza del server in cui è ospitata una replica secondaria non può essere presente alcun nome di database primario. Ciò significa che non può essere ancora presente alcun nuovo database secondario.  
  
    -   Sarà necessario specificare una condivisione di rete affinché la procedura guidata sia in grado di creare e accedere ai backup. Per la replica primaria, all'account usato per avviare il [!INCLUDE[ssDE](../../../includes/ssde-md.md)] devono essere associate le autorizzazioni del file system in lettura e scrittura per una condivisione di rete. Per le repliche secondarie, all'account deve essere associata l'autorizzazione di lettura per la condivisione di rete.  
  
     Se non è possibile usare la procedura guidata per eseguire la sincronizzazione dei dati iniziale completa, sarà necessario preparare i database secondari manualmente. Tale operazione può essere eseguita prima o dopo l'esecuzione della procedura guidata. Per altre informazioni, vedere [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="use-the-new-availability-group-wizard"></a>Usare la "Creazione guidata Gruppo di disponibilità"
  
1.  In Esplora oggetti connettersi all'istanza del server che ospita la replica primaria del gruppo di disponibilità ed espandere l'albero del server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic con il pulsante destro del mouse sul gruppo di disponibilità a cui si aggiunge un database e selezionare il comando **Aggiungi database** . Verrà avviata la procedura guidata Aggiungi database a gruppo di disponibilità.  
  
4.  Nella pagina **Seleziona database** selezionare uno o più database. Per altre informazioni, vedere [Pagina Selezione database &#40;Creazione guidata Gruppo di disponibilità/Aggiungi database&#41;](../../../database-engine/availability-groups/windows/select-databases-page-new-availability-group-wizard-and-add-database-wizard.md).  
  
     Se il database contiene una chiave master di database, immettere la password per la chiave master di database nella colonna **Password** .  
  
5.  Nella pagina **Seleziona sincronizzazione dei dati iniziale** specificare come creare e aggiungere i nuovi database secondari al gruppo di disponibilità. Scegliere una delle opzioni seguenti:  

    - **Seeding automatico**
      
      Selezionare questa opzione per usare il seeding automatico. Il seeding automatico usa il trasporto del flusso di log per trasmettere il backup mediante un'infrastruttura VDI nella replica secondaria per ogni database del gruppo di disponibilità tramite endpoint configurati. Ciò consente di ripristinare il backup del database nella replica secondaria senza doverlo fare manualmente. Per altre informazioni sul seeding automatico, vedere [Seeding automatico](automatic-seeding-secondary-replicas.md).
  
    -   **Completo**  
  
         Selezionare questa opzione se l'ambiente soddisfa i requisiti per l'avvio automatico della sincronizzazione dei dati iniziale. Per altre informazioni, vedere [Prerequisiti, restrizioni e raccomandazioni](#Prerequisites), più indietro in questo argomento.  
  
         Se si seleziona **Completo**, dopo avere creato il gruppo di disponibilità verrà tentato il backup di ogni database primario e del relativo log delle transazioni su una condivisione di rete e verranno ripristinati i backup su ogni istanza del server in cui è ospitata una replica secondaria. Ogni database secondario verrà quindi aggiunto al gruppo di disponibilità.  
  
         Nel campo **Specificare un percorso di rete condiviso accessibile da tutte le repliche:** specificare una condivisione di backup a cui possano accedere in lettura e scrittura tutte le istanze del server che ospitano repliche. I backup del log faranno parte della catena di backup del log. Archiviare i file di backup del log in modo appropriato.  
  
        > [!IMPORTANT]  
        >  Per informazioni sulle autorizzazioni del file system necessarie, vedere [Prerequisiti](#Prerequisites)più indietro in questo argomento.  
  
    -   **Solo join**  
  
         Se sono stati preparati manualmente database secondari sulle istanze del server che ospiteranno le repliche secondarie, è possibile selezionare questa opzione. I database secondari esistenti verranno uniti al gruppo di disponibilità tramite la procedura guidata.  
  
    -   **Ignora sincronizzazione dei dati iniziale**  
  
         Selezionare questa opzione se si desidera usare i backup dei database e del log dei database primari. Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
     Per altre informazioni, vedere la pagina [Seleziona sincronizzazione dati iniziale &#40;procedure guidate Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md).  
  
6.  Nella pagina **Connetti a repliche secondarie esistenti** , se le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospitano le repliche di disponibilità per questo gruppo di disponibilità vengono tutte eseguite come un servizio nello stesso account utente, scegliere **Connetti tutto**. Se alcune delle istanze del server vengono eseguite come un servizio con account diversi, fare clic sul pulsante **Connetti** singolo a destra di ogni nome di istanza del server.  
  
     Per altre informazioni, vedere la pagina [Connetti a repliche secondarie esistenti &#40;procedura guidata Aggiungi replica: procedura guidata Aggiungi database&#41;](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md).  
  
7.  Nella pagina **Convalida** viene verificato se i valori specificati in questa procedura guidata soddisfano i requisiti della Creazione guidata Gruppo di disponibilità. Per apportare una modifica, è possibile fare clic su **Precedente** per tornare a una pagina precedente della procedura guidata per modificare uno o più valori. Scegliere **Avanti** per tornare alla pagina **Convalida** , quindi fare clic su **Ripeti convalida**.  
  
     Per altre informazioni, vedere [Pagina Convalida &#40;procedure guidate Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/validation-page-always-on-availability-group-wizards.md).  
  
8.  Nella pagina **Riepilogo** rivedere le scelte effettuate per il nuovo gruppo di disponibilità. Per apportare una modifica, fare clic su **Indietro** per tornare alla pagina pertinente. Dopo avere apportato la modifica, fare clic su **Avanti** per tornare alla pagina **Riepilogo** .  
  
     Per altre informazioni, vedere [Pagina Riepilogo &#40;procedure guidate Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/summary-page-always-on-availability-group-wizards.md).  
  
     Se si è soddisfatti delle selezioni, è possibile fare clic su Script per creare uno script dei passaggi eseguiti dalla procedura guidata. Per creare e configurare il nuovo gruppo di disponibilità, fare quindi clic su **Fine**.  
  
9. Nella pagina **Stato** viene visualizzato lo stato di avanzamento dei passaggi per la creazione del gruppo di disponibilità, ovvero configurazione di endpoint, creazione del gruppo di disponibilità e creazione di un join della replica secondaria al gruppo.  
  
     Per altre informazioni, vedere [Pagina Stato installazione &#40;procedure guidate Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/progress-page-always-on-availability-group-wizards.md).  
  
10. Al termine di questi passaggi, nella pagina **Risultati** vengono visualizzati i relativi risultati. Se tutti questi passaggi sono stati eseguiti correttamente, il nuovo gruppo di disponibilità è completamente configurato. Se si verifica un errore durante uno dei passaggi, potrebbe essere necessario completare manualmente la configurazione. Per informazioni sulla causa di un determinato errore, fare clic sul collegamento "Errore" nella colonna **Risultato** .  
  
     Al termine della procedura guidata, fare clic su **Chiudi** per uscire.  
  
     Per altre informazioni, vedere [Pagina Risultati &#40;procedure guidate gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md).  
  
11. Se la sincronizzazione dati iniziale non è stata avviata automaticamente su tutti i database secondari, è necessario configurare eventuali database secondari non ancora aggiunti. Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Aggiungere un database a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)   
 [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)   
 [Aggiungere un database a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
  
