---
title: 'Guida alla migrazione da MySQL a SQL Server:'
description: 'Questa guida illustra come eseguire la migrazione dei database MySQL a Microsoft SQL Server usando la migrazione SQL Server per MySQL (SSMA per MySQL). '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 2befdaec3602634faaa2017ae2ca225a938cfab3
ms.sourcegitcommit: 2f971c85d87623c0aed1612406130d840e7bdb2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2021
ms.locfileid: "105744498"
---
# <a name="migration-guide-mysql-to-sql-server"></a>Guida alla migrazione: da MySQL a SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

Questa guida consente di migrare i database MySQL in SQL Server. 

Per altre guide alla migrazione, vedere [Migrazione dei database](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Prerequisiti 

Per eseguire la migrazione del database MySQL a SQL Server, è necessario:

- Per verificare che l'ambiente di origine sia supportato. Attualmente sono supportati MySQL 5,6 e 5,7. 
- [SQL Server Migration Assistant per MySQL](https://www.microsoft.com/download/details.aspx?id=54257).
- Connettività e autorizzazioni sufficienti per accedere sia all'origine che alla destinazione. 

## <a name="pre-migration"></a>Pre-migrazione 

Dopo aver soddisfatto i prerequisiti, si è pronti per individuare l'ambiente MySQL di origine e valutare la fattibilità della migrazione.

### <a name="assess"></a>Valutare 

Usare SQL Server Migration Assistant (SSMA) per MySQL per esaminare i dati e gli oggetti di database e valutare i database per la migrazione.

Per creare una valutazione, seguire questa procedura: 

1. Aprire SQL Server Migration Assistant per MySQL. 
1. Selezionare **file** e scegliere **nuovo progetto**. 
1. Specificare il nome del progetto, un percorso in cui salvare il progetto e la destinazione della migrazione. Selezionare **SQL Server** nell'opzione **Esegui migrazione a** :

   ![Nuovo progetto](./media/mysql-to-sql-server/new-project.png)

1. Specificare i dettagli della connessione nella finestra di dialogo **Connetti a MySQL** e connettersi al server MySQL:

   ![Connettersi a MySQL](./media/mysql-to-sql-server/connect-to-mysql.png)

1. Selezionare i database MySQL di cui si vuole eseguire la migrazione:

   ![Selezionare il database MySQL di cui si vuole eseguire la migrazione](./media/mysql-to-sql-server/select-database.png)

1. Fare clic con il pulsante destro del mouse sul database MySQL in **MySQL Metadata Explorer** e scegliere **Crea report**. In alternativa, è possibile selezionare **Crea report** dalla barra di spostamento in alto a linee:

   ![Creazione di report](./media/mysql-to-sql-server/create-report.png)

1. Leggere il report HTML per esaminare le statistiche di conversione e gli eventuali errori o avvisi. È anche possibile aprire il report in Excel per ottenere un inventario degli oggetti MySQL e il lavoro richiesto per eseguire le conversioni dello schema. La posizione predefinita del report è la cartella report all'interno di SSMAProjects. 

   ad esempio `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\` 
   
   ![Report di conversione](./media/mysql-to-sql-server/conversion-report.png)

### <a name="validate-type-mappings"></a>Convalida mapping dei tipi

Convalidare i mapping dei tipi di dati predefiniti e modificarli in base ai requisiti, se necessario. A questo scopo, attenersi alla procedura seguente: 

1. Selezionare **Tools** (Strumenti) dal menu. 
1. Selezionare **Project Settings** (Impostazioni progetto). 
1. Selezionare la scheda **mapping dei tipi** :

   ![Mapping dei tipi](./media/mysql-to-sql-server/type-mappings.png)

1. È possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **MySQL Metadata Explorer**. 


Per altre informazioni sulle impostazioni di conversione in SSMA, vedere [Impostazioni progetto](../../../ssma/mysql/project-settings-conversion-mysqltosql.md).

### <a name="convert-schema"></a>Converti schema

La conversione di oggetti di database accetta le definizioni degli oggetti da MySQL, le converte in oggetti SQL Server simili e quindi carica tali informazioni nei metadati SSMA. Non carica le informazioni nell'istanza di SQL Server. È quindi possibile visualizzare gli oggetti e le relative proprietà utilizzando Esplora metadati SQL Server.

Durante la conversione, SSMA stampa i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro Elenco errori. Usare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database MySQL o il processo di conversione per ottenere i risultati della conversione desiderati.

Per convertire lo schema, seguire questa procedura:

1. Opzionale Per convertire le query dinamiche o ad hoc, fare clic con il pulsante destro del mouse sul nodo e scegliere **Aggiungi istruzione**. 
1. Selezionare **Connetti a SQL Server** dalla barra di spostamento in alto a linee. 
     1. Immettere i dettagli della connessione per l'istanza di SQL Server. 
     1. Scegliere il database di destinazione dall'elenco a discesa o specificare un nuovo nome, nel qual caso verrà creato un database nel server di destinazione. 
     1. Fornire i dettagli di autenticazione. 
     1. Selezionare **Connetti**:

   ![Connetti a SQL](./media/mysql-to-sql-server/connect-to-sql-server.png)

1. Fare clic con il pulsante destro del mouse sul database MySQL in **MySQL Metadata Explorer** e scegliere **Converti schema**. In alternativa, è possibile selezionare **Converti schema** dalla barra di spostamento in alto a linee:

   ![Converti schema](./media/mysql-to-sql-server/convert-schema.png)

1. Al termine della conversione, confrontare ed esaminare gli oggetti convertiti con gli oggetti originali per identificare i potenziali problemi e risolverli in base alle indicazioni:

   ![Confrontare ed esaminare l'oggetto](./media/mysql-to-sql-server/table-comparison.png)

   Confrontare il testo Transact-SQL convertito con il codice originale ed esaminare le indicazioni:

   ![Confrontare ed esaminare il codice convertito](./media/mysql-to-sql-server/procedure-comparison.png)
   
1. Selezionare **Verifica risultati** nel riquadro Output ed esaminare gli errori nel riquadro **Elenco errori** . 
1. Salvare il progetto in locale per un esercizio di correzione dello schema offline. Scegliere **Salva progetto** dal menu **File**. In questo modo è possibile valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di poter pubblicare lo schema in SQL Server.

Per altre informazioni, vedere [conversione di database MySQL](../../../ssma/mysql/converting-mysql-databases-mysqltosql.md).

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

1. Pubblicare lo schema: fare clic con il pulsante destro del mouse sul database in **SQL Server Esplora metadati** e scegliere **Sincronizza con database**.  Questa azione pubblica il database MySQL nell'istanza SQL Server:

   ![Sincronizza con database](./media/mysql-to-sql-server/synchronize-database.png)

   Esaminare il mapping tra il progetto di origine e la destinazione:

   ![Sincronizza con database-revisione](./media/mysql-to-sql-server/synchronize-database-review.png)

1. Migrare i dati: fare clic con il pulsante destro del mouse sul database o sull'oggetto di cui si vuole eseguire la migrazione in **MySQL Metadata Explorer** e scegliere **Migrate data**. In alternativa, è possibile selezionare **migrare i dati** dalla barra di spostamento in alto a linea. Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere tabelle, quindi selezionare la casella di controllo accanto alla tabella. Per omettere i dati dalle singole tabelle, deselezionare la casella di controllo:

   ![Migrazione dei dati](./media/mysql-to-sql-server/migrate-data.png)

1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**: 

   ![Report di migrazione dati](./media/mysql-to-sql-server/migration-report.png)

1. Connettersi all'istanza di SQL Server utilizzando [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) e convalidare la migrazione riesaminando i dati e lo schema: 

   ![Convalida in SSMA](./media/mysql-to-sql-server/validate-in-ssms.png)



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

> [!Note]
> Per ulteriori dettagli su questi problemi e su passaggi specifici per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md).

## <a name="migration-assets"></a>Risorse per la migrazione 

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere le risorse seguenti, che sono state sviluppate a supporto di un progetto di migrazione del mondo reale.

| Titolo/collegamento                    | Descrizione            |
| ----------------------------- | ---------------------- |
| [Strumento e modello di valutazione del carico di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Questo strumento fornisce le piattaforme di destinazione "più idonee" suggerite, la preparazione per il cloud e il livello di monitoraggio e aggiornamento dell'applicazione/database per un determinato carico di lavoro. Offre semplici calcoli con un solo clic e generazione di report che consentono di accelerare le valutazioni di grandi dimensioni grazie a un processo decisionale automatizzato e uniforme della piattaforma di destinazione.                |

Le risorse sono state sviluppate dal team di progettazione di SQL Data. La carta di base di questo team consente di sbloccare e accelerare la modernizzazione complessa per i progetti di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure.

## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni sulla migrazione di database MySQL a SQL Server, vedere [la documentazione di SSMA per MySQL](../../../ssma/mysql/sql-server-migration-assistant-for-mysql-mysqltosql.md).

- Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per supportare diversi scenari di migrazione di database e dati, nonché per le attività speciali, vedere l'articolo [servizio e strumenti per la migrazione dei dati](https://docs.microsoft.com/azure/dms/dms-tools-matrix).

- Per altre guide alla migrazione, vedere [Migrazione dei database](https://datamigration.microsoft.com/). 

