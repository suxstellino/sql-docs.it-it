---
title: Configurare criteri flessibili per il failover automatico di un gruppo di disponibilità
description: Descrive come configurare criteri di failover flessibili per un gruppo di disponibilità Always On usando Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.date: 11/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 1ed564b4-9835-4245-ae35-9ba67419a4ce
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.custom: seo-lt-2019
ms.openlocfilehash: 487b155561944e1303aac0fdb7453fc2d494802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342457"
---
# <a name="configure-a-flexible-automatic-failover-policy-for-an-always-on-availability-group"></a>Configurare criteri flessibili per il failover automatico di un gruppo di disponibilità Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  In questo argomento verrà descritto come configurare i criteri di failover flessibili per un gruppo di disponibilità AlwaysOn tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell in SQL Server. Con i criteri di failover flessibili viene garantito un controllo granulare delle condizioni che causano un [failover automatico](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) per un gruppo di disponibilità. Modificando le condizioni di errore che attivano un failover automatico e la frequenza di controlli di integrità, è possibile aumentare o diminuire la probabilità di un failover automatico per supportare il Contratto di servizio per la disponibilità elevata.  

   I criteri di failover flessibili di un gruppo di disponibilità vengono definiti in base al relativo livello delle condizioni di errore e alla soglia di Timeout controllo integrità. Quando viene rilevato che un gruppo di disponibilità ha superato il livello di condizione di errore o la soglia di Timeout controllo integrità, la DLL risorse del gruppo di disponibilità risponde al cluster WSFC (Windows Server Failover Clustering). Tramite il cluster WSFC viene quindi iniziato un failover automatico alla replica secondaria.  
 
  > [!NOTE]  
  > Non è possibile configurare i criteri di failover flessibili di un gruppo di disponibilità tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 
## <a name="limitations-on-automatic-failovers"></a><a name="Limitations"></a> Limitazioni sui failover automatici  
  
-   Affinché si verifichi un failover automatico, la replica primaria corrente e una replica secondaria devono essere configurate per la modalità di disponibilità con commit sincrono e failover automatico e la replica secondaria deve essere sincronizzata con quella primaria.  
  
-   Il failover automatico è supportato solo con tre repliche.  
  
-   Se un gruppo di disponibilità supera la relativa soglia di errore WSFC, non verrà effettuato il tentativo di failover automatico per il gruppo di disponibilità da parte del cluster WSFC. Inoltre, il gruppo di risorse WSFC del gruppo di disponibilità rimarrà in uno stato di errore finché l'amministratore del cluster non porterà manualmente online il gruppo di risorse con errori o l'amministratore del database non eseguirà un failover manuale del gruppo di disponibilità. La *soglia di errore WSFC* è definita come il numero massimo di errori supportati per il gruppo di disponibilità durante un determinato periodo di tempo. Il periodo di tempo predefinito è sei ore e il valore predefinito per il numero massimo di errori durante questo periodo è *n*-1, dove *n* è il numero di nodi WSFC. Per modificare i valori soglia dell'errore per un determinato gruppo di disponibilità, utilizzare la console di Gestione cluster di failover WSFC.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria.  
   
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
  
|Attività|Autorizzazioni|  
|----------|-----------------|  
|Per configurare i criteri di failover flessibili per un nuovo gruppo di disponibilità|Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.|  
|Per modificare i criteri di un gruppo di disponibilità esistente|È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.|  

##  <a name="health-check-timeout-threshold"></a><a name="HCtimeout"></a> Soglia di Timeout controllo integrità  
 La DLL risorse di WSFC del gruppo di disponibilità esegue un *controllo di integrità* della replica primaria chiamando la stored procedure [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) sull'istanza di SQL Server che ospita la replica primaria. **sp_server_diagnostics** restituisce i risultati in un intervallo uguale a 1/3 della soglia di timeout controllo integrità per il gruppo di disponibilità. La soglia di timeout controllo integrità predefinita è 30 secondi, in base alla quale i risultati di **sp_server_diagnostics** vengono restituiti a intervalli di 10 secondi. Se la stored procedure **sp_server_diagnostics** risulta lenta o non restituisce informazioni, la DLL risorse attenderà per l'intero intervallo della soglia di timeout controllo integrità prima di determinare che la replica primaria non risponde. Se la replica primaria non risponde, viene avviato un failover automatico, se è attualmente supportato.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** non esegue controlli di integrità a livello di database.  
  
##  <a name="failure-condition-level"></a><a name="FClevel"></a> Livello delle condizioni di errore  
 La possibilità che i dati di diagnostica e le informazioni sull'integrità restituiti da **sp_server_diagnostics** assicurino un failover automatico dipende dal livello di condizione di errore del gruppo di disponibilità. Il *livello di condizione di errore* specifica quale condizione di errore attiverà un failover automatico. I livelli delle condizioni di errore sono 5 e vanno dal livello meno restrittivo (livello 1), al livello più restrittivo (livello 5). Un dato livello di condizione include tutti i livelli meno restrittivi. Il livello più restrittivo, il livello 5, include le quattro condizioni meno restrittive e così via.  
  
> [!IMPORTANT]  
>  I database danneggiati e quelli sospetti non vengono rilevati da alcun livello della condizione di errore. Pertanto, di un database danneggiato o sospetto, per un errore hardware, un danneggiamento dati o per altro problema, non viene mai attivato un failover automatico.  
  
 Nella tabella seguente vengono descritte le condizioni di errore corrispondenti a ciascun livello.  
  
|Level|Condizione di errore|[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valore|Valore PowerShell|  
|-----------|-----------------------|------------------------------|----------------------|  
|Uno|In caso di server inaccessibile. Specifica che viene avviato un failover automatico quando si verifica una delle condizioni seguenti:<br /><br /> Il servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non è attivo.<br /><br /> Il lease del gruppo di disponibilità per la connessione al cluster WSFC scade poiché non viene ricevuto alcun acknowledgement dall'istanza del server. Per altre informazioni, vedere [Funzionamento: timeout lease di SQL Server Always On](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).<br /><br /> <br /><br /> Si tratta del livello meno restrittivo.|1|**OnServerDown**|  
|Due|In caso di mancata risposta del server. Specifica che viene avviato un failover automatico quando si verifica una delle condizioni seguenti:<br /><br /> L'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non si connette al cluster e viene superata la soglia Timeout controllo integrità specificata dall'utente per il gruppo di disponibilità.<br /><br /> La replica di disponibilità si trova in uno stato di errore.|2|**OnServerUnresponsive**|  
|Tre|In caso di errori critici del server. Specifica che viene avviato un failover automatico in caso di errori interni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] critici, ad esempio spinlock orfani, gravi violazioni dell'accesso in scrittura o dumping eccessivo.<br /><br /> Si tratta del livello predefinito.|3|**OnCriticalServerError**|  
|Quattro|In caso di errori con gravità moderata del server. Specifica che viene avviato un failover automatico in caso di errori interni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con gravità moderata, ad esempio una condizione persistente di memoria insufficiente nel pool di risorse interno di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|4|**OnModerateServerError**|  
|Cinque|In qualsiasi condizione di errore qualificata. Specifica che viene avviato un failover automatico in caso di qualsiasi condizione di errore qualificata, tra cui:<br /><br /> Rilevamento di un deadlock nell'utilità di pianificazione.<br /><br /> Rilevamento di un deadlock irrisolvibile.<br /><br /> <br /><br /> Si tratta del livello più restrittivo.|5|**OnAnyQualifiedFailureConditions**|  
  
> [!NOTE]  
>  La mancata risposta da parte di un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] alle richieste client non è rilevante per gruppi di disponibilità.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per configurare i criteri di failover flessibili**  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Per un nuovo gruppo di disponibilità, usare l'istruzione [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Se si modifica un gruppo di disponibilità esistente, usare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Per impostare il livello di condizione del failover, usare l'opzione FAILURE_CONDITION_LEVEL = *n* dove *n* è un numero intero da 1 a 5.  
  
         Ad esempio, l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente modifica il livello di condizione di errore di un gruppo di disponibilità esistente, `AG1`, nel livello uno:  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);  
        ```  
  
         La relazione di questi valori interi con i livelli di condizione di errore è la seguente:  
  
        |[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valore|Level|Il failover automatico viene avviato...|  
        |------------------------------|-----------|-------------------------------------------|  
        |1|Uno|In caso di server inaccessibile. Il servizio SQL Server viene arrestato a causa di un failover o un riavvio.|  
        |2|Due|In caso di mancata risposta del server. Viene soddisfatta qualsiasi condizione di valore inferiore, il servizio SQL Server è connesso al cluster e viene superata la soglia di Timeout controllo integrità o la replica primaria corrente si trova in uno stato di errore.|  
        |3|Tre|In caso di errori critici del server. Viene soddisfatta qualsiasi condizione di valore inferiore o si verifica un errore critico interno del server.<br /><br /> Si tratta del livello predefinito.|  
        |4|Quattro|In caso di errori con gravità moderata del server. Viene soddisfatta qualsiasi condizione di valore inferiore o si verifica un errore non critico del server.|  
        |5|Cinque|In qualsiasi condizione di errore qualificata. Viene soddisfatta qualsiasi condizione di valore inferiore o si verifica una condizione di errore appropriata.|  
  
         Per altre informazioni sui livelli di condizione del failover, vedere [Criteri di failover flessibili per failover automatico di un gruppo di disponibilità &#40;SQL Server&#41;]().  
  
    -   Per configurare la soglia di Timeout controllo integrità, usare l'opzione HEALTH_CHECK_TIMEOUT = *n* dove *n* è un numero intero compreso tra 15000 millisecondi (15 secondi) e 4294967295 millisecondi. Il valore predefinito è 30000 millisecondi (30 secondi)  
  
         Ad esempio, l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguente modificata la soglia di Timeout controllo integrità di un gruppo di disponibilità esistente, `AG1`, in 60.000 millisecondi (un minuto).  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT = 60000);  
        ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Con PowerShell  
 **Per configurare i criteri di failover flessibili**  
  
1.  Impostare il valore predefinito (**cd**) sull'istanza del server che ospita la replica primaria.  
  
2.  Quando si aggiunge una replica di disponibilità a un gruppo di disponibilità, usare il cmdlet **New-SqlAvailabilityGroup** . Quando si modifica una replica di disponibilità esistente, usare il cmdlet **Set-SqlAvailabilityGroup** .  
  
    -   Per impostare il livello delle condizioni di failover, usare il parametro **FailureConditionLevel**_level_ , dove *level* è uno dei valori seguenti:  
  
        |valore|Level|Il failover automatico viene avviato...|  
        |-----------|-----------|-------------------------------------------|  
        |**OnServerDown**|Uno|In caso di server inaccessibile. Il servizio SQL Server viene arrestato a causa di un failover o un riavvio.|  
        |**OnServerUnresponsive**|Due|In caso di mancata risposta del server. Viene soddisfatta qualsiasi condizione di valore inferiore, il servizio SQL Server è connesso al cluster e viene superata la soglia di Timeout controllo integrità o la replica primaria corrente si trova in uno stato di errore.|  
        |**OnCriticalServerError**|Tre|In caso di errori critici del server. Viene soddisfatta qualsiasi condizione di valore inferiore o si verifica un errore critico interno del server.<br /><br /> Si tratta del livello predefinito.|  
        |**OnModerateServerError**|Quattro|In caso di errori con gravità moderata del server. Viene soddisfatta qualsiasi condizione di valore inferiore o si verifica un errore non critico del server.|  
        |**OnAnyQualifiedFailureConditions**|Cinque|In qualsiasi condizione di errore qualificata. Viene soddisfatta qualsiasi condizione di valore inferiore o si verifica una condizione di errore appropriata.|  
  
         Per altre informazioni sui livelli di condizione del failover, vedere [Criteri di failover flessibili per failover automatico di un gruppo di disponibilità &#40;SQL Server&#41;]().  
  
         Ad esempio, il comando seguente modifica il livello di condizione di errore di un gruppo di disponibilità esistente, `AG1`, nel livello uno:  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `   
        -FailureConditionLevel OnServerDown  
        ```  
  
    -   Per impostare la soglia di Timeout controllo integrità, usare il parametro **HealthCheckTimeout**_n_ , dove *n* è un intero compreso tra 15000 millisecondi (15 secondi) e 4294967295 millisecondi. Il valore predefinito è 30000 millisecondi (30 secondi).  
  
         Ad esempio, il comando seguente modifica la soglia di Timeout controllo integrità di un gruppo di disponibilità esistente, `AG1`, in 120.000 millisecondi (due minuti).  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
        -HealthCheckTimeout 120000  
        ```  
  
> [!NOTE]  
>  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Per impostare e utilizzare il provider PowerShell per SQL Server**  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Visualizzare la Guida di SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  

##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **To configure automatic failover**  
  
-   [Modificare la modalità di disponibilità di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md). Il failover automatico richiede la modalità di disponibilità con commit sincrono.  
  
-   [Modificare la modalità di failover di una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurare i criteri di failover flessibili per controllare le condizioni per il failover automatico &#40;Gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenuto correlato  
  
-   [Funzionamento: timeout lease di SQL Server Always On](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modalità di disponibilità &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Windows Server Failover Clustering &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Criteri di failover per istanze del cluster di failover](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
