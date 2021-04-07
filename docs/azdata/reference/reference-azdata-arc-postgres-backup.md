---
title: Riferimento ad azdata arc postgres backup
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc postgres backup.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e303d6549898084c791d7dd203533d9bd18b50cd
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556954"
---
# <a name="azdata-arc-postgres-backup"></a>azdata arc postgres backup

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[Creazione backup Postgres azdata Arc](#azdata-arc-postgres-backup-create) | Creare una copia di backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
[eliminazione backup Postgres azdata Arc](#azdata-arc-postgres-backup-delete) | Eliminare un backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
[elenco di backup Postgres azdata Arc](#azdata-arc-postgres-backup-list) | Elenca i backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
[ripristino del backup Postgres azdata Arc](#azdata-arc-postgres-backup-restore) | Ripristinare un backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
## <a name="azdata-arc-postgres-backup-create"></a>Creazione backup Postgres azdata Arc
Creare una copia di backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres backup create --server-name -sn 
                                  [--name -n]  
                                  
[--no-wait]
```
### <a name="examples"></a>Esempi
Crea un backup per 'pg' di servizio.
```bash
azdata arc postgres backup create -sn pg
```
Crea un backup con nome per 'pg' di servizio.
```bash
azdata arc postgres backup create -sn pg -n backup1
```
### <a name="required-parameters"></a>Parametri necessari
#### `--server-name -sn`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--name -n`
Nome del backup. Questo parametro è facoltativo.
#### `--no-wait`
Se specificato, il comando non attende che il backup venga completato prima di restituire.
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
## <a name="azdata-arc-postgres-backup-delete"></a>eliminazione backup Postgres azdata Arc
Eliminare un backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres backup delete --server-name -sn 
                                  [--name -n]  
                                  
[-id]
```
### <a name="examples"></a>Esempio
Eliminare un backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
```
### <a name="required-parameters"></a>Parametri necessari
#### `--server-name -sn`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--name -n`
Nome del backup. Questo parametro si escludono a vicenda con-ID.
#### `-id`
ID del backup da eliminare. Questo parametro si escludono a vicenda con--Name.
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
## <a name="azdata-arc-postgres-backup-list"></a>elenco di backup Postgres azdata Arc
Elenca i backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres backup list --server-name -sn 
                                
```
### <a name="examples"></a>Esempio
Elenca i backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres backup list -sn pg
```
### <a name="required-parameters"></a>Parametri necessari
#### `--server-name -sn`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
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
## <a name="azdata-arc-postgres-backup-restore"></a>ripristino del backup Postgres azdata Arc
Ripristinare un backup di un gruppo di server con iperscalabilità PostgreSQL abilitato per Azure Arc.
```bash
azdata arc postgres backup restore --server-name -sn 
                                   [--backup-id -id]  
                                   
[--source-server-name -ssn]  
                                   
[--time -t]
```
### <a name="examples"></a>Esempio
Ripristinare un backup in base all'ID
```bash
azdata arc postgres backup restore -sn pg -id 123e4567e89b12d3a456426655440000
```
Ripristinare un backup in base all'ora (ripristino temporizzato)
```bash
azdata arc postgres backup restore -sn pg-dst -ssn pg-src --time "2020-11-18 17:25:34Z"
```
Ripristinare un backup in base all'intervallo di tempo (ripristino temporizzato)
```bash
azdata arc postgres backup restore -sn pg-dst -ssn pg-src --time 1d
```
### <a name="required-parameters"></a>Parametri necessari
#### `--server-name -sn`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--backup-id -id`
ID del backup. Se non specificato, verrà ripristinato il backup più recente effettuato.
#### `--source-server-name -ssn`
Nome del gruppo di server di origine con iperscalabilità di Azure Arc abilitato. Se non viene specificato, il backup verrà ripristinato nel gruppo di server identificato da--Server-Name.
#### `--time -t`
Punto nel tempo in cui eseguire il ripristino, dato come timestamp o numero e suffisso (m per minutes, h per hours, d per Days e w per weeks). ad esempio 1,5 h Torna indietro 90 minuti. Se specificato, è necessario assegnare--Source-Server-Name per ripristinare il backup da un gruppo di server con scalabilità PostgreSQL abilitata per Azure Arc separato.
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

