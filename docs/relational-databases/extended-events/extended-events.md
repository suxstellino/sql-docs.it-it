---
title: Panoramica di Eventi estesi - SQL Server
description: L'architettura degli eventi estesi di SQL Server consente di raccogliere i dati necessari per identificare e risolvere un problema di prestazioni. È configurabile e scalabile.
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: overview
helpviewer_keywords:
- extended events [SQL Server]
- xe
- XEvents
ms.assetid: bf3b98a6-51ed-4f2d-9c26-92f07f1fa947
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d861263a44b7cbf2d13c3e7f46a573b02a575a45
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492421"
---
# <a name="extended-events-overview"></a>Panoramica degli eventi estesi

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

L'architettura di Eventi estesi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente agli utenti di raccogliere la quantità di dati necessaria per risolvere i problemi o identificare un problema di prestazioni. Eventi estesi è configurabile e facilmente scalabile.

Per altre informazioni sugli eventi estesi, vedere: [Avvio rapido: Eventi estesi in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).

## <a name="benefits-of-ssnoversion-extended-events"></a>Vantaggi degli eventi estesi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Si tratta di un sistema di monitoraggio delle prestazioni leggero in cui vengono usate le risorse minime per le prestazioni. Per gli eventi estesi sono disponibili due interfacce utente grafiche per creare, modificare, visualizzare e analizzare i dati della sessione. Queste interfacce sono denominate:

- Creazione guidata nuova sessione
- Nuova sessione

## <a name="extended-events-concepts"></a>Concetti degli eventi estesi  
 Eventi estesi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è basato su concetti esistenti, ad esempio un evento o un consumer di eventi, usa concetti di Event Tracing for Windows (ETW) e ne introduce di nuovi.  
  
 Nella seguente tabella vengono descritti i concetti negli eventi estesi.  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Pacchetti degli eventi estesi di SQL Server](../../relational-databases/extended-events/sql-server-extended-events-packages.md)|Descrive i pacchetti di Eventi estesi che contengono oggetti. Questi oggetti vengono usati per ottenere ed elaborare dati durante l'esecuzione di una sessione di Eventi estesi.|  
|[Destinazioni degli eventi estesi di SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))|Vengono descritti i consumer di eventi che possono ricevere dati durante una sessione dell'evento.|  
|[Motore degli eventi estesi di SQL Server](../../relational-databases/extended-events/sql-server-extended-events-engine.md)|Viene descritto il motore che implementa e gestisce una sessione degli eventi estesi.|  
|[Sessioni degli eventi estesi di SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md)|Viene descritta la sessione Eventi estesi.|  
| &nbsp; | &nbsp; |
  
## <a name="extended-events-architecture"></a>Architettura degli eventi estesi  

Eventi estesi è il nome di un sistema generale di gestione degli eventi per sistemi server. L'infrastruttura degli eventi estesi supporta la correlazione di dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]e in certe condizioni, la correlazione di dati dal sistema operativo e dalle applicazioni di database. Nel caso del sistema operativo, l'output di Eventi estesi deve essere indirizzato a Event Tracing for Windows (ETW). ETW consente di correlare i dati di un evento con i dati di un evento del sistema operativo o di un'applicazione.  

In tutte le applicazioni sono presenti punti di esecuzione utili sia all'interno che all'esterno di un'applicazione. All'interno dell'applicazione, l'elaborazione asincrona può essere accodata utilizzando informazioni raccolte durante l'esecuzione iniziale di un'attività. All'esterno dell'applicazione, i punti di esecuzione forniscono utilità di monitoraggio con informazioni. Le informazioni sono relative alle caratteristiche del comportamento e delle prestazioni dell'applicazione monitorata.  

 Gli eventi estesi supportano l'utilizzo di dati degli eventi all'esterno di un processo. Questi dati sono in genere utilizzati da:  
  
-   strumenti di analisi, ad esempio Traccia SQL e il Monitoraggio di sistema.  
  
-   strumenti di log, ad esempio il registro eventi di Windows o il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Utenti che amministrano un prodotto o sviluppano applicazioni su un prodotto.  
  
 Gli aspetti chiave della progettazione degli eventi estesi sono i seguenti:  
  
-   Il motore degli eventi estesi è agnostico in termini di eventi. Il motore riesce ad associare qualsiasi evento a qualsiasi destinazione, perché il motore non è vincolato al contenuto dell'evento. Per ulteriori informazioni sul motore degli eventi estesi, vedere [SQL Server Extended Events Engine](../../relational-databases/extended-events/sql-server-extended-events-engine.md).  
  
-   Gli eventi sono separati dai consumer di eventi chiamati *destinazioni* negli eventi estesi. Ciò significa che qualsiasi destinazione può ricevere qualsiasi evento. Inoltre, qualsiasi evento generato può essere utilizzato automaticamente dalla destinazione, che può scrivere nel log o fornire un contesto dell'evento supplementare. Per ulteriori informazioni, vedere [SQL Server Extended Events Targets](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).  
  
-   Gli eventi sono distinti dall'azione da intraprendere quando un evento si verifica. Di conseguenza, qualsiasi azione può essere associata a qualsiasi evento.  
  
-   I predicati consentono di filtrare dinamicamente i casi in cui i dati degli eventi devono essere acquisiti. Il filtro dinamico garantisce maggiore flessibilità per l'infrastruttura di Eventi estesi. Per ulteriori informazioni, vedere [SQL Server Extended Events Packages](../../relational-databases/extended-events/sql-server-extended-events-packages.md).  
  
 Gli eventi estesi possono generare in modo sincrono dati degli eventi (e in modo asincrono elaborare tali dati) il che fornisce una soluzione flessibile per la gestione degli eventi. Inoltre, gli eventi estesi forniscono le funzionalità seguenti:  
  
-   Approccio unificato alla gestione degli eventi nel sistema server, consentendo agli utenti di isolare eventi specifici ai fini della risoluzione dei problemi.  
  
-   Supporto e integrazione con gli strumenti ETW esistenti.  
  
-   Meccanismo di gestione degli eventi interamente configurabile, basato su [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Possibilità di monitorare dinamicamente i processi attivi, con un impatto minimo su tali processi.  
  
-   Sessione di integrità di sistema predefinita che viene eseguita senza effetti visibili sulle prestazioni. Tale sessione consente di raccogliere dati di sistema che è possibile utilizzare per risolvere i problemi relativi alle prestazioni. Per altre informazioni, vedere [Utilizzare la sessione system_health](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="extended-events-tasks"></a>Attività degli eventi estesi  

L'utilizzo di [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] per eseguire funzioni, DMV e istruzioni DDL (Data Definition Language) [!INCLUDE[tsql](../../includes/tsql-md.md)] o viste del catalogo consente di creare soluzioni per la risoluzione dei problemi relativi agli eventi estesi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] semplici o complesse per l'ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Utilizzare **Esplora oggetti** per gestire sessioni di eventi.|[Gestire sessioni di eventi in Esplora oggetti](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)|  
|Viene descritto come creare una sessione di eventi estesi.|[Creare una sessione Eventi estesi](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))|  
|Viene descritto come visualizzare e aggiornare i dati di destinazione.| [Visualizzazione avanzata dei dati di destinazione da eventi estesi in SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)|  
|Viene descritto come utilizzare gli strumenti degli eventi estesi per creare e gestire sessioni di eventi estesi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Strumenti degli eventi estesi](../../relational-databases/extended-events/extended-events-tools.md)|  
|Viene descritto come alterare una sessione Eventi estesi.|[Modificare una sessione Eventi estesi](../../relational-databases/extended-events/alter-an-extended-events-session.md)|  
|Viene descritto come ottenere informazioni sui campi associati agli eventi.|[Recuperare i campi per tutti gli eventi](/previous-versions/sql/sql-server-2016/bb677249(v=sql.130))|  
|Viene descritto come individuare gli eventi disponibili nei pacchetti registrati.|[Visualizzare gli eventi per i pacchetti registrati](./selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)|  
|Viene descritto come individuare le destinazioni degli eventi estesi disponibili nei pacchetti registrati.|[Visualizzare le destinazioni degli eventi estesi per i pacchetti registrati](/previous-versions/sql/sql-server-2016/bb677247(v=sql.130))|  
|Viene descritto come visualizzare gli eventi e le azioni Eventi estesi equivalenti a ogni evento di Traccia SQL e alle colonne associate.|[Visualizzare gli eventi estesi equivalenti alle classi di eventi di Traccia SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)|  
|Viene descritto come trovare i parametri che è possibile impostare quando si utilizza l'argomento ADD TARGET in CREATE EVENT SESSION o ALTER EVENT SESSION.|[Recuperare i parametri configurabili per l'argomento ADD TARGET](/previous-versions/sql/sql-server-2016/bb677176(v=sql.130))|  
|Viene descritto come convertire uno script di Traccia SQL esistente in una sessione Eventi estesi.|[Convertire uno script di Traccia SQL esistente in una sessione Eventi estesi](../../relational-databases/extended-events/convert-an-existing-sql-trace-script-to-an-extended-events-session.md)|  
|Viene descritto come determinare quali query mantengono il blocco, il piano della query e lo stack [!INCLUDE[tsql](../../includes/tsql-md.md)] al momento del blocco.|[Individuare le query che mantengono attivi i blocchi](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)|  
|Viene descritto come individuare l'origine dei blocchi che hanno effetti negativi sulle prestazioni del database.|[Cercare gli oggetti con il maggior numero di blocchi acquisiti](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)|  
|Viene illustrato come utilizzare gli eventi estesi con Analisi eventi per Windows al fine di monitorare l'attività del sistema.|[Monitorare l'attività del sistema mediante gli eventi estesi](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)|  
|Uso delle viste catalogo e delle viste a gestione dinamica (DMV) per gli eventi estesi | [Istruzioni SELECT e JOIN da viste di sistema per eventi estesi in SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) |
| &nbsp; | &nbsp; |

Usare la query Transact-SQL (T-SQL) seguente per elencare tutti i possibili eventi estesi e le relative descrizioni:

```sql
SELECT
     obj1.name as [XEvent-name],
     col2.name as [XEvent-column],
     obj1.description as [Descr-name],
     col2.description as [Descr-column]
  FROM
               sys.dm_xe_objects        as obj1
      JOIN sys.dm_xe_object_columns as col2 on col2.object_name = obj1.name
  ORDER BY
    obj1.name,
    col2.name
```


## <a name="code-examples-can-differ-for-azure-sql-database"></a>Gli esempi di codice possono essere diversi per il database SQL di Azure

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>Vedere anche

[Applicazioni livello dati](../../relational-databases/data-tier-applications/data-tier-applications.md)  
[Supporto dell'applicazione livello dati per oggetti e versioni di SQL Server](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))  
[Distribuire un'applicazione livello dati](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)  
[Monitorare le applicazioni livello dati](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)  
&nbsp;  
[Viste a gestione dinamica degli eventi estesi](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)  
[Viste del catalogo degli eventi estesi (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)  
&nbsp;  
[XELite: libreria multipiattaforma per la lettura di XEvent da file XEL o flussi SQL live](https://www.nuget.org/packages/Microsoft.SqlServer.XEvent.XELite/), rilasciata a maggio 2019.  
[Cmdlet di PowerShell Read-SQLXEvent](https://www.powershellgallery.com/packages/SqlServer.XEvent), rilasciato nel giugno 2019.  
[SQL Mysteries: Causality tracking vs Event Sequence for XEvent Sessions (blog published April 1, 2019)](https://bobsql.com/sql-mysteries-causality-tracking-vs-event-sequence-for-xevent-sessions/) (Misteri di SQL: rilevamento della causalità e sequenza di eventi per le sessioni di XEvent - Blog pubblicato l'1 aprile 2019)