---
description: sys.fn_cdc_is_bit_set (Transact-SQL)
title: sys.fn_cdc_is_bit_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_cdc_is_bit_set
- sys.fn_cdc_is_bit_set_TSQL
- sys.fn_cdc_is_bit_set
- fn_cdc_is_bit_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_cdc_is_bit_set
- fn_cdc_is_bit_set
ms.assetid: 792fe7cf-b3b8-4f25-8329-78d63f0e6921
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 77d06cc44b1c7c090e0da7cce360d0a7eed12a71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206113"
---
# <a name="sysfn_cdc_is_bit_set-transact-sql"></a>sys.fn_cdc_is_bit_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indica se una colonna acquisita è stata aggiornata controllando se la posizione ordinale è impostata all'interno di una maschera di bit specificata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_cdc_is_bit_set ( position , update_mask )  
```  
  
## <a name="arguments"></a>Argomenti  
 *position*  
 Posizione ordinale da controllare nella maschera. *position* è di **tipo int**.  
  
 *update_mask*  
 Maschera che identifica le colonne aggiornate. *update_mask* è di tipo **varbinary (128)**.  
  
## <a name="return-type"></a>Tipo restituito  
 **bit**  
  
## <a name="remarks"></a>Commenti  
 Questa funzione è utilizzata in genere come parte di una query che modifica dei dati per indicare se una colonna è stata modificata. In questo scenario, la funzione [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) viene utilizzata prima della query per ottenere l'ordinale di colonna richiesto. **sys.fn_cdc_is_bit_set** viene quindi applicato a ogni riga di dati delle modifiche restituita, fornendo le informazioni specifiche della colonna come parte del set di risultati restituito.  
  
 È consigliabile utilizzare questa funzione al posto della funzione [sys.fn_cdc_has_column_changed](../../relational-databases/system-functions/sys-fn-cdc-has-column-changed-transact-sql.md) quando si determina se le colonne sono state modificate per tutte le righe di un set di risultati restituito.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente è utilizzato `sys.fn_cdc_is_bit_set` da anteporre al set di risultati generato dalla funzione per query `cdc.fn_cdc_get_all_changes_HR_Department` la colonna '`IsGroupNmUpdated`' utilizzando il numero ordinale di colonna pre-calcolato e il valore di `__$update_mask` come argomenti alla chiamata.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @GroupNm_ordinal int;  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HR_Department');  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SET @GroupNm_ordinal = sys.fn_cdc_get_column_ordinal('HR_Department','GroupName');  
  
SELECT sys.fn_cdc_is_bit_set(@GroupNm_ordinal,__$update_mask) as 'IsGroupNmUpdated', *  
FROM cdc.fn_cdc_get_all_changes_HR_Department( @from_lsn, @to_lsn, 'all')  
WHERE __$operation = 4;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni Change Data Capture &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)   
 [sys.fn_cdc_get_column_ordinal &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md)   
 [sys.fn_cdc_has_column_changed &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-has-column-changed-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
