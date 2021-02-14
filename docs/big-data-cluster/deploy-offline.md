---
title: Eseguire la distribuzione offline
titleSuffix: SQL Server big data clusters
description: Informazioni su come eseguire una distribuzione offline di un cluster Big Data di SQL Server 2019 e su come caricare le immagini del contenitore in un repository privato.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9cbe53718818116ae9a0c435a1cf985c132f759b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047277"
---
# <a name="perform-an-offline-deployment-of-a-sql-server-big-data-cluster"></a>Eseguire una distribuzione offline di un cluster Big Data di SQL Server

Questo articolo descrive come eseguire una distribuzione offline di un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. I cluster Big Data devono avere accesso a un repository Docker da cui eseguire il pull delle immagini dei contenitori. In un'installazione offline le immagini richieste vengono inserite in un repository Docker privato. Il repository privato viene quindi usato come origine delle immagini per una nuova distribuzione.

## <a name="prerequisites"></a>Prerequisites

- Motore Docker 1.8 o versione successiva in qualsiasi distribuzione di Linux supportata oppure Docker per Mac/Windows. Per altre informazioni, vedere [Installare Docker](https://docs.docker.com/engine/installation/).

## <a name="load-images-into-a-private-repository"></a>Caricare le immagini in un repository privato

I passaggi seguenti descrivono come eseguire il pull delle immagini dei contenitori dei cluster Big Data dal repository Microsoft e quindi eseguirne il push nel repository privato.

> [!TIP]
> I passaggi seguenti descrivono il processo. Per semplificare l'attività, tuttavia, è possibile usare lo [script automatico](#automated) invece di eseguire manualmente questi comandi.

1. Eseguire il pull delle immagini dei contenitori dei cluster Big Data ripetendo il comando seguente. Sostituire `<SOURCE_IMAGE_NAME>` con ogni [nome di immagine](#images). Sostituire `<SOURCE_DOCKER_TAG>` con il tag per la versione del cluster Big Data, ad esempio **2019-GDR1-ubuntu-16.04**.  

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG>
   ```

1. Accedere al registro Docker privato di destinazione.

   ```PowerShell
   docker login <TARGET_DOCKER_REGISTRY> -u <TARGET_DOCKER_USERNAME> -p <TARGET_DOCKER_PASSWORD>
   ```

1. Aggiungere un tag alle immagini locali con il comando seguente per ogni immagine:

   ```PowerShell
   docker tag mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG> <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```

1. Eseguire il push delle immagini locali nel repository Docker privato:

   ```PowerShell
   docker push <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```
 
> [!WARNING]
> Non modificare le immagini del cluster Big Data dopo che è stato eseguito il push nel repository privato. L'esecuzione di una distribuzione con immagini modificate comporterà una configurazione del cluster Big Data non supportata.


### <a name="big-data-cluster-container-images"></a><a id="images"></a> Immagini dei contenitori dei cluster Big Data

Per un'installazione offline, sono necessarie le immagini dei contenitori dei cluster Big Data seguenti:
- **mssql-app-service-proxy**
- **mssql-control-watchdog**
- **mssql-controller**
- **mssql-dns**
- **mssql-hadoop**
- **mssql-mleap-serving-runtime**
- **mssql-mlserver-py-runtime**
- **mssql-mlserver-r-runtime**
- **mssql-monitor-collectd**
- **mssql-monitor-elasticsearch**
- **mssql-monitor-fluentbit**
- **mssql-monitor-grafana**
- **mssql-monitor-influxdb**
- **mssql-monitor-kibana**
- **mssql-monitor-telegraf**
- **mssql-security-knox**
- **mssql-security-support**
- **mssql-server-controller**
- **mssql-server-data**
- **mssql-ha-operator**
- **mssql-ha-supervisor**
- **mssql-service-proxy**
- **mssql-ssis-app-runtime**


## <a name="automated-script"></a><a id="automated"></a> Script automatico

È possibile usare uno script Python automatico che effettua automaticamente il pull di tutte le immagini dei contenitori necessarie e il push in un repository privato.

> [!NOTE]
> Python è un prerequisito per l'uso dello script. Per altre informazioni su come installare Python, vedere la [documentazione di Python](https://wiki.python.org/moin/BeginnersGuide/Download).

1. Dalla shell Bash o da PowerShell scaricare lo script con **curl**:

   ```PowerShell
   curl -o push-bdc-images-to-custom-private-repo.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/offline/push-bdc-images-to-custom-private-repo.py"
   ```

1. Eseguire quindi lo script con uno dei comandi seguenti:

   **Windows:**

   ```PowerShell
   python push-bdc-images-to-custom-private-repo.py
   ```

   **Linux:**

   ```bash
   sudo python push-bdc-images-to-custom-private-repo.py
   ```

1. Seguire le istruzioni per immettere le informazioni sul repository Microsoft e sul repository privato. Al completamento dello script, tutte le immagini richieste dovrebbero trovarsi nel repository privato.

1. Seguire [queste](deployment-custom-configuration.md#docker) istruzioni per informazioni su come personalizzare il file di configurazione della distribuzione di `control.json` in modo da usare il registro contenitori e il repository. Prima della distribuzione, è necessario impostare le variabili di ambiente `DOCKER_USERNAME` e `DOCKER_PASSWORD` per consentire l'accesso al repository privato.

## <a name="install-tools-offline"></a>Installare gli strumenti offline

Le distribuzioni di cluster Big Data richiedono diversi strumenti, tra cui **Python**, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] e **kubectl**. Usare le procedure seguenti per installare questi strumenti in un server offline.

### <a name="install-python-offline"></a><a id="python"></a> Installare Python offline

1. In un computer con accesso a Internet, scaricare uno dei file compressi seguenti contenenti Python:

   | Sistema operativo | Download |
   |---|---|
   | Windows | [https://go.microsoft.com/fwlink/?linkid=2074021](https://go.microsoft.com/fwlink/?linkid=2074021) |
   | Linux   | [https://go.microsoft.com/fwlink/?linkid=2065975](https://go.microsoft.com/fwlink/?linkid=2065975) |
   | OSX     | [https://go.microsoft.com/fwlink/?linkid=2065976](https://go.microsoft.com/fwlink/?linkid=2065976) |

1. Copiare il file compresso nel computer di destinazione ed estrarlo nella cartella desiderata.

1. Solo per Windows, eseguire `installLocalPythonPackages.bat` dalla cartella di installazione e passare il percorso completo della cartella come parametro.

   ```PowerShell
   installLocalPythonPackages.bat "C:\python-3.6.6-win-x64-0.0.1-offline\0.0.1"
   ```

### <a name="install-azdata-offline"></a><a id="azdata"></a> Installare azdata offline

1. In un computer con accesso a Internet e con [Python](https://wiki.python.org/moin/BeginnersGuide/Download) eseguire il comando seguente per scaricare tutti i pacchetti di [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] nella cartella corrente.

   ```PowerShell
   pip download -r https://aka.ms/azdata
   ```

1. Copiare i pacchetti scaricati e il file `requirements.txt` nel computer di destinazione.

1. Eseguire il comando seguente nel computer di destinazione, specificando la cartella in cui sono stati copiati i file in precedenza.

   ```PowerShell
   pip install --no-index --find-links <path-to-packages> -r <path-to-requirements.txt>
   ```

### <a name="install-kubectl-offline"></a><a id="kubectl"></a> Installare kubectl offline

Per installare **kubectl** in un computer offline, seguire questa procedura.

1. Usare **curl** per scaricare **kubectl** nella cartella desiderata. Per altre informazioni, vedere [Installare i file binari di kubectl usando curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl).

1. Copiare la cartella nel computer di destinazione.

## <a name="deploy-from-private-repository"></a>Eseguire la distribuzione dal repository privato

Per eseguire la distribuzione dal repository privato, seguire la procedura descritta nella [guida alla distribuzione](deployment-guidance.md), ma usare un file di configurazione della distribuzione personalizzato che specifichi le informazioni sul repository Docker privato. I comandi [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] seguenti illustrano come modificare le impostazioni di Docker in un file di configurazione della distribuzione personalizzato denominato `control.json`:

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.repository=<your-docker-repository>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.registry=<your-docker-registry>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.imageTag=<your-docker-image-tag>"
```

La distribuzione richiede il nome utente e la password Docker oppure è possibile specificarli nelle variabili di ambiente `DOCKER_USERNAME` e `DOCKER_PASSWORD`.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle distribuzioni di cluster Big Data, vedere [Come distribuire [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).
