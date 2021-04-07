---
title: Informazioni di riferimento su azdata context
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata context.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3d7f09126ed0c3475718c8182a27b6b49b2f379b
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557064"
---
# <a name="azdata-context"></a>azdata context

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata context list](#azdata-context-list) | Elenca i contesti disponibili nel profilo utente.
[azdata context delete](#azdata-context-delete) | Elimina il contesto con lo spazio dei nomi specificato dal profilo utente.
[azdata context set](#azdata-context-set) | Imposta il contesto con lo spazio dei nomi specificato come contesto attivo nel profilo utente.
## <a name="azdata-context-list"></a>azdata context list
È possibile impostare o eliminare uno qualsiasi di questi con `azdata context set` o `azdata context delete`. Per accedere a un nuovo contesto, usare `azdata login`.
```bash
azdata context list [--active -a] 
                    
```
### <a name="examples"></a>Esempi
Elenco di tutti i contesti disponibili nel profilo utente.
```bash
azdata context list
```
Indicazione del contesto attivo nel profilo utente.
```bash
azdata context list --active
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--active -a`
Elenca solo il contesto attualmente attivo.
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
## <a name="azdata-context-delete"></a>azdata context delete
Se il contesto eliminato è attivo, l'utente dovrà impostare un nuovo contesto attivo. Per visualizzare i contesti disponibili per l'impostazione o l'eliminazione `azdata context list` . Quando viene fornito uno spazio dei nomi che corrisponde a più spazi dei nomi del contesto, è necessario specificare all [--namespace--username--endpoint] per l'univocità allo scopo di eliminare tale contesto.
```bash
azdata context delete --namespace -ns 
                      [--endpoint -e]  
                      
[--username -u]
```
### <a name="examples"></a>Esempio
Elimina il contesto nel profilo utente in base a uno spazio dei nomi univoco.
```bash
azdata context delete --namespace contextNamespace
```
Elimina il contesto nel profilo utente in base allo spazio dei nomi, al nome utente e all'endpoint del controller.
```bash
azdata context set --namespace contextNamespace --username johndoe --endpoint https://<ip or domain name>:30080
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--namespace -ns`
Spazio dei nomi del contesto che si vuole eliminare.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--endpoint -e`
Endpoint del controller del cluster "https://host:port".
#### `--username -u`
Utente dell'account.
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
## <a name="azdata-context-set"></a>azdata context set
Per visualizzare i contesti disponibili per l'impostazione, usare `azdata context list`. Se non viene elencato alcun contesto, è necessario eseguire l'accesso con `azdata login` per creare un contesto nel profilo utente. L'entità a cui si accede diventerà il contesto attivo. Se si accede a più entità, sarà possibile usare questo comando per passare da un contesto attivo a un altro. Per visualizzare il contesto attualmente attivo `azdata context list --active` . Quando viene fornito uno spazio dei nomi che corrisponde a più spazi dei nomi del contesto, è necessario specificare all [--namespace--username--endpoint] per l'univocità per poter impostare il contesto attivo.
```bash
azdata context set --namespace -ns 
                   [--endpoint -e]  
                   
[--username -u]
```
### <a name="examples"></a>Esempio
Imposta il contesto attivo nel profilo utente in base a uno spazio dei nomi univoco.
```bash
azdata context set --namespace contextNamespace
```
Imposta il contesto attivo nel profilo utente in base allo spazio dei nomi, al nome utente e all'endpoint del controller.
```bash
azdata context set --namespace contextNamespace --username johndoe --endpoint https://<ip or domain name>:30080     
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--namespace -ns`
Spazio dei nomi del contesto che si vuole impostare.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--endpoint -e`
Endpoint del controller del cluster "https://host:port".
#### `--username -u`
Utente dell'account.
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

