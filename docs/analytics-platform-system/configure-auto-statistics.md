---
title: Statistiche automatiche
description: Descrive la funzionalità di statistiche automatiche introdotta nel sistema di piattaforma di analisi AU7.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 4311195ce4b748ffeb73c4a001086b3f199b2c5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052782"
---
# <a name="configure-auto-statistics"></a>Configurare le statistiche automatiche

Informazioni su come configurare la data warehouse parallela per l'utilizzo delle statistiche automatiche per la creazione e l'aggiornamento automatico delle statistiche.  Usare questa funzionalità per migliorare i piani di query e migliorare quindi le prestazioni di esecuzione delle query.

**Si applica a:** APS (a partire da 2016-AU7)

## <a name="what-are-statistics"></a>Informazioni sulle statistiche
Le statistiche per l'ottimizzazione delle query sono oggetti contenenti informazioni statistiche sulla distribuzione dei valori in una o più colonne di una tabella. In Query Optimizer queste statistiche vengono utilizzate per effettuare la stima della cardinalità o del numero di righe nel risultato della query. Queste stime della cardinalità consentono a Query Optimizer di creare un piano di query di alta qualità. Ad esempio, in APS, il Query Optimizer MPP utilizza le stime della cardinalità per scegliere di eseguire il shuffle o replicare il più piccolo di due tabelle utilizzate in una clausola join e di migliorare le prestazioni di esecuzione delle query.  Per ulteriori informazioni, vedere [statistiche](../relational-databases/statistics/statistics.md) e [DBCC SHOW_STATISTICS](../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)

## <a name="what-are-auto-statistics"></a>Che cosa sono le statistiche automatiche?
Le statistiche automatiche sono statistiche che la Query Optimizer crea e aggiorna automaticamente per migliorare il piano di query. Le statistiche possono diventare obsolete dopo le operazioni di caricamento, inserimento, aggiornamento ed eliminazione. Senza statistiche automatiche, è necessario eseguire un'analisi personalizzata per comprendere quali colonne necessitano di statistiche e quando le statistiche devono essere aggiornate.

Le statistiche automatiche includono le tre impostazioni seguenti: 

### <a name="auto_create_statistics"></a>AUTO_CREATE_STATISTICS
Quando l'opzione per la creazione automatica delle statistiche, AUTO_CREATE_STATISTICS, è impostata su ON, Query Optimizer crea le statistiche necessarie per colonne singole nel predicato di query, per migliorare le stime della cardinalità per il piano di query. Queste statistiche di colonna singola vengono create in colonne che ancora non dispongono di un istogramma in un oggetto statistiche esistente.

### <a name="auto_update_statistics"></a>AUTO_UPDATE_STATISTICS 
Quando l'opzione per l'aggiornamento automatico delle statistiche, AUTO_UPDATE_STATISTICS, è impostata su ON, Query Optimizer determina se le statistiche potrebbero non essere aggiornate, quindi ne esegue l'aggiornamento qualora vengano utilizzate tramite una query. Le statistiche diventano obsolete in seguito a operazioni di inserimento, aggiornamento, eliminazione o unione che modificano la distribuzione dei dati nella tabella o nella vista indicizzata. Query Optimizer determina che le statistiche potrebbero non essere aggiornate contando il numero di modifiche apportate ai dati dopo l'ultimo aggiornamento delle statistiche e confrontando il numero di modifiche con una soglia basata sul numero di righe nella tabella o nella vista indicizzata.

### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC
L'opzione relativa all'aggiornamento asincrono delle statistiche, AUTO_UPDATE_STATISTICS_ASYNC, determina se Query Optimizer usa gli aggiornamenti sincroni o asincroni delle statistiche. Per APS, l'opzione di aggiornamento asincrono delle statistiche è abilitata per impostazione predefinita e Query Optimizer aggiorna le statistiche in modo asincrono. L'opzione AUTO_UPDATE_STATISTICS_ASYNC si applica a oggetti statistiche creati per indici, colonne singole nei predicati di query e statistiche create con l'istruzione CREATE STATISTICS.

## <a name="configuration-settings-for-system-administrators"></a>Impostazioni di configurazione per gli amministratori di sistema
Dopo l'aggiornamento a APS AU7, le statistiche automatiche sono abilitate per impostazione predefinita. L'amministratore di sistema può abilitare o disabilitare le statistiche automatiche con l'opzione relativa alle opzioni della [funzionalità](appliance-feature-switch.md) nel Configuration Manager Appliance.  Una volta abilitata, gli utenti possono modificare le impostazioni delle statistiche per ogni database.
Per modificare i valori delle opzioni di funzionalità è necessario riavviare il servizio su punti di ripristino.

## <a name="change-auto-statistics-settings-on-a-database"></a>Modificare le impostazioni delle statistiche automatiche in un database
Quando le statistiche automatiche sono abilitate dall'amministratore di sistema, è possibile utilizzare [ALTER database (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) per modificare le impostazioni delle statistiche in un database. Se l'opzione relativa alla funzionalità auto Statistics è abilitata dall'amministratore di sistema, per tutti i nuovi database creati dopo l'aggiornamento a AU7 sarà abilitata l'opzione auto Statistics. Per tutti i database esistenti prima dell'aggiornamento a AU7 sono state disabilitate le statistiche automatiche. Nell'esempio seguente vengono abilitate le statistiche automatiche sul database esistente myPDW.

```sql
ALTER DATABASE myPDW SET AUTO_CREATE_STATISTICS ON
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS ON 
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS_ASYNC ON
```
 
AUTO_UPDATE opzione STATISTICS_ASYNC funziona solo se AUTO_UPDATE_STATISTICS è ON.  Pertanto, le statistiche non vengono aggiornate quando AUTO_UPDATE_STATISTICS è disattivato e AUTO_UPDATE_STATISTICS_ASYNC è ON. 

### <a name="error-messages"></a>messaggi di errore
È possibile che venga visualizzato il messaggio di errore "questa opzione non è supportata in PDW".  Questo errore si verifica quando l'amministratore di sistema non ha abilitato le statistiche automatiche e si tenta di impostare le opzioni relative alle statistiche automatiche in ALTER DATABASE. 

### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni
Le statistiche automatiche non sono compatibili con le tabelle esterne. 

### <a name="check-the-current-values"></a>Controllare i valori correnti
La query seguente restituisce i valori correnti delle impostazioni di statistiche automatiche per tutti i database.

```sql
SELECT NAME
    , IS_AUTO_CREATE_STATS_ON 
    , IS_AUTO_UPDATE_STATS_ON
    , IS_AUTO_UPDATE_STATS_ASYNC_ON
FROM
    sys.databases;
```

Il valore restituito 1 indica che l'impostazione è impostata su on e 0 indica che l'impostazione è disattivata. 

## <a name="next-steps"></a>Passaggi successivi
Per informazioni sulle prestazioni delle query, vedere [monitoraggio delle query attive](monitoring-active-queries.md)
