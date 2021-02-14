---
title: Eseguire applicazioni con azdata
titleSuffix: SQL Server Big Data Clusters
description: Esecuzione di applicazioni con azdata nei cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b83c79398dbcc6c7d55eb0ddfe6cb1d8f0d398ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039591"
---
# <a name="run-apps-with-azdata---sql-server-big-data-clusters"></a>Eseguire le applicazioni con azdata - Cluster Big Data di SQL Server

Questo articolo descrive come eseguire un'applicazione in cluster Big Data di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- [Cluster Big Data di SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capabilities

In SQL Server 2019 è possibile creare, eliminare, descrivere, inizializzare, elencare, eseguire e aggiornare l'applicazione. La tabella seguente descrive i comandi per la distribuzione di applicazioni che è possibile usare con **azdata**.

|Comando |Descrizione |
|:---|:---|
|`azdata app describe` | Descrive un'applicazione. |
|`azdata app run` | Esegue un'applicazione. |


Le sezioni seguenti descrivono più dettagliatamente questi comandi.


## <a name="run-an-app"></a>Eseguire un'app

Se lo stato dell'app è `Ready`, è possibile usarla eseguendola con i parametri di input specificati. Usare la sintassi seguente per eseguire un'app:

```bash
azdata app run --name <app_name> --version <app_version> --inputs <inputs_params>
```

L'esempio seguente mostra il comando run:

```bash
azdata app run --name add-app --version v1 --inputs x=1,y=2
```

Se l'esecuzione riesce, l'output sarà lo stesso di quello specificato quando è stata creata l'app. Di seguito è riportato un esempio di risultato.

```json
{
  "changedFiles": [],
  "consoleOutput": "",
  "errorMessage": "",
  "outputFiles": {},
  "outputParameters": {
    "result": 3
  },
  "success": true
}
```


## <a name="describe-an-app"></a>Descrivere un'app

Il comando describe fornisce informazioni dettagliate sull'app, tra cui l'endpoint nel cluster. Viene usato in genere da uno sviluppatore di app per compilare un'app tramite il client Swagger e il servizio Web per interagire con l'app in modo RESTful. Per altre informazioni, vedere [Utilizzare applicazioni in cluster Big Data](app-consume.md).

```json
{
  "input_param_defs": [
    {
      "name": "x",
      "type": "int"
    },
    {
      "name": "y",
      "type": "int"
    }
  ],
  "links": {
    "app": "https://10.1.1.3:30080/api/app/add-app/v1",
    "swagger": "https://10.1.1.3:30080/api/app/add-app/v1/swagger.json"
  },
  "name": "add-app",
  "output_param_defs": [
    {
      "name": "result",
      "type": "int"
    }
  ],
  "state": "Ready",
  "version": "v1"
}
```

Per altre informazioni su come integrare app distribuite in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] nelle proprie applicazioni, vedere [Utilizzare applicazioni in cluster Big Data](app-consume.md). È anche possibile fare riferimento ad altri esempi in [Esempi di distribuzione di app](https://aka.ms/sql-app-deploy).

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).