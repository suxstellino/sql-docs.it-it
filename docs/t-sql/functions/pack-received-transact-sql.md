---
description: '&#x40;&#x40;PACK_RECEIVED (Transact-SQL)'
title: '@@PACK_RECEIVED (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@PACK_RECEIVED_TSQL'
- '@@PACK_RECEIVED'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@PACK_RECEIVED function'
- number of packets read
- packets [SQL Server], number read
ms.assetid: 5c0b3d36-bfad-4f0b-abb8-e8f6391b32cd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 535a59f8181f1661146d4583ecadd286942799eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101790"
---
# <a name="x40x40pack_received-transact-sql"></a>&#x40;&#x40;PACK_RECEIVED (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il numero di pacchetti di input letti dalla rete dopo l'ultimo avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
@@PACK_RECEIVED  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **integer**  
  
## <a name="remarks"></a>Commenti  
 Per visualizzare un report contenente dati statistici relativi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], inclusi i pacchetti inviati e ricevuti, eseguire **sp_monitor**.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato l'utilizzo di `@@PACK_RECEIVED`.  
  
```sql  
SELECT @@PACK_RECEIVED AS 'Packets Received';   
```  
  
 Set di risultati di esempio:  
  
```  
Packets Received  
----------------  
128  
```  
  
## <a name="see-also"></a>Vedere anche  
 [@@PACK_SENT](../../t-sql/functions/pack-sent-transact-sql.md)   
 [sp_monitor](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Funzioni statistiche di sistema](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
