---
title: Informazioni su Azure Data Studio
description: Azure Data Studio è uno strumento gratuito e leggero per la gestione di SQL Server, database SQL di Azure e Azure Synapse Analytics, che viene eseguito in Windows, macOS e Linux.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 10/20/2020
ms.openlocfilehash: b9adac14bfd458b96fe4e46c478af2d2b5d6a181
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464870"
---
# <a name="what-is-azure-data-studio"></a>Che cos'è Azure Data Studio?

Azure Data Studio è uno strumento di database multipiattaforma per professionisti della gestione di dati che usano piattaforme dati locali e nel cloud in Windows, macOS e Linux.

Azure Data Studio offre un'esperienza di modifica moderna per la gestione dei dati tra più origini con funzionalità IntelliSense, frammenti di codice, integrazione del controllo del codice sorgente e un terminale integrato. Azure Data Studio è stato progettato in base alle esigenze dell'utente della piattaforma dati, con grafici predefiniti di set di risultati delle query e dashboard personalizzabili.

Il codice sorgente per Azure Data Studio e i relativi provider di dati è disponibile in GitHub in base a un contratto di licenza che fornisce il diritto di modificare e usare il software, ma non di ridistribuirlo o ospitarlo in un servizio cloud. Per altre informazioni, vedere [Domande frequenti - Azure Data Studio](faq.yml).

**[Scaricare e installare Azure Data Studio](./download-azure-data-studio.md)**

## <a name="sql-code-editor-with-intellisense"></a>Editor di codice SQL con IntelliSense

Azure Data Studio offre un'esperienza di codifica SQL moderna e incentrata sulla tastiera che semplifica le attività quotidiane grazie a funzionalità integrate quali finestre a più schede, un editor SQL avanzato, IntelliSense, completamento delle parole chiave, frammenti di codice, esplorazione del codice e integrazione del controllo del codice sorgente (Git). Consente di eseguire query SQL su richiesta e visualizzare e salvare i risultati come testo, JSON o Excel, oltre che modificare i dati, organizzare le connessioni ai database preferiti e sfogliare gli oggetti di database in un'esperienza di esplorazione familiare. Per informazioni su come usare l'editor SQL, vedere [Usare l'editor SQL per creare oggetti di database](tutorial-sql-editor.md).

## <a name="smart-sql-code-snippets"></a>Frammenti di codice SQL intelligenti

I frammenti di codice SQL generano la sintassi SQL appropriata per creare database, tabelle, viste, stored procedure, utenti, accessi e ruoli, nonché aggiornare gli oggetti di database esistenti. Usare i frammenti di codice intelligenti per creare rapidamente copie del database a scopo di sviluppo o test e per generare ed eseguire script di creazione e inserimento.

Azure Data Studio offre anche funzionalità per la creazione di frammenti di codice SQL personalizzati. Per altre informazioni, vedere [Creare e usare frammenti di codice](code-snippets.md).

## <a name="customizable-server-and-database-dashboards"></a>Dashboard di server e database personalizzabili

È possibile creare dashboard avanzati e personalizzabili per monitorare e risolvere rapidamente i colli di bottiglia delle prestazioni nei database. Per informazioni sui widget di informazioni dettagliate e sui dashboard per database e server, vedere [Gestire server e database con i widget di informazioni dettagliate](insight-widgets.md).

## <a name="connection-management-server-groups"></a>Gestione delle connessioni (gruppi di server)

I gruppi di server consentono di organizzare le informazioni di connessione per i server e i database che si utilizzano. Per informazioni dettagliate, vedere [Gruppi di server](server-groups.md).

## <a name="integrated-terminal"></a>Terminale integrato

Nella finestra del terminale integrato all'interno dell'interfaccia utente di Azure Data Studio si possono usare i propri strumenti da riga di comando preferiti, ad esempio Bash, PowerShell, sqlcmd, bcp e ssh. Per informazioni sul terminale integrato, vedere [Terminale integrato](integrated-terminal.md).

## <a name="extensibility-and-extension-authoring"></a>Estendibilità e creazione di estensioni

È possibile migliorare l'esperienza di Azure Data Studio estendendo le funzionalità dell'installazione di base. Azure Data Studio rende disponibili punti di estendibilità per le attività di gestione dei dati, oltre al supporto per la creazione di estensioni.

Per informazioni sull'estendibilità in Azure Data Studio, vedere [Estendibilità](extensibility.md).
Per informazioni sulla creazione di estensioni, vedere [Creazione di estensioni](extensions/extension-authoring.md).

## <a name="feature-comparison-with-sql-server-management-studio-ssms"></a>Confronto delle funzionalità con Server Management Studio (SSMS)

**Usare Azure Data Studio se:**

- Ci si occupa per lo più di modificare o eseguire query.
- È necessario poter creare grafici e visualizzare set di risultati rapidamente.
- Si può eseguire la maggior parte delle attività amministrative tramite il terminale integrato usando sqlcmd o PowerShell.
- Si ha una necessità minima di esperienze di tipo procedura guidata.
- Non è necessario eseguire operazioni approfondite di configurazione amministrativa o della piattaforma.
- L'ambiente di esecuzione è macOS o Linux.

**Usare SQL Server Management Studio se:**

- Si eseguono operazioni approfondite di configurazione amministrativa o della piattaforma.
- Ci si occupa di gestione della sicurezza, incluse gestione degli utenti, valutazione della vulnerabilità e configurazione delle funzionalità di sicurezza.
- È necessario usare dashboard e procedure guidate di ottimizzazione delle prestazioni.
- Si usano diagrammi di database e progettazione tabelle.
- È necessario accedere ai server registrati.
- Si usano statistiche sulle query dinamiche o statistiche client.

### <a name="shell-features"></a>Funzionalità shell

|Funzionalità|Azure Data Studio|SSMS|
|:---|:---|:---|
|Accesso ad Azure|Sì|Sì|
|Dashboard|Sì| |
|Estensioni|Sì| |
|Terminale integrato|Sì||
|Esplora oggetti|Sì|Sì|
|Scripting per gli oggetti|Sì|Sì|
|Sistema di progetto|Sì||
|Selezione da tabella|Sì|Sì|
|Controllo del codice sorgente|Sì||
|Riquadro attività|Sì||
|Temi, compresa la Modalità scura|Sì||
|Azure Resource Explorer|Anteprima||
|Procedura guidata Genera script||Sì|
|Proprietà degli oggetti||Sì|
|Progettazione tabelle||Sì|

### <a name="query-editor"></a>Editor di query

|Funzionalità|Azure Data Studio|SSMS|
|:---|:---|:---|
|Visualizzatore grafico|Sì||
|Esportazione risultati in CSV, JSON, XLSX|Sì||
|Risultati in un file||Sì|
|Risultati in formato testo||Sì|
|IntelliSense|Sì|Sì|
|Frammenti di codice|Sì|Sì|
|Showplan|Anteprima|Sì|
|Statistiche client||Sì|
|Statistiche sulle query dinamiche||Sì|
|Opzioni query||Sì|
|Visualizzatore spaziale||Sì|
|SQLCMD|Sì|Sì|

### <a name="operating-system-support"></a>Supporto nei sistemi operativi

|Funzionalità|Azure Data Studio|SSMS|
|:---|:---|:---|
|Windows|Sì|Sì|
|macOS|Sì||
|Linux|Sì||

### <a name="data-engineering"></a>Ingegneria dei dati

|Funzionalità|Azure Data Studio|SSMS|
|:---|:---|:---|
|Procedura guidata dati esterni|Anteprima||
|Integrazione HDFS|Anteprima||
|Notebook|Anteprima||

### <a name="database-administration"></a>Amministrazione del database

|Funzionalità|Azure Data Studio|SSMS|
|:---|:---|:---|
|Backup/ripristino|Sì|Sì|
|Importazione file flat|Sì|Sì|
|SQL Agent|Anteprima|Sì|
|SQL Profiler|Anteprima|Sì|
|Always On||Sì|
|Always Encrypted||Sì|
|Procedura guidata Copia dati||Sì|
|Ottimizzazione guidata dati||Sì|
|Diagrammi di database||Sì|
|Visualizzatore log degli errori||Sì|
|Piani di manutenzione||Sì|
|Query multiserver||Sì|
|Gestione basata su criteri||Sì|
|PolyBase||Sì|
|Archivio query||Sì|
|Server registrati||Sì|
|Replica||Sì|
|Gestione della sicurezza||Sì|
|Broker di servizio||Sì|
|SQL Assessment|Anteprima|Sì|
|SQL Mail||Sì|
|Esplora modelli||Sì|
|Valutazione della vulnerabilità||Sì|
|Gestione XEvent||Sì|

### <a name="database-development"></a>Sviluppo di database
|Funzionalità|Azure Data Studio|SSMS|
|:---|:---|:---|
|Importazione\esportazione DACPAC|Sì|Sì|
|Progetti SQL|Anteprima||
|Confronto schema|Sì||

## <a name="next-steps"></a>Passaggi successivi

- [Scaricare e installare Azure Data Studio](./download-azure-data-studio.md)
- [Domande frequenti su Azure Data Studio](faq.yml)
- [Connettersi ed eseguire query in SQL Server](quickstart-sql-server.md)
- [Connettersi ed eseguire query nel database SQL di Azure](quickstart-sql-database.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
