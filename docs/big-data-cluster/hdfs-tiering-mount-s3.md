---
title: Montare S3 per la suddivisione in livelli HDFS
titleSuffix: SQL Server big data clusters
description: Questo articolo illustra come configurare la suddivisione in livelli HDFS per montare un file system S3 esterno in HDFS in un cluster Big Data di SQL Server 2019.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6fce2d87f618fec23f204d14cd813beea4ac74bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046511"
---
# <a name="how-to-mount-s3-for-hdfs-tiering-in-a-big-data-cluster"></a>Come montare S3 per la suddivisione in livelli HDFS in un cluster Big Data

Le sezioni seguenti forniscono un esempio di come configurare la suddivisione in livelli HDFS con un'origine dati di archiviazione S3.

## <a name="prerequisites"></a>Prerequisiti

- [Cluster Big Data distribuito](deployment-guidance.md)
- [Strumenti per Big Data](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**
- Creare e caricare dati in un bucket S3 
  - Caricare file CSV o Parquet nel bucket S3. Si tratta dei dati HDFS esterni che verranno montati in HDFS nel cluster Big Data.

## <a name="access-keys"></a>Chiavi di accesso

### <a name="set-environment-variable-for-access-key-credentials"></a>Impostare la variabile di ambiente per le credenziali della chiave di accesso

Aprire un prompt dei comandi in un computer client in grado di accedere al cluster Big Data. Impostare una variabile di ambiente usando il formato seguente. Si noti che le credenziali devono essere inserite in un elenco delimitato da virgole. Il comando "set" viene usato in Windows. Se si usa Linux, usare invece "export".

   ```text
    set MOUNT_CREDENTIALS=fs.s3a.access.key=<Access Key ID of the key>,
    fs.s3a.secret.key=<Secret Access Key of the key>
   ```

   > [!TIP]
   > Per altre informazioni su come creare chiavi di accesso S3, vedere [Chiavi di accesso S3](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys).

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a> Montare la risorsa di archiviazione HDFS remota

Dopo aver preparato un file di credenziali con chiavi di accesso, è ora possibile iniziare il montaggio. La procedura seguente consente di montare la risorsa di archiviazione HDFS remota in S3 nell'archiviazione HDFS locale del cluster Big Data.

1. Usare **kubectl** per trovare l'indirizzo IP per il servizio **controller-svc-external** dell'endpoint nel cluster Big Data. Cercare **External-IP**.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. Accedere con **azdata** usando l'indirizzo IP esterno dell'endpoint del controller con il nome utente e la password del cluster:

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080/
   ```
   
1. Impostare la variabile di ambiente MOUNT_CREDENTIALS seguendo le istruzioni sopra riportate

1. Montare la risorsa di archiviazione HDFS remota in Azure usando **azdata bdc hdfs mount create**. Sostituire i valori segnaposto prima di eseguire il comando seguente:

   ```bash
   azdata bdc hdfs mount create --remote-uri s3a://<S3 bucket name> --mount-path /mounts/<mount-name>
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

L'esempio seguente aggiorna il montaggio.

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
