---
description: SQL Server Agent
title: SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, about SQL Server Agent
- automatic administration steps
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/03/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 751a627530b7d1017bfd8f0f89aa626ea84bc35c
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186538"
---
# <a name="sql-server-agent"></a>SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

SQL Server Agent è un servizio di Microsoft Windows che esegue attività amministrative pianificate, denominate *processi* in SQL Server.

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>Vantaggi di SQL Server Agent

SQL Server Agent utilizza SQL Server per archiviare le informazioni sul processo. I processi sono costituiti da uno o più passaggi, ciascuno dei quali contiene un'attività, ad esempio il backup di un database.

SQL Server Agent possibile eseguire un processo in base a una pianificazione, in risposta a un evento specifico o su richiesta. Se, ad esempio, l'esigenza è quella di eseguire il backup di tutti i server aziendali ogni sera in orario non lavorativo, è possibile automatizzare questa attività, Pianificare l'esecuzione del backup dopo il 22:00 dal lunedì al venerdì. Se il backup rileva un problema, SQL Server Agent possibile registrare l'evento e ricevere una notifica.

> [!NOTE]
> Per impostazione predefinita, il servizio SQL Server Agent viene disabilitato quando viene installato SQL Server, a meno che l'utente non scelga esplicitamente di avviare automaticamente il servizio.

## <a name="sql-server-agent-components"></a><a name="Components"></a>Componenti di SQL Server Agent

SQL Server Agent utilizza i componenti seguenti per definire le attività da eseguire, quando eseguire le attività e come segnalare l'esito positivo o negativo delle attività.

### <a name="jobs"></a>Processi

Un *processo* è una serie specificata di azioni che SQL Server Agent esegue. Usare i processi per definire un'attività amministrativa eseguibile una o più volte e monitorabile per verificarne l'esito positivo o negativo. Un processo può essere eseguito in un server locale o in più server remoti.

> [!IMPORTANT]
> SQL Server Agent processi in esecuzione al momento di un evento di failover su un'istanza del cluster di failover SQL Server non riprendono dopo il failover in un altro nodo del cluster di failover. SQL Server Agent processi in esecuzione nel momento in cui un nodo Hyper-V viene sospeso, non riprendere se la pausa causa un failover a un altro nodo. I processi che iniziano ma che non riescono a essere completati a causa di un evento di failover vengono registrati come avviati, ma non mostrano voci di log aggiuntive per il completamento o l'errore. SQL Server Agent processi in questi scenari sembrano non essere mai terminati.

È possibile eseguire i processi in diversi modi:

- In base a una o più pianificazioni.

- In risposta a uno o più avvisi.

- Tramite l'esecuzione della stored procedure sp_start_job.

Ogni azione di un processo viene definita *passaggio del processo*. Un passaggio di processo, ad esempio, può essere costituito dall'esecuzione di un' \- istruzione Transact SQL, dall'esecuzione di un pacchetto SSIS o dall'invio di un comando a un Analysis Services server. I passaggi del processo vengono gestiti come parte di un processo.

Ogni passaggio del processo viene eseguito in un contesto di sicurezza specifico. Per i passaggi di processo che utilizzano Transact \- SQL, utilizzare l'istruzione Execute As per impostare il contesto di sicurezza per il passaggio di processo. Per gli altri tipi di passaggi di processo, utilizzare un account proxy per impostare il contesto di sicurezza corrispondente.

### <a name="schedules"></a>Pianificazioni

Una *pianificazione* specifica quando viene eseguito un processo. La stessa pianificazione può includere l'esecuzione di più processi e allo stesso processo possono essere applicate più pianificazioni. Per quanto riguarda quando un processo deve essere eseguito, una pianificazione può definire le condizioni seguenti:

- Ogni volta che SQL Server Agent viene avviato.

- Quando l'utilizzo della CPU del computer è a un livello definito come inattivo.

- Una sola volta in corrispondenza di una data e un'ora specifiche.

- In base a una pianificazione ricorrente.

Per altre informazioni, vedere [Creare e collegare le pianificazioni ai processi](../../ssms/agent/create-and-attach-schedules-to-jobs.md).

### <a name="alerts"></a>Avvisi

Un *avviso* è una risposta automatica a un evento specifico. Ad esempio, un evento può essere generato dall'avvio di un processo o dal raggiungimento della soglia specifica delle risorse di sistema. Le condizioni per la generazione di un avviso vengono definite dall'utente.

Un avviso può rispondere a una delle condizioni seguenti:

- Eventi SQL Server

- Condizioni delle prestazioni SQL Server

- Eventi WMI (Microsoft Windows Management Instrumentation) nel computer in cui viene eseguito SQL Server Agent

Un avviso può eseguire le azioni seguenti:

- Invio di una notifica a uno o più operatori

- Eseguire un processo

Per altre informazioni, vedere [Avvisi](../../ssms/agent/alerts.md).  

### <a name="operators"></a>Operatori

Un *operatore* definisce le informazioni di contatto per un utente responsabile della manutenzione di una o più istanze di SQL Server. In alcune organizzazioni le mansioni di operatore vengono assegnate a un unico dipendente. In organizzazioni con più server, tali mansioni possono essere ripartite tra più dipendenti. Un operatore non contiene informazioni di sicurezza e non definisce un'entità di sicurezza.  

SQL Server possibile notificare gli avvisi agli operatori tramite...

- Posta elettronica

- Cercapersone (tramite posta elettronica)

- **net send**

> [!NOTE]
> Per inviare notifiche tramite **net send**, è necessario che il servizio Windows Messenger sia avviato nel computer in cui risiede SQL Server Agent.

> [!IMPORTANT]
> Le opzioni Cercapersone e **net send** vengono rimosse da SQL Server Agent in una versione futura di SQL Server. Evitare pertanto di utilizzarle in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui sono state implementate.  

Per inviare notifiche agli operatori tramite posta elettronica o cercapersone, è necessario configurare SQL Server Agent per l'utilizzo di Posta elettronica database. Per altre informazioni, vedere [Posta elettronica database](../../relational-databases/database-mail/database-mail.md).

È possibile definire un operatore come alias assegnato a un gruppo di utenti. In questo modo, tutti i membri di tale alias non vengono verificati nello stesso momento. Per altre informazioni, vedere [Operatori](../../ssms/agent/operators.md).

## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>Sicurezza per l'amministrazione di SQL Server Agent

SQL Server Agent utilizza i ruoli predefiniti del database **SQLAgentUserRole**, **SQLAgentReaderRole** e **SQLAgentOperatorRole** nel database **msdb** per controllare l'accesso ai SQL Server Agent per gli utenti che non sono membri del ruolo predefinito del server **sysadmin** . Sottosistemi e proxy consentono agli amministratori del database di garantire l'esecuzione di tutti i passaggi di processo con le autorizzazioni minime necessarie all'esecuzione della relativa attività.  

### <a name="roles"></a>Ruoli

I membri dei ruoli predefiniti del database **SQLAgentUserRole**, **SQLAgentReaderRole** e **SQLAgentOperatorRole** in **msdb** e i membri del ruolo predefinito del server **sysadmin** hanno accesso a SQL Server Agent. Un utente che non appartiene a nessuno di questi ruoli non può utilizzare SQL Server Agent. Per altre informazioni sui ruoli usati da SQL Server Agent, vedere [implementare la sicurezza SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).

### <a name="subsystems"></a>Sottosistemi

Un sottosistema è un oggetto predefinito che rappresenta funzionalità disponibili per un passaggio di processo. Ogni proxy ha accesso a uno o più sottosistemi. I sottosistemi offrono sicurezza in quanto delimitano l'accesso alle funzionalità disponibili per un proxy. Ogni passaggio di processo viene eseguito nel contesto di un proxy, ad eccezione dei \- passaggi di processo Transact SQL. I \- passaggi del processo Transact SQL utilizzano il comando Execute As per impostare il contesto di sicurezza sul proprietario del processo.  

SQL Server definisce i sottosistemi elencati nella tabella seguente:  

|Nome sottosistema|Descrizione|
|--------------|-----------|
|Script Microsoft ActiveX|Esegue un passaggio di processo con script ActiveX.<br /><br />**Avviso** di Il sottosistema di scripting ActiveX viene rimosso da SQL Server Agent in una versione futura di Microsoft SQL Server. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.|  
|Sistema operativo (**CmdExec**)|Esegue un programma eseguibile.|
|PowerShell|Esegue un passaggio di processo con script di PowerShell.|
|Server di distribuzione repliche|Esegue un passaggio di processo tramite cui viene attivata l'utilità Agente distribuzione repliche.|
|Merge repliche|Esegue un passaggio di processo tramite cui viene attivata l'utilità Agente merge repliche.|
|Lettura coda repliche|Esegue un passaggio di processo tramite cui viene attivata l'utilità Agente di lettura coda repliche.|
|Snapshot repliche|Esegue un passaggio di processo tramite cui viene attivata l'utilità Agente snapshot repliche.|
|Lettura log repliche|Esegue un passaggio di processo tramite cui viene attivata l'utilità Agente lettura log repliche.|
|Comando di Analysis Services|Eseguire un comando Analysis Services.|
|Query di Analysis Services|Eseguire una query Analysis Services.|
|Esecuzione pacchetti SSIS|Eseguire un pacchetto SSIS.|

> [!NOTE]
> Poiché \- i passaggi di processo Transact SQL non utilizzano proxy, non esiste alcun SQL Server Agent sottosistema per i \- passaggi del processo Transact SQL.  

SQL Server Agent impone restrizioni del sottosistema anche quando l'entità di sicurezza per il proxy normalmente dispone dell'autorizzazione per eseguire l'attività nel passaggio di processo. Un proxy per un utente membro del ruolo predefinito del server sysadmin, ad esempio, non può eseguire un passaggio di processo SSIS, a meno che il proxy non abbia accesso al sottosistema SSIS, anche se l'utente può eseguire pacchetti SSIS.  

### <a name="proxies"></a>Proxy

SQL Server Agent Usa proxy per gestire i contesti di sicurezza. È possibile utilizzare un proxy in più passaggi di processo. I membri del ruolo predefinito del server **sysadmin** sono autorizzati alla creazione di proxy.  

Ogni proxy corrisponde a una credenziale di sicurezza e può essere associato a un set di sottosistemi e a un set di account di accesso. È possibile utilizzare il proxy solo per i passaggi di processo che utilizzano un sottosistema associato al proxy stesso. Per creare un passaggio di processo che utilizza un proxy specifico, il proprietario del processo deve utilizzare un account di accesso associato a tale proxy oppure un membro di un ruolo con accesso illimitato ai proxy. I membri del ruolo predefinito del server **sysadmin** hanno privilegi di accesso senza limitazioni ai proxy. I membri del ruolo **SQLAgentUserRole**, **SQLAgentReaderRole** o **SQLAgentOperatorRole** possono utilizzare solo i proxy ai quali sono specificamente autorizzati ad accedere. È necessario concedere l'accesso a proxy specifici a ogni utente membro di uno di questi SQL Server Agent ruoli predefiniti del database, in modo che l'utente possa creare passaggi di processo che utilizzano tali proxy.  

## <a name="related-tasks"></a>Attività correlate

Per configurare SQL Server Agent per automatizzare l'amministrazione SQL Server, attenersi alla procedura seguente:

1. Individuare le attività amministrative o gli eventi server che si verificano regolarmente e che possono essere gestiti a livello di programmazione. È possibile automatizzare un'attività se implica una sequenza prevedibile di passaggi e si verifica a un'ora specifica o in risposta a un evento specifico.

2. Definire un set di processi, pianificazioni, avvisi e operatori utilizzando SQL Server Management Studio, \- script Transact SQL o SQL Server Management Objects (Smo). Per altre informazioni, vedere [Crea processi](../../ssms/agent/create-jobs.md).  

3. Eseguire i processi di SQL Server Agent definiti.

> [!NOTE]
> Per l'istanza predefinita di SQL Server, il servizio SQL Server è denominato SQLSERVERAGENT. Per le istanze denominate, il servizio SQL Server Agent è denominato SQLAgent $*InstanceName*.

Se si eseguono più istanze di SQL Server, è possibile utilizzare l'amministrazione multiserver per automatizzare le attività comuni in tutte le istanze. Per altre informazioni, vedere [Amministrazione automatizzata in un'organizzazione](../../ssms/agent/automated-administration-across-an-enterprise.md).

Usare le attività seguenti per iniziare a usare SQL Server Agent:

|Descrizione|Argomento|
|-----------|-----|
|Viene descritto come configurare SQL Server Agent.|[Configurazione di SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md)|  
|Viene descritto come avviare, arrestare e sospendere il servizio SQL Server Agent.|[Avvio, arresto o sospensione del servizio SQL Server Agent](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|
|Descrive le considerazioni di cui tener conto per specificare un account per il servizio SQL Server Agent.|[Selezionare un account per il servizio SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|
|Descrive come utilizzare il log degli errori di SQL Server Agent.|[Log degli errori di SQL Server Agent](../../ssms/agent/sql-server-agent-error-log.md)|  
|Viene descritto come utilizzare gli oggetti prestazioni.|[Utilizzo degli oggetti prestazioni](../../ssms/agent/use-performance-objects.md)|
|Viene descritta la creazione guidata piano di manutenzione, un'utilità che consente di creare processi, avvisi e operatori per automatizzare l'amministrazione di un'istanza di SQL Server.|[Utilizzare la Creazione guidata piano di manutenzione](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|
|Viene descritto come utilizzare SQL Server Agent per automatizzare le attività amministrative.|[Automatizzazione delle attività amministrative &#40;SQL Server Agent&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|

## <a name="nosqlps"></a>NOSQLPS

[!INCLUDE [sql-server-powershell-no-sqlps](../../includes/sql-server-powershell-no-sqlps.md)]

## <a name="see-also"></a>Vedere anche

- [Configurazione superficie di attacco](../../relational-databases/security/surface-area-configuration.md)
- [SQL Server Agent con PowerShell](../../powershell/sql-server-powershell.md#sql-server-agent)
