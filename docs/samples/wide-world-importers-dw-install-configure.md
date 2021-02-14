---
title: Installare & configurare il database di esempio DW WideWorldImporters
description: Seguire queste istruzioni per scaricare, installare e configurare il database di esempio WideWorldImportersDW con SQL Server Management Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 70128430c6ea0943605bd29f00597d51dee71d86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077752"
---
# <a name="wideworldimportersdw-installation-and-configuration"></a>Installazione e configurazione di WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Istruzioni per l'installazione e la configurazione per il database WideWorldImportersDW.

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (o versione successiva) o il [database SQL di Azure](https://azure.microsoft.com/services/sql-database/). Per utilizzare la versione completa dell'esempio, utilizzare SQL Server Evaluation/Developer/Enterprise Edition.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Per ottenere risultati ottimali, usare la versione di giugno 2016 o successiva.

## <a name="download"></a>Scarica

La versione più recente dell'esempio:

[Wide-World-Importers-versione](https://go.microsoft.com/fwlink/?LinkID=800630)

Scaricare il backup del database di esempio WideWorldImportersDW/BacPac corrispondente all'edizione di SQL Server o al database SQL di Azure.

Il codice sorgente per ricreare il database di esempio è disponibile nel percorso seguente. Si noti che il popolamento dei dati è basato su ETL del database OLTP (WideWorldImporters):

[Wide-World-Importers-source](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

## <a name="install"></a>Installazione


### <a name="sql-server"></a>SQL Server

Per ripristinare un backup in un'istanza di SQL Server, è possibile usare Management Studio.

1. Aprire SQL Server Management Studio e connettersi all'istanza di SQL Server di destinazione.
2. Fare clic con il pulsante destro del mouse sul nodo **database** e selezionare **Ripristina database**.
3. Selezionare **Device (dispositivo** ) e fare clic sul pulsante **...**
4. Nella finestra di dialogo **Seleziona dispositivi di backup** fare clic su **Aggiungi**, passare al backup del database nel file System del server e selezionare il backup. Fare clic su **OK**.
5. Se necessario, modificare il percorso di destinazione per i file di dati e di log nel riquadro **file** . Si noti che è consigliabile inserire i file di dati e di log in unità diverse.
6. Fare clic su **OK**. Verrà avviato il ripristino del database. Al termine, il database WideWorldImporters verrà installato nell'istanza di SQL Server.

### <a name="azure-sql-database"></a>Database SQL di Azure

Per importare un BACPAC in un nuovo database SQL, è possibile usare Management Studio.

1. opzionale Se non si dispone ancora di un SQL Server in Azure, passare al [portale di Azure](https://portal.azure.com/) e creare un nuovo database SQL. Nel processo di creazione di un database, si creerà un server. Prendere nota del server.
   - Vedere [questa esercitazione](/azure/azure-sql/database/single-database-create-quickstart) per creare un database in pochi minuti
2. Aprire SQL Server Management Studio e connettersi al server in Azure.
3. Fare clic con il pulsante destro del mouse sul nodo **database** e selezionare **Importa Data-Tier applicazione**.
4. Nelle **impostazioni di importazione** selezionare **Importa da disco locale** e selezionare il BACPAC del database di esempio dal file System.
5. In **Impostazioni database** modificare il nome del database in *WideWorldImportersDW* e selezionare l'edizione di destinazione e l'obiettivo di servizio da usare.
6. Fare clic su **Avanti** e **fine** per avviare la distribuzione. Questa operazione richiederà qualche minuto. Quando si specifica un obiettivo di servizio minore di S2, potrebbe essere necessario più tempo.

## <a name="configuration"></a>Configurazione

[Si applica a SQL Server 2016 (e versioni successive) Developer/Evaluation/Enterprise Edition]

Il database di esempio può usare la polibase per eseguire query sui file in Hadoop o nell'archiviazione BLOB di Azure. Questa funzionalità, tuttavia, non viene installata per impostazione predefinita con SQL Server, quindi è necessario selezionarla durante SQL Server installazione. Pertanto, è necessario eseguire un passaggio di post-installazione.

1. In SQL Server Management Studio connettersi al database WideWorldImportersDW e aprire una nuova finestra query.
2. Eseguire il comando T-SQL seguente per abilitare l'uso di polibase nel database:

   ESEGUIRE [applicazione]. [Configuration_ApplyPolyBase]