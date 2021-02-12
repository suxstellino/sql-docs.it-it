---
title: Usare curl per caricare dati in HDFS | Microsoft Docs
titleSuffix: SQL Server big data clusters
description: Usare curl per caricare dati nei sistemi HDFS dei cluster Big Data di SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2310ffa3849a9fc4b72cbb21e5f2f881fb447019
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038936"
---
# <a name="use-curl-to-load-data-into-hdfs-on-big-data-clusters-2019"></a>Usare curl per caricare dati nel sistema HDFS di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo illustra come usare **curl** per caricare dati nel sistema HDFS di [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md)

## <a name="obtain-the-service-external-ip"></a>Ottenere l'indirizzo IP esterno del servizio

Al termine della distribuzione viene avviato WebHDFS, a cui è possibile accedere attraverso Knox. L'endpoint Knox viene esposto tramite un servizio Kubernetes denominato **gateway-svc-external**.  Per creare l'URL WebHDFS necessario per caricare/scaricare i file, sono necessari l'indirizzo IP esterno del servizio **gateway-svc-external** e il nome del cluster Big Data. È possibile ottenere l'indirizzo IP esterno del servizio **gateway-svc-external** eseguendo il comando seguente:

```terminal
kubectl get service gateway-svc-external -n <big data cluster name> -o json | jq -r .status.loadBalancer.ingress[0].ip
```

> [!NOTE]
> `<big data cluster name>` è il nome del cluster specificato nel file di configurazione della distribuzione. Il nome predefinito è `mssql-cluster`.

## <a name="construct-the-url-to-access-webhdfs"></a>Costruire l'URL per accedere a WebHDFS

A questo punto, è possibile costruire l'URL per accedere a WebHDFS, come indicato di seguito:

`https://<gateway-svc-external service external IP address>:30443/gateway/default/webhdfs/v1/`

Ad esempio:

`https://13.66.190.205:30443/gateway/default/webhdfs/v1/`

## <a name="authentication-with-active-directory"></a>Autenticazione con Active Directory

Per le distribuzioni con Active Directory, usare il parametro di autenticazione con `curl` e l'autenticazione Negotiate. 

Per usare `curl` con l'autenticazione di Active Directory, eseguire questo comando:

```
kinit <username>
```

Il comando genera un token Kerberos per l'uso con `curl`. I comandi illustrati nelle sezioni seguenti specificano il parametro `--anyauth` per `curl`. Per gli URL che richiedono l'autenticazione Negotiate, `curl` rileva automaticamente e usa il token Kerberos generato anziché il nome utente e la password per l'autenticazione agli URL.

## <a name="list-a-file"></a>Elencare un file

Per elencare il file in **hdfs:///product_review_data**, usare il comando curl seguente:

```terminal
curl -i -k --anyauth -u root:<AZDATA_PASSWORD> -X GET 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/?op=liststatus'
```

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Per gli endpoint che non usano la radice, usare il comando curl seguente:

```terminal
curl -i -k --anyauth -u <AZDATA_USERNAME>:<AZDATA_PASSWORD> -X GET 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/?op=liststatus'
```

## <a name="put-a-local-file-into-hdfs"></a>Inserire un file locale in HDFS

Per inserire un nuovo file **test. csv** dalla directory locale alla directory product_review_data, usare il comando curl seguente (è necessario il parametro **Content-Type**):

```terminal
curl -i -L -k --anyauth -u root:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/test.csv?op=create' -H 'Content-Type: application/octet-stream' -T 'test.csv'
```

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Per gli endpoint che non usano la radice, usare il comando curl seguente:

```terminal
curl -i -L -k --anyauth -u <AZDATA_USERNAME>:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/test.csv?op=create' -H 'Content-Type: application/octet-stream' -T 'test.csv'
```

## <a name="create-a-directory"></a>Creare una directory

Per creare un **tes**  di directory in `hdfs:///`, usare il comando seguente:

```terminal
curl -i -L -k --anyauth -u root:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/test?op=MKDIRS'
```

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
Per gli endpoint che non usano la radice, usare il comando curl seguente:

```terminal
curl -i -L -k --anyauth -u <AZDATA_USERNAME>:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/test?op=MKDIRS'
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data di SQL Server, vedere [Che cos'è un cluster Big Data di SQL Server?](big-data-cluster-overview.md).
