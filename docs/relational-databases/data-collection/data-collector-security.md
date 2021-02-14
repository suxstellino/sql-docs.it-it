---
description: Sicurezza agente di raccolta dati
title: Sicurezza dell'agente di raccolta dati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
- security [data collector]
- data collector [SQL Server], security
ms.assetid: e75d6975-641e-440a-a642-cb39a583359a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a61b6d819fd014183c68485b1b9a8f0a7c362598
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343679"
---
# <a name="data-collector-security"></a>Sicurezza agente di raccolta dati
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'agente di raccolta dati usano il modello di sicurezza basato sui ruoli implementato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Questo modello consente all'amministratore del database di eseguire le varie attività dell'agente di raccolta dati in un contesto di sicurezza che ha solo le autorizzazioni necessarie per eseguire quell'attività. Questo approccio è usato anche per operazioni con tabelle interne, a cui è possibile accedere solo mediante una stored procedure o una vista. Per le tabelle interne non vengono concesse autorizzazioni. Le autorizzazioni vengono invece controllate sull'utente della stored procedure o della vista usata per accedere a una tabella.  
  
> [!IMPORTANT]  
>  Un altro aspetto chiave di questo modello di sicurezza è costituito dalle autorizzazioni concentriche. Nelle autorizzazioni concentriche i ruoli con privilegi di livello più alto ereditano le autorizzazioni dei ruoli con privilegi di livello più basso su oggetti (compresi avvisi, operatori, processi, pianificazioni e proxy). Per altre informazioni, vedere [SQL Server Agent Fixed Database Roles](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Nelle sezioni seguenti vengono descritte la sicurezza della raccolta di dati in generale, nonché i ruoli che è necessario concedere agli utenti affinché possano configurare e usare l'agente di raccolta dati ed eseguire attività associate al data warehouse di gestione.  
  
## <a name="general-security"></a>Sicurezza generale  
 L'agente di raccolta dati viene installato secondo gli standard documentati specificati per [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
### <a name="network-security"></a>Sicurezza di rete  
 Le informazioni riservate possono essere passate tra le istanze di destinazione, l'istanza relazionale associata al server di configurazione, i set di raccolta in esecuzione ed il server che ospita il data warehouse di gestione.  
  
 Per proteggere i dati trasferiti su una rete vengono implementati i meccanismi di sicurezza standard, ad esempio la crittografia del protocollo per [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions-for-configuring-and-using-the-data-collector"></a>Autorizzazioni per la configurazione e l'utilizzo dell'agente di raccolta dati  
 A seconda dell'attività, gli utenti devono essere membri di uno o più ruoli predefiniti del database forniti per l'agente di raccolta dati. I ruoli sono i seguenti, elencati a partire dall'accesso con privilegi di livello più alto fino a quello con privilegi minimi:  
  
-   **dc_admin**  
  
-   **dc_operator**  
  
-   **dc_proxy**  
  
 Questi ruoli sono archiviati nel database msdb. Per impostazione predefinita, nessun utente è membro di questi ruoli di database. L'appartenenza dell'utente a questi ruoli deve essere concessa esplicitamente.  
  
 Gli utenti membri del ruolo predefinito del server **sysadmin** hanno accesso completo agli oggetti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e alle viste dell'agente di raccolta dati. Tuttavia è necessario che vengano aggiunti esplicitamente ai ruoli dell'agente di raccolta dati.  
  
> [!IMPORTANT]  
>  I membri dei ruoli db_ssisadmin e dc_admin potrebbero essere in grado di elevare i loro privilegi a sysadmin. Questa elevazione dei privilegi può verificarsi perché tali ruoli possono modificare i pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e i pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] possono essere eseguiti da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando il contesto di sicurezza sysadmin di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Per impedire questa elevazione dei privilegi durante l'esecuzione dei piani di manutenzione, set di raccolta dati e altri pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurare i processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che eseguono pacchetti in modo da utilizzare un account proxy con privilegi limitati o aggiungere solo i membri sysadmin ai ruoli db_ssisadmin e dc_admin.  
  
### <a name="dc_admin-role"></a>Ruolo dc_admin  
 Gli utenti assegnati al ruolo **dc_admin** hanno accesso completo di amministratore (creazione, lettura, aggiornamento ed eliminazione) alla configurazione dell'agente di raccolta dati su un'istanza del server. I membri di questo ruolo possono eseguire le seguenti operazioni:  
  
-   Impostare proprietà a livello di agente di raccolta  
  
-   Aggiungere nuovi set di raccolta  
  
-   Installare nuovi tipi di raccolta  
  
-   Eseguire tutte le operazioni consentite al ruolo **dc_operator** .  
  
 Il ruolo **dc_admin** è un membro dei seguenti ruoli:  
  
-   **SQLAgentUserRole**. Questo ruolo è necessario per creare pianificazioni ed eseguire processi.  
  
    > [!NOTE]  
    >  I proxy creati per l'agente di raccolta dati devono concedere l'accesso a **dc_admin** affinché possano essere creati e usati in ogni passaggio di processo che richiede un proxy.  
  
-   **dc_operator**. I membri di **dc_admin** ereditano le autorizzazioni concesse a **dc_operator**.  
  
### <a name="dc_operator-role"></a>Ruolo dc_operator  
 I membri del ruolo **dc_operator** hanno accesso in lettura e aggiornamento. Tale ruolo supporta le attività di operazioni relative all'esecuzione e alla configurazione dei set di raccolta. I membri di questo ruolo possono eseguire le seguenti operazioni:  
  
-   Avviare o arrestare un set di raccolta.  
  
-   Enumerare set di raccolta esistenti.  
  
-   Visualizzare le informazioni dettagliate (ad esempio elementi della raccolta e frequenza di raccolta) associate ad un set di raccolta.  
  
-   Modificare la frequenza di caricamento per i set di raccolta esistenti.  
  
-   Modificare la frequenza di raccolta per gli elementi della raccolta appartenenti a un set di raccolta esistente.  
  
 Il ruolo **dc_operator** è un membro dei seguenti ruoli richiesti per l'enumerazione e la visualizzazione dei pacchetti dell'agente di raccolta dati:  
  
-   **db_ssisltduser**  
  
-   **db_ssisoperator**  
  
 Per altre informazioni, vedere [Ruoli Integration Services &#40;servizio SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
### <a name="dc_proxy-role"></a>Ruolo dc_proxy  
 I membri del ruolo **dc_proxy** hanno accesso in lettura ai set di raccolta dell'agente di raccolta dati e alle proprietà a livello di agente di raccolta. I membri di questo ruolo possono inoltre eseguire processi di cui sono proprietari e creare passaggi di processo eseguibili come un account proxy esistente.  
  
 I membri di questo ruolo possono eseguire le seguenti operazioni:  
  
-   Visualizzare informazioni di configurazione del set di raccolta (ad esempio parametri di input per elementi della raccolta e la frequenza di raccolta per questi elementi).  
  
-   Ottenere informazioni crittografate interne a cui è possibile accedere soltanto mediante una stored procedure firmata, ad esempio informazioni di connessione al data warehouse usate per il caricamento dei dati.  
  
-   Registrare eventi di runtime del set di raccolta.  
  
 Il ruolo **dc_proxy** è un membro dei seguenti ruoli richiesti per l'enumerazione e la visualizzazione dei pacchetti dell'agente di raccolta dati:  
  
-   **db_ssisltduser**.  
  
-   **db_ssisoperator**  
  
 Per altre informazioni, vedere [Ruoli Integration Services &#40;servizio SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
## <a name="permissions-for-configuring-and-using-the-management-data-warehouse"></a>Autorizzazioni per la configurazione e l'utilizzo del data warehouse di gestione  
 A seconda dell'attività, gli utenti devono essere membri di uno o più ruoli predefiniti del database forniti per accedere al data warehouse di gestione. I ruoli sono i seguenti, elencati a partire dall'accesso con privilegi di livello più alto fino a quello con privilegi minimi:  
  
-   **mdw_admin**  
  
-   **mdw_writer**  
  
-   **mdw_reader**  
  
 Questi ruoli sono archiviati nel database msdb. Per impostazione predefinita, nessun utente è membro di questi ruoli di database. L'appartenenza dell'utente a questi ruoli deve essere concessa esplicitamente.  
  
 Gli utenti membri del ruolo predefinito del server **sysadmin** hanno accesso completo alle viste dell'agente di raccolta dati. Tuttavia è necessario che vengano aggiunti esplicitamente ai ruoli del database per eseguire altre operazioni.  
  
### <a name="mdw_admin-role"></a>Ruolo mdw_admin  
 I membri del ruolo **mdw_admin** hanno accesso in lettura, scrittura, aggiornamento ed eliminazione al data warehouse di gestione.  
  
 I membri di questo ruolo possono eseguire le seguenti operazioni:  
  
-   Modificare lo schema del data warehouse di gestione se necessario (ad esempio per aggiungere una nuova tabella quando viene installato un nuovo tipo di raccolta).  
  
    > [!NOTE]  
    >  In caso di modifica dello schema l'utente deve essere anche membro del ruolo **dc_admin** per installare un nuovo tipo di agente di raccolta, in quanto tale azione richiede un'autorizzazione per aggiornare la configurazione dell'agente di raccolta dati in msdb.  
  
-   Eseguire processi di manutenzione sul data warehouse di gestione, ad esempio archiviazione o pulizia.  
  
### <a name="mdw_writer-role"></a>Ruolo mdw_writer  
 I membri del ruolo **mdw_writer** possono caricare e scrivere dati nel data warehouse di gestione. Gli agenti di raccolta dati che archiviano dati nel data warehouse di gestione devono essere membri di questo ruolo.  
  
### <a name="mdw_reader-role"></a>Ruolo mdw_reader  
 I membri del ruolo **mdw_reader** hanno accesso in lettura al data warehouse di gestione. Poiché lo scopo di questo ruolo è supportare la risoluzione dei problemi fornendo accesso ai dati della cronologia, i membri di questo ruolo non possono visualizzare gli altri elementi dello schema del data warehouse di gestione.  
  
## <a name="see-also"></a>Vedere anche  
 [Implementazione della sicurezza di SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md)  
  
  
