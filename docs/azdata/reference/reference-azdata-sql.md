---
title: Informazioni di riferimento su azdata sql
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata sql.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2a55bb03e6e597efe8216948b651382ff01dc50c
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557014"
---
# <a name="azdata-sql"></a>azdata sql

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata sql shell](#azdata-sql-shell) | L'interfaccia della riga di comando SQL consente agli utenti di interagire con SQL Server e Azure SQL tramite T-SQL.
[azdata sql query](#azdata-sql-query) | L'interfaccia della riga di comando SQL consente agli utenti di interagire con SQL Server e Azure SQL tramite T-SQL.
## <a name="azdata-sql-shell"></a>azdata sql shell
L'interfaccia della riga di comando SQL consente agli utenti di interagire con SQL Server e Azure SQL tramite T-SQL.
```bash
azdata sql shell [--username -u] 
                 [--database -d]  
                 
[--server -s]  
                 
[--integrated -e]  
                 
[--mssqlclirc]  
                 
[--row-limit]  
                 
[--less-chatty]  
                 
[--auto-vertical-output]  
                 
[--encrypt -n]  
                 
[--trust-server-certificate -c]  
                 
[--connect-timeout -l]  
                 
[--application-intent -k]  
                 
[--multi-subnet-failover -m]  
                 
[--packet-size]  
                 
[--dac-connection -a]  
                 
[--input-file -i]  
                 
[--output-file]  
                 
[--enable-sqltoolsservice-logging]  
                 
[--prompt]
```
### <a name="examples"></a>Esempi
Esempio di riga di comando per avviare l'esperienza interattiva.
```bash
azdata sql shell
```
Riga di comando di esempio con un server, un utente e un database specificati
```bash
azdata sql shell --server localhost --username sa --database master         
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--username -u`
Nome utente per la connessione al database.
#### `--database -d`
Nome del database a cui connettersi.
#### `--server -s`
Nome o indirizzo dell'istanza di SQL Server.
#### `--integrated -e`
Usare l'autenticazione integrata in Windows.
#### `--mssqlclirc`
Percorso del file di configurazione mssqlclirc.
#### `--row-limit`
Impostare la soglia per la richiesta di limite di righe. Usare 0 per disabilitare la richiesta.
#### `--less-chatty`
Ignorare il messaggio introduttivo all'avvio e il messaggio conclusivo alla chiusura.
#### `--auto-vertical-output`
Passare automaticamente alla modalità di output verticale se il risultato ha una larghezza maggiore di quella del terminale.
#### `--encrypt -n`
SQL Server usa la crittografia SSL per tutti i dati se nel server è installato un certificato.
#### `--trust-server-certificate -c`
Il canale verrà crittografato ignorando la catena di certificati per convalidare l'attendibilità.
#### `--connect-timeout -l`
Tempo di attesa in secondi per la connessione al server prima che venga terminata la richiesta.
#### `--application-intent -k`
Dichiara il tipo di carico di lavoro dell'applicazione durante la connessione a un database in un gruppo di disponibilità di SQL Server.
#### `--multi-subnet-failover -m`
Se l'applicazione si connette al gruppo di disponibilità AlwaysOn in subnet diverse, questa impostazione consente un rilevamento e una connessione più rapidi al server attualmente attivo.
#### `--packet-size`
Dimensioni in byte dei pacchetti di rete usati per comunicare con SQL Server
#### `--dac-connection -a`
Connettersi a SQL Server usando la connessione amministrativa dedicata.
#### `--input-file -i`
Specifica il file che contiene un batch di istruzioni SQL per l'elaborazione.
#### `--output-file`
Specifica il file che riceve l'output da una query.
#### `--enable-sqltoolsservice-logging`
Abilita la registrazione diagnostica per SqlToolsService.
#### `--prompt`
Formato della richiesta (impostazione predefinita: \d>
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
## <a name="azdata-sql-query"></a>azdata sql query
L'interfaccia della riga di comando SQL consente agli utenti di interagire con SQL Server e Azure SQL tramite T-SQL.
```bash
azdata sql query -q 
                 [--database -d]  
                 
[--username -u]  
                 
[--server -s]  
                 
[--integrated -e]
```
### <a name="examples"></a>Esempi
Esempio di riga di comando per selezionare l'elenco dei nomi di tabelle.
```bash
azdata sql query --server localhost --username sa --database master -q "SELECT name FROM SYS.TABLES"
```
### <a name="required-parameters"></a>Parametri necessari
#### `-q`
Query SQL da eseguire.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--database -d`
Nome del database a cui connettersi.
`master`
#### `--username -u`
Nome utente per la connessione al database.
#### `--server -s`
Nome o indirizzo dell'istanza di SQL Server.
#### `--integrated -e`
Usare l'autenticazione integrata in Windows.
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

