---
title: Validate SSIS packages deployed to Azure (Convalidare pacchetti SSIS distribuiti in Azure) | Microsoft Docs
description: Informazioni su come la procedura guidata Distribuzione pacchetto di SSIS controlla i pacchetti per individuare eventuali problemi noti che potrebbero impedire l'esecuzione dei pacchetti come previsto in Azure.
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 3da89cbe8d0028a2b362e6358d1b30b3075e030c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345678"
---
# <a name="validate-sql-server-integration-services-ssis-packages-deployed-to-azure"></a>Convalidare i pacchetti SQL Server Integration Services (SSIS) distribuiti in Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Quando si distribuisce un progetto di SQL Server Integration Services (SSIS) nel catalogo SSIS (SSISDB) in un server di Azure, la procedura guidata Distribuzione pacchetto prevede un passaggio di convalida aggiuntivo dopo la pagina **Verifica**. Questo passaggio di convalida controlla che i pacchetti del progetto non contengano problemi noti che potrebbero impedire la corretta esecuzione Runtime di integrazione di Azure SSIS. Quindi la procedura guidata consente di visualizzare gli avvisi applicabili nella pagina relativa alla **convalida**.

> [!IMPORTANT]
> La convalida descritta in questo articolo si verifica quando si distribuisce un progetto con una versione di SQL Server Data Tools (SSDT) 17.4 o successiva. Per scaricare la versione più recente di SSDT, vedere [Scaricare la versione più recente di SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md).

Per altre informazioni sulla distribuzione guidata dei pacchetti, vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../packages/deploy-integration-services-ssis-projects-and-packages.md).

## <a name="validate-connection-managers"></a>Convalidare le gestioni connessioni

La procedura guidata controlla che in alcune gestioni connessioni non siano presenti i problemi seguenti che potrebbero causare un errore di connessione:
- **Autenticazione di Windows**. Se una stringa di connessione usa l'autenticazione di Windows, la convalida genera un avviso. L'autenticazione di Windows richiede ulteriori passaggi di configurazione. Per altre informazioni, vedere [Connettersi a dati e condivisioni file con autenticazione di Windows](/azure/data-factory/ssis-azure-connect-with-windows-auth).
- **Percorso file**. Se una stringa di connessione contiene un percorso file locale hard-coded come `C:\\...`, la convalida genera un avviso. L'esecuzione di pacchetti che contengono un percorso assoluto potrebbe non riuscire.
- **Percorso UNC**. Se una stringa di connessione contiene un percorso UNC, la convalida genera un avviso. L'esecuzione di pacchetti che contengono un percorso UNC potrebbe non riuscire, in genere ciò si verifica perché i pacchetti UNC richiedono l'autenticazione di Windows per effettuare l'accesso.
- **Nome host**. Se una proprietà del server contiene il nome host anziché l'indirizzo IP, la convalida genera un avviso. L'esecuzione di pacchetti che contengono il nome host potrebbe non riuscire, in genere ciò si verifica perché la rete virtuale di Azure richiede la corretta configurazione di DNS per supportare la risoluzione dei nomi DNS.
- **Provider o driver**. Se un provider o il driver non è supportato, la convalida genera un avviso. In questo momento è supportato solamente un numero ridotto di driver e provider predefiniti.

La procedura guidata esegue i controlli di convalida seguenti per le gestioni connessioni nell'elenco.

| Gestione connessione | Autenticazione di Windows | Percorso del file | Percorso UNC | Nome host | Provider o driver |
|--------------------|----------|-----------|-----|-----------|-------------------|
| Ado                | âœ“        |           |     | âœ“         | âœ“                 |
| AdoNet             | âœ“        |           |     | âœ“         | âœ“                 |
| Cache              |          | âœ“         | âœ“   |           |                   |
| Excel              |          | âœ“         | âœ“   |           |                   |
| File               |          | âœ“         | âœ“   |           |                   |
| FlatFile           |          | âœ“         | âœ“   |           |                   |
| FTP                |          |           |     | âœ“         |                   |
| MsOLAP100          |          |           |     | âœ“         | âœ“                 |
| MultiFile          |          | âœ“         | âœ“   |           |                   |
| MultiFlatFile      |          | âœ“         | âœ“   |           |                   |
| OData              | âœ“        |           |     | âœ“         |                   |
| Odbc               | âœ“        |           |     | âœ“         | âœ“                 |
| OleDb              | âœ“        |           |     | âœ“         | âœ“                 |
| SmoServer          | âœ“        |           |     | âœ“         |                   |
| SMTP               | âœ“        |           |     | âœ“         |                   |
| SQLMOBILE          |          | âœ“         | âœ“   |           |                   |
| WMI                | âœ“        |           |     |           |                   |
|||||||

## <a name="validate-sources-and-destinations"></a>Convalidare origini e destinazioni
Le origini di terze parti e le destinazioni seguenti non sono supportate:

-   Origine e destinazione Oracle di Attunity
-   Origine e destinazione Teradata di Attunity
-   Origine e destinazione SAP BW

## <a name="validate-tasks-and-components"></a>Convalidare attività e componenti

### <a name="execute-process-task"></a>Attività Esegui processo

La convalida genera un avviso se un comando punta a un file locale con un percorso assoluto o a un file con un percorso UNC. Questi percorsi potrebbero causare un esito negativo dell'esecuzione in Azure.

### <a name="script-task-and-script-component"></a>Attività e componente Script

La convalida genera un avviso se un pacchetto contiene un'attività script o un componente script, che può fare riferimento o chiamare assembly non supportati. Questi riferimenti o chiamate possono causare un errore di esecuzione.

### <a name="other-components"></a>Altri componenti

Il formato Orc non è supportato nella destinazione HDFS e nella destinazione di Azure Data Lake Store.

## <a name="next-steps"></a>Passaggi successivi
Per informazioni su come pianificare l'esecuzione del pacchetto in Azure, vedere [Pianificare pacchetti SSIS in Azure](ssis-azure-schedule-packages.md).