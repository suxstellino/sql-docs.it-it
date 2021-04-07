---
title: informazioni di riferimento sulle impostazioni Spark di azdata BDC
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi delle impostazioni Spark di azdata BDC.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 157154685979cb0a99d68bb78b6348305ee916cf
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556584"
---
# <a name="azdata-bdc-spark-settings"></a>impostazioni Spark azdata BDC

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi di **Impostazioni Spark** nello strumento **azdata** . Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[set di impostazioni Spark azdata BDC](#azdata-bdc-spark-settings-set) | Impostare le impostazioni dell'ambito del servizio Spark.
[azdata impostazioni Spark di integrazione applicativa dei dati](#azdata-bdc-spark-settings-show) | Mostra le impostazioni dell'ambito del servizio Spark e, facoltativamente, le impostazioni Spark per le risorse specificate
## <a name="azdata-bdc-spark-settings-set"></a>set di impostazioni Spark azdata BDC
Offre la possibilità di impostare un'impostazione con ambito di servizio o di risorsa. Specificare il nome completo dell'impostazione e il valore. Non applica l'impostazione all'oggetto BDC in esecuzione. Eseguire Apply a tale scopo.
```bash
azdata bdc spark settings set [--resources -r] 
                              [--settings -s]
```
### <a name="examples"></a>Esempio
Impostare il numero predefinito di core del driver su 1 e la memoria del driver su 1664 per il servizio Spark.
```bash
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=1,spark-defaults-conf.spark.driver.memory=1664m
```
Impostare il numero predefinito di core dell'executor su 1 per il pool di archiviazione.
```bash
azdata bdc spark settings set --settings spark-defaults-conf.spark.executor.cores=1 –resources storage-0
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--resources -r`
Imposta le impostazioni o le impostazioni specificate per le risorse specificate. Le risorse possono essere specificate come un elenco delimitato da virgole.
#### `--settings -s`
Imposta il valore configurato per le impostazioni fornite. È possibile impostare più impostazioni utilizzando un elenco delimitato da virgole.
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
## <a name="azdata-bdc-spark-settings-show"></a>azdata impostazioni Spark di integrazione applicativa dei dati
Mostra le impostazioni di ambito del servizio Spark di integrazione applicativa dei dati (facoltativamente con ambito risorsa). Per impostazione predefinita, questo comando Mostra le impostazioni dell'ambito del servizio configurate dall'utente. Sono disponibili filtri per mostrare tutte le impostazioni (gestite dal sistema e configurabili), le impostazioni configurabili o le impostazioni in sospeso. Per visualizzare un'impostazione specifica dell'ambito del servizio o della risorsa, è possibile specificare il nome dell'impostazione. Usare "ricorsivo" per visualizzare le impostazioni per tutte le risorse come parte del servizio.
```bash
azdata bdc spark settings show [--resources -r] 
                               [--settings -s]  
                               
[--filter-option -f]  
                               
[--recursive -rec]  
                               
[--include-details -i]  
                               
[--description -d]
```
### <a name="examples"></a>Esempio
Mostra le impostazioni dell'ambito del servizio Spark configurato dall'utente.
```bash
azdata bdc spark settings show
```
Mostra il valore in esecuzione e configurato per i core del driver Spark nel pool di archiviazione.
```bash
azdata bdc spark settings show --settings spark-defaults-conf.spark.driver.cores --resources storage-0 --include-details
```
Mostra qualsiasi impostazione configurabile relativa alla memoria per il servizio Spark.
```bash
azdata bdc spark settings show --settings *memory* --resources storage-0
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--resources -r`
Consente di visualizzare le impostazioni relative alle risorse specificate. Le risorse possono essere specificate come un elenco delimitato da virgole.
#### `--settings -s`
Visualizza le informazioni per i nomi delle impostazioni specificati.
#### `--filter-option -f`
Filtrare le impostazioni dell'ambito del cluster visualizzate, anziché solo le impostazioni "utente configurato". Sono disponibili filtri per mostrare tutte le impostazioni (configurabili dall'utente & gestite dal sistema), tutte le impostazioni configurabili o le impostazioni in sospeso.
`userConfigured`
#### `--recursive -rec`
Mostra le informazioni sull'impostazione per l'ambito specificato (servizio o risorsa del servizio) e tutti i componenti con ambito inferiore (risorse).
#### `--include-details -i`
Include dettagli aggiuntivi per le impostazioni selezionate per essere visualizzate.
#### `--description -d`
Include una descrizione dell'impostazione.
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
