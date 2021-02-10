---
title: Eseguire la migrazione dello schema HR Oracle a SQL Server in Linux | Microsoft Docs
description: Convertire lo schema Oracle di esempio in SQL Server in Linux
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: ssma
ms.openlocfilehash: fb21fdc7da82d6dae2390ddcf0884ea1e8ac8a58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044391"
---
# <a name="migrate-an-oracle-schema-to-sql-server-2017-on-linux-with-the-sql-server-migration-assistant"></a>Eseguire la migrazione di uno schema Oracle a SQL Server 2017 in Linux con il SQL Server Migration Assistant

Questa esercitazione usa SQL Server Migration Assistant (SSMA) per Oracle in Windows per convertire lo schema **HR** di esempio oracle in [SQL Server 2017 in Linux](../../linux/sql-server-linux-overview.md).

> [!div class="checklist"]
> * Scaricare e installare SSMA in Windows
> * Creare un progetto SSMA per gestire la migrazione
> * Connettersi a Oracle
> * Eseguire un report di migrazione
> * Convertire lo schema HR di esempio
> * Migrazione dei dati

## <a name="prerequisites"></a>Prerequisiti

- Istanza di Oracle 12C (12.2.0.1.0) con lo schema **HR** installato
- Istanza funzionante di SQL Server in Linux

> [!NOTE]
> Gli stessi passaggi possono essere usati per SQL Server di destinazione in Windows, ma è necessario selezionare Windows nell'impostazione **Esegui migrazione al** progetto.

## <a name="download-and-install-ssma-for-oracle"></a>Scaricare e installare SSMA per Oracle

Sono disponibili diverse edizioni di SQL Server Migration Assistant, a seconda del database di origine.  Scaricare la versione corrente di [SQL Server Migration Assistant per Oracle](https://aka.ms/ssmafororacle) e installarla usando le istruzioni disponibili nella pagina di download.

> [!NOTE]
> A questo punto, **SSMA per Oracle Extension Pack** non è supportato in Linux, ma non è necessario per questa esercitazione.

## <a name="create-and-set-up-project"></a>Creazione e configurazione di un progetto

Per creare un nuovo progetto SSMA, attenersi alla procedura seguente:

1. Aprire SSMA per Oracle e scegliere **nuovo progetto** dal menu **file** .

1. Assegnare un nome al progetto.

1. Scegliere "SQL Server 2017 (Linux)-Preview" nel campo **migrare a** .

Per impostazione predefinita, SSMA per Oracle non utilizza gli schemi di esempio Oracle. Per abilitare lo schema HR, attenersi alla procedura seguente:

1. In SSMA selezionare il menu **strumenti** .

1. Selezionare **Impostazioni progetto predefinite**, quindi scegliere **carica oggetti di sistema**.

1. Verificare che **HR** sia selezionato e scegliere **OK**.

## <a name="connect-to-oracle"></a>Connettersi a Oracle

Connettere quindi SSMA a Oracle.

1. Sulla barra degli strumenti fare clic su **Connetti a Oracle**.

1. Immettere il nome del server, la porta, il SID Oracle, il nome utente e la password.

   ![Connettersi a Oracle](./media/sql-server-linux-convert-from-oracle/ConnectToOracle.png)

1. Fare clic su **Connetti**. In pochi istanti, SSMA per Oracle si connette al database e ne legge i metadati.

## <a name="create-a-report"></a>Creare un report

Utilizzare la procedura seguente per generare un report di migrazione.

1. In **Oracle Metadata Explorer** espandere il nodo del server.

1. Espandere **schemi**, fare clic con il pulsante destro del mouse su **HR** e scegliere **Crea report**.

   ![Creazione report di Oracle Metadata Explorer](./media/sql-server-linux-convert-from-oracle/CreateReport.png)

1. Verrà visualizzata una nuova finestra del browser con un report in cui sono elencati tutti gli avvisi e gli errori associati alla conversione.

   > [!NOTE]
   > Per questa esercitazione non è necessario eseguire alcuna operazione con tale elenco. Se si eseguono questi passaggi per il proprio database Oracle, è necessario esaminare il report per risolvere eventuali problemi di conversione importanti per il database.

   ![Report di migrazione di esempio](./media/sql-server-linux-convert-from-oracle/SSMAReport.png)

## <a name="connect-to-sql-server"></a>Connessione a SQL Server

Scegliere quindi **Connetti a SQL Server** e immettere le informazioni di connessione appropriate.  Se si usa un nome di database che non esiste già, SSMA per Oracle lo crea automaticamente.

![Connessione a SQL Server](./media/sql-server-linux-convert-from-oracle/ConnectToSQLServer.png)

## <a name="convert-schema"></a>Converti schema

Fare clic con il pulsante destro del mouse su **HR** in **Oracle Metadata Explorer** e scegliere Converti schema.

![Converti schema](./media/sql-server-linux-convert-from-oracle/ConvertSchema.png)

## <a name="synchronize-database"></a>Sincronizza database

Sincronizzare quindi il database.

1. Al termine della conversione, usare **SQL Server Metadata Explorer** per passare al database creato nel passaggio precedente.

1. Fare clic con il pulsante destro del mouse sul database, scegliere **Sincronizza con database**, quindi fare clic su OK.

   ![Sincronizza con database](./media/sql-server-linux-convert-from-oracle/SynchronizeWithDatabase.png)

## <a name="migrate-data"></a>Eseguire la migrazione dei dati

Il passaggio finale consiste nel migrare i dati.

1. In **Oracle Metadata Explorer** fare clic con il pulsante destro del mouse su **HR** e selezionare **Migrate data**.

1. Per la fase di migrazione dei dati è necessario immettere nuovamente le credenziali Oracle e SQL Server.

1. Al termine, esaminare il report di migrazione dei dati, che dovrebbe essere simile allo screenshot seguente:

   ![Report di migrazione dati](./media/sql-server-linux-convert-from-oracle/DataMigrationReport.png)

## <a name="next-steps"></a>Passaggi successivi

Per uno schema Orcale più complesso, il processo di conversione comporterebbe più tempo, test e possibili modifiche alle applicazioni client. Lo scopo di questa esercitazione è mostrare come è possibile usare SSMA per Oracle come parte del processo di migrazione globale.

In questa esercitazione sono state illustrate le procedure per:
> [!div class="checklist"]
> * Installare SSMA in Windows
> * Creare un nuovo progetto SSMA
> * Valutazione ed esecuzione di una migrazione da Oracle

Esplorare quindi altri modi per usare SSMA:

> [!div class="nextstepaction"]
>[Documentazione di SQL Server Migration Assistant](../sql-server-migration-assistant.md)
