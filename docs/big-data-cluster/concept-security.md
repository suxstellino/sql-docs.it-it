---
title: Concetti relativi alla sicurezza
titleSuffix: SQL Server big data clusters
description: Questo articolo presenta alcuni concetti relativi alla sicurezza per i cluster Big Data di SQL Server. È inclusa la descrizione degli endpoint del cluster e dell'autenticazione del cluster.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e4fbb1222168200d2107198091db7109ef6247ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039411"
---
# <a name="security-concepts-for-big-data-clusters-2019"></a>Concetti relativi alla sicurezza per i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo presenta i concetti principali relativi alla sicurezza nel cluster Big Data

I [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] forniscono autorizzazione e autenticazione coerenti. Un cluster Big Data può essere integrato con Active Directory (AD) usando una distribuzione completamente automatizzata dell'integrazione di AD in un dominio esistente. Dopo aver configurato un cluster Big Data con l'integrazione di AD, è possibile sfruttare le identità e i gruppi di utenti esistenti per l'accesso unificato in tutti gli endpoint. Inoltre, dopo avere creato tabelle esterne in SQL Server, è possibile controllare l'accesso alle origini dati concedendo l'accesso alle tabelle esterne a utenti e gruppi di AD, centralizzando in questo modo i criteri di accesso ai dati in un'unica posizione.

Questo video di 14 minuti offre una panoramica della sicurezza dei cluster Big Data:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Big-Data-Cluster-Security/player?WT.mc_id=dataexposed-c9-niner]


## <a name="authentication"></a>Authentication

Gli endpoint del cluster esterni supportano l'autenticazione di AD. Usare l'identità di AD per l'autenticazione nel cluster Big Data.

### <a name="cluster-endpoints"></a>Endpoint del cluster

Esistono cinque punti di ingresso al cluster Big Data

* Istanza master: endpoint TDS per l'accesso all'istanza master di SQL Server nel cluster, tramite strumenti di database e applicazioni come SSMS o Azure Data Studio. Quando si usano i comandi HDFS o SQL Server da [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], lo strumento si connette agli altri endpoint, a seconda dell'operazione.

* Gateway per l'accesso ai file HDFS, Spark (Knox) - endpoint HTTPS per l'accesso ad alcuni servizi, ad esempio webHDFS e Spark.

* Endpoint del servizio di gestione del cluster (controller): servizio di gestione del cluster Big Data che espone API REST per la gestione del cluster. Lo strumento azdata richiede la connessione a questo endpoint.

* Proxy di gestione: per l'accesso ai dashboard Ricerca log e Metriche.

* Proxy dell'applicazione: endpoint per la gestione delle applicazioni distribuite all'interno del cluster Big Data.

![Endpoint del cluster](media/concept-security/cluster_endpoints.png)

Attualmente non è possibile aprire porte aggiuntive per accedere al cluster dall'esterno.

## <a name="authorization"></a>Autorizzazione

In tutto il cluster la sicurezza integrata tra diversi componenti consente di passare l'identità dell'utente originale durante l'esecuzione di query da Spark e SQL Server, fino a HDFS. Come indicato sopra, i diversi endpoint del cluster esterni supportano l'autenticazione di Active Directory.

Esistono due livelli di controllo delle autorizzazioni nel cluster per la gestione dell'accesso ai dati. L'autorizzazione nel contesto dei Big Data viene eseguita in SQL Server, usando le autorizzazioni di SQL Server tradizionali per gli oggetti, e in HDFS con elenchi di controllo (ACL), che associano le identità utente ad autorizzazioni specifiche.

Un cluster Big Data sicuro implica un supporto costante e coerente per gli scenari di autenticazione e autorizzazione, sia in SQL Server sia in HDFS/Spark. L'autenticazione è il processo di verifica dell'identità di un utente o di un servizio per garantire che corrisponda a quella dichiarata dall'utente o dal servizio stesso. Per autorizzazione si intende la concessione o la negazione dell'accesso a risorse specifiche in base all'identità dell'utente richiedente. Questo passaggio viene eseguito dopo l'identificazione di un utente tramite l'autenticazione.

Nel contesto dei Big Data l'autorizzazione viene in genere eseguita tramite gli elenchi di controllo di accesso (ACL), che associano le identità utente ad autorizzazioni specifiche. HDFS supporta l'autorizzazione limitando l'accesso alle API del servizio, ai file HDFS e all'esecuzione del processo.

## <a name="encryption-in-flight-and-other-security-mechanisms"></a>Crittografia in anteprima e altri meccanismi di sicurezza

La crittografia delle comunicazioni tra i client e gli endpoint esterni, nonché tra i componenti all'interno del cluster, è protetta con TLS/SSL tramite certificati.

Tutte le comunicazioni da SQL Server a SQL Server, ad esempio la comunicazione dell'istanza master di SQL Server con un pool di dati, vengono protette tramite account di accesso SQL.

> [!IMPORTANT]
>  I cluster Big Data usano `etcd` per archiviare le credenziali. Come procedura consigliata, è necessario assicurarsi che il cluster Kubernetes sia configurato per l'uso della crittografia `etcd` per i dati inattivi. Per impostazione predefinita, i segreti archiviati in `etcd` non sono crittografati. La documentazione di Kubernetes fornisce informazioni dettagliate su questa attività amministrativa: https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/ e https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/.

## <a name="data-encryption-at-rest"></a>Crittografia di dati inattivi

La funzionalità di crittografia dei dati inattivi nei cluster Big Data di SQL Server supporta lo scenario principale della crittografia a livello di applicazione per i componenti SQL Server e HDFS. Per una guida completa sull'uso della funzionalità, vedere l'articolo [Concetti sulla crittografia dei dati inattivi e guida alla configurazione](encryption-at-rest-concepts-and-configuration.md).

> [!IMPORTANT]
> La crittografia del volume è consigliata per tutte le distribuzioni di cluster Big Data di SQL Server. Anche i volumi di archiviazione forniti dal cliente configurati in cluster Kubernetes devono essere crittografati, per un approccio completo alla crittografia dei dati inattivi. La funzionalità di crittografia dei dati inattivi nei cluster Big Data di SQL Server è un livello di sicurezza aggiuntivo che offre la crittografia a livello di applicazione dei file di dati e di log di SQL Server e il supporto delle zone di crittografia HDFS.


## <a name="basic-administrator-login"></a>Account di accesso amministratore di base

È possibile scegliere di distribuire il cluster in modalità AD o usando solo l'account di accesso amministratore di base. L'uso del solo account di accesso amministratore di base non è una modalità di sicurezza supportata per la produzione ed è destinata alla valutazione del prodotto.

Anche se si sceglie la modalità Active Directory, per l'amministratore del cluster vengono comunque creati account di accesso di base. Questa funzionalità offre un accesso alternativo, nel caso in cui la connettività di AD non sia attiva.

A questo account di accesso di base vengono concesse le autorizzazioni di amministratore nel cluster in fase di distribuzione. L'utente dell'account di accesso sarà un amministratore di sistema nell'istanza master di SQL Server e un amministratore nel controller del cluster.
I componenti Hadoop non supportano l'autenticazione in modalità mista e di conseguenza non è possibile usare un account di accesso amministratore di base per l'autenticazione nel gateway (Knox).

Le credenziali di accesso che è necessario definire durante la distribuzione includono:

Nome utente dell'amministratore del cluster:

 + `AZDATA_USERNAME=<username>`

Password dell'amministratore del cluster:  
 + `AZDATA_PASSWORD=<password>`

> [!NOTE]
> Si noti che in modalità non Active Directory è necessario usare il nome utente in combinazione con la password precedente, per l'autenticazione nel gateway (Knox) per l'accesso a HDFS/Spark. Prima di SQL Server 2019 CU5, il nome utente era `root`.
> 
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

## <a name="next-steps"></a>Passaggi successivi

[Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)

[Workshop: Architettura Microsoft dei [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)

[Controllo degli accessi in base al ruolo di Kubernetes](kubernetes-rbac.md)
