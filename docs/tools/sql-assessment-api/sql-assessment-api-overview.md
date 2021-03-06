---
title: API Valutazione SQL Server
description: Informazioni sull'API Valutazione SQL che offre uno strumento per valutare la configurazione di SQL Server per le procedure consigliate.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 3/5/2021
ms.openlocfilehash: 37e31834df74bf91fcf31004973c1556b8f34552
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247463"
---
# <a name="sql-assessment-api"></a>API Valutazione SQL

L'API Valutazione SQL fornisce un meccanismo per valutare la configurazione del SQL Server per le procedure consigliate. L'API viene fornita con un set di regole contenente le regole per le procedure consigliate dal team di SQL Server. Questo set di regole è stato migliorato con il rilascio di nuove versioni, ma allo stesso tempo l'API è stata creata con lo scopo di offrire una soluzione altamente personalizzabile ed estendibile. Gli utenti possono quindi ottimizzare le regole predefinite e crearne di proprie.

L'API Valutazione SQL è utile quando si desidera assicurarsi che la configurazione del SQL Server sia conforme alle procedure consigliate. Dopo una valutazione iniziale, la stabilità della configurazione può essere monitorata tramite valutazioni pianificate a intervalli regolari.

L'API può essere usata per valutare:
 
* SQL Server in Macchine virtuali di Azure

* Istanza gestita di database SQL di Azure

* SQL Server 2012 e versioni successive

* SQL in sistemi basati su Linux

L'API viene usata anche dall'estensione SQL Server assessment per Azure Data Studio (ADS).

>[!NOTE]
>L'API Valutazione SQL fornisce la valutazione su diverse aree, ma non approfondisce la sicurezza. Si consiglia di usare la [valutazione della vulnerabilità SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) per migliorare in modo proattivo la sicurezza del database.

## <a name="rules"></a>Regole

Le regole (talvolta denominate controlli) sono definite nei file in formato JSON. Il formato di RuleSet richiede la specifica di un nome e una versione di RuleSet. Quando si usano i RuleSet personalizzati, è possibile sapere facilmente quali sono le indicazioni relative al set di regole.

Il set di regole di Microsoft spedito è disponibile su GitHub. È possibile visualizzare l' [intero set di regole](https://github.com/microsoft/sql-server-samples/blob/567d49a42d4cf10e4942b19290ab80828b451b77/samples/manage/sql-assessment-api/DefaultRuleset.csv) nel [repository degli esempi](https://aka.ms/sql-assessment-api).

## <a name="sql-assessment-cmdlets-and-associated-extensions"></a>Cmdlet di Valutazione SQL ed estensioni associate

L'API Valutazione SQL fa parte di:

* [Azure Data Studio (ADS)](../../azure-data-studio/what-is-azure-data-studio.md)

    Versione di rilascio a partire dal 2020 giugno e versioni successive.

* [Oggetti SMO (SQL Server Management Objects)](../../relational-databases/server-management-objects-smo/installing-smo.md)

    Versione di rilascio a partire da luglio 2019 e versioni successive.

* [Modulo SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md)

    Versione di rilascio a partire da luglio 2019 e versioni successive.

Prima di iniziare a usare l'API Valutazione SQL, assicurarsi di:

* [Installare gli annunci](https://techcommunity.microsoft.com/t5/sql-server/released-sql-server-assessment-extension-for-azure-data-studio/ba-p/1470603)

* [Installare SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

* [Installare il modulo SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md)

Il modulo SqlServer sta ottenendo due nuovi cmdlet per lavorare con Valutazione SQL API:

* **Get-SqlAssessmentItem**: fornisce un elenco dei controlli di valutazione disponibili per un oggetto SQL Server

* **Invoke-SqlAssessment**: fornisce i risultati di una valutazione

Il Framework SMO è integrato dall'estensione dell'API Valutazione SQL che fornisce i metodi seguenti:

* **GetAssessmentItems** : restituisce i controlli disponibili per un determinato oggetto SQL (IEnumerable<... >)

* **GetAssessmentResults** : valuta in modo sincrono la valutazione e restituisce risultati ed errori se presenti (IEnumerable<... >)

* **GetAssessmentResultsList** : valuta in modo asincrono la valutazione e restituisce i risultati e gli errori, se presenti (Task<... >)

## <a name="get-started-using-sql-assessment-cmdlets"></a>Introduzione all'uso dei cmdlet di Valutazione SQL

La valutazione viene eseguita rispetto a un determinato oggetto di SQL Server. Nel set di regole predefinito sono disponibili controlli solo per due tipi di oggetti: Server e Database (oltre a questi, l'API supporta altri due tipi: Filegroup e AvailabilityGroup). Se si vuole valutare un'istanza di SQL e tutti i relativi database, è necessario eseguire separatamente i cmdlet di Valutazione SQL per ogni oggetto. In alternativa, è possibile passare gli oggetti per la valutazione ai cmdlet di Valutazione SQL in una variabile o nella pipeline.

Gli oggetti SqlServer e RegisteredServer sono equivalenti ed è quindi possibile passare entrambi ai cmdlet di Valutazione SQL.

Per iniziare, esaminare gli esempi riportati di seguito.

1. Ottenere un elenco dei controlli disponibili per un'istanza predefinita locale per acquisire familiarità con i controlli. In questo esempio viene inviato tramite pipe l'output del cmdlet Get-SqlInstance al cmdlet Get-SqlAssessmentItem per passare l'oggetto istanza.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

2. Ottenere un elenco dei controlli disponibili per tutti i database dell'istanza. In questo esempio si usano il cmdlet Get-Item e un percorso implementato con il provider di SQL Server Windows PowerShell per ottenere un elenco dei database e quindi inviarlo tramite pipe al cmdlet Get-SqlDatabase.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    È anche possibile usare il cmdlet Get-SqlDatabase per eseguire la stessa operazione.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

3. Ottenere un elenco dei controlli disponibili per tutti i database dell'istanza. In questo esempio si usano il cmdlet Get-Item e un percorso implementato con il provider di SQL Server Windows PowerShell per ottenere un elenco dei database e quindi inviarlo tramite pipe al cmdlet Get-SqlDatabase.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    È anche possibile usare il cmdlet Get-SqlDatabase per eseguire la stessa operazione.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

4. Richiamare la valutazione per l'istanza e salvare i risultati in una tabella SQL. In questo esempio viene inviato tramite pipe l'output del cmdlet Get-SqlInstance al cmdlet Invoke-SqlAssessment, i cui risultati vengono inviati tramite pipe al cmdlet Write-SqlTableData. Il cmdlet Invoke-Assessment viene eseguito con il parametro `-FlattenOutput` in questo esempio. Questo parametro rende l'output adatto per il cmdlet Write-SqlTableData. Il secondo genera un errore se si omette il parametro.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

    Verrà ora richiamata una valutazione per tutti i database dell'istanza e si aggiungeranno i risultati alla stessa tabella.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

5. Per comprendere meglio le raccomandazioni, seguire le descrizioni e i collegamenti nella tabella.

6. Personalizzare le regole in base all'ambiente e ai requisiti dell'organizzazione (vedere di seguito).

7. Pianificare un'attività o un processo per eseguire la valutazione a intervalli regolari o su richiesta per misurare lo stato di avanzamento.

## <a name="customizing-rules"></a>Personalizzazione delle regole

Le regole sono progettate per essere personalizzabili ed estendibili. Il set di regole Microsoft è progettato per essere usato nella maggior parte degli ambienti. Tuttavia, non è possibile avere un unico set di regole applicabile a ogni singolo ambiente. Gli utenti possono scrivere i propri file JSON e personalizzare le regole esistenti o aggiungerne di nuove. Alcuni esempi di personalizzazione e di set di regole completi rilasciati da Microsoft sono disponibili nel [repository di esempi](https://aka.ms/sql-assessment-api). Per altri dettagli su come eseguire i cmdlet di Valutazione SQL con file JSON personalizzati, usare il cmdlet Get-Help.

### <a name="options-available-with-rule-customization-feature"></a>Opzioni disponibili con la funzionalità di personalizzazione delle regole

#### <a name="enablingdisabling-certain-rules-or-groups-of-rules-using-tags"></a>Abilitazione/disabilitazione di determinate regole o gruppi di regole (tramite tag)

È possibile silenziare regole specifiche quando non sono applicate all'ambiente in uso o finché non vengono eseguite le attività pianificate per risolvere il problema.

#### <a name="changing-threshold-parameters"></a>Modifica dei parametri di soglia

Le regole specifiche hanno soglie che vengono confrontate rispetto al valore corrente di una metrica per individuare un problema. Se le soglie predefinite non sono adatte, è possibile modificarle.

#### <a name="adding-more-rules-written-by-you-or-third-parties"></a>Aggiunta di altre regole scritte dall'utente o da terze parti

È possibile raggruppare i set di regole aggiungendo uno o più file JSON come parametri alla chiamata API Valutazione SQL. L'organizzazione può scrivere tali file autonomamente o ottenerli da terze parti. Ad esempio, è possibile disporre di un file JSON che disabilita regole specifiche del set di regole Microsoft e di un altro file JSON di un esperto del settore che include regole utili per l'ambiente in uso, seguito da un altro file JSON che modifica alcuni valori di soglia di quello specifico file JSON.

>[!IMPORTANT]
>Si consiglia di non usare set di regole provenienti da origini non attendibili finché non vengono esaminati accuratamente per accertarsi che siano sicuri.

## <a name="next-steps"></a>Passaggi successivi

* [Oggetti SMO (SQL Server Management Objects)](../../relational-databases/server-management-objects-smo/overview-smo.md)
* [PowerShell](../../powershell/download-sql-server-ps-module.md)
* [Valutazione della vulnerabilità SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment)
