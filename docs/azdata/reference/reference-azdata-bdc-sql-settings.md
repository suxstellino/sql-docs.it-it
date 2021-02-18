---
title: informazioni di riferimento sulle impostazioni SQL BDC azdata
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi delle impostazioni SQL BDC di azdata.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 208f136f246cbcd6af612fbd4867a2a50c0f0cae
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567963"
---
# <a name="azdata-bdc-sql-settings"></a>Impostazioni SQL BDC azdata

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **delle impostazioni SQL** nello strumento **azdata** . Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi
|Comando|Descrizione|
| --- | --- |
[set di impostazioni SQL BDC azdata](#azdata-bdc-sql-settings-set) | Impostare SQL Service-impostazioni ambito.
[Mostra impostazioni SQL BDC azdata](#azdata-bdc-sql-settings-show) | Mostra le impostazioni dell'ambito del servizio SQL e, facoltativamente, le impostazioni SQL per le risorse specificate.

## <a name="azdata-bdc-sql-settings-set"></a>set di impostazioni SQL BDC azdata
Offre la possibilità di impostare un'impostazione con ambito di servizio o di risorsa. Specificare il nome completo dell'impostazione e il valore. Non applica l'impostazione all'oggetto BDC in esecuzione. Eseguire l'aggiornamento a tale scopo.
```bash
azdata bdc sql settings set --settings -s 
                        
```
### <a name="examples"></a>Esempio
Abilitare SQL Agent nell'istanza di SQL Server master.
```bash 
azdata bdc sql settings set --settings mssql.sqlagent.enabled=true --resources master 
``` 
Impostare il numero di CPU per SQL Server nel pool di dati.
```bash 
azdata bdc sql settings set --settings mssql.numberOfCpus=10 –resources data-0 
``` 

### <a name="required-parameters"></a>Parametri necessari
#### `--settings -s`
Imposta il valore configurato per le impostazioni fornite. È possibile impostare più impostazioni utilizzando un elenco delimitato da virgole.
### <a name="optional-parameters"></a>Parametri facoltativi 
#### `--resources` 
Imposta le impostazioni o le impostazioni specificate per le risorse specificate. Le risorse possono essere elencate come un elenco delimitato da virgole. 

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

## <a name="azdata-bdc-sql-settings-show"></a>Mostra impostazioni SQL BDC azdata
Mostra le impostazioni dell' `sql` ambito del servizio (facoltativamente Resource-scope) dell'integrazione applicativa dei dati. Per impostazione predefinita, questo comando Mostra le impostazioni dell'ambito del servizio configurate dall'utente. Sono disponibili filtri per mostrare tutte le impostazioni (gestite dal sistema & configurabili), le impostazioni configurabili o le impostazioni in sospeso. Per visualizzare un'impostazione specifica dell'ambito del servizio o della risorsa, è possibile specificare il nome dell'impostazione. Usare "ricorsivo" per visualizzare le impostazioni per tutte le risorse come parte del servizio. 
```bash

azdata bdc sql settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Esempio
Mostra le impostazioni dell'ambito del servizio SQL configurato dall'utente 
```bash
azdata bdc sql settings show
```
Mostra la configurazione di max server memory SQL nel pool di dati.
```bash
azdata bdc sql settings show --settings mssql.maxServerMemory --resources data-0 
```
Mostra le modifiche delle impostazioni in sospeso per le impostazioni ambito servizio SQL e ambito risorsa.
```bash
azdata bdc sql settings show --filter-options=pending --recursive --include-details
```
### <a name="optional-parameters"></a>Parametri facoltativi 
#### `--filter-options | -f` 
Opzioni per filtrare le impostazioni del livello di servizio o dell'ambito di risorsa visualizzate, anziché solo le impostazioni configurate dall'utente. Sono disponibili filtri per mostrare tutte le impostazioni (configurabili dall'utente & gestite dal sistema), tutte le impostazioni configurabili o le impostazioni in sospeso. Opzioni: `userConfigured` , `all` , `pending` , `configurable` .
#### `--settings | -s` 
Visualizza le informazioni per i nomi di impostazione specificati 
#### `--include-details | -i` 
Include dettagli aggiuntivi per le impostazioni selezionate per essere visualizzate 
#### `--description | -d` 
Includere la descrizione dell'impostazione. Deve essere usato con--include-Details 
#### `--resources | -r` 
Consente di visualizzare le impostazioni relative alle risorse specificate. Le risorse possono essere elencate come un elenco delimitato da virgole. 
#### `--recursive | -rec` 
Mostra le informazioni sull'impostazione per l'ambito specificato (servizio o risorsa di servizio), nonché tutti i componenti con ambito inferiore (risorse). 

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

Per altre informazioni su come installare lo strumento **azdata**, vedere [Installare azdata per gestire i cluster Big Data di SQL Server 2019](../install/deploy-install-azdata.md).
