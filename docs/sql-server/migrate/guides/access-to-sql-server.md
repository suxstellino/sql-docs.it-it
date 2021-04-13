---
title: 'Accesso a SQL Server: Guida alla migrazione'
description: Questa guida illustra come eseguire la migrazione dei database di Microsoft Access a Microsoft SQL Server usando SQL Server Migration Assistant per l'accesso (SSMA per l'accesso).
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 11c604a2e50b21138419e5c2d5a6f213f8fc5559
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210967"
---
# <a name="migration-guide-access-to-sql-server"></a>Guida alla migrazione: accesso a SQL Server

[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

In questa guida si apprenderà come eseguire la migrazione dei database di Microsoft Access a SQL Server usando SQL Server Migration Assistant per l'accesso (SSMA per l'accesso).

Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://datamigration.microsoft.com/).

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare la migrazione del database di Access a SQL Server:

- Verificare che l'ambiente di origine sia supportato.
- Ottenere [SSMA per l'accesso](https://www.microsoft.com/download/details.aspx?id=54255).
- Ottenere connettività e autorizzazioni sufficienti per accedere sia all'origine che alla destinazione.

## <a name="pre-migration"></a>Pre-migrazione

Dopo aver soddisfatto i prerequisiti, si è pronti per individuare la topologia dell'ambiente e valutare la fattibilità della migrazione.

### <a name="assess"></a>Valutare

Utilizzando SSMA per l'accesso, è possibile esaminare i dati e gli oggetti di database e valutare i database per la migrazione. Per ulteriori informazioni sullo strumento, vedere [SQL Server Migration Assistant per l'accesso](/sql/ssma/access/sql-server-migration-assistant-for-access-accesstosql).

Per creare una valutazione:

1. Aprire [SSMA per Access](https://www.microsoft.com/download/details.aspx?id=54255).
1. Selezionare **file**, quindi fare clic su **nuovo progetto**.
1. Immettere un nome di progetto e un percorso per salvare il progetto. Selezionare quindi un SQL Server destinazione migrazione dall'elenco a discesa e fare clic su **OK**.

   ![Screenshot che mostra il nuovo progetto.](./media/access-to-sql-server/new-project.png)

1. Selezionare **Aggiungi database** e selezionare i database da aggiungere al progetto.

   ![Screenshot che mostra l'aggiunta di database.](./media/access-to-sql-server/add-databases.png)

1. In **Esplora metadati di Access**, fare clic con il pulsante destro del mouse sul database che si desidera valutare, quindi scegliere **Crea report**. In alternativa, è possibile selezionare la scheda **Crea report** nell'angolo in alto a destra.

   ![Screenshot che mostra la creazione di report.](./media/access-to-sql-server/create-report.png)

1. Leggere il report HTML per esaminare le statistiche di conversione e gli eventuali errori o avvisi. È inoltre possibile aprire il report in Excel per ottenere un inventario degli oggetti di accesso e lo sforzo necessario per eseguire le conversioni dello schema. Il percorso predefinito per il report si trova nella cartella report in SSMAProjects, come illustrato di seguito:

   `drive:\<username>\Documents\SSMAProjects\MyAccessMigration\report\report_2020_11_12T02_47_55\`.

   ![Screenshot che mostra un report di esempio.](./media/access-to-sql-server/sample-report.png)

### <a name="validate-the-data-types"></a>Convalidare i tipi di dati

Verificare i mapping dei tipi di dati predefiniti e modificarli in base ai requisiti, se necessario. A tale scopo, procedere nel seguente modo:

1. Scegliere **Impostazioni progetto** dal menu **strumenti** .
1. Selezionare la scheda **mapping dei tipi** .

   ![Screenshot che mostra il mapping dei tipi.](./media/access-to-sql-server/type-mappings.png)

1. È possibile modificare il mapping dei tipi per ogni tabella selezionando la tabella in **Esplora metadati di Access**.

### <a name="convert"></a>Conversione

Per convertire gli oggetti di database:

1. Selezionare **Connetti a SQL Server** e immettere i dettagli della connessione.

   ![Screenshot che mostra la connessione a SQL Server.](./media/access-to-sql-server/connect-to-sql-server.png)

1. Fare clic con il pulsante destro del mouse sul database in **Esplora metadati di Access** e scegliere **Converti schema**. In alternativa, è possibile selezionare la scheda **Converti schema** nell'angolo superiore destro.

   ![Screenshot che mostra la conversione dello schema.](./media/access-to-sql-server/convert-schema.png)

1. Al termine della conversione, confrontare ed esaminare gli oggetti convertiti con gli oggetti originali per identificare i potenziali problemi e risolverli in base alle indicazioni.

   ![Screenshot che mostra il confronto di query convertite.](./media/access-to-sql-server/query-comparison.png)

1. Confrontare il testo Transact-SQL convertito con il codice originale ed esaminare le indicazioni.

   ![Screenshot che mostra la revisione degli oggetti convertiti.](./media/access-to-sql-server/table-comparison.png)

1. Opzionale Per convertire un singolo oggetto, fare clic con il pulsante destro del mouse sull'oggetto e scegliere **Converti schema**. Un oggetto che è stato convertito viene visualizzato in grassetto in **Esplora metadati di Access**.

   ![Screenshot che Mostra gli oggetti in grassetto in Esplora metadati convertiti.](./media/access-to-sql-server/converted-items-bold.png)
 
1. Nel riquadro Output selezionare **Verifica risultati** ed esaminare gli errori nel riquadro **Elenco errori** .
1. Salvare il progetto in locale per un esercizio di correzione dello schema offline. Scegliere **Salva progetto** dal menu **file** . Questo passaggio consente di valutare gli schemi di origine e di destinazione offline ed eseguire la correzione prima di pubblicare lo schema in SQL Server.

## <a name="migrate"></a>Migrate

Dopo aver valutato i database e risolto eventuali discrepanze, il passaggio successivo consiste nell'eseguire il processo di migrazione. La migrazione dei dati è un'operazione di caricamento bulk che consente di spostare righe di dati in SQL Server nelle transazioni. Il numero di righe da caricare in SQL Server in ogni transazione viene configurato nelle impostazioni del progetto.

Per pubblicare lo schema ed eseguire la migrazione dei dati usando SSMA per l'accesso:

1. Se non è già stato fatto, selezionare **Connetti a SQL Server** e immettere i dettagli della connessione.

1. Pubblicare lo schema facendo clic con il pulsante destro del mouse sul database in **SQL Server Esplora metadati** e selezionando **Sincronizza con database**. Questa azione pubblica lo schema MySQL per SQL Server.

   ![Screenshot che mostra la sincronizzazione con il database.](./media/access-to-sql-server/synchronize-with-database.png)

1. Esaminare il mapping tra il progetto di origine e la destinazione.

   ![Screenshot che mostra la revisione della sincronizzazione con il database.](./media/access-to-sql-server/synchronize-with-database-review.png)

1. Migrare i dati facendo clic con il pulsante destro del mouse sul database o sull'oggetto di cui si vuole eseguire la migrazione in **Esplora metadati di Access** e selezionando **Migrate data** In alternativa, è possibile selezionare la scheda **Migrate data** . Per eseguire la migrazione dei dati per un intero database, selezionare la casella di controllo accanto al nome del database. Per eseguire la migrazione dei dati da singole tabelle, espandere il database, espandere **tabelle**, quindi selezionare le caselle di controllo accanto alle tabelle. Per omettere i dati dalle singole tabelle, deselezionare le caselle di controllo.

   ![Screenshot che mostra la migrazione dei dati.](./media/access-to-sql-server/migrate-data.png)

1. Al termine della migrazione, visualizzare il **report di migrazione dei dati**.

   ![Screenshot che mostra il report Migrate data.](./media/access-to-sql-server/migrate-data-review.png)

1. Connettersi all'istanza di SQL Server utilizzando [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)e convalidare la migrazione riesaminando i dati e lo schema.

   ![Screenshot che mostra la convalida in SQL Server Management Studio.](./media/access-to-sql-server/validate-in-ssms.png)

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

| Titolo | Descrizione |
| -------------- | --------------- |
| [Strumento e modello di valutazione del carico di lavoro dei dati](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Questo strumento fornisce le piattaforme di destinazione consigliate, la preparazione per il cloud e il livello di monitoraggio e aggiornamento dell'applicazione o del database per un determinato carico di lavoro. Offre semplici calcoli con un solo clic e la generazione di report che consentono di accelerare le valutazioni di grandi dimensioni fornendo un processo decisionale di piattaforma di destinazione automatizzato e uniforme. |

Le risorse sono state sviluppate dal team di progettazione di SQL Data. La carta di base di questo team consente di sbloccare e accelerare la modernizzazione complessa per i progetti di migrazione della piattaforma dati alla piattaforma dati di Microsoft Azure.


## <a name="next-steps"></a>Passaggi successivi

- Dopo la migrazione, vedere la [Guida alla convalida e all'ottimizzazione post-migrazione](/sql/relational-databases/post-migration-validation-and-optimization-guide).
- Per una matrice di servizi e strumenti di Microsoft e di terze parti disponibili per agevolare diversi scenari di migrazione di database e dati e attività specializzate, vedere [strumenti e servizi di migrazione dei dati](/azure/dms/dms-tools-matrix).
- Per altre guide alla migrazione, vedere [guide alla migrazione del database di Azure](https://datamigration.microsoft.com/).
- Per i video sulla migrazione, vedere [Panoramica del percorso di migrazione](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).
