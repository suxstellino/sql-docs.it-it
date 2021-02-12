---
title: Informazioni di riferimento sui comandi azdata bdc config
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata bdc config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 28c9820cd147d5ce4b93fe7605a1f36187f4dfd7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048931"
---
# <a name="azdata-bdc-config"></a>azdata bdc config

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
[azdata bdc config init](#azdata-bdc-config-init) | Inizializza un profilo di configurazione del cluster Big Data che è possibile usare con bdc create.
[azdata bdc config list](#azdata-bdc-config-list) | Elenca i profili di configurazione disponibili.
[azdata bdc config show](#azdata-bdc-config-show) | Visualizza la configurazione corrente del cluster Big Data o la configurazione di un file locale specificato dall'utente, ad esempio custom/bdc.json.
[azdata bdc config add](#azdata-bdc-config-add) | Aggiunge un valore per un percorso JSON in un file di configurazione.
[azdata bdc config remove](#azdata-bdc-config-remove) | Rimuove un valore per un percorso JSON in un file di configurazione.
[azdata bdc config replace](#azdata-bdc-config-replace) | Sostituisce un valore per un percorso JSON in un file di configurazione.
[azdata bdc config patch](#azdata-bdc-config-patch) | Applica una patch a un file di configurazione in base a un file di patch JSON.
## <a name="azdata-bdc-config-init"></a>azdata bdc config init
Inizializza un profilo di configurazione del cluster Big Data che è possibile usare con bdc create. È possibile indicare l'origine specifica del profilo di configurazione negli argomenti.
```bash
azdata bdc config init [--path -p] 
                       [--source -s]  
                       
[--force -f]  
                       
[--accept-eula -a]
```
### <a name="examples"></a>Esempi
Esperienza guidata di inizializzazione della configurazione del cluster Big Data: si riceveranno le indicazioni relative ai valori necessari.
```bash
azdata bdc config init
```
Inizializzazione della configurazione del cluster Big Data con argomenti: viene creato un profilo di configurazione aks-dev-test in ./custom.
```bash
azdata bdc config init --source aks-dev-test --target custom
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--path -p`
Percorso del file in cui si vuole inserire il profilo di configurazione. Il valore predefinito è <cwd>/custom.
#### `--source -s`
Origine del profilo di configurazione: ['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--force -f`
Forza la sovrascrittura del file di destinazione.
#### `--accept-eula -a`
Indica se accettare le condizioni di licenza: [yes/no]. Se non si vuole usare questo argomento, è possibile impostare la variabile di ambiente ACCEPT_EULA su "yes". Le condizioni di licenza di questo prodotto sono disponibili in https://aka.ms/eula-azdata-en.
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
## <a name="azdata-bdc-config-list"></a>azdata bdc config list
Elenca i profili di configurazione disponibili da usare in `bdc config init`
```bash
azdata bdc config list [--config-profile -c] 
                       [--type -t]  
                       
[--accept-eula -a]
```
### <a name="examples"></a>Esempi
Mostrare tutti i nomi dei profili di configurazione disponibili.
```bash
azdata bdc config list
```
Mostrare il codice JSON di un profilo di configurazione specifico.
```bash
azdata bdc config list --config-profile aks-dev-test
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--config-profile -c`
Profilo di configurazione predefinito: ['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--type -t`
Tipo di configurazione che si vuole visualizzare.
#### `--accept-eula -a`
Indica se accettare le condizioni di licenza: [yes/no]. Se non si vuole usare questo argomento, è possibile impostare la variabile di ambiente ACCEPT_EULA su "yes". Le condizioni di licenza di questo prodotto sono disponibili in https://aka.ms/eula-azdata-en.
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
## <a name="azdata-bdc-config-show"></a>azdata bdc config show
Visualizza la configurazione corrente del cluster Big Data o la configurazione di un file locale specificato dall'utente, ad esempio custom/bdc.json. Il comando può accettare anche un percorso JSON se si vuole ottenere solo una sezione.  È anche possibile specificare un file di destinazione per l'output.  Se non si specifica un file di destinazione, l'output verrà inviato nel terminale.
```bash
azdata bdc config show [--config-file -c] 
                       [--target -t]  
                       
[--json-path -j]  
                       
[--force -f]
```
### <a name="examples"></a>Esempi
Mostrare la configurazione del cluster Big Data nella console
```bash
azdata bdc config show
```
In un file di configurazione locale ottenere un valore alla fine di un percorso di chiave JSON semplice.
```bash
azdata bdc config show --config-file custom-config/bdc.json --json-path "metadata.name" --target section.json
```
In un file di configurazione locale, ottiene le risorse all'interno di un servizio
```bash
azdata bdc config show --config-file custom-config/bdc.json --json-path "$.spec.services.sql.resources" --target section.json
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--config-file -c`
Percorso del file di configurazione del cluster Big Data se non si vuole applicare la configurazione del cluster a cui si è attualmente connessi, ad esempio custom/bdc.json.
#### `--target -t`
File di output in cui archiviare il risultato. Valore predefinito: indirizzamento a StdOut.
#### `--json-path -j`
Percorso di chiave JSON che conduce alla sezione o al valore desiderato della configurazione, ad esempio key1.key2.key3. Usa il linguaggio di query jsonpath https://jsonpath.com/, ad esempio: -j '$.spec.pools[?(@.spec.type == "Master")]..endpoints'
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
## <a name="azdata-bdc-config-add"></a>azdata bdc config add
Aggiunge il valore nel percorso JSON nel file di configurazione.  Tutti gli esempi seguenti si riferiscono alla shell Bash.  Se si usa un'altra riga di comando, potrebbe essere necessario usare le notazioni di escape in modo appropriato.  In alternativa, è possibile usare la funzionalità del file di patch.
```bash
azdata bdc config add --path -p 
                      --json-values -j
```
### <a name="examples"></a>Esempi
Esempio 1 - Aggiungere lo spazio di archiviazione del piano di controllo.
```bash
azdata bdc config add --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Percorso del file di configurazione del cluster Big Data per la configurazione che si vuole impostare, ad esempio custom/bdc.json.
#### `--json-values -j`
Elenco di coppie chiave/valore di percorsi JSON dei valori: key1.subkey1=value1,key2.subkey2=value2. È possibile specificare valori JSON inline, ad esempio: key='{"kind":"cluster","name":"test-cluster"}' oppure fornire un percorso di file, ad esempio key=./values.json. L'operazione di aggiunta NON supporta le istruzioni condizionali.  Se il valore inline specificato è esso stesso una coppia chiave/valore con caratteri '=' e ',' usare caratteri di escape per tali caratteri.  Ad esempio, ch1="ch2\=val2\,ch3\=val3". Per esempi relativi all'aspetto del percorso, vedere http://jsonpatch.com/.  Se si vuole accedere a una matrice, è necessario indicare l'indice, ad esempio key.0=value.
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
## <a name="azdata-bdc-config-remove"></a>azdata bdc config remove
Rimuove il valore nel percorso JSON nel file di configurazione.  Tutti gli esempi seguenti si riferiscono alla shell Bash.  Se si usa un'altra riga di comando, potrebbe essere necessario usare le notazioni di escape in modo appropriato.  In alternativa, è possibile usare la funzionalità del file di patch.
```bash
azdata bdc config remove --path -p 
                         --json-path -j
```
### <a name="examples"></a>Esempi
Esempio 1 - Rimuovere lo spazio di archiviazione del piano di controllo.
```bash
azdata bdc config remove --path custom/control.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Percorso del file di configurazione del cluster Big Data per la configurazione che si vuole impostare, ad esempio custom/bdc.json.
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
## <a name="azdata-bdc-config-replace"></a>azdata bdc config replace
Sostituisce il valore nel percorso JSON nel file di configurazione.  Tutti gli esempi seguenti si riferiscono alla shell Bash.  Se si usa un'altra riga di comando, potrebbe essere necessario usare le notazioni di escape in modo appropriato.  In alternativa, è possibile usare la funzionalità del file di patch.
```bash
azdata bdc config replace --path -p 
                          --json-values -j
```
### <a name="examples"></a>Esempi
Esempio 1 - Sostituire la porta di un singolo endpoint (endpoint controller).
```bash
azdata bdc config replace --path custom/control.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Esempio 2 - Sostituire lo spazio di archiviazione del piano di controllo.
```bash
azdata bdc config replace --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
Esempio 3 - Sostituire le specifiche della risorsa storage-0, incluse le repliche.
```bash
azdata bdc config replace --path custom/bdc.json --json-values "$.spec.resources.storage-0.spec={"replicas": 2,"storage": {"className": "managed-premium","size": "10Gi","accessMode": "ReadWriteOnce"},"type": "Storage"}"
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path -p`
Percorso del file di configurazione del cluster Big Data per la configurazione che si vuole impostare, ad esempio custom/bdc.json.
#### `--json-values -j`
Elenco di coppie chiave/valore di percorsi JSON dei valori: key1.subkey1=value1,key2.subkey2=value2. È possibile specificare valori JSON inline, ad esempio: key='{"kind":"cluster","name":"test-cluster"}' oppure fornire un percorso di file, ad esempio key=./values.json. L'operazione di sostituzione supporta le istruzioni condizionali tramite la libreria jsonpath.  Per usarle, il percorso deve iniziare con $. In questo modo sarà possibile usare un'istruzione condizionale come -j $.key1.key2[?(@.key3=='someValue'].key4=value. Se il valore inline specificato è esso stesso una coppia chiave/valore con caratteri '=' e ',' usare caratteri di escape per tali caratteri.  Ad esempio, ch1="ch2\=val2\,ch3\=val3". Vedere gli esempi seguenti. Per altre informazioni, vedere https://jsonpath.com/
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
## <a name="azdata-bdc-config-patch"></a>azdata bdc config patch
Applica una patch al file di configurazione in base al file di patch specificato. Vedere http://jsonpatch.com/ per comprendere meglio come devono essere composti i percorsi. L'operazione di sostituzione consente di usare istruzioni condizionali nel percorso grazie alla libreria jsonpath https://jsonpath.com/. Tutti i file JSON di patch devono iniziare con una chiave "patch" che abbia una matrice di patch con le relative informazioni per operazione (add, replace, remove), percorso e valore. L'operazione "remove" non richiede un valore, ma solo un percorso. Vedere gli esempi seguenti.
```bash
azdata bdc config patch --path 
                        --patch-file -p
```
### <a name="examples"></a>Esempi
Esempio 1 - Sostituire la porta di un singolo endpoint (endpoint controller) con il file di patch.
```bash
azdata bdc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Esempio 2 - Sostituire lo spazio di archiviazione del piano di controllo con il file di patch.
```bash
azdata bdc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
Esempio 3 - Sostituire lo spazio di archiviazione del pool, incluse le repliche (pool di archiviazione) con il file di patch.
```bash
azdata bdc config patch --path custom/bdc.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.resources.storage-0.spec","value":{"replicas": 2,"storage": {"className": "managed-premium","size": "10Gi","accessMode": "ReadWriteOnce"},"type": "Storage"}}]}
```
### <a name="required-parameters"></a>Parametri obbligatori
#### `--path`
Percorso del file di configurazione del cluster Big Data per la configurazione che si vuole impostare, ad esempio custom/bdc.json.
#### `--patch-file -p`
Percorso di un file JSON di patch basato sulla libreria jsonpatch: http://jsonpatch.com/. Il file JSON di patch deve iniziare con una chiave denominata "patch", il cui valore è una matrice di operazioni di patch da eseguire. Per il percorso di un'operazione di patch, è possibile usare la notazione con punto, ad esempio key1.key2, per la maggior parte delle operazioni. Se si vuole eseguire un'operazione di sostituzione e si sta sostituendo un valore in una matrice che richiede un'istruzione condizionale, usare la notazione jsonpath facendo iniziare il percorso con $. In questo modo sarà possibile usare un'istruzione condizionale come $.key1.key2[?(@.key3=='someValue'].key4. Vedere gli esempi seguenti. Per altre informazioni sulle istruzioni condizionali, vedere https://jsonpath.com/.
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

