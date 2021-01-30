---
description: '&#x40;&#x40;CPU_BUSY (Transact-SQL)'
title: CPU_BUSY (Transact-SQL)
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@CPU_BUSY_TSQL'
- '@@CPU_BUSY'
dev_langs:
- TSQL
helpviewer_keywords:
- CPU [SQL Server]
- status information [SQL Server], CPU
- ticks [SQL Server]
- time [SQL Server], CPU activity
- '@@CPU_BUSY function'
- statistical information [SQL Server], CPU
- CPU [SQL Server], activity
ms.assetid: 81ae0e64-79fa-4a74-9aa5-37045c4cd211
author: cawrites
ms.author: chadam
ms.openlocfilehash: a51d0b6e891ccefed7f3e6454c114f1765d44685
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184142"
---
# <a name="x40x40cpu_busy-transact-sql"></a>&#x40;&#x40;CPU_BUSY (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questa funzione restituisce il periodo di tempo in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato attivo dopo l'ultimo avvio. `@@CPU_BUSY` restituisce un risultato misurato in incrementi di tempo di CPU, o "tick". Questo valore è cumulativo per tutte le CPU, quindi può essere maggiore del tempo trascorso effettivo. Per convertire in microsecondi, moltiplicare per [@@TIMETICKS](./timeticks-transact-sql.md).
  
> [!NOTE]  
>  Se il periodo di tempo restituito nelle variabili @@CPU_BUSY o @@IO_BUSY è superiore a circa 49 giorni di tempo di CPU cumulativo, si riceve un avviso di overflow aritmetico. In tal caso, il valore delle variabili `@@CPU_BUSY`, `@@IO_BUSY` e `@@IDLE` non è preciso.  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
@@CPU_BUSY  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]


## <a name="return-types"></a>Tipi restituiti
**integer**
  
## <a name="remarks"></a>Commenti  
Per visualizzare un report contenente dati statistici relativi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], inclusa l'attività della CPU, eseguire [sp_monitor](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md).
  
## <a name="examples"></a>Esempi  
Questo esempio restituisce l'attività della CPU di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in corrispondenza della data e dell'ora correnti. L'esempio converte uno dei valori nel tipo di dati `float` per evitare problemi di overflow aritmetico quando si calcola un valore in microsecondi.
  
```sql
SELECT @@CPU_BUSY * CAST(@@TIMETICKS AS FLOAT) AS 'CPU microseconds',   
   GETDATE() AS 'As of' ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
CPU microseconds As of
---------------- -----------------------
18406250         2006-12-05 17:00:50.600
```
  
## <a name="see-also"></a>Vedi anche
[sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)  
[@@IDLE &#40;Transact-SQL&#41;](../../t-sql/functions/idle-transact-sql.md)  
[@@IO_BUSY &#40;Transact-SQL&#41;](../../t-sql/functions/io-busy-transact-sql.md)  
[sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)  
[Funzioni statistiche di sistema &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)
  
  
