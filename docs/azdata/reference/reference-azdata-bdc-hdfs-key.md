---
title: riferimento alla chiave HDFS BDC azdata
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata BDC HDFS Key.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c208d1c726bae41b349abdf475627afbc82ed2b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342499"
---
# <a name="azdata-bdc-hdfs-key"></a>chiave HDFS BDC azdata

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi
|Comando|Descrizione|
| --- | --- |
[creazione della chiave HDFS BDC azdata](#azdata-bdc-hdfs-key-create) | Creare una chiave di HDFS.
[Elenco chiavi HDFS azdata BDC](#azdata-bdc-hdfs-key-list) | Elenca tutte le chiavi della zona di crittografia Hadoop.
[azdata del rollup della chiave HDFS BDC](#azdata-bdc-hdfs-key-roll) | Esegue il rollback di una chiave HDFS.
## <a name="azdata-bdc-hdfs-key-create"></a>creazione della chiave HDFS BDC azdata
Creare una chiave HDFS con il nome e la dimensione specificati.
```bash
azdata bdc hdfs key create --name -n 
                           [--size -size]
```
### <a name="examples"></a>Esempio
Per creare una chiave a 256 bit con il nome Key1, azdata HDFS Key create--name Key1--size 256
```bash
azdata hdfs key create --name key1 --size 256
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome della chiave della zona di crittografia Hadoop. 
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--size -size`
Lunghezza in bit della chiave di crittografia Hadoop. la lunghezza predefinita è 256.
`256`
### <a name="global-arguments"></a>Argomenti globali
#### `--debug`
Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug.
#### `--help -h`
Visualizza questo messaggio della guida ed esce.
#### `--output -o`
Formato di output.  Valori consentiti: json, jsonc, table, tsv.  Valore predefinito: json.
#### `--query -q`
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.
## <a name="azdata-bdc-hdfs-key-list"></a>Elenco chiavi HDFS azdata BDC
Elenca tutte le chiavi della zona di crittografia Hadoop.
```bash
azdata bdc hdfs key list 
```
### <a name="examples"></a>Esempio
Elencare tutte le chiavi della zona di crittografia Hadoop, elenco chiavi HDFS BDC azdata
```bash
azdata bdc hdfs key list
```
### <a name="global-arguments"></a>Argomenti globali
#### `--debug`
Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug.
#### `--help -h`
Visualizza questo messaggio della guida ed esce.
#### `--output -o`
Formato di output.  Valori consentiti: json, jsonc, table, tsv.  Valore predefinito: json.
#### `--query -q`
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.
## <a name="azdata-bdc-hdfs-key-roll"></a>azdata del rollup della chiave HDFS BDC
Esegue il Rolling di una chiave HDFS con il nome specificato.
```bash
azdata bdc hdfs key roll --name -n 
                         
```
### <a name="examples"></a>Esempio
Per eseguire il rollforward di una chiave denominata Key1, azdata HDFS Key Roll--Name Key1.
```bash
azdata hdfs key roll --name key1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome della chiave della zona di crittografia da ripristinare in una nuova versione. 
### <a name="global-arguments"></a>Argomenti globali
#### `--debug`
Aumenta il livello di dettaglio della registrazione per mostrare tutti i log di debug.
#### `--help -h`
Visualizza questo messaggio della guida ed esce.
#### `--output -o`
Formato di output.  Valori consentiti: json, jsonc, table, tsv.  Valore predefinito: json.
#### `--query -q`
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md). 

Per altre informazioni su come installare lo strumento **azdata**, vedere [Installare azdata](..\install\deploy-install-azdata.md).
