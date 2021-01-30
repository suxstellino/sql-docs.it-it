---
description: sp_delete_backuphistory (Transact-SQL)
title: sp_delete_backuphistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_backuphistory
- sp_delete_backuphistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_backuphistory
ms.assetid: bdb56834-616e-47e4-b942-e895d2325e97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac5426940ed3fd2a94c055968a22ec1000e4d564
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195545"
---
# <a name="sp_delete_backuphistory-transact-sql"></a>sp_delete_backuphistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Riduce le dimensioni delle tabelle di cronologia di backup e ripristino eliminando le voci per set di backup precedenti alla data specificata. Vengono aggiunte righe aggiuntive alle tabelle di cronologia di backup e ripristino dopo l'esecuzione di ogni operazione di backup o ripristino. è pertanto consigliabile eseguire periodicamente **sp_delete_backuphistory**.  
  
> [!NOTE]  
>  Le tabelle di cronologia di backup e ripristino risiedono nel database **msdb** .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_delete_backuphistory [ @oldest_date = ] 'oldest_date'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @oldest_date = ] 'oldest\_date'` Data meno recente mantenuta nelle tabelle di cronologia di backup e ripristino. *oldest_date* è di tipo **DateTime** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_delete_backuphistory** necessario eseguire dal database **msdb** e influire sulle tabelle seguenti:  
  
-   [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
  
-   [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
  
-   [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
  
-   [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
  
-   [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
-   [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
  
-   [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
  
-   [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
  
 I file di backup fisici vengono conservati anche se tutta la cronologia viene eliminata.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** , ma è possibile concedere le autorizzazioni ad altri utenti.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono eliminate dalle tabelle di cronologia di backup e ripristino tutte le voci create prima delle ore 0.00 del 14 gennaio 2010.  
  
```  
USE msdb;  
GO  
EXEC sp_delete_backuphistory @oldest_date = '01/14/2010';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_delete_database_backuphistory &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)   
 [Informazioni sulla cronologia e sull'intestazione del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
