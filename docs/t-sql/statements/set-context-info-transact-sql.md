---
description: SET CONTEXT_INFO (Transact-SQL)
title: SET CONTEXT_INFO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_CONTEXT_INFO_TSQL
- SET CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- context information [SQL Server]
- CONTEXT_INFO option
- SET CONTEXT_INFO statement
ms.assetid: a0b7b9f3-dbda-4350-a274-bd9ecd5c0a74
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 271cf281b2e3dc5f015dd02bc1778fab24be0794
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464160"
---
# <a name="set-context_info-transact-sql"></a>SET CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Associa fino a 128 byte di informazioni binarie alla sessione o connessione corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET CONTEXT_INFO { binary_str | @binary_var }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *binary_str*  
 È una costante **binary** o una costante convertibile in modo implicito in **binary** da associare alla sessione o connessione corrente.  
  
 **@** *binary_var*  
 È una variabile di tipo **varbinary** o **binary** contenente un valore contestuale da associare alla sessione o connessione corrente.  
  
## <a name="remarks"></a>Osservazioni  
 Il modo preferito per recuperare informazioni sul contesto per la sessione corrente consiste nell'utilizzare la funzione CONTEXT_INFO. Le informazioni sul contesto della sessione sono anche archiviate nelle colonne **context_info** nelle viste di sistema seguenti:  
  
-   **sys.dm_exec_requests**  
  
-   **sys.dm_exec_sessions**  
  
-   **sys.sysprocesses**  
  
 Non è possibile specificare l'istruzione SET CONTEXT_INFO in una funzione definita dall'utente. Per l'istruzione SET CONTEXT_INFO inoltre non è possibile specificare un valore Null perché la tabella sysprocesses non ammette valori Null.  
  
 L'opzione SET CONTEXT_INFO non supporta espressioni che non siano costanti o nomi di variabili. Per impostare sul risultato di una chiamata di funzione le informazioni relative al contesto, è innanzitutto necessario inserire il risultato della chiamata di funzione in una variabile di tipo **binary** o **varbinary**.  
  
 Quando viene eseguita l'istruzione SET CONTEXT_INFO in una stored procedure o in un trigger, a differenza di altre istruzioni SET il nuovo valore impostato per le informazioni sul contesto viene mantenuto anche dopo il completamento della stored procedure o del trigger.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-setting-context-information-by-using-a-constant"></a>R. Impostazione delle informazioni relative al contesto tramite una costante  
 Nell'esempio seguente viene illustrato l'utilizzo della costante `SET CONTEXT_INFO`, mediante l'impostazione del valore e la visualizzazione dei risultati. Si noti che per l'esecuzione di una query su `sys.dm_exec_sessions` sono necessarie le autorizzazioni SELECT e VIEW SERVER STATE, mentre non lo sono per l'utilizzo della funzione CONTEXT_INFO.  
  
```sql
SET CONTEXT_INFO 0x01010101;  
GO  
SELECT context_info   
FROM sys.dm_exec_sessions  
WHERE session_id = @@SPID;  
GO  
```  
  
### <a name="b-setting-context-information-by-using-a-function"></a>B. Impostazione delle informazioni relative al contesto tramite una funzione  
 L'esempio seguente dimostra l'utilizzo dell'output di una funzione per impostare il valore contestuale, dove il valore restituito dalla funzione deve essere innanzitutto inserito in una variabile **binary**.  
  
```sql
DECLARE @BinVar varbinary(128);  
SET @BinVar = CAST(REPLICATE( 0x20, 128 ) AS varbinary(128) );  
SET CONTEXT_INFO @BinVar;  
  
SELECT CONTEXT_INFO() AS MyContextInfo;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza a livello di riga](../../relational-databases/security/row-level-security.md) [istruzioni set &#40;&#41;Transact-SQL ](../../t-sql/statements/set-statements-transact-sql.md)   
 [CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)  
 [SESSION_CONTEXT &#40;&#41;Transact-SQL ](../../t-sql/functions/session-context-transact-sql.md)  
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sp_set_session_context &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
  
