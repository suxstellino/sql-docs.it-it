---
title: Riferimento ad azdata arc dc
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc dc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1f1464d84655b942f294f71af53cf55b226372c5
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557434"
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
azdata arc dc create 
```
### <a name="examples"></a>Esempi
Distribuzione del controller dati.
```bash
azdata arc dc create
```
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
azdata arc dc delete 
```
### <a name="examples"></a>Esempi
Distribuzione del controller dati.
```bash
azdata arc dc delete
```
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
azdata arc dc export 
```
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
azdata arc dc upload 
```
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

