---
description: Gestire passaggi di processo
title: Gestire passaggi di processo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server replication]
- job steps [SQL Server Agent]
- jobs [SQL Server Agent], Integration Services package step
- executable programs as job steps
- operating systems [SQL Server], job steps
- Transact-SQL job step
- job steps [Transact-SQL]
- Integration Services packages, job steps
- replication job steps [SQL Server]
- logs [SQL Server], jobs
- SQL Server Agent jobs, job steps
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: 51352afc-a0a4-428b-8985-f9e58bb57c31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8892ff46f9c6c9fd382b39284cbc4bc81449a5d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355179"
---
# <a name="manage-job-steps"></a>Gestire passaggi di processo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte ma non tutte le funzionalità di SQL Server Agent. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Un passaggio di processo è un'operazione eseguita dal processo in un database o in un server. Ogni processo deve essere composto da almeno un passaggio. I passaggi di processo possono essere costituiti dagli elementi seguenti:  
  
-   Programmi eseguibili e comandi del sistema operativo.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni, incluse stored procedure e stored procedure estese.  
  
-   Script di PowerShell.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Script ActiveX.  
  
-   Attività di replica.  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] attività.  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pacchetti.  
  
Ogni passaggio di processo viene eseguito in un contesto di sicurezza specifico. Se tramite il passaggio di processo viene specificato un proxy, il passaggio viene eseguito nel contesto di sicurezza della credenziale per il proxy. Se tramite il passaggio di processo non viene specificato un proxy, il passaggio viene eseguito nel contesto dell'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Solo i membri del ruolo predefinito del server sysadmin possono creare processi in cui non venga specificato esplicitamente un proxy.  
  
Poiché i passaggi di processo vengono eseguiti nel contesto di un utente specifico di [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows, l'utente deve essere in possesso delle autorizzazioni e della configurazione necessarie per l'esecuzione del passaggio di processo. Se, ad esempio, si crea un processo che richiede una lettera di unità o un percorso UNC (Universal Naming Convention), i passaggi di processo possono essere eseguiti con l'account utente di Windows durante la verifica delle operazioni. L'utente di Windows per il passaggio di processo, tuttavia, deve inoltre disporre delle autorizzazioni richieste, delle configurazioni della lettera di unità o dell'accesso all'unità necessaria. In caso contrario, il passaggio di processo non verrà eseguito correttamente. Per evitare questo problema, assicurarsi che il proxy per ogni passaggio di processo disponga delle autorizzazioni necessarie per l'operazione eseguita dal passaggio. Per altre informazioni, vedere [Sicurezza e protezione (Motore di database)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).  
  
## <a name="job-step-logs"></a>Log dei passaggi di processo  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent può scrivere output da alcuni passaggi di processo in un file del sistema operativo o nella tabella sysjobstepslogs del database msdb. I tipi di passaggi di processo seguenti consentono la scrittura dell'output in entrambe le destinazioni:  
  
-   Programmi eseguibili e comandi del sistema operativo.  
  
-   Istruzioni[!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] attività.  
  
Solo i passaggi di processo eseguiti dagli utenti membri del ruolo predefinito del server sysadmin possono scrivere l'output dei passaggi di processo nei file del sistema operativo. Se i passaggi di processo vengono eseguiti da utenti membri dei ruoli del database predefiniti SQLAgentUserRole, SQLAgentReaderRole o SQLAgentOperatorRole nel database msdb, l'output di questi passaggi di processo può essere scritto solo nella tabella sysjobstepslogs.  
  
I log dei passaggi di processo vengono eliminati automaticamente quando vengono eliminati i processi o i passaggi di processo.  
  
> [!NOTE]  
> La registrazione delle attività di replica e dei passaggi di processo dei pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene gestita dal rispettivo sottosistema. Non è possibile utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per configurare la registrazione di questi tipi di passaggi di processo.  
  
## <a name="executable-programs-and-operating-system-commands-as-job-steps"></a>Utilizzo di programmi eseguibili e comandi del sistema operativo come passaggi di processo  
I programmi eseguibili e i comandi del sistema operativo possono essere utilizzati come passaggi di processo. Questi file possono avere estensione bat, cmd, com o exe.  
  
Quando si utilizza un programma eseguibile o un comando del sistema operativo come passaggio di processo, è necessario specificare gli elementi seguenti:  
  
-   Il codice di uscita del processo, restituito se il comando ha esito positivo.  
  
-   Comando da eseguire. Per eseguire un comando del sistema operativo, è necessario immettere solo il comando specifico. Per un programma esterno, è necessario immettere il nome del programma e gli argomenti del programma, ad esempio: **C:\Programmi\Microsoft SQL Server\100\Tools\Binn\sqlcmd.exe -e -q "sp_who"**  
  
    > [!NOTE]  
    > È necessario indicare il percorso completo del programma eseguibile se questo non è incluso in una directory specificata nel percorso di sistema o nel percorso per l'account utente con cui viene eseguito il passaggio di processo.  
  
## <a name="transact-sql-job-steps"></a>Passaggi di processo Transact-SQL  
Quando si crea un passaggio di processo [!INCLUDE[tsql](../../includes/tsql-md.md)] , è necessario eseguire le operazioni seguenti:  
  
-   Identificare il database in cui eseguire il processo.  
  
-   Digitare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] da eseguire. L'istruzione può chiamare una stored procedure o una stored procedure estesa.  
  
In alternativa, è possibile aprire un file [!INCLUDE[tsql](../../includes/tsql-md.md)] esistente come comando per il passaggio di processo.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] I passaggi di processo non usano proxy di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Il passaggio di processo viene invece eseguito come proprietario del passaggio oppure come account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent se il proprietario è un membro del ruolo predefinito del server sysadmin. I membri del ruolo predefinito del server sysadmin possono anche specificare che i passaggi di processo [!INCLUDE[tsql](../../includes/tsql-md.md)] vengano eseguiti nel contesto di un altro utente tramite il parametro *database_user_name* della stored procedure sp_add_jobstep. Per altre informazioni, vedere [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
> [!NOTE]  
> Un singolo passaggio di processo [!INCLUDE[tsql](../../includes/tsql-md.md)] può contenere più batch. [!INCLUDE[tsql](../../includes/tsql-md.md)] I passaggi di processo possono contenere comandi GO incorporati.  
  
## <a name="powershell-scripting-job-steps"></a>Utilizzo di script di PowerShell come passaggi di processo  
Quando si utilizza uno script di PowerShell per creare un passaggio di processo, come comando del passaggio è necessario specificare uno dei due elementi seguenti:  
  
-   Il testo di uno script di PowerShell.  
  
-   Un file script di PowerShell esistente da aprire.  
  
Il sottosistema PowerShell di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] apre una sessione di PowerShell e carica gli snap-in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Lo script di PowerShell usato come comando del passaggio di processo può fare riferimento al provider e ai cmdlet di PowerShell per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni sulla scrittura di script di PowerShell tramite gli snap-in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell, vedere [SQL Server PowerShell](../../powershell/sql-server-powershell.md).  
  
## <a name="activex-scripting-job-steps"></a>Utilizzo di Script ActiveX come passaggi di processo  
  
> [!IMPORTANT]
> Il passaggio di processo con script ActiveX verrà rimosso da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent in una versione futura di [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.  
  
Quando si crea un passaggio di processo con script ActiveX, è necessario eseguire le operazioni seguenti:  
  
-   Identificare il linguaggio di script con cui scrivere il passaggio di processo.  
  
-   Scrivere lo script ActiveX.  
  
È inoltre possibile aprire un file di script ActiveX esistente come comando per il passaggio di processo. In alternativa, i comandi degli script ActiveX possono essere compilati esternamente, ad esempio tramite Microsoft Visual Basic, e quindi eseguiti come file eseguibili.  
  
Quando un comando di un passaggio di processo è uno script ActiveX, è possibile utilizzare l'oggetto SQLActiveScriptHost per stampare l'output nella cronologia dei passaggi di processo o per creare oggetti COM. SQLActiveScriptHost è un oggetto globale introdotto dal sistema di hosting di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent nello spazio dei nomi dello script. L'oggetto dispone di due metodi (Stampa e CreateObject). Nell'esempio seguente viene illustrato il funzionamento degli script ActiveX in Visual Basic Scripting Edition (VBScript).  
  
```  
' VBScript example for ActiveX Scripting job step  
' Create a Dmo.Server object. The object connects to the  
' server on which the script is running.  
  
Set oServer = CreateObject("SQLDmo.SqlServer")  
oServer.LoginSecure = True  
oServer.Connect "(local)"  
'Disconnect and destroy the server object  
oServer.DisConnect  
Set oServer = nothing  
```  
  
## <a name="replication-job-steps"></a>Passaggi dei processi di replica  
Quando si creano pubblicazioni e sottoscrizioni tramite la replica, i processi di replica vengono creati per impostazione predefinita. Il tipo di processo creato è determinato dal tipo di replica (snapshot, transazionale o di tipo merge) e dalle opzioni utilizzate.  
  
I passaggi dei processi di replica attivano uno degli agenti di replica seguenti:  
  
-   Agente snapshot (processo Snapshot)  
  
-   Agente lettura log (processo LogReader)  
  
-   Agente di distribuzione (processo Distribuzione)  
  
-   Agente di merge (processo Merge)  
  
-   Agente di lettura coda (processo QueueReader)  
  
Durante la configurazione della replica, è possibile specificare di eseguire gli agenti di replica in tre modi diversi: in modo continuativo dopo l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, su richiesta o in base a una pianificazione. Per altre informazioni sugli agenti di replica, vedere [Panoramica degli agenti di replica](../../relational-databases/replication/agents/replication-agents-overview.md).  
  
## <a name="analysis-services-job-steps"></a>Passaggi di processo Analysis Services  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent supporta due tipi diversi di passaggi di processo Analysis Services, ovvero i passaggi di processo con comandi e i passaggi di processo con query.  
  
### <a name="analysis-services-command-job-steps"></a>Passaggi di processo con comandi di Analysis Services  
Quando si crea un passaggio di processo con un comando di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] , è necessario eseguire le operazioni seguenti:  
  
-   Identificare il server OLAP di database in cui eseguire il passaggio di processo.  
  
-   Digitare l'istruzione da eseguire. È necessario che l'istruzione sia un file XML per il metodo  **Execute** di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]. L'istruzione non può includere una busta SOAP completa o un file XML per il metodo  **Discover** di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]. A differenza di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , i passaggi di processo di **Agent non supportano le buste SOAP complete e il metodo** Discover [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="analysis-services-query-job-steps"></a>Passaggi di processo con query Analysis Services  
Quando si crea un passaggio di processo con una query di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] , è necessario eseguire le operazioni seguenti:  
  
-   Identificare il server OLAP di database in cui eseguire il passaggio di processo.  
  
-   Digitare l'istruzione da eseguire. L'istruzione deve essere una query MDX.  
  
Per altre informazioni su MDX, vedere [Elementi fondamentali dell'istruzione MDX (MDX)](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15).  
  
## <a name="integration-services-packages"></a>Pacchetti Integration Services  
Quando si crea un passaggio di processo con un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è necessario eseguire le operazioni seguenti:  
  
-   Identificare l'origine del pacchetto.  
  
-   Identificare la posizione del pacchetto.  
  
-   Se per il pacchetto sono necessari file di configurazione, identificare i file di configurazione.  
  
-   Se per il pacchetto sono necessari file di comando, identificare i file di comando.  
  
-   Identificare la verifica da utilizzare per il pacchetto. È possibile, ad esempio, specificare che il pacchetto deve essere firmato o deve disporre di un ID pacchetto specifico.  
  
-   Identificare le origini dei dati per il pacchetto.  
  
-   Identificare i provider di log per il pacchetto.  
  
-   Specificare le variabili e i valori da impostare prima di eseguire il pacchetto.  
  
-   Identificare le opzioni di esecuzione.  
  
-   Aggiungere o modificare le opzioni della riga di comando.  
  
Se il pacchetto è stato distribuito al catalogo SSIS e si specifica **Catalogo SSIS** come origine del pacchetto, molte di queste informazioni di configurazione vengono ottenute automaticamente dal pacchetto. Nella scheda **Configurazione** è possibile specificare l'ambiente, i valori dei parametri, i valori della gestione connessione, gli override delle proprietà e se il pacchetto è in esecuzione in un ambiente di runtime a 32 bit.  
  
Per altre informazioni sulla creazione di passaggi di processo eseguiti come pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vedere [Processi di SQL Server Agent per i pacchetti](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione|Argomento|  
|-|-|  
|Viene descritto come creare un passaggio di processo con un programma eseguibile.|[Creare un passaggio di processo CmdExec](../../ssms/agent/create-a-cmdexec-job-step.md)|  
|Viene descritto come reimpostare le autorizzazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|[Configurare un utente per la creazione e la gestione di processi di SQL Server Agent](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)|  
|Viene descritto come creare un passaggio di processo di [!INCLUDE[tsql](../../includes/tsql-md.md)] Agent.|[Create a Transact-SQL Job Step](../../ssms/agent/create-a-transact-sql-job-step.md)|  
|Viene descritto come definire le opzioni per i passaggi di processo Transact-SQL di Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|[Definire le opzioni del passaggio di processo Transact-SQL](../../ssms/agent/define-transact-sql-job-step-options.md)|  
|Viene descritto come creare un passaggio di processo dello script ActiveX.|[Create an ActiveX Script Job Step](../../ssms/agent/create-an-activex-script-job-step.md)|  
|Viene descritto come creare e definire passaggi di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent tramite cui vengono eseguiti comandi e query di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services.|[Create an Analysis Services Job Step](../../ssms/agent/create-an-analysis-services-job-step.md)|  
|Viene descritta quale azione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] occorre eseguire se si verifica un errore durante l'esecuzione del processo.|[Impostare il flusso di interventi in caso di esito positivo o negativo del passaggio di processo](../../ssms/agent/set-job-step-success-or-failure-flow.md)|  
|Viene descritto come visualizzare informazioni dettagliate sui passaggi di processo nella finestra di dialogo Proprietà passaggio processo.|[Visualizzare informazioni sui passaggi di processo](../../ssms/agent/view-job-step-information.md)|  
|Viene descritto come eliminare un log dei passaggi di processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.|[Delete a Job Step Log](../../ssms/agent/delete-a-job-step-log.md)|  
  
## <a name="see-also"></a>Vedere anche  
[sysjobstepslogs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
[Crea processi](../../ssms/agent/create-jobs.md)  
[sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
