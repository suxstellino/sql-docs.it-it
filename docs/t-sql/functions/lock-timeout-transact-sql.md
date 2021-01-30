---
description: '&#x40;&#x40;LOCK_TIMEOUT (Transact-SQL)'
title: '@@LOCK_TIMEOUT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@LOCK_TIMEOUT'
- '@@LOCK_TIMEOUT_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- timeout options [SQL Server], locks
- '@@LOCK_TIMEOUT function'
- current lock time-out setting
- locking [SQL Server], time-outs
ms.assetid: 6bf8bf97-60b8-40c1-b89d-8f5a00bcae2e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 68181dbd097d0f61c8051ef5d3d113360bb52ada
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200600"
---
# <a name="x40x40lock_timeout-transact-sql"></a>&#x40;&#x40;LOCK_TIMEOUT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce l'impostazione corrente del timeout del blocco, in millisecondi, per la sessione corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
@@LOCK_TIMEOUT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti
 **integer**  
  
## <a name="remarks"></a>Commenti  
 L'istruzione SET LOCK_TIMEOUT consente a un'applicazione di impostare il periodo di tempo massimo durante il quale un'istruzione rimane in attesa di una risorsa bloccata. Quando il periodo di attesa di un'istruzione supera il valore massimo impostato con l'opzione LOCK_TIMEOUT, l'istruzione bloccata viene annullata automaticamente e nell'applicazione viene restituito un messaggio di errore.  
  
 @@LOCK_TIMEOUT restituisce il valore -1 se l'istruzione SET LOCK_TIMEOUT non è stata ancora eseguita nella sessione corrente.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrato il set di risultati ottenuto quando per l'opzione LOCK_TIMEOUT non è stato impostato alcun valore.  
  
```sql  
SELECT @@LOCK_TIMEOUT AS [Lock Timeout];  
GO  
```  
  
 Set di risultati:  
  
```  
Lock Timeout  
------------  
-1  
```  
  
 Nell'esempio seguente l'opzione LOCK_TIMEOUT viene impostata su 1800 millisecondi, quindi viene richiamata la funzione @@LOCK_TIMEOUT.  
  
```sql  
SET LOCK_TIMEOUT 1800;  
SELECT @@LOCK_TIMEOUT AS [Lock Timeout];  
GO  
```  
  
 Set di risultati:  
  
```  
Lock Timeout  
------------  
1800          
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di configurazione &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SET LOCK_TIMEOUT &#40;Transact-SQL&#41;](../../t-sql/statements/set-lock-timeout-transact-sql.md)  
  
  
