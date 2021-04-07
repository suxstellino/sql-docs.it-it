---
title: Informazioni di riferimento su azdata bdc hdfs
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata bdc hdfs.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 27f27123d5b39efdc4c607a119858048dfc87859
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556704"
---
# <a name="azdata-bdc-hdfs"></a>azdata bdc hdfs

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata bdc hdfs status](reference-azdata-bdc-hdfs-status.md) | Comandi relativi allo stato del servizio HDFS.
[impostazioni HDFS BDC azdata](reference-azdata-bdc-hdfs-settings.md) | Comandi delle impostazioni BDC per il servizio HDFS.
[crittografia azdata BDC HDFS-zona](reference-azdata-bdc-hdfs-encryption-zone.md) | Gestire le zone di crittografia in HDFS.
[chiave HDFS BDC azdata](reference-azdata-bdc-hdfs-key.md) | Gestire le chiavi della zona di crittografia Hadoop.
[azdata bdc hdfs shell](#azdata-bdc-hdfs-shell) | La shell HDFS è una semplice shell dei comandi interattiva per file system HDFS.
[azdata bdc hdfs ls](#azdata-bdc-hdfs-ls) | Elenca lo stato del file o della directory specificata.
[azdata bdc hdfs exists](#azdata-bdc-hdfs-exists) | Determina se un file o una directory esiste realmente.  Restituisce True se esiste; in caso contrario, False.
[azdata bdc hdfs mkdir](#azdata-bdc-hdfs-mkdir) | Crea una directory nel percorso specificato.
[azdata bdc hdfs mv](#azdata-bdc-hdfs-mv) | Sposta il file o il percorso specificato nella posizione indicata.
[azdata bdc hdfs create](#azdata-bdc-hdfs-create) | Crea il file di testo nella posizione specificata.  È possibile aggiungere contenuto di testo semplice tramite il parametro data.
[azdata bdc hdfs cat](#azdata-bdc-hdfs-cat) | Legge il contenuto di un file.  Offset e lunghezza in byte sono parametri facoltativi.
[azdata bdc hdfs rm](#azdata-bdc-hdfs-rm) | Rimuove un file o una directory.
[azdata bdc hdfs rmr](#azdata-bdc-hdfs-rmr) | Rimuove in modo ricorsivo un file o una directory.
[azdata bdc hdfs chmod](#azdata-bdc-hdfs-chmod) | Modifica l'autorizzazione sul file o sulla directory specificata.
[azdata bdc hdfs chown](#azdata-bdc-hdfs-chown) | Modifica il proprietario o il gruppo del file specificato.
[azdata bdc hdfs cp](#azdata-bdc-hdfs-cp) | Copia un file o una directory tra il computer locale e HDFS.
[azdata bdc hdfs mount](reference-azdata-bdc-hdfs-mount.md) | Gestire il montaggio di archivi remoti in HDFS.
## <a name="azdata-bdc-hdfs-shell"></a>azdata bdc hdfs shell
La shell HDFS è una semplice shell dei comandi interattiva per file system HDFS.
```bash
azdata bdc hdfs shell 
```
### <a name="examples"></a>Esempi
Avviare la shell.
```bash
azdata bdc hdfs shell
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
## <a name="azdata-bdc-hdfs-ls"></a>azdata bdc hdfs ls
Elenca lo stato del file o della directory specificata.
```bash
azdata bdc hdfs ls --path -p 
                   
```
### <a name="examples"></a>Esempi
Stato dell'elenco.
```bash
azdata bdc hdfs ls --path "tmp/"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Percorso dello stato dell'elenco.
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
## <a name="azdata-bdc-hdfs-exists"></a>azdata bdc hdfs exists
Determina se un file o una directory esiste realmente.  Restituisce True se esiste; in caso contrario, False.
```bash
azdata bdc hdfs exists --path -p 
                       
```
### <a name="examples"></a>Esempi
Verificare la presenza di file o directory.
```bash
azdata bdc hdfs exists --path "tmp/"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Percorso di cui verificare la presenza.
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
## <a name="azdata-bdc-hdfs-mkdir"></a>azdata bdc hdfs mkdir
Crea una directory nel percorso specificato.
```bash
azdata bdc hdfs mkdir --path -p 
                      
```
### <a name="examples"></a>Esempi
Creare una directory.
```bash
azdata bdc hdfs mkdir --path "tmp/"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome della directory da creare.
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
## <a name="azdata-bdc-hdfs-mv"></a>azdata bdc hdfs mv
Sposta il file o il percorso specificato nella posizione indicata.
```bash
azdata bdc hdfs mv --source-path -s 
                   --target-path -t
```
### <a name="examples"></a>Esempi
Spostare un file o una directory.
```bash
azdata bdc hdfs mv --source-path "tmp/" --target-path "dest/"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--source-path -s`
La directory da spostare.
#### `--target-path -t`
La posizione in cui spostare la directory.
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
## <a name="azdata-bdc-hdfs-create"></a>azdata bdc hdfs create
Crea il file di testo nella posizione specificata.  È possibile aggiungere contenuto di testo semplice tramite il parametro data.
```bash
azdata bdc hdfs create --path -p 
                       --data -d
```
### <a name="examples"></a>Esempi
Creare un file.
```bash
azdata bdc hdfs create --path "tmp/test.txt" --data "This is a test."
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome del file da creare.
#### `--data -d`
Contenuto del file,  progettato per contenuto di testo semplice.
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
## <a name="azdata-bdc-hdfs-cat"></a>azdata bdc hdfs cat
Legge il contenuto di un file.  Offset e lunghezza in byte sono parametri facoltativi.
```bash
azdata bdc hdfs cat --path -p 
                    --offset  
                    
--length -l
```
### <a name="examples"></a>Esempi
Leggere un file.
```bash
azdata bdc hdfs cat --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome del file da leggere.
#### `--offset`
Numero dell'offset di byte all'interno del file da leggere.
#### `--length -l`
Lunghezza dei dati da leggere.
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
## <a name="azdata-bdc-hdfs-rm"></a>azdata bdc hdfs rm
Rimuove un file o una directory.
```bash
azdata bdc hdfs rm --path -p 
                   
```
### <a name="examples"></a>Esempi
Rimuove un file o una directory.
```bash
azdata bdc hdfs rm --path "tmp/"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome del file da rimuovere.
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
## <a name="azdata-bdc-hdfs-rmr"></a>azdata bdc hdfs rmr
Rimuove in modo ricorsivo un file o una directory.
```bash
azdata bdc hdfs rmr --path -p 
                    
```
### <a name="examples"></a>Esempi
Rimuovere una directory in modo ricorsivo.
```bash
azdata bdc hdfs rmr --path "tmp/"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome del file da rimuovere in modo ricorsivo.
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
## <a name="azdata-bdc-hdfs-chmod"></a>azdata bdc hdfs chmod
Modifica l'autorizzazione sul file o sulla directory specificata.
```bash
azdata bdc hdfs chmod --path -p 
                      --permission
```
### <a name="examples"></a>Esempi
Modificare le autorizzazioni di un file o una directory.
```bash
azdata bdc hdfs chmod --permission 775 --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome del file o della directory per cui impostare le autorizzazioni.
#### `--permission`
Ottetti delle autorizzazioni da impostare.  Esempio: "775".
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
## <a name="azdata-bdc-hdfs-chown"></a>azdata bdc hdfs chown
Modifica il proprietario o il gruppo del file specificato.
```bash
azdata bdc hdfs chown --path -p 
                      --owner  
                      
--group -g
```
### <a name="examples"></a>Esempi
Modificare il proprietario e il gruppo.
```bash
azdata bdc hdfs chown --owner hdfs --group superusergroup --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Nome del file o della directory per cui modificare il proprietario.
#### `--owner`
Nome del proprietario da impostare.
#### `--group -g`
Nome del gruppo da impostare.
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
## <a name="azdata-bdc-hdfs-cp"></a>azdata bdc hdfs cp
Copia un file o una directory tra il computer locale e HDFS.  Se l'input è una directory, viene copiato l'intero albero di directory.  Se il file o la directory di destinazione esiste già, il comando ha esito negativo.  Per specificare la directory HDFS remota, anteporre il prefisso "hdfs:" al percorso
```bash
azdata bdc hdfs cp --from-path -f 
                   --to-path -t
```
### <a name="examples"></a>Esempi
Copia di un file o di una directory tra il computer locale e HDFS.
```bash
azdata bdc hdfs cp --from_path "tmp/test.txt" --to-path "hdfs:/user/me/test.txt"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--from-path -f`
Nome del percorso da cui eseguire la copia.  Anteporre il prefisso "hdfs:" al percorso per indicare un percorso HDFS.
#### `--to-path -t`
Nome del percorso in cui eseguire la copia.  Anteporre il prefisso "hdfs:" al percorso per indicare un percorso HDFS.
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

