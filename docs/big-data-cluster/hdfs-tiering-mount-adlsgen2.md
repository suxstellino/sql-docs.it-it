---
title: Montare ADLS Gen2 per la suddivisione in livelli HDFS
titleSuffix: How to mount ADLS Gen2
description: Questo articolo offre un esempio di come configurare la suddivisione in livelli HDFS con un'origine dati Azure Data Lake Storage Gen2.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 06/29/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b41ad6a56da30fb30677d781630e30ad5e07d333
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046551"
---
# <a name="how-to-mount-adls-gen2-for-hdfs-tiering-in-a-big-data-cluster"></a>Come montare ADLS Gen2 per la suddivisione in livelli HDFS in un cluster Big Data

Le sezioni seguenti forniscono un esempio di come configurare la suddivisione in livelli HDFS con un'origine dati Azure Data Lake Storage Gen2.

## <a name="prerequisites"></a>Prerequisiti

- [Cluster Big Data distribuito](deployment-guidance.md)
- [Strumenti per Big Data](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="load-data-into-azure-data-lake-storage"></a><a id="load"></a> Caricare i dati in Azure Data Lake Storage

La sezione seguente descrive come configurare Azure Data Lake Storage Gen2 per testare la suddivisione in livelli HDFS. Se si hanno già dati archiviati in Azure Data Lake Storage, è possibile ignorare questa sezione per usare i propri dati.

1. [Creare un account di archiviazione con le funzionalità di Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-quickstart-create-account).

1. [Creare un file system](/azure/storage/blobs/data-lake-storage-explorer) in questo account di archiviazione per i dati.

1. Caricare un file CSV o Parquet nel contenitore. Si tratta dei dati HDFS esterni che verranno montati in HDFS nel cluster Big Data.

## <a name="credentials-for-mounting"></a>Credenziali per il montaggio

### <a name="use-oauth-credentials-to-mount"></a>Usare le credenziali OAuth per il montaggio

Per usare le credenziali OAuth per il montaggio, è necessario seguire questa procedura:

1. Accedere al [portale di Azure](https://portal.azure.com)
1. Passare ad "Azure Active Directory". Il servizio è visualizzato sulla barra di spostamento a sinistra.
1. Sulla barra di spostamento a destra selezionare "Registrazioni app" e creare una nuova registrazione
1. Creare un'applicazione Web e seguire la procedura guidata. **Tenere a mente il nome dell'app creata in questa posizione**. Sarà necessario aggiungere questo nome all'account ADLS come utente autorizzato. Prendere nota anche dell'ID client dell'applicazione nella sezione di panoramica quando si seleziona l'app.
1. Dopo aver creato l'applicazione Web, passare a "Certificati e segreti", fare clic su **Nuovo segreto client** per crearne uno e selezionare la durata della chiave. Fare clic su **Aggiungi** per aggiungere il segreto.
1. Tornare alla pagina Registrazioni app e fare clic su "Endpoint" nella parte superiore. **Prendere nota dell'URL dell'endpoint di token OAuth (v2)**
1. A questo punto dovrebbero essere state annotate le informazioni seguenti per OAuth:

    - "ID client applicazione" per l'applicazione Web
    - Segreto client
    - Endpoint di token

### <a name="adding-the-service-principal-to-your-adls-account"></a>Aggiungere l'entità servizio all'account ADLS

1. Tornare al portale, passare al file system dell'account di archiviazione ADLS e selezionare Controllo di accesso (IAM) nel menu a sinistra.
1. Selezionare "Aggiungi un'assegnazione di ruolo". 
1. Selezionare il ruolo "Collaboratore ai dati dei BLOB di archiviazione".
1. Cercare il nome creato in precedenza (si noti che non viene visualizzato nell'elenco, ma è possibile trovarlo cercando il nome completo).
1. Salvare il ruolo.

Attendere 5-10 minuti prima di usare le credenziali per il montaggio

### <a name="set-environment-variable-for-oauth-credentials"></a>Impostare la variabile di ambiente per le credenziali OAuth

Aprire un prompt dei comandi in un computer client in grado di accedere al cluster Big Data. Impostare una variabile di ambiente usando il formato seguente. Le credenziali devono essere inserite in un elenco delimitato da virgole. Il comando "set" viene usato in Windows. Se si usa Linux, usare invece "export".

**Nota**: quando si forniscono le credenziali, è necessario rimuovere tutte le interruzioni di riga o lo spazio tra le virgole ",". La formattazione seguente è finalizzata solo a semplificare la lettura.

```console
   set MOUNT_CREDENTIALS=fs.azure.account.auth.type=OAuth,
   fs.azure.account.oauth.provider.type=org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider,
   fs.azure.account.oauth2.client.endpoint=[token endpoint],
   fs.azure.account.oauth2.client.id=[Application client ID],
   fs.azure.account.oauth2.client.secret=[client secret]
```

## <a name="use-access-keys-to-mount"></a>Usare le chiavi di accesso per il montaggio

È anche possibile eseguire il montaggio usando chiavi di accesso che è possibile ottenere per l'account ADLS nel portale di Azure.

 > [!TIP]
   > Per altre informazioni su come trovare la chiave di accesso (`<storage-account-access-key>`) per l'account di archiviazione, vedere [Visualizzare la stringa di connessione e le chiavi dell'account](/azure/storage/common/storage-account-keys-manage#view-access-keys-and-connection-string).

### <a name="set-environment-variable-for-access-key-credentials"></a>Impostare la variabile di ambiente per le credenziali della chiave di accesso

1. Aprire un prompt dei comandi in un computer client in grado di accedere al cluster Big Data.

1. Aprire un prompt dei comandi in un computer client in grado di accedere al cluster Big Data. Impostare una variabile di ambiente usando il formato seguente. Le credenziali devono essere inserite in un elenco delimitato da virgole. Il comando "set" viene usato in Windows. Se si usa Linux, usare invece "export".

**Nota**: quando si forniscono le credenziali, è necessario rimuovere tutte le interruzioni di riga o lo spazio tra le virgole ",". La formattazione seguente è finalizzata solo a semplificare la lettura.

```console
set MOUNT_CREDENTIALS=fs.azure.abfs.account.name=<your-storage-account-name>.dfs.core.windows.net,
fs.azure.account.key.<your-storage-account-name>.dfs.core.windows.net=<storage-account-access-key>
```

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a> Montare la risorsa di archiviazione HDFS remota

Ora che è stata impostata la variabile di ambiente MOUNT_CREDENTIALS per le chiavi di accesso o tramite OAuth, è possibile iniziare il montaggio. La procedura seguente consente di montare la risorsa di archiviazione HDFS remota in Azure Data Lake nell'archiviazione HDFS locale del cluster Big Data.

1. Usare **kubectl** per trovare l'indirizzo IP per il servizio **controller-svc-external** dell'endpoint nel cluster Big Data. Cercare **External-IP**.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. Accedere con **azdata** usando l'indirizzo IP esterno dell'endpoint del controller con il nome utente e la password del cluster:

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080
   ```
1. Impostare la variabile di ambiente MOUNT_CREDENTIALS (scorrere verso l'alto per istruzioni)

1. Montare la risorsa di archiviazione HDFS remota in Azure usando **azdata bdc hdfs mount create**. Sostituire i valori segnaposto prima di eseguire il comando seguente:

   ```bash
   azdata bdc hdfs mount create --remote-uri abfs://<blob-container-name>@<storage-account-name>.dfs.core.windows.net/ --mount-path /mounts/<mount-name>
   ```

   > [!NOTE]
   > Il comando mount create è asincrono. A questo punto, non sono presenti messaggi che indicano se il montaggio è riuscito o meno. Vedere la sezione relativa allo [stato](#status) per controllare lo stato dei montaggi.

Se il montaggio è riuscito, dovrebbe essere possibile eseguire una query sui dati di HDFS, nonché eseguire processi Spark. Il montaggio verrà visualizzato in HDFS per il cluster Big Data nel percorso specificato da `--mount-path`.

## <a name="get-the-status-of-mounts"></a><a id="status"></a> Ottenere lo stato dei montaggi

Per elencare lo stato di tutti i montaggi nel cluster Big Data, usare il comando seguente:

```bash
azdata bdc hdfs mount status
```

Per elencare lo stato di un montaggio in un percorso specifico in HDFS, usare il comando seguente:

```bash
azdata bdc hdfs mount status --mount-path <mount-path-in-hdfs>
```

## <a name="refresh-a-mount"></a>Aggiornare un montaggio

L'esempio seguente aggiorna il montaggio. Questo aggiornamento cancellerà anche la cache di montaggio.

```bash
azdata bdc hdfs mount refresh --mount-path <mount-path-in-hdfs>
```

## <a name="delete-the-mount"></a><a id="delete"></a> Eliminare il montaggio

Per eliminare il montaggio, usare il comando **azdata bdc hdfs mount delete** e specificare il percorso di montaggio in HDFS:

```bash
azdata bdc hdfs mount delete --mount-path <mount-path-in-hdfs>
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
