---
title: Distribuire SQL Server in Macchine virtuali di Azure
description: Questa esercitazione illustra come creare SQL Server in Macchine virtuali di Azure
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, maghan
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: d73e4b95059dab183f9a0af4896c5a6f86b7e564
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091758"
---
# <a name="create-sql-server-on-azure-virtual-machines-using-azure-data-studio"></a>Creare SQL Server in Macchine virtuali di Azure usando Azure Data Studio

È possibile creare una macchina virtuale (VM) SQL usando Azure Data Studio tramite la Distribuzione guidata e i notebook.

## <a name="pre-requisites"></a>Prerequisiti

- [Azure Data Studio](download-azure-data-studio.md) installato
- Un account e una sottoscrizione di Azure attivi Se non se ne ha una, [creare un account gratuito](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Usare la Distribuzione guidata

Seguire questa procedura per usare la Distribuzione guidata, che fornirà le indicazioni per definire le impostazioni necessarie tramite un'esperienza di interfaccia utente intuitiva.

Per prima cosa, trovare e selezionare la macchina virtuale SQL di Azure nella Distribuzione guidata.

1. In Azure Data Studio selezionare il viewlet Connessioni nel riquadro di spostamento a sinistra.

2. Selezionare il pulsante **...** nella parte superiore del pannello Connessioni e scegliere **Nuova distribuzione**.

3. Nella Distribuzione guidata selezionare il riquadro **Macchina virtuale SQL di Azure** e selezionare la casella di controllo per accettare le condizioni.

4. Se richiesto, installare gli strumenti necessari e quindi selezionare il pulsante **Seleziona** in basso.

Immettere quindi tutti i parametri necessari nella procedura guidata della macchina virtuale SQL di Azure.

1. Connettersi all'account Azure personale, se non si è già connessi. Se si verificano problemi in questa pagina della procedura guidata, è possibile aggiornare la connessione.

2. Selezionare i valori desiderati per la sottoscrizione, il gruppo di risorse e l'area. Fare quindi clic su **Avanti**.

3. Immettere un nome univoco per la macchina virtuale e le credenziali con nome utente e password.

4. Selezionare l'immagine, lo SKU e la versione preferiti e quindi selezionare la dimensione preferita per la macchina virtuale. È possibile leggere altre informazioni sulle [dimensioni disponibili per la macchina virtuale](/azure/virtual-machines/sizes) per facilitare la selezione. Fare quindi clic su **Avanti**.

5. Selezionare una rete virtuale esistente dall'elenco a discesa oppure selezionare la casella di controllo **Nuova rete virtuale** per immettere il nome di una nuova rete virtuale.

6. Eseguire la stessa operazione per selezionare o creare una subnet e un indirizzo IP pubblico.

7. Se si vuole connettersi alla macchina virtuale tramite Desktop remoto (RDP), selezionare la casella di controllo **Abilita porta RDP in ingresso**. Fare quindi clic su **Avanti**.

8. Immettere le impostazioni di SQL Server preferite. Per testare questa esperienza è consigliabile impostare la connettività SQL su **Pubblica (Internet)** , immettere la porta 1433 e abilitare l'autenticazione SQL con il nome utente e la password preferiti. Fare quindi clic su **Avanti**.

9. Rivedere i parametri specificati e quindi selezionare **Genera script nel notebook**.

Una volta aperto il notebook, è possibile esaminare il contenuto e il codice e apportare le modifiche desiderate. Non è tuttavia consigliabile eseguire modifiche perché possono verificarsi errori di convalida.

L'ultimo passaggio consiste nel selezionare **Esegui tutte le celle** per eseguire tutte le celle del notebook. Al termine della procedura, si avrà un sistema completo in esecuzione composto da:

- Una macchina virtuale di Azure
- Una macchina virtuale SQL
- Una rete virtuale, una subnet e un indirizzo IP pubblico
- Un gruppo di sicurezza di rete e un'interfaccia di rete
- Accesso a RDP nella macchina virtuale
- Accesso a un'ampia gamma di funzionalità di gestibilità per SQL Server per controllare facilmente la pianificazione dei backup, la pianificazione dell'applicazione di patch, l'edizione e le licenze di SQL Server, le configurazioni delle prestazioni di archiviazione e molto altro ancora.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come eseguire la migrazione dei dati nella nuova macchina virtuale SQL, vedere l'articolo seguente.

> [!div class="nextstepaction"]
> [Eseguire la migrazione di un database in una VM di SQL Server](/azure/azure-sql/virtual-machines/windows/migrate-to-vm-from-sql-server)

Per altre informazioni sull'uso di SQL Server in Azure, vedere [SQL Server in macchine virtuali di Azure](/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) e le [domande frequenti](/azure/azure-sql/virtual-machines/windows/frequently-asked-questions-faq).