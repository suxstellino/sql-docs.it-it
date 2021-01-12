---
description: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
title: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_CLEANUP_VERSION
- CHANGE_TRACKING_CLEANUP_VERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGE_TRACKING_MIN_VALID_VERSION
- change tracking [SQL Server], CHANGE_TRACKING_MIN_VALID_VERSION
ms.assetid: 5a43d23f-adcf-4c0b-95ad-07cee03c1f9d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 81e386ef391283f3a3f3095b7dd321055a676332
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093903"
---
# <a name="change_tracking_min_valid_version-transact-sql"></a>CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce la versione minima sul client che è possibile utilizzare per ottenere informazioni sul rilevamento delle modifiche dalla tabella specificata, quando si utilizza la funzione [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .  
    
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
CHANGE_TRACKING_MIN_VALID_VERSION ( table_object_id )  
```  
  
## <a name="arguments"></a>Argomenti  
 *table_object_id*  
 ID oggetto della tabella. *table_object_id* è di **tipo int**.  
  
## <a name="return-type"></a>Tipo restituito  
 **bigint**  
  
## <a name="remarks"></a>Osservazioni  
 Usare questa funzione per convalidare il valore del parametro *last_sync_version* per CHANGETABLE. Se *last_sync_version* è minore del valore riportato da questa funzione, i risultati restituiti da una chiamata successiva a CHANGETABLE potrebbero non essere validi.  
  
 CHANGE_TRACKING_MIN_VALID_VERSION utilizza le informazioni seguenti per determinare il valore restituito:  
  
-   Quando è stato abilitato il rilevamento delle modifiche per la tabella.  
  
-   Quando è stata eseguita l'attività di pulizia in background per rimuovere le informazioni sul rilevamento delle modifiche anteriori al periodo di memorizzazione specificato per il database.  
  
-   Se la tabella è stata troncata. In questo modo vengono rimosse tutte le informazioni sul rilevamento delle modifiche associate alla tabella.  
  
 La funzione restituisce NULL se si verifica una delle condizioni seguenti:  
  
-   Rilevamento modifiche non abilitato per il database.  
  
-   ID oggetto della tabella specifica non valido per il database corrente.  
  
-   Autorizzazione insufficiente per la tabella specificata dall'ID oggetto.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato come determinare la validità della versione specificata. È necessario ottenere la minima versione valida della tabella `dbo.Employees`, confrontandola quindi con il valore della variabile `@last_sync_version`. Se il valore di `@last_sync_version` è inferiore al valore di `@min_valid_version`, l'elenco delle righe modificate non sarà valido.  
  
> [!NOTE]  
>  È possibile ottenere il valore da una tabella o da un altro percorso in cui è stato memorizzato l'ultimo numero di versione utilizzato per sincronizzare i dati.  
  
```  
-- The tracked change is tagged with the specified context   
DECLARE @min_valid_version bigint, @last_sync_version bigint;  
  
SET @min_valid_version =   
CHANGE_TRACKING_MIN_VALID_VERSION(OBJECT_ID('dbo.Employees'));  
  
SET @last_sync_version = 11  
IF (@last_sync_version < @min_valid_version)  
-- Error � do not obtain changes  
ELSE  
-- Obtain changes using CHANGETABLE(CHANGES ...)  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di rilevamento delle modifiche &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
  
