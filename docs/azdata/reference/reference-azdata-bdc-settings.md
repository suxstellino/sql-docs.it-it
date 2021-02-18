---
title: informazioni di riferimento sulle impostazioni BDC azdata
titleSuffix: SQL Server big data clusters
description: Articolo di riferimento per i comandi delle impostazioni di azdata BDC.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 37688a962fc17679a1a642af1b83609d2b8f480a
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342494"
---
# <a name="azdata-bdc-settings"></a>impostazioni di integrazione applicativa dei dati azdata

Si applica al [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L'articolo seguente fornisce informazioni di riferimento sui comandi **sql** dello strumento **azdata**. Per altre informazioni su altri comandi **azdata**, vedere [Informazioni di riferimento su azdata](reference-azdata.md).

## <a name="commands"></a>Comandi
|Comando|Descrizione|
| --- | --- |
[set di impostazioni BDC azdata](#azdata-bdc-settings-set) | Impostare le impostazioni dell'ambito del cluster.
[azdata le impostazioni di integrazione applicativa dei dati](#azdata-bdc-settings-apply) | Applicare le modifiche alle impostazioni in sospeso per l'integrazione applicativa dei dati.
[Annulla-Applica impostazioni BDC azdata](#azdata-bdc-settings-cancel-apply) | Annulla le impostazioni di integrazione applicativa dei dati.
[Mostra impostazioni di integrazione applicativa dei dati azdata](#azdata-bdc-settings-show) | Mostra le impostazioni dell'ambito del cluster o tutte le impostazioni del cluster usando--ricorsivo.
[ripristino delle impostazioni BDC azdata](#azdata-bdc-settings-revert) | Ripristina le modifiche delle impostazioni in sospeso nel Catalogo dati business in tutti gli ambiti.
## <a name="azdata-bdc-settings-set"></a>set di impostazioni BDC azdata
Impostare un'impostazione dell'ambito del cluster. Specificare il nome completo dell'impostazione e il valore. Eseguire Apply per applicare l'impostazione e aggiornare le impostazioni di integrazione applicativa dei dati.
```bash
azdata bdc settings set --settings -s 
                        
```
### <a name="examples"></a>Esempio
Impostare il valore predefinito del cluster per "BDC. telemetria. customerFeedback".
```bash
azdata bdc settings set --settings bdc.telemetry.customerFeedback=false
```
### <a name="required-parameters"></a>Parametri necessari
#### `--settings -s`
Imposta il valore configurato per le impostazioni fornite. È possibile impostare più impostazioni utilizzando un elenco delimitato da virgole.
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
## <a name="azdata-bdc-settings-apply"></a>azdata le impostazioni di integrazione applicativa dei dati
Applicare le modifiche alle impostazioni in sospeso per l'integrazione applicativa dei dati.
```bash
azdata bdc settings apply [--force -f] 
                          
```
### <a name="examples"></a>Esempio
Applicare le modifiche alle impostazioni in sospeso per l'integrazione applicativa dei dati.
```bash
azdata bdc settings apply
```
Force Apply, all'utente non verrà richiesta alcuna conferma e tutti i problemi verranno stampati come parte di stderr.
```bash
azdata bdc settings apply --force
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--force -f`
Force Apply, all'utente non verrà richiesta alcuna conferma e tutti i problemi verranno stampati come parte di stderr.
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
## <a name="azdata-bdc-settings-cancel-apply"></a>Annulla-Applica impostazioni BDC azdata
In caso di errore durante l'applicazione delle impostazioni, restituire il cluster di Big data allo stato dell'ultima esecuzione. Questo comando sarà un no-op se applicato a un cluster in esecuzione. Le modifiche apportate alle impostazioni in sospeso saranno ancora in sospeso dopo l'annullamento.
```bash
azdata bdc settings cancel-apply [--force -f] 
                                 
```
### <a name="examples"></a>Esempio
Annulla le impostazioni di integrazione applicativa dei dati.
```bash
azdata bdc settings cancel-apply
```
Force Cancel-Apply, l'utente non verrà richiesto di confermare e tutti i problemi verranno stampati come parte di stderr.
```bash
azdata bdc settings cancel-apply --force
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--force -f`
Force Cancel-Apply, l'utente non verrà richiesto di confermare e tutti i problemi verranno stampati come parte di stderr.
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
## <a name="azdata-bdc-settings-show"></a>Mostra impostazioni di integrazione applicativa dei dati azdata
Mostra le impostazioni a livello di cluster del catalogo dati business. Per impostazione predefinita, questo comando Mostra le impostazioni dell'ambito del cluster configurate dall'utente. Sono disponibili altri filtri per mostrare tutte le impostazioni (gestite dal sistema & configurabili dall'utente & ereditate), tutte le impostazioni configurabili o le impostazioni in sospeso. Per visualizzare un'impostazione specifica dell'ambito del cluster, è possibile specificare il nome dell'impostazione. Se si desidera visualizzare le impostazioni in tutti gli ambiti (cluster, servizio e risorsa), è possibile specificare "ricorsivo".
```bash
azdata bdc settings show [--settings -s] 
                         [--filter-option -f]  
                         
[--recursive -rec]  
                         
[--include-details -i]  
                         
[--description -d]
```
### <a name="examples"></a>Esempio
Indica se la raccolta di dati di telemetria BDC è abilitata.
```bash
azdata bdc settings show --settings bdc.telemetry.customerFeedback
```
Mostra le impostazioni configurate dall'utente nel Catalogo dati business in tutti gli ambiti.
```bash
azdata bdc settings show --recursive
```
Mostra tutte le impostazioni in sospeso nell'integrazione applicativa dei dati in tutti gli ambiti.
```bash
azdata bdc settings show –filter-option=pending --recursive
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--settings -s`
Visualizza le informazioni per i nomi delle impostazioni specificati.
#### `--filter-option -f`
Filtrare le impostazioni dell'ambito del cluster visualizzate, anziché solo le impostazioni "utente configurato". Sono disponibili filtri per mostrare tutte le impostazioni (configurabili dall'utente & gestite dal sistema), tutte le impostazioni configurabili o le impostazioni in sospeso.
`userConfigured`
#### `--recursive -rec`
Mostra le informazioni sull'impostazione per l'ambito del cluster e per tutti i componenti con ambito inferiore (servizi, risorse).
#### `--include-details -i`
Include dettagli aggiuntivi per le impostazioni selezionate per essere visualizzate.
#### `--description -d`
Include una descrizione dell'impostazione.
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
## <a name="azdata-bdc-settings-revert"></a>ripristino delle impostazioni BDC azdata
Ripristina le modifiche delle impostazioni in sospeso nel Catalogo dati business in tutti gli ambiti.
```bash
azdata bdc settings revert [--force -f] 
                           
```
### <a name="examples"></a>Esempio
Ripristinare le modifiche apportate alle impostazioni di integrazione applicative in sospeso in tutti gli ambiti.
```bash
azdata bdc settings revert
```
Forza il ripristino, all'utente non verrà richiesta alcuna conferma e tutti i problemi verranno stampati come parte di stderr.
```bash
azdata bdc settings revert --force
```
### <a name="optional-parameters"></a>Parametri facoltativi
#### `--force -f`
Forza il ripristino, all'utente non verrà richiesta alcuna conferma e tutti i problemi verranno stampati come parte di stderr.
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
