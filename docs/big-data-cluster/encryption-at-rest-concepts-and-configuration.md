---
title: Crittografia di dati inattivi
titleSuffix: SQL Server Big Data Clusters
description: Informazioni dettagliate sulla crittografia di dati inattivi in un cluster Big Data di SQL Server 2019.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69749c0da2a5f7ef672a4673b5bc857898e9964f
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489185"
---
# <a name="encryption-at-rest-concepts-and-configuration-guide"></a>Concetti sulla crittografia dei dati inattivi e guida alla configurazione

A partire da cluster Big Data di SQL Server CU8 è disponibile un set completo di funzionalità di crittografia dei dati inattivi che consente di usare la crittografia a livello di applicazione a tutti i dati archiviati nella piattaforma. Questa guida illustra i concetti, l'architettura e la configurazione del set di funzionalità di crittografia dei dati inattivi per cluster Big Data di SQL Server.

In cluster Big Data di SQL Server i dati vengono archiviati nelle due posizioni seguenti:

* Istanza master di __SQL Server__
* __HDFS__ usato dal pool di archiviazione e Spark.

Per poter crittografare in modo trasparente i dati in cluster Big Data di SQL Server, è possibile adottare due approcci:

* __Crittografia dei volumi__. Questo approccio è supportato dalla piattaforma Kubernetes ed è la procedura consigliata per le distribuzioni di cluster Big Data. Questa guida non illustra la crittografia dei volumi. Leggere la documentazione relativa all'appliance o alla piattaforma Kubernetes per informazioni su come crittografare correttamente i volumi usati per cluster Big Data di SQL Server.
* __Crittografia a livello di applicazione__. Questa architettura si riferisce alla crittografia dei dati da parte dell'applicazione che gestisce i dati prima che vengano scritti su disco. Nel caso in cui i volumi siano esposti, per un utente malintenzionato sarebbe impossibile ripristinare gli elementi dati in un'altra posizione, a meno che il sistema di destinazione non sia stato configurato con le stesse chiavi di crittografia.

Il __set di funzionalità di crittografia dei dati inattivi di SQL Server cluster di Big Data__ supporta lo scenario principale della __crittografia a livello di applicazione__ per i componenti __SQL Server__ e __HDFS__ .

Sono incluse le funzionalità riportate di seguito:

* __Crittografia dei dati inattivi gestita dal sistema__. Questa funzionalità è disponibile in CU8.
* __Crittografia dei dati inattivi gestita dall'utente (BYOK)__ , integrando sia provider di chiavi gestiti dal servizio sia provider esterni. Attualmente sono supportate solo le chiavi create dall'utente e gestite dal servizio.

## <a name="key-definitions"></a>Definizioni chiave

### <a name="sql-server-big-data-clusters-key-management-service-kms"></a>Servizio di gestione delle chiavi di cluster Big Data di SQL Server

Si tratta di un servizio ospitato nel controller, responsabile della gestione delle chiavi e dei certificati per il set di funzionalità di crittografia dei dati inattivi per il cluster Big Data di SQL Server. Supporta le funzionalità seguenti:

* Gestione e archiviazione sicure di chiavi e certificati usati per la crittografia dei dati inattivi.
* Compatibilità del servizio di gestione delle chiavi Hadoop. Viene usato come servizio di gestione delle chiavi per il componente HDFS nel cluster Big Data.
* Gestione dei certificati TDE di SQL Server.

Nella parte restante del documento si userà __BDC KMS__ per fare riferimento a questo servizio. Anche il termine __BDC__ viene usato per fare riferimento alla piattaforma di computing __di cluster Big Data di SQL Server__.

### <a name="system-managed-keys-and-certificates"></a>Chiavi e certificati gestiti dal sistema

Il servizio BDC KMS gestisce tutte le chiavi e i certificati per SQL Server e HDFS.

### <a name="user-provided-certificates"></a>Certificati forniti dall'utente

Chiavi e certificati forniti dall'utente che devono essere gestiti tramite il servizio BDC KMS. Sono comunemente noti come BYOK (Bring Your Own Key).

### <a name="external-providers"></a>Provider esterni

Soluzioni di chiavi esterne compatibili con il servizio BDC KMS per la delega esterna. Questa funzionalità non è attualmente supportata.

## <a name="encryption-at-rest-on-sql-server-big-data-clusters"></a>Crittografia dei dati inattivi nei cluster SQL Server Big Data

Leggere attentamente questo documento per valutare completamente lo scenario.

Il set di funzionalità introduce il __servizio del controller BDC KMS__ che offre chiavi e certificati gestiti dal sistema per la crittografia dei dati inattivi sia in SQL Server che in HDFS. Tali chiavi e certificati sono gestiti dal servizio. Questa documentazione offre informazioni operative su come interagire con il servizio.

* Le istanza di __SQL Server__ sfruttano la funzionalità [Transparent Data Encryption (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md).
* __HDFS__ usa il servizio di gestione delle chiavi Hadoop nativo all'interno di ogni pod per interagire con il servizio BDC KMS nel controller. In questo modo si abilitano zone di crittografia HDFS, che offrono percorsi sicuri in HDFS.

### <a name="sql-server-instances"></a>Istanze di SQL Server

* Nei pod di SQL Server viene installato un certificato generato dal sistema da usare con i comandi TDE. Lo standard di denominazione del certificato gestito dal sistema è `TDECertificate` + `timestamp`. Ad esempio `TDECertificate2020_09_15_22_46_27`.
* I database con provisioning BDC dell'istanza master e i database utente non vengono crittografati automaticamente. Gli amministratori di database possono usare il certificato installato per crittografare qualsiasi database.
* Il pool di calcolo e il pool di archiviazione vengono crittografati automaticamente usando il certificato generato dal sistema.
* La crittografia dei pool di dati, anche se tecnicamente possibile tramite i comandi `EXECUTE AT` di T-SQL, non è consigliata e non è attualmente supportata. Usare questa tecnica per crittografare i database dei pool di dati potrebbe non essere efficace e la crittografia potrebbe non essere eseguita allo stato desiderato. Si crea anche un percorso di aggiornamento incompatibile per le versioni successive.
* La rotazione della chiave SQL Server viene eseguita utilizzando i comandi amministrativi T-SQL standard. Per istruzioni complete, vedere [la Guida all'uso di SQL Server cluster di Big Data Transparent Data Encryption (Transparent Data Encryption)](encryption-at-rest-sql-server-tde.md) .
* Il monitoraggio della crittografia avviene tramite le DMV standard di SQL Server per TDE.
* Sono supportati il backup e il ripristino di un database abilitato per TDE nel cluster.
* La disponibilità elevata è supportata. Se un database nell'istanza primaria di SQL Server viene crittografato, verrà crittografata anche tutta la replica secondaria del database.

### <a name="hdfs-encryption-zones"></a>Zone di crittografia HDFS

* Per abilitare la funzionalità delle zone di crittografia per HDFS, è necessaria l'[integrazione di Active Directory](active-directory-prerequisites.md).
* Nel servizio di gestione delle chiavi Hadoop viene effettuato il provisioning di una chiave generata dal sistema. Il nome della chiave è `securelakekey`. In CU8 la chiave predefinita è di 256 bit. È supportata la crittografia AES a 256 bit.
* Viene eseguito il provisioning di una zona di crittografia predefinita usando la chiave generata dal sistema precedente in un percorso denominato `/securelake`.
* Gli utenti possono creare chiavi e zone di crittografia aggiuntive usando le istruzioni specifiche disponibili in questa guida. Gli utenti possono scegliere le dimensioni della chiave (128, 192 o 256) durante la creazione della chiave.
* La rotazione della chiave delle zone di crittografia HDFS viene eseguita usando azdata. Per istruzioni complete, vedere la [Guida all'uso di SQL Server cluster di Big data per le aree di crittografia HDFS](encryption-at-rest-hdfs-encryption-zones.md) .
* Non è supportato il montaggio per la suddivisione in livelli HDFS su una zona di crittografia.

## <a name="encryption-at-rest-administration"></a>Amministrazione della crittografia di Rest

L'elenco seguente contiene le funzionalità di amministrazione per la crittografia dei inattivi

* [SQL Server](encryption-at-rest-sql-server-tde.md) gestione di Transparent Data Encryption viene eseguita utilizzando i comandi T-SQL standard.
* Le [zone di crittografia HDFS](encryption-at-rest-hdfs-encryption-zones.md) e la gestione delle chiavi HDFS vengono eseguite usando i comandi azdata.
* Le seguenti funzionalità di amministrazione vengono eseguite utilizzando [notebook operativi](cluster-manage-notebooks.md):
    - Backup e ripristino della chiave HDFS
    - Eliminazione della chiave HDFS

## <a name="configuration-guide"></a>Guida alla configurazione

Durante le __nuove distribuzioni di cluster Big Data di SQL Server__ (CU8 e versioni successive), la __crittografia dei dati inattivi verrà abilitata e configurata per impostazione predefinita__. Ciò significa che:

* Il componente KMS BDC verrà distribuito nel controller e genererà un set predefinito di chiavi e certificati.
* SQL Server verrà distribuito con TDE abilitato e i certificati verranno installati dal controller.
* Il servizio di gestione delle chiavi Hadoop (per HDFS) verrà configurato per interagire con il servizio BDC KMS per le operazioni di crittografia. Le zone di crittografia HDFS sono configurate e pronte per l'uso.

Vengono applicati i requisiti e i comportamenti predefiniti descritti nella sezione precedente.

__Se si esegue una nuova distribuzione di SQL Server BDC CU8 o si esegue l'aggiornamento direttamente a CU9, non sono necessari altri passaggi__.

### <a name="upgrade-scenarios"></a>Scenari di aggiornamento

Nei cluster esistenti, il processo di aggiornamento non impone la nuova crittografia o la nuova crittografia sui dati utente che non sono già stati crittografati. Questo comportamento è dovuto alla progettazione. Considerare quanto segue per ogni componente:

* __SQL Server__

    1. __Istanza master di SQL Server__. Il processo di aggiornamento non ha effetto sui database dell'istanza master né sui certificati TDE. È tuttavia consigliabile eseguire il backup dei database e dei certificati TDE installati manualmente prima di avviare il processo di aggiornamento. È anche consigliabile archiviare tali elementi all'esterno del cluster BDC di SQL Server.
    1. __Pool di calcolo e di archiviazione__. Questi database sono gestiti dal sistema, sono volatili e vengono ricreati e crittografati automaticamente durante l'aggiornamento del cluster.
    1. __Pool di dati__. L'aggiornamento non ha effetti sui database nella parte dei pool di dati nelle istanze di SQL Server.

* __HDFS__

    1. __HDFS__. Il processo di aggiornamento non interessa i file e le cartelle HDFS al di fuori delle zone di crittografia.

### <a name="upgrading-to-cu9-from-cu8-or-earlier"></a>Aggiornamento a CU9 da CU8 o versioni precedenti

Non sono necessari altri passaggi.

### <a name="upgrading-to-cu8-from-cu6-or-earlier"></a>Aggiornamento a CU8 da CU6 o versioni precedenti

   > [!CAUTION]
   > Prima di eseguire l'aggiornamento a cluster Big Data di SQL Server CU8, eseguire un backup completo dei dati.


__Le zone di crittografia non vengono configurate__. Il componente del servizio di gestione delle chiavi Hadoop non viene configurato per usare il servizio BDC KMS. Per configurare e abilitare la funzionalità zone di crittografia HDFS dopo l'aggiornamento, seguire le istruzioni della sezione successiva.

#### <a name="enable-hdfs-encryption-zones-after-upgrade-to-cu8"></a>Abilitare le zone di crittografia HDFS dopo l'aggiornamento a CU8

Se il cluster è stato aggiornato a CU8 ( `azdata upgrade` ) e si vuole abilitare le zone di crittografia HDFS sono disponibili due opzioni:

* Esecuzione del Azure Data Studio [notebook operativo](cluster-manage-notebooks.md) denominato __SOP0128: Abilita le zone di crittografia HDFS nei cluster di Big Data__ per eseguire la configurazione.
* Esecuzione di script come descritto sotto.

Requisiti:

- Cluster integrato di [Active Directory](active-directory-prerequisites.md).

- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] configurato e connesso al cluster in modalità AD.

Attenersi alla procedura seguente per riconfigurare il cluster con il supporto delle zone di crittografia.

1. Dopo aver eseguito l'aggiornamento con `azdata`, salvare gli script seguenti.

    Requisiti di esecuzione degli script:
        
    * Entrambi gli script devono trovarsi nella stessa directory. 
    * `kubectl` in `PATH
    * File ```config``` per Kubernetes nella cartella ```$HOME/.kube```
    
    Questo script deve essere denominato __```run-key-provider-patch.sh```__ :

    ```console
    #!/bin/bash
    
    if [[ -z "${BDC_DOMAIN}" ]]; then
      echo "BDC_DOMAIN environment variable with the domain name of the cluster is not defined."
      exit 1
    fi
    
    if [[ -z "${BDC_CLUSTER_NS}" ]]; then
      echo "BDC_CLUSTER_NS environment variable with the cluster namespace is not defined."
      exit 1
    fi
    
    kubectl get configmaps -n test
    
    diff <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    diff <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    # Replace the config maps.
    #
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    # Restart the pods which need to have the necessary changes with the core-site.xml
    kubectl delete pods -n $BDC_CLUSTER_NS nmnode-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-1
    
    # Wait for sometime for pods to restart
    #
    sleep 300
    
    # Check for the KMS process status.
    #
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop nmnode-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-1 -- bash -c 'ps aux | grep kms'
    ```

    Questo script deve essere denominato __```updatekeyprovider.py```__ :

    ```python
    #!/usr/bin/env python3
    
    import json
    import re
    import sys
    import xml.etree.ElementTree as ET
    import os
    
    class CommentedTreeBuilder(ET.TreeBuilder):
        def comment(self, data):
            self.start(ET.Comment, {})
            self.data(data)
            self.end(ET.Comment)
    
    domain_name = os.environ['BDC_DOMAIN']
    
    parser = ET.XMLParser(target=CommentedTreeBuilder())
    
    core_site = 'core-site.xml'
    j = json.load(sys.stdin)
    cs = j['data'][core_site]
    csxml = ET.fromstring(cs, parser=parser)
    props = [prop.find('value').text for prop in csxml.findall(
        "./property/name/..[name='hadoop.security.key.provider.path']")]
    
    kms_provider_path=''
    
    for x in range(5):
        if len(kms_provider_path) != 0:
            kms_provider_path = kms_provider_path + ';'
        kms_provider_path = kms_provider_path + 'nmnode-0-0.' + domain_name
    
    if len(props) == 0:
        prop = ET.SubElement(csxml, 'property')
        name = ET.SubElement(prop, 'name')
        name.text = 'hadoop.security.key.provider.path'
        value = ET.SubElement(prop, 'value')
        value.text = 'kms://https@' + kms_provider_path + ':9600/kms'
        cs = ET.tostring(csxml, encoding='utf-8').decode('utf-8')
    
    j['data'][core_site] = cs
    
    kms_site = 'kms-site.xml.tmpl'
    ks = j['data'][kms_site]
    
    kp_uri_regex = re.compile('(<name>hadoop.kms.key.provider.uri</name>\s*<value>\s*)(.*)(\s*</value>)', re.MULTILINE)
    
    def replace_uri(match_obj):
        key_provider_uri = 'bdc://https@hdfsvault-svc.' + domain_name
        if match_obj.group(2) == 'jceks://file@/var/run/secrets/keystores/kms/kms.jceks' or match_obj.group(2) == key_provider_uri:
            return match_obj.group(1) + key_provider_uri + match_obj.group(3)
        return match_obj.group(0)
    
    ks = kp_uri_regex.sub(replace_uri, ks)
    
    j['data'][kms_site] = ks
    print(json.dumps(j, indent=4, sort_keys=True))
    ```

    Eseguire __```run-key-provider-patch.sh```__ con i parametri appropriati. 

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come usare in modo efficace la crittografia dei dati inattivi in cluster Big Data di SQL Server, vedere gli articoli seguenti:

- [Crittografia dei dati inattivi - TDE di SQL Server](encryption-at-rest-sql-server-tde.md)
- [Crittografia dei dati inattivi - Zone di crittografia HDFS](encryption-at-rest-hdfs-encryption-zones.md)

Per altre informazioni su [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vedere la panoramica seguente:

- [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
