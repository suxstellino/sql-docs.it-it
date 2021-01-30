---
description: CHANGE_TRACKING_CURRENT_VERSION (Transact-SQL)
title: CHANGE_TRACKING_CURRENT_VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CHANGE_TRACKING_CURRENT_VERSION_TSQL
- CHANGE_TRACKING_CURRENT_VERSION
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], CHANGE_TRACKING_CURRENT_VERSION
- CHANGE_TRACKING_CURRENT_VERSION
ms.assetid: 3027c4f7-6b4d-4089-a369-5926e8a8da1c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a4726f31cc8bd5ffe12f0a79ceeacee383ca947e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196190"
---
# <a name="change_tracking_current_version-transact-sql"></a>CHANGE_TRACKING_CURRENT_VERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una versione associata all'ultima transazione completata. Questa versione può essere utilizzata per enumerare le modifiche utilizzando [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
CHANGE_TRACKING_CURRENT_VERSION ( )  
```  
  
## <a name="return-type"></a>Tipo restituito  
 **bigint**  
  
## <a name="remarks"></a>Osservazioni  
 Restituisce NULL quando il rilevamento delle modifiche non è abilitato per il database.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene dichiarata la variabile locale `@next_baseline` per memorizzare la versione corrente delle modifiche registrate. La funzione `CHANGE_TRACKING_CURRENT_VERSION()` viene quindi utilizzata per ottenere il valore per la variabile.  
  
```sql  
DECLARE @next_baseline bigint;  
SET @next_baseline = CHANGE_TRACKING_CURRENT_VERSION();  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di rilevamento delle modifiche &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)   
 [Rilevare le modifiche ai dati &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
