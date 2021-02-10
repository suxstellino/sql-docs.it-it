---
title: Installare in Docker
titleSuffix: SQL Server Machine Learning Services
description: Informazioni su come installare SQL Server Machine Learning Services (Python e R) in Docker.
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 05/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 1e6e658023419a9a676b861efb8769a5bbf6cadf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030795"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-docker"></a>Installare SQL Server Machine Learning Services (Python e R) in Docker

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

Questo articolo illustra come installare SQL Server Machine Learning Services in Docker. È possibile usare Machine Learning Services per eseguire gli script Python e R nel database. Non vengono forniti contenitori predefiniti con Machine Learning Services. È possibile crearne uno dai contenitori di SQL Server usando [un modello di esempio disponibile in GitHub](https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/examples/mssql-mlservices).

## <a name="prerequisites"></a>Prerequisiti

- Interfaccia della riga di comando di Git.

- Motore Docker 1.8 o versione successiva in qualsiasi distribuzione di Linux supportata oppure Docker per Mac/Windows. Per altre informazioni, vedere [Get Docker](https://docs.docker.com/get-docker/) (Ottenere Docker).

- Vedere anche i [requisiti di sistema per SQL Server in Linux](sql-server-linux-setup.md#system).

## <a name="clone-the-mssql-docker-repository"></a>Clonare il repository mssql-docker

Il comando seguente consente di clonare il repository git `mssql-docker` in una directory locale.

1. Aprire un terminale Bash in Linux o Mac.

2. Creare una directory in cui mantenere una copia locale del repository mssql-docker.

3. Eseguire il comando git clone per clonare il repository mssql-docker:

    ```bash
    git clone https://github.com/microsoft/mssql-docker mssql-docker
    ```

## <a name="build-a-sql-server-linux-container-image"></a>Creare un'immagine del contenitore di SQL Server su Linux

Completare i passaggi seguenti per creare l'immagine docker:

1. Passare alla directory mssql-mlservices:
    
    ```bash
    /mssql-docker/linux/preview/examples/mssql-mlservices
    ```

2. Nella stessa directory eseguire il comando seguente:

    ```bash
    docker build -t mssql-server-mlservices .
    ```

3. Eseguire il comando:

    ```bash
    docker run -d -e MSSQL_PID=Developer -e ACCEPT_EULA=Y -e ACCEPT_EULA_ML=Y -e MSSQL_SA_PASSWORD=<password> -v <directory on the host OS>:/var/opt/mssql -p 1433:1433 mssql-server-mlservices
    ```
  
    > [!NOTE]
    > Tutti i valori seguenti possono essere usati per MSSQL_PID: Developer (gratuito), Express (gratuito), Enterprise (a pagamento), Standard (a pagamento). Se si usa un'edizione a pagamento, assicurarsi di avere acquistato una licenza. Sostituire (password) con la password effettiva. Il montaggio del volume con -v è facoltativo. Sostituire (directory sul sistema operativo host) con una directory effettiva in cui si prevede di montare i file di dati e di log del database.
    

4. Confermare eseguendo questo comando:

    ```bash
    docker ps -a
    ```

   > [!NOTE]
   > Per creare l'immagine Docker è necessario installare pacchetti di dimensioni pari a diversi GB. Il completamento dell'esecuzione dello script può richiedere tempo, a seconda della larghezza di banda di rete.

## <a name="run-the-sql-server-linux-container-image"></a>Eseguire l'immagine del contenitore di SQL Server su Linux

1. Impostare le variabili di ambiente prima di eseguire il contenitore. Impostare la variabile di ambiente PATH_TO_MSSQL su una directory host:

   ```bash
   export MSSQL_PID='Developer'
   export ACCEPT_EULA='Y'
   export ACCEPT_EULA_ML='Y'
   export PATH_TO_MSSQL='/home/mssql/'
   ```
  
   > [!NOTE]
   > Il processo di esecuzione delle edizioni di SQL Server di produzione nei contenitori è leggermente diverso. Per altre informazioni, vedere [Configurare immagini dei contenitori SQL Server in Docker](./sql-server-linux-docker-container-deployment.md). Se si usano gli stessi nomi di contenitore e le stesse porte, il resto di questa procedura dettagliata funziona comunque con i contenitori di produzione.

2. Per visualizzare i contenitori di Docker, usare il comando `docker ps`:

   ```bash
   sudo docker ps -a
   ```

3. Se nella colonna **STATUS** è impostato lo stato **Up**, SQL Server è in esecuzione nel contenitore e in ascolto sulla porta specificata nella colonna **PORTS**. Se la colonna **STATUS** del contenitore di SQL Server è impostata su **Exited**, vedere la [sezione relativa alla risoluzione dei problemi della guida alla configurazione](./sql-server-linux-docker-container-troubleshooting.md).

 
    Output:

    ```
    CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
    941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour     0.0.0.0:1401->1433/tcp   sql1
    ```

## <a name="enable-machine-learning-services"></a>Abilitare Machine Learning Services

Per abilitare Machine Learning Services, connettersi all'istanza di SQL Server ed eseguire l'istruzione T-SQL seguente:

```sql
EXEC sp_configure  'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE
```

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori Python possono apprendere come usare Python con SQL Server seguendo queste esercitazioni:

+ [Esercitazione su Python: Stimare il noleggio di sci con la regressione lineare in Machine Learning Services per SQL Server](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Esercitazione su Python: Categorizzazione dei clienti tramite clustering K-Means con Machine Learning Services per SQL Server](../machine-learning/tutorials/python-clustering-model.md)

Gli sviluppatori R possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di R con SQL Server. Per il passaggio successivo, vedere i collegamenti seguenti:

+ [Avvio rapido: Eseguire R in T-SQL](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [Esercitazione: Analisi nel database per sviluppatori R](../machine-learning/tutorials/r-taxi-classification-introduction.md)