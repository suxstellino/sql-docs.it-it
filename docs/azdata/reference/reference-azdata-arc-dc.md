---
title: Riferimento ad azdata arc dc
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc dc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b223b3b69ba3eda1be5f882b4cabd868c345b20c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049011"
---
# <a name="azdata-arc-dc"></a>azdata arc dc

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc dc create](#azdata-arc-dc-create) | Creare un controller dei dati.
[azdata arc dc delete](#azdata-arc-dc-delete) | Eliminare un controller dei dati.
[azdata arc dc endpoint](reference-azdata-arc-dc-endpoint.md) | Comandi dell'endpoint.
[azdata arc dc status](reference-azdata-arc-dc-status.md) | Comandi di stato.
[azdata arc dc config](reference-azdata-arc-dc-config.md) | Comandi di configurazione.
[azdata arc dc debug](reference-azdata-arc-dc-debug.md) | Comandi di debug.
[azdata arc dc export](#azdata-arc-dc-export) | Esportare metriche, log o utilizzo.
[azdata arc dc upload](#azdata-arc-dc-upload) | Caricare il file di dati esportato.
## <a name="azdata-arc-dc-create"></a>azdata arc dc create
Creare un controller dati. È necessaria la configurazione kube nel sistema in uso con le variabili di ambiente seguenti ['AZDATA_USERNAME', 'AZDATA_PASSWORD'].
```bash
azdata arc dc create --namespace -ns 
                     --name -n  
                     
--connectivity-mode  
                     
--resource-group -g  
                     
--location -l  
                     
--subscription -s  
                     
[--profile-name]  
                     
[--path -p]  
                     
[--storage-class -sc]
```
### <a name="examples"></a>Esempi
Distribuzione del controller dati.
```bash
azdata arc dc create
```
### <a name="required-parameters"></a>Parametri necessari
#### `--namespace -ns`
Spazio dei nomi Kubernetes in cui distribuire il controller dati. Se è già presente, verrà usato quello esistente. Se non è presente, verrà prima effettuato un tentativo di crearlo.
#### `--name -n`
Nome del controller dati.
#### `--connectivity-mode`
Connettività ad Azure, indiretta o diretta, in cui verrà usato il controller dati.
#### `--resource-group -g`
Gruppo di risorse di Azure in cui deve essere aggiunta la risorsa del controller dati.
#### `--location -l`
Posizione di Azure in cui verranno archiviati i metadati del controller dati, ad esempio eastus.
#### `--subscription -s`
ID sottoscrizione di Azure in cui deve essere aggiunta la risorsa del controller dati.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--profile-name`
Nome di un profilo di configurazione esistente. Eseguire `azdata arc dc config list` per visualizzare le opzioni disponibili. Una delle opzioni seguenti: ['azure-arc-aks-premium-storage', 'azure-arc-ake', 'azure-arc-openshift', 'azure-arc-gke', 'azure-arc-aks-default-storage', 'azure-arc-kubeadm', 'azure-arc-eks', 'azure-arc-azure-openshift', 'azure-arc-aks-hci'].
#### `--path -p`
Percorso di una directory contenente un profilo di configurazione personalizzato da usare. Eseguire `azdata arc dc config init` per creare un profilo di configurazione personalizzato.
#### `--storage-class -sc`
La classe di archiviazione da usare per tutti i dati e i volumi permanenti dei log per tutti i pod del controller dati che li richiedono.
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
## <a name="azdata-arc-dc-delete"></a>azdata arc dc delete
Eliminare il controller dati. È necessaria la configurazione kube nel sistema in uso.
```bash
azdata arc dc delete --name -n 
                     --namespace -ns  
                     
[--force -f]
```
### <a name="examples"></a>Esempi
Distribuzione del controller dati.
```bash
azdata arc dc delete
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del controller dati.
#### `--namespace -ns`
Spazio dei nomi Kubernetes in cui è presente il controller dati.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--force -f`
Forzare l'eliminazione del controller dati.
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
## <a name="azdata-arc-dc-export"></a>azdata arc dc export
Esportare metriche, log o utilizzo in un file.
```bash
azdata arc dc export --type -t 
                     --path -p  
                     
[--force -f]
```
### <a name="required-parameters"></a>Parametri necessari
#### `--type -t`
Tipo di dati da esportare. Opzioni: log, metriche e utilizzo.
#### `--path -p`
Percorso completo o relativo che include il nome file del file da esportare.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--force -f`
Forzare la creazione del file di output. Sovrascrive qualsiasi file esistente nello stesso percorso.
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
## <a name="azdata-arc-dc-upload"></a>azdata arc dc upload
Caricare il file di dati esportato da un controller dati in Azure.
```bash
azdata arc dc upload --path -p 
                     
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso completo o relativo che include il nome file del file da caricare.
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

