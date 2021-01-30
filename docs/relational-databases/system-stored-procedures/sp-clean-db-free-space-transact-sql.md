---
description: sp_clean_db_free_space (Transact-SQL)
title: sp_clean_db_free_space (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_clean_db_free_space_TSQL
- sp_clean_db_free_space
dev_langs:
- TSQL
helpviewer_keywords:
- sp_clean_db_free_space
- ghost records
ms.assetid: faa96f7e-be92-47b1-8bc5-4dbba5331655
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1779f07e6c8923f56bb4597623a9b079679b3779
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206531"
---
# <a name="sp_clean_db_free_space-transact-sql"></a>sp_clean_db_free_space (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove le informazioni residue lasciate nelle pagine del database a causa delle routine di modifica dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sp_clean_db_free_space pulisce tutte le pagine in tutti i file del database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
sp_clean_db_free_space   
  [ @dbname = ] 'database_name'   
  [ , [ @cleaning_delay = ] 'delay_in_seconds' ] [;]  
```  
  
## <a name="arguments"></a>Argomenti  
 @dbname ='*database_name*'  
 Nome del database da pulire. *dbname* è di **tipo sysname** e non può essere null.  
  
 @cleaning_delay ='*delay_in_seconds*'  
 Specifica un intervallo di ritardo tra le pulizie delle pagine per ridurre l'impatto sul sistema di I/O. *delay_in_seconds* è di **tipo int** e il valore predefinito è 0.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Le operazioni di aggiornamento o le operazioni di eliminazione da una tabella che provocano lo spostamento di una riga consentono di liberare immediatamente spazio in una pagina poiché rimuovono i riferimenti alla riga specifica. In alcune circostanze, tuttavia, la riga può rimanere fisicamente nella pagina di dati come record fantasma. I record fantasma vengono rimossi periodicamente da un processo in background. Questi dati residui non vengono restituiti dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] in risposta alle query. Tuttavia, negli ambienti in cui la sicurezza fisica dei file di dati o di backup è a rischio, è possibile usare `sp_clean_db_free_space` per pulire questi record fantasma. Per eseguire questa operazione per ogni file di database, usare [sp_clean_db_file_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md). 
  
 La quantità di tempo necessaria per eseguire sp_clean_db_free_space dipende dalle dimensioni del file, dallo spazio libero disponibile e dalla capacità del disco. Poiché `sp_clean_db_free_space` l'esecuzione di può influire in modo significativo sulle attività di I/O, è consigliabile eseguire questa procedura al di fuori delle ore di lavoro normali.  
  
 Prima di eseguire `sp_clean_db_free_space` , è consigliabile creare un backup completo del database.  
  
 Il [sp_clean_db_file_free_space](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) correlato stored procedure può pulire un singolo file.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al `db_owner` ruolo del database.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono eliminate tutte le informazioni residue dal database `AdventureWorks2012`.  
  
```sql  
USE master;  
GO  
EXEC sp_clean_db_free_space @dbname = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Guida al processo di pulizia fantasma](../ghost-record-cleanup-process-guide.md)    
 [sp_clean_db_file_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
  
  
