---
description: cdc.fn_cdc_get_all_changes_&lt;capture_instance&gt;  (Transact-SQL)
title: cdc.fn_cdc_get_all_changes_&lt;capture_instance&gt;  (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_all_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_all_changes_<capture_instance>
ms.assetid: c6bad147-1449-4e20-a42e-b51aed76963c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8bb04e74ab2dd613173bf194fe4ca5412d79ac7e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095033"
---
# <a name="cdcfn_cdc_get_all_changes_ltcapture_instancegt--transact-sql"></a>cdc.fn_cdc_get_all_changes_&lt;capture_instance&gt;  (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ciascuna modifica applicata alla tabella di origine all'interno dell'intervallo del numero di sequenza del file di log (LSN) specificato. Se a una riga di origine vengono applicate più modifiche durante l'intervallo, ogni modifica è riportata nel set di risultati restituito. Oltre alla restituzione dei dati delle modifiche, quattro colonne di metadati forniscono le informazioni necessarie per applicare le modifiche a un'altra origine dati. Al contenuto delle colonne dei metadati e alle righe restituite nel set di risultati vengono applicate le opzioni di filtro di riga. Quando è specificata l'opzione di filtro di riga 'all', per l'identificazione di ogni modifica è disponibile esattamente una riga. Quando è specificata l'opzione 'all update old', le operazioni di aggiornamento sono rappresentate su due righe: una contiene i valori delle colonne acquisite prima dell'aggiornamento e l'altra contiene i valori delle colonne acquisite dopo l'aggiornamento.  
  
 Questa funzione di enumerazione viene creata nel momento in cui una tabella di origine è abilitata per Change Data Capture. Il nome della funzione è derivato e utilizza il formato **cdc.fn_cdc_get_all_changes_**_capture_instance_ dove *capture_instance* è il valore specificato per l'istanza di acquisizione quando la tabella di origine è abilitata per la Change Data Capture.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
cdc.fn_cdc_get_all_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all update old  
}  
```  
  
## <a name="arguments"></a>Argomenti  
 *from_lsn*  
 Valore LSN che rappresenta l'endpoint inferiore dell'intervallo LSN da includere nel set di risultati. *from_lsn* è **binario (10)**.  
  
 Nel set di risultati vengono incluse solo le righe di [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) tabella delle modifiche con un valore in **_ _ $ start_lsn** maggiore o uguale a *from_lsn* .  
  
 *to_lsn*  
 Valore LSN che rappresenta l'endpoint superiore dell'intervallo LSN da includere nel set di risultati. *to_lsn* è **binario (10)**.  
  
 Solo le righe del [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) tabella delle modifiche con un valore in **_ _ $ start_lsn** maggiore o uguale a *from_lsn* e minore o uguale a *to_lsn* sono incluse nel set di risultati.  
  
 <row_filter_option>:: = {All | All Update Old}  
 Opzione applicata al contenuto delle colonne dei metadati e alle righe restituite nel set di risultati.  
  
 Le opzioni possibili sono le seguenti:  
  
 all  
 Restituisce tutte le modifiche all'interno dell'intervallo LSN specificato. Per le modifiche dovute a un'operazione di aggiornamento, questa opzione restituisce solo la riga che contiene i nuovi valori dopo l'applicazione dell'aggiornamento.  
  
 all update old  
 Restituisce tutte le modifiche all'interno dell'intervallo LSN specificato. Per le modifiche dovute a un'operazione di aggiornamento, questa opzione restituisce sia la riga che contiene i valori di colonna precedenti l'aggiornamento che la riga che contiene i valori di colonna successivi all'aggiornamento.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|Numero LSN di commit associato alla modifica. Mantiene l'ordine del commit della modifica. Le modifiche di cui è stato eseguito il commit nella stessa transazione condividono lo stesso valore LSN di commit.|  
|**__$seqval**|**binary(10)**|Valore di sequenza utilizzato per ordinare le modifiche a una riga all'interno di una transazione.|  
|**__$operation**|**int**|Identifica l'operazione DML (Data Manipulation Language) necessaria per applicare la riga di dati di modifica all'origine dati di destinazione. Può essere uno dei valori seguenti:<br /><br /> 1 = eliminazione<br /><br /> 2 = inserimento<br /><br /> 3 = aggiornamento (i valori di colonna acquisiti sono quelli precedenti l'aggiornamento). Questo valore si applica solo quando è specificata l'opzione di filtro di riga 'all update old'.<br /><br /> 4 = aggiornamento (i valori di colonna acquisiti sono quelli successivi all'aggiornamento).|  
|**__$update_mask**|**varbinary(128)**|Maschera di bit in cui a ogni colonna acquisita identificata per l'istanza di acquisizione corrisponde un bit. Tutti i bit definiti per questo valore sono impostati su 1 quando **_ _ $ operation** = 1 o 2. Quando **_ _ $ operation** = 3 o 4, solo i bit corrispondenti alle colonne modificate sono impostati su 1.|  
|**\<captured source table columns>**|variabile|Le colonne rimanenti restituite dalla funzione sono le colonne acquisite identificate quando l'istanza di acquisizione è stata creata. Se nessuna colonna è stata specificata nell'elenco delle colonne acquisite, vengono restituite tutte le colonne nella tabella di origine.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** o **db_owner** ruolo predefinito del database. Per tutti gli altri utenti, è richiesta l'autorizzazione SELECT su tutte le colonne acquisite nella tabella di origine e, se è stato definito un ruolo di controllo per l'istanza di acquisizione, l'appartenenza a tale ruolo del database. Quando il chiamante non dispone dell'autorizzazione per visualizzare i dati di origine, la funzione restituisce l'errore 229 ("autorizzazione SELECT negata per l'oggetto ' fn_cdc_get_all_changes_.. .', database ' \<DatabaseName> ', schema ' CDC '").  
  
## <a name="remarks"></a>Osservazioni  
 Se l'intervallo LSN specificato è esterno alla cronologia di rilevamento delle modifiche per l'istanza di acquisizione, la funzione restituisce l'errore 208 ("Numero di argomenti insufficienti per la procedura o la funzione cdc.fn_cdc_get_all_changes").  
  
 Alle colonne con tipo di dati **Image**, **Text** e **ntext** viene sempre assegnato un valore null quando **_ _ $ operation** = 1 o **_ _ $ operation** = 3. Alle colonne con tipo di dati **varbinary (max)**, **varchar (max)** o **nvarchar (max)** viene assegnato un valore null quando **_ _ $ operation** = 3 a meno che la colonna non sia stata modificata durante l'aggiornamento. Quando **_ _ $ operation** = 1, a queste colonne viene assegnato il relativo valore al momento dell'eliminazione. Le colonne calcolate incluse in un'istanza di acquisizione hanno sempre un valore NULL.  
  
## <a name="examples"></a>Esempi  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]Sono disponibili diversi modelli che illustrano come usare le funzioni di query Change Data Capture. Questi modelli sono disponibili nel menu **Visualizza** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Per altre informazioni, vedere [Esplora modelli](../../ssms/template/template-explorer.md).  
  
 In questo esempio viene illustrato il `Enumerate All Changes for Valid Range Template`. Viene utilizzata la funzione `cdc.fn_cdc_get_all_changes_HR_Department` per riportare tutte le modifiche attualmente disponibili per l'istanza di acquisizione `HR_Department`, definita per tabella di origine HumanResources.Department nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
-- ========  
-- Enumerate All Changes for Valid Range Template  
-- ========  
USE AdventureWorks2012;  
GO  
  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HR_Department');  
SET @to_lsn   = sys.fn_cdc_get_max_lsn();  
SELECT * FROM cdc.fn_cdc_get_all_changes_HR_Department  
  (@from_lsn, @to_lsn, N'all');  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)   
 [sys.sp_cdc_get_captured_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
