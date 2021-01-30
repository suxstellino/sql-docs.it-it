---
description: sp_add_log_file_recover_suspect_db (Transact-SQL)
title: sp_add_log_file_recover_suspect_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_file_recover_suspect_db_TSQL
- sp_add_log_file_recover_suspect_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_file_recover_suspect_db
ms.assetid: b41ca3a5-7222-4c22-a012-e66a577a82f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ecde40e66d5754638872a12c79ca580b0b5f2982
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206735"
---
# <a name="sp_add_log_file_recover_suspect_db-transact-sql"></a>sp_add_log_file_recover_suspect_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge un file di log a un filegroup quando non è possibile completare il recupero di un database a causa di spazio del log insufficiente (errore 9002). Una volta aggiunto il file, **sp_add_log_file_recover_suspect_db** disattiva l'impostazione sospetta e completa il ripristino del database. I parametri sono gli stessi di quelli per ALTER DATABASE *database_name* aggiungere il file di log.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_add_log_file_recover_suspect_db [ @dbName= ] 'database' ,   
    [ @name = ] 'logical_file_name' ,   
    [ @filename= ] 'os_file_name' ,   
    [ @size = ] 'size' ,   
    [ @maxsize = ] 'max_size' ,   
    [ @filegrowth = ] 'growth_increment'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbName = ] 'database'` Nome del database. il *database* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @name = ] 'logical_file_name'` Nome utilizzato in per fare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riferimento al file. Deve essere un nome univoco nel server. *logical_file_name* è di **tipo nvarchar (260)** e non prevede alcun valore predefinito.  
  
`[ @filename = ] 'os_file_name'` È il percorso e il nome file usati dal sistema operativo per il file. Il file deve essere disponibile nel server in cui è installato [!INCLUDE[ssDE](../../includes/ssde-md.md)]. *os_file_name* è di **tipo nvarchar (260)** e non prevede alcun valore predefinito.  
  
`[ @size = ] 'size_ '` Dimensioni iniziali del file. *size* è di **tipo nvarchar (20)** e il valore predefinito è null. Specificare un numero intero, ovvero non includere decimali. È possibile utilizzare i suffissi MB e KB per specificare megabyte o kilobyte. Il valore predefinito è MB. Il valore minimo è 512 KB. Se non si specifica *size* , il valore predefinito è 1 MB.  
  
`[ @maxsize = ] 'max_size_ '` Dimensione massima consentita per la crescita del file. *max_size* è di **tipo nvarchar (20)** e il valore predefinito è null. Specificare un numero intero, ovvero non includere decimali. È possibile utilizzare i suffissi MB e KB per specificare megabyte o kilobyte. Il valore predefinito è MB.  
  
 Se *max_size* non è specificato, le dimensioni del file aumenteranno fino a quando il disco non è pieno. Prima che si verifichi questa situazione, l'amministratore riceve un avviso dal registro applicazioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
`[ @filegrowth = ] 'growth_increment_ '` Quantità di spazio aggiunta al file ogni volta che è necessario spazio nuovo. *growth_increment* è di **tipo nvarchar (20)** e il valore predefinito è null. Il valore 0 indica che le dimensioni non verranno aumentate. Specificare un numero intero, ovvero non includere decimali. È possibile specificare il valore in megabyte (MB) o in kilobyte (KB) oppure in forma di percentuale (%). Se si utilizza il suffisso %, l'incremento corrisponde alla percentuale specificata delle dimensioni del file quando si verifica l'incremento. Se si specifica un valore senza il suffisso MB, KB o %, il suffisso predefinito è MB.  
  
 Se *growth_increment* è null, il valore predefinito è 10% e il valore di dimensione minima è 64 KB. Le dimensioni specificate vengono arrotondate al blocco di 64 KB più prossimo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** . Queste autorizzazioni non sono trasferibili.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente, il database `db1` è stato contrassegnato come sospetto durante il recupero a causa di spazio insufficiente nel log (errore 9002).  
  
```  
USE master;  
GO  
EXEC sp_add_log_file_recover_suspect_db db1, logfile2,  
'C:\Program Files\Microsoft SQL  
    Server\MSSQL13.MSSQLSERVER\MSSQL\Data\db1_logfile2.ldf',   
    '1MB';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_add_data_file_recover_suspect_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-data-file-recover-suspect-db-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
