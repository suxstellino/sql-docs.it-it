---
title: WideWorldImportersDW-flusso di lavoro ETL | Microsoft Docs
description: Usare il pacchetto ETL con SQL Server Integration Services (SSIS) per eseguire periodicamente la migrazione dei dati dal database WideWorldImporters al WideWorldImportersDW.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ce9cd8ee5a4153cb1d744d505a26b400497b460b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354054"
---
# <a name="wideworldimportersdw-etl-workflow"></a>Flusso di lavoro ETL WideWorldImportersDW
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Usare il pacchetto ETL *WWI_Integration* per eseguire la migrazione dei dati dal database wideworldimporters al database WideWorldImportersDW in base alle modifiche apportate ai dati. Il pacchetto viene eseguito periodicamente, in genere giornaliero.

Il pacchetto garantisce prestazioni elevate utilizzando SQL Server Integration Services per orchestrare operazioni T-SQL bulk (anziché trasformazioni separate in Integration Services).

Per prima cosa vengono caricate le dimensioni e quindi vengono caricate le tabelle dei fatti. È possibile rieseguire il pacchetto in qualsiasi momento dopo un errore.

Il flusso di lavoro ha un aspetto simile al seguente:

 ![Flusso di lavoro ETL WideWorldImporters](media/wide-world-importers/wideworldimporters-etl-workflow.png)

Il flusso di lavoro inizia con un'attività di espressione che determina il tempo di taglio appropriato. L'ora di taglio è l'ora corrente meno pochi minuti. Questo approccio è più affidabile rispetto alla richiesta di dati direttamente all'ora corrente. Ogni millisecondi viene troncato a partire dal momento.

L'elaborazione principale viene avviata popolando la tabella delle dimensioni date. L'elaborazione garantisce che tutte le date per l'anno corrente siano state popolate nella tabella.

Successivamente, una serie di attività flusso di dati carica ogni dimensione. Quindi, caricano ogni fatto.

## <a name="prerequisites"></a>Prerequisiti

- SQL Server 2016 (o versione successiva) con i database WideWorldImporters e WideWorldImportersDW (nello stesso o in istanze diverse di SQL Server)
- SQL Server Management Studio
- SQL Server 2016 Integration Services
  - Assicurarsi di creare un catalogo Integration Services. Per creare un catalogo Integration Services, in SQL Server Management Studio Esplora oggetti fare clic con il pulsante destro del mouse su **Integration Services** e quindi scegliere **Aggiungi catalogo**. Lasciare le opzioni predefinite. Viene richiesto di abilitare SQLCLR e specificare una password.


## <a name="download"></a>Scarica

Per la versione più recente dell'esempio, vedere [Wide-World-Importers-Release](https://go.microsoft.com/fwlink/?LinkID=800630). Scaricare il file di pacchetto *ETL. ispac Integration Services giornaliero* .

Per il codice sorgente per ricreare il database di esempio, vedere [Wide-World-Importers](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-ssis).

## <a name="install"></a>Installazione

1. Distribuire il pacchetto di Integration Services:
   1. In Esplora risorse aprire il pacchetto *ETL. ispac giornaliero* . Viene avviata la distribuzione guidata SQL Server Integration Services.
   2. In **Seleziona origine** seguire le impostazioni predefinite per la distribuzione del progetto, con il percorso che punta al pacchetto *ETL. ispac giornaliero* .
   3. In **Selezione destinazione** immettere il nome del server che ospita il catalogo Integration Services.
   4. Selezionare un percorso nel catalogo Integration Services, ad esempio, in una nuova cartella denominata *wideworldimporters*.
   5. Selezionare **Distribuisci** per terminare la procedura guidata.

2. Creare un processo di SQL Server Agent per il processo ETL:
   1. In Management Studio fare clic con il pulsante destro del mouse su **SQL Server Agent**, quindi scegliere **nuovo**  >  **processo**.
   2. Immettere un nome, ad esempio *WIDEWORLDIMPORTERS ETL*.
   3. Aggiungere un **passaggio di processo** del tipo **SQL Server Integration Services pacchetto**.
   4. Selezionare il server in cui è presente il catalogo Integration Services, quindi selezionare il pacchetto *ETL giornaliero* .
   5. In   >  **gestioni connessioni** di configurazione assicurarsi che le connessioni all'origine e alla destinazione siano configurate correttamente. Il valore predefinito è la connessione all'istanza locale.
   6. Selezionare **OK** per creare il processo.

3. Eseguire o pianificare il processo.
