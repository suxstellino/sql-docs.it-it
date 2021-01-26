---
description: Utilizzo dei profili agenti di replica
title: Usare i profili agenti di replica | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], agents and profiles
- replication agent profiles [SQL Server]
- agents [SQL Server replication], profiles
- profiles [SQL Server], replication agents
ms.assetid: 9c290a88-4e9f-4a7e-aab5-4442137a9918
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 613d3be8703563967241dda54725eda4c777a712
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765591"
---
# <a name="work-with-replication-agent-profiles"></a>Utilizzo dei profili agenti di replica
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come utilizzare i profili degli agenti di replica in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects). Il comportamento di ogni agente di replica è controllato da un set di parametri che è possibile impostare tramite i profili agenti. Ogni agente dispone di un profilo predefinito e alcuni anche di profili predefiniti aggiuntivi. In un momento specifico, per un agente è attivo un solo profilo.  
  
 **Contenuto dell'articolo**  
  
-   **Per utilizzare i profili degli agenti di replica, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
    -   Accedere alla finestra di dialogo Profili agenti  
  
    -   Specificare un profilo per un agente  
  
    -   Creare un profilo  
  
    -   Modificare un profilo  
  
    -   Eliminare un profilo  
  
     [Transact-SQL](#TsqlProcedure)  
  
    -   Creare un profilo  
  
    -   Modificare un profilo  
  
    -   Eliminare un profilo  
  
    -   Utilizzare i profili agenti durante la sincronizzazione  
  
    -   Esempio Transact-SQL  
  
     [oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
    -   Creare un profilo  
  
    -   Modificare un profilo  
  
    -   Eliminare un profilo  
  
-   **Completamento:**  [dopo avere modificato i parametri degli agenti](#FollowUp)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
###  <a name="to-access-the-agent-profiles-dialog-box-from-sql-server-management-studio"></a><a name="Access_SSMS"></a> Per accedere alla finestra di dialogo Profili agenti da SQL Server Management Studio  
  
1.  Nella pagina **Generale** della finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** fare clic su **Impostazioni predefinite profili**.  

#### <a name="to-access-the-agent-profiles-dialog-box-from-replication-monitor"></a>Per accedere alla finestra di dialogo Profili agenti da Monitoraggio replica  
  
-   Per aprire la finestra di dialogo per tutti gli agenti, fare clic con il pulsante destro del mouse su un server di pubblicazione e quindi scegliere **Profili agenti**.  
  
-   Per aprire la finestra di dialogo per un singolo agente:  
  
    1.  Espandere un gruppo di server di pubblicazione nel riquadro a sinistra di Monitoraggio replica, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
    2.  Per i profili dell'agente di distribuzione e dell'agente di merge, fare clic con il pulsante destro del mouse su una sottoscrizione nella scheda **Tutte le sottoscrizioni** e quindi scegliere **Profilo agente**. Per gli altri agenti, fare clic con il pulsante destro del mouse sull'agente nella scheda **Agenti** , quindi scegliere **Profilo agente**.  
  
###  <a name="to-specify-a-profile-for-an-agent"></a><a name="Specify_SSMS"></a> Per specificare un profilo per un agente  
  
1.  Se nella finestra di dialogo **Profili agenti** vengono visualizzati i profili di più agenti, selezionare un agente.  
  
2.  Selezionare un profilo nella colonna **Predefinito per i nuovi agenti** della griglia **Profili agenti** . Per impostazione predefinita, il profilo viene applicato solo agli agenti per le nuove pubblicazioni e sottoscrizioni.  
  
3.  Per specificare che tutti gli agenti del tipo selezionato per le pubblicazioni o le sottoscrizioni esistenti devono utilizzare questo profilo, fare clic su **Modifica agenti esistenti**.  
  
###  <a name="to-view-and-edit-the-parameters-associated-with-a-profile"></a><a name="Modify_SSMS"></a> Per visualizzare e modificare i parametri associati a un profilo  
  
1.  Se nella finestra di dialogo **Profili agenti** vengono visualizzati i profili di più agenti, selezionare un agente.  
  
2.  Fare clic sul pulsante delle proprietà ( **...** ) accanto a un profilo.  
  
3.  Visualizzare i parametri e i valori nella finestra di dialogo **Proprietà profilo \<ProfileName>** .  
  
    -   È possibile modificare i parametri nei profili definiti dall'utente, ma non quelli nei profili di sistema predefiniti.  
  
    -   Per visualizzare tutti i parametri per un agente, deselezionare la casella di controllo **Mostra solo i parametri utilizzati in questo profilo** . Per informazioni sui parametri degli agenti, vedere i collegamenti alla fine di questo argomento.  
  
4.  Fare clic su **Close**.  
  
###  <a name="to-create-a-user-defined-profile"></a><a name="Create_SSMS"></a> Per creare un profilo definito dall'utente  
  
1.  Se nella finestra di dialogo **Profili agenti** vengono visualizzati i profili di più agenti, selezionare un agente.  
  
2.  Fare clic su **Nuovo**.  
  
3.  Nella finestra di dialogo di inizializzazione **Nuovo profilo agente** selezionare un profilo esistente su cui basare il nuovo profilo.  
  
4.  Nella finestra di dialogo **Nuovo profilo agente** immettere i valori nelle caselle di testo **Nome** e **Descrizione** .  
  
5.  Modificare i parametri per personalizzare il profilo. Per visualizzare tutti i parametri per un agente, deselezionare la casella di controllo **Mostra solo i parametri utilizzati in questo profilo** . Per informazioni sui parametri degli agenti, vedere i collegamenti alla fine di questo argomento.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="to-delete-a-user-defined-profile"></a><a name="Delete_SSMS"></a> Per eliminare un profilo definito dall'utente  
  
1.  Se nella finestra di dialogo **Profili agenti** vengono visualizzati i profili di più agenti, selezionare un agente.  
  
2.  Se un profilo è associato a uno o più agenti, modificare il profilo per tali agenti:  
  
    1.  Selezionare un altro profilo nella griglia **Profili agenti** .  
  
    2.  Fare clic su **Modifica agenti esistenti**.  
  
        > [!NOTE]  
        >  Verrà modificato il profilo di tutti gli agenti del tipo selezionato per le pubblicazioni o le sottoscrizioni esistenti e non solo di quelli che utilizzano il profilo che si desidera eliminare.  
  
3.  Selezionare il profilo che si desidera eliminare, quindi fare clic su **Elimina**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_tsql"></a> Per creare un nuovo profilo agente  
  
1.  Nel database di distribuzione eseguire [sp_add_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md). Specificare **\@name**, il valore **1** per **\@profile_type** e uno dei valori seguenti per **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Se questo profilo diventerà il nuovo profilo predefinito per il tipo di agente di replica, specificare il valore **1** per **\@default**. L'identificatore per il nuovo profilo viene restituito mediante il parametro di output **\@profile_id**. Viene creato un nuovo profilo con un set di parametri del profilo basato sul profilo predefinito per il tipo di agente specificato.  
  
2.  Una volta creato il nuovo profilo, è possibile personalizzarlo aggiungendo, rimuovendo o modificando i parametri predefiniti.  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_tsql"></a> Per modificare un profilo agente esistente  
  
1.  Nel database di distribuzione eseguire [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Specificare uno dei valori seguenti per **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Vengono restituiti tutti i profili per il tipo di agente specificato. Tenere presente il valore di **profile_id** nel set di risultati del profilo da modificare.  
  
2.  Nel database di distribuzione eseguire [sp_help_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md). Specificare l'identificatore del profilo ottenuto nel passaggio 1 per **\@profile_id**. Vengono restituiti tutti i parametri per il profilo. Tenere presente il nome dei parametri del profilo da modificare o rimuovere.  
  
3.  Per modificare il valore di un parametro in un profilo, eseguire [sp_change_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md). Specificare l'identificatore del profilo ottenuto nel passaggio 1 per **\@profile_id**, il nome del parametro da modificare per **\@parameter_name** e un nuovo valore del parametro per **\@parameter_value**.  
  
    > [!NOTE]  
    >  Non è possibile modificare un profilo agente esistente in modo da impostarlo come profilo predefinito di un agente. A tale scopo, è necessario creare un nuovo profilo come profilo predefinito, come indicato nella procedura indicata in precedenza.  
  
4.  Per rimuovere un parametro da un profilo, eseguire [sp_drop_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md). Specificare l'identificatore del profilo ottenuto nel passaggio 1 per **\@profile_id** e il nome del parametro da rimuovere per **\@parameter_name**.  
  
5.  Per aggiungere un nuovo parametro a un profilo è necessario effettuare le seguenti operazioni:  
  
    -   Eseguire una query sulla tabella [MSagentparameterlist &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msagentparameterlist-transact-sql.md) nel database di distribuzione per determinare i parametri del profilo che è possibile impostare per ogni tipo di agente.  
  
    -   Nel database di distribuzione eseguire [sp_add_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md). Specificare l'identificatore del profilo ottenuto nel passaggio 1 per **\@profile_id**, il nome di un parametro valido da aggiungere per **\@parameter_name** e il valore del parametro per **\@parameter_value**.  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_tsql"></a> Per eliminare un profilo agente  
  
1.  Nel database di distribuzione eseguire [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Specificare uno dei valori seguenti per **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Vengono restituiti tutti i profili per il tipo di agente specificato. Tenere presente il valore di **profile_id** nel set di risultati del profilo da rimuovere.  
  
2.  Nel database di distribuzione eseguire [sp_drop_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md). Specificare l'identificatore del profilo ottenuto nel passaggio 1 per **\@profile_id**.  
  
###  <a name="to-use-agent-profiles-during-synchronization"></a><a name="Synch_tsql"></a> Per utilizzare i profili agenti durante la sincronizzazione  
  
1.  Nel database di distribuzione eseguire [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Specificare uno dei valori seguenti per **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Vengono restituiti tutti i profili per il tipo di agente specificato. Tenere presente il valore di **profile_name** nel set di risultati del profilo da utilizzare.  
  
2.  Se l'agente è avviato da un processo dell'agente, modificare il passaggio del processo che avvia l'agente per specificare il valore di **profile_name** ottenuto nel passaggio 1 dopo il parametro della riga di comando **-ProfileName** . Per altre informazioni, vedere [Visualizzare e modificare i parametri del prompt dei comandi dell'agente di replica &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md).  
  
3.  Quando si avvia l'agente dal prompt dei comandi, specificare il valore di **profile_name** ottenuto nel passaggio 1 dopo il parametro della riga di comando **-ProfileName** .  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio viene creato un profilo personalizzato per l'agente di merge denominato **custom_merge**, viene modificato il valore del parametro **-UploadReadChangesPerBatch** , viene aggiunto un nuovo parametro **-ExchangeType** e vengono restituite informazioni sul profilo creato.  
  
 [!code-sql[HowTo#sp_addagentprofileparam](../../../relational-databases/replication/codesnippet/tsql/work-with-replication-ag_1.sql)]  
  
##  <a name="using-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_RMO"></a> Per creare un nuovo profilo agente  
  
1.  Creare una connessione al database di distribuzione tramite un'istanza della classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.AgentProfile>.  
  
3.  Impostare le proprietà indicate di seguito nell'oggetto.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> : nome del profilo.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AgentType%2A> : valore di <xref:Microsoft.SqlServer.Replication.AgentType> che specifica il tipo di agente di replica per il quale viene creato il profilo.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> : oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creato nel passaggio 1.  
  
    -   (Facoltativo) <xref:Microsoft.SqlServer.Replication.AgentProfile.Description%2A> : descrizione del profilo.  
  
    -   (Facoltativo) <xref:Microsoft.SqlServer.Replication.AgentProfile.Default%2A> : impostare questa proprietà su **true** se tutti i processi del nuovo agente per <xref:Microsoft.SqlServer.Replication.AgentType> utilizzeranno questo profilo per impostazione predefinita.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.Create%2A> per creare il profilo nel server.  
  
5.  Una volta reso disponibile il profilo nel server, è possibile personalizzarlo aggiungendo, rimuovendo o modificando i valori dei parametri dell'agente di replica.  
  
6.  Per assegnare il profilo a un processo dell'agente di replica esistente, chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.AssignToAgent%2A> . Passare il nome del database di distribuzione per *distributionDBName* e l'ID del processo per *agentID*.  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_RMO"></a> Per modificare un profilo agente esistente  
  
1.  Creare una connessione al database di distribuzione tramite un'istanza della classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passare l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creato nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se il metodo restituisce **false**, verificare che il server di distribuzione esista.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumAgentProfiles%2A> . Passare un valore di <xref:Microsoft.SqlServer.Replication.AgentType> per limitare i profili restituiti a un tipo specifico di agente di replica.  
  
5.  Ottenere l'oggetto <xref:Microsoft.SqlServer.Replication.AgentProfile> desiderato dall'oggetto <xref:System.Collections.ArrayList>restituito, dove la proprietà <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> dell'oggetto corrisponde al nome del profilo.  
  
6.  Chiamare uno dei metodi di <xref:Microsoft.SqlServer.Replication.AgentProfile> indicati di seguito per modificare il profilo.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AddParameter%2A> : aggiunge un parametro supportato al profilo, dove *name* è il nome del parametro dell'agente di replica e *value* è il valore specificato. Per enumerare tutti i parametri dell'agente supportati per un tipo di agente specificato, chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> . Questo metodo restituisce una classe <xref:System.Collections.ArrayList> di oggetti <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> che rappresentano tutti i parametri supportati.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.RemoveParameter%2A> : rimuove un parametro esistente dal profilo, dove *name* è il nome del parametro dell'agente di replica. Per enumerare tutti i parametri dell'agente corrente definiti per il profilo, chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> . Questo metodo restituisce una classe <xref:System.Collections.ArrayList> di oggetti <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> che rappresentano il parametro esistente per il profilo.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.ChangeParameter%2A> : modifica l'impostazione di un parametro esistente nel profilo, dove *name* è il nome del parametro dell'agente e *newValue* è il valore nel quale viene modificato il parametro. Per enumerare tutti i parametri dell'agente corrente definiti per il profilo, chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> . Questo metodo restituisce una classe <xref:System.Collections.ArrayList> di oggetti <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> che rappresentano il parametro esistente per il profilo. Per enumerare tutte le impostazioni del parametro dell'agente supportate, chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> . Questo metodo restituisce una classe <xref:System.Collections.ArrayList> di oggetti <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> che rappresentano i valori supportati per tutti i parametri.  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_RMO"></a> Per eliminare un profilo agente  
  
1.  Creare una connessione al database di distribuzione tramite un'istanza della classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.AgentProfile>. Impostare il nome del profilo per la proprietà <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> e l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creato nel passaggio 1 per la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se il metodo restituisce **false**, il nome specificato non è corretto o il profilo non esiste nel server.  
  
4.  Verificare che la proprietà <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A> sia impostata su <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.User>, in modo da indicare un profilo del cliente. Non è necessario rimuovere un profilo con valore <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.System> per <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A>.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.AgentProfile.Remove%2A> per rimuovere dal server il profilo definito dall'utente rappresentato da questo oggetto.  
  
##  <a name="follow-up-after-changing-agent-parameters"></a><a name="FollowUp"></a> Completamento: dopo avere modificato i parametri degli agenti  
Le modifiche apportate al parametro dell'agente verranno applicate al successivo avvio dell'agente. Se l'agente viene eseguito in modo continuo, è necessario arrestarlo e riavviarlo. A partire da SQL Server 2017 CU3, alcune modifiche dei parametri degli agenti hanno effetto senza che sia necessario riavviare gli agenti. 
  
## <a name="see-also"></a>Vedere anche  
 [Profili degli agenti di replica](../../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)   
 [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agente di lettura coda repliche](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
  
