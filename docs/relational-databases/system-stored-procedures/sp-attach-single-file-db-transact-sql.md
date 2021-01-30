---
description: sp_attach_single_file_db (Transact-SQL)
title: sp_attach_single_file_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_single_file_db
- sp_attach_single_file_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_single_file_db
ms.assetid: 13bd1044-9497-4293-8390-1f12e6b8e952
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8fd050ff5706cc2406fcb4502c5d93a8253bc4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197703"
---
# <a name="sp_attach_single_file_db-transact-sql"></a>sp_attach_single_file_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Collega al server corrente un database che include solo un file di dati. non è possibile usare **sp_attach_single_file_db** con più file di dati.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] È invece consigliabile utilizzare CREATE DATABASE *database_name* per l'associazione. Per alte informazioni, vedere [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md). Non utilizzare questa procedura per database replicati.  
  
> [!IMPORTANT]  
>  È consigliabile evitare di collegare o ripristinare database provenienti da origini sconosciute o non attendibili. Tali database possono contenere codice dannoso che potrebbe eseguire codice [!INCLUDE[tsql](../../includes/tsql-md.md)] indesiderato o causare errori modificando lo schema o la struttura fisica di database. Prima di utilizzare un database da un'origine sconosciuta o non attendibile, eseguire [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sul database in un server non di produzione ed esaminare il codice contenuto nel database, ad esempio le stored procedure o altro codice definito dall'utente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_attach_single_file_db [ @dbname= ] 'dbname'  
    , [ @physname= ] 'physical_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Nome del database da collegare al server. Il nome deve essere univoco. *dbname* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @physname = ] 'physical_name'` Nome fisico, incluso il percorso, del file di database. *physical_name* è di **tipo nvarchar (260)** e il valore predefinito è null.  
  
> [!NOTE]  
>  Questo argomento esegue il mapping al parametro FILENAME dell'istruzione CREATE DATABASE. Per alte informazioni, vedere [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
 Quando si collega un database di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] contenente file di cataloghi full-text in un'istanza del server di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , i file di catalogo vengono collegati dal percorso precedente insieme agli altri file del database, come in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per altre informazioni, vedere [Aggiornamento della ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Utilizzare **sp_attach_single_file_db** solo nei database precedentemente scollegati dal server utilizzando un'operazione di **sp_detach_db** esplicita o nei database copiati.  
  
 **sp_attach_single_file_db** funziona solo nei database che dispongono di un unico file di log. Quando **sp_attach_single_file_db** collega il database al server, compila un nuovo file di log. Se il database è di sola lettura, il file di log verrà creato nella relativa posizione precedente.  
  
> [!NOTE]  
>  Non è possibile scollegare o collegare uno snapshot del database.  
  
 Non utilizzare questa procedura per database replicati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per informazioni sul modo in cui vengono gestite le autorizzazioni quando viene collegato un database, vedere [create database &#40;SQL Server&#41;Transact-SQL ](../../t-sql/statements/create-database-transact-sql.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene scollegato il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], quindi uno dei file inclusi nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene collegato al server corrente.  
  
```  
USE master;  
GO  
EXEC sp_detach_db @dbname = 'AdventureWorks2012';  
EXEC sp_attach_single_file_db @dbname = 'AdventureWorks2012',   
    @physname =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_Data.mdf';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
