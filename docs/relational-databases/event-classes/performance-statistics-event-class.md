---
description: Performance Statistics - classe di evento
title: Classe di evento Performance Statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Performance Statistics event class
ms.assetid: da9cd2c4-6fdd-4ada-b74f-105e3541393c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f1ffd07f6573a37005fb7bd20bfa015a0a2102b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196318"
---
# <a name="performance-statistics-event-class"></a>Performance Statistics - classe di evento
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La classe di evento Performance Statistics consente di eseguire il monitoraggio delle prestazioni di query, stored prcoedure e trigger in esecuzione. Ciascuna delle sei sottoclassi di evento indica un evento generato nel corso di query, stored procedure e trigger all'interno del sistema. L'utilizzo di tali sottoclassi di evento in combinazione con le viste a gestione dinamica sys.dm_exec_query_stats, sys.dm_exec_procedure_stats e sys.dm_exec_trigger_stats associate consente di ricostituire la cronologia delle prestazioni di qualsiasi query, stored procedure o trigger specifico.  
  
## <a name="performance-statistics-event-class-data-columns"></a>Colonne di dati della classe di evento Performance Statistics  
 Nelle tabelle seguenti sono descritte le colonne di dati della classe di evento associate a ciascuna delle sottoclassi di evento EventSubClass 0, EventSubClass 1, EventSubClass 2, EventSubClass 3, EventSubClass 4 ed EventSubClass 5.  
  
### <a name="eventsubclass-0"></a>EventSubClass 0  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|NULL|52|Sì|  
|BinaryData|**image**|NULL|2|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 0 = Nuovo testo SQL del batch non presente nella cache.<br /><br /> Di seguito sono elencati i tipi di sottoclasse EventSubClass generati nella traccia per batch ad hoc.<br /><br /> Batch ad hoc con *n* query, dove n rappresenta un numero:<br /><br /> 1 di tipo 0|21|Sì|  
|IntegerData2|**int**|NULL|55|Sì|  
|ObjectID|**int**|NULL|22|Sì|  
|Offset|**int**|NULL|61|Sì|  
|PlanHandle|**Immagine**|NULL|65|Sì|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**image**|Handle SQL utilizzabile per ottenere il testo SQL del batch tramite la vista a gestione dinamica sys.dm_exec_sql_text.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|Testo SQL del batch.|1|Sì|  
  
### <a name="eventsubclass-1"></a>EventSubClass 1  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Numero cumulativo di ricompilazioni del piano.|52|Sì|  
|BinaryData|**image**|XML binario del piano compilato.|2|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 1 = Le query incluse in una stored procedure sono state compilate.<br /><br /> Di seguito sono elencati i tipi di sottoclasse EventSubClass generati nella traccia per stored procedure.<br /><br /> Stored procedure con *n* query, dove n rappresenta un numero:<br /><br /> Numero *n* di tipo 1|21|Sì|  
|IntegerData2|**int**|Fine dell'istruzione nella stored procedure.<br /><br /> -1 per la fine della stored procedure.|55|Sì|  
|ObjectID|**int**|ID dell'oggetto assegnato dal sistema.|22|Sì|  
|Offset|**int**|Offset iniziale dell'istruzione nella stored procedure o nel batch.|61|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**image**|Handle SQL utilizzabile per ottenere il testo SQL della stored procedure tramite la vista a gestione dinamica dm_exec_sql_text.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|NULL|1|Sì|  
|PlanHandle|**image**|Handle del piano compilato per la stored procedure. Utilizzabile per ottenere il piano XML tramite la vista a gestione dinamica sys.dm_exec_query_plan.|65|Sì|  
|ObjectType|**int**|Valore che rappresenta il tipo di oggetto coinvolto nell'evento.<br /><br /> 8272 = stored procedure|28|Sì|  
|BigintData2|**bigint**|Quantità di memoria totale, espressa in kilobyte, utilizzata durante la compilazione.|53|Sì|  
|CPU|**int**|Tempo totale di CPU, espresso in millisecondi, dedicato alla compilazione.|18|Sì|  
|Duration|**int**|Tempo totale, espresso in microsecondi, dedicato alla compilazione.|13|Sì|  
|IntegerData|**int**|Dimensioni, espresse in kilobyte, del piano compilato.|25|Sì|  
  
### <a name="eventsubclass-2"></a>EventSubClass 2  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Numero cumulativo di ricompilazioni del piano.|52|Sì|  
|BinaryData|**image**|XML binario del piano compilato.|2|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 2 = Le query incluse in un'istruzione SQL ad hoc sono state compilate.<br /><br /> Di seguito sono elencati i tipi di sottoclasse EventSubClass generati nella traccia per batch ad hoc.<br /><br /> Batch ad hoc con *n* query, dove n rappresenta un numero:<br /><br /> Numero *n* di tipo 2|21|Sì|  
|IntegerData2|**int**|Fine dell'istruzione nel batch.<br /><br /> -1 per la fine del batch.|55|Sì|  
|ObjectID|**int**|N/D|22|Sì|  
|Offset|**int**|Offset iniziale dell'istruzione nel batch.<br /><br /> 0 per l'inizio del batch.|61|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**image**|Handle SQL. Utilizzabile per ottenere il testo SQL del batch tramite la vista a gestione dinamica dm_exec_sql_text.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|NULL|1|Sì|  
|PlanHandle|**image**|Handle del piano compilato per il batch. Utilizzabile per ottenere il piano XML del batch tramite la vista a gestione dinamica dm_exec_query_plan.|65|Sì|  
|BigintData2|**bigint**|Quantità di memoria totale, espressa in kilobyte, utilizzata durante la compilazione.|53|Sì|  
|CPU|**int**|Tempo totale di CPU, espresso in microsecondi, dedicato alla compilazione.|18|Sì|  
|Duration|**int**|Tempo totale, espresso in millisecondi, dedicato alla compilazione.|13|Sì|  
|IntegerData|**int**|Dimensioni, espresse in kilobyte, del piano compilato.|25|Sì|  
  
### <a name="eventsubclass-3"></a>EventSubClass 3  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Numero cumulativo di ricompilazioni del piano.|52|Sì|  
|BinaryData|**image**|NULL|2|Sì|  
|DatabaseID|**int**|ID del database specificato nell'istruzione di *database* USE oppure il database predefinito se per un'istanza specifica l'istruzione di *database* USE non è stata eseguita. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] visualizza il nome del database se la colonna di dati ServerName è acquisita nella traccia e il server è disponibile. Determinare il valore per un database utilizzando la funzione DB_ID.|3|Sì|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 3 = Una query memorizzata nella cache è stata distrutta e anche i dati relativi alla cronologia delle prestazioni associati al piano stanno per essere distrutti.<br /><br /> Di seguito sono elencati i tipi di sottoclasse EventSubClass generati nella traccia.<br /><br /> Batch ad hoc con *n* query, dove n rappresenta un numero:<br /><br /> 1 di tipo 3 quando la query viene scaricata dalla cache<br /><br /> Stored procedure con *n* query, dove n rappresenta un numero:<br /><br /> 1 di tipo 3 quando la query viene scaricata dalla cache.|21|Sì|  
|IntegerData2|**int**|Fine dell'istruzione nella stored procedure o nel batch.<br /><br /> -1 per la fine della stored procedure o del batch.|55|Sì|  
|ObjectID|**int**|NULL|22|Sì|  
|Offset|**int**|Offset iniziale dell'istruzione nella stored procedure o nel batch.<br /><br /> 0 per l'inizio della stored procedure o del batch.|61|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**image**|Handle SQL utilizzabile per ottenere il testo SQL della stored procedure o del batch tramite la vista a gestione dinamica dm_exec_sql_text.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|QueryExecutionStats|1|Sì|  
|PlanHandle|**image**|Handle del piano compilato per la stored procedure o il batch. Utilizzabile per ottenere il piano XML tramite la vista a gestione dinamica dm_exec_query_plan.|65|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
  
### <a name="eventsubclass-4"></a>EventSubClass 4  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|NULL|52|Sì|  
|BinaryData|**image**|NULL|2|Sì|  
|DatabaseID|**int**|ID del database in cui è contenuta la stored procedure specifica.|3|Sì|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 4 = Una stored procedure memorizzata nella cache è stata rimossa dalla cache e i dati relativi alla cronologia delle prestazioni associati verranno distrutti.|21|Sì|  
|IntegerData2|**int**|NULL|55|Sì|  
|ObjectID|**int**|ID della stored procedure. Tale nome è lo stesso di quello della colonna object_id in sys.procedures.|22|Sì|  
|Offset|**int**|NULL|61|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**image**|Handle SQL utilizzabile per ottenere il testo SQL della stored procedure eseguita tramite la vista a gestione dinamica dm_exec_sql_text.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|ProcedureExecutionStats|1|Sì|  
|PlanHandle|**image**|Handle del piano compilato per la stored procedure. Utilizzabile per ottenere il piano XML tramite la vista a gestione dinamica dm_exec_query_plan.|65|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
  
### <a name="eventsubclass-5"></a>EventSubClass 5  
  
|Nome colonna di dati|Tipo di dati|Descrizione|ID colonna|Filtrabile|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|NULL|52|Sì|  
|BinaryData|**image**|NULL|2|Sì|  
|DatabaseID|**int**|ID del database in cui è contenuto il trigger specifico.|3|Sì|  
|EventSequence|**int**|Sequenza di un determinato evento all'interno della richiesta.|51|No|  
|SessionLoginName|**nvarchar**|Nome dell'account di accesso dell'utente che ha avviato la sessione. Se ad esempio si stabilisce la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con l'account di accesso Login1 e si esegue un'istruzione con l'account di accesso Login2, SessionLoginName indica Login1 e LoginName indica Login2. In questa colonna sono visualizzati sia gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che quelli di Windows.|64|Sì|  
|EventSubClass|**int**|Tipo di sottoclasse di evento.<br /><br /> 5 = Un trigger memorizzato nella cache è stata rimosso dalla cache e i dati relativi alla cronologia delle prestazioni associati verranno distrutti.|21|Sì|  
|IntegerData2|**int**|NULL|55|Sì|  
|ObjectID|**int**|ID del trigger. Tale ID è lo stesso di quello della colonna object_id nelle viste del catalogo sys.triggers/sys.server_triggers.|22|Sì|  
|Offset|**int**|NULL|61|Sì|  
|SPID|**int**|ID della sessione in cui si è verificato l'evento.|12|Sì|  
|SqlHandle|**image**|Handle SQL utilizzabile per ottenere il testo SQL del trigger tramite la vista a gestione dinamica dm_exec_sql_text.|63|Sì|  
|StartTime|**datetime**|Ora di inizio dell'evento, se disponibile.|14|Sì|  
|TextData|**ntext**|TriggerExecutionStats|1|Sì|  
|PlanHandle|**image**|Handle del piano compilato per il trigger. Utilizzabile per ottenere il piano XML tramite la vista a gestione dinamica dm_exec_query_plan.|65|Sì|  
|GroupID|**int**|ID del gruppo del carico di lavoro in cui viene generato l'evento di Traccia SQL.|66|Sì|  
  
## <a name="see-also"></a>Vedere anche  
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Classe di evento Showplan XML For Query Compile](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)   
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  
