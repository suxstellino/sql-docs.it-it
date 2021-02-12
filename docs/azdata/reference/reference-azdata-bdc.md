---
title: Informazioni di riferimento su azdata bdc
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata bdc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f10ba617642d42b4b1d091374305c5f97ba1b474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052322"
---
# <a name="azdata-bdc"></a>azdata bdc

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata bdc spark](reference-azdata-bdc-spark.md) | I comandi Spark consentono agli utenti di interagire con il sistema Spark creando e gestendo sessioni, istruzioni e batch.
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | Il modulo HDFS fornisce i comandi per accedere a un file system HDFS.
[azdata bdc create](#azdata-bdc-create) | Crea un cluster Big Data.
[azdata bdc delete](#azdata-bdc-delete) | Elimina un cluster Big Data.
[azdata bdc upgrade](#azdata-bdc-upgrade) | Aggiorna le immagini distribuite in ogni contenitore del cluster Big Data di SQL Server.
[azdata bdc config](reference-azdata-bdc-config.md) | Comandi di configurazione.
[azdata bdc endpoint](reference-azdata-bdc-endpoint.md) | Comandi dell'endpoint.
[azdata bdc debug](reference-azdata-bdc-debug.md) | Comandi di debug.
[azdata bdc status](reference-azdata-bdc-status.md) | Comandi di stato del cluster Big Data.
[azdata bdc control](reference-azdata-bdc-control.md) | Comandi del servizio di controllo.
[azdata bdc sql](reference-azdata-bdc-sql.md) | Comandi del servizio sql.
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | Comandi del servizio HDFS.
[azdata bdc spark](reference-azdata-bdc-spark.md) | Comandi del servizio Spark.
[azdata bdc gateway](reference-azdata-bdc-gateway.md) | Comandi del servizio gateway.
[azdata bdc app](reference-azdata-bdc-app.md) | Comandi del servizio app.
## <a name="azdata-bdc-create"></a>azdata bdc create
Crea un cluster Big Data di SQL Server. È necessaria la configurazione di Kubernetes nel sistema in uso con le variabili di ambiente seguenti ['AZDATA_USERNAME', 'AZDATA_PASSWORD'].
```bash
azdata bdc create [--name -n] 
                  [--config-profile -c]  
                  
[--accept-eula -a]  
                  
[--node-label -l]  
                  
[--force -f]
```
### <a name="examples"></a>Esempi
Esperienza guidata di distribuzione del cluster Big Data: si riceveranno le indicazioni relative ai valori necessari.
```bash
azdata bdc create
```
Distribuzione del cluster Big Data con argomenti e profilo di configurazione personalizzato inizializzato tramite `azdata bdc config init`.
```bash
azdata bdc create --accept-eula yes --config-profile ./path/to/config/profile
```
Distribuzione del cluster Big Data con nome del cluster personalizzato specificato e profilo di configurazione predefinito aks-dev-test.
```bash
azdata bdc create --name <cluster_name> --accept-eula yes --config-profile aks-dev-test
```
Distribuzione del cluster Big Data con argomenti: viene usato il flag --force e non verrà quindi visualizzato alcun messaggio di richiesta.
```bash
azdata bdc create --accept-eula yes --config-profile aks-dev-test --force
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--name -n`
Nome del cluster Big Data, usato per gli spazi dei nomi kubernetes.
#### `--config-profile -c`
Profilo di configurazione del cluster Big Data, usato per la distribuzione del cluster: ['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--accept-eula -a`
Indica se accettare le condizioni di licenza: [yes/no]. Se non si vuole usare questo argomento, è possibile impostare la variabile di ambiente ACCEPT_EULA su "yes". Le condizioni di licenza di azdata sono disponibili all'indirizzo https://aka.ms/eula-azdata-en.
#### `--node-label -l`
Etichetta del nodo del cluster Big data, usata per definire i nodi in cui eseguire la distribuzione.
#### `--force -f`
Forza la creazione, all'utente non viene richiesto di inserire alcun valore e tutti i problemi vengono stampati come parte di stderr.
### <a name="global-arguments"></a>Argomenti globali
#### `--debug`
Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug.
#### `--help -h`
Visualizza questo messaggio della guida ed esce.
#### `--output -o`
Formato di output.  Valori consentiti: json, jsonc, table, tsv.  Valore predefinito: json.
#### `--query -q`
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.
## <a name="azdata-bdc-delete"></a>azdata bdc delete
Elimina il cluster Big Data di SQL Server. È necessaria la configurazione di Kubernetes nel sistema in uso.
```bash
azdata bdc delete --name -n 
                  [--force -f]
```
### <a name="examples"></a>Esempi
Eliminazione di un cluster Big Data.
```bash
azdata bdc delete --name <cluster_name>
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--name -n`
Nome del cluster Big Data, usato per lo spazio dei nomi kubernetes.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--force -f`
Forza l'eliminazione di un cluster Big Data.
### <a name="global-arguments"></a>Argomenti globali
#### `--debug`
Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug.
#### `--help -h`
Visualizza questo messaggio della guida ed esce.
#### `--output -o`
Formato di output.  Valori consentiti: json, jsonc, table, tsv.  Valore predefinito: json.
#### `--query -q`
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.
## <a name="azdata-bdc-upgrade"></a>azdata bdc upgrade
Aggiorna le immagini distribuite in ogni contenitore del cluster Big Data di SQL Server. Le immagini aggiornate si basano sull'immagine Docker passata. Se le immagini aggiornate provengono da un repository di immagini Docker diverso rispetto alle immagini attualmente distribuite, è necessario anche il parametro "repository".
```bash
azdata bdc upgrade --name -n 
                   --tag -t  
                   
[--repository -r]  
                   
[--controller-timeout -k]  
                   
[--stability-threshold -s]  
                   
[--component-timeout -p]
```
### <a name="examples"></a>Esempi
Aggiornamento del cluster Big Data con un nuovo tag immagine "cu2" dallo stesso repository.
```bash
azdata bdc upgrade -t cu2
```
Aggiornamento del cluster Big Data con nuove immagini con tag "cu2" dal nuovo repository "foo/bar/baz".
```bash
azdata bdc upgrade -t cu2 -r foo/bar/baz
```
Aggiornamento del cluster Big Data con nuove immagini con tag "cu2" dallo stesso repository. L'aggiornamento attenderà 30 minuti per l'aggiornamento del controller e 30 minuti per l'aggiornamento del database del controller. Attenderà quindi che il controller e il database del controller vengano eseguiti per tre minuti senza causare l'arresto anomalo dell'aggiornamento del resto del cluster. Ogni fase successiva dell'aggiornamento avrà a disposizione 40 minuti per il completamento.
```bash
azdata bdc upgrade -t cu2 --controller-timeout=30 --component-timeout=40 --stability-threshold=3
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--name -n`
Nome del cluster Big Data, usato per gli spazi dei nomi kubernetes.
#### `--tag -t`
Tag dell'immagine Docker di destinazione a cui aggiornare tutti i contenitori del cluster.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--repository -r`
Repository Docker da cui tutti i contenitori del cluster devono trarre le proprie immagini.
#### `--controller-timeout -k`
Numero di minuti di attesa per l'aggiornamento del controller o del database del controller prima di eseguire il rollback dell'aggiornamento.
#### `--stability-threshold -s`
Numero di minuti di attesa dopo un aggiornamento prima di contrassegnarlo come stabile.
#### `--component-timeout -p`
Numero di minuti di attesa per il completamento di ogni fase dell'aggiornamento (dopo l'aggiornamento del controller) prima di sospendere l'aggiornamento.
### <a name="global-arguments"></a>Argomenti globali
#### `--debug`
Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug.
#### `--help -h`
Visualizza questo messaggio della guida ed esce.
#### `--output -o`
Formato di output.  Valori consentiti: json, jsonc, table, tsv.  Valore predefinito: json.
#### `--query -q`
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md). 

Per altre informazioni su come installare lo strumento **azdata**, vedere [Installare azdata](..\install\deploy-install-azdata.md).

