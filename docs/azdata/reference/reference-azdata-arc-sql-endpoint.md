---
title: riferimento all'endpoint SQL di azdata Arc
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi dell'endpoint SQL di azdata Arc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 13ff758f200bd675bc21c020b9c957fb286ecfd6
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556840"
---
# <a name="azdata-arc-sql-endpoint"></a>endpoint SQL Arc azdata

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[elenco di endpoint SQL di azdata Arc](#azdata-arc-sql-endpoint-list) | Elencare gli endpoint SQL.
## <a name="azdata-arc-sql-endpoint-list"></a>elenco di endpoint SQL di azdata Arc
Elencare gli endpoint SQL.
```bash
azdata arc sql endpoint list [--name -n] 
                             
```
### <a name="examples"></a>Esempio
Elenca gli endpoint per un'istanza gestita di SQL.
```bash
azdata arc sql endpoint list -n sqlmi1
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--name -n`
Nome dell'istanza di SQL da visualizzare. Se omesso, verranno visualizzati tutti gli endpoint per tutte le istanze.
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

