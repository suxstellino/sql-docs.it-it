---
title: riferimento azdata
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 138fb6ae901102d5d2c27ae616dfaa2aec226ee0
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556469"
---
# <a name="azdata"></a>azdata

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi

|Comando|Descrizione|
| --- | --- |
|[azdata postgres](reference-azdata-postgres.md) | Runner di query e shell interattiva Postgres. |
[azdata login](#azdata-login) | Accedere all'endpoint controller del cluster e impostarne lo spazio dei nomi come contesto attivo. Per usare una password all'accesso, è necessario impostare la variabile di ambiente AZDATA_PASSWORD.
[azdata logout](#azdata-logout) | Consente di disconnettersi dal cluster.
|[azdata context](reference-azdata-context.md) | Comandi di gestione del contesto. |
|[azdata arc](reference-azdata-arc.md) | Comandi per l'uso di Azure Arc per i servizi dati di Azure. |
|[azdata bdc](reference-azdata-bdc.md) | Consente di selezionare, gestire e usare cluster Big Data di SQL Server. |
|[azdata notebook](reference-azdata-notebook.md) | Comandi per la visualizzazione, l'esecuzione e la gestione di notebook da un terminale. |
|[azdata app](reference-azdata-app.md) | Consente di creare, eliminare, eseguire e gestire applicazioni. |
|[azdata sql](reference-azdata-sql.md) | L'interfaccia della riga di comando dei database SQL consente agli utenti di interagire con SQL Server tramite T-SQL. |
|[azdata extension](reference-azdata-extension.md) | Consente di gestire e aggiornare le estensioni dell'interfaccia della riga di comando. |
## <a name="azdata-login"></a>azdata login
Quando si distribuisce il cluster, durante la distribuzione viene elencato l'endpoint del controller, che è necessario usare per poter accedere.  Se non si conosce l'endpoint del controller, è possibile effettuare l'accesso se nel sistema è presente la configurazione kube del cluster nel percorso predefinito <user home>/.kube/config oppure usando la variabile di ambiente KUBECONFIG, ad esempio export KUBECONFIG=path/to/.kube/config.  Quando si esegue l'accesso, lo spazio dei nomi del cluster viene impostato sul contesto attivo.
```bash
azdata login [--auth] 
             [--endpoint -e]  
             
[--accept-eula -a]  
             
[--namespace -ns]  
             
[--username -u]  
             
[--principal -p]
```
### <a name="examples"></a>Esempi
Accesso tramite autenticazione di base.
```bash
azdata login --auth basic --username johndoe --endpoint https://<ip or domain name>:30080
```
Accesso tramite Active Directory.
```bash
azdata login --auth ad --endpoint https://<ip or domain name>:30080                
```
Accesso tramite Active Directory con un'entità di sicurezza esplicita.
```bash
azdata login --auth ad --principal johndoe@COSTOSO.COM --endpoint https://<ip or domain name>:30080
```
Accedere in modo interattivo. Se non è stato specificato come argomento, verrà sempre richiesto il nome del cluster. Le variabili di ambiente AZDATA_USERNAME, AZDATA_PASSWORD e ACCEPT_EULA non verranno richieste se sono già state impostate nel sistema. Se nel sistema è presente la configurazione kube o si usa la variabile di ambiente KUBECONFIG per specificare il percorso di configurazione, l'esperienza interattiva tenterà prima di usare il file di configurazione e, se la configurazione avrà esito negativo, chiederà quindi all'utente di specificarlo.
```bash
azdata login
```
Accedere (in modo non interattivo). Eseguire l'accesso con il nome del cluster, il nome utente del controller, l'endpoint del controller e il set di accettazione EULA come argomenti. È necessario impostare la variabile di ambiente AZDATA_PASSWORD.  Se non si vuole specificare l'endpoint del controller, è necessario che nel sistema sia presente la configurazione kube nel percorso predefinito <user home>/.kube/config oppure usando la variabile di ambiente KUBECONFIG, ad esempio export KUBECONFIG=path/to/.kube/config.
```bash
azdata login --namespace ClusterName --username johndoe@contoso.com  --endpoint https://<ip or domain name>:30080 --accept-eula yes
```
Accedere con la configurazione kube presente nel computer e con i valori impostati per le variabili di ambiente AZDATA_USERNAME, AZDATA_PASSWORD e ACCEPT_EULA.
```bash
azdata login -n ClusterName
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--auth`
Strategia di autenticazione. Autenticazione di base o di Active Directory. L'impostazione predefinita corrisponde all'autenticazione di base.
#### `--endpoint -e`
Endpoint del controller del cluster "https://host:port". Se non si vuole usare questo argomento, è possibile usare la configurazione kube presente nel sistema. Assicurarsi che la configurazione si trovi nel percorso predefinito <user home>/.kube/config oppure usare la variabile di ambiente KUBECONFIG.
#### `--accept-eula -a`
Indica se accettare le condizioni di licenza: [yes/no]. Se non si vuole usare questo argomento, è possibile impostare la variabile di ambiente ACCEPT_EULA su "yes". Le condizioni di licenza di questo prodotto sono disponibili in https://aka.ms/eula-azdata-en.
#### `--namespace -ns`
Spazio dei nomi del piano di controllo del cluster.
#### `--username -u`
Utente dell'account. Se non si vuole usare questo argomento, è possibile impostare la variabile di ambiente AZDATA_USERNAME.
#### `--principal -p`
Area di autenticazione Kerberos. Nella maggior parte dei casi, l'area di autenticazione Kerberos corrisponde al nome di dominio in lettere maiuscole.
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
## <a name="azdata-logout"></a>azdata logout
Consente di disconnettersi dal cluster.
```bash
azdata logout 
```
### <a name="examples"></a>Esempi
Disconnettere l'utente.
```bash
azdata logout
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

