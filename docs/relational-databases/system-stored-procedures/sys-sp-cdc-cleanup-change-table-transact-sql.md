---
description: sys.sp_cdc_cleanup_change_table (Transact-SQL)
title: sys.sp_cdc_cleanup_change_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_cleanup_change_table
- sp_cdc_cleanup_change_table_TSQL
- sys.sp_cdc_cleanup_change_table
- sys.sp_cdc_cleanup_change_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_cleanup_change_tables
- sp_cdc_cleanup_change_tables
ms.assetid: 02295794-397d-4445-a3e3-971b25e7068d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fc6ca4964beff8adc8f32598948964d597076804
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210752"
---
# <a name="syssp_cdc_cleanup_change_table-transact-sql"></a>sys.sp_cdc_cleanup_change_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove le righe dalla tabella delle modifiche nel database corrente in base al valore di *low_water_mark* specificato. Questa stored procedure è fornita agli utenti che vogliono gestire direttamente il processo di pulizia della tabella delle modifiche. Tuttavia, è necessario fare attenzione poiché la procedura influisce su tutti gli utenti dei dati della tabella delle modifiche.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_cleanup_change_table   
  [ @capture_instance = ] 'capture_instance',   
  [ @low_water_mark = ] low_water_mark ,  
  [ @threshold = ]'delete threshold'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @capture_instance =]'*capture_instance*'  
 Nome dell'istanza di acquisizione associata alla tabella delle modifiche. *capture_instance* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
 *capture_instance* necessario assegnare un nome a un'istanza di acquisizione esistente nel database corrente.  
  
 [ @low_water_mark =] *low_water_mark*  
 Numero di sequenza del file di log (LSN) che deve essere utilizzato come nuovo limite minimo per l' *istanza di acquisizione*. *low_water_mark* è **binario (10)** e non prevede alcun valore predefinito.  
  
 Se il valore è diverso da null, deve apparire come valore start_lsn di una voce corrente nella tabella [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) . Se altre voci in cdc.lsn_time_mapping condividono la stessa ora di commit della voce identificata dal nuovo limite minimo, il valore LSN minore associato a tale gruppo di voci viene scelto come limite minimo.  
  
 Se il valore viene impostato in modo esplicito su NULL, il limite *minimo* corrente per l' *istanza di acquisizione* viene utilizzato per definire il limite superiore per l'operazione di pulizia.  
  
 [ @threshold =]'*Elimina soglia*'  
 Numero massimo di voci che possono essere eliminate utilizzando un'unica istruzione nel processo di pulizia. *delete_threshold* è di tipo **bigint** e il valore predefinito è 5000.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 sys.sp_cdc_cleanup_change_table esegue le operazioni seguenti:  
  
1.  Se il @low_water_mark parametro non è null, imposta il valore di start_lsn per l' *istanza di acquisizione* sul nuovo limite *minimo*.  
  
    > [!NOTE]  
    >  Il nuovo limite minimo potrebbe non essere quello specificato nella chiamata alla stored procedure. Se altre voci nella tabella cdc.lsn_time_mapping condividono la stessa ora di commit, il valore start_lsn più piccolo rappresentato nel gruppo di voci viene selezionato come limite minimo modificato. Se il @low_water_mark parametro è null o il limite minimo corrente è maggiore del nuovo LowWatermark, il valore start_lsn per l'istanza di acquisizione viene lasciato invariato.  
  
2.  Le voci della tabella delle modifiche con valori __$start_lsn inferiori al limite minimo vengono quindi eliminate. La soglia di eliminazione viene utilizzata per limitare il numero di righe eliminate in una singola transazione. Viene restituito un errore sull'eliminazione delle voci, che però non influisce sulle modifiche apportate al limite minimo dell'istanza di acquisizione in base alla chiamata.  

 Utilizzare sys.sp_cdc_cleanup_change_table nelle circostanze seguenti:  
  
-   I report del processo Cleanup Agent eliminano gli errori.  
  
     Un amministratore può eseguire questa stored procedure in modo esplicito per riprovare un'operazione non riuscita. Per ripetere la pulizia per una determinata istanza di acquisizione, eseguire sys.sp_cdc_cleanup_change_table e specificare NULL per il @low_water_mark parametro.  
  
-   I criteri basati sulla memorizzazione utilizzati dal processo di Cleanup Agent non sono adeguati.  
  
     Poiché questa stored procedure esegue il processo di pulizia per una singola istanza di acquisizione, è possibile utilizzarla per compilare una strategia di pulizia personalizzata in modo da applicare le regole di pulizia in base alla singola istanza di acquisizione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner.  
  
## <a name="see-also"></a>Vedere anche  
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_get_min_lsn &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [sys.fn_cdc_increment_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md)  
  
  
