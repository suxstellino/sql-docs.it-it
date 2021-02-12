---
title: Riferimento ad azdata arc postgres server
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc postgres server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 79d2cad23c6aed747837635e4d19d42a5daf41dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052642"
---
# <a name="azdata-arc-postgres-server"></a>azdata arc postgres server

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc postgres server create](#azdata-arc-postgres-server-create) | Creare un gruppo di server PostgreSQL.
[azdata arc postgres server edit](#azdata-arc-postgres-server-edit) | Modificare la configurazione di un gruppo di server PostgreSQL.
[azdata arc postgres server delete](#azdata-arc-postgres-server-delete) | Eliminare un gruppo di server PostgreSQL.
[azdata arc postgres server show](#azdata-arc-postgres-server-show) | Visualizzare i dettagli di un gruppo di server PostgreSQL.
[azdata arc postgres server list](#azdata-arc-postgres-server-list) | Visualizzare l'elenco dei gruppi di server PostgreSQL.
[azdata arc postgres server config](reference-azdata-arc-postgres-server-config.md) | Comandi di configurazione.
[azdata arc postgres server backup](reference-azdata-arc-postgres-server-backup.md) | Gestire i backup dei gruppi di server PostgreSQL.
## <a name="azdata-arc-postgres-server-create"></a>azdata arc postgres server create
Per impostare la password del gruppo di server, impostare la variabile di ambiente AZDATA_PASSWORD
```bash
azdata arc postgres server create --name -n 
                                  [--path]  
                                  
[--cores-limit -cl]  
                                  
[--cores-request -cr]  
                                  
[--memory-limit -ml]  
                                  
[--memory-request -mr]  
                                  
[--storage-class-data -scd]  
                                  
[--storage-class-logs -scl]  
                                  
[--storage-class-backups -scb]  
                                  
[--extensions]  
                                  
[--volume-size-data -vsd]  
                                  
[--volume-size-logs -vsl]  
                                  
[--volume-size-backups -vsb]  
                                  
[--workers -w]  
                                  
[--engine-version -ev]  
                                  
[--no-external-endpoint]  
                                  
[--dev]  
                                  
[--port]  
                                  
[--no-wait]  
                                  
[--engine-settings -e]
```
### <a name="examples"></a>Esempi
Creare un gruppo di server PostgreSQL.
```bash
azdata arc postgres server create -n pg1
```
Creare un gruppo di server PostgreSQL con le impostazioni del motore. Entrambi gli esempi seguenti sono validi.
```bash
azdata arc postgres server create -n pg1 --engine-settings "key1=val1"
azdata arc postgres server create -n pg1 --engine-settings "key2=val2"
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome dell’istanza.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path`
Percorso del file JSON di origine per il gruppo di server PostgreSQL. Questo indirizzo è facoltativo.
#### `--cores-limit -cl`
Numero massimo di core della CPU per l'istanza di Postgres che è possibile usare per ogni nodo. Sono supportati i core frazionari.
#### `--cores-request -cr`
Numero minimo di core della CPU che devono essere disponibili per ogni nodo per pianificare il servizio. Sono supportati i core frazionari.
#### `--memory-limit -ml`
Limite di memoria dell'istanza di Postgres come numero seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--memory-request -mr`
Richiesta di memoria dell'istanza di Postgres come numero seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--storage-class-data -scd`
Classe di archiviazione da usare per i volumi permanenti di dati.
#### `--storage-class-logs -scl`
Classe di archiviazione da usare per i volumi permanenti di log.
#### `--storage-class-backups -scb`
Classe di archiviazione da usare per i volumi permanenti di backup.
#### `--extensions`
Elenco delimitato da virgole delle estensioni Postgres che devono essere caricate all'avvio. Per informazioni sui valori supportati, vedere la documentazione di Postgres.
#### `--volume-size-data -vsd`
Dimensioni del volume di archiviazione da usare per i dati come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-logs -vsl`
Dimensioni del volume di archiviazione da usare per i log come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-backups -vsb`
Dimensioni del volume di archiviazione da usare per i backup come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--workers -w`
Numero di nodi di lavoro di cui eseguire il provisioning in un cluster partizionato oppure zero (impostazione predefinita) per Postgres a nodo singolo.
#### `--engine-version -ev`
Deve essere 11 o 12. Il valore predefinito è 12.
#### `--no-external-endpoint`
Se specificato, non verrà creato alcun servizio esterno. In caso contrario, verrà creato un servizio esterno usando lo stesso tipo di servizio del controller di dati.
#### `--dev`
Se specificato, viene considerata un'istanza di sviluppo e non verrà fatturata.
#### `--port`
facoltativo.
#### `--no-wait`
Se specificato, il comando non attenderà che lo stato dell'istanza sia pronto prima della restituzione.
#### `--engine-settings -e`
Elenco delimitato da virgole delle impostazioni del motore Postgres nel formato 'key1=val1, key2=val2'.
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
## <a name="azdata-arc-postgres-server-edit"></a>azdata arc postgres server edit
Modificare la configurazione di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server edit --name -n 
                                [--path]  
                                
[--workers -w]  
                                
[--engine-version -ev]  
                                
[--cores-limit -cl]  
                                
[--cores-request -cr]  
                                
[--memory-limit -ml]  
                                
[--memory-request -mr]  
                                
[--extensions]  
                                
[--dev]  
                                
[--port]  
                                
[--no-wait]  
                                
[--engine-settings -e]  
                                
[--replace-engine-settings -re]  
                                
[--admin-password]
```
### <a name="examples"></a>Esempi
Modificare la configurazione di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server edit --src ./spec.json -n pg1
```
Modificare un gruppo di server PostgreSQL con le impostazioni del motore.
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key2=val2"
```
Modifica un gruppo di server PostgreSQL e sostituisce le impostazioni del motore esistenti con la nuova impostazione key1=val1.
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key1=val1" --replace-engine-settings
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server PostgreSQL in corso di modifica. Il nome con cui viene distribuita l'istanza non può essere modificato.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path`
Percorso del file JSON di origine per il gruppo di server PostgreSQL. Questo indirizzo è facoltativo.
#### `--workers -w`
Numero di nodi di lavoro di cui eseguire il provisioning in un cluster partizionato oppure zero (impostazione predefinita) per Postgres a nodo singolo.
#### `--engine-version -ev`
La versione del motore non può essere modificata. --engine-version può essere usato in associazione a --name per identificare un gruppo di server PostgreSQL Hyperscale quando due gruppi di server con versione del motore diversa hanno lo stesso nome. --engine-version è facoltativo e se usato per identificare un gruppo di server deve essere 11 o 12.
#### `--cores-limit -cl`
Numero massimo di core della CPU per l'istanza Postgres che è possibile usare per ogni nodo. I core frazionari sono supportati. Per rimuovere cores_limit, specificare il valore come stringa vuota.
#### `--cores-request -cr`
Numero minimo di core della CPU che devono essere disponibili per ogni nodo per pianificare il servizio. I core frazionari sono supportati. Per rimuovere cores_request, specificare il valore come stringa vuota.
#### `--memory-limit -ml`
Limite di memoria per l'istanza di Postgres come numero seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte). Per rimuovere memory_limit, specificare il valore come stringa vuota.
#### `--memory-request -mr`
Richiesta di memoria per l'istanza di Postgres come numero seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte). Per rimuovere memory_request, specificare il valore come stringa vuota.
#### `--extensions`
Elenco delimitato da virgole delle estensioni Postgres che devono essere caricate all'avvio. Per informazioni sui valori supportati, vedere la documentazione di Postgres.
#### `--dev`
Se specificato, viene considerata un'istanza di sviluppo e non verrà fatturata.
#### `--port`
facoltativo.
#### `--no-wait`
Se specificato, il comando non attenderà che lo stato dell'istanza sia pronto prima della restituzione.
#### `--engine-settings -e`
Elenco delimitato da virgole delle impostazioni del motore Postgres nel formato 'key1=val1, key2=val2'. Le impostazioni specificate verranno unite con le impostazioni esistenti. Per rimuovere un'impostazione, specificare un valore vuoto, ad esempio 'removedKey='. Se si modifica un'impostazione del motore che richiede un riavvio, il servizio verrà riavviato per applicare immediatamente le impostazioni.
#### `--replace-engine-settings -re`
Quando viene specificato con --engine-settings, verranno sostituite tutte le impostazioni del motore personalizzate esistenti con il nuovo set di impostazioni e valori.
#### `--admin-password`
Se specificato, la password di amministratore del server Postgres verrà impostata sul valore della variabile di ambiente AZDATA_PASSWORD se presente e su un valore richiesto in caso contrario.
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
## <a name="azdata-arc-postgres-server-delete"></a>azdata arc postgres server delete
Eliminare un gruppo di server PostgreSQL.
```bash
azdata arc postgres server delete --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>Esempi
Eliminare un gruppo di server PostgreSQL.
```bash
azdata arc postgres server delete -n pg1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server PostgreSQL.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--engine-version -ev`
--engine-version può essere usato in associazione a --name per identificare un gruppo di server PostgreSQL Hyperscale quando due gruppi di server con versione del motore diversa hanno lo stesso nome. --engine-version è facoltativo e se usato per identificare un gruppo di server deve essere 11 o 12.
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
## <a name="azdata-arc-postgres-server-show"></a>azdata arc postgres server show
Visualizzare i dettagli di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server show --name -n 
                                [--engine-version -ev]  
                                
[--path -p]
```
### <a name="examples"></a>Esempi
Visualizzare i dettagli di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server show -n pg1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server PostgreSQL.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--engine-version -ev`
--engine-version può essere usato in associazione a --name per identificare un gruppo di server PostgreSQL Hyperscale quando due gruppi di server con versione del motore diversa hanno lo stesso nome. --engine-version è facoltativo e se usato per identificare un gruppo di server deve essere 11 o 12.
#### `--path -p`
Percorso in cui scrivere la specifica completa per il gruppo di server PostgreSQL. Se omesso, la specifica verrà scritta nell'output standard.
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
## <a name="azdata-arc-postgres-server-list"></a>azdata arc postgres server list
Visualizzare l'elenco dei gruppi di server PostgreSQL.
```bash
azdata arc postgres server list 
```
### <a name="examples"></a>Esempi
Visualizzare l'elenco dei gruppi di server PostgreSQL.
```bash
azdata arc postgres server list
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

