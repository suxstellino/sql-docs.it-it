---
title: Distribuire il database SQL di Azure tramite Notebook
description: Questa esercitazione illustra come creare un database SQL di Azure.
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, markingmyname
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/16/2020
ms.openlocfilehash: 8bac1e0d10208283e40843288ab7dbe7d042b8ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052212"
---
# <a name="create-an-azure-sql-database-using-azure-data-studio"></a>Creare un database SQL di Azure usando Azure Data Studio

È possibile creare un database SQL di Azure usando Azure Data Studio tramite la Distribuzione guidata e i notebook.

## <a name="pre-requisites"></a>Prerequisiti

 - [Azure Data Studio](download-azure-data-studio.md) installato
 - Un account e una sottoscrizione di Azure attivi Se non se ne possiede una, [creare un account gratuito](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Usare la Distribuzione guidata

Seguire questa procedura per usare la Distribuzione guidata, che fornirà le indicazioni per definire le impostazioni necessarie tramite un'esperienza di interfaccia utente intuitiva.

Prima di tutto, individuare e selezionare l'opzione Database SQL di Azure nella Distribuzione guidata.

 1. In Azure Data Studio selezionare il viewlet Connessioni nel riquadro di spostamento a sinistra.
 2. Selezionare il pulsante **...** nella parte superiore del pannello Connessioni e scegliere **Nuova distribuzione**.
 3. Nella Distribuzione guidata selezionare il riquadro **Database SQL di Azure** e selezionare la casella di controllo per accettare le condizioni.
 4. Nell'elenco a discesa Tipo di risorsa selezionare il tipo di risorsa che si vuole creare, ovvero un database, un server di database o un pool elastico. Se non sono presenti database SQL in Azure, è consigliabile iniziare a crearne uno.
 5. Selezionare **Crea nel portale di Azure** se si sceglie di creare un server o un pool oppure fare clic su **Seleziona** se si sceglie di creare un database.

Per creare un database, attenersi alla procedura riportata di seguito.

 1. Accedere all'account Azure personale, se non si è già connessi. Se si verificano problemi in questa pagina della procedura guidata, è possibile aggiornare la connessione.
 2. Selezionare la sottoscrizione e il server desiderati. Fare quindi clic su **Avanti**.
 3. Immettere un nome per il database.
 4. Immettere un nome per la regola del firewall e l'intervallo IP del computer client locale che esegue Azure Data Studio. In questo modo è possibile connettersi al server e al database da ADS subito dopo che è stato creato.
 5. Selezionare **Avanti**.
 6. Rivedere i parametri specificati e quindi selezionare **Genera script nel notebook**.

Una volta aperto il notebook, è possibile esaminare il contenuto e il codice e apportare le modifiche desiderate. In particolare, è possibile modificare le impostazioni predefinite di calcolo e archiviazione se si dispone di preferenze specifiche per le prestazioni o i costi. Tenere tuttavia presente che qualsiasi modifica apportata al notebook può potenzialmente causare errori di convalida.

L'ultimo passaggio consiste nel selezionare **Esegui tutte le celle** per eseguire tutte le celle del notebook. Al termine dell'operazione, si disporrà di un database SQL creato e in esecuzione a cui sarà possibile connettersi ed eseguire query da ADS.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver creato il database, il server o il pool, è possibile [connettersi ed eseguire query](quickstart-sql-database.md) sul database in Azure Data Studio.
