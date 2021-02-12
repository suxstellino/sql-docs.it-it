---
title: Riferimento ad azdata arc sql mi
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc sql mi.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6a3d561f6b09ca6720a70a067e5985fbc96dceb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048951"
---
# <a name="azdata-arc-sql-mi"></a>azdata arc sql mi

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc sql mi create](#azdata-arc-sql-mi-create) | Crea un'istanza gestita di SQL.
[azdata arc sql mi edit](#azdata-arc-sql-mi-edit) | Modifica la configurazione di un'istanza gestita di SQL.
[azdata arc sql mi delete](#azdata-arc-sql-mi-delete) | Elimina un'istanza gestita di SQL.
[azdata arc sql mi show](#azdata-arc-sql-mi-show) | Visualizza i dettagli di un'istanza gestita di SQL.
[azdata arc sql mi list](#azdata-arc-sql-mi-list) | Elenca le istanze gestite di SQL.
[azdata arc sql mi config](reference-azdata-arc-sql-mi-config.md) | Comandi di configurazione.
## <a name="azdata-arc-sql-mi-create"></a>azdata arc sql mi create
Per impostare la password dell'istanza gestita di SQL, impostare la variabile di ambiente AZDATA_PASSWORD
```bash
azdata arc sql mi create --name -n 
                         [--path]  
                         
[--cores-limit -cl]  
                         
[--cores-request -cr]  
                         
[--memory-limit -ml]  
                         
[--memory-request -mr]  
                         
[--storage-class-data -scd]  
                         
[--storage-class-logs -scl]  
                         
[--storage-class-data-logs -scdl]  
                         
[--storage-class-backups -scb]  
                         
[--volume-size-data -vsd]  
                         
[--volume-size-logs -vsl]  
                         
[--volume-size-data-logs -vsdl]  
                         
[--volume-size-backups -vsb]  
                         
[--no-external-endpoint]  
                         
[--dev]  
                         
[--no-wait]
```
### <a name="examples"></a>Esempi
Crea un'istanza gestita di SQL.
```bash
azdata arc sql mi create -n sqlmi1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome dell'istanza gestita di SQL.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path`
Percorso del file src per il file JSON dell'istanza gestita di SQL.
#### `--cores-limit -cl`
Limite di core dell'istanza gestita come numero intero.
#### `--cores-request -cr`
Richiesta di core dell'istanza gestita come numero intero.
#### `--memory-limit -ml`
Limite della capacità dell'istanza gestita come numero intero.
#### `--memory-request -mr`
Richiesta di capacità dell'istanza gestita come numero intero di memoria in GB.
#### `--storage-class-data -scd`
Classe di archiviazione da usare per i dati (con estensione mdf). Se non viene indicato alcun valore, non verrà specificata nessuna classe di archiviazione. Kubernetes userà quindi la classe di archiviazione predefinita.
#### `--storage-class-logs -scl`
Classe di archiviazione da usare per i log (/var/log). Se non viene indicato alcun valore, non verrà specificata nessuna classe di archiviazione. Kubernetes userà quindi la classe di archiviazione predefinita.
#### `--storage-class-data-logs -scdl`
Classe di archiviazione da usare per i log di database (con estensione ldf). Se non viene indicato alcun valore, non verrà specificata nessuna classe di archiviazione. Kubernetes userà quindi la classe di archiviazione predefinita.
#### `--storage-class-backups -scb`
Classe di archiviazione da usare per i backup (/var/opt/MSSQL/backups). Se non viene indicato alcun valore, non verrà specificata nessuna classe di archiviazione. Kubernetes userà quindi la classe di archiviazione predefinita.
#### `--volume-size-data -vsd`
Dimensioni del volume di archiviazione da usare per i dati come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-logs -vsl`
Dimensioni del volume di archiviazione da usare per i log come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-data-logs -vsdl`
Dimensioni del volume di archiviazione da usare per i log di dati come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-backups -vsb`
Dimensioni del volume di archiviazione da usare per i backup come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--no-external-endpoint`
Se specificato, non verrà creato alcun servizio esterno. In caso contrario, verrà creato un servizio esterno usando lo stesso tipo di servizio del controller dati.
#### `--dev`
Se specificato, viene considerata un'istanza di sviluppo e non verrà fatturata.
#### `--no-wait`
Se specificato, il comando non attenderà che lo stato dell'istanza sia pronto prima della restituzione.
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
## <a name="azdata-arc-sql-mi-edit"></a>azdata arc sql mi edit
Modifica la configurazione di un'istanza gestita di SQL.
```bash
azdata arc sql mi edit --name -n 
                       [--path]  
                       
[--cores-limit -cl]  
                       
[--cores-request -cr]  
                       
[--memory-limit -ml]  
                       
[--memory-request -mr]  
                       
[--dev]  
                       
[--no-wait]
```
### <a name="examples"></a>Esempi
Modifica la configurazione di un'istanza gestita di SQL.
```bash
azdata arc sql mi edit --path ./spec.json -n sqlmi1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome dell'istanza gestita di SQL sottoposta a modifica. Il nome con cui viene distribuita l'istanza non può essere modificato.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path`
Percorso del file src per il file JSON dell'istanza gestita di SQL.
#### `--cores-limit -cl`
Limite di core dell'istanza gestita come numero intero.
#### `--cores-request -cr`
Richiesta di core dell'istanza gestita come numero intero.
#### `--memory-limit -ml`
Limite della capacità dell'istanza gestita come numero intero.
#### `--memory-request -mr`
Richiesta di capacità dell'istanza gestita come numero intero di memoria in GB.
#### `--dev`
Se specificato, viene considerata un'istanza di sviluppo e non verrà fatturata.
#### `--no-wait`
Se specificato, il comando non attenderà che lo stato dell'istanza sia pronto prima della restituzione.
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
## <a name="azdata-arc-sql-mi-delete"></a>azdata arc sql mi delete
Elimina un'istanza gestita di SQL.
```bash
azdata arc sql mi delete --name -n 
                         
```
### <a name="examples"></a>Esempi
Elimina un'istanza gestita di SQL.
```bash
azdata arc sql mi delete -n sqlmi1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome dell'istanza gestita di SQL da eliminare.
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
## <a name="azdata-arc-sql-mi-show"></a>azdata arc sql mi show
Visualizza i dettagli di un'istanza gestita di SQL.
```bash
azdata arc sql mi show --name -n 
                       [--path -p]
```
### <a name="examples"></a>Esempi
Visualizza i dettagli di un'istanza gestita di SQL.
```bash
azdata arc sql mi show -n sqlmi1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome dell'istanza gestita di SQL da visualizzare.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path -p`
Percorso in cui deve essere scritta la specifica completa per l'istanza gestita di SQL. Se omesso, la specifica verrà scritta nell'output standard.
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
## <a name="azdata-arc-sql-mi-list"></a>azdata arc sql mi list
Elenca le istanze gestite di SQL.
```bash
azdata arc sql mi list 
```
### <a name="examples"></a>Esempi
Elenca le istanze gestite di SQL.
```bash
azdata arc sql mi list
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

