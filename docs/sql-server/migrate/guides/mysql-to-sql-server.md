---
title: 'Guida alla migrazione da MySQL a SQL Server:'
description: Questa guida illustra come eseguire la migrazione dei database MySQL a Microsoft SQL Server usando SQL Server Migration Assistant per MySQL (SSMA per MySQL).
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: ab682d07c27417b61b608090ad80c4f0eca5ebc0
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210952"
---
# <a name="migration-guide-mysql-to-sql-server"></a>Guida alla migrazione: da MySQL a SQL Server

[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

Questa guida illustra come eseguire la migrazione dei database MySQL a SQL Server.

Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare la migrazione del database MySQL a SQL Server:

- Verificare che l'ambiente di origine sia supportato. Attualmente sono supportati MySQL 5,6 e 5,7.
- Ottenere [SQL Server Migration Assistant per MySQL (SSMA per MySQL)](https://www.microsoft.com/download/details.aspx?id=54257).
- Ottenere connettività e autorizzazioni sufficienti per accedere sia all'origine che alla destinazione.

## <a name="pre-migration"></a>Pre-migrazione

Dopo aver soddisfatto i prerequisiti, si è pronti per individuare l'ambiente MySQL di origine e valutare la fattibilità della migrazione.

### <a name="assess"></a>Valutare

Usando SSMA per MySQL, è possibile esaminare i dati e gli oggetti di database e valutare i database per la migrazione.

Per creare una valutazione:

1. Aprire SSMA per MySQL.
1. Scegliere **Nuovo progetto** dal menu **File**.
1. Immettere il nome del progetto e un percorso per salvare il progetto e la destinazione della migrazione. Quindi selezionare **SQL Server** nell'opzione **migra a** .

   ![Screenshot che mostra il nuovo progetto.](./media/mysql-to-sql-server/new-project.png)

1. Nella finestra di dialogo **Connetti a MySQL** immettere i dettagli della connessione e quindi connettersi al server MySQL.

   ![Screenshot che mostra la connessione a MySQL.](./media/mysql-to-sql-server/connect-to-mysql.png)

1. Selezionare i database MySQL di cui si vuole eseguire la migrazione.

   ![Screenshot che mostra la selezione del database MySQL di cui si vuole eseguire la migrazione.](./media/mysql-to-sql-server/select-database.png)

1. Fare clic con il pulsante destro del mouse sul database MySQL in **MySQL Metadata Explorer** e scegliere **Crea report**. In alternativa, è possibile selezionare la scheda **Crea report** nell'angolo in alto a destra.

   ![Screenshot che mostra la creazione di report.](./media/mysql-to-sql-server/create-report.png)

1. Leggere il report HTML per esaminare le statistiche di conversione e gli eventuali errori o avvisi. È anche possibile aprire il report in Excel per ottenere un inventario degli oggetti MySQL e il lavoro richiesto per eseguire le conversioni dello schema. Il percorso predefinito per il report si trova nella cartella report in SSMAProjects, come illustrato di seguito:

    `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`.
   
   ![Screenshot che mostra un report di conversione.](./media/mysql-to-sql-server/conversion-report.png)

### <a name="validate-the-type-mappings"></a>Convalidare i mapping dei tipi

Verificare i mapping dei tipi di dati predefiniti e modificarli in base ai requisiti, se necessario. A tale scopo, procedere nel seguente modo:

1. Scegliere **Impostazioni progetto** dal menu **strumenti** .
1. Selezionare la scheda **mapping dei tipi** .

   ![Screenshot che mostra il mapping dei tipi.](./media/mysql-to-sql-server/type-mappings.png)

1. È possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **MySQL Metadata Explorer**.

Per altre informazioni sulle impostazioni di conversione in SSMA per MySQL, vedere [Impostazioni progetto](../../../ssma/mysql/project-settings-conversion-mysqltosql.md).

### <a name="convert-the-schema"></a>Convertire lo schema

La conversione di oggetti di database accetta le definizioni degli oggetti da MySQL, le converte in oggetti SQL Server simili e quindi carica queste informazioni nei metadati di SSMA per MySQL. Non carica le informazioni nell'istanza di SQL Server. È quindi possibile visualizzare gli oggetti e le relative proprietà utilizzando SQL Server Esplora metadati.

Durante la conversione, SSMA per MySQL Visualizza i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro **Elenco errori** . Usare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database MySQL o il processo di conversione per ottenere i risultati della conversione desiderati.

Per convertire lo schema:

1. Opzionale Per convertire le query dinamiche o ad hoc, fare clic con il pulsante destro del mouse sul nodo e scegliere **Aggiungi istruzione**.
1. Selezionare la scheda **Connetti a SQL Server** .
     1. Immettere i dettagli della connessione per l'istanza di SQL Server.
     1. Selezionare il database di destinazione dall'elenco a discesa oppure immettere un nuovo nome, nel qual caso verrà creato un database nel server di destinazione.
     1. Immettere i dettagli di autenticazione, quindi selezionare **Connetti**.

   ![Screenshot che mostra la connessione a SQL Server.](./media/mysql-to-sql-server/connect-to-sql-server.png)

1. Fare clic con il pulsante destro del mouse sul database MySQL in **MySQL Metadata Explorer**, quindi scegliere **Converti schema**. In alternativa, è possibile selezionare la scheda **Converti schema** nell'angolo superiore destro.

   ![Screenshot che mostra la conversione dello schema.](./media/mysql-to-sql-server/convert-schema.png)

1. Al termine della conversione, confrontare ed esaminare gli oggetti convertiti con gli oggetti originali per identificare i potenziali problemi e risolverli in base alle indicazioni.

   ![Screenshot che mostra il confronto e la revisione degli oggetti.](./media/mysql-to-sql-server/table-comparison.png)

1. Confrontare il testo Transact-SQL convertito con il codice originale ed esaminare le indicazioni.

   ![Screenshot che mostra il confronto e la revisione del codice convertito.](./media/mysql-to-sql-server/procedure-comparison.png)
   
1. Nel riquadro Output selezionare **Verifica risultati** ed esaminare gli errori nel riquadro **Elenco errori** .
1. Salvare il progetto in locale per un esercizio di correzione dello schema offline. Scegliere **Salva progetto** dal menu **file** . Questo passaggio consente di valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di pubblicare lo schema in SQL Server.

Per altre informazioni, vedere [screenshot che mostra la conversione dei database MySQL.](../../../ssma/mysql/converting-mysql-databases-mysqltosql.md)

## <a name="migration"></a>Migrazione

Una volta soddisfatti i prerequisiti necessari e aver completato le attività associate alla fase di *pre-migrazione* , si è pronti per eseguire la migrazione dello schema e dei dati.

Sono disponibili due opzioni per la migrazione dei dati:

- **Migrazione dei dati sul lato client**
     -   Per eseguire la migrazione dei dati sul lato client, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .

    > [!NOTE]
    > Quando si usa SQL Express Edition come database di destinazione, è consentita solo la migrazione dei dati sul lato client e la migrazione dei dati sul lato server non è supportata.

- **Migrazione dei dati sul lato server**
    -   Prima di eseguire la migrazione dei dati sul lato server, verificare quanto segue:
        - Il pacchetto di estensione SSMA per MySQL è installato nell'istanza di SQL Server.
        - Il servizio SQL Server Agent è in esecuzione nell'istanza di SQL Server.
    -   Per eseguire la migrazione dei dati sul lato server, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .

> [!IMPORTANT]  
> Se si prevede di usare il motore di migrazione dei dati lato server, prima di eseguire la migrazione dei dati, è necessario installare il pacchetto di estensione SSMA per MySQL e i provider MySQL nel computer che esegue SSMA per MySQL. È inoltre necessario che il servizio SQL Server Agent sia in esecuzione. Per altre informazioni su come installare il pacchetto di estensione, vedere [installazione di componenti SQL Server Migration Assistant in SQL Server (da MySQL a SQL)](../../../ssma/mysql/installing-ssma-components-on-sql-server-mysqltosql.md).

Per pubblicare lo schema ed eseguire la migrazione dei dati:

1. Pubblicare lo schema facendo clic con il pulsante destro del mouse sul database in **SQL Server Esplora metadati** e selezionando **Sincronizza con database**. Questa azione pubblica il database MySQL nell'istanza di SQL Server.

   ![Screenshot che mostra la sincronizzazione con il database.](./media/mysql-to-sql-server/synchronize-database.png)

1. Esaminare il mapping tra il progetto di origine e la destinazione.

   ![Screenshot che mostra la revisione della sincronizzazione con il database.](./media/mysql-to-sql-server/synchronize-database-review.png)

1. Migrare i dati facendo clic con il pulsante destro del mouse sul database o sull'oggetto di cui si vuole eseguire la migrazione in **MySQL Metadata Explorer** e selezionando **Migrate data**. In alternativa, è possibile selezionare la scheda **Migrate data** . Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare le caselle di controllo accanto alle tabelle. Per omettere i dati dalle singole tabelle, deselezionare le caselle di controllo.

   ![Screenshot che mostra la migrazione dei dati.](./media/mysql-to-sql-server/migrate-data.png)

1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**.

   ![Screenshot che mostra il report di migrazione dei dati.](./media/mysql-to-sql-server/migration-report.png)

1. Connettersi all'istanza di SQL Server utilizzando [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)e convalidare la migrazione riesaminando i dati e lo schema.

   ![Screenshot che mostra la convalida in SQL Server Management Studio.](./media/mysql-to-sql-server/validate-in-ssms.png)

## <a name="post-migration"></a>Post-migrazione

Dopo aver completato la fase di *migrazione* , è necessario completare una serie di attività post-migrazione per assicurarsi che tutto funzioni correttamente nel modo più semplice ed efficiente possibile.

### <a name="remediate-applications"></a>Correggere le applicazioni

Dopo aver eseguito la migrazione dei dati nell'ambiente di destinazione, tutte le applicazioni che in precedenza utilizzano l'origine devono iniziare a utilizzare la destinazione. Per eseguire questa attività, in alcuni casi è necessario apportare modifiche alle applicazioni.

### <a name="perform-tests"></a>Eseguire test

L'approccio di test per la migrazione del database prevede le attività seguenti:

1. **Sviluppare i test di convalida**: per testare la migrazione del database, è necessario usare query SQL. È necessario creare le query di convalida da eseguire sia sul database di origine che su quello di destinazione. Le query di convalida devono coprire l'ambito definito.
1. **Configurare un ambiente di test**: l'ambiente di test deve contenere una copia del database di origine e del database di destinazione. Assicurarsi di isolare l'ambiente di test.
1. **Eseguire i test di convalida**: eseguire i test di convalida sull'origine e sulla destinazione, quindi analizzare i risultati.
1. **Eseguire test delle prestazioni**: eseguire test delle prestazioni sull'origine e sulla destinazione, quindi analizzare e confrontare i risultati.

### <a name="optimize"></a>Ottimizzazione

La fase post-migrazione è fondamentale per riconciliare eventuali problemi di accuratezza dei dati, verificare la completezza e risolvere i problemi relativi alle prestazioni con il carico di lavoro.

> [!Note]
> Per ulteriori informazioni su questi problemi e sui passaggi per attenuarli, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](../../../relational-databases/post-migration-validation-and-optimization-guide.md).

## <a name="migration-assets"></a>Risorse per la migrazione

Per ulteriori informazioni sul completamento di questo scenario di migrazione, vedere la seguente risorsa. È stata sviluppata in supporto di un impegno di progetto di migrazione reale.

| Titolo                    | Descrizione            |
| ----------------------------- | ---------------------- |
| [Strumento e modello di valutazione del carico di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Questo strumento fornisce le piattaforme di destinazione consigliate, la preparazione per il cloud e il livello di monitoraggio e aggiornamento dell'applicazione o del database per un determinato carico di lavoro. Offre semplici calcoli con un solo clic e la generazione di report che consentono di accelerare le valutazioni di grandi dimensioni fornendo un processo decisionale di piattaforma di destinazione automatizzato e uniforme.                |

Le risorse sono state sviluppate dal team di progettazione di SQL Data. La carta di base di questo team consente di sbloccare e accelerare la modernizzazione complessa per i progetti di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure.


## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni sulla migrazione di database MySQL a SQL Server, vedere [la documentazione SQL Server Migration Assistant per MySQL](../../../ssma/mysql/sql-server-migration-assistant-for-mysql-mysqltosql.md).
- Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per agevolare diversi scenari di migrazione di database e dati e attività speciali, vedere [Servizi e strumenti per la migrazione dei dati](https://docs.microsoft.com/azure/dms/dms-tools-matrix).
- Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://datamigration.microsoft.com/).

