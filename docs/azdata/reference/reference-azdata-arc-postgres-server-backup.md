---
title: Riferimento ad azdata arc postgres server backup
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc postgres server backup.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f07009577054d4eaa6336a7268d5ca6b0ccc085
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556914"
---
# <a name="azdata-arc-postgres-server-backup"></a>azdata arc postgres server backup

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc postgres server backup create](#azdata-arc-postgres-server-backup-create) | Creare un backup di un gruppo di server PostgreSQL.
[azdata arc postgres server backup delete](#azdata-arc-postgres-server-backup-delete) | Eliminare un backup di un gruppo di server PostgreSQL.
[azdata arc postgres server backup restore](#azdata-arc-postgres-server-backup-restore) | Ripristinare un backup di un gruppo di server PostgreSQL.
[azdata arc postgres server backup restorestatus](#azdata-arc-postgres-server-backup-restorestatus) | Ottiene lo stato dell'operazione di ripristino più recente, se disponibile.
[azdata arc postgres server backup show](#azdata-arc-postgres-server-backup-show) | Visualizzare i dettagli di un backup di un gruppo di server PostgreSQL.
## <a name="azdata-arc-postgres-server-backup-create"></a>azdata arc postgres server backup create
Creare un backup di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server backup create 
```
### <a name="examples"></a>Esempi
Crea un backup per 'pg' di servizio.
```bash
azdata arc postgres server backup create -sn pg
```
Crea un backup con nome per 'pg' di servizio.
```bash
azdata arc postgres server backup create -sn pg -n backup1
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
## <a name="azdata-arc-postgres-server-backup-delete"></a>azdata arc postgres server backup delete
Eliminare un backup di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server backup delete 
```
### <a name="examples"></a>Esempi
Eliminare un backup di un gruppo di server PostgreSQL.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
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
## <a name="azdata-arc-postgres-server-backup-restore"></a>azdata arc postgres server backup restore
Ripristinare un backup di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server backup restore 
```
### <a name="examples"></a>Esempi
Ripristinare il backup più recente.
```bash
azdata arc postgres server restore -sn pg```
Restore a backup by ID
```bash
azdata arc postgres server restore -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-restorestatus"></a>azdata arc postgres server backup restorestatus
Ottiene lo stato dell'operazione di ripristino più recente, se disponibile.
```bash
azdata arc postgres server backup restorestatus 
```
### <a name="examples"></a>Esempi
Ottenere lo stato di ripristino recente per 'pg' di servizio in base all'ID.
```bash
azdata arc postgres server backup restorestatus -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-show"></a>azdata arc postgres server backup show
Visualizzare i dettagli di un backup di un gruppo di server PostgreSQL.
```bash
azdata arc postgres server backup show 
```
### <a name="examples"></a>Esempi
Ottiene un backup per 'pg' di servizio in base all'ID.
```bash
azdata arc postgres server backup show -sn pg -id 123e4567e89b12d3a456426655440000
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


