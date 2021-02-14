---
title: Riferimento ad azdata arc dc endpoint
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc dc endpoint.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0e0fadcace976f55ea2e22fd1bd2c1c957f5c238
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049026"
---
# <a name="azdata-arc-dc-endpoint"></a>azdata arc dc endpoint

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc dc endpoint list](#azdata-arc-dc-endpoint-list) | Visualizzare l'endpoint del controller dati.
## <a name="azdata-arc-dc-endpoint-list"></a>azdata arc dc endpoint list
Visualizzare l'endpoint del controller dati.
```bash
azdata arc dc endpoint list [--endpoint-name -e] 
                            
```
### <a name="examples"></a>Esempi
Visualizza l'endpoint del controller dati in un determinato spazio dei nomi.
```bash
azdata arc dc endpoint list --namespace <ns>
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--endpoint-name -e`
Nome dell'endpoint del controller dati di Arc.
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

