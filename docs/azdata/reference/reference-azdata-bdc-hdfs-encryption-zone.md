---
title: azdata BDC HDFS Encryption-riferimento alla zona
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata BDC HDFS Encryption-zone.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e2050d19f2b1feb3e32e15e721b9868ba3cbaf1c
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557269"
---
# <a name="azdata-bdc-hdfs-encryption-zone"></a>crittografia azdata BDC HDFS-zona

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata BDC HDFS Encryption-area Creazione](#azdata-bdc-hdfs-encryption-zone-create) | Converte una cartella HDFS in una zona di crittografia.
[azdata BDC HDFS Encryption-elenco zone](#azdata-bdc-hdfs-encryption-zone-list) | Elencare tutte le zone di crittografia e le chiavi associate alle zone di crittografia.
[azdata BDC HDFS Encryption-zona Get-File-Encryption-info](#azdata-bdc-hdfs-encryption-zone-get-file-encryption-info) | Ottiene le informazioni di crittografia per un percorso di file HDFS specificato.
[crittografia azdata BDC HDFS-stato zona](#azdata-bdc-hdfs-encryption-zone-status) | Ottenere lo stato di nuova crittografia per il cluster HDFS.
[crittografia HDFS BDC azdata-ricrittografia zona](#azdata-bdc-hdfs-encryption-zone-reencrypt) | Controlla la nuova crittografia della zona di crittografia specificata dall'argomento--path.
## <a name="azdata-bdc-hdfs-encryption-zone-create"></a>azdata BDC HDFS Encryption-area Creazione
Converte una cartella HDFS in una zona di crittografia usando la chiave specificata per crittografare i file nell'area di crittografia.
```bash
azdata bdc hdfs encryption-zone create --path -p 
                                       --keyname -k
```
### <a name="examples"></a>Esempio
Per convertire una cartella esistente/User/securefolder in una zona di crittografia, usando una chiave denominata securelake
```bash
azdata bdc hdfs encryption-zone create --path /home/securefolder --keyname securelake
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso della cartella HDFS da convertire nell'area di crittografia.
#### `--keyname -k`
Chiave del servizio di gestione delle chiavi Hadoop da usare per proteggere la zona di crittografia.
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
## <a name="azdata-bdc-hdfs-encryption-zone-list"></a>azdata BDC HDFS Encryption-elenco zone
Elencare tutte le zone di crittografia e le chiavi associate alle zone di crittografia.
```bash
azdata bdc hdfs encryption-zone list 
```
### <a name="examples"></a>Esempio
Elenca tutte le zone di crittografia.
```bash
azdata bdc hdfs encryption-zone list
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
## <a name="azdata-bdc-hdfs-encryption-zone-get-file-encryption-info"></a>azdata BDC HDFS Encryption-zona Get-File-Encryption-info
Ottiene le informazioni di crittografia per un percorso di file HDFS specificato.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path -p 
                                                         
```
### <a name="examples"></a>Esempio
Ottenere le informazioni di crittografia per un file in/User/securefolder/data.csv.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path /user/securefolder/data.csv
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso del file HDFS per il quale devono essere recuperate le informazioni di crittografia.
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
## <a name="azdata-bdc-hdfs-encryption-zone-status"></a>crittografia azdata BDC HDFS-stato zona
Ottenere lo stato di nuova crittografia per il cluster HDFS.
```bash
azdata bdc hdfs encryption-zone status 
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
## <a name="azdata-bdc-hdfs-encryption-zone-reencrypt"></a>crittografia HDFS BDC azdata-ricrittografia zona
Controlla la nuova crittografia della zona di crittografia specificata dall'argomento--path.
```bash
azdata bdc hdfs encryption-zone reencrypt --path -p 
                                          --action -a
```
### <a name="examples"></a>Esempio
Avviare la nuova crittografia della zona di crittografia securelake.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
```
Annullare la nuova crittografia della zona di crittografia securelake.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action cancel
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso della cartella della zona di crittografia HDFS che deve essere crittografata di nuovo
#### `--action -a`
Eseguire nuovamente la crittografia dell'azione da eseguire. I valori validi sono Start e Cancel
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

