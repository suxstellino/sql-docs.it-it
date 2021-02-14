---
title: Distribuire più cluster nel dominio di Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Informazioni su come distribuire più cluster Big Data di SQL Server in un singolo dominio di Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8b82b4a5802467841ca345ac24c83e1356a79f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040121"
---
# <a name="deploy-multiple-big-data-clusters-2019-in-the-same-active-directory-domain"></a>Distribuire più [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] nello stesso dominio di Active Directory

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questo articolo illustra gli aggiornamenti di SQL Server 2019 CU5 che consentono la distribuzione e l'integrazione di più cluster Big Data di SQL Server 2019 con lo stesso dominio di Active Directory.

Prima di SQL 2019 CU5, due problemi impedivano la distribuzione di più cluster BDC in un dominio di Active Directory.

- Conflitto di denominazione per i nomi di entità servizio e il dominio DNS
- Nomi di entità account di dominio

## <a name="object-name-collisions"></a>Conflitti dei nomi di oggetto

### <a name="service-principal-names-spn-and-dns-domain-naming-conflict"></a>Conflitto di denominazione per i nomi di entità servizio e il dominio DNS

Il nome di dominio specificato in fase di distribuzione viene utilizzato come dominio DNS di Active Directory. Ciò significa che i pod possono connettersi tra loro nella rete interna tramite questo dominio DNS. Inoltre, gli utenti si connettono agli endpoint del cluster BDC usando questo dominio DNS. Di conseguenza, qualsiasi nome di entità servizio (SPN) creato per un servizio all'interno del cluster BDC avrà il nome del pod, del servizio o dell'endpoint Kubernetes qualificato con questo dominio DNS di Active Directory. Se un utente distribuisce un secondo cluster nel dominio, i nomi SPN generati avranno lo stesso nome di dominio completo poiché i nomi dei pod e il nome di dominio DNS non sono diversi tra i due cluster. Si consideri, ad esempio, il caso in cui il dominio DNS di Active Directory è `contoso.local`. Uno dei nomi SPN generati per il server SQL Server del pool master nel pod `master-0` sarà `MSSQLSvc/master-0.contoso.local:1433`. Nel secondo cluster che l'utente tenterà di distribuire, il nome del pod per `master-0` sarà lo stesso e l'utente fornirà lo stesso dominio DNS di Active Directory (``contoso.local``), ottenendo la stessa stringa del nome SPN. Active Directory impedirà la creazione di un nome SPN in conflitto, causando un errore di distribuzione per il secondo cluster.

### <a name="domain-account-principal-names"></a>Nomi di entità account di dominio

Durante una distribuzione di cluster BDC con un dominio di Active Directory, vengono generate più entità account per i servizi in esecuzione all'interno del cluster BDC. Si tratta essenzialmente di account utente di Active Directory. Prima di SQL 2019 CU5 i nomi per questi account non sono univoci tra i cluster. Questo si manifesta nel tentativo di creare lo stesso nome di account utente per un particolare servizio in BDC in due cluster diversi. Il cluster eseguito per secondo entrerà un conflitto in Active Directory e non potrà creare il proprio account.

## <a name="resolution-for-collisions"></a>Risoluzione dei conflitti

### <a name="solution-to-solve-the-problem-with-spns-and-dns-domain---sql-2019-cu5"></a>Soluzione del problema relativo ai nomi SPN e al dominio DNS - SQL 2019 CU5

Poiché i nomi SPN devono essere diversi in due cluster, il nome del dominio DNS passato in fase di distribuzione deve essere diverso. È possibile specificare nomi DNS diversi usando l'impostazione appena introdotta nel file di configurazione della distribuzione: `subdomain`. Se il sottodominio è diverso tra due cluster e la comunicazione interna può essere eseguita su questo sottodominio, i nomi SPN includeranno il sottodominio che soddisfa il requisito di univocità.

>[!NOTE]
>Il valore passato tramite l'impostazione del sottodominio non è un nuovo dominio di Active Directory, ma un dominio DNS usato internamente.

Si consideri di nuovo, ad esempio, il caso di uno dei nomi SPN per il server SQL Server del pool master. Se il sottodominio è `bdc`, il nome SPN descritto in precedenza verrà modificato in `MSSQLSvc/master-0.bdc.contoso.local:1433`.  

La personalizzazione del valore del parametro di sottodominio appena introdotto nella specifica di configurazione di Active Directory è facoltativa. Per impostazione predefinita, il nome o lo spazio dei nomi del cluster BDC verrà usato per calcolare il valore dell'impostazione del sottodominio. Se gli utenti vogliono eseguire l'override del nome del sottodominio, possono usare il nuovo parametro del sottodominio introdotto nella specifica di configurazione di Active Directory.

### <a name="solution-to-solve-the-problem-regarding-account-names-uniqueness"></a>Soluzione del problema relativo all'univocità dei nomi di account

Per aggiornare i nomi di account a uno schema che garantisce l'univocità, è stato introdotto il concetto di prefisso dell'account, ovvero una parte del nome dell'account univoca tra due cluster. La parte rimanente del nome dell'account è costante per un determinato servizio. Il nuovo formato del nome dell'account sarà simile a `<prefix>-<name>-<podId>`. 

>[!NOTE]
>Active Directory impone il limite di 20 caratteri per i nomi degli account. Il cluster BDC deve usare 8 caratteri per distinguere pod e StatefulSet. Rimangono così al massimo 12 caratteri per il prefisso dell'account.

La personalizzazione del nome dell'account è facoltativa. Usare il parametro `accountPrefix` nella specifica di configurazione di Active Directory. SQL Server 2019 CU5 introduce `accountPrefix` nella specifica di configurazione. Per impostazione predefinita, come prefisso dell'account viene usato il nome del sottodominio. Se il nome del sottodominio supera i 12 caratteri, come prefisso dell'account viene usata la sottostringa di 12 caratteri iniziale del nome del sottodominio.

Il sottodominio si applica solo al DNS. Il nuovo nome dell'account utente LDAP è quindi `bdc-ldap@contoso.local` e non `bdc-ldap@bdc.contoso.local`.

## <a name="semantics"></a>Semantica

In sintesi, questa è la semantica dei parametri aggiunti in SQL 2019 CU5 per più cluster in un dominio:

### `subdomain`

- Campo facoltativo
- Tipo di dati: string
- Definizione: sottodominio DNS univoco da usare per il cluster BDC. Questo valore deve essere diverso per ogni cluster distribuito nel dominio di Active Directory.  
- Valore predefinito: se non specificato, come valore predefinito verrà usato il nome del cluster.
- Lunghezza massima: 63 caratteri per etichetta (ogni stringa separata da un punto).
- Osservazioni: I nomi DNS degli endpoint devono usare il sottodominio nel relativo nome di dominio completo.

### `accountPrefix`

- Campo facoltativo
- Tipo di dati: string
- Definizione: prefisso univoco per gli account di Active Directory che verranno generati dal cluster BDC. Questo valore deve essere diverso per ogni cluster distribuito nel dominio di Active Directory.
- Valore predefinito: se non specificato, come valore predefinito verrà usato il nome del sottodominio. Quando il sottodominio non è specificato, come nome di sottodominio verrà usato il nome del cluster e, di conseguenza, il nome del cluster verrà ereditato anche come prefisso di account. Se invece il sottodominio è specificato ed è un nome costituito da più parti (contiene uno o più punti), l'utente deve definire un prefisso di account. 
- Lunghezza massima: 12 caratteri 

## <a name="impact-on-ad-domain-and-dns-server"></a>Effetti sul dominio di Active Directory e sul server DNS 

Non sono necessarie modifiche nel controller di dominio o nel dominio di Active Directory per gestire la distribuzione di più cluster BDC rispetto allo stesso dominio di Active Directory. Il sottodominio DNS verrà creato automaticamente nel server DNS durante la registrazione dei nomi DNS degli endpoint esterni. 

## <a name="impact-on-setting-up-the-deployment-configuration-file-used-for-the-bdc-deployment"></a>Effetti sull'impostazione del file di configurazione della distribuzione usato per la distribuzione del cluster BDC 

La sezione *activeDirectory* nella configurazione del piano di controllo *control.json* avrà due nuovi parametri facoltativi: `subdomain` e `accountPrefix`. Specificare i valori per queste impostazioni solo se si vuole eseguire l'override del comportamento predefinito, ovvero usare il nome del cluster per ogni parametro. Il nome del cluster coincide con quello dello spazio dei nomi.

È inoltre possibile usare i nomi DNS degli endpoint scelti purché siano completi e non siano in conflitto tra due cluster Big Data distribuiti nello stesso dominio. Facoltativamente, è possibile usare il valore del parametro del sottodominio per assicurarsi che i nomi DNS siano diversi tra i cluster.  Si consideri, ad esempio, l'endpoint del gateway. Se si vuole usare il nome `gateway` per l'endpoint e registrarlo automaticamente nel server DNS come parte della distribuzione del cluster BDC, usare `gateway.bdc1.contoso.local` come nome DNS, se `bdc1` è il sottodominio e `contoso.local` è il nome di dominio DNS di Active Directory. Altri valori accettabili sono `gateway-bdc1.contoso.local` o semplicemente `gateway.contoso.local`.

## <a name="examples"></a>Esempi

Di seguito è riportato un esempio di configurazione della sicurezza di Active Directory, nel caso in cui si voglia eseguire l'override del sottodominio e del prefisso di account. 

```json
    "security": { 
        "activeDirectory": { 
            "ouDistinguishedName":"OU=contosoou,DC=contoso,DC=local", 
            "dnsIpAddresses": [ "10.10.10.10" ], 
            "domainControllerFullyQualifiedDns": [ "contoso-win2016-dc.contoso.local" ], 
            "domainDnsName":"contoso.local", 
            "subdomain": "bdc", 
            "accountPrefix": "myprefix", 
            "clusterAdmins": [ "contosoadmins" ], 
            "clusterUsers": [ "contosousers1", "contosousers2" ] 
        } 
    } 
  
```

Di seguito è riportato un esempio di specifica degli endpoint del piano di controllo. È possibile usare qualsiasi valore per i nomi DNS, purché siano univoci e completi:
  
```json
        "endpoints": [ 
            { 
                "serviceType": "NodePort", 
                "port": 30080, 
                "name": "Controller", 
                "dnsName": "control-bdc1.contoso.local" 
            }, 
            { 
                "serviceType": "NodePort", 
                "port": 30777, 
                "name": "ServiceProxy", 
                "dnsName": "monitor-bdc1.contoso.local" 
            } 
        ] 
  
```

## <a name="questions"></a>Domande

### <a name="do-you-need-to-create-separate-ous-for-different-clusters"></a>È necessario creare unità organizzative separate per diversi cluster?

Anche se non è necessario, è consigliabile farlo. Specificando unità organizzative separate per cluster distinti è possibile gestire più facilmente gli account utente generati.

### <a name="how-to-revert-back-to-the-pre-cu5-behavior-in-sql-2019"></a>Come ripristinare il comportamento precedente alla versione CU5 di SQL 2019?

Il nuovo parametro `subdomain` introdotto con l'aggiornamento potrebbe non essere utilizzabile in alcuni scenari, ad esempio quando è necessario distribuire una versione precedente alla CU5 ed è già stato eseguito l'aggiornamento di [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Questo scenario è altamente improbabile, ma se è necessario ripristinare il comportamento precedente alla versione CU5, è possibile impostare il parametro `useSubdomain` su `false` nella sezione relativa ad Active Directory di `control.json`.

L'esempio seguente imposta `useSubdomain` su `false` per questo scenario.

```console
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false" 
```

## <a name="next-steps"></a>Passaggi successivi

[Risolvere i problemi di integrazione di Active Directory di cluster Big Data di SQL Server](troubleshoot-active-directory.md)
