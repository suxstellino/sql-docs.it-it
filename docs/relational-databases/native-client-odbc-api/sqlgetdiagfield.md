---
description: SQLGetDiagField
title: SQLGetDiagField | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetDiagField function
ms.assetid: 395245ba-0372-43ec-b9a4-a29410d85a6d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2aa29dda546e709b06b7a87300ffe66e5505ffd5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754151"
---
# <a name="sqlgetdiagfield"></a>SQLGetDiagField
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client specifica i campi di diagnostica aggiuntivi seguenti per **SQLGetDiagField**. Questi campi supportano la segnalazione dettagliata degli errori per le applicazioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e sono disponibili in tutti i record di diagnostica generati negli handle di istruzione ODBC e di connessione ODBC collegati. I campi sono definiti in sqlncli.h.  
  
|Campo del record di diagnostica|Descrizione|  
|------------------------------|-----------------|  
|SQL_DIAG_SS_LINE|Segnala il numero di riga di una stored procedure che genera un errore. Il valore di SQL_DIAG_SS_LINE è significativo solo se SQL_DIAG_SS_PROCNAME restituisce un valore. Il valore viene restituito come numero intero senza segno a 16 bit.|  
|SQL_DIAG_SS_MSGSTATE|Stato di un messaggio di errore. Per informazioni sullo stato del messaggio di errore, vedere [RAISERROR](../../t-sql/language-elements/raiserror-transact-sql.md). Il valore viene restituito come numero intero con segno a 32 bit.|  
|SQL_DIAG_SS_PROCNAME|Nome della stored procedure che genera un errore, se appropriato. Il valore viene restituito come stringa di caratteri. La lunghezza della stringa (in caratteri) dipende dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Può essere determinato chiamando [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) richiedendo il valore per SQL_MAX_PROCEDURE_NAME_LEN.|  
|SQL_DIAG_SS_SEVERITY|Livello di gravità del messaggio di errore associato. Il valore viene restituito come numero intero con segno a 32 bit.|  
|SQL_DIAG_SS_SRVNAME|Nome del server in cui si è verificato l'errore. Il valore viene restituito come stringa di caratteri. La lunghezza della stringa (in caratteri) viene definita dalla macro SQL_MAX_SQLSERVERNAME in sqlncli.h.|  
  
 I campi di diagnostica specifici di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenenti dati caratteri, SQL_DIAG_SS_PROCNAME e SQL_DIAG_SS_SRVNAME, restituiscono i dati in questione al cliente come stringhe Unicode, ANSI o con terminazione Null. Se necessario, il conteggio dei caratteri deve essere regolato in base alla larghezza dei caratteri. In alternativa, è possibile utilizzare un tipo di dati C portabile, ad esempio TCHAR o SQLTCHAR, per assicurare una lunghezza corretta della variabile di programma.  
  
 Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client segnala i codici di funzione dinamica aggiuntivi seguenti che identificano l'ultima istruzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tentata. Il codice di funzione dinamica viene restituito nell'intestazione (record 0) del set di record di diagnostica ed è pertanto disponibile a ogni esecuzione, indipendentemente dall'esito di quest'ultima.  
  
|Codice di funzione dinamica|Source (Sorgente)|  
|---------------------------|------------|  
|SQL_DIAG_DFC_SS_ALTER_DATABASE|ALTER DATABASE - istruzione|  
|SQL_DIAG_DFC_SS_CHECKPOINT|Istruzione CHECKPOINT|  
|SQL_DIAG_DFC_SS_CONDITION|L'errore si è verificato nella clausola WHERE o HAVING di un'istruzione.|  
|SQL_DIAG_DFC_SS_CREATE_DATABASE|Istruzione CREATE DATABASE|  
|SQL_DIAG_DFC_SS_CREATE_DEFAULT|Istruzione CREATE DEFAULT |  
|SQL_DIAG_DFC_SS_CREATE_PROCEDURE|CREATE PROCEDURE - istruzione|  
|SQL_DIAG_DFC_SS_CREATE_RULE|Istruzione CREATE RULE|  
|SQL_DIAG_DFC_SS_CREATE_TRIGGER|CREATE TRIGGER - istruzione|  
|SQL_DIAG_DFC_SS_CURSOR_DECLARE|DECLARE CURSOR - istruzione|  
|SQL_DIAG_DFC_SS_CURSOR_OPEN|OPEN - istruzione|  
|SQL_DIAG_DFC_SS_CURSOR_FETCH|FETCH - istruzione|  
|SQL_DIAG_DFC_SS_CURSOR_CLOSE|Istruzione CLOSE|  
|SQL_DIAG_DFC_SS_DEALLOCATE_CURSOR|DEALLOCATE - istruzione|  
|SQL_DIAG_DFC_SS_DBCC|Istruzione DBCC|  
|SQL_DIAG_DFC_SS_DENY|DENY - istruzione|  
|SQL_DIAG_DFC_SS_DROP_DATABASE|DROP DATABASE - istruzione|  
|SQL_DIAG_DFC_SS_DROP_DEFAULT|Istruzione DROP DEFAULT|  
|SQL_DIAG_DFC_SS_DROP_PROCEDURE|Istruzione DROP PROCEDURE|  
|SQL_DIAG_DFC_SS_DROP_RULE|Istruzione DROP RULE|  
|SQL_DIAG_DFC_SS_DROP_TRIGGER|Istruzione DROP TRIGGER|  
|SQL_DIAG_DFC_SS_DUMP_DATABASE|Istruzione BACKUP o DUMP DATABASE |  
|SQL_DIAG_DFC_SS_DUMP_TABLE|Istruzione DUMP TABLE|  
|SQL_DIAG_DFC_SS_DUMP_TRANSACTION|Istruzione BACKUP o DUMP TRANSACTION. Restituito anche per un'istruzione CHECKPOINT se il file **tronca. log in chkpt.** l'opzione di database è on.|  
|SQL_DIAG_DFC_SS_GOTO|Istruzione per il controllo di flusso GOTO|  
|SQL_DIAG_DFC_SS_INSERT_BULK|Istruzione INSERT BULK |  
|SQL_DIAG_DFC_SS_KILL|Istruzione KILL|  
|SQL_DIAG_DFC_SS_LOAD_DATABASE|Istruzione LOAD o RESTORE DATABASE|  
|SQL_DIAG_DFC_SS_LOAD_HEADERONLY|Istruzione LOAD o RESTORE HEADERONLY.|  
|SQL_DIAG_DFC_SS_LOAD_TABLE|Istruzione LOAD TABLE|  
|SQL_DIAG_DFC_SS_LOAD_TRANSACTION|Istruzione LOAD o RESTORE TRANSACTION |  
|SQL_DIAG_DFC_SS_PRINT|istruzione PRINT|  
|SQL_DIAG_DFC_SS_RAISERROR|istruzione RAISERROR|  
|SQL_DIAG_DFC_SS_READTEXT|Istruzione READTEXT|  
|SQL_DIAG_DFC_SS_RECONFIGURE|Istruzione RECONFIGURE|  
|SQL_DIAG_DFC_SS_RETURN|Istruzione per il controllo di flusso RETURN|  
|SQL_DIAG_DFC_SS_SELECT_INTO|SELECT INTO - istruzione|  
|SQL_DIAG_DFC_SS_SET|Istruzione SET (generica, tutte le opzioni) |  
|SQL_DIAG_DFC_SS_SET_IDENTITY_INSERT|SET IDENTITY_INSERT - istruzione|  
|SQL_DIAG_DFC_SS_SET_ROW_COUNT|SET ROWCOUNT - istruzione|  
|SQL_DIAG_DFC_SS_SET_STATISTICS|Istruzione SET STATISTICS IO o SET STATISTICS TIME|  
|SQL_DIAG_DFC_SS_SET_TEXTSIZE|SET TEXTSIZE - istruzione|  
|SQL_DIAG_DFC_SS_SETUSER|SETUSER - istruzione|  
|SQL_DIAG_DFC_SS_SET_XCTLVL|Istruzione SET TRANSACTION ISOLATION LEVEL|  
|SQL_DIAG_DFC_SS_SHUTDOWN|Istruzione SHUTDOWN|  
|SQL_DIAG_DFC_SS_TRANS_BEGIN|Istruzione BEGIN TRAN |  
|SQL_DIAG_DFC_SS_TRANS_COMMIT|Istruzione COMMIT TRAN|  
|SQL_DIAG_DFC_SS_TRANS_PREPARE|Preparazione al commit di una transazione distribuita|  
|SQL_DIAG_DFC_SS_TRANS_ROLLBACK|Istruzione ROLLBACK TRAN |  
|SQL_DIAG_DFC_SS_TRANS_SAVE|Istruzione SAVE TRAN|  
|SQL_DIAG_DFC_SS_TRUNCATE_TABLE|TRUNCATE TABLE - istruzione|  
|SQL_DIAG_DFC_SS_UPDATE_STATISTICS|UPDATE STATISTICS - istruzione|  
|SQL_DIAG_DFC_SS_UPDATETEXT|UPDATETEXT, istruzione|  
|SQL_DIAG_DFC_SS_USE|USE - istruzione|  
|SQL_DIAG_DFC_SS_WAITFOR|Istruzione per il controllo di flusso WAITFOR |  
|SQL_DIAG_DFC_SS_WRITETEXT|Istruzione WRITETEXT|  
  
## <a name="sqlgetdiagfield-and-table-valued-parameters"></a>SQLGetDiagField e parametri con valori di tabella  
 SQLGetDiagField può essere usato per recuperare due campi di diagnostica: SQL_DIAG_SS_TABLE_COLUMN_NUMBER e SQL_DIAG_SS_TABLE_ROW_NUMBER. Tali campi consentono di identificare il valore che ha generato l'errore o l'avviso associato al record di diagnostica.  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLGetDiagField (funzione)](../../odbc/reference/syntax/sqlgetdiagfield-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
