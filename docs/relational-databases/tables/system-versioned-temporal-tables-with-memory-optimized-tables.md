---
description: Tabelle temporali con controllo delle versioni di sistema con tabelle con ottimizzazione per la memoria
title: Tabelle temporali con controllo delle versioni di sistema con tabelle con ottimizzazione per la memoria | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 23274522-e5cf-4095-bed8-bf986d6342e0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 05ed5ec0d34d2a1a612b76c0117d0bf66675ad7a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171893"
---
# <a name="system-versioned-temporal-tables-with-memory-optimized-tables"></a>Tabelle temporali con controllo delle versioni di sistema con tabelle con ottimizzazione per la memoria


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Le tabelle temporali con controllo delle versioni di sistema per [tabelle con ottimizzazione per la memoria](../in-memory-oltp/sample-database-for-in-memory-oltp.md) sono progettate per offrire una soluzione economica per scenari in cui sono necessari [controllo dei dati e analisi temporizzate](./temporal-table-usage-scenarios.md) su dati raccolti con carichi di lavoro OLTP in memoria. Queste tabelle garantiscono elevata velocità effettiva transazionale, concorrenza senza blocco e, allo stesso tempo, possibilità di archiviare grandi quantità di dati cronologici che possono essere facilmente sottoposti a query.

## <a name="overview"></a>Panoramica

Le tabelle temporali con controllo delle versioni di sistema mantengono in automatico una cronologia completa delle modifiche ai dati ed espongono pratiche estensioni di Transact-SQL per l'analisi temporizzata. In uno scenario tipico, la cronologia dei dati viene mantenuta per un periodo di tempo molto lungo (più mesi, anche anni), anche se non viene sottoposta spesso a query.

L'analisi temporizzata di controllo dei dati può essere richiesta in ambienti diversi, in particolare nei sistemi OLTP che elaborano grandi numeri di richieste e negli ambienti in cui viene usata la tecnologia OLTP in memoria. Tuttavia, l'uso di tabelle ottimizzate per la memoria negli scenari temporali è difficile perché generalmente una quantità enorme di dati cronologici supera il limite della memoria RAM disponibile. Allo stesso tempo, non è consigliabile usare la RAM per archiviare dati cronologici di sola lettura a cui si accede con minor frequenza man mano che diventano obsoleti.

Le tabelle temporali con controllo delle versioni di sistema per le tabelle con ottimizzazione per la memoria garantiscono elevata velocità effettiva transazionale, concorrenza senza blocco e, allo stesso tempo, possibilità di archiviare grandi quantità di dati cronologici tramite le tabelle in memoria per l'archiviazione dei dati correnti (la tabella temporale) e le tabelle basate su disco per i dati cronologici. L'impatto sulle operazioni DML è ridotto al minimo tramite l'uso di una tabella di staging ottimizzata per la memoria, interna e generata automaticamente che archivia la cronologia recente e abilita i DML per l'esecuzione da codice compilato in modo nativo.

La figura seguente illustra questa architettura.![Architettura in memoria temporale](../../relational-databases/tables/media/temporal-in-memory-architecture.png "Architettura in memoria temporale")

## <a name="implementation-details"></a>Dettagli dell'implementazione

I fattori seguenti sulle tabelle temporali con controllo delle versioni di sistema con tabelle ottimizzate per la memoria sono considerazioni che è necessario tenere presente durante la creazione di una tabella ottimizzata per la memoria con controllo delle versioni di sistema. Per le opzioni di sintassi e un esempio, vedere [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

- Solo le tabelle ottimizzate per la memoria resistenti possono essere tabelle con controllo delle versioni di sistema (**DURABILITY = SCHEMA_AND_DATA**).
- La tabella di cronologia per la tabella ottimizzata per la memoria con controllo delle versioni di sistema deve essere basata su disco, che sia stata creata dall'utente finale o dal sistema.
- Le query che interessano solo la tabella corrente in memoria possono essere usate in [moduli T-SQL compilati in modo nativo](../in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md). Le query temporali che usano la clausola FOR SYSTEM TIME non sono supportate nei moduli compilati in modo nativo. È supportato invece l'uso della clausola FOR SYSTEM TIME insieme a tabelle ottimizzate per la memoria in query ad hoc e in moduli non nativi.
- Con **SYSTEM_VERSIONING = ON** viene creata automaticamente una tabella di staging interna ottimizzata per la memoria per accettare le modifiche più recenti alle versioni di sistema in seguito alle operazioni di aggiornamento ed eliminazione nella tabella ottimizzata per la memoria corrente.
- I dati della tabella di staging interna ottimizzata per la memoria vengono spostati regolarmente nella tabella di cronologia basata su disco dall'attività di scaricamento asincrono dei dati. Questo meccanismo di scaricamento dei dati ha l'obiettivo di mantenere i buffer interni alla memoria inferiori al 10% dell'uso della memoria da parte dei relativi oggetti padre. È possibile monitorare il consumo totale della memoria della tabella temporale ottimizzata per la memoria con controllo delle versioni di sistema eseguendo la query [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md) e riepilogando i dati per la tabella di staging interna ottimizzata per la memoria e la tabella temporale corrente.
- È possibile applicare uno scaricamento dati chiamando [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md).
- Con **SYSTEM_VERSIONING = OFF** o quando viene modificato lo schema della tabella con controllo delle versioni del sistema aggiungendo, eliminando o modificando le colonne, tutti i contenuti del buffer interno di gestione temporanea vengono spostati nella tabella di cronologia basata su disco.
- L'esecuzione di query sui dati cronologici è efficace nel livello di isolamento SNAPSHOT e restituisce sempre un'unione tra il buffer di gestione in memoria e la tabella basata su disco senza duplicati.
- Le operazioni **ALTER TABLE** che modificano internamente lo schema della tabella devono eseguire uno scaricamento di dati, che potrebbe prolungare la durata dell'operazione.

## <a name="the-internal-memory-optimized-staging-table"></a>Tabella interna di gestione temporanea con ottimizzazione per la memoria

La tabella di staging interna ottimizzata per la memoria è un oggetto interno creato dal sistema per ottimizzare le operazioni DML.

- Il nome della tabella viene generato nel formato seguente: **Memory_Optimized_History_Table_<id_oggetto>** dove *<id_oggetto>* è l'identificatore della tabella temporale corrente.
- La tabella consente di replicare lo schema della tabella temporale corrente e una colonna di tipo BIGINT. Questa colonna aggiuntiva garantisce l'univocità delle righe spostate nel buffer interno della cronologia.
- Il nome della colonna aggiuntiva ha il formato seguente: **Change_ID[_< suffix>]** , dove *_\<suffix>* viene aggiunto facoltativamente nel caso in cui la tabella includa già una colonna *Change_ID*.
- Le dimensioni massime delle righe per una tabella ottimizzata per la memoria con controllo delle versioni di sistema vengono ridotte di 8 byte a causa della colonna BIGINT aggiuntiva della tabella di staging. La nuova dimensione massima è ora di 8052 byte.
- La tabella di staging interna ottimizzata per la memoria non è rappresentata in Esplora oggetti di SQL Server Management Studio.
- I metadati relativi a questa tabella nonché la connessione con la tabella temporale corrente sono disponibili in [sys.internal_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md).

## <a name="the-data-flush-task"></a>Attività di scaricamento di dati

Lo scaricamento dei dati è un'attività attivata in modo regolare che controlla se tutte le tabelle ottimizzate per la memoria soddisfano una condizione basato sulle dimensioni della memoria per lo spostamento dei dati. Lo spostamento dei dati inizia quando il consumo di memoria della tabella di staging interna raggiunge l'8% del consumo di memoria di una tabella temporale corrente.

L'attività di scaricamento di dati viene attivata su base periodica con una pianificazione che varia in base al carico di lavoro esistente. Con un carico di lavoro pesante, ad esempio con intervalli di 5 secondi, e con un carico di lavoro leggero, con intervalli di 1 minuto. Un thread viene generato per ogni tabella di staging interna ottimizzata per la memoria che necessita di pulizia.

Lo scaricamento dei dati consente di eliminare tutti i record dal buffer in memoria interno precedenti alla transazione corrente meno recente al fine di spostare questi record sulla tabella di cronologia basata su disco.

È possibile applicare uno scaricamento dati chiamando [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md) e specificando il nome dello schema e della tabella: **sys.sp_xtp_flush_temporal_history \@schema_name, \@object_name**. Con questo comando eseguito dall'utente viene richiamato lo stesso processo di spostamento dei dati richiamato per questa attività dal sistema su pianificazione interna.

## <a name="see-also"></a>Vedere anche

- [Tabelle temporali](../../relational-databases/tables/temporal-tables.md)
- [Introduzione alle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Scenari di utilizzo delle tabelle temporali](../../relational-databases/tables/temporal-table-usage-scenarios.md)
- [Verifiche di coerenza del sistema della tabella temporale](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Partizionamento con le tabelle temporali](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Considerazioni e limitazioni delle tabelle temporali](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Sicurezza di una tabella temporale](../../relational-databases/tables/temporal-table-security.md)
- [Gestire la conservazione dei dati cronologici nelle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Funzioni e viste per i metadati delle tabelle temporali](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
