---
title: 'Guida alla migrazione da IBM DB2 a SQL Server:'
description: Questa guida illustra come eseguire la migrazione dei database DB2 a Microsoft SQL Server usando SQL Server Migration Assistant (SSMA) per DB2.
ms.custom: ''
ms.date: 03/19/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 994d78b9e1eec46659e97c114d99355ccf4a974b
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981055"
---
# <a name="migration-guide-ibm-db2-to-sql-server"></a>Guida alla migrazione: IBM DB2 per SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

In questa guida si apprenderà come eseguire la migrazione dei database utente da IBM DB2 a SQL Server tramite SQL Server Migration Assistant (SSMA) per DB2. 

Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://docs.microsoft.com/data-migration). 


## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare la migrazione del database DB2 a SQL Server, procedere come segue:

- Verificare che l' [ambiente di origine sia supportato](../../../ssma/db2/installing-ssma-for-db2-client-db2tosql.md#prerequisites).
- Scaricare e installare [SSMA per DB2](https://www.microsoft.com/download/details.aspx?id=54254).
- Ottenere connettività e autorizzazioni sufficienti per accedere sia all'origine che alla destinazione. 

## <a name="pre-migration"></a>Pre-migrazione

Dopo aver soddisfatto i prerequisiti, si è pronti per individuare la topologia dell'ambiente e valutare la fattibilità della migrazione. 

### <a name="assess-and-convert"></a>Valutazione e conversione 

Usare SSMA per DB2 per esaminare i dati e gli oggetti di database e valutare i database per la migrazione. 

Per creare una valutazione, procedere come segue:

1. Aprire SSMA per DB2. 
1. Selezionare **file**, quindi fare clic su **nuovo progetto**. 
1. Specificare un nome e un percorso per il progetto e quindi nell'elenco a discesa selezionare un SQL Server destinazione della migrazione. Selezionare **OK**.

   :::image type="content" source="media/db2-to-sql-server/new-project.png" alt-text="Screenshot del riquadro nuovo progetto in SSMA per DB2.":::


1. Selezionare **Connetti a DB2**, quindi immettere i dettagli della connessione DB2.

   :::image type="content" source="media/db2-to-sql-server/connect-to-db2.png" alt-text="Screenshot del riquadro Connetti a DB2.":::


1. Fare clic con il pulsante destro del mouse sullo schema DB2 di cui si desidera eseguire la migrazione e quindi scegliere **Crea report** per generare un report HTML. In alternativa, è possibile selezionare **Crea report** in alto a destra.

   :::image type="content" source="media/db2-to-sql-server/create-report.png" alt-text="Screenshot dei collegamenti ' Crea report ' in DB2 Metadata Explorer.":::

1. Esaminare il report HTML per comprendere le statistiche di conversione e gli eventuali errori o avvisi. È inoltre possibile aprire il report in Excel per ottenere un inventario degli oggetti DB2 e lo sforzo necessario per eseguire le conversioni dello schema. Il percorso predefinito per il report si trova nella cartella report in SSMAProjects, come illustrato di seguito:  

   `drive:\<username>\Documents\SSMAProjects\MyDb2Migration\report\report_<date>` 

   :::image type="content" source="media/db2-to-sql-server/report.png" alt-text="Screenshot di un report di conversione in SSMA.":::


### <a name="validate-data-types"></a>Convalidare i tipi di dati

Convalidare i mapping del tipo di dati predefiniti e modificarli in base ai requisiti, se necessario. A tale scopo, procedere nel seguente modo: 

1. Selezionare **strumenti**, quindi selezionare **Impostazioni progetto**.  
1. Selezionare la scheda **mapping dei tipi** .

   :::image type="content" source="media/db2-to-sql-server/type-mapping.png" alt-text="Screenshot del riquadro &quot;mapping dei tipi&quot; in SSMA per DB2.":::

1. È possibile modificare il mapping dei tipi per ogni tabella selezionando il nome della tabella nel riquadro **DB2 Metadata Explorer** . 

### <a name="convert-schema"></a>Converti schema 

Per convertire lo schema, procedere come segue:

1. Opzionale Per convertire query dinamiche o specializzate, fare clic con il pulsante destro del mouse sul nodo, quindi scegliere **Aggiungi istruzione**. 

1. Selezionare la scheda **Connetti a SQL Server** , quindi immettere i dettagli della connessione per l'istanza di SQL Server. 

    a. Nell'elenco a discesa **database** selezionare il database di destinazione o specificare un nuovo nome per creare un database nel server di destinazione.   
    b. Fornire i dettagli di autenticazione.   
    c. Selezionare **Connetti**.

   :::image type="content" source="media/db2-to-sql-server/connect-to-sql-server.png" alt-text="Screenshot del riquadro Connetti a SQL Server in SSMA per DB2.":::

1. Fare clic con il pulsante destro del mouse sullo schema che si sta utilizzando, quindi scegliere **Converti schema**. In alternativa, è possibile selezionare la scheda **Converti schema** in alto a destra.

   :::image type="content" source="media/db2-to-sql-server/convert-schema.png" alt-text="Screenshot del comando &quot;Convert schema&quot; nel riquadro &quot;DB2 Metadata Explorer&quot;.":::

1. Al termine della conversione, confrontare la struttura convertita con la struttura originale per identificare i potenziali problemi e risolverli in base alle indicazioni.

   :::image type="content" source="media/db2-to-sql-server/compare-review-schema-structure.png" alt-text="Screenshot che mostra un confronto tra gli oggetti convertiti e gli oggetti originali.":::

1. Nel riquadro Output selezionare l'icona **Verifica risultati** , quindi esaminare gli eventuali errori nel riquadro **Elenco errori** . 
1. Per un esercizio di correzione dello schema offline, salvare il progetto localmente selezionando **file**  >  **Salva progetto**. In questo modo è possibile valutare gli schemi di origine e di destinazione offline e risolverli prima di pubblicare lo schema nell'istanza di SQL Server.



## <a name="migrate"></a>Migrate

Al termine della valutazione dei database e della risoluzione di eventuali discrepanze, il passaggio successivo consiste nell'eseguire il processo di migrazione.

Per pubblicare lo schema ed eseguire la migrazione dei dati, eseguire le operazioni seguenti:

1. Pubblicare lo schema. Nel riquadro **SQL Server Metadata Explorer** fare clic con il pulsante destro del mouse sul database, quindi scegliere **Sincronizza con database**.

   :::image type="content" source="media/db2-to-sql-server/synchronize-with-database.png" alt-text="Screenshot del comando ' Sincronizza con database ' nel riquadro SQL Server Metadata Explorer.":::

1. Eseguire la migrazione dei dati. Nel riquadro **DB2 Metadata Explorer** fare clic con il pulsante destro del mouse sullo schema o sull'oggetto di cui si desidera eseguire la migrazione, quindi selezionare **Migrate data**. In alternativa, è possibile selezionare la scheda **migrazione dati** in alto a destra.
 
   Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare la casella di controllo accanto alla tabella. Per omettere i dati dalle singole tabelle, deselezionare la casella di controllo.

   :::image type="content" source="media/db2-to-sql-server/migrate-data.png" alt-text="Screenshot dei collegamenti dati di migrazione.":::

1. Specificare i dettagli della connessione per le istanze di DB2 e SQL Server. 
1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**.  

   :::image type="content" source="media/db2-to-sql-server/data-migration-report.png" alt-text="Screenshot del report di migrazione dei dati.":::

1. Connettersi all'istanza di SQL Server tramite [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms), quindi convalidare la migrazione esaminando i dati e lo schema. 

   :::image type="content" source="media/db2-to-sql-server/compare-schema-in-ssms.png" alt-text="Screenshot di SQL Server Management Server.":::

## <a name="post-migration"></a>Post-migrazione 

Dopo aver completato la fase di *migrazione* , è necessario completare una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni 

Dopo aver eseguito la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza utilizzano l'origine devono iniziare a utilizzare la destinazione. Per ottenere questo risultato, in alcuni casi sarà necessario apportare modifiche alle applicazioni.

### <a name="perform-tests"></a>Eseguire test

L'approccio di test alla migrazione del database è costituito dalle attività seguenti:

1. **Sviluppare i test di convalida**: per testare la migrazione del database, è necessario usare le query SQL. È necessario creare le query di convalida per l'esecuzione nei database di origine e di destinazione. Le query di convalida devono coprire l'ambito definito.

1. **Configurare un ambiente di test**: l'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.

1. **Eseguire i test di convalida**: eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.

1. **Eseguire test delle prestazioni**: eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.


## <a name="migration-assets"></a>Risorse per la migrazione 

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere le risorse seguenti. Sono state sviluppate a supporto di un engagement di progetto di migrazione reale.

| Titolo | Descrizione |
| --- | --- |
| [Strumento e modello di valutazione dei carichi di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Fornisce le piattaforme di destinazione "Best Fit" suggerite, la preparazione per il cloud e i livelli di monitoraggio e aggiornamento dell'applicazione/database per i carichi di lavoro specificati. Offre un semplice calcolo con un solo clic e una generazione di report che consente di accelerare le valutazioni di grandi dimensioni fornendo un processo decisionale automatico e uniforme per la piattaforma di destinazione. |
| [Pacchetto di individuazione e valutazione degli asset di dati zOS per IBM DB2](https://github.com/Microsoft/DataMigrationTeam/tree/master/Db2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package) | Dopo aver eseguito lo script SQL in un database, è possibile esportare i risultati in un file nel file system. Sono supportati diversi formati di file, incluso CSV, in modo che sia possibile acquisire i risultati in strumenti esterni, ad esempio fogli di calcolo. Questo metodo può essere utile se si vogliono condividere facilmente i risultati con i team che non hanno installato il workbench.|
| [Script e artefatti di inventario per IBM DB2 LUW](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20Db2%20LUW%20Inventory%20Scripts%20and%20Artifacts) | Include una query SQL che raggiunge le tabelle di sistema IBM DB2 LUW versione 11,1 e fornisce un conteggio degli oggetti in base al tipo di schema e di oggetto, una stima approssimativa di "dati non elaborati" in ogni schema e il dimensionamento delle tabelle in ogni schema, con risultati archiviati in formato CSV. |
| [Guida all'installazione di IBM DB2 LUW pure scale in Azure](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Db2%20PureScale%20on%20Azure.pdf) | Funge da punto di partenza per un piano di implementazione di IBM DB2. Sebbene i requisiti aziendali differiscano, viene applicato lo stesso modello di base. Questo modello di architettura può essere usato anche per le applicazioni OLAP in Azure. |

Le risorse sono state sviluppate dal team di progettazione di SQL Data. La carta di base di questo team consente di sbloccare e accelerare la modernizzazione complessa per i progetti di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure.

## <a name="next-steps"></a>Passaggi successivi

Dopo la migrazione, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per agevolare diversi scenari di migrazione di database e dati e attività speciali, vedere [Servizi e strumenti per la migrazione dei dati](/azure/dms/dms-tools-matrix).

Per altre guide alla migrazione, vedere [Guida alla migrazione del database di Azure](https://datamigration.microsoft.com/). 

Per i video sulla migrazione, vedere [Panoramica del percorso di migrazione](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).
