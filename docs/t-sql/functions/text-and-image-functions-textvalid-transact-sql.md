---
description: Funzioni per i valori text e image - TEXTVALID (Transact-SQL)
title: TEXTVALID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTVALID_TSQL
- TEXTVALID
dev_langs:
- TSQL
helpviewer_keywords:
- invalid text pointers [SQL Server]
- valid text pointers [SQL Server]
- TEXTVALID function
- checking text pointers
- text-pointer values
- verifying text pointers
ms.assetid: 9411c349-b59b-4740-a270-92f91d81ad23
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 17a2d30b57e5c60624509f5adaafc2fb94fc30d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181468"
---
# <a name="text-and-image-functions---textvalid-transact-sql"></a>Funzioni per i valori text e image - TEXTVALID (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Funzione **text**, **ntext** o **image** che controlla se un puntatore di testo specifico è valido.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Non è disponibile una funzionalità alternativa.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TEXTVALID ( 'table.column' ,text_ ptr )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *tabella*  
 Nome della tabella che si desidera utilizzare.  
  
 *column*  
 Nome della colonna che si desidera utilizzare.  
  
 *text_ptr*  
 Puntatore di testo che si desidera controllare.  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="remarks"></a>Osservazioni  
 Restituisce 1 se il puntatore è valido e 0 in caso contrario. Si noti che l'identificatore per la colonna di tipo **text** deve includere il nome della tabella. Non è possibile utilizzare UPDATETEXT, WRITETEXT o READTEXT senza un puntatore di testo valido.  
  
 Le funzioni e istruzioni seguenti sono utili anche quando si usano dati di tipo **text**, **ntext** e **image**.  
  
|Funzione o istruzione|Descrizione|  
|---------------------------|-----------------|  
|PATINDEX **(** ' _%pattern%_ ' **,** _expression_ **)**|Restituisce la posizione dei caratteri di una determinata stringa di caratteri nelle colonne di tipo **text** e **ntext**.|  
|DATALENGTH **(** _expression_ **)**|Restituisce la lunghezza dei dati nelle colonne **text**, **ntext** e **image**.|  
|SET TEXTSIZE|Restituisce il limite in byte dei dati di tipo **text**, **ntext** o **image** da restituire con un'istruzione SELECT.|  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene stabilito se esiste un puntatore di testo valido per ogni valore della colonna `logo` della tabella `pub_info`.  
  
> [!NOTE]  
>  Per eseguire l'esempio, è necessario installare il database **pubs**.  
  
```sql
USE pubs;  
GO  
SELECT pub_id, 'Valid (if 1) Text data'   
   = TEXTVALID ('pub_info.logo', TEXTPTR(logo))   
FROM pub_info  
ORDER BY pub_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
pub_id Valid (if 1) Text data   
------ ----------------------   
0736   1                        
0877   1                        
1389   1                        
1622   1                        
1756   1                        
9901   1                        
9952   1                        
9999   1                        
  
(8 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
 [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)   
 [Funzioni per i valori text e image &#40;Transact-SQL&#41;](./text-and-image-functions-textptr-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)  
  
