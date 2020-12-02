---
description: '&#x40;&#x40;TOTAL_ERRORS (Transact-SQL)'
title: '@@TOTAL_ERRORS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@TOTAL_ERRORS'
- '@@TOTAL_ERRORS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@TOTAL_ERRORS function'
- total errors [SQL Server]
- errors [SQL Server], read/write
- number of disk read/write errors
- disks [SQL Server], errors
- write errors [SQL Server]
- read/write errors
ms.assetid: 09e62428-ee0e-4ef5-b969-da9d255f1199
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 851909caa64e8391275d5908f49e652bf1d993ec
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128349"
---
# <a name="x40x40total_errors-transact-sql"></a>&#x40;&#x40;TOTAL_ERRORS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il numero di errori di scrittura su disco rilevati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dopo l'ultimo avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
@@TOTAL_ERRORS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **integer**  
  
## <a name="remarks"></a>Commenti  
 Questa funzione non restituisce tutti gli errori di scrittura rilevati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Gli errori di scrittura occasionali e non irreversibili vengono gestiti dal server stesso e non vengono considerati errori veri e propri. Per visualizzare un report contenente dati statistici relativi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluso il numero totale di errori, eseguire **sp_monitor**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato il numero di errori rilevati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in corrispondenza della data e dell'ora correnti.  
  
```sql
SELECT @@TOTAL_ERRORS AS 'Errors', GETDATE() AS 'As of';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Errors      As of                   
----------- ----------------------  
0           3/28/2003 12:32:11 PM   
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Funzioni statistiche di sistema &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
