---
title: riferimento all'endpoint SQL di azdata Arc
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi dell'endpoint SQL di azdata Arc.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 190bc4360d8f8cd35f0ceda04887a1fc912243e3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342532"
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
Stringa di query JMESPath. Per altre informazioni ed esempi, vedere [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Aumenta il livello di dettaglio della registrazione. Usare --debug per log di debug completi.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md). 

Per altre informazioni su come installare lo strumento **azdata**, vedere [Installare azdata](..\install\deploy-install-azdata.md).
