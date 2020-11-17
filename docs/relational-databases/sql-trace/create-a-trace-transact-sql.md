---
description: Creare una traccia (Transact-SQL)
title: Creare una traccia (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- traces [SQL Server], example
- traces [SQL Server], creating
ms.assetid: 79dd4254-e3c6-467a-bb6f-f99e51757e99
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ff2970bf4d450c425f169be7b2bb72c24db7d2d0
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364793"
---
# <a name="create-a-trace-transact-sql"></a>Creare una traccia (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come utilizzare stored procedure per creare una traccia.  
  
### <a name="to-create-a-trace"></a>Per creare una traccia  
  
1.  Eseguire **sp_trace_create** con i parametri necessari per creare una nuova traccia. La nuova traccia sarà in stato di arresto (*status* uguale a **0**).  
  
2.  Eseguire **sp_trace_setevent** con i parametri necessari per selezionare gli eventi e le colonne da tracciare.  
  
3.  Facoltativamente, eseguire **sp_trace_setfilter** per impostare qualsiasi filtro o una combinazione di filtri.  

     È possibile eseguire **sp_trace_setevent** e **sp_trace_setfilter** solo su tracce esistenti arrestate.  
  
    > [!IMPORTANT]  
    >  A differenza di quanto avviene con le normali stored procedure, i parametri di tutte le stored procedure di SQL Server Profiler (<strong>sp_trace_ *xx*</strong>) sono rigidamente tipizzati e non supportano la conversione automatica del tipo di dati. Se tali parametri non vengono chiamati con i tipi di dati corretti per i parametri di input, come indicato nella descrizione dell'argomento, la stored procedure restituisce un errore.  
  
## <a name="examples"></a>Esempi

 Nell'esempio di codice riportato di seguito viene illustrata la creazione di una traccia utilizzando [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'esempio è costituito da tre sezioni, relative alla creazione della traccia, al popolamento del file di traccia e all'arresto della traccia. Personalizzare la traccia aggiungendo gli eventi per cui si desidera eseguirla. Per l'elenco di eventi e colonne, vedere [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).  
  
### <a name="a-create-a-trace"></a>R. Creare una traccia
 Nel codice seguente viene creata una traccia, vengono aggiunti eventi, quindi viene avviata la traccia:  
  
```  
DECLARE @RC int, @TraceID int, @on BIT  
EXEC @rc = sp_trace_create @TraceID output, 0, N'C:\SampleTrace'  
  
-- Select the return code to see if the trace creation was successful.  
SELECT RC = @RC, TraceID = @TraceID  
  
-- Set the events and data columns you need to capture.  
SELECT @on = 1  
  
-- 10 is RPC:Completed event. 1 is TextData column.   
EXEC sp_trace_setevent @TraceID, 10, 1, @on   
-- 13 is SQL:BatchStarting, 11 is LoginName  
EXEC sp_trace_setevent @TraceID, 13, 11, @on   
-- 13 is SQL:BatchStarting, 14 is StartTime  
EXEC sp_trace_setevent @TraceID, 13, 14, @on   
-- 12 is SQL:BatchCompleted, 15 is EndTime  
EXEC sp_trace_setevent @TraceID, 12, 15, @on   
-- 13 is SQL:BatchStarting, 1 is TextData  
EXEC sp_trace_setevent @TraceID, 13, 1, @on   
  
-- Set any filter. Not provided in this example  
--EXEC sp_trace_setfilter 1, 10, 0, 6, N'%Profiler%'  
  
-- Start Trace (status 1 = start)  
EXEC @RC = sp_trace_setstatus @TraceID, 1  
GO  
  
```  
  
### <a name="b-populate-the-trace-file"></a>B. Popolare il file di traccia
 Dopo che la traccia è stata creata e avviata, eseguire il codice seguente per popolare la traccia con attività.  
  
```  
SELECT * FROM master.sys.databases  
GO  
SELECT * FROM ::fn_trace_getinfo(default)  
GO  
  
```  
  
### <a name="c-stop-the-trace"></a>C. Arrestare la traccia
 La traccia può essere arrestata e riavviata in qualsiasi momento. In questo esempio eseguire il seguente codice per arrestare e chiudere la traccia e successivamente eliminarne la definizione.  
  
```  
DECLARE @TraceID int  
-- Populate a variable with the trace_id of the current trace  
SELECT  @TraceID = TraceID FROM ::fn_trace_getinfo(default) WHERE VALUE = N'C:\SampleTrace.trc'  
  
-- First stop the trace.   
EXEC sp_trace_setstatus @TraceID, 0  
  
-- Close and then delete its definition from SQL Server.   
EXEC sp_trace_setstatus @TraceID, 2  
  
```  
  
### <a name="d-examine-the-trace-file"></a>D. Esaminare il file di traccia
 Per esaminare il file di traccia, aprire il file SampleTrace.trc utilizzando [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di SQL Server Profiler &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)  
  
  
