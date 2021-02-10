---
title: Opzione funzionalità
description: Visualizza informazioni sulle due opzioni di funzionalità introdotte in Analytics Platform System AU7.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 656b1592dd50567f59744e39c24be7d93b5b220f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052832"
---
# <a name="appliance-feature-switches"></a>Opzioni funzionalità dispositivo

La pagina **commutatore funzionalità** Visualizza informazioni sulle opzioni di funzionalità introdotte in AU7 Platform System e versioni successive. Usare questa pagina di configurazione per aggiornare o abilitare/disabilitare le funzionalità e le impostazioni nel sistema di piattaforma di analisi.

> [!NOTE]
> Per le modifiche ai valori delle opzioni di funzionalità è necessario riavviare il servizio.

![Opzione della funzionalità Appliance DWConfig](media/feature-switch/SQL_Server_PDW_DWConfig_feature_switch.png "Opzione della funzionalità Appliance DWConfig")

## <a name="autostatsenabled"></a>AutoStatsEnabled

Controlla la funzionalità di statistiche automatiche. Questa opzione di funzionalità è impostata su true per impostazione predefinita dopo l'aggiornamento a AU7. Qualsiasi database creato dopo l'aggiornamento erediterà la creazione automatica e l'aggiornamento asincrono delle statistiche. Per i database esistenti, gli amministratori di database possono abilitare le statistiche automatiche con [ALTER database (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw). Per ulteriori informazioni sulle statistiche, vedere [statistiche](../relational-databases/statistics/statistics.md).

## <a name="maxdopforinsertqueries"></a>MaxDOPForInsertQueries

Consente di selezionare le impostazioni MAXDOP maggiori di 1 per le operazioni di inserimento/selezione. Le opzioni per questa impostazione sono 0, 1, 2 e 4 e il valore predefinito è 1.

## <a name="optimizecommonsubexpressions"></a>OptimizeCommonSubExpressions

Consente di migliorare le prestazioni delle query eliminando lo spostamento dei dati per sottoespressione comune in SQL Query Optimizer. Una spiegazione dettagliata di questa funzionalità è disponibile [qui](common-sub-expression-elimination.md).

## <a name="usecatalogqueries"></a>UseCatalogQueries

L'utilizzo di oggetti del catalogo per alcune chiamate di metadati anziché l'utilizzo di SMO ha mostrato un miglioramento delle prestazioni. Impostare su true per impostazione predefinita in CU 7.1, questa opzione controlla il comportamento.

## <a name="dmsprocessstopmessagetimeoutinseconds"></a>DmsProcessStopMessageTimeoutInSeconds

Controlla il tempo di attesa del servizio di spostamento dei dati (DMS) per la sincronizzazione in un sistema occupato quando viene annullata una query che interessa lo spostamento dei dati. L'aggiornamento a AU7 imposta questo valore su 900 secondi (15 minuti) per impostazione predefinita. L'intervallo valido è pari a 0-3600 secondi.
