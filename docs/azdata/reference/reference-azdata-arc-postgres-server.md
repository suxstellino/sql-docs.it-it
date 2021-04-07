---
title: Riferimento ad azdata arc postgres server
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc postgres server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 09c873049d81a62ae53f882396fb0230e11ce155
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556884"
---
# <a name="azdata-arc-postgres-server"></a>azdata arc postgres server

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc postgres server create](#azdata-arc-postgres-server-create) | Creare un gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
[azdata arc postgres server edit](#azdata-arc-postgres-server-edit) | Modificare la configurazione di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
[azdata arc postgres server delete](#azdata-arc-postgres-server-delete) | Eliminare un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
[azdata arc postgres server show](#azdata-arc-postgres-server-show) | Mostra i dettagli di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
[azdata arc postgres server list](#azdata-arc-postgres-server-list) | Elenca i gruppi di server con iperscalabilità di PostgreSQL abilitati per Azure Arc.
[azdata arc postgres server config](reference-azdata-arc-postgres-server-config.md) | Comandi di configurazione.
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
                                  
[--volume-claim-mounts -vcm]  
                                  
[--extensions]  
                                  
[--volume-size-data -vsd]  
                                  
[--volume-size-logs -vsl]  
                                  
[--volume-size-backups -vsb]  
                                  
[--workers -w]  
                                  
[--engine-version -ev]  
                                  
[--no-external-endpoint]  
                                  
[--port]  
                                  
[--no-wait]  
                                  
[--engine-settings -e]
```
### <a name="examples"></a>Esempi
Creare un gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server create -n pg1
```
Creare un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc con le impostazioni del motore. Entrambi gli esempi seguenti sono validi.
```bash
azdata arc postgres server create -n pg1 --engine-settings "key1=val1"
azdata arc postgres server create -n pg1 --engine-settings "key2=val2"
```
Creare un gruppo di server PostgreSQL con i montaggi del volume Claim.
```bash
azdata arc postgres server create -n pg1 --volume-claim-mounts backup-pvc:backup
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path`
Percorso del file JSON di origine per il gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc. Questo indirizzo è facoltativo.
#### `--cores-limit -cl`
Il numero massimo di core CPU per il gruppo di server di scalabilità PostgreSQL abilitato per Azure Arc che può essere usato per nodo. Sono supportati i core frazionari.
#### `--cores-request -cr`
Numero minimo di core della CPU che devono essere disponibili per ogni nodo per pianificare il servizio. Sono supportati i core frazionari.
#### `--memory-limit -ml`
Il limite di memoria del gruppo di server per l'iperscalabilità PostgreSQL abilitato per Azure Arc è un numero seguito da Ki (kilobyte), mi (megabyte) o gi (gigabyte).
#### `--memory-request -mr`
La richiesta di memoria del gruppo di server di iperscalabilità PostgreSQL abilitato per Azure Arc è un numero seguito da Ki (kilobyte), mi (megabyte) o gi (gigabyte).
#### `--storage-class-data -scd`
Classe di archiviazione da usare per i volumi permanenti di dati.
#### `--storage-class-logs -scl`
Classe di archiviazione da usare per i volumi permanenti di log.
#### `--storage-class-backups -scb`
Classe di archiviazione da usare per i volumi permanenti di backup.
#### `--volume-claim-mounts -vcm`
Elenco delimitato da virgole di montaggi di attestazioni del volume. Un montaggio di attestazioni volume è una coppia di un'attestazione di volume permanente esistente (nello stesso spazio dei nomi) e di un tipo di volume (e metadati facoltativi a seconda del tipo di volume) separati da due punti. Il volume permanente verrà montato in ogni pod per il gruppo di server PostgreSQL. Il percorso di montaggio può dipendere dal tipo di volume.
#### `--extensions`
Elenco delimitato da virgole delle estensioni Postgres che devono essere caricate all'avvio. Per informazioni sui valori supportati, vedere la documentazione di Postgres.
#### `--volume-size-data -vsd`
Dimensioni del volume di archiviazione da usare per i dati come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-logs -vsl`
Dimensioni del volume di archiviazione da usare per i log come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--volume-size-backups -vsb`
Dimensioni del volume di archiviazione da usare per i backup come numero positivo seguito da Ki (kilobyte), Mi (megabyte) o Gi (gigabyte).
#### `--workers -w`
Numero di nodi di lavoro di cui eseguire il provisioning in un gruppo di server. In anteprima, la riduzione del numero di nodi del ruolo di lavoro non è supportata. Per ulteriori informazioni, fare riferimento alla documentazione.
#### `--engine-version -ev`
Deve essere 11 o 12. Il valore predefinito è 12.
`12`
#### `--no-external-endpoint`
Se specificato, non verrà creato alcun servizio esterno. In caso contrario, verrà creato un servizio esterno usando lo stesso tipo di servizio del controller dati.
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
Modificare la configurazione di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server edit --name -n 
                                [--path]  
                                
[--workers -w]  
                                
[]  
                                
[--cores-limit -cl]  
                                
[--cores-request -cr]  
                                
[--memory-limit -ml]  
                                
[--memory-request -mr]  
                                
[--extensions]  
                                
[--port]  
                                
[--no-wait]  
                                
[--engine-settings -e]  
                                
[--replace-engine-settings -re]  
                                
[--admin-password]
```
### <a name="examples"></a>Esempio
Modificare la configurazione di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server edit --src ./spec.json -n pg1
```
Modificare un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc con le impostazioni del motore.
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key2=val2"
```
Consente di modificare un gruppo di server di scalabilità PostgreSQL abilitato per Azure Arc e di sostituire le impostazioni del motore esistenti con la nuova impostazione Key1 = val1.
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key1=val1" --replace-engine-settings
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc in corso di modifica. Il nome con cui viene distribuita l'istanza non può essere modificato.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path`
Percorso del file JSON di origine per il gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc. Questo indirizzo è facoltativo.
#### `--workers -w`
Numero di nodi di lavoro di cui eseguire il provisioning in un gruppo di server. In anteprima, la riduzione del numero di nodi del ruolo di lavoro non è supportata. Per ulteriori informazioni, fare riferimento alla documentazione.
#### <a name=""></a>``
La versione del motore non può essere modificata. --engine-version può essere usato in associazione a --name per identificare un gruppo di server PostgreSQL Hyperscale quando due gruppi di server con versione del motore diversa hanno lo stesso nome. --engine-version è facoltativo e se usato per identificare un gruppo di server deve essere 11 o 12.
#### `--cores-limit -cl`
Il numero massimo di core CPU per il gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc che può essere usato per nodo, i core frazionari sono supportati. Per rimuovere cores_limit, specificare il valore come stringa vuota.
#### `--cores-request -cr`
Numero minimo di core della CPU che devono essere disponibili per ogni nodo per pianificare il servizio. I core frazionari sono supportati. Per rimuovere cores_request, specificare il valore come stringa vuota.
#### `--memory-limit -ml`
Il limite di memoria per il gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc è un numero seguito da Ki (kilobyte), mi (megabyte) o gi (gigabyte). Per rimuovere memory_limit, specificare il valore come stringa vuota.
#### `--memory-request -mr`
La richiesta di memoria per il gruppo di server con iperscalabilità PostgreSQL abilitata per Azure Arc è un numero seguito da Ki (kilobyte), mi (megabyte) o gi (gigabyte). Per rimuovere memory_request, specificare il valore come stringa vuota.
#### `--extensions`
Elenco delimitato da virgole delle estensioni Postgres che devono essere caricate all'avvio. Per informazioni sui valori supportati, vedere la documentazione di Postgres.
#### `--port`
facoltativo.
#### `--no-wait`
Se specificato, il comando non attenderà che lo stato dell'istanza sia pronto prima della restituzione.
#### `--engine-settings -e`
Elenco delimitato da virgole delle impostazioni del motore Postgres nel formato 'key1=val1, key2=val2'. Le impostazioni specificate verranno unite con le impostazioni esistenti. Per rimuovere un'impostazione, specificare un valore vuoto, ad esempio 'removedKey='. Se si modifica un'impostazione del motore che richiede un riavvio, il servizio verrà riavviato per applicare immediatamente le impostazioni.
#### `--replace-engine-settings -re`
Quando viene specificato con --engine-settings, verranno sostituite tutte le impostazioni del motore personalizzate esistenti con il nuovo set di impostazioni e valori.
#### `--admin-password`
Se specificato, la password dell'amministratore del gruppo di server di scalabilità di PostgreSQL abilitata per Azure Arc verrà impostata sul valore della variabile di ambiente AZDATA_PASSWORD se presente e in caso contrario.
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
Eliminare un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server delete --name -n 
                                  []  
                                  
[--force -f]
```
### <a name="examples"></a>Esempio
Eliminare un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server delete -n pg1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
### <a name="optional-parameters"></a>Parametri facoltativi
#### <a name=""></a>``
--engine-version può essere usato in associazione a --name per identificare un gruppo di server PostgreSQL Hyperscale quando due gruppi di server con versione del motore diversa hanno lo stesso nome. --engine-version è facoltativo e se usato per identificare un gruppo di server deve essere 11 o 12.
#### `--force -f`
Forzare l'eliminazione del gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc senza conferma.
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
Mostra i dettagli di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server show --name -n 
                                []  
                                
[--path -p]
```
### <a name="examples"></a>Esempio
Mostra i dettagli di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres server show -n pg1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--name -n`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
### <a name="optional-parameters"></a>Parametri facoltativi
#### <a name=""></a>``
--engine-version può essere usato in associazione a --name per identificare un gruppo di server PostgreSQL Hyperscale quando due gruppi di server con versione del motore diversa hanno lo stesso nome. --engine-version è facoltativo e se usato per identificare un gruppo di server deve essere 11 o 12.
#### `--path -p`
Percorso in cui deve essere scritta la specifica completa per il gruppo di server di iperscalabilità PostgreSQL abilitato per Azure Arc. Se omesso, la specifica verrà scritta nell'output standard.
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
Elenca i gruppi di server con iperscalabilità di PostgreSQL abilitati per Azure Arc.
```bash
azdata arc postgres server list 
```
### <a name="examples"></a>Esempio
Elenca i gruppi di server con iperscalabilità di PostgreSQL abilitati per Azure Arc.
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

