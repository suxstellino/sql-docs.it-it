---
title: Informazioni di riferimento su azdata app
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata app.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5c6dfa43eb652c94e820a773c5604058f1346d6a
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557034"
---
# <a name="azdata-app"></a>azdata app

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata app template](reference-azdata-app-template.md) | Modelli.
[azdata app init](#azdata-app-init) | Avvia un nuovo scheletro dell'applicazione.
[azdata app create](#azdata-app-create) | Crea un'applicazione.
[azdata app update](#azdata-app-update) | Aggiorna un'applicazione.
[azdata app list](#azdata-app-list) | Elenca le applicazioni.
[azdata app delete](#azdata-app-delete) | Elimina un'applicazione.
[azdata app run](#azdata-app-run) | Esegue un'applicazione.
[azdata app describe](#azdata-app-describe) | Descrive un'applicazione.
## <a name="azdata-app-init"></a>azdata app init
Consente di avviare lo scheletro di una nuova applicazione e/o file specifici basati su ambienti di runtime.
```bash
azdata app init [--spec -s] 
                [--name -n]  
                
[--version -v]  
                
[--template -t]  
                
[--destination -d]  
                
[--url -u]
```
### <a name="examples"></a>Esempi
Eseguire lo scaffolding solo di una nuova applicazione `spec.yaml`.
```bash
azdata app init --spec
```
Eseguire lo scaffolding dello scheletro di una nuova applicazione R in base al modello `r`.
```bash
azdata app init --name reduce --template r
```
Eseguire lo scaffolding dello scheletro di una nuova applicazione Python in base al modello `python`.
```bash
azdata app init --name reduce --template python
```
Eseguire lo scaffolding dello scheletro di una nuova applicazione SSIS in base al modello `ssis`.
```bash
azdata app init --name reduce --template ssis            
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--spec -s`
Genera solo un'applicazione spec.yaml.
#### `--name -n`
Nome dell'applicazione.
#### `--version -v`
Versione dell'applicazione.
#### `--template -t`
Nome del modello. Per un elenco completo dei nomi di modello supportati, eseguire `azdata app template list`
#### `--destination -d`
Posizione in cui inserire lo scheletro dell'applicazione. Valore predefinito: directory di lavoro corrente.
#### `--url -u`
Specificare un percorso di un repository di modelli diverso. Valore predefinito: https://github.com/Microsoft/SQLBDC-AppDeploy.git
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
## <a name="azdata-app-create"></a>azdata app create
Crea un'applicazione.
```bash
azdata app create --spec -s 
                  
```
### <a name="examples"></a>Esempi
Creare una nuova applicazione da una directory contenente specifiche di distribuzione di spec.yaml valide.
```bash
azdata app create --spec /path/to/dir/with/spec/yaml
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--spec -s`
Percorso di una directory con un file di specifiche YAML che descrive l'applicazione.
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
## <a name="azdata-app-update"></a>azdata app update
Aggiorna un'applicazione.
```bash
azdata app update [--spec -s] 
                  [--yes -y]
```
### <a name="examples"></a>Esempi
Aggiornare un'applicazione esistente da una directory contenente specifiche di distribuzione di spec.yaml valide.
```bash
azdata app update --spec /path/to/dir/with/spec/yaml    
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--spec -s`
Percorso di una directory con un file di specifiche YAML che descrive l'applicazione.
#### `--yes -y`
Non richiedere conferma quando si aggiorna un'applicazione dal file spec.yaml di CWD.
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
## <a name="azdata-app-list"></a>azdata app list
Elenca le applicazioni.
```bash
azdata app list [--name -n] 
                [--version -v]
```
### <a name="examples"></a>Esempi
Elencare le applicazioni per nome e versione.
```bash
azdata app list --name reduce  --version v1
```
Elencare tutte le versioni dell'applicazione in base al nome.
```bash
azdata app list --name reduce
```
Elencare tutte le versioni dell'applicazione in base al nome.
```bash
azdata app list
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--name -n`
Nome dell'applicazione.
#### `--version -v`
Versione dell'applicazione.
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
## <a name="azdata-app-delete"></a>azdata app delete
Elimina un'applicazione.
```bash
azdata app delete --name -n 
                  --version -v
```
### <a name="examples"></a>Esempi
Eliminare le applicazioni per nome e versione.
```bash
azdata app delete --name reduce --version v1    
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--name -n`
Nome dell'applicazione.
#### `--version -v`
Versione dell'applicazione.
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
## <a name="azdata-app-run"></a>azdata app run
Esegue un'applicazione.
```bash
azdata app run --name -n 
               --version -v  
               
[--inputs]
```
### <a name="examples"></a>Esempi
Eseguire l'applicazione senza parametri di input.
```bash
azdata app run --name reduce --version v1
```
Eseguire l'applicazione con 1 parametro di input.
```bash
azdata app run --name reduce --version v1 --inputs x=10
```
Eseguire l'applicazione con più parametri di input.
```bash
azdata app run --name reduce --version v1 --inputs x=10,y5.6    
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--name -n`
Nome dell'applicazione.
#### `--version -v`
Versione dell'applicazione.
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--inputs`
Parametri di input dell'applicazione in un formato CSV di `name=value`.
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
## <a name="azdata-app-describe"></a>azdata app describe
Descrive un'applicazione.
```bash
azdata app describe [--spec -s] 
                    [--name -n]  
                    
[--version -v]
```
### <a name="examples"></a>Esempi
Descrivere un'applicazione.
```bash
azdata app describe --name reduce --version v1    
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--spec -s`
Percorso di una directory con un file di specifiche YAML che descrive l'applicazione.
#### `--name -n`
Nome dell'applicazione.
#### `--version -v`
Versione dell'applicazione.
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

