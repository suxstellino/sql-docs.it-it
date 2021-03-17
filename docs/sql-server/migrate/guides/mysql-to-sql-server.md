---
title: Migrare MySQL a SQL Server
description: 'Questa guida illustra come eseguire la migrazione dei database MySQL a Microsoft SQL Server usando la migrazione SQL Server per MySQL (SSMA per MySQL). '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: f944cca6674abf9488729c945dbd4a53aee9ef27
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603795"
---
# <a name="migration-guide-mysql-to-sql-server"></a>Guida alla migrazione: da MySQL a SQL Server

Questa guida consente di migrare i database MySQL in SQL Server. 

Per altre guide alla migrazione, vedere [Migrazione dei database](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Prerequisiti 

Per eseguire la migrazione del database MySQL a SQL Server, è necessario:

- Connettività al database MySQL di origine per eseguire la valutazione e la conversione.
- [SQL Server Migration Assistant per MySQL](https://aka.ms/ssmaformysql).

## <a name="pre-migration"></a>Pre-migrazione 

Dopo aver soddisfatto i prerequisiti, si è pronti per individuare l'ambiente MySQL di origine e valutare la fattibilità della migrazione.
Prima di iniziare la migrazione tramite SSMA, è necessario:

1.  Creare un nuovo progetto.  
2.  Connettersi a un database MySQL.
3.  Una volta completata la connessione, gli schemi MySQL verranno visualizzati in MySQL Metadata Explorer. Fare clic con il pulsante destro del mouse su oggetti in MySQL Metadata Explorer per eseguire attività quali la creazione di report per la valutazione delle conversioni in SQL Server.

### <a name="assess"></a>Valutare 


Per usare [SSMA per MySQL](https://aka.ms/ssmaformysql) per creare una valutazione, seguire questa procedura: 

1. Aprire SQL Server Migration Assistant per MySQL. 
1. Selezionare **file** e scegliere **nuovo progetto**. Specificare il nome del progetto, un percorso in cui salvare il progetto e la destinazione della migrazione.
1. Selezionare **SQL Server** nell'opzione **Esegui migrazione a** . 
1. Specificare i dettagli della connessione nella finestra di dialogo **Connetti a MySQL** e connettersi al server MySQL. 
1. Fare clic con il pulsante destro del mouse sullo schema MySQL in **MySQL Metadata Explorer** e scegliere **Crea report**. In alternativa, è possibile selezionare **Crea report** dalla barra di spostamento in alto a linee. 
1. Esaminare il report HTML con statistiche di conversione, errori e avvisi. Analizzare il report per comprendere i problemi di conversione e le relative risoluzioni. 

   È inoltre possibile accedere a questo report dalla cartella SSMA Projects come selezionato nella prima schermata. Dall'esempio precedente, individuare il file di report.xml da:

   `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`
 
   e aprirlo in Excel per ottenere un inventario degli oggetti MySQL e lo sforzo necessario per eseguire le conversioni dello schema.

### <a name="validate-type-mappings"></a>Convalida mapping dei tipi

Prima di eseguire la conversione dello schema, convalidare i mapping del tipo di dati predefiniti o modificarli in base ai requisiti. Questa operazione può essere eseguita passando al menu **strumenti** e scegliendo **Impostazioni progetto** oppure è possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **MySQL Metadata Explorer**.

Per altre informazioni sulle impostazioni di conversione in SSMA, vedere [Impostazioni progetto](../../../ssma/mysql/project-settings-conversion-mysqltosql.md)

### <a name="convert-schema"></a>Converti schema

La conversione di oggetti di database accetta le definizioni degli oggetti da MySQL, le converte in oggetti SQL Server simili e quindi carica tali informazioni nei metadati SSMA. Non carica le informazioni nell'istanza di SQL Server. È quindi possibile visualizzare gli oggetti e le relative proprietà utilizzando Esplora metadati SQL Server.

Durante la conversione, SSMA stampa i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro Elenco errori. Usare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database MySQL o il processo di conversione per ottenere i risultati della conversione desiderati.

Per convertire lo schema, seguire questa procedura:

1. Opzionale Per convertire le query dinamiche o ad hoc, fare clic con il pulsante destro del mouse sul nodo e scegliere **Aggiungi istruzione**. 
1. Selezionare **Connetti a SQL Server** nella barra di spostamento in alto a linee e specificare SQL Server dettagli. È possibile scegliere di connettersi a un database esistente o specificare un nuovo nome, nel qual caso verrà creato un database nel server di destinazione.
1. Fare clic con il pulsante destro del mouse sullo schema MySQL in **MySQL Metadata Explorer** e scegliere **Converti schema**. In alternativa, è possibile selezionare **Converti schema** dalla barra di spostamento in alto a linee. 
1. Confrontare ed esaminare la struttura dello schema per identificare i potenziali problemi. 

   Dopo la conversione dello schema è possibile salvare il progetto localmente per un esercizio di correzione dello schema offline. Scegliere **Salva progetto** dal menu **File**. In questo modo è possibile valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di poter pubblicare lo schema in SQL Server.

Per altre informazioni, vedere [conversione di database MySQL](../../../ssma/mysql/converting-mysql-databases-mysqltosql.md)

## <a name="migration"></a>Migrazione 

Una volta soddisfatti i prerequisiti necessari e aver completato le attività associate alla fase di **pre-migrazione** , si è pronti per eseguire la migrazione dello schema e dei dati.

Per eseguire la migrazione dei dati, si verificano due casi:

- **Migrazione dei dati sul lato client:**
     -   Per eseguire la  **migrazione dei dati lato client**, selezionare l'opzione  **motore di migrazione dati lato client**  nella finestra di dialogo  **Impostazioni progetto**  .

    > [!NOTE]
    > Quando si usa SQL Express Edition come database di destinazione, è consentita solo la migrazione dei dati lato client e la migrazione dei dati sul lato server non è supportata.

- **Migrazione dei dati lato server:**
    -   Prima di eseguire la migrazione dei dati sul lato server, verificare quanto segue:
        - Il pacchetto di estensione SSMA per MySQL è installato nell'istanza di SQL Server.
        - Il servizio SQL Server Agent è in esecuzione nell'istanza di SQL Server. 
    -   Per eseguire la  **migrazione dei dati sul lato server**, selezionare l'opzione  **motore di migrazione dei dati sul lato server**  nella finestra di dialogo  **Impostazioni progetto**  .

> [!IMPORTANT]  
> Se il motore usato è il motore di migrazione dei dati lato server, prima di eseguire la migrazione dei dati, è necessario installare il pacchetto di estensione SSMA per MySQL e i provider MySQL nel computer che esegue SSMA. È inoltre necessario che il servizio SQL Server Agent sia in esecuzione. Per ulteriori informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti SSMA in SQL Server (da MySQL a SQL)](../../../ssma/mysql/installing-ssma-components-on-sql-server-mysqltosql.md) .  

Per pubblicare lo schema ed eseguire la migrazione dei dati, attenersi alla procedura seguente: 

1. Fare clic con il pulsante destro del mouse sul database in **SQL Server Esplora metadati** e scegliere **Sincronizza con database**.  Questa azione pubblica lo schema MySQL nell'istanza di SQL Server.
1. Fare clic con il pulsante destro del mouse sullo schema MySQL in **MySQL Metadata Explorer** e scegliere **Migrate data**.  In alternativa, è possibile selezionare **migrare i dati** dalla barra di spostamento in alto a linea.  
1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**: 
1. Convalidare la migrazione esaminando i dati e lo schema nell'istanza SQL Server usando SQL Server Management Studio (SSMS).

## <a name="post-migration"></a>Post-migrazione 

Dopo aver completato la fase di **migrazione** , è necessario eseguire una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni

Dopo la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza usavano l'origine devono iniziare a usare la destinazione. Per ottenere questo risultato, in alcuni casi sarà necessario apportare modifiche alle applicazioni.

### <a name="perform-tests"></a>Eseguire test

L'approccio di test per la migrazione del database consiste nell'eseguire le attività seguenti:

1. **Sviluppare i test di convalida**. Per testare la migrazione del database, è necessario usare le query SQL. È necessario creare le query di convalida da eseguire sia sul database di origine che su quello di destinazione. Le query di convalida devono essere estese all'ambito definito.

2. **Configurare l'ambiente di test**. L'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.

3. **Eseguire i test di convalida**. Eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.

4. **Eseguire test delle prestazioni**. Eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.


### <a name="optimize"></a>Ottimizzazione

La fase post-migrazione è fondamentale per riconciliare eventuali problemi di accuratezza dei dati e verificare la completezza, nonché per risolvere i problemi di prestazioni del carico di lavoro.

> [!NOTE] 
> Per ulteriori dettagli su questi problemi e su passaggi specifici per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](https://docs.microsoft.com/sql/relational-databases/post-migration-validation-and-optimization-guide).

## <a name="migration-assets"></a>Risorse per la migrazione 

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere le risorse seguenti, che sono state sviluppate a supporto di un progetto di migrazione del mondo reale.

| Titolo/collegamento                    | Descrizione            |
| ----------------------------- | ---------------------- |
| [Strumento e modello di valutazione del carico di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Questo strumento fornisce le piattaforme di destinazione "più idonee" suggerite, la preparazione per il cloud e il livello di monitoraggio e aggiornamento dell'applicazione/database per un determinato carico di lavoro. Offre semplici calcoli con un solo clic e generazione di report che consentono di accelerare le valutazioni di grandi dimensioni grazie a un processo decisionale automatizzato e uniforme della piattaforma di destinazione.                |

Queste risorse sono state sviluppate come parte del programma Data SQL Ninja, sponsorizzato dal team di progettazione Azure Data Group. Obiettivo principale del programma Data SQL Ninja è rendere disponibili opportunità per velocizzare progetti di modernizzazione complessi e la migrazione delle piattaforme dati alla piattaforma dati di Microsoft Azure. Se si ritiene che l'organizzazione possa essere interessata a partecipare al programma Data SQL Ninja, contattare il team dell'account per richiedere l'invio di una candidatura.

## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni sulla migrazione di database MySQL a SQL Server, vedere [la documentazione di SSMA per MySQL](../../../ssma/mysql/sql-server-migration-assistant-for-mysql-mysqltosql.md)

- Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per supportare diversi scenari di migrazione di database e dati, nonché per le attività speciali, vedere l'articolo [servizio e strumenti per la migrazione dei dati](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

- Per altre guide alla migrazione, vedere [Migrazione dei database](https://datamigration.microsoft.com/). 

