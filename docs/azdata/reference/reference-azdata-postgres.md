---
title: Riferimento ad azdata postgres
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata postgres.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6403db5f9b716684c3a791ade183a8f9c5edad47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052272"
---
# <a name="azdata-postgres"></a>azdata postgres

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata postgres shell](#azdata-postgres-shell) | Interfaccia della shell della riga di comando per Postgres. Vedere https://www.pgcli.com/
[azdata postgres query](#azdata-postgres-query) | Il comando query consente l'esecuzione di comandi PostgreSQL in una sessione di database.
## <a name="azdata-postgres-shell"></a>azdata postgres shell
Interfaccia della shell della riga di comando per Postgres. Vedere https://www.pgcli.com/
```bash
azdata postgres shell [--dbname -d] 
                      [--host]  
                      
[--port -p]  
                      
[--password -w]  
                      
[--no-password]  
                      
[--single-connection]  
                      
[--username -u]  
                      
[--pgclirc]  
                      
[--dsn]  
                      
[--list-dsn]  
                      
[--row-limit]  
                      
[--less-chatty]  
                      
[--prompt]  
                      
[--prompt-dsn]  
                      
[--list -l]  
                      
[--auto-vertical-output]  
                      
[--warn]  
                      
[--no-warn]
```
### <a name="examples"></a>Esempi
Esempio di riga di comando per avviare l'esperienza interattiva.
```bash
azdata postgres shell
```
Riga di comando di esempio con un database e un utente specificati
```bash
azdata postgres shell --dbname <database> --username <username> --host <host>
```
Esempio di riga di comando per iniziare a usare una stringa di connessione completa.
```bash
azdata postgres shell --dbname postgres://user:passw0rd@example.com:5432/master 
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--dbname -d`
Nome del database a cui connettersi.
#### `--host`
Indirizzo host del database Postgres.
#### `--port -p`
Numero di porta su cui è in ascolto l'istanza di Postgres.
#### `--password -w`
Forzare la richiesta della password.
#### `--no-password`
Non richiedere mai una password.
#### `--single-connection`
Non usare una connessione separata per i completamenti.
#### `--username -u`
Nome utente per la connessione al database Postgres.
#### `--pgclirc`
Percorso del file pgclirc.
#### `--dsn`
Usare un DSN configurato nella sezione [alias_dsn] del file pgclirc.
#### `--list-dsn`
Elenco dei DSN configurati nella sezione [alias_dsn] del file pgclirc.
#### `--row-limit`
Imposta la soglia per la richiesta di limite di righe. Usare 0 per disabilitare la richiesta.
#### `--less-chatty`
Ignorare il messaggio introduttivo all'avvio e il messaggio conclusivo alla chiusura.
#### `--prompt`
Formato della richiesta (impostazione predefinita: "\u@\h:\d> ").
#### `--prompt-dsn`
Formato della richiesta per le connessioni con alias DSN (impostazione predefinita: "\u@\h:\d> ").
#### `--list -l`
Elencare i database disponibili e quindi disconnettersi.
#### `--auto-vertical-output`
Passare automaticamente alla modalità di output verticale se il risultato ha una larghezza maggiore della larghezza del terminale.
#### `--warn`
Avvisare prima di eseguire una query distruttiva.
#### `--no-warn`
Avvisare prima di eseguire una query distruttiva.
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
## <a name="azdata-postgres-query"></a>azdata postgres query
Il comando query consente l'esecuzione di comandi PostgreSQL in una sessione di database.
```bash
azdata postgres query --q -q 
                      [--host]  
                      
[--dbname -d]  
                      
[--port -p]  
                      
[--username -u]
```
### <a name="examples"></a>Esempi
Elencare tutte le tabelle in information_schema.
```bash
azdata postgres query --host <host> --username <username> -q "SELECT * FROM information_schema.tables"
```
### <a name="required-parameters"></a>Parametri necessari
#### `--q -q`
Query PostgreSQL da eseguire.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--host`
Indirizzo host del database Postgres.
`localhost`
#### `--dbname -d`
Database in cui eseguire la query.
#### `--port -p`
Numero di porta su cui è in ascolto l'istanza di Postgres.
`5432`
#### `--username -u`
Nome utente per la connessione al database Postgres.
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

