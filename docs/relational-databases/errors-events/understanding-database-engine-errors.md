---
title: Informazioni sugli errori del Motore di database | Microsoft Docs
description: Informazioni sugli attributi degli errori generati dal motore di database di SQL Server e su come accedere a tutti i messaggi di errore di sistema e definiti dall'utente da sys.messages.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- errors [SQL Server], about errors
- errors [SQL Server], Database Engine
- errors [SQL Server]
- Database Engine [SQL Server], errors
ms.assetid: ddaca9d3-956f-46a5-8cd3-a7a15ec75878
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c836216ffa3acd6bdb24c660114980c1f3d2c259
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737701"
---
# <a name="understanding-database-engine-errors"></a>Informazioni sugli errori del Motore di database
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La tabella seguente descrive gli attributi degli errori generati da [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|Numero di errore|Numero di errore univoco per ogni messaggio di errore.|  
|Stringa del messaggio di errore|Il messaggio di errore include informazioni di diagnostica relative alla causa dell'errore. Molti messaggi di errore contengono variabili di sostituzione con informazioni specifiche, ad esempio il nome dell'oggetto che ha generato l'errore.|  
|Gravità|Indica il livello di gravità dell'errore. Gli errori con un livello di gravità basso, ad esempio 1 o 2, sono messaggi informativi o avvisi di basso livello, mentre gli errori che presentano un livello di gravità elevato segnalano problemi da risolvere immediatamente. Per altre informazioni sui livelli di gravità, vedere [Gravità degli errori del Motore di database](../../relational-databases/errors-events/database-engine-error-severities.md).|  
|State|Alcuni messaggi di errore possono essere generati in più punti del codice di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. L'errore 1105, ad esempio, può essere generato in risposta a più condizioni diverse. A ogni condizione specifica che genera un errore viene assegnato un codice di stato univoco.<br /><br /> Quando si consultano database che contengono informazioni relative a problemi noti, ad esempio la [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base, è possibile utilizzare il numero di contesto per determinare se il problema registrato corrisponde all'errore che si è verificato. Se ad esempio un articolo della Knowledge Base descrive un errore 1105 che presenta uno stato 2, mentre il messaggio di errore 1105 ricevuto dall'utente presenta uno stato 3, è probabile che la causa dell'errore sia diversa da quella indicata nell'articolo.<br /><br /> Il codice di contesto di un errore consente inoltre al personale del supporto tecnico [!INCLUDE[msCoName](../../includes/msconame-md.md)] di individuare la posizione nel codice sorgente da cui è stato generato il codice di errore. Queste informazioni possono fornire indicazioni aggiuntive utili per la diagnostica del problema.|  
|Nome della stored procedure|Rappresenta il nome della stored procedure o del trigger in cui si è verificato l'errore.|  
|Numero di riga|Indica quale istruzione di un batch, di una stored procedure, di un trigger o di una funzione ha generato l'errore.|  
  
 Tutti i messaggi di errore di sistema e definiti dall'utente in un'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] sono contenuti nella vista del catalogo **sys.messages** . È possibile utilizzare l'istruzione RAISERROR per restituire errori definiti dall'utente a un'applicazione.  
  
 Tutte le API di database, ad esempio lo spazio dei nomi **SQLClient** di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], ADO (ActiveX Data Objects), OLE DB e ODBC (Open Database Connectivity) forniscono gli attributi di base dell'errore. Queste informazioni includono il numero di errore e la stringa di messaggio. Non tutte le API forniscono tuttavia anche tutti gli altri attributi dell'errore.  
  
 Le informazioni relative a un errore che si verifica nell'ambito del blocco TRY di un costrutto TRY...CATCH possono essere ottenute nel codice [!INCLUDE[tsql](../../includes/tsql-md.md)] con funzioni quali ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY ed ERROR_STATE nell'ambito del blocco CATCH associato. Per altre informazioni, vedere [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene eseguita una query sulla vista del catalogo `sys.messages` per restituire un elenco di tutti i messaggi di errore di sistema e definiti dall'utente in [!INCLUDE[ssDE](../../includes/ssde-md.md)] con testo in lingua inglese (`1033`).  
  
```  
SELECT  
    message_id,  
    language_id,  
    severity,  
    is_event_logged,  
    text  
  FROM sys.messages  
  WHERE language_id = 1033;  
```  
  
 Per altre informazioni, vedere [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md).  
  
## <a name="see-also"></a>Vedere anche  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)  
  
  
