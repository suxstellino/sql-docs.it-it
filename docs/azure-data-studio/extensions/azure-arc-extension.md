---
title: Estensione Azure Arc (anteprima)
description: Informazioni su come installare e usare l'estensione Azure Arc per provare i servizi dati di Azure Arc.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 195404df404c6fec7ef98f41a9921d937c16eab7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048591"
---
# <a name="azure-arc-extension-for-azure-data-studio-preview"></a>Estensione Azure Arc per Azure Data Studio (anteprima)

L'[estensione Azure Arc (anteprima)](/azure/azure-arc/data/) consente di creare e gestire le risorse dei servizi dati di Azure Arc.

**Le azioni principali comprendono:**
- Creare una risorsa
    - Data Controller
    - Istanza gestita di SQL per Azure Arc
    - PostgreSQL per Azure Arc
- Gestire una risorsa
    - Visualizzare il dashboard Data Controller (Controller dati)
    - Visualizzare l'istanza gestita di SQL per il dashboard di Azure Arc
    - Visualizzare PostgreSQL per il dashboard di Azure Arc
- Eseguire un book di Jupyter Azure Arc

## <a name="install-the-extension"></a>Installare l'estensione
- Installare l'estensione dell'**interfaccia della riga di Azure Data** dalla raccolta.
- Installare l'estensione **Azure Arc** dalla raccolta.
- Riavviare Azure Data Studio

## <a name="sign-in-with-azure-account"></a>Accedere con l'account di Azure
1. Selezionare gli account in basso a sinistra
1. Selezionare Aggiungi account
1. Verrà avviato un browser. Accedere all'account Azure personale.

## <a name="create-a-resource"></a>Creare una risorsa
Questa estensione supporta la distribuzione di con dati di Azure Arc, Postgres per Azure Arc e Istanza gestita di SQL per Azure Arc. Le distribuzioni possono essere eseguite tramite la distribuzione guidata incorporata.

1. Selezionare il viewlet Connessioni sulla barra delle attività a sinistra
1. Selezionare i tre puntini e selezionare **Nuova distribuzione**
1. Seguire le istruzioni per creare una nuova risorsa di Azure Arc.

## <a name="manage-a-resource"></a>Gestire una risorsa
Dopo aver distribuito un controller dati Azure Arc da azdata, dal portale di Azure o da Azure Data Studio, è possibile connettersi a esso da Azure Data Studio.

1. Aprire il viewlet Connections sulla barra delle attività a sinistra.
1. Espandere **Azure Arc controllers** (Controller Azure Arc)
1. Selezionare **Connect controller** (Connetti controller)
1. Compilare i parametri e connettersi.

Dopo aver stabilito la connessione, è possibile visualizzare le risorse distribuite nel controller dati. È quindi possibile fare clic con il pulsante destro del mouse e selezionare **Manage** (Gestisci) per accedere al dashboard delle risorse.  

Questi dashboard offrono informazioni aggiuntive sulla risorsa, inclusa l'opzione per aprire il portale di Azure.

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sui servizi di dati Azure Arc, [vedere la documentazione.](/azure/azure-arc/data/)