---
title: Failover di un gruppo di disponibilità
description: Descrive come eseguire il failover manuale pianificato/forzato di un gruppo di disponibilità Always On con SQL Server Management Studio, Transact-SQL o SQL PowerShell.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.failoverwizard.connecttoreplicas.f1
- sql13.swb.failoverwizard.progress.f1
- sql13.swb.failoverwizard.selectnewprimary.f1
- sql13.swb.failoverwizard.f1
- sql13.swb.failoverwizard.confirmdataloss.f1
helpviewer_keywords:
- failover [SQL Server], failover
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], configuring
ms.assetid: 4a602584-63e4-4322-aafc-5d715b82b834
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6e0d0975bf9663f630ca7494d173ff852a3f7b4e
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765551"
---
# <a name="use-the-fail-over-availability-group-wizard-sql-server-management-studio"></a>Utilizzare la Procedura guidata Failover del gruppo di disponibilità (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come eseguire un failover manuale pianificato o un failover manuale forzato (failover forzato) su un gruppo di disponibilità AlwaysOn tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Per un gruppo di disponibilità il failover si verifica al livello di una replica di disponibilità. Se si esegue il failover su a una replica secondaria con stato SYNCHRONIZED, tramite la procedura guidata viene eseguito un failover manuale pianificato (senza perdita di dati). Se si esegue il failover su una replica secondaria con stato UNSYNCHRONIZED o NOT SYNCHRONIZING, la procedura guidata esegue un failover manuale forzato, noto anche come *failover forzato* (con possibile perdita di dati). In entrambe le forme di failover manuale la replica secondaria a cui si è connessi assume il ruolo primario. Con un failover manuale pianificato attualmente comporta il passaggio della replica primaria precedente al ruolo secondario. Dopo un failover forzato, quando la replica primaria precedente torna online assume il ruolo secondario.  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
 Prima del primo failover manuale pianificato, vedere la sezione "Prima di iniziare" in [Eseguire un failover manuale pianificato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
 Prima del primo failover forzato, vedere le sezioni "Prima di iniziare" e "Completamento: Attività essenziali dopo un failover forzato" in [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Un comando del failover viene restituito non appena la replica secondaria di destinazione ha accettato il comando. Tuttavia, il recupero del database si verifica in modo asincrono dopo che il gruppo di disponibilità ha completato il failover.  
    
###  <a name="prerequisites-for-using-the-failover-availability-group-wizard"></a><a name="Prerequisites"></a> Prerequisiti relativi all'utilizzo della Creazione guidata Gruppo di disponibilità di failover  
  
-   È necessario essere connessi all'istanza del server che ospita una replica di disponibilità attualmente disponibile.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per utilizzare la Creazione guidata Gruppo di disponibilità di failover**  
  
1.  In Esplora oggetti connettersi all'istanza del server in cui viene ospitata una replica secondaria del gruppo di disponibilità di cui è necessario eseguire il failover ed espandere l'albero di server.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Per avviare la procedura guidata di failover del gruppo di disponibilità, fare clic con il pulsante destro del mouse sul gruppo di disponibilità di cui eseguire il failover e selezionare **Failover**.  
  
4.  Le informazioni presentate nella pagina **Introduzione** variano a seconda che una replica secondaria sia idonea per un failover pianificato. Se in questa pagina è indicato "**Eseguire un failover pianificato per il gruppo di disponibilità**", è possibile eseguire failover del gruppo di disponibilità senza perdita di dati.  
  
5.  Nella pagina **Seleziona nuova replica primaria** è possibile visualizzare lo stato della replica primaria corrente e del quorum WSFC, prima di scegliere la replica secondaria che diventerà la nuova replica primaria (la *destinazione di failover*). Per un failover manuale pianificato assicurarsi di selezionare una replica secondaria il cui valore **Conformità failover** sia "**Senza perdita di dati**". Per un failover forzato e per tutte le possibili destinazioni di failover, questo valore sarà "**Perdita di dati, Avvisi(** _#_ **)** ", dove *#* indica il numero di avvisi presenti per una replica secondaria specificata. Per visualizzare gli avvisi per una destinazione del failover specificata, fare clic sul valore "Conformità failover".  
  
     Per ulteriori informazioni, vedere [Pagina Seleziona nuova replica primaria](#SelectNewPrimaryReplica)più avanti in questo argomento.  
  
6.  Nella pagina **Connetti alla replica** , connettersi alla destinazione del failover. Per ulteriori informazioni, vedere [Pagina Connetti alla replica](#ConnectToReplica)più avanti in questo argomento.  
  
7.  Se si esegue un failover forzato, nella procedura guidata viene visualizzata la pagina **Conferma potenziale perdita di dati** . Per procedere con il failover, è necessario selezionare **Fare clic qui per confermare il failover con potenziale perdita di dati**. Per ulteriori informazioni, vedere[Pagina Conferma potenziale perdita di dati](#ConfirmPotentialDataLoss)più avanti in questo argomento.  
  
8.  Nella pagina **Riepilogo** rivedere le implicazioni del failover sulla replica secondaria selezionata.  
  
     Se si è soddisfatti delle selezioni, è possibile fare clic su **Script** per creare uno script dei passaggi eseguiti nel corso della procedura guidata. Per eseguire quindi il failover del gruppo di disponibilità sulla replica secondaria selezionata, fare clic su **Fine**.  
  
9. Nella pagina **Stato** viene visualizzato lo stato di avanzamento del failover sul gruppo di disponibilità.  
  
10. Al termine dell'operazione di failover, nella pagina **Risultati** verrà visualizzato il risultato. Al termine della procedura guidata, fare clic su **Chiudi** per uscire.  
  
     Per altre informazioni, vedere [Pagina Risultati &#40;procedure guidate gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md).  
  
11. Dopo un failover forzato, vedere la sezione "Completamento: dopo un failover forzato" in [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
## <a name="help-for-pages-that-are-exclusive-to-this-wizard"></a>Guida relativa alle pagine specifiche di questa procedura guidata  
 In questa sezione vengono descritte le pagine univoche della [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)].  
  
 **Argomenti della sezione**  
  
-   [Pagina Seleziona nuova replica primaria](#SelectNewPrimaryReplica)  
  
-   [Pagina Connetti alla replica](#ConnectToReplica)  
  
-   [Pagina Conferma potenziale perdita di dati](#ConfirmPotentialDataLoss)  
  
 Le altre pagine di questa procedura guidata condividono la Guida con una o più procedure guidate relative ai gruppi di disponibilità AlwaysOn e sono documentate in argomenti della Guida sensibile al contesto distinti.  
  
###  <a name="select-new-primary-replica-page"></a><a name="SelectNewPrimaryReplica"></a> Select New Primary Replica Page  
 In questa sezione vengono descritte le opzioni della pagina **Seleziona nuova replica primaria** . Utilizzare questa pagina per selezionare la replica secondaria (destinazione del failover) sulla quale verrà eseguito il failover del gruppo di disponibilità. La replica diventerà la nuova replica primaria.  
  
#### <a name="page-options"></a>Opzioni della pagina  
 **Replica primaria corrente**  
 Visualizza il nome della replica primaria corrente, se è online.  
  
 **Stato replica primaria**  
 Visualizza lo stato della replica primaria corrente, se è online.  
  
 **Stato quorum**  
 Per i tipi di cluster WSFC, visualizza lo stato del quorum per la replica di disponibilità, tra i seguenti:  
  
   |valore|Descrizione|  
   |-----------|-----------------|  
   |**Quorum normale**|Il cluster è stato avvito con un quorum normale.|  
   |**Quorum forzato**|Il cluster è stato avvito con un quorum forzato.|  
   |**Stato quorum sconosciuto**|Lo stato del quorum del cluster non è disponibile.|  
   |**Non applicabile**|Il nodo che ospita la replica di disponibilità non dispone di quorum.|  
  
 Per altre informazioni, vedere [Modalità quorum WSFC e configurazione del voto &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  

 Per il tipo di cluster NONE, non si applica lo stato del quorum.

 Per il tipo di cluster EXTERNAL, lo stato del quorum è gestito dalla gestine del cluster e non è visibile a SQL Server.
  
 **Scegliere una nuova replica primaria**  
 Utilizzare questa griglia per selezionare una replica secondaria che diventerà la nuova replica primaria. Le colonne nella griglia sono le seguenti:  
  
 **Istanza del server**  
 Visualizza il nome di un'istanza del server che ospita una replica secondaria.  
  
 **Modalità di disponibilità**  
 Visualizza la modalità di disponibilità dell'istanza del server, tra le seguenti:  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Commit sincrono**|Nella modalità commit sincrono, prima di eseguire il commit delle transazioni, una replica primaria con commit sincrono attende l'acknowledgement della finalizzazione del log da parte della replica secondaria con commit sincrono. Nella modalità commit sincrono si può essere sicuri che al termine della sincronizzazione di un determinato database secondario con il database primario, le transazioni di cui è stato eseguito il commit sono completamente protette.|  
|**Commit asincrono**|Nella modalità commit asincrono, la replica primaria esegue il commit delle transazioni senza attendere l'acknowledgement della finalizzazione del log da parte di una replica con commit asincrono. La modalità commit asincrono riduce la latenza delle transazioni sui database secondari, ma consente un certo ritardo rispetto ai database primari, rendendo possibile la perdita di dati.|  
  
 Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 **Modalità di failover**  
 Visualizza la modalità di failover dell'istanza del server, tra le seguenti:  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Automatico**|Una replica secondaria configurata per il failover automatico supporta inoltre il failover manuale pianificato quando la replica secondaria viene sincronizzata con la replica primaria.|  
|**Manuale**|Sono disponibili due tipi di failover manuale: pianificato (con perdita di dati) e forzato (con possibile perdita di dati). Per una determinata replica secondaria è supportato solo uno di questi tipi, a seconda della modalità di disponibilità e, per la modalità commit sincrono, dello stato di sincronizzazione della replica secondaria. Per determinare quale forma di failover manuale è supportata attualmente da una replica secondaria specificata, vedere la colonna **Conformità failover** di questa griglia.|  
  
 Per altre informazioni, vedere [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Conformità failover**  
 Visualizza la conformità failover della replica secondaria, tra le seguenti:  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Senza perdita di dati**|Questa replica secondaria attualmente supporta il failover pianificato. Questo valore è presente solo quando una replica secondaria in modalità commit sincrono viene attualmente sincronizzata con la replica primaria.|  
|**Perdita di dati, Avvisi(** _#_ **)**|Questa replica secondaria supporta attualmente il failover forzato (con possibile perdita di dati). Questo valore è presente quando la replica secondaria non viene sincronizzata con la replica primaria. Per informazioni sulla potenziale perdita di dati, fare clic sul collegamento relativo agli avvisi per la perdita di dati.|  
  
 **Aggiorna**  
 Fare clic per aggiornare la griglia.  
  
 **Annulla**  
 Fare clic per annullare la procedura guidata. Nella pagina **Seleziona nuova replica primaria** l'annullamento della procedura guidata ne provoca la chiusura senza eseguire alcuna azione.  
  
###  <a name="confirm-potential-data-loss-page"></a><a name="ConfirmPotentialDataLoss"></a> Confirm Potential Data Loss Page  
 In questa sezione vengono descritte le opzioni della pagina **Conferma potenziale perdita di dati** visualizzata solo se si esegue un failover forzato. Questo argomento viene utilizzato solo nella [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]. Utilizzare questa pagina per indicare se si è disposti per a rischiare una possibile perdita di dati per forzare il failover del gruppo di disponibilità.  
  
#### <a name="confirm-potential-data-loss-options"></a>Opzioni di Conferma potenziale perdita di dati  
 Se la replica secondaria selezionata non è sincronizzata con la replica primaria, nella procedura guidata verrà visualizzato un avviso per indicare che il failover su questa replica secondaria potrebbe provocare una perdita di dati in uno o più database.  
  
 **Fare clic qui per confermare il failover con potenziale perdita di dati.**  
 Se si è disposti a rischiare una perdita di dati per rendere disponibili agli utenti i database in questo gruppo di disponibilità, selezionare questa casella di controllo. Se non si è disposti a rischiare una perdita di dati, è possibile fare clic su **Precedente** per tornare alla pagina **Seleziona nuova replica primaria** o su **Annulla** per uscire dalla procedura guidata senza eseguire il failover sul gruppo di disponibilità.  
  
 **Annulla**  
 Fare clic per annullare la procedura guidata. Nella pagina **Conferma potenziale perdita di dati** l'annullamento della procedura guidata ne provoca la chiusura senza eseguire alcuna azione.  
  
###  <a name="connect-to-replica-page"></a><a name="ConnectToReplica"></a> Connect to Replica Page  
 In questa sezione vengono descritte le opzioni della pagina **Connetti alla replica** della [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]. Questa pagina viene visualizzata solo se non si è connessi alla replica secondaria di destinazione. Utilizzare questa pagina per connettersi alla replica secondaria selezionata come nuova replica primaria.  
  
#### <a name="page-options"></a>Opzioni della pagina  
 **Colonne della griglia:**  
 **Istanza del server**  
 Consente di visualizzare il nome dell'istanza del server che ospiterà la replica di disponibilità.  
  
 **Connesso come**  
 Visualizza l'account connesso all'istanza del server, dopo che è stata stabilita la connessione. Se in questa colonna viene visualizzato "**Non connesso**" per un'istanza del server specificata, sarà necessario fare clic sul pulsante **Connetti** .  
  
 **Connettere**  
 Fare clic su questo pulsante se l'istanza del server viene eseguita con un account diverso rispetto ad altre istanze del server a cui è necessario connettersi.  
  
 **Annulla**  
 Fare clic per annullare la procedura guidata. Nella pagina **Connetti alla replica** l'annullamento della procedura guidata ne provoca la chiusura senza eseguire alcuna azione.  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Eseguire un failover manuale pianificato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)   
 [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)   
 [Ripristino di emergenza WSFC tramite quorum forzato &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
