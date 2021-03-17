---
title: Eseguire la migrazione di Oracle a SQL Server
description: 'In questa guida viene illustrato come eseguire la migrazione degli schemi Oracle a Microsoft SQL Server utilizzando SQL Server migrazione per Oracle (SSMA per Oracle). '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 7ab329891001a8ce71d77a97ced85f2ef63ad002
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603806"
---
# <a name="migration-guide-migrate-oracle-to-sql-server"></a>Guida alla migrazione: eseguire la migrazione di Oracle a SQL Server

In questa guida viene illustrato come eseguire la migrazione dei database Oracle per SQL Server utilizzando SQL Server Migration Assistant per Oracle. 

Per altri scenari, vedere la [Guida alla migrazione del database](https://datamigration.microsoft.com/).

## <a name="prerequisites"></a>Prerequisiti 

Per eseguire la migrazione del database Oracle a SQL Server, è necessario:

- Per verificare che l'ambiente di origine sia supportato.
- Per installare [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019?filetype=EXE).
- Per scaricare [SQL Server Migration Assistant (SSMA) per Oracle](https://www.microsoft.com/download/details.aspx?id=54258).
- Le [autorizzazioni necessarie per SSMA per Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) e [provider](/sql/ssma/oracle/connect-to-oracle-oracletosql).


## <a name="pre-migration"></a>Pre-migrazione

Quando si prepara la migrazione al cloud, verificare che l'ambiente di origine sia supportato e che siano stati risolti tutti i prerequisiti. Ciò consentirà di garantire una migrazione efficiente e corretta.

Questa parte del processo comporta l'esecuzione di un inventario dei database di cui è necessario eseguire la migrazione, la valutazione di tali database per potenziali problemi o blocchi di migrazione e la risoluzione di eventuali elementi che potrebbero essere stati individuati. 

> [!Important]
> SQL Server Migration Assistant per Oracle non supporta la migrazione di tutte le funzionalità di Oracle. Per le soluzioni alternative associate, vedere [approccio alla migrazione per le funzionalità Oracle selezionate](https://blogs.msdn.microsoft.com/datamigration/2017/05/10/migration-approach-for-oracle-features/)


### <a name="discover"></a>Rilevazione

Utilizzare [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883) per identificare le origini dati esistenti e i dettagli sulle funzionalità utilizzate dall'azienda per ottenere una migliore comprensione e pianificazione della migrazione. Questo processo comporta l'analisi della rete per identificare tutte le istanze Oracle dell'organizzazione insieme alla versione e alle funzionalità in uso.

Per usare MAP Toolkit per eseguire un'analisi dell'inventario, seguire questa procedura: 

1. Aprire [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883).
1. Selezionare **Crea/Seleziona database**.
1. Selezionare **Crea un database di inventario**, immettere un nome per il nuovo database di inventario che si sta creando, fornire una breve descrizione e quindi fare clic su **OK**. 
1. Selezionare **Raccogli dati di inventario** per aprire la **procedura guidata di inventario e valutazione**. 
1. Nella **procedura guidata di inventario e valutazione** scegliere **Oracle** e quindi fare clic su **Avanti**. 
1. Scegliere l'opzione di ricerca computer più adatta alle esigenze e all'ambiente aziendali, quindi selezionare **Avanti**: 
1. Immettere le credenziali o creare nuove credenziali per i sistemi che si desidera esplorare, quindi selezionare **Avanti**.
1. Impostare l'ordine delle credenziali, quindi fare clic su **Avanti**. 
1. Specificare le credenziali per ogni computer che si desidera individuare. È possibile usare credenziali univoche per ogni computer o computer oppure è possibile scegliere di usare l'elenco **tutte le credenziali del computer** .  
1. Verificare il riepilogo della selezione e quindi fare clic su **fine**.
1. Al termine dell'analisi, visualizzare il report di riepilogo **raccolta dati** . L'analisi potrebbe richiedere alcuni minuti e dipende dal numero di database. Al termine, selezionare **Chiudi** . 
1. Selezionare le **Opzioni** per generare un report sui dettagli relativi a Oracle assessment e database. Selezionare entrambe le opzioni (una alla volta) per generare il report.




### <a name="assess"></a>Valutare

Dopo aver identificato le origini dati, utilizzare il [SQL Server Migration Assistant (SSMA) per Oracle](https://www.microsoft.com/download/details.aspx?id=54258) per valutare le istanze di Oracle che eseguono la migrazione alla VM SQL Server, in modo da comprendere i gap tra i due. Con Migration Assistant è possibile esaminare i dati e gli oggetti di database, valutare i database per la migrazione, migrare oggetti di database SQL Server e quindi migrare i dati SQL Server.

Per creare una valutazione, seguire questa procedura: 

1. Aprire il  [SQL Server Migration Assistant (SSMA) per Oracle](https://www.microsoft.com/download/details.aspx?id=54258). 
1. Selezionare **File** e quindi scegliere **Nuovo progetto**. 
1. Specificare un nome di progetto, una posizione in cui salvare il progetto e quindi selezionare la destinazione della migrazione SQL Server nell'elenco a discesa. Selezionare **OK**. 
1. Immettere i valori per i dettagli della connessione Oracle nella finestra di dialogo **Connetti a Oracle** .
1. In **Oracle Metadata Explorer** selezionare lo schema Oracle e quindi fare clic su **Crea report** per generare un report HTML con le statistiche di conversione e gli errori o gli avvisi, se presenti.

1. Esaminare il report HTML per le statistiche di conversione, nonché gli errori e gli avvisi. Analizzarlo per comprendere le soluzioni e i problemi di conversione.

   È inoltre possibile accedere a questo report dalla cartella SSMA Projects come selezionato nella prima schermata. Dall'esempio precedente, individuare il file di report.xml da: 

   `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`

    e quindi aprirlo in Excel per ottenere un inventario degli oggetti Oracle e lo sforzo necessario per eseguire le conversioni dello schema.


### <a name="validate-data-types"></a>Convalidare i tipi di dati

Convalidare i mapping dei tipi di dati predefiniti e modificarli in base ai requisiti, se necessario. A questo scopo, attenersi alla procedura seguente: 

1. Selezionare **Tools** (Strumenti) dal menu. 
1. Selezionare **Project Settings** (Impostazioni progetto). 
1. Selezionare la scheda **Type mappings** (Mapping tipi). 
1. È possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **Esplora metadati Oracle**. 



### <a name="convert-schema"></a>Converti schema

Per convertire lo schema, seguire questa procedura: 

1. Opzionale Per convertire le query dinamiche o ad hoc, fare clic con il pulsante destro del mouse sul nodo e scegliere **Aggiungi istruzione**.
1. Scegliere **Connetti a SQL Server** dalla barra di spostamento in alto a sinistra e specificare i dettagli di connessione per l'SQL Server. È possibile scegliere di connettersi a un database esistente o specificare un nuovo nome, nel qual caso verrà creato un database nel server di destinazione.
1. Fare clic con il pulsante destro del mouse sullo schema e scegliere **Converti schema**.
1. Al termine della conversione dello schema, confrontare ed esaminare la struttura dello schema per identificare i potenziali problemi.

   È possibile salvare il progetto localmente per un esercizio di correzione dello schema offline. Questa operazione può essere eseguita selezionando **Salva progetto** dal menu **file** . In questo modo è possibile valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di poter pubblicare lo schema in SQL Server.


## <a name="migrate"></a>Migrate

Una volta soddisfatti i prerequisiti necessari e aver completato le attività associate alla fase di **pre-migrazione** , si è pronti per eseguire la migrazione dello schema e dei dati. La migrazione prevede due passaggi: la pubblicazione dello schema e la migrazione dei dati. 


Per pubblicare lo schema ed eseguire la migrazione dei dati, attenersi alla procedura seguente: 

1. Fare clic con il pulsante destro del mouse sul database da **Esplora metadati SQL Server**  e scegliere **Sincronizza con database**. Questa azione consente di pubblicare lo schema Oracle in SQL Server. 
1. Fare clic con il pulsante destro del mouse sullo schema Oracle da **Oracle Metadata Explorer** e scegliere **Migrate data**. In alternativa, è possibile selezionare migrare i dati dall'esplorazione della riga superiore.
1. Specificare i dettagli della connessione per Oracle e SQL Server nella finestra di dialogo.
1. Al termine della migrazione, visualizzare il report di migrazione dei dati:
1. Connettersi al SQL Server usando [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) per esaminare i dati e lo schema nell'istanza di SQL Server. 

Oltre a usare SSMA, è anche possibile usare SQL Server Integration Services (SSIS) per eseguire la migrazione dei dati. Per altre informazioni, vedere: 
- Il Blog [SQL Server Migration Assistant: come valutare ed eseguire la migrazione dei dati da piattaforme dati non Microsoft a SQL Server](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/).
- L'articolo [Introduzione con SQL Server Integration Services](https://docs.microsoft.com//sql/integration-services/sql-server-integration-services).
- Il white paper [SQL Server Integration Services: SSIS per Azure e lo spostamento di dati ibridi](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx).



## <a name="post-migration"></a>Post-migrazione 

Dopo aver completato la fase di **migrazione** , è necessario eseguire una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni

Dopo la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza usavano l'origine devono iniziare a usare la destinazione. Per ottenere questo risultato, in alcuni casi sarà necessario apportare modifiche alle applicazioni.

[Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) è un'estensione per Visual Studio Code che consente di analizzare il codice sorgente Java e rilevare chiamate API di accesso ai dati e query, offrendo una visualizzazione a un singolo riquadro degli elementi che devono essere risolti per supportare il nuovo back-end del database. Per altre informazioni, vedere il Blog [eseguire la migrazione dell'applicazione Java da Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727) . 

### <a name="perform-tests"></a>Eseguire test

L'approccio di test per la migrazione del database consiste nell'eseguire le attività seguenti:

1. **Sviluppare i test di convalida**. Per testare la migrazione del database, è necessario usare le query SQL. È necessario creare le query di convalida da eseguire sia sul database di origine che su quello di destinazione. Le query di convalida devono essere estese all'ambito definito.

2. **Configurare l'ambiente di test**. L'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.

3. **Eseguire i test di convalida**. Eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.

4. **Eseguire test delle prestazioni**. Eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.


### <a name="optimize"></a>Ottimizzazione

La fase post-migrazione è fondamentale per riconciliare eventuali problemi di accuratezza dei dati e verificare la completezza, nonché per risolvere i problemi di prestazioni del carico di lavoro.

> [!Note]
> Per ulteriori dettagli su questi problemi e su passaggi specifici per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](https://docs.microsoft.com//sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="migration-assets"></a>Risorse per la migrazione 

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere le risorse seguenti, che sono state sviluppate a supporto di un progetto di migrazione del mondo reale.


| **Titolo/collegamento**                                                                                                                                          | **Descrizione**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Strumento e modello di valutazione del carico di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Questo strumento fornisce le piattaforme di destinazione "più idonee" suggerite, la preparazione per il cloud e il livello di monitoraggio e aggiornamento dell'applicazione/database per un determinato carico di lavoro. Offre semplici calcoli con un solo clic e generazione di report che consentono di accelerare le valutazioni di grandi dimensioni grazie a un processo decisionale automatizzato e uniforme della piattaforma di destinazione.                                                          |
| [Elementi di script di inventario Oracle](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Questo asset include una query PL/SQL che raggiunge le tabelle di sistema Oracle e fornisce un conteggio degli oggetti in base al tipo di schema, al tipo di oggetto e allo stato. Fornisce inoltre una stima approssimativa dei dati non elaborati in ogni schema e il dimensionamento delle tabelle in ogni schema, con risultati archiviati in formato CSV.                                                                                                               |
| [Automatizzare la raccolta SSMA Oracle Assessment & il consolidamento](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Questo set di risorse utilizza un file con estensione CSV come voce (sources.csv nelle cartelle del progetto) per produrre i file XML necessari per eseguire SSMA assessment in modalità console. Il source.csv viene fornito dal cliente in base a un inventario delle istanze esistenti di Oracle. I file di output sono AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml e VariableValueFile.xml.|
| [SSMA per errori comuni di Oracle e come risolverli](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | Con Oracle è possibile assegnare una condizione non scalare nella clausola WHERE. Tuttavia, SQL Server non supporta questo tipo di condizione. Di conseguenza, SQL Server Migration Assistant (SSMA) per Oracle non converte le query con una condizione non scalare nella clausola WHERE, generando invece un errore O2SS0001. In questa white paper vengono fornite informazioni dettagliate sul problema e su come risolverlo.          |
| [Manuale della migrazione da Oracle a SQL Server](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | Questo documento è incentrato sulle attività associate alla migrazione di uno schema Oracle alla versione più recente di SQL Server base. Se la migrazione richiede modifiche a funzionalità o funzionalità, il possibile effetto di ogni modifica sulle applicazioni che utilizzano il database deve essere considerato attentamente.                                                     |

Queste risorse sono state sviluppate come parte del programma Data SQL Ninja, sponsorizzato dal team di progettazione Azure Data Group. Obiettivo principale del programma Data SQL Ninja è rendere disponibili opportunità per velocizzare progetti di modernizzazione complessi e la migrazione delle piattaforme dati alla piattaforma dati di Microsoft Azure. Se si ritiene che l'organizzazione possa essere interessata a partecipare al programma Data SQL Ninja, contattare il team dell'account per richiedere l'invio di una candidatura.

## <a name="next-steps"></a>Passaggi successivi

Dopo la migrazione, vedere [Guida di ottimizzazione e convalida post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

Per un elenco dei servizi e degli strumenti di Microsoft e di terze parti disponibili per agevolare diversi scenari di migrazione di database e dati, nonché per attività speciali, vedere [Strumenti e servizi per la migrazione dei dati](/azure/dms/dms-tools-matrix).

Per altre guide alla migrazione, vedere [Migrazione dei database](https://datamigration.microsoft.com/). 

Per contenuti video, vedere:
- [Panoramica del percorso di migrazione](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
