---
title: Riferimento ad azdata arc dc status
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc dc status.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 91bcda5ac4fc7cb39887698cdea19a55b47d8445
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052672"
---
# <a name="azdata-arc-dc-status"></a>azdata arc dc status

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc dc status show](#azdata-arc-dc-status-show) | Mostra lo stato del controller dati.
## <a name="azdata-arc-dc-status-show"></a>azdata arc dc status show
Mostra lo stato del controller dati.
```bash
azdata arc dc status show [--namespace -ns] 
                          
```
### <a name="examples"></a>Esempi
Visualizza lo stato del controller dati in un determinato spazio dei nomi.
```bash
azdata arc dc status show --namespace <ns>
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--namespace -ns`
Spazio dei nomi Kubernetes in cui è presente il controller dati.
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

