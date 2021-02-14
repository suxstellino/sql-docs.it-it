---
title: Configurare backup in repliche secondarie per un gruppo di disponibilità
description: Descrive come configurare backup in repliche secondarie per un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 74bc40bb-9f57-44e4-8988-1d69c0585eb6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7258eff52e3614b30eeb001a5f570bafd4c7f5cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348988"
---
# <a name="configure-backups-on-secondary-replicas-of-an-always-on-availability-group"></a>Configurare backup in repliche secondarie per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Questo argomento descrive come configurare il backup in repliche secondarie per un gruppo di disponibilità Always On usando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Per altre informazioni sul backup in repliche secondarie, vedere [Repliche secondarie attive: Backup in repliche secondarie &#40;gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
 È necessario essere connessi all'istanza del server che ospita la replica primaria.  
 
   > [!NOTE]
   > Non è necessario che la replica secondaria sia leggibile per l'offload dei backup. I backup avranno comunque esito positivo nella replica secondaria anche se `Readable Secondary` è impostato su `no`. 
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
  
|Attività|Autorizzazioni|  
|----------|-----------------|  
|Per configurare il backup delle repliche secondarie in caso di creazione di un gruppo di disponibilità|Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.|  
|Per modificare un gruppo di disponibilità o una replica di disponibilità|È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 **Per configurare il backup delle repliche secondarie**  
  
1.  In Esplora oggetti connettersi all'istanza del server in cui viene ospitata la replica primaria e fare clic sul nome del server per espandere il relativo albero.  
  
2.  Espandere il nodo **Disponibilità elevata AlwaysOn** e il nodo **Gruppi di disponibilità**.  
  
3.  Fare clic sul gruppo di disponibilità di cui si desidera configurare le preferenze di backup e scegliere il comando **Proprietà** .  
  
4.  Nella finestra di dialogo **Proprietà gruppo di disponibilità** selezionare **Preferenze di backup** .  
  
5.  Nel pannello **Specificare la destinazione dei backup** selezionare la preferenza di backup automatico per il gruppo di disponibilità, scegliendo una delle seguenti opzioni:  
  
     **Preferisco secondario**  
     Specifica che i backup devono essere eseguiti su una replica secondaria tranne quando la replica primaria è l'unica replica online. In tal caso il backup deve essere eseguito sulla replica primaria. Questa è l'opzione predefinita.  
  
     **Solo secondaria**  
     Specifica che i backup non devono mai essere eseguiti sulla replica primaria. Se la replica primaria è l'unica replica online, il backup non viene eseguito.  
  
     **Server/istanza primaria**  
     Specifica che i backup devono essere sempre eseguiti sulla replica primaria. Questa opzione è utile se sono necessarie funzionalità di backup, ad esempio la creazione di backup differenziali, che non sono supportate quando il backup viene eseguito su una replica secondaria.  
  
    > [!IMPORTANT]  
    >  Se si intende usare il log shipping per preparare un qualsiasi database secondario per un gruppo di disponibilità, impostare la preferenza di backup automatico su **Primario** finché tutti i database secondari non saranno stati preparati e uniti in join al gruppo di disponibilità.  
  
     **Qualsiasi replica**  
     Specifica che si preferisce che i processi di backup ignorino il ruolo delle repliche di disponibilità nella scelta della replica per l'esecuzione dei backup. Si noti che i processi di backup potrebbero valutare altri fattori, ad esempio la priorità di backup di ogni replica di disponibilità in combinazione con lo stato operativo e lo stato connesso.  
  
    > [!IMPORTANT]  
    >  Non è prevista l'applicazione dell'impostazione relativa alle preferenze di backup automatico. L'interpretazione di questa preferenza dipende dall'eventuale logica su cui si basano gli script dei processi di backup per i database in un determinato gruppo di disponibilità. L'impostazione relativa alle preferenze di backup automatico non incide sui backup ad hoc. Per ulteriori informazioni, vedere [Completamento: Dopo avere configurato il backup su repliche secondarie](#FollowUp) più avanti in questo argomento.  
  
6.  Utilizzare la griglia **Priorità di backup replica** per modificare la priorità di backup delle repliche di disponibilità. In questa griglia è indicata la priorità di backup corrente di ogni istanza del server che ospita una replica per il gruppo di disponibilità. Le colonne della griglia sono le seguenti:  
  
     **Istanza del server**  
     Nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ospita la replica di disponibilità.  
  
     **Priorità di backup (Più bassa=1, Più alta=100)**  
     Specifica la priorità di esecuzione dei backup nella replica rispetto alle altre repliche nello stesso gruppo di disponibilità. Il valore è un numero intero compreso nell'intervallo 0-100. 1 indica la priorità più bassa e 100 indica la priorità più alta. Se **Priorità di backup** = 1, la replica di disponibilità viene scelta per l'esecuzione dei backup solo se non sono disponibili repliche di disponibilità con priorità più alta.  
  
     **Escludi replica**  
     Selezionare questa opzione se si desidera che questa replica di disponibilità non venga mai scelta per l'esecuzione dei backup. Ciò si rivela utile, ad esempio, per una replica di disponibilità remota in cui non si desidera eseguire mai il failover dei backup.  
  
7.  Fare clic su **OK** per eseguire il commit delle modifiche.  
  
 **Modalità alternative da accedere alla pagina delle Preferenze di backup**  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usare la procedura guidata Aggiungi replica a gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utilizzare la finestra di dialogo Nuovo gruppo di disponibilità &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per configurare il backup delle repliche secondarie**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Per un nuovo gruppo di disponibilità, usare l'istruzione [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md). Se si modifica un gruppo di disponibilità esistente, usare l'istruzione [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per configurare il backup delle repliche secondarie**  
  
1.  Impostare il valore predefinito (**cd**) sull'istanza del server che ospita la replica primaria.  
  
2.  Facoltativamente, configurare la priorità di backup di ogni replica di disponibilità aggiunta o modificata. Tale priorità viene utilizzata dall'istanza del server che ospita la replica primaria per stabilire quale replica deve soddisfare una richiesta di backup automatico in un database nel gruppo di disponibilità (viene scelta la replica con la priorità più alta). La priorità può essere impostata su qualsiasi numero compreso tra 0 e 100 inclusi. 0 indica che la replica non deve essere considerata per soddisfare le richieste di backup.  L'impostazione predefinita è 50.  
  
     Quando si aggiunge una replica di disponibilità a un gruppo di disponibilità, usare il cmdlet **New-SqlAvailabilityReplica** . Quando si modifica una replica di disponibilità esistente, usare il cmdlet **Set-SqlAvailabilityReplica** . In entrambi i casi, specificare il parametro **BackupPriority**_n_ , dove *n* è un valore compreso tra 0 e 100.  
  
     Ad esempio, nel comando seguente viene impostata la priorità di backup della replica di disponibilità `MyReplica` su **60**.  
  
    ```  
    Set-SqlAvailabilityReplica -BackupPriority 60 `  
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
3.  Configurare facoltativamente la preferenza di backup automatico per il gruppo di disponibilità in fase di creazione o modifica. Questa preferenza indica la modalità di valutazione della replica primaria da parte di un processo di backup nella scelta della posizione in cui eseguire i backup. L'impostazione predefinita è quella di preferire le repliche secondarie.  
  
     Quando si crea un gruppo di disponibilità, usare il cmdlet **New-SqlAvailabilityGroup** . Quando si modifica un gruppo di disponibilità esistente, usare il cmdlet **Set-SqlAvailabilityGroup** . In entrambi i casi, specificare il parametro **AutomatedBackupPreference** .  
  
     dove  
  
     **Server/istanza primaria**  
     Specifica che i backup devono essere sempre eseguiti sulla replica primaria. Questa opzione è utile se sono necessarie funzionalità di backup, ad esempio la creazione di backup differenziali, che non sono supportate quando il backup viene eseguito su una replica secondaria.  
  
    > [!IMPORTANT]  
    >  Se si intende usare il log shipping per preparare un qualsiasi database secondario per un gruppo di disponibilità, impostare la preferenza di backup automatico su **Primario** finché tutti i database secondari non saranno stati preparati e uniti in join al gruppo di disponibilità.  
  
     **SecondaryOnly**  
     Specifica che i backup non devono mai essere eseguiti sulla replica primaria. Se la replica primaria è l'unica replica online, il backup non viene eseguito.  
  
     **Server/istanza secondaria**  
     Specifica che i backup devono essere eseguiti su una replica secondaria tranne quando la replica primaria è l'unica replica online. In tal caso il backup deve essere eseguito sulla replica primaria. Questo è il comportamento predefinito.  
  
     **Nessuno**  
     Specifica che si preferisce che i processi di backup ignorino il ruolo delle repliche di disponibilità nella scelta della replica per l'esecuzione dei backup. Si noti che i processi di backup potrebbero valutare altri fattori, ad esempio la priorità di backup di ogni replica di disponibilità in combinazione con lo stato operativo e lo stato connesso.  
  
    > [!IMPORTANT]  
    >  Non è prevista l'applicazione di **AutomatedBackupPreference**. L'interpretazione di questa preferenza dipende dall'eventuale logica su cui si basano gli script dei processi di backup per i database in un determinato gruppo di disponibilità. L'impostazione relativa alle preferenze di backup automatico non incide sui backup ad hoc. Per ulteriori informazioni, vedere [Completamento: Dopo avere configurato il backup su repliche secondarie](#FollowUp) più avanti in questo argomento.  
  
     Ad esempio, il comando seguente imposta la proprietà **AutomatedBackupPreference** del gruppo di disponibilità `MyAg` su **SecondaryOnly**. I backup automatici dei database in questo gruppo di disponibilità non verranno mai eseguiti nella replica primaria, ma verranno reindirizzati alla replica secondaria con l'impostazione della priorità di backup più elevata.  
  
    ```  
    Set-SqlAvailabilityGroup `  
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `  
    -AutomatedBackupPreference SecondaryOnly  
    ```  
  
> [!NOTE]  
>  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Visualizzare la Guida di SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="follow-up-after-configuring-backup-on-secondary-replicas"></a><a name="FollowUp"></a> Completamento: Dopo avere configurato il backup su repliche secondarie  
 Per prendere in considerazione le preferenze di backup automatico per un gruppo di disponibilità specifico, in ogni istanza del server in cui è ospitata una replica di disponibilità la cui priorità di backup è maggiore di zero (>0) è necessario generare script dei processi di backup per i database nel gruppo di disponibilità. Per determinare se la replica corrente è la replica di backup preferita, usare la funzione [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) nello script di backup. Se la replica di disponibilità ospitata dall'istanza del server corrente è la replica preferita per i backup, questa funzione restituisce 1. In caso contrario, la funzione restituisce 0. Eseguendo uno script semplice in ogni replica di disponibilità tramite cui viene eseguita una query su questa funzione, è possibile determinare in quale replica deve essere eseguito un processo di backup specificato. Di seguito è riportato un esempio di un tipico frammento di script per un processo di backup:  
  
```sql  
IF (NOT sys.fn_hadr_backup_is_preferred_replica(@DBNAME))  
BEGIN  
      Select 'This is not the preferred replica, exiting with success';  
      RETURN 0 -- This is a normal, expected condition, so the script returns success  
END  
BACKUP DATABASE @DBNAME TO DISK=<disk>  
   WITH COPY_ONLY;  
```  
  
 Generando uno script per un processo di backup con questo tipo di logica è possibile pianificare il processo affinché venga eseguito in ogni replica di disponibilità presente nella stessa pianificazione. Ognuno di questi processi analizza gli stessi dati per determinare il processo da eseguire, pertanto solo uno dei processi pianificati procede effettivamente alla fase di backup.  In caso di failover, nessuno degli script o processi deve essere modificato. Inoltre, se si riconfigura un gruppo di disponibilità per aggiungere una replica di disponibilità, la gestione del processo di backup richiede la copia o la pianificazione del processo di backup. Se si rimuove una replica di disponibilità, eliminare semplicemente il processo di backup dall'istanza del server che la ospitava.  
  
> [!TIP]  
>  Se si usa la [Creazione guidata piano di manutenzione](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)per creare un processo di backup specifico, il processo includerà automaticamente la logica di scripting che chiama e controlla la funzione **sys.fn_hadr_backup_is_preferred_replica** . Tuttavia, il processo di backup non restituirà il messaggio che indica che non si tratta della replica preferita. Assicurarsi di creare i processi per ogni database di disponibilità in ogni istanza del server che ospita una replica di disponibilità per il gruppo di disponibilità.  
  
##  <a name="to-obtain-information-about-backup-preference-settings"></a><a name="ForInfoAboutBuPref"></a> Per ottenere informazioni sulle impostazioni delle preferenze di backup  
 Gli elementi seguenti sono utili per ottenere informazioni pertinenti per il backup di una replica secondaria.  
  
|Visualizza|Informazioni|Colonne pertinenti|  
|----------|-----------------|----------------------|  
|[sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)|Indica se la replica corrente è la replica di backup preferita|Non applicabile.|  
|[sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)|preferenza di backup automatico|**automated_backup_preference**<br /><br /> **automated_backup_preference_desc**|  
|[sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)|Priorità di backup di una determinata replica di disponibilità|**backup_priority**|  
|[sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)|Indica se la replica è locale all'istanza del server<br /><br /> Ruolo corrente<br /><br /> Stato operativo<br /><br /> Stato Connesso<br /><br /> Integrità della sincronizzazione di una replica di disponibilità|**is_local**<br /><br /> **role**, **role_desc**<br /><br /> **operational_state**, **operational_state_desc**<br /><br /> **connected_state**, **connected_state_desc**<br /><br /> **synchronization_health**, **synchronization_health_desc**|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guida alle soluzioni AlwaysOn di Microsoft SQL Server per la disponibilità elevata e il ripristino di emergenza)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server AlwaysOn Team Blog: blog ufficiale del team di SQL Server AlwaysOn](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Repliche secondarie attive: Backup in repliche secondarie &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
