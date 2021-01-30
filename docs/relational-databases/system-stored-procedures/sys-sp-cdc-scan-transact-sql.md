---
description: sys.sp_cdc_scan (Transact-SQL)
title: sys.sp_cdc_scan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_scan_TSQL
- sp_cdc_scan
- sys.sp_cdc_scan
- sp_cdc_scan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_scan
ms.assetid: 46e4294c-97b8-47d6-9ed9-b436a9929353
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b839f65e6361c88cb842e5d18de9173db8c3727f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159764"
---
# <a name="syssp_cdc_scan-transact-sql"></a>sys.sp_cdc_scan (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esegue l'operazione di analisi del log di Change Data Capture.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_scan [ [ @maxtrans = ] max_trans ]   
     [ , [ @maxscans = ] max_scans ]   
     [ , [ @continuous = ] continuous ]   
     [ , [ @pollinginterval = ] polling_interval ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @maxtrans = ] max_trans` Numero massimo di transazioni da elaborare in ogni ciclo di analisi. *max_trans* è di **tipo int** e il valore predefinito è 500.  
  
`[ @maxscans = ] max_scans` Numero massimo di cicli di analisi da eseguire per estrarre tutte le righe dal log. *max_scans* è di **tipo int** e il valore predefinito è 10.  
  
`[ @continuous = ] continuous` Indica se il stored procedure deve terminare dopo l'esecuzione di un singolo ciclo di analisi (0) o se viene eseguito in modo continuo, sospendendo il tempo specificato da *polling_interval* prima di rieseguire il ciclo di analisi (1). *Continuous* è di **tinyint** e il valore predefinito è 0.  
  
`[ @pollinginterval = ] polling_interval` Numero di secondi tra i cicli di analisi del log. *polling_interval* è di tipo **bigint** e il valore predefinito è 0.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 sys.sp_cdc_scan viene chiamata internamente da sys.sp_MScdc_capture_job se il processo di acquisizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent viene utilizzato da Change Data Capture. La procedura non può essere eseguita in modo esplicito se è già attiva un'operazione di analisi del log di Change Data Capture o se il database è abilitato per la replica transazionale. Questa stored procedure deve essere utilizzata dagli amministratori che desiderano personalizzare il comportamento del processo di acquisizione che viene configurato automaticamente.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner.  
  
## <a name="see-also"></a>Vedere anche  
 [dbo.cdc_jobs &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)  
  
  
