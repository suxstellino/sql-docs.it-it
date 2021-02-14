---
title: Riferimento ad azdata arc dc debug
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc dc debug.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3979dbd8561eeedf3e5c0ab03a51b04b2bd73674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052682"
---
# <a name="azdata-arc-dc-debug"></a>azdata arc dc debug

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc dc debug copy-logs](#azdata-arc-dc-debug-copy-logs) | Copia i log.
[azdata arc dc debug dump](#azdata-arc-dc-debug-dump) | Attiva il dump della memoria.
## <a name="azdata-arc-dc-debug-copy-logs"></a>azdata arc dc debug copy-logs
Copia i log di debug dal controller dati. È necessaria la configurazione di Kubernetes nel sistema in uso.
```bash
azdata arc dc debug copy-logs --namespace -ns 
                              [--container -c]  
                              
[--target-folder -d]  
                              
[--pod]  
                              
[--resource-kind -rk]  
                              
[--resource-name -rn]  
                              
[--timeout -t]  
                              
[--skip-compress -sc]  
                              
[--exclude-dumps -ed]
```
### <a name="required-parameters"></a>Parametri necessari
#### `--namespace -ns`
Spazio dei nomi Kubernetes del controller dati.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--container -c`
Copia i log relativi ai contenitori con nome simile. Facoltativo; per impostazione predefinita, vengono copiati i log di tutti i contenitori. Questo parametro non può essere specificato più volte. In caso contrario, verrà usato l'ultimo parametro specificato.
#### `--target-folder -d`
Percorso della cartella di destinazione in cui copiare i log. Facoltativo; per impostazione predefinita, il risultato viene creato nella cartella locale.  Questo parametro non può essere specificato più volte. In caso contrario, verrà usato l'ultimo parametro specificato.
#### `--pod`
Copia i log relativi ai pod con nome simile. Facoltativo; per impostazione predefinita, vengono copiati i log di tutti i pod. Questo parametro non può essere specificato più volte. In caso contrario, verrà usato l'ultimo parametro specificato.
#### `--resource-kind -rk`
Copiare i log per la risorsa di un determinato tipo. Questo parametro non può essere specificato più volte. In caso contrario, verrà usato l'ultimo parametro specificato. Se specificato, è necessario specificare anche --resource-name per identificare la risorsa.
#### `--resource-name -rn`
Copiare i log per la risorsa con il nome specificato. Questo parametro non può essere specificato più volte. In caso contrario, verrà usato l'ultimo parametro specificato. Se specificato, è necessario specificare anche --resource-kind per identificare la risorsa.
#### `--timeout -t`
Numero di secondi di attesa per il completamento del comando. Il valore predefinito è 0, ovvero illimitato.
#### `--skip-compress -sc`
Indica se ignorare o meno la compressione della cartella dei risultati. Il valore predefinito è False, ovvero la cartella dei risultati viene compressa.
#### `--exclude-dumps -ed`
Indica se escludere o meno i dump dalla cartella dei risultati. Il valore predefinito è False, ovvero i dump vengono inclusi.
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
## <a name="azdata-arc-dc-debug-dump"></a>azdata arc dc debug dump
Attiva il dump della memoria e lo copia dal contenitore. È necessaria la configurazione di Kubernetes nel sistema in uso.
```bash
azdata arc dc debug dump --namespace -ns 
                         [--container -c]  
                         
[--target-folder -d]
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--namespace -ns`
Spazio dei nomi Kubernetes del controller dati.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--container -c`
Contenitore di destinazione da attivare per il dump dei processi in esecuzione.
`controller`
#### `--target-folder -d`
Cartella di destinazione in cui copiare il dump. `./output/dump`
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

