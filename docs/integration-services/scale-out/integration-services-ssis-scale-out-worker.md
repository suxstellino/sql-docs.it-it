---
title: SQL Server Integration Services (SSIS) Scale Out Worker | Microsoft Docs
description: Informazioni dettagliate sul componente Scale Out Master di SQL Server Integration Services (SSIS) Scale Out.
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 35131a210e54557d63cb3e1986bddfd9d75e35ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338375"
---
# <a name="integration-services-ssis-scale-out-worker"></a>Ruolo di lavoro di scalabilità orizzontale di Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Scale Out Worker esegue il servizio Scale Out Worker per eseguire il pull di attività di esecuzione da Scale Out Master. Quindi il servizio worker esegue i pacchetti in locale con `ISServerExec.exe`.

## <a name="configure-the-scale-out-worker-service"></a>Configurare il servizio Scale Out Worker
È possibile configurare il servizio Scale Out Worker usando il file `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`. Il servizio deve essere riavviato dopo l'aggiornamento del file di configurazione.

|Configurazione  |Descrizione  |Valore predefinito|
|---------|---------|---------|
|DisplayName|Nome visualizzato del ruolo di lavoro di scalabilità orizzontale. **NON in uso in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017.**|Nome computer|
|Descrizione|Descrizione del ruolo di lavoro di scalabilità orizzontale. **NON in uso in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 2017.**|Empty|
|MasterEndpoint|Endpoint per la connessione al master di scalabilità orizzontale.|Endpoint impostato durante l'installazione del ruolo di lavoro di scalabilità orizzontale|
|MasterHttpsCertThumbprint|Identificazione personale del certificato TLS/SSL del client usato per autenticare Scale Out Master|Identificazione personale del certificato client specificato durante l'installazione del ruolo di lavoro di scalabilità orizzontale.|
|WorkerHttpsCertThumbprint|Identificazione personale del certificato del master di scalabilità orizzontale usato per autenticare il ruolo di lavoro di scalabilità orizzontale.|Identificazione personale di un certificato creato e installato automaticamente durante l'installazione del ruolo di lavoro di scalabilità orizzontale|
|StoreLocation|Percorso dell'archivio del certificato del ruolo di lavoro.|LocalMachine|
|StoreName|Nome dell'archivio del certificato del ruolo di lavoro.|My|
|AgentHeartbeatInterval|Intervallo di heartbeat del ruolo di lavoro di scalabilità orizzontale.|00:01:00|
|TaskHeartbeatInterval|Intervallo del ruolo di lavoro di scalabilità orizzontale indicante lo stato dell'attività.|00:00:10|
|HeartbeatErrorTolerance|Dopo il periodo di tempo definito dall'ultimo heartbeat dell'attività, quest'ultima viene terminata se si riceve un errore di heartbeat.|00:10:00|
|TaskRequestMaxCPU|Limite massimo di CPU per il ruolo di lavoro di scalabilità orizzontale per richiedere attività.|70|
|TaskRequestMinMemory|Limite massimo di memoria espressa in MB per il ruolo di lavoro di scalabilità orizzontale per richiedere attività.|100.0|
|MaxTaskCount|Numero massimo di attività che il ruolo di lavoro di scalabilità orizzontale può gestire.|10|
|LeaseInterval|Intervallo di lease di un'attività gestito dal ruolo di lavoro di scalabilità orizzontale.|00:01:00|
|TasksRootFolder|Cartella dei log delle attività. Se il valore è vuoto, viene usato il percorso cartella `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Tasks`. [account] è l'account che esegue il servizio Ruolo di lavoro di scalabilità orizzontale. Per impostazione predefinita, l'account è SSISScaleOutWorker140.|Empty|
|TaskLogLevel|Livello di log dell'attività del ruolo di lavoro di scalabilità orizzontale. (Verbose 0x01, Information 0x02, Warning 0x04, Error 0x08, Progress 0x10, CriticalError 0x20, Audit 0x40)|126 (Information, Warning, Error, Progress, CriticalError, Audit)|
|TaskLogSegment|Intervallo di tempo di un file di log dell'attività.|00:00:00|
|TaskLogEnabled|Specifica se il log dell'attività è abilitato.|true|
|ExecutionLogCacheFolder|Cartella usata per memorizzare nella cache il log di esecuzione del pacchetto. Se il valore è vuoto, viene usato il percorso cartella `\<drive\>:\Users\[account]\AppData\Local\SSIS\Cluster\Agent\ELogCache`. [account] è l'account che esegue il servizio Ruolo di lavoro di scalabilità orizzontale. Per impostazione predefinita, l'account è SSISScaleOutWorker140.|Empty|
|ExecutionLogMaxBufferLogCount|Numero massimo di log di esecuzione memorizzati nella cache, un buffer del log di esecuzione in memoria.|10000|
|ExecutionLogMaxInMemoryBufferCount|Numero massimo di buffer del log di esecuzione in memoria per i log di esecuzione.|10|
|ExecutionLogRetryCount|Numero di tentativi se si verifica un errore del log di esecuzione.|3|
|ExecutionLogRetryTimeout|Timeout per i tentativi se si verifica un errore del log di esecuzione. i\Se viene raggiunto ExecutionLogRetryTimeout, ExecutionLogRetryCount viene ignorato. |7.00:00:00 (7 giorni)|
|AgentId|ID agente worker di Scale Out Worker|Generato automaticamente|
||||    

## <a name="view-the-scale-out-worker-log"></a>Visualizzare il log di Scale Out Worker
Il file di log del servizio Scale Out Worker si trova nella cartella `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Agent`.

Il percorso del log di ogni singola attività è configurato nel file `WorkerSettings.config` in `TasksRootFolder`. Se non è specificato alcun valore, il log si trova nella cartella `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. 

Il parametro *[account]* è l'account che esegue il servizio Scale Out Worker. Per impostazione predefinita, l'account è `SSISScaleOutWorker140`.

## <a name="next-steps"></a>Passaggi successivi
[Integration Services (SSIS) Scale Out Master](integration-services-ssis-scale-out-master.md)
