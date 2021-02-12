---
title: Risoluzione dei problemi dei cluster Big Data con notebook Jupyter e Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Risoluzione dei problemi dei cluster Big Data con notebook Jupyter e Azure Data Studio nel cluster Big Data di SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 550c56a7ceb82b1215e6e921adb68a1438e87464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048241"
---
# <a name="troubleshooting-big-data-clusters-bdc-with-notebooks"></a>Risoluzione dei problemi dei cluster Big Data con i notebook

Questa pagina è un indice dei notebook per i cluster Big Data di SQL Server. I notebook eseguibili (con estensione ipynb) sono progettati per facilitare la risoluzione dei problemi dei cluster Big Data in SQL Server 2019.

Ogni notebook è progettato per cercare le proprie dipendenze. Un comando **Run all cells** (Esegui tutte le celle) viene eseguito correttamente oppure genera un'eccezione con un hint con collegamento ipertestuale a un altro notebook per risolvere la dipendenza mancante. Seguire il collegamento ipertestuale dell'hint al notebook successivo, scegliere **Run all cells** (Esegui tutte le celle) e, al termine, tornare al notebook originale e scegliere di nuovo **Run all cells** (Esegui tutte le celle).

Se vengono installate tutte le dipendenze, ma il comando **Run all cells** (Esegui tutte le celle) non viene eseguito, ogni notebook analizzerà i risultati e, dove possibile, produrrà un hint con collegamento ipertestuale a un altro notebook per facilitare la risoluzione del problema.


## <a name="troubleshooting-big-data-cluster-bdc"></a>Risoluzione dei problemi del cluster Big Data

Questa sezione contiene un set di notebook per ottenere i log da un cluster Big Data di SQL Server.

|Nome |Descrizione |
|---|---|---|---|
|TSG100 - Strumento di risoluzione dei problemi del cluster Big Data|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi del cluster Big Data e informazioni su quando usarli  |
|TSG101 - Strumento di risoluzione dei problemi di SQL Server|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi di SQL Server e informazioni su quando usarli  |
|TSG102 - Strumento di risoluzione dei problemi relativi a HDFS|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi di HDFS e informazioni su quando usarli  |
|TSG103 - Strumento di risoluzione dei problemi relativi a Spark|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi di Spark e informazioni su quando usarli  |
|TSG104 - Strumento di risoluzione dei problemi di controllo|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi del controller e informazioni su quando usarli  |
|TSG105 - Strumento di risoluzione dei problemi relativi al gateway|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi del gateway Knox e informazioni su quando usarli  |
|TSG106 - Strumento di risoluzione dei problemi relativi alle app|Panoramica di tutti i notebook disponibili per la risoluzione dei problemi di distribuzione delle app e informazioni su quando usarli  |



## <a name="diagnose-issues-from-big-data-clusters-bdc"></a>Diagnosticare i problemi da cluster Big Data

Set di notebook per la diagnosi di situazioni e stati con un cluster Big Data.

|Nome |Descrizione |
|---|---|---|---|
|TSG002 - CrashLoopBackoff|Questo TSG si connette a ogni contenitore il cui ultimo tentativo di entrare in uno stato di esecuzione non è riuscito e ottiene i log dei contenitori correnti e precedenti. Questa operazione è utile per il debug dei problemi CrashLoopBackOff segnalati nei pod di kubectl get.|
|TSG025 - Browser FSM - Query stato FSM controller|Usare questo notebook per connettersi al database del controller e visualizzare lo stato della macchina a stati finiti (FSM, Finite State Machine). Usare questo notebook per elencare le macchine a stati attive e identificare i flussi di lavoro bloccati.|
|TSG026 - Connettersi al nodo del pool di dati (per eseguire T-SQL)|Usare questo notebook per connettersi al nodo del pool di dati (per eseguire T-SQL)|
|TSG027 - Osservare la distribuzione del cluster|Usare questo notebook per osservare la distribuzione del cluster. Il notebook offre guide per risolvere i problemi di creazione di cluster Big Data di SQL Server. I comandi seguenti risultano spesso utili per individuare le cause sottostanti. |
|TSG029 - Trovare dump nel cluster|Usare questo notebook per cercare core dump e minidump da processi come SQL Server o controller in un cluster Big Data.|
|TSG032 - Utilizzo di CPU e memoria per tutti i contenitori|Usare questo notebook per controllare l'utilizzo di CPU e memoria per tutti i contenitori.|
|TSG037 - Determinare il pod del pool master che ospita la replica primaria|Usare questo notebook per determinare il pod del pool master che ospita la replica primaria per il cluster Big Data quando è abilitata la disponibilità elevata del pool master.|
|TSG044 - Eseguire sqlcmd nel contenitore del pool master|Usare questo notebook per connettersi direttamente a un nodo del pool master tramite T-SQL.|
|TSG055 - Tempo Curl in Sparkhead|Usare questo notebook per diagnosticare il passaggio per individuare il tempo di risposta Curl dal pod Controller al pod Sparkhead.|
|TSG060 - Spazio su disco del volume permanente per tutti i PVC BDC|Usare questo notebook per connettersi a ogni contenitore e ottenere lo spazio su disco usato/disponibile per ogni volume permanente (PV) mappato a ogni attestazione di volume permanente (PVC) di un cluster Big Data.|
|TSG078 - Cluster integro|Usare questo notebook per verificare l'integrità del cluster Big Data.|
|TSG079 - Generare core dump di controller|Usare questo notebook per generare core dump del controller.|
|TSG086 - Eseguire top in tutti i contenitori|Usare questo notebook per eseguire top in tutti i contenitori.|
|TSG087 - Usare l'interfaccia della riga di comando hadoop fs sul pod namenode|Usare questo notebook per usare l'interfaccia della riga di comando hadoop fs sul pod namenode.|
|TSG108 - Visualizzare il mapping di configurazione di aggiornamento del controller|Usare questo notebook per risolvere l'errore durante l'esecuzione dell'aggiornamento di un cluster Big Data usando **azdata bdc upgrade**.|
|TSG112 - Controlli di pre-distribuzione di Active Directory|Usare questo notebook per verificare che la configurazione di un cluster Big Data sia valida per una distribuzione di Active Directory (AD).|
|TSG115 - Convertitore log di sicurezza SQL Server in Linux|Usare questo notebook per analizzare i log generati dai logger secuirty.ldap e security.kerberos per SQL Server in Linux. Per abilitare questi logger, inserire le righe seguenti in /var/opt/mssql/logger.ini nel computer che esegue SQL Server in Linux. Nota: nel file viene fatta distinzione tra maiuscole e minuscole.|
|TSG116 - Convertitore log di supporto di sicurezza del cluster Big Data SQL|Usare questo notebook per analizzare i log generati dal servizio di supporto di sicurezza nel cluster Big Data SQL. Per ottenere i log, i log di debug vengono copiati dal cluster ed estratti. Seguire questa procedura - eseguire "azdata bdc debug copy-logs -n <namespace> *". Verranno creati più file con estensione tar.gz - Estrarre i contenuti di debuglogs-* <namespace>-<date>-<time>.tar.gz - Individuare il log di supporto di sicurezza archiviato in ./<namespace>/control-<…>/security-support/supervisol/log/secsupp-stderr---<…>.log.|
|TSG119 - Controlli di post-distribuzione di Active Directory|Questo notebook è progettato per convalidare la configurazione del cluster Big Data dopo una distribuzione di AD. Verrà verificata la presenza di voci DNS per tutti gli endpoint con un attributo dnsName e queste voci DNS devono essere record host, non alias, ad esempio record A non record CNAME. Verranno verificate anche la presenza di account di Active Directory noti e la loro abilitazione e l'esistenza di nomi SPN previsti|






## <a name="repair-issues-from-big-data-clusters-bdc"></a>Correggere i problemi da cluster Big Data

Un set di notebook per riparare le situazioni e gli stati noti di un cluster Big Data di SQL Server.

|Nome |Descrizione |
|---|---|---|---|
|TSG005 - Ciclo di inoltro rilevato|Usare questo notebook per gestire il ciclo di inoltro rilevato poiché l'utilità dnsmasq può inserire un loopback locale in resolv.conf, che può causare lo stato CrashLoopBackOff dei pod controller durante la distribuzione iniziale del cluster: https://askubuntu.com/questions/627899/nameserver-127-0-1-1-in-resolv-conf-wont-go-away|
|TSG011 - Riavviare il server sparkhistory|Usare questo notebook per riavviare il server sparkhistory poiché il processo Java sparkhistory può smettere di rispondere durante l'avvio. Il riavvio del server sparkhistory (supervisorctl restart sparkhistory) può risolvere questo problema.|
|TSG018 - Terminare il processo sqlservr nel pool master| Usare questo notebook quando T-SQL SHUTDOWN non ricicla il processo ./sqlservr. Usare questo notebook per terminare il processo sqlservr principale, che verrà riavviato automaticamente dal processo front-end ./sqlservr.|
|TSG024 - Namenode in modalità sicura| Usare questo notebook quando HDFS si trova in modalità sicura. Ad esempio, se viene riciclato un numero eccessivo di pod nel pool di archiviazione, è possibile che la modalità sicura venga abilitata automaticamente.|
|TSG028 - Riavviare lo strumento di gestione dei nodi su tutti i nodi del pool di archiviazione| Usare questo notebook quando è necessario riavviare lo strumento di gestione dei nodi su tutti i nodi del pool di archiviazione.|
|TSG038 - Errori di creazione BDC a causa di una chiave mancante nel documento| Usare questo notebook in caso di errori di creazione del cluster Big Data causati dalla mancanza di una chiave nel documento.|
|TSG039 - Nome di oggetto 'role_permissions' non valido| Usare questo notebook quando si verifica un problema di oggetto non valido a causa dell'autorizzazione del ruolo in gateway.log di Knox|
|TSG040 - Impossibile ottenere i nomi di file dal controller con errore| Usare questo notebook quando si verifica il timeout del gateway 504 durante il recupero dei nomi di file dal controller.|
|TSG041 - Impossibile creare un nuovo contesto I/O asincrono (aumentare sysctl fs.aio-max-nr)| Usare questo notebook quando non è possibile creare un nuovo contesto I/O asincrono (aumentare sysctl fs.aio-max-nr).|
|TSG045: Numero massimo di dischi dati che possono essere collegati a una macchina virtuale di questa dimensione (AKS)| Usare questo notebook quando si raggiunge il numero massimo di dischi dati che possono essere collegati a una macchina virtuale di questa dimensione (AKS).|
|TSG047 - ConfigException - È previsto un solo oggetto con questo nome| Usare questo notebook quando è presente Configexception che prevede un solo oggetto con nome.|
|TSG048 - La distribuzione si blocca mentre viene visualizzato il messaggio "Waiting for controller pod to be up"| Usare questo notebook quando la distribuzione si blocca quando viene visualizzato il messaggio "Waiting for controller pod to be up".|
|TSG050 - La creazione del cluster si blocca mentre viene visualizzato il messaggio "timeout expired waiting for volumes to attach or mount for pod"| Usare questo notebook quando la creazione del cluster si blocca quando viene visualizzato il messaggio "timeout expired waiting for volumes to attach or mount for pod".|
|TSG052 - Tentativo non riuscito di ottenere il DNS master-svc e nuovo tentativo| Usare questo notebook quando la creazione del cluster si blocca quando viene visualizzato il messaggio "timeout expired waiting for volumes to attach or mount for pod".|
|TSG057 - Errore durante l'avvio del servizio controller .System.TimeoutException| Usare questo notebook quando si verifica System.TimeoutException all'avvio del servizio controller.|
|TSG067 - Impossibile completare la configurazione kube| Usare questo notebook quando non è possibile completare la configurazione kube.|
|TSG074 - Eliminazione di App-Deploys| Usare questo notebook quando si verifica un problema per eliminare le app nel cluster Big Data.|
|TSG075 - Errore FailedCreatePodSandBox perché il plug-in CNI di rete non è riuscito a configurare un pod| Usare questo notebook quando si verifica l'eccezione FailedCreatePodSandBox perché NetworkPlugin cni non ha configurato il pod.|
|TSG080 - Eliminare sessioni Spark tramite azdata| Usare questo notebook quando si verifica un problema durante l'eliminazione delle sessioni Spark.|
|TSG109 - Impostare i timeout degli aggiornamenti| Usare questo notebook quando si verificano problemi di aggiornamento del cluster Big Data.|
|TSG110 - Azdata restituisce ApiError| Usare questo notebook quando Azdata restituisce ApiError.|

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md)
