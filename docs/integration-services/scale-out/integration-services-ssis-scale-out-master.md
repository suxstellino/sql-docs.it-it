---
title: Scale Out Master | Microsoft Docs
description: Informazioni sul componente Scale Out Master del servizio SQL Server Integration Services (SSIS) Scale Out Master.
ms.custom: performance
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 8f3b205acd32ba48b86ac797fd99fe4cef4fccf5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347404"
---
# <a name="integration-services-ssis-scale-out-master"></a>Master di scalabilità orizzontale di Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Scale Out Master gestisce il sistema di scalabilità orizzontale tramite il catalogo SSISDB e il servizio Scale Out Master. 

Il catalogo SSISDB archivia tutte le informazioni relative ai componenti Scale Out Worker, ai pacchetti e alle esecuzioni. Fornisce l'interfaccia per abilitare un ruolo di lavoro di scalabilità orizzontale ed eseguire pacchetti nel servizio di scalabilità orizzontale. Per altre informazioni, vedere [Procedura dettagliata: Installare il servizio Integration Services (SSIS) Scale Out](walkthrough-set-up-integration-services-scale-out.md) ed [Eseguire pacchetti nel servizio Integration Services (SSIS) Scale Out](run-packages-in-integration-services-ssis-scale-out.md).

Scale Out Master è un servizio Windows responsabile delle comunicazioni con i componenti Scale Out Worker. Restituisce lo stato dell'esecuzione di pacchetti nei componenti Scale Out Worker su HTTPS ed elabora i dati nel database SSISDB. 

## <a name="scale-out-views-and-stored-procedures-in-ssisdb"></a>Viste Scale Out e stored procedure in SSISDB

### <a name="views"></a>Viste

- [[catalog].[master_properties]](../../integration-services/system-views/catalog-master-properties-ssisdb-database.md)
- [[catalog].[worker_agents]](../../integration-services/system-views/catalog-worker-agents-ssisdb-database.md)

### <a name="stored-procedures"></a>Stored procedure

- Per la gestione di componenti Scale Out Worker:
    - [[catalog].[disable_worker_agent]](../../integration-services/system-stored-procedures/catalog-disable-worker-agent-ssisdb-database.md)
    - [[catalog].[enable_worker_agent]](../../integration-services/system-stored-procedures/catalog-enable-worker-agent-ssisdb-database.md)

- Per l'esecuzione di pacchetti in Scale Out:
    - [[catalog].[create_execution]](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md)
    - [[catalog].[add_execution_worker]](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)
    - [[catalog].[start_execution]](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)

## <a name="configure-the-scale-out-master-service"></a>Configurare il servizio Scale Out Master

È possibile configurare il servizio Scale Out Master usando il file `<drive>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`. Il servizio deve essere riavviato dopo l'aggiornamento del file di configurazione.


|Configurazione  |Descrizione  |Default Value  |
|---------|---------|---------|
|PortNumber|Numero di porta di rete usato per comunicare con un ruolo di lavoro di scalabilità orizzontale.|8391|
|SSLCertThumbprint|Identificazione personale del certificato TLS/SSL usato per proteggere la comunicazione con un'istanza di Scale Out Worker.|Identificazione personale del certificato TLS/SSL specificato durante l'installazione di Scale Out Master|
|SqlServerName|Nome dell'istanza di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] contenente il catalogo SSISDB. Ad esempio, NomeServer\\NomeIstanza.|Nome dell'istanza di SQL Server installata con il master di scalabilità orizzontale.|
|CleanupCompletedJobsIntervalInMs|Intervallo per l'eliminazione dei processi di esecuzione completati, espresso in millisecondi.|43200000|
|DealWithExpiredTasksIntervalInMs|Intervallo per la gestione dei processi di esecuzione scaduti, espresso in millisecondi.|300000|
|MasterHeartbeatIntervalInMs|Intervallo per l'heartbeat del master di scalabilità orizzontale, espresso in millisecondi. Specifica la frequenza con cui Scale Out Master aggiorna il proprio stato online nel catalogo SSISDB.|30000|
|SqlConnectionTimeoutInSecs|Timeout della connessione SQL in secondi per la connessione a SSISDB.|15|
||||    

## <a name="view-the-scale-out-master-service-log"></a>Visualizzare il log del servizio Scale Out Master

Il file di log del servizio Scale Out Master si trova nella cartella `<drive>:\Users\[account]\AppData\Local\SSIS\ScaleOut\Master`. 

Il parametro *[account]* è l'account che esegue il servizio Scale Out Master. Per impostazione predefinita, tale account è `SSISScaleOutMaster140`.

## <a name="next-steps"></a>Passaggi successivi

[Integration Services (SSIS) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
