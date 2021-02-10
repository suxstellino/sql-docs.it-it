---
title: Distribuire SQL Edge di Azure con Azure Data Studio
description: Come distribuire istanze di Azure SQL Edge in Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: b0dd541ce4be57dd9b065b93413ea567c7f121b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052222"
---
# <a name="deploy-azure-sql-edge-with-azure-data-studio-preview"></a>Distribuire SQL Edge di Azure con Azure Data Studio (anteprima)

[SQL Edge di Azure](/azure/azure-sql-edge/overview) è un motore di database relazionale ottimizzato per distribuzioni IoT e Azure IoT Edge. Fornisce funzionalità per la creazione di un livello di elaborazione e archiviazione dei dati ad alte prestazioni per le applicazioni e le soluzioni IoT. Questo articolo illustra come distribuire un'istanza di SQL Edge di Azure con Azure Data Studio e gli scenari di distribuzione supportati con la distribuzione guidata.  

Gli scenari seguenti sono supportati dalla distribuzione guidata in Azure Data Studio:

- Istanza di contenitore locale
- Istanza di contenitore remoto
- Nuovo hub IoT e macchina virtuale di Azure
- Dispositivo esistente di un hub IoT di Azure
- Più dispositivi di un hub IoT di Azure

| Strumenti richiesti | `Docker` | `Azure CLI` |
| ------------- | :---: | :---: |
| Istanza di contenitore locale | x | |
| Istanza di contenitore remoto | | |
| Nuovo hub IoT e macchina virtuale di Azure | | x |
| Dispositivo esistente di un hub IoT di Azure |  | x |
| Più dispositivi di un hub IoT di Azure |   |  x |

> [!NOTE]
> La distribuzione di SQL Edge di Azure (anteprima) è disponibile tramite l'estensione "Estensione distribuzione di SQL Edge di Azure", mentre queste funzionalità sono in versione di anteprima. Per rendere disponibili le funzionalità, installare l'estensione in Azure Data Studio.

Per avviare una distribuzione in Azure Data Studio, aprire il menu nella vista **Connessioni** e selezionare l'opzione **Nuova distribuzione**.  Per tutti gli scenari di SQL Edge di Azure, selezionare **SQL Edge di Azure** nella schermata seguente e lo scenario di interesse dall'elenco a discesa **Destinazione di distribuzione**. La distribuzione guidata genera un notebook T-SQL che può essere eseguito per completare la distribuzione. Si noti che le informazioni di connessione, inclusa la password amministratore di SQL, sono contenute nel notebook per impostazione predefinita.

![panoramica della distribuzione](media/deploy-azure-sql-edge/deploy-overview.png)

## <a name="local-container-instance"></a>Istanza di contenitore locale

SQL Edge di Azure può essere distribuito in un contenitore Docker in localhost specificando il nome del contenitore, la password utente `sa` e la porta host per la connettività SQL Edge di Azure.  Dopo aver completato la distribuzione, nella parte inferiore del notebook sono presenti numerosi collegamenti per altre azioni:

- **Select here to connect to the Azure SQL Edge instance** (Selezionare qui per connettersi all'istanza di SQL Edge di Azure): consente di creare una connessione in Azure Data Studio alla nuova istanza di SQL Edge di Azure
- **Open the terminal window** (Aprire la finestra del terminale): consente di aprire un terminale, nuovo o già esistente, in Azure Data Studio
- **Arrestare il contenitore**: consente di copiare un comando nel terminale che arresta il contenitore quando viene eseguito dall'utente
- **Rimuovere il contenitore**: consente di copiare un comando nel terminale che rimuove il contenitore quando viene eseguito dall'utente

## <a name="remote-container-instance"></a>Istanza di contenitore remoto

SQL Edge di Azure può essere distribuito in un contenitore Docker in un host remoto con Docker installato specificando le informazioni sul contenitore e le informazioni sul computer di destinazione/host.  Dopo aver completato la distribuzione, nella parte inferiore del notebook è presente un collegamento di connessione.  A causa dell'ambiente host del contenitore remoto, per arrestare o rimuovere il contenitore è necessario copiare i comandi per l'esecuzione in una shell remota.

## <a name="new-azure-iot-hub-and-vm"></a>Nuovo hub IoT e macchina virtuale di Azure

La distribuzione guidata di SQL Edge di Azure può creare diverse risorse di Azure necessarie per distribuire una macchina virtuale abilitata per Edge connessa a un hub IoT di Azure. È necessaria una sottoscrizione di Azure attiva. Questa distribuzione crea gli elementi seguenti:

- Gruppo di risorse (se il gruppo di risorse corrente non è selezionato)
- Gruppo di sicurezza di rete
- Macchina virtuale
- Indirizzo IP pubblico statico

Facoltativamente, è possibile inserire un file dacpac in una cartella compressa e distribuirlo alla nuova istanza di SQL Edge di Azure come parte del processo.  Se è disponibile un file dacpac, viene creato un account di Archiviazione BLOB di Azure nello stesso gruppo di risorse.

## <a name="existing-device-of-an-azure-iot-hub"></a>Dispositivo esistente di un hub IoT di Azure

Se sono presenti un hub IoT esistente e un dispositivo connesso, SQL Edge di Azure può essere distribuito nel dispositivo in base al gruppo di risorse, al nome dell'hub IoT e all'ID del dispositivo.
L'indirizzo IP specificato durante la distribuzione guidata viene usato per generare un collegamento rapido di connessione nella parte inferiore del notebook.

Facoltativamente, è possibile inserire un file dacpac in una cartella compressa e distribuirlo alla nuova istanza di SQL Edge di Azure come parte del processo.  Se è disponibile un file dacpac, viene creato un account di Archiviazione BLOB di Azure nello stesso gruppo di risorse.

## <a name="multiple-devices-of-an-azure-iot-hub"></a>Più dispositivi di un hub IoT di Azure

Se sono presenti un hub IoT esistente e un dispositivo connesso, SQL Edge di Azure può essere distribuito nel dispositivo in base al gruppo di risorse, al nome dell'hub IoT e a una [condizione di destinazione](/azure/iot-edge/module-deployment-monitoring#target-condition) per selezionare uno o più dispositivi.
L'indirizzo IP specificato durante la distribuzione guidata viene usato per generare un collegamento rapido di connessione nella parte inferiore del notebook.

Facoltativamente, è possibile inserire un file dacpac in una cartella compressa e distribuirlo alla nuova istanza di SQL Edge di Azure come parte del processo.  Se è disponibile un file dacpac, viene creato un account di Archiviazione BLOB di Azure nello stesso gruppo di risorse.

## <a name="next-steps"></a>Passaggi successivi

- [Altre informazioni su SQL Edge di Azure](/azure/azure-sql-edge/)