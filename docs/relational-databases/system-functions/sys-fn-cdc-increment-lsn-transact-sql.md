---
description: sys.fn_cdc_increment_lsn (Transact-SQL)
title: sys.fn_cdc_increment_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn
- fn_cdc_increment_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_increment_lsn
- sys.fn_cdc_increment_lsn
ms.assetid: e53b6703-358b-4c9a-912a-8f7c7331069b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee5d44cbe53d31609a454eb5acbed52a53ae194e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099675"
---
# <a name="sysfn_cdc_increment_lsn-transact-sql"></a>sys.fn_cdc_increment_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il successivo numero di sequenza del file di log (LSN) nella sequenza basata sul numero LSN specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_cdc_increment_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>Argomenti  
 *lsn_value*  
 Valore LSN. *lsn_value* è **binario (10)**.  
  
## <a name="return-type"></a>Tipo restituito  
 **binary(10)**  
  
## <a name="remarks"></a>Osservazioni  
 Il valore LSN restituito dalla funzione è sempre maggiore del valore specificato e non esiste alcun valore LSN tra i due valori.  
  
 Per eseguire sistematicamente query su un flusso di dati delle modifiche, è possibile ripetere periodicamente la chiamata della funzione di query, specificando ogni volta un nuovo intervallo di query per delimitare le modifiche restituite nella query. Per assicurarsi che non si verifichino perdite di dati, il limite superiore della query precedente viene spesso utilizzato per generare il limite inferiore della query successiva. Poiché l'intervallo di query è un intervallo chiuso, il nuovo limite inferiore deve essere più grande del limite superiore precedente, ma piccolo abbastanza da garantire che nessuna modifica abbia valori LSN compresi tra questo valore e il limite superiore precedente. La funzione sys.fn_cdc_increment_lsn viene utilizzata per ottenere questo valore.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo del database public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzato `sys.fn_cdc_increment_lsn` al fine di generare un nuovo valore di limite inferiore per una query di acquisizione di dati delle modifiche basata sul limite superiore salvato da una precedente query e salvato nella variabile `@save_to_lsn`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @save_to_lsn binary(10);  
SET @save_to_lsn = <previous_upper_bound_value>;  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * from cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all' );  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fn_cdc_decrement_lsn &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
