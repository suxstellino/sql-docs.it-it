---
title: 'Distribuzione: Notebook di Azure Data Studio'
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come usare il codice e la documentazione in un notebook da Azure Data Studio per distribuire un cluster Big Data di SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 868c7685c59ae1ccf43057fdd03dfbb0b5bdaf21
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637381"
---
# <a name="deploy-sql-server-big-data-cluster-with-azure-data-studio-notebook"></a>Distribuire un cluster Big Data di SQL Server con un notebook di Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)] fornisce un'estensione per Azure Data Studio contenente notebook di distribuzione. Un notebook di distribuzione include codice e documentazione che è possibile usare in Azure Data Studio per creare un cluster Big Data di SQL Server.

Introdotti inizialmente come progetto open source, i [notebook](../azure-data-studio/notebooks/notebooks-guidance.md) sono stati implementati in [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md). È possibile usare markdown per testo nelle celle di testo e uno dei kernel disponibili per scrivere codice nelle celle di codice.

È possibile usare notebook per distribuire cluster Big Data per [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)].

## <a name="prerequisites"></a>Prerequisiti

Per avviare il notebook, è necessario soddisfare i prerequisiti seguenti:

* Versione più recente della [build Insider di Azure Data Studio](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-master) installata

Oltre ai prerequisiti precedenti, la distribuzione di un cluster Big Data di SQL Server 2019 richiede anche:

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Interfaccia della riga di comando di Azure (in caso di distribuzione in Azure)](/cli/azure/install-azure-cli)

## <a name="launch-the-notebook"></a>Avviare il notebook

1. Avviare Azure Data Studio.

2. Nella scheda **Connessioni** fare clic sui puntini di sospensione ( **...** ) e quindi selezionare **Deploy SQL Server** (Distribuisci SQL Server).

   ![Deploy SQL Server (Distribuisci SQL Server)](media/notebooks-deploy/deploy-notebooks.png)

3. Dalle opzioni di distribuzione selezionare **SQL Server Big Data Cluster** (Cluster Big Data di SQL Server).

4. In **Destinazione di distribuzione**, sotto la voce **Opzioni**, selezionare **New Azure Kubernetes Cluster** (Nuovo cluster Azure Kubernetes) oppure **Existing Azure Kubernetes Service cluster** (Cluster del servizio Azure Kubernetes esistente).

5. Accettare l'informativa sulla privacy e le condizioni di licenza

6. Questa finestra di dialogo permette anche di verificare se nell'host sono presenti gli strumenti necessari per il tipo di distribuzione SQL scelto. Il pulsante **Seleziona** non è abilitato fino al completamento del controllo degli strumenti.

7. Fare clic sul pulsante **Seleziona**. Questa azione avvia l'esperienza di distribuzione.

## <a name="set-deployment-configuration-template"></a>Impostare il modello di configurazione della distribuzione

È possibile personalizzare le impostazioni del profilo di distribuzione seguendo le istruzioni di seguito.

### <a name="target-configuration-template"></a>Modello di configurazione di destinazione

Selezionare il modello di configurazione di destinazione dai modelli disponibili. I profili disponibili vengono filtrati in base al tipo di destinazione di distribuzione scelto nella finestra di dialogo precedente.

   ![Modello di configurazione della distribuzione - Passaggio 1](media/notebooks-deploy/deployment-configuration-template.png)

### <a name="azure-settings"></a>Impostazioni di Azure

Se la destinazione di distribuzione è un nuovo servizio Azure Kubernetes, per creare il cluster del servizio Azure Kubernetes sono necessarie informazioni aggiuntive, tra cui l'ID sottoscrizione di Azure, il gruppo di risorse, il nome del cluster del servizio Azure Kubernetes, il numero di macchine virtuali, le dimensioni e altre informazioni.

   ![Impostazioni di Azure](media/notebooks-deploy/azure-settings.png)

Se la destinazione di distribuzione è un cluster Kubernetes esistente, la procedura guidata chiede il percorso del file config *kube* per importare le impostazioni del cluster Kubernetes. Assicurarsi che sia selezionato il contesto del cluster appropriato in cui possa essere distribuito il cluster Big Data di SQL Server 2019.

   ![Contesto del cluster di destinazione](media/notebooks-deploy/target-cluster-context.png)

### <a name="cluster-docker-and-ad-settings"></a>Impostazioni del cluster, di Docker e di Active Directory

1. Immettere il nome del cluster Big Data di SQL Server 2019, il nome utente amministratore e la password.
Nota: lo stesso account viene usato per il controller e SQL Server.

   ![Impostazioni del cluster](media/notebooks-deploy/cluster-settings.png)

2. Immettere le impostazioni di Docker appropriate

   ![Impostazioni di Docker](media/notebooks-deploy/docker-settings.png)

3. Se l'autenticazione di Active Directory è disponibile, immettere le impostazioni di Active Directory

   ![Impostazioni di Active Directory](media/notebooks-deploy/active-directory-settings.png)

### <a name="service-settings"></a>Impostazioni del servizio

Questa schermata contiene gli input per le diverse impostazioni, tra cui **Scala**, **Endpoint**, **Archiviazione** e altre **impostazioni di archiviazione avanzate**. Immettere i valori appropriati e selezionare **Avanti**.

#### <a name="scale-settings"></a>Impostazioni di scalabilità

Immettere il numero di istanze di ognuno dei componenti del cluster Big Data.

È possibile includere un'istanza di Spark insieme ad HDFS. Questa viene inclusa nel pool di archiviazione o autonomamente nel pool Spark.

   ![Impostazioni del servizio](media/notebooks-deploy/service-settings.png)

Per altre informazioni su ognuno di questi componenti, è possibile fare riferimento a [Istanza master](concept-master-instance.md), [Pool di dati](concept-data-pool.md), [Pool di archiviazione](concept-storage-pool.md) o [Pool di calcolo](concept-compute-pool.md).

#### <a name="endpoint-settings"></a>Impostazioni degli endpoint

Gli endpoint predefiniti vengono immessi automaticamente. Tuttavia, possono essere modificati in base alle esigenze.

   ![Impostazioni degli endpoint](media/notebooks-deploy/endpoint-settings.png)

#### <a name="storage-settings"></a>Impostazioni di archiviazione

Le impostazioni di archiviazione includono le dimensioni della classe di archiviazione e dell'attestazione per dati e log. Le impostazioni possono essere applicate ai pool di archiviazione, di dati e dell'istanza master di SQL Server.

   ![Impostazioni di archiviazione](media/notebooks-deploy/storage-settings.png)

#### <a name="advanced-storage-settings"></a>Impostazioni di archiviazione avanzate

È possibile aggiungere altre impostazioni di archiviazione in **Advanced storage settings** (Impostazioni di archiviazione avanzate)

* Pool di archiviazione (HDFS)
* Pool di dati
* Istanza master di SQL Server

   ![Impostazioni di archiviazione avanzate](media/notebooks-deploy/advanced-storage-settings.png)

### <a name="summary"></a>Summary

Questa schermata contiene il riepilogo di tutto l'input fornito per distribuire il cluster Big Data di SQL Server 2019. I file di configurazione possono essere scaricati tramite il pulsante **Save config files** (Salva file di configurazione). Selezionare **Script to Notebook** (Script in notebook) per inserire lo script dell'intera configurazione della distribuzione in un notebook. Quando il notebook è aperto, selezionare **Run Cells** (Esegui celle) per avviare la distribuzione del cluster Big Data di SQL Server 2019 nella destinazione selezionata.

   ![Summary](media/notebooks-deploy/deploy-sql-server-big-data-cluster-on-a-new-AKS-cluster.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla distribuzione, vedere le [linee guida per la distribuzione di cluster Big Data di SQL Server](deployment-guidance.md).