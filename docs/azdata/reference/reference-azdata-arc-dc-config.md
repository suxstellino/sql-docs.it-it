---
title: Riferimento ad azdata arc dc config
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata arc dc config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 88bafba7df6756174f9eb33d6b22414a02a54521
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049066"
---
# <a name="azdata-arc-dc-config"></a>azdata arc dc config

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata arc dc config init](#azdata-arc-dc-config-init) | Inizializza un profilo di configurazione del controller dati che è possibile usare per la creazione del controllo.
[azdata arc dc config list](#azdata-arc-dc-config-list) | Elenca i profili di configurazione disponibili.
[azdata arc dc config add](#azdata-arc-dc-config-add) | Aggiunge un valore per un percorso JSON in un file di configurazione.
[azdata arc dc config remove](#azdata-arc-dc-config-remove) | Rimuove un valore per un percorso JSON in un file di configurazione.
[azdata arc dc config replace](#azdata-arc-dc-config-replace) | Sostituisce un valore per un percorso JSON in un file di configurazione.
[azdata arc dc config patch](#azdata-arc-dc-config-patch) | Applica una patch a un file di configurazione in base a un file di patch JSON.
## <a name="azdata-arc-dc-config-init"></a>azdata arc dc config init
Inizializza un profilo di configurazione del controller dati che è possibile usare per la creazione del controllo. È possibile indicare l'origine specifica del profilo di configurazione negli argomenti.
```bash
azdata arc dc config init [--path -p] 
                          [--source -s]  
                          
[--force -f]
```
### <a name="examples"></a>Esempi
Esperienza guidata di inizializzazione della configurazione del controller dati: si riceveranno le richieste per i valori necessari.
```bash
azdata arc dc config init
```
arc dc config init con argomenti, crea un profilo di configurazione di aks-dev-test in ./custom.
```bash
azdata arc dc config init --source aks-dev-test --path custom
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path -p`
Percorso del file in cui si vuole inserire il profilo di configurazione. Il valore predefinito è <cwd>/custom.
#### `--source -s`
Origine del profilo di configurazione: ['azure-arc-aks-premium-storage', 'azure-arc-ake', 'azure-arc-openshift', 'azure-arc-gke', 'azure-arc-aks-default-storage', 'azure-arc-aks-dev-test', 'azure-arc-kubeadm', 'azure-arc-kubeadm-dev-test', 'azure-arc-eks', 'azure-arc-azure-openshift', 'azure-arc-aks-hci']
#### `--force -f`
Forza la sovrascrittura del file di destinazione.
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
## <a name="azdata-arc-dc-config-list"></a>azdata arc dc config list
Elenca i profili di configurazione disponibili da usare in `arc dc config init`
```bash
azdata arc dc config list [--config-profile -c] 
                          
```
### <a name="examples"></a>Esempi
Mostrare tutti i nomi dei profili di configurazione disponibili.
```bash
azdata arc dc config list
```
Mostrare il codice JSON di un profilo di configurazione specifico.
```bash
azdata arc dc config list --config-profile aks-dev-test
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--config-profile -c`
Profilo di configurazione predefinito: ['azure-arc-aks-premium-storage', 'azure-arc-ake', 'azure-arc-openshift', 'azure-arc-gke', 'azure-arc-aks-default-storage', 'azure-arc-aks-dev-test', 'azure-arc-kubeadm', 'azure-arc-kubeadm-dev-test', 'azure-arc-eks', 'azure-arc-azure-openshift', 'azure-arc-aks-hci']
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
## <a name="azdata-arc-dc-config-add"></a>azdata arc dc config add
Aggiunge il valore nel percorso JSON nel file di configurazione.  Tutti gli esempi seguenti si riferiscono alla shell Bash.  Se si usa un'altra riga di comando, potrebbe essere necessario usare le notazioni di escape in modo appropriato.  In alternativa, è possibile usare la funzionalità del file di patch.
```bash
azdata arc dc config add --path -p 
                         --json-values -j
```
### <a name="examples"></a>Esempi
Es. 1 - Aggiungere l'archiviazione del controller dati.
```bash
azdata arc dc config add --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso del file di configurazione del controller dati per la configurazione che si vuole impostare, ad esempio custom/control.json.
#### `--json-values -j`
Elenco di coppie chiave/valore di percorsi JSON dei valori: key1.subkey1=value1,key2.subkey2=value2. È possibile specificare valori JSON inline, ad esempio: key='{"kind":"cluster","name":"test-cluster"}' oppure fornire un percorso di file, ad esempio key=./values.json. L'operazione di aggiunta NON supporta le istruzioni condizionali.  Se il valore inline specificato è una coppia chiave-valore con "=" e "," usare caratteri di escape per questi caratteri.  Ad esempio, ch1="ch2\=val2\,ch3\=val3". Per esempi relativi all'aspetto del percorso, vedere http://jsonpatch.com/.  Se si vuole accedere a una matrice, è necessario indicare l'indice, ad esempio key.0=value.
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
## <a name="azdata-arc-dc-config-remove"></a>azdata arc dc config remove
Rimuove il valore nel percorso JSON nel file di configurazione.  Tutti gli esempi seguenti si riferiscono alla shell Bash.  Se si usa un'altra riga di comando, potrebbe essere necessario usare le notazioni di escape in modo appropriato.  In alternativa, è possibile usare la funzionalità del file di patch.
```bash
azdata arc dc config remove --path -p 
                            --json-path -j
```
### <a name="examples"></a>Esempi
Es. 1 - Rimuovere l'archiviazione del controller dati.
```bash
azdata arc dc config remove --path custom/control.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso del file di configurazione del controller dati per la configurazione che si vuole impostare, ad esempio custom/control.json.
#### `--json-path -j`
Elenco di percorsi JSON basati sulla libreria jsonpatch che indica i valori da rimuovere, ad esempio: key1.subkey1,key2.subkey2. L'operazione di rimozione NON supporta le istruzioni condizionali. Per esempi relativi all'aspetto del percorso, vedere http://jsonpatch.com/.  Se si vuole accedere a una matrice, è necessario indicare l'indice, ad esempio key.0=value.
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
## <a name="azdata-arc-dc-config-replace"></a>azdata arc dc config replace
Sostituisce il valore nel percorso JSON nel file di configurazione.  Tutti gli esempi seguenti si riferiscono alla shell Bash.  Se si usa un'altra riga di comando, potrebbe essere necessario usare le notazioni di escape in modo appropriato.  In alternativa, è possibile usare la funzionalità del file di patch.
```bash
azdata arc dc config replace --path -p 
                             --json-values -j
```
### <a name="examples"></a>Esempi
Es. 1 - Sostituire la porta di un singolo endpoint (endpoint controller dati).
```bash
azdata arc dc config replace --path custom/control.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Es. 2 - Sostituire l'archiviazione del controller dati.
```bash
azdata arc dc config replace --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path -p`
Percorso del file di configurazione del controller dati per la configurazione che si vuole impostare, ad esempio custom/control.json.
#### `--json-values -j`
Elenco di coppie chiave/valore di percorsi JSON dei valori: key1.subkey1=value1,key2.subkey2=value2. È possibile specificare valori JSON inline, ad esempio: key='{"kind":"cluster","name":"test-cluster"}' oppure fornire un percorso di file, ad esempio key=./values.json. L'operazione di sostituzione supporta le istruzioni condizionali tramite la libreria jsonpath.  Per usarle, il percorso deve iniziare con $. In questo modo sarà possibile usare un'istruzione condizionale, ad esempio -j $.key1.key2[?(@.key3=="someValue"].key4=value. Se il valore inline specificato è una coppia chiave-valore con "=" e "," usare caratteri di escape per questi caratteri.  Ad esempio, ch1="ch2\=val2\,ch3\=val3". Vedere gli esempi seguenti. Per altre informazioni, vedere https://jsonpath.com/
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
## <a name="azdata-arc-dc-config-patch"></a>azdata arc dc config patch
Applica una patch al file di configurazione in base al file di patch specificato. Vedere http://jsonpatch.com/ per comprendere meglio come devono essere composti i percorsi. L'operazione di sostituzione consente di usare istruzioni condizionali nel percorso grazie alla libreria jsonpath https://jsonpath.com/. Tutti i file JSON di patch devono iniziare con una chiave "patch" che abbia una matrice di patch con le relative informazioni per operazione (add, replace, remove), percorso e valore. L'operazione "remove" non richiede un valore, ma solo un percorso. Vedere gli esempi seguenti.
```bash
azdata arc dc config patch --path 
                           --patch-file -p
```
### <a name="examples"></a>Esempi
Es. 1 - Sostituire la porta di un singolo endpoint (endpoint controller) con il file di patch.
```bash
azdata arc dc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Es. 2 - Sostituire l'archiviazione del controller dati con un file di patch.
```bash
azdata arc dc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
### <a name="required-parameters"></a>Parametri necessari
#### `--path`
Percorso del file di configurazione del controller dati per la configurazione che si vuole impostare, ad esempio custom/control.json.
#### `--patch-file -p`
Percorso di un file JSON di patch basato sulla libreria jsonpatch: http://jsonpatch.com/. Il file JSON di patch deve iniziare con una chiave denominata "patch", il cui valore è una matrice di operazioni di patch da eseguire. Per il percorso di un'operazione di patch, è possibile usare la notazione con punto, ad esempio key1.key2, per la maggior parte delle operazioni. Se si vuole eseguire un'operazione di sostituzione e si sta sostituendo un valore in una matrice che richiede un'istruzione condizionale, usare la notazione jsonpath facendo iniziare il percorso con $. In questo modo sarà possibile usare un'istruzione condizionale, ad esempio $.key1.key2[?(@.key3=="someValue"].key4. Vedere gli esempi seguenti. Per altre informazioni sulle istruzioni condizionali, vedere https://jsonpath.com/.
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

