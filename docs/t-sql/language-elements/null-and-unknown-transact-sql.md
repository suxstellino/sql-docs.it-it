---
description: NULL e UNKNOWN (Transact-SQL)
title: NULL e UNKNOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 9d491846-4730-4740-a680-77c69fae4a58
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 645f70cbef933dd372227202162d9dedeca2c377
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185258"
---
# <a name="null-and-unknown-transact-sql"></a>NULL e UNKNOWN (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  NULL indica che il valore è sconosciuto. Un valore Null è diverso da un valore zero o vuoto. Non esistono due valori Null uguali. I confronti tra due valori Null o tra un valore NULL e qualsiasi altro valore restituiscono UNKNOWN in quanto ogni valore NULL è sconosciuto.  
  
 In genere, i valori Null indicano dati sconosciuti, non applicabili o da aggiungere in seguito. È possibile, ad esempio, che l'iniziale del secondo nome di un cliente non sia nota nel momento in cui il cliente emette un ordine.  
  
 Per i valori Null, tenere presente quanto segue:  
  
-   Per verificare i valori Null in una query, utilizzare IS NULL o IS NOT NULL nella clausola WHERE.  
  
-   È possibile inserire valori Null in una colonna specificando NULL in modo esplicito in un'istruzione INSERT o UPDATE oppure escludendo una colonna da un'istruzione INSERT.  
  
-   Non è possibile usare i valori Null come informazioni necessarie per distinguere una riga in una tabella da un'altra riga in una tabella, ad esempio le chiavi primarie, o per le informazioni usate per distribuire le righe, ad esempio le chiavi di distribuzione.  
  
 Se i dati includono valori Null, gli operatori logici e di confronto possono restituire potenzialmente un terzo valore, ovvero UNKNOWN, anziché TRUE o FALSE. Questa logica a tre valori è necessaria, ma causa numerosi errori nelle applicazioni. Gli operatori logici in un'espressione booleana che include valori UNKNOWN restituiranno UNKNOWN a meno che il risultato dell'operatore non dipenda dall'espressione UNKNOWN. Queste tabelle offrono esempi di questo comportamento.  
  
 La tabella seguente illustra il risultato che si ottiene quando si applica l'operatore AND a due espressioni booleane in cui un'espressione restituisce UNKNOWN.  
  
|Espressione 1|Espressione 2|Risultato|  
|---------------|---------------|------------|  
|true|UNKNOWN|UNKNOWN|  
|UNKNOWN|UNKNOWN|UNKNOWN|  
|FALSE|UNKNOWN|FALSE|  
  
 La tabella seguente illustra il risultato che si ottiene quando si applica l'operatore OR a due espressioni booleane in cui un'espressione restituisce UNKNOWN.  
  
|Espressione 1|Espressione 2|Risultato|  
|---------------|---------------|------------|  
|true|UNKNOWN|TRUE|  
|UNKNOWN|UNKNOWN|UNKNOWN|  
|FALSE|UNKNOWN|UNKNOWN|  
  
## <a name="see-also"></a>Vedere anche  
 [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md)   
 [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md)   
 [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md)   
 [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md)  
  
  
