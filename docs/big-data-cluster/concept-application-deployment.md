---
title: Che cos'è la distribuzione di applicazioni?
titleSuffix: SQL Server Big Data Clusters
description: Informazioni su come la distribuzione delle applicazioni offre le interfacce necessarie per creare, gestire ed eseguire le applicazioni in un cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/12/2021
ms.topic: conceptual
ms.prod: sql
dev_langs:
- yaml
- console
ms.technology: big-data-cluster
ms.openlocfilehash: 6738f43ca2b995f73ecc554406b90eabeb3424e2
ms.sourcegitcommit: 52dd1719d7b63581b1d34b755bf9d077c0fc6c44
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2021
ms.locfileid: "107372986"
---
# <a name="what-is-application-deployment-on-a-sql-server-big-data-cluster"></a>Che cos'è la distribuzione di applicazioni in un cluster Big Data di SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La distribuzione di applicazioni consente la distribuzione di applicazioni SQL Server cluster Big Data (BDC) fornendo interfacce per creare, gestire ed eseguire applicazioni. Le applicazioni distribuite in un cluster BDC traggono vantaggio dalla potenza di calcolo del cluster e possono accedere ai dati disponibili nel cluster. In questo modo, viene aumentata la scalabilità e le prestazioni delle applicazioni nel caso in cui le applicazioni gestite si trovino nella posizione in cui risiedono i dati. I runtime dell'applicazione supportati [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in sono: R, Python, dtexec e MLeap.

Le sezioni seguenti descrivono l'architettura e le funzionalità della distribuzione di applicazioni.

## <a name="application-deployment-architecture"></a>Architettura della distribuzione di applicazioni

La distribuzione di applicazioni è costituita da un controller e da gestori di runtime delle app. Quando si crea un'applicazione, viene fornito un file di specifica (`spec.yaml`). Questo file `spec.yaml` contiene tutte le informazioni necessarie al controller per distribuire correttamente l'applicazione. Di seguito è riportato un esempio di contenuti per `spec.yaml`:

```yaml
#spec.yaml
name: add-app #name of your python script
version: v1  #version of the app
runtime: Python #the language this app uses (R or Python)
src: ./add.py #full path to the location of the app
entrypoint: add #the function that will be called upon execution
replicas: 1  #number of replicas needed
poolsize: 1  #the pool size that you need your app to scale
inputs:  #input parameters that the app expects and the type
  x: int
  y: int
output: #output parameter the app expects and the type
  result: int
```

Il controller esamina il `runtime` specificato nel file `spec.yaml` e chiama il gestore di runtime corrispondente. Il gestore di runtime crea quindi l'applicazione. Per prima cosa, viene creato un set di repliche Kubernetes contenente uno o più POD, ognuno dei quali contiene l'applicazione da distribuire. Il numero di POD è definito dal set di parametri `replicas` nel file `spec.yaml` per l'applicazione. Ogni pod può includere uno o più pool. Il numero di pool è definito dal set di parametri `poolsize` nel file `spec.yaml`.

Queste impostazioni determinano la quantità di richieste che la distribuzione può gestire in parallelo. Il numero massimo di richieste contemporanee è uguale alla `replicas` per il numero di `poolsize`. Se si hanno 5 repliche e 2 pool per replica, quindi, la distribuzione potrà gestire 10 richieste in parallelo. Vedere l'immagine seguente per una rappresentazione grafica di `replicas` e `poolsize`:

![Dimensione del pool e repliche](media/big-data-cluster-create-apps/poolsize-vs-replicas.png)

Dopo la creazione del set di repliche e l'avvio dei pod, viene creato un processo cron se nel file `schedule` è stato impostato un oggetto `spec.yaml`. Viene creato infine un servizio Kubernetes che può essere usato per gestire ed eseguire l'applicazione (vedere di seguito).

Quando viene eseguita un'applicazione, il servizio Kubernetes dell'applicazione inoltra le richieste a una replica e restituisce i risultati.

## <a name="security-considerations-for-applications-deployments-on-openshift"></a><a id="app-deploy-security"></a> Considerazioni sulla sicurezza per la distribuzione di applicazioni

SQL Server 2019 CU5 abilita il supporto per la distribuzione di BDC in Red Hat OpenShift e un modello di sicurezza aggiornato per il BDC in modo che i contenitori con privilegi non siano più necessari. Oltre ai contenitori senza privilegi, i contenitori vengono eseguiti come utente non radice per impostazione predefinita per tutte le nuove distribuzioni che usano [SQL Server 2019 CU5.](release-notes-big-data-cluster.md#cu5)

Al momento della versione CU5, il passaggio di installazione delle applicazioni distribuite con le interfacce [app deploy](app-create.md) verrà comunque eseguito come utente *ROOT*. Questa operazione è necessaria perché durante l'installazione vengono installati pacchetti aggiuntivi che verranno utilizzati dall'applicazione. Altro codice utente distribuito come parte dell'applicazione verrà eseguito come utente con privilegi limitati. 

Inoltre, la funzionalità è una funzionalità facoltativa necessaria per consentire la pianificazione `CAP_AUDIT_WRITE` di SQL Server Integration Services (SSIS) tramite processi cron. Quando il file di specifica yaml dell'applicazione specifica una pianificazione, l'applicazione verrà attivata tramite un processo cron, che richiede la funzionalità aggiuntiva. In alternativa, l'applicazione può essere attivata su richiesta con tramite una chiamata al servizio Web, che `azdata app run` non richiede la funzionalità `CAP_AUDIT_WRITE` . Si noti `CAP_AUDIT_WRITE` che la funzionalità non è più necessaria a partire SQL Server versione `cronjob` 2019 CU8. 



> [!NOTE]
> L'SCC personalizzato nell'articolo sulla distribuzione di [OpenShift](deploy-openshift.md) non include questa funzionalità perché non è necessaria per una distribuzione predefinita di BDC. Per abilitare questa funzionalità, è prima necessario aggiornare il file yaml SCC personalizzato per includere CAP_AUDIT_WRITE.

```yaml
...
allowedCapabilities:
- SETUID
- SETGID
- CHOWN
- SYS_PTRACE
- AUDIT_WRITE
...
```

## <a name="how-to-work-with-app-deploy-inside-big-data-cluster"></a>Come usare la distribuzione di app all'interno di un cluster Big Data

Le due interfacce principali per la distribuzione di applicazioni sono: 

- [Interfaccia della riga di comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](app-create.md)
- [Estensione di Visual Studio Code e Azure Data Studio](app-deployment-extension.md)

Un'applicazione può essere eseguita anche mediante un servizio Web RESTful. Per altre informazioni, vedere [Utilizzare applicazioni in cluster Big Data](app-consume.md).

## <a name="app-deploy-scenarios"></a>Scenari di distribuzione di app

La distribuzione di applicazioni consente la distribuzione di applicazioni in un SQL Server BDC fornendo interfacce per creare, gestire ed eseguire applicazioni.

:::image type="content" source="media/concept-application-deployment/big-data-cluster-app-pool-process-overview.png" alt-text="Identificare le origini (R, Python, SSIS (dtexec), distribuirle con la riga di comando, Azure Data Studio o Visual Studio Code e usarle con una pianificazione interattiva dell'API RESTful.":::

Di seguito sono riportati gli scenari di destinazione per la distribuzione di app:

- Distribuire servizi Web Python o R all'interno del cluster BDC per risolvere diversi casi d'uso, ad esempio l'inferenza di Machine Learning, la gestione delle API e così via.
- Creare un endpoint di inferenza di Machine Learning usando il motore MLeap.
- Pianificare ed eseguire pacchetti da file DTSX usando l'utilità dtexec per la trasformazione e lo spostamento dei dati.

### <a name="use-app-deploy-python-runtime"></a>Usare il runtime python di distribuzione dell'app

Nella distribuzione di app, il runtime Python BDC consente all'applicazione Python all'interno del cluster BDC di risolvere diversi casi d'uso, ad esempio l'inferenza di Machine Learning, la gestione delle API e altro ancora.

Python 3.5 per Ubuntu 16.04 e Python 3.8 per Ubuntu 20.04.

Nella distribuzione `spec.yaml` dell'app vengono fornite le informazioni che il controller deve conoscere per distribuire l'applicazione. Di seguito sono riportati i campi che è possibile specificare:

- `name`: nome dell'applicazione
- `version`: la versione dell'applicazione, ad esempio `v1`
- `runtime`: il runtime di distribuzione dell'app, è necessario specificarlo come: `Python`
- `src`: percorso dell'applicazione Python
- `entry point`: funzione del punto di ingresso nello script src da eseguire per questa applicazione Python.

Oltre a sopra, è necessario specificare l'input e l'output dell'applicazione Python. Verrà generato un `spec.yaml` file simile al seguente:

```yaml
#spec.yaml
name: add-app
version: v1
runtime: Python
src: ./add.py
entrypoint: add
replicas: 1
poolsize: 1
inputs:
  x: int
  y: int
output:
  result: int
```

È possibile creare la struttura di file e cartella di base necessaria per distribuire un'app Python in esecuzione nel cluster BDC:

```console
azdata app init --template python --name hello-py --version v1
```

Per i passaggi successivi, [vedere Come distribuire un'app SQL Server cluster Big Data.](app-create.md)

#### <a name="app-deploy-python-runtime-limitations"></a>Limitazioni del runtime di distribuzione di Python per l'app

Il runtime python di distribuzione dell'app non supporta lo scenario di pianificazione. Dopo la distribuzione dell'app Python e quindi l'esecuzione in BDC, viene configurato un endpoint RESTful per l'ascolto delle richieste in ingresso.

### <a name="use-app-deploy-r-runtime"></a>Usare il runtime R di distribuzione dell'app

Nella distribuzione di app, il runtime Python del cluster BDC consente all'applicazione R all'interno del cluster BDC di risolvere diversi casi d'uso, ad esempio l'inferenza di Machine Learning, la gestione delle API e altro ancora.

Il runtime R di distribuzione dell'app supporta Microsoft R Open (MRO) 3.5.2.

#### <a name="how-to-use-it"></a>Come usarlo?

Nella distribuzione dell'app è `spec.yaml` la posizione in cui si forniscono le informazioni che il controller deve conoscere per distribuire l'applicazione. Di seguito sono riportati i campi che è possibile specificare:

- `name`: nome dell'applicazione
- `version`: la versione dell'applicazione, ad esempio `v1`
- `runtime`: il runtime di distribuzione dell'app, è necessario specificarlo come: `R`
- `src`: percorso dell'applicazione R
- `entry point`: il punto di ingresso per eseguire questa applicazione R

Oltre a quello precedente, è necessario specificare l'input e l'output dell'applicazione R. Verrà generato un `spec.yaml` file simile al seguente:

```yaml
#spec.yaml
name: roll-dice
version: v1
runtime: R
src: ./roll-dice.R
entrypoint: rollEm
replicas: 1
poolsize: 1
inputs:
  x: integer
output:
  result: data.fram
```

È possibile creare la struttura di file e cartelle di base necessaria per distribuire una nuova applicazione R usando il comando seguente:

```console
azdata app init --template r --name hello-r --version v1
```

Per i passaggi successivi, [vedere Come distribuire un'app SQL Server cluster Big Data.](app-create.md)

#### <a name="more-details-on-limitations"></a>Altre informazioni sulle limitazioni

La limitazione è in linea con [Microsoft R Application Network.](https://mran.microsoft.com/open)

### <a name="using-app-deploy-dtexec-runtime"></a>Uso del runtime dtexec di app deploy

Nella distribuzione dell'app, l'utilità dtexec integrata nel runtime del data center BDC, che deriva da SSIS in Linux (mssql-server-is) . Distribuzione di app usa l'utilità dtexec per caricare pacchetti da file *.dtsx. Supporta l'esecuzione di pacchetti SSIS in base a una pianificazione in stile cron o su richiesta tramite richieste di servizio Web.

Questa funzionalità usa `/opt/ssis/bin/dtexec /FILE` da SQL Server 2019 Integration Service in Linux, supporta il formato dtsx per SQL Server [2019 Integration Service in Linux (mssql-server-is 15.0.2)](../linux/sql-server-linux-setup-ssis.md). Per altre informazioni sull'utilità dtexec, vedere [Utilità dtexec](../integration-services/packages/dtexec-utility.md).

Nella distribuzione `spec.yaml` dell'app vengono fornite le informazioni che il controller deve conoscere per distribuire l'applicazione. Di seguito sono riportati i campi che è possibile specificare:

- `name`: l'applicazione `name`
- `version`: la versione dell'applicazione, ad esempio `v1`
- `runtime`: il runtime di distribuzione dell'app, per eseguire l'utilità dtexec, è necessario specificarlo come: `SSIS`
- `entrypoint`: specificare un punto di ingresso, in genere si tratta del file con estensione dtsx in questo caso.
- `options`: specificare opzioni aggiuntive per , ad esempio per connettersi a un database con stringa di `/opt/ssis/bin/dtexec /FILE` connessione, seguendo il modello seguente: 

   ```console
   /REP V /CONN "sqldatabasename"\;"\"Data Source=xx;User ID=xx;Password=xx\""
   ```

  Per informazioni dettagliate sulla sintassi, vedere [Utilità dtexec](../integration-services/packages/dtexec-utility.md).

- `schedule`: specificare la frequenza con cui il processo deve essere eseguito, per le istanze, quando si usa l'espressione cron per specificare questo valore come "*/1 * * * *" che indica che il processo viene eseguito in base ai minuti.

È possibile creare la struttura di file e cartella di base necessaria per distribuire una nuova applicazione SSIS usando il comando seguente:

```console
azdata app init --name hello-is –version v1 --template ssis                                 
```

In questo modo viene `spec.yaml` generato un file nel modo seguente:

```yaml
#spec.yaml
entrypoint: ./hello.dtsx
name: hello-is
options: /REP V
poolsize: 2
replicas: 2
runtime: SSIS
schedule: '*/2 * * * *'
version: v1
```

Nell'esempio viene creato anche un pacchetto `hello.dtsx` di esempio.

Tutti i file dell'app sono nella stessa directory di `spec.yaml` . Deve essere al livello radice della directory del codice sorgente `spec.yaml` dell'app, incluso il file dtsx.

Per i passaggi successivi, [vedere Come distribuire un'app SQL Server cluster Big Data.](app-create.md)

#### <a name="limitations-of-dtsx-utility"></a>Limitazioni dell'utilità dtsx

Per questa funzionalità vengono applicate tutte le limitazioni e i problemi noti per SQL Server Integration Services (SSIS) in Linux. Per altre informazioni, vedere [Limitazioni e problemi noti di SSIS in Linux](../linux/sql-server-linux-ssis-known-issues.md)

### <a name="using-app-deploy-mleap-runtime"></a>Uso del runtime MLeap per la distribuzione di app

Il runtime MLeap di distribuzione dell'app supporta MLeap Serving v0.13.0.

Nella distribuzione dell'app è `spec.yaml` la posizione in cui si forniscono le informazioni che il controller deve conoscere per distribuire l'applicazione. Di seguito sono riportati i campi che è possibile specificare:

- `name`: nome dell'applicazione 
- `version`: la versione dell'applicazione, ad esempio , ad esempio `v1` 
- `runtime`: il runtime di distribuzione dell'app, è necessario specificarlo come: `Mleap`

Oltre a quello riportato in precedenza, è `bundleFileName` necessario specificare l'oggetto dell'applicazione MLeap. Verrà generato un `spec.yaml` file simile al seguente:

```yaml
#spec.yaml
name: mleap-census
version: v1
runtime: Mleap
bundleFileName: census-bundle.zip
replicas: 1
```

È possibile creare la struttura di file e cartelle di base necessaria per distribuire una nuova applicazione MLeap usando il comando seguente:

```console
azdata app init --template mleap --name hello-mleap --version v1
```

Per i passaggi successivi, [vedere Come distribuire un'app SQL Server cluster Big Data.](app-create.md)

#### <a name="mleap-limitations"></a>Limitazioni di MLeap

Le limitazioni sono allineate alla visione del progetto open source MLeap di combust su [GitHub.](https://github.com/combust/mleap)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come creare ed eseguire applicazioni in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere gli argomenti seguenti:

- [Distribuire applicazioni con azdata](app-create.md)
- [Distribuire applicazioni usando l'estensione di distribuzione dell'app](app-deployment-extension.md)
- [Utilizzare applicazioni in cluster Big Data](app-consume.md)

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere la panoramica seguente:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
