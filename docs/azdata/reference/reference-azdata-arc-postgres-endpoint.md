---
title: Riferimento ad azdata arc postgres endpoint
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc postgres endpoint.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: dc94dad852cf016ac9bcb47935544df2a97eebd7
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557424"
---
# <a name="azdata-arc-postgres-endpoint"></a>azdata arc postgres endpoint

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc postgres endpoint list](#azdata-arc-postgres-endpoint-list) | Elenca gli endpoint del gruppo di server con iperscalabilità PostgreSQL abilitati per Azure Arc.
## <a name="azdata-arc-postgres-endpoint-list"></a>azdata arc postgres endpoint list
Elenca gli endpoint del gruppo di server con iperscalabilità PostgreSQL abilitati per Azure Arc.
```bash
azdata arc postgres endpoint list [--name -n] 
                                  []
```
### <a name="examples"></a>Esempio
Elenca gli endpoint del gruppo di server con iperscalabilità PostgreSQL abilitati per Azure Arc.
```bash
azdata arc postgres endpoint list -n postgres01
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--name -n`
Nome del gruppo di server di scalabilità di PostgreSQL abilitato per Azure Arc.
#### <a name=""></a>``
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

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md). 

Per altre informazioni su come installare lo strumento **azdata**, vedere [Installare azdata](..\install\deploy-install-azdata.md).

