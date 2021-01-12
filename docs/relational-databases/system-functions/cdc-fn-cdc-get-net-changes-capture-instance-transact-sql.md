---
description: cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL)
title: cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_net_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_net_changes_<capture_instance>
ms.assetid: 43ab0d1b-ead4-471c-85f3-f6c4b9372aab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7dee462b6b4a66b61d790bb06673d36f0907ef73
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099705"
---
# <a name="cdcfn_cdc_get_net_changes_ltcapture_instancegt-transact-sql"></a>cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga di modifica NET per ogni riga di origine modificata nell'intervallo di numeri di sequenza del file di log (LSN) specificato.  
  
 **Attendere, che cos'è un LSN?** Ogni record presente nel [log delle transazioni SQL Server](../logs/the-transaction-log-sql-server.md) viene identificato in modo univoco da un numero di sequenza del file di log (LSN). LSN sono ordinati in modo tale che se LSN2 è maggiore di LSN1, la modifica descritta dal record di log a cui fa riferimento LSN2 si è verificata **dopo** la modifica descritta dall'LSN del record di log.  
  
 Il numero LSN di un record di log in cui si è verificato un evento significativo può essere utile per la creazione di sequenze di ripristino corrette. Poiché LSN sono ordinati, è possibile confrontarli per verificarne l'uguaglianza e la disuguaglianza, ovvero, \<, > =, \<=, > =. Questi confronti sono utili nella creazione di sequenze di ripristino.  
  
 Quando una riga di origine presenta più modifiche durante l'intervallo LSN, una singola riga che riflette il contenuto finale della riga viene restituita dalla funzione di enumerazione descritta di seguito. Se, ad esempio, una transazione inserisce una riga nella tabella di origine e una transazione successiva nell'intervallo LSN aggiorna una o più colonne della riga, la funzione **restituisce una sola riga,** che include i valori di colonna aggiornati.  
  
 Questa funzione di enumerazione viene creata nel momento in cui una tabella di origine è abilitata per l'acquisizione dei dati delle modifiche e quando viene specificato il rilevamento delle modifiche nette. Per abilitare il rilevamento delle modifiche nette, è necessario che la tabella di origine disponga di una chiave primaria o un indice univoco. Il nome della funzione è derivato e utilizza il formato cdc.fn_cdc_get_net_changes_ *capture_instance*, dove *capture_instance* è il valore specificato per l'istanza di acquisizione quando la tabella di origine è stata abilitata per la Change Data Capture. Per ulteriori informazioni, vedere [sys.sp_cdc_enable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
cdc.fn_cdc_get_net_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all with mask  
 | all with merge  
}  
```  
  
## <a name="arguments"></a>Argomenti  
 *from_lsn*  
 Il numero LSN che rappresenta l'endpoint inferiore dell'intervallo LSN da includere nel set di risultati. *from_lsn* è **binario (10)**.  
  
 Nel set di risultati vengono incluse solo le righe di [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) tabella delle modifiche con un valore in _ _ $ start_lsn maggiore o uguale a *from_lsn* .  
  
 *to_lsn*  
 Il numero LSN che rappresenta l'endpoint superiore dell'intervallo LSN da includere nel set di risultati. *to_lsn* è **binario (10)**.  
  
 Solo le righe del [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) tabella delle modifiche con un valore in _ _ $ start_lsn minore o uguale a *from_lsn* o uguale a *to_lsn* sono incluse nel set di risultati.  
  
 *<row_filter_option>* :: = {All | All con mask | all con merge}  
 Opzione applicata al contenuto delle colonne dei metadati e alle righe restituite nel set di risultati. Le opzioni possibili sono le seguenti:  
  
 all  
 Restituisce il numero LSN della modifica finale alla riga e l'operazione necessaria per applicare la riga nelle colonne di metadati _ _ $ start_lsn e \_ \_ $Operation. La colonna \_ \_ $Update _mask è sempre null.  
  
 all with mask  
 Restituisce il numero LSN della modifica finale alla riga e l'operazione necessaria per applicare la riga nelle colonne di metadati _ _ $ start_lsn e \_ \_ $Operation. Inoltre, quando un'operazione di aggiornamento restituisce ( \_ \_ $Operation = 4) le colonne acquisite modificate nell'aggiornamento sono contrassegnate nel valore restituito in \_ \_ $Update _mask.  
  
 all with merge  
 Restituisce il numero LSN della modifica finale alla riga nelle colonne metadati __$start_lsn. La colonna \_ \_ $Operation sarà uno dei due valori seguenti: 1 per Delete e 5 per indicare che l'operazione necessaria per applicare la modifica è un inserimento o un aggiornamento. La colonna \_ \_ $Update _mask è sempre null.  
  
 La logica necessaria a determinare l'operazione precisa per una determinata modifica aggiunge maggiore complessità alla query, pertanto questa opzione è progettata per migliorare le prestazioni di esecuzione delle query quando è sufficiente indicare che l'operazione necessaria ad applicare i dati di modifica è un inserimento o un aggiornamento, ma non è necessario distinguere in modo esplicito tra i due. Questa opzione è molto interessante negli ambienti di destinazione dove è direttamente disponibile un'operazione di unione, ad esempio un ambiente [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|__$start_lsn|**binary(10)**|Valore LSN associato al commit della transazione per la modifica.<br /><br /> Tutte le modifiche di cui è stato eseguito il commit nella stessa transazione condividono lo stesso valore LSN di commit. Se, ad esempio, un'operazione di aggiornamento nella tabella di origine modifica due colonne in due righe, la tabella delle modifiche conterrà quattro righe, ognuna con lo stesso _ _ $ start_lsnvalue.|  
|__$operation|**int**|Identifica l'operazione DML (Data Manipulation Language) necessaria per applicare la riga di dati di modifica all'origine dati di destinazione.<br /><br /> Se il valore del parametro row_filter_option è 'all' oppure 'all with mask', il valore in questa colonna può essere uno dei seguenti:<br /><br /> 1 = eliminazione<br /><br /> 2 = inserimento<br /><br /> 4 = aggiornamento<br /><br /> Se il valore del parametro row_filter_option è 'all with merge', il valore in questa colonna può essere uno dei seguenti:<br /><br /> 1 = eliminazione|  
|__$update_mask|**varbinary(128)**|Maschera di bit in cui a ogni colonna acquisita identificata per l'istanza di acquisizione corrisponde un bit. Tutti i bit definiti per questo valore sono impostati su 1 quando __$operation = 1 o 2. Quando \_ \_ $Operation = 3 o 4, solo i bit corrispondenti alle colonne modificate sono impostati su 1.|  
|*\<captured source table columns>*|variabile|Le colonne rimanenti restituite dalla funzione sono le colonne della tabella di origine identificate come colonne acquisite durante la creazione dell'istanza di acquisizione. Se nessuna colonna è stata specificata nell'elenco delle colonne acquisite, vengono restituite tutte le colonne nella tabella di origine.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server sysadmin o al ruolo predefinito del database db_owner. Per tutti gli altri utenti, è richiesta l'autorizzazione SELECT su tutte le colonne acquisite nella tabella di origine e, se è stato definito un ruolo di controllo per l'istanza di acquisizione, l'appartenenza a tale ruolo del database. Se il chiamante non dispone delle autorizzazioni per visualizzare i dati di origine, la funzione restituisce un errore 208 (Il nome di oggetto non è valido).  
  
## <a name="remarks"></a>Osservazioni  
 Se l'intervallo LSN specificato è esterno alla cronologia di rilevamento delle modifiche per l'istanza di acquisizione, la funzione restituisce un errore 208, in cui è indicato che il nome di oggetto non è valido.

 Le modifiche apportate all'identificatore univoco di una riga comfn_cdc_get_net_changes porteranno la visualizzazione del comando di aggiornamento iniziale con un comando DELETE e then INSERT.  Questo comportamento è necessario per tenere traccia della chiave prima e dopo la modifica.
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene utilizzata la funzione `cdc.fn_cdc_get_net_changes_HR_Department` per segnalare le modifiche nette apportate alla tabella di origine `HumanResources.Department` durante un intervallo di tempo specifico.  
  
 Innanzitutto, per contrassegnare l'inizio dell'intervallo di tempo viene utilizzata la funzione `GETDATE`. Dopo l'applicazione delle istruzioni DML alla tabella di origine, la funzione `GETDATE` viene chiamata nuovamente per identificare la fine dell'intervallo di tempo. La funzione [sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) viene quindi utilizzata per eseguire il mapping dell'intervallo di tempo a un intervallo di query Change Data Capture delimitato da valori LSN. Infine, la funzione `cdc.fn_cdc_get_net_changes_HR_Department` viene eseguita per ottenere tutte le modifiche alla tabella di origine per l'intervallo di tempo. La riga inserita ed eliminata non viene visualizzata nel set dei risultati restituito dalla funzione. Ciò avviene perché una riga aggiunta ed eliminata all'interno di una finestra di query non produce modifiche totali sulla tabella di origine per l'intervallo. Prima di eseguire questo esempio, è necessario prima eseguire l'esempio B in [sys.sp_cdc_enable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @begin_time datetime, @end_time datetime, @from_lsn binary(10), @to_lsn binary(10);  
-- Obtain the beginning of the time interval.  
SET @begin_time = GETDATE() -1;  
-- DML statements to produce changes in the HumanResources.Department table.  
INSERT INTO HumanResources.Department (Name, GroupName)  
VALUES (N'MyDept', N'MyNewGroup');  
  
UPDATE HumanResources.Department  
SET GroupName = N'Resource Control'  
WHERE GroupName = N'Inventory Management';  
  
DELETE FROM HumanResources.Department  
WHERE Name = N'MyDept';  
  
-- Obtain the end of the time interval.  
SET @end_time = GETDATE();  
-- Map the time interval to a change data capture query range.  
SET @from_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than or equal', @begin_time);  
SET @to_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);  
  
-- Return the net changes occurring within the query window.  
SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@from_lsn, @to_lsn, 'all');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [Informazioni su Change Data Capture &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
