---
title: Performance Dashboard | Microsoft Docs
description: Informazioni su SQL Server Management Studio Performance Dashboard, che fornisce rapidamente informazioni dettagliate su SQL Server e Istanza gestita di SQL di Azure.
ms.custom: ''
ms.date: 11/13/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- performance dashboard [SQL Server]
- performance dashboard reports
- perf dashboard
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pelopes
ms.author: pelopes
manager: amitban
ms.openlocfilehash: ef87d8cdfb44d36faff5e06cfdc0b0f691142c03
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170403"
---
# <a name="performance-dashboard"></a>Performance Dashboard
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] versione 17.2 e successive includono Performance Dashboard. Il dashboard è stato progettato per consentire di visualizzare rapidamente informazioni dettagliate sullo stato delle prestazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) e [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] di Istanza gestita di database SQL di Azure. 

Performance Dashboard consente di comprendere rapidamente se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] sta riscontrando un collo di bottiglia delle prestazioni. E se questo viene trovato, acquisisce facilmente dati di diagnostica aggiuntivi che potrebbero essere necessari per risolvere il problema. Alcuni dei problemi di prestazioni comuni che Performance Dashboard può contribuire a identificare includono:
-  Colli di bottiglia della CPU (e quali query stanno utilizzando la maggior parte di CPU)
-  Colli di bottiglia di I/O (e quali query stanno eseguendo la maggior parte delle operazioni di I/O)
-  Raccomandazioni relative agli indici generate da Query Optimizer (indici mancanti)
-  Processi bloccati
-  Contesa di risorse, inclusa la contesa latch

Performance Dashboard consente anche di identificare query dispendiose che possono essere state eseguite in precedenza. Per definire i costi elevati sono disponibili varie metriche: CPU, Scritture logiche, Letture logiche, Durata, Letture fisiche e Tempo CLR.

Performance Dashboard è suddiviso nelle sezioni e nei sottoreport seguenti:
-  Utilizzo CPU sistema
-  Richieste in attesa correnti
-  Attività corrente
   -  Richieste utente
   -  Sessioni utente
   -  Percentuale riscontri cache
-  Informazioni cronologiche
   -  In attesa
   -  Latch
   -  Statistiche di I/O
   -  Query dispendiose
- Informazioni varie
  -  Tracce attive
  -  Sessioni Xevent attive
  -  Database
  -  Indici mancanti

> [!NOTE] 
> Performance Dashboard usa internamente viste a gestione dinamica (DMV) e funzioni a gestione dinamica (DMF) relative a [Esecuzione](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md), [Indice](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md) e [I/O](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md).

## <a name="to-view-the-performance-dashboard"></a>Per visualizzare Performance Dashboard 
  
Per visualizzare Performance Dashboard fare clic con il pulsante destro del mouse sul nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Esplora oggetti, selezionare **Report**, **Report standard** e fare clic su **Performance Dashboard**.  
  
![Performance Dashboard in menu](../../relational-databases/performance/media/perf_dashboard_ssms.png "Performance Dashboard in menu")  
  
Performance Dashboard verrà visualizzato in una nuova scheda. Di seguito è riportato un esempio in cui è chiaramente presente un collo di bottiglia della CPU:  
  
![Schermata principale di Performance Dashboard](../../relational-databases/performance/media/perf_dashboard.png "Schermata principale di Performance Dashboard")  
  
## <a name="remarks"></a>Osservazioni
Il report **Indici mancanti** segnala gli indici potenzialmente mancanti che Query Optimizer ha identificato durante la compilazione della query. Queste raccomandazioni devono essere tuttavia recepite con cautela. Microsoft consiglia di valutare la creazione di indici con un punteggio maggiore di 100.000 perché tali indici hanno il miglioramento massimo previsto per le query utente. 

> [!TIP]
> Valutare sempre se il suggerimento di un nuovo indice sia confrontabile a un indice esistente nella stessa tabella, in cui gli stessi risultati pratici possono essere ottenuti modificando semplicemente un indice esistente anziché crearne uno nuovo. Se ad esempio si ha un nuovo indice suggerito nelle colonne C1, C2 e C3, è consigliabile valutare prima di tutto se è presente un indice nelle colonne C1 e C2. In caso affermativo potrebbe essere preferibile aggiungere semplicemente la colonna C3 all'indice esistente, mantenendo l'ordine delle colonne preesistenti, per evitare di creare un nuovo indice.
> Per altre informazioni, vedere [Architettura e guida per la progettazione degli indici](../../relational-databases/sql-server-index-design-guide.md).

Il report **Attese** filtra tutte le attese di inattività e sospensione. Per altre informazioni sulle attese, vedere [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) e il [documento relativo all'ottimizzazione delle prestazioni di SQL Server 2005 tramite l'uso di attese e code](https://download.microsoft.com/download/4/7/a/47a548b9-249e-484c-abd7-29f31282b04d/performance_tuning_waits_queues.doc).

I report **Query dispendiose** vengono reimpostati quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene riavviato perché i dati nella DMV sottostante sono stati cancellati. A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], informazioni dettagliate sulle query dispendiose sono reperibili in Query Store. 


> [!NOTE]
> Performance Dashboard all'inizio è stato rilasciato come download autonomo per [SQL Server 2005](https://techcommunity.microsoft.com/t5/SQL-Server-Support/SQL-Server-2005-Performance-Dashboard-Reports/ba-p/315415) e successivamente è stato aggiornato per [SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29063). Sebbene il renderer dei report di SQL Server Management Studio non supporti l'accesso tramite gli Appunti al testo contenuto nei report, è possibile accedere al testo tramite i report autonomi.  Se è necessario copiare il testo delle query dai report, scaricare i report autonomi.

## <a name="permissions"></a>Autorizzazioni  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono necessarie le autorizzazioni `VIEW SERVER STATE` e `ALTER TRACE`. In [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] è richiesta l'autorizzazione `VIEW DATABASE STATE` per il database.

## <a name="see-also"></a>Vedere anche  
 [Monitoraggio e ottimizzazione delle prestazioni](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Strumenti per il monitoraggio e l'ottimizzazione delle prestazioni](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Aprire Monitoraggio attività &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Monitoraggio attività](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Monitoraggio delle prestazioni con Archivio query](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
