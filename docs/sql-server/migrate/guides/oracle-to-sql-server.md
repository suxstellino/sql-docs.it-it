---
title: 'Guida alla migrazione da Oracle a SQL Server:'
description: In questa guida viene illustrato come eseguire la migrazione degli schemi Oracle a Microsoft SQL Server utilizzando SQL Server Migration Assistant (SSMA) per Oracle.
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 1d84ff8831d04b84075e73c44a3ab9bdcd694b70
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981025"
---
# <a name="migration-guide-oracle-to-sql-server"></a>Guida alla migrazione: da Oracle a SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

In questa guida si apprenderà come eseguire la migrazione dei database Oracle a SQL Server usando SQL Server Migration Assistant per Oracle (SSMA per Oracle). 

Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Prerequisiti 

Prima di iniziare a eseguire la migrazione del database Oracle al SQL Server, procedere come segue:

- Verificare che l'ambiente di origine sia supportato.
- Scaricare e installare [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019?filetype=EXE).
- Scaricare e installare [SSMA per Oracle](https://www.microsoft.com/download/details.aspx?id=54258).
- Ottenere le [autorizzazioni necessarie per SSMA per Oracle](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) e [provider](/sql/ssma/oracle/connect-to-oracle-oracletosql).
- Ottenere connettività e autorizzazioni sufficienti per accedere sia all'origine che alla destinazione. 


## <a name="pre-migration"></a>Pre-migrazione

Quando si prepara la migrazione al cloud, verificare che l'ambiente di origine sia supportato e che siano stati soddisfatti tutti gli altri prerequisiti. Questa operazione consentirà di garantire una migrazione efficiente e corretta.

Questa parte del processo implica l'esecuzione di un inventario dei database di cui è necessario eseguire la migrazione, la relativa valutazione per potenziali problemi o blocchi di migrazione e la risoluzione di eventuali elementi che potrebbero essere stati individuati. 


### <a name="discover"></a>Rilevazione

Per una migliore comprensione e pianificazione della migrazione, utilizzare [Microsoft Assessment and Planning (Map) Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883) per identificare le origini dati esistenti e i dettagli sulle funzionalità utilizzate dall'organizzazione. Questo processo comporta l'analisi della rete per identificare tutte le istanze, le versioni e le funzionalità di Oracle dell'organizzazione.

Per usare MAP Toolkit per eseguire un'analisi dell'inventario, seguire questa procedura: 

1. Aprire [MAP Toolkit](https://go.microsoft.com/fwlink/?LinkID=316883).

1. Nel riquadro **Panoramica** selezionare **Crea/Seleziona database**. 

   ![Screenshot del collegamento "Crea/Seleziona database" nel riquadro Panoramica di MAP Toolkit.](./media/oracle-to-sql-server/select-database.png)

1. In **Crea o seleziona un database** selezionare **Crea un database di inventario**, immettere un nome per il database di inventario che si sta creando, fornire una breve descrizione e quindi fare clic su **OK**.

   :::image type="content" source="media/oracle-to-sql-server/create-inventory-database.png" alt-text="Screenshot dell'opzione ' crea un database di inventario ' nel Toolkit MAPS.":::

1. Selezionare **Raccogli dati di inventario** per aprire la **procedura guidata di inventario e valutazione**. 

   :::image type="content" source="media/oracle-to-sql-server/collect-inventory-data.png" alt-text="Screenshot del collegamento ' Raccogli dati di inventario ' nella procedura guidata di inventario e valutazione.":::

1. Nella procedura guidata selezionare **Oracle** e quindi fare clic su **Avanti**. 

   ![Screenshot dell'opzione Oracle e del pulsante Avanti nella procedura guidata di inventario e valutazione.](./media/oracle-to-sql-server/choose-oracle.png)

1. Selezionare l'opzione di ricerca computer più adatta alle esigenze e all'ambiente dell'organizzazione e quindi fare clic su **Avanti**. 

   ![Screenshot dell'elenco di metodi di individuazione dei computer più adatti alle esigenze dell'organizzazione.](./media/oracle-to-sql-server/choose-search-option.png)

1. Immettere le credenziali correnti o creare nuove credenziali per i sistemi che si desidera esplorare, quindi selezionare **Avanti**.

    ![Screenshot del riquadro della procedura guidata per l'immissione delle credenziali del computer.](./media/oracle-to-sql-server/choose-credentials.png)

1. Impostare l'ordine delle credenziali, quindi fare clic su **Avanti**.

   ![Screenshot del riquadro della procedura guidata per l'impostazione dell'ordine delle credenziali.](./media/oracle-to-sql-server/set-credential-order.png)  

1. Specificare le credenziali per ogni computer che si desidera individuare. È possibile usare credenziali univoche per ogni computer o computer oppure selezionarle dall'elenco **computer** .

   ![Screenshot dell'opzione "use all computers Credential list" per specificare le credenziali per ogni computer che si desidera individuare.](./media/oracle-to-sql-server/specify-credentials-for-each-computer.png)

1. Verificare il riepilogo della selezione e quindi fare clic su **fine**.

   ![Screenshot della pagina di riepilogo della procedura guidata per verificare le selezioni effettuate.](./media/oracle-to-sql-server/review-summary.png)

1. Al termine dell'analisi, visualizzare il report di riepilogo **raccolta dati** . L'analisi potrebbe richiedere alcuni minuti, a seconda del numero di database. Al termine, selezionare **Chiudi**.

   ![Screenshot della pagina del report di riepilogo raccolta dati.](./media/oracle-to-sql-server/collection-summary-report.png)

1. Selezionare le **Opzioni** per generare un report sui dettagli relativi a Oracle assessment e database. Selezionare entrambe le opzioni, una alla volta, per generare il report.


### <a name="assess"></a>Valutare

Dopo aver identificato le origini dati, utilizzare [SSMA per Oracle](https://www.microsoft.com/download/details.aspx?id=54258) per valutare l'istanza di Oracle di cui si sta eseguendo la migrazione alla macchina virtuale SQL Server in modo da comprendere i gap tra i due. Con Migration Assistant è possibile esaminare gli oggetti e i dati di database, valutare i database per la migrazione, migrare gli oggetti di database in SQL Server, quindi migrare i dati SQL Server.

Per creare una valutazione, procedere come segue: 

1. Aprire [SSMA per Oracle](https://www.microsoft.com/download/details.aspx?id=54258). 
1. Selezionare **file**, quindi fare clic su **nuovo progetto**. 
1. Specificare un nome e un percorso per il progetto e quindi nell'elenco a discesa selezionare un SQL Server destinazione della migrazione. Selezionare **OK**. 

   ![Screenshot del riquadro nuovo progetto in SSMA per Oracle.](./media/oracle-to-sql-server/new-project.png)

1. Selezionare **Connetti a Oracle**, immettere i dettagli della connessione Oracle, quindi selezionare **Connetti**.

   ![Screenshot del riquadro Connetti a Oracle.](./media/oracle-to-sql-server/connect-to-oracle.png)

1. Nel riquadro **Filtra oggetti** selezionare gli schemi Oracle di cui si desidera eseguire la migrazione e quindi fare clic su **OK**.

   ![Screenshot del riquadro "Filtra oggetti" per la selezione degli schemi da caricare.](./media/oracle-to-sql-server/select-schema.png)

1. Nel riquadro **Oracle Metadata Explorer** selezionare gli schemi Oracle con cui si sta lavorando, quindi selezionare **Crea report** per generare un report HTML con statistiche di conversione ed errori o avvisi, se presenti. In alternativa, è possibile selezionare la scheda **Crea report** in alto a destra.

   ![Screenshot dei collegamenti "Crea report" in Esplora metadati Oracle.](./media/oracle-to-sql-server/create-report.png)

1. Esaminare il report HTML per comprendere le statistiche di conversione e gli eventuali errori o avvisi. È inoltre possibile aprire il report in Excel per ottenere un inventario degli oggetti Oracle e lo sforzo necessario per eseguire le conversioni dello schema. Il percorso predefinito per il report è la cartella report all'interno di SSMAProjects. Ad esempio: 

   `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`

   ![Screenshot di un report di conversione in SSMA.](./media/oracle-to-sql-server/conversion-report.png)


### <a name="validate-data-types"></a>Convalidare i tipi di dati

Convalidare i mapping del tipo di dati predefiniti e modificarli in base ai requisiti, se necessario. A tale scopo, procedere nel seguente modo: 

1. Selezionare **strumenti**, quindi selezionare **Impostazioni progetto**.  
1. Selezionare la scheda **mapping dei tipi** .

   ![Screenshot del riquadro "mapping dei tipi" in SSMA per Oracle.](./media/oracle-to-sql-server/type-mappings.png)

1. È possibile modificare il mapping dei tipi per ogni tabella selezionando il nome della tabella nel riquadro di **Esplora metadati di Oracle** . 

### <a name="convert-schema"></a>Converti schema

Per convertire lo schema, procedere come segue: 

1. Opzionale Per convertire query dinamiche o specializzate, fare clic con il pulsante destro del mouse sul nodo, quindi scegliere **Aggiungi istruzione**. 

1. Selezionare la scheda **Connetti a SQL Server** , quindi immettere i dettagli della connessione per l'istanza di SQL Server.  

    a. Nell'elenco a discesa **database** selezionare il database di destinazione o specificare un nuovo nome per creare un database nel server di destinazione.   
    b. Fornire i dettagli di autenticazione.   
    c. Selezionare **Connetti**.

   ![Screenshot del riquadro Connetti a SQL Server in SSMA per Oracle.](./media/oracle-to-sql-server/connect-to-sql.png)

1. Nel riquadro **Oracle Metadata Explorer** fare clic con il pulsante destro del mouse sullo schema che si sta utilizzando, quindi scegliere **Converti schema**. In alternativa, è possibile selezionare la scheda **Converti schema** in alto a destra.

   ![Screenshot del comando "Convert schema" del riquadro "Oracle Metadata Explorer".](./media/oracle-to-sql-server/convert-schema.png)

1. Al termine della conversione, confrontare gli oggetti convertiti con quelli originali per identificare i potenziali problemi e risolverli in base alle indicazioni.

   ![Screenshot che mostra un confronto tra gli oggetti convertiti e gli oggetti originali.](./media/oracle-to-sql-server/table-mapping.png)

   Confrontare il testo Transact-SQL convertito con il codice originale ed esaminare le indicazioni.

   ![Screenshot che mostra un confronto tra il testo convertito e il codice originale.](./media/oracle-to-sql-server/procedure-comparison.png)

1. Nel riquadro Output selezionare l'icona **Verifica risultati** , quindi esaminare gli eventuali errori nel riquadro **Elenco errori** . 
1. Per un esercizio di correzione dello schema offline, salvare il progetto localmente selezionando **file**  >  **Salva progetto**. In questo modo è possibile valutare gli schemi di origine e di destinazione offline e risolverli prima di pubblicare lo schema nell'istanza di SQL Server.


## <a name="migrate-database"></a>Migrazione di database

Dopo aver soddisfatto i prerequisiti e completato le attività associate alla fase di *pre-migrazione* , si è pronti per eseguire la migrazione dello schema e del database. La migrazione prevede due passaggi: la pubblicazione dello schema e la migrazione del database. 

Per pubblicare lo schema ed eseguire la migrazione del database, eseguire le operazioni seguenti: 

1. Pubblicare lo schema. Nel riquadro **SQL Server Metadata Explorer** fare clic con il pulsante destro del mouse sul database, quindi scegliere **Sincronizza con database**. Questa azione consente di pubblicare lo schema Oracle nell'istanza di SQL Server.

   ![Screenshot del comando "Sincronizza con database" nel riquadro SQL Server Metadata Explorer.](./media/oracle-to-sql-server/synchronize-database.png)

1. Esaminare il mapping tra il progetto di origine e la destinazione, come illustrato di seguito:

   ![Screenshot del riquadro "Sincronizza con il database" per la revisione del mapping del database.](./media/oracle-to-sql-server/synchronize-database-review.png)

1. Eseguire la migrazione dei dati. Nel riquadro **Oracle Metadata Explorer** fare clic con il pulsante destro del mouse sullo schema o sull'oggetto di cui si desidera eseguire la migrazione, quindi selezionare **Migrate data**. In alternativa, è possibile selezionare la scheda **migrazione dati** in alto a destra. 

   Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare la casella di controllo accanto alla tabella. Per omettere i dati dalle singole tabelle, deselezionare la casella di controllo.

   ![Screenshot dei collegamenti dati di migrazione.](./media/oracle-to-sql-server/migrate-data.png)

1. Nel riquadro **migra dati** immettere i dettagli della connessione per Oracle e SQL Server.

1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**.

    ![Screenshot del report di migrazione dei dati.](./media/oracle-to-sql-server/data-migration-report.png)

1. Connettersi all'istanza di SQL Server tramite [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms), quindi convalidare la migrazione esaminando i dati e lo schema.

   ![Screenshot di SQL Server Management Server.](./media/oracle-to-sql-server/validate-in-ssms.png)


Oltre a usare SSMA, è possibile usare SQL Server Integration Services (SSIS) per eseguire la migrazione dei dati. Per altre informazioni, vedere: 
- [Introduzione a SQL Server Integration Services](https://docs.microsoft.com//sql/integration-services/sql-server-integration-services) (articolo)
- [SQL Server Integration Services: SSIS per Azure e lo spostamento di dati ibridi](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx) (white paper tecnico)

## <a name="post-migration"></a>Post-migrazione 

Dopo aver completato la fase di *migrazione* , è necessario completare una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni

Dopo aver eseguito la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza utilizzano l'origine devono iniziare a utilizzare la destinazione. Per ottenere questo risultato, in alcuni casi sarà necessario apportare modifiche alle applicazioni.

[Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) è un'estensione per Visual Studio Code con cui è possibile analizzare il codice sorgente Java e rilevare le chiamate API e le query di accesso ai dati. Il Toolkit offre una visualizzazione a un singolo riquadro degli elementi da risolvere per supportare il nuovo back-end del database. Per altre informazioni, vedere il Blog [eseguire la migrazione dell'applicazione Java da Oracle](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727) . 

### <a name="perform-tests"></a>Eseguire test

L'approccio di test alla migrazione del database è costituito dalle attività seguenti:

1. **Sviluppare i test di convalida**: per testare la migrazione del database, è necessario usare le query SQL. È necessario creare le query di convalida per l'esecuzione nei database di origine e di destinazione. Le query di convalida devono coprire l'ambito definito.

1. **Configurare un ambiente di test**: l'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.

1. **Eseguire i test di convalida**: eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.

1. **Eseguire test delle prestazioni**: eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.


### <a name="optimize"></a>Ottimizzazione

La fase post-migrazione è fondamentale per riconciliare eventuali problemi di accuratezza dei dati, verificare la completezza e risolvere i problemi relativi alle prestazioni con il carico di lavoro.

Per ulteriori informazioni su questi problemi e sui passaggi per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md).


## <a name="migration-assets"></a>Risorse per la migrazione 

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere le risorse seguenti. Sono state sviluppate a supporto di un engagement di progetto di migrazione reale.


| Titolo | Descrizione |
| --- | --- |
| [Strumento e modello di valutazione dei carichi di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Fornisce le piattaforme di destinazione "Best Fit" suggerite, la preparazione per il cloud e i livelli di monitoraggio e aggiornamento dell'applicazione/database per i carichi di lavoro specificati. Offre un semplice calcolo con un solo clic e una generazione di report che consente di accelerare le valutazioni di grandi dimensioni fornendo un processo decisionale automatico e uniforme per la piattaforma di destinazione. |
| [Elementi di script di inventario Oracle](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts) | Include una query PL/SQL che raggiunge le tabelle di sistema Oracle e fornisce un conteggio degli oggetti in base al tipo di schema, al tipo di oggetto e allo stato. Fornisce inoltre una stima approssimativa dei dati non elaborati in ogni schema e il dimensionamento delle tabelle in ogni schema, con risultati archiviati in formato CSV. |
| [Automatizzare la raccolta SSMA Oracle Assessment & il consolidamento](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation) | Un set di risorse che usa un file CSV come voce (sources.csv nelle cartelle del progetto) per produrre i file XML necessari per eseguire SSMA assessment in modalità console. Il file di source.csv viene fornito dal cliente in base a un inventario delle istanze Oracle esistenti. I file di output sono AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml e VariableValueFile.xml. |
| [Problemi di SSMA e possibili soluzioni per la migrazione di database Oracle](https://aka.ms/dmj-wp-ssma-oracle-errors) | Viene illustrato come Oracle consente di assegnare una condizione non scalare nella clausola WHERE. Tuttavia, SQL Server non supporta questo tipo di condizione. Di conseguenza, SSMA per Oracle non converte le query con una condizione non scalare nella clausola WHERE, generando invece un errore O2SS0001. In questa white paper vengono fornite informazioni dettagliate sul problema e su come risolverlo. |
| [Manuale della migrazione da Oracle a SQL Server](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf) | Si concentra sulle attività associate alla migrazione di uno schema Oracle alla versione più recente di SQL Server base. Se la migrazione richiede modifiche alle caratteristiche e alle funzionalità, il possibile effetto di ogni modifica sulle applicazioni che utilizzano il database deve essere considerato attentamente. |

Le risorse sono state sviluppate dal team di progettazione di SQL Data. La carta di base di questo team consente di sbloccare e accelerare la modernizzazione complessa per i progetti di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure.

## <a name="next-steps"></a>Passaggi successivi

Dopo la migrazione, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per agevolare diversi scenari di migrazione di database e dati e attività speciali, vedere [Servizi e strumenti per la migrazione dei dati](/azure/dms/dms-tools-matrix).

Per altre guide alla migrazione, vedere [Guida alla migrazione del database di Azure](https://datamigration.microsoft.com/). 

Per i video sulla migrazione, vedere [Panoramica del percorso di migrazione](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).
