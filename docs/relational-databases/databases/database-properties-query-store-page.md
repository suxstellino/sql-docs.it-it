---
title: Proprietà database (pagina Archivio query)
description: Informazioni su come usare la scheda Query Store nella finestra di dialogo Proprietà database per configurare le modalità, gli intervalli, le soglie e altre proprietà di Query Store.
ms.custom: ''
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.querystore.f1
author: stevestein
ms.author: sstein
ms.date: 01/25/2021
ms.openlocfilehash: 786d73d8f90b12d094a5f53e1045448e05fa7517
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812773"
---
# <a name="database-properties-query-store-page"></a>Proprietà database (pagina Archivio query)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

  Accedere a questa pagina dal database principale e usarla per configurare e modificare le proprietà dell'archivio query del database. È anche possibile configurare queste opzioni con le [opzioni ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md). Per informazioni sull'archivio query, vedere [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="options"></a>Opzioni  
 Modalità operativa  
 I valori validi sono OFF, READ_ONLY e READ_WRITE. OFF disabilita l'archivio query. In modalità READ_WRITE l'archivio query raccoglie e mantiene le informazioni di statistiche sull'esecuzione di runtime e piani di query. In modalità READ_ONLY le informazioni possono essere lette dall'archivio query, ma non vengono aggiunte nuove informazioni. Se lo spazio massimo allocato dell'archivio query è esaurito, la modalità operativa dell'archivio query passa a READ_ONLY.  
  
 Modalità operativa (effettiva)  
 Ottiene la modalità operativa effettiva dell'archivio query.  
  
 Modalità operativa (richiesta)  
 Ottiene e imposta la modalità operativa desiderata dell'archivio query.  
  
 Intervallo di scaricamento dati (minuti)  
 Determina la frequenza con cui i dati scritti nell'archivio query vengono mantenuti su disco. Per ottimizzare le prestazioni, i dati raccolti dall'archivio query vengono scritti in modo asincrono sul disco. La frequenza con cui si verifica questo trasferimento asincrono è configurabile.  
  
 Intervallo di raccolta statistiche  
 Ottiene e imposta il valore dell'intervallo di raccolta delle statistiche.  
  
 Dimensioni massime (MB)  
 Ottiene e imposta lo spazio totale allocato all'archivio query.  
  
 Modalità di acquisizione dell'archivio query  
 -   Nessuna non acquisisce nuove query.  
  
-   Tutte acquisisce tutte le query.  
  
-   Auto acquisisce le query in base all'utilizzo di risorse.  
  
 Soglia per query non aggiornate (giorni)  
 Ottiene e imposta la soglia per query non aggiornate. Configurare l'argomento STALE_QUERY_THRESHOLD_DAYS per specificare il numero di giorni per la conservazione dei dati nell'archivio query.  
  
 Elimina dati query  
 Rimuove il contenuto dell'archivio query.  
  
 Grafici a torta  
 Il grafico a sinistra illustra l'utilizzo totale del file di database sul disco e la parte del file occupata dai dati dell'archivio query.  
  
 Il grafico a destra illustra la parte della quota dell'archivio query attualmente utilizzata. Si noti che la quota non è visualizzata nel grafico a sinistra e può superare le dimensioni correnti del database.  
  
## <a name="remarks"></a>Osservazioni  
 La funzionalità dell'archivio query mette a disposizione degli amministratori di database informazioni dettagliate sulle prestazioni e sulla scelta del piano di query. Semplifica la risoluzione dei problemi in quanto consente di individuare rapidamente le variazioni delle prestazioni causate da modifiche nei piani di query. La funzionalità acquisisce automaticamente una cronologia delle query, dei piani e delle statistiche di runtime e li conserva in modo che sia possibile esaminarli successivamente. I dati vengono separati dagli intervalli di tempo, consentendo di visualizzare i modelli di utilizzo del database e capire quando sono state apportate modifiche al piano di query nel server. Per configurare l'archivio query, si può usare questa pagina delle proprietà del database dell'archivio query oppure l'opzione [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) . Per presentare le informazioni, nell'archivio query viene usata una finestra di dialogo di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Per altre informazioni sull'archivio query, vedere [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di Archivio query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [Viste del catalogo di Archivio query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)