---
title: Pagina Seleziona sincronizzazione dati iniziale (procedura guidata gruppi di disponibilità)
description: Descrizione della pagina "Seleziona sincronizzazione dati iniziale" della creazione guidata gruppo di disponibilità Always On in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectinitialdatasync.f1
- sql13.swb.newagwizard.selectinitialdatasync.f1
- sql13.swb.addreplicawizard.selectinitialdatasync.f1
ms.assetid: 457b1140-4819-4def-8f7c-54a406e6db12
author: cawrites
ms.author: chadam
ms.openlocfilehash: 73ee631c0dc853a60e07f09aa2ade5ce58487a0e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171093"
---
# <a name="select-initial-data-synchronization-page-always-on-availability-group-wizards"></a>Pagina Seleziona sincronizzazione dati iniziale (procedure guidate gruppi di disponibilità Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Usare la pagina **Seleziona sincronizzazione dei dati iniziale** AlwaysOn per indicare le preferenze per la sincronizzazione dati iniziale dei nuovi database secondari. Questa pagina è condivisa da tre procedure guidate: [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)], [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]e [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)].  
  
 Le scelte possibili includono **Seeding automatico**, **Backup completo di database e log**, **Solo join**, o **Ignora sincronizzazione dati iniziale**. Prima di selezionare **Seeding automatico**, **Completo** o **Solo join**, verificare che l'ambiente soddisfi i prerequisiti.  
    
##  <a name="recommendations"></a><a name="Recommendations"></a> Raccomandazioni  
  
-   Sospendere le attività di backup dei log per i database primari durante la sincronizzazione dati iniziale.  
  
-   Per i database di grandi dimensioni, le operazioni di backup e ripristino completo possono risultare intense in termini di tempo e risorse. In tali casi, si consiglia di preparare i database secondari manualmente. Per ulteriori informazioni, vedere [Per preparare i database secondari manualmente](#PrepareSecondaryDbs)più avanti in questo argomento.  
  
-   La sincronizzazione dati iniziale completa richiede di specificare una condivisione di rete. Prima di utilizzare una procedura guidata per eseguire la sincronizzazione dati iniziale completa, si consiglia di implementare un piano di sicurezza per le autorizzazioni di accesso alla cartella della condivisione di rete. Si tratta di una precauzione importante perché chiunque disponga di un'autorizzazione READ alla cartella potrebbe accedere a dati potenzialmente sensibili nel file di backup. Inoltre, per proteggere le operazioni di backup e ripristino, si consiglia di proteggere i canali della rete tra ogni istanza del server che ospita una replica di disponibilità e la cartella della condivisione di rete.  
  
     Se è necessario proteggere le operazioni di backup e ripristino con una sicurezza elevata, si consiglia di selezionare l'opzione **Solo join** o **Ignora sincronizzazione dati iniziale** .  
  
## <a name="automatic-seeding"></a><a name="Auto"></a> Seeding automatico
 
 SQL Server crea automaticamente le repliche secondarie per ogni database nel gruppo. Il seeding automatico richiede che il percorso dei dati e del file di log sia lo stesso in ogni istanza di SQL Server inclusa nel gruppo. Disponibile in [!INCLUDE[sssql16-md.md](../../../includes/sssql16-md.md)] e versioni successive. Vedere [Inizializzare automaticamente un gruppo di disponibilità Always On](automatically-initialize-always-on-availability-group.md).

##  <a name="full-database-and-log-backup"></a><a name="Full"></a> Backup completo di database e log 
 Per ogni database primario, tramite l'opzione **Backup completo di database e log** vengono eseguite diverse operazioni in un flusso di lavoro: creare un backup completo e del log del database primario, creare i database secondari corrispondenti ripristinando i backup in ogni istanza del server in cui è ospitata una replica secondaria e creare un join di ogni database secondario al gruppo di disponibilità.  
  
 Selezionare questa opzione solo se l'ambiente soddisfa i prerequisiti seguenti per l'utilizzo della sincronizzazione dati iniziale completa e si desidera avviare automaticamente la sincronizzazione dati tramite la procedura guidata.  
  
 **Prerequisiti per l'utilizzo della sincronizzazione dati iniziale per backup completo di file e log**  
  
-   Tutti i percorsi di file dei database devono essere identici in ogni istanza del server in cui è ospitata una replica per il gruppo di disponibilità.  
  
    > [!NOTE]  
    >  Se i percorsi di backup e ripristino di file dell'istanza del server in cui viene eseguita la procedura guidata e quelli nelle istanze del server in cui deve essere ospitata una replica secondaria sono diversi, le operazioni di backup e ripristino devono essere eseguite manualmente mediante l'opzione WITH MOVE. Per ulteriori informazioni, vedere [Per preparare i database secondari manualmente](#PrepareSecondaryDbs)più avanti in questo argomento.  
  
-   In un'istanza del server in cui è ospitata una replica secondaria non può essere presente alcun nome di database primario. Ciò significa che non può essere ancora presente alcun nuovo database secondario.  
  
-   Sarà necessario specificare una condivisione di rete affinché la procedura guidata sia in grado di creare e accedere ai backup. Per la replica primaria, all'account usato per avviare il [!INCLUDE[ssDE](../../../includes/ssde-md.md)] devono essere associate le autorizzazioni del file system in lettura e scrittura per una condivisione di rete. Per le repliche secondarie, all'account deve essere associata l'autorizzazione di lettura per la condivisione di rete.  
  
    > [!IMPORTANT]  
    >  I backup del log faranno parte della catena di backup del log. Archiviare i file di backup del log in modo appropriato.  
  
 **Se i prerequisiti non vengono soddisfatti**  
  
 Non sarà possibile creare i database secondari per questo gruppo di disponibilità tramite la procedura guidata. Per informazioni su come prepararli, vedere [Per preparare i database secondari manualmente](#PrepareSecondaryDbs)più avanti in questo argomento.  
  
 **Se i prerequisiti vengono soddisfatti**  
  
 Se tutti questi prerequisiti vengono soddisfatti e si desidera eseguire la sincronizzazione dati iniziale completa tramite la procedura guidata, selezionare l'opzione **Backup completo di file e log** e specificare una condivisione di rete. In questo modo verrà eseguito il backup completo del database e il backup del log di ogni database selezionato e tali backup verranno inseriti nella condivisione di rete specificata. Quindi, in ogni istanza del server che ospita una delle nuove repliche secondarie, tramite la procedura guidata verranno creati i database secondari ripristinando i backup mediante RESTORE WITH NORECOVERY. Dopo la creazione di ogni database secondario, tramite la procedura guidata verrà creato un join del nuovo database secondario al gruppo di disponibilità. Subito dopo la creazione di un join di un database secondario, verranno avviate le sincronizzazioni dati in tale database.  
  
 **Specificare un percorso di rete condiviso accessibile da tutte le repliche**  
 Per creare e ripristinare i backup, nella procedura guidata viene richiesto di specificare una condivisione di rete. È necessario che l'account utilizzato per avviare il [!INCLUDE[ssDE](../../../includes/ssde-md.md)] in ogni istanza del server che ospiterà una replica di disponibilità disponga delle autorizzazioni del file system in lettura e scrittura sulla condivisione di rete.  
  
> [!IMPORTANT]  
>  I backup del log faranno parte della catena di backup del log. Archiviare i file di backup.  
  
##  <a name="join-only"></a><a name="Joinonly"></a> Solo join  
 Selezionare questa opzione solo se i nuovi database secondari esistono già in ogni istanza del server che ospita una replica secondaria per il gruppo di disponibilità. Per informazioni sulla preparazione dei database secondari, vedere [Per preparare i database secondari manualmente](#PrepareSecondaryDbs), più avanti in questa sezione.  
  
 Se si seleziona **Solo join**, tramite la procedura guidata si tenterà di creare un join di ogni database secondario esistente al gruppo di disponibilità.  
  
## <a name="skip-initial-data-synchronization"></a><a name="Skip"></a> Ignora sincronizzazione dati iniziale  
 Selezionare questa opzione se si desidera eseguire backup personalizzati del database e del log di ogni database primario e ripristinarli in ogni istanza del server che ospita una replica secondaria. Dopo la chiusura della procedura guidata, sarà quindi necessario creare un join di ogni database secondario in ogni replica secondaria.  
  
> [!NOTE]  
>  Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="to-prepare-secondary-databases-manually"></a><a name="PrepareSecondaryDbs"></a> Per preparare i database secondari manualmente  
 Per preparare i database secondari senza utilizzare alcuna procedura guidata [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , è possibile adottare uno degli approcci seguenti:  
  
-   Ripristinare manualmente un backup recente del database primario utilizzando RESTORE WITH NORECOVERY, quindi ripristinare ogni successivo backup del log utilizzando RESTORE WITH NORECOVERY. Se i percorsi di file del database primario e del database secondario sono diversi, è necessario utilizzare l'opzione WITH MOVE. Eseguire questa sequenza di ripristino in ogni istanza del server che ospita una replica secondaria per il gruppo di disponibilità.  È possibile utilizzare [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell per eseguire tali operazioni di backup e ripristino.  
  
     **Per altre informazioni:**  
  
     [Preparare manualmente un database secondario per un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   Se vengono aggiunti uno o più database primari per il log shipping a un gruppo di disponibilità, è possibile eseguire la migrazione di uno o più database secondari corrispondenti da log shipping a [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Per ulteriori informazioni, vedere [Prerequisiti per la migrazione dal log shipping ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md).  
  
    > [!NOTE]  
    >  Dopo aver creato tutti i database secondari per il gruppo di disponibilità, se si desidera eseguire backup nelle repliche secondarie, sarà necessario configurare nuovamente le preferenze di backup automatico del gruppo di disponibilità.  
  
     **Per altre informazioni:**  
  
     [Prerequisiti per la migrazione dal log shipping ai gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
     [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 Dopo avere creato un database secondario, applicare tutti i backup del log correnti al nuovo database secondario.  
  
 Facoltativamente, è possibile preparare tutti i database secondari prima di eseguire la procedura guidata, quindi nella pagina **Specificare la sincronizzazione dati iniziale** della procedura guidata selezionare **Solo join** per creare un join automatico dei nuovi database secondari al gruppo di disponibilità.  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> Attività correlate  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi database a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
-   [Usare la Procedura guidata Failover del gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)  
  
-   [Creare un join di un database secondario a un gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
