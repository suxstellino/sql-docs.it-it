---
description: CDC. &lt; capture_instance &gt; _CT (Transact-SQL)
title: CDC. &lt; capture_instance &gt; _CT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc
- cdc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.<capture_instance>_CT
ms.assetid: 979c8110-3c54-4e76-953c-777194bc9751
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7fa737644611f24d9d0858fd04066d3ba0571ee3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102728"
---
# <a name="cdcltcapture_instancegt_ct-transact-sql"></a>CDC. &lt; capture_instance &gt; _CT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Tabella delle modiche creata quando Change Data Capture è abilitato in una tabella di origine. La tabella restituisce una riga per ogni operazione di inserimento ed eliminazione eseguita nella tabella di origine e due righe per ogni operazione di aggiornamento eseguita nella tabella di origine. Se non viene specificato al momento dell'abilitazione della tabella di origine, il nome della tabella delle modifiche viene derivato. Il formato del nome è CDC. *capture_instance* _CT dove *capture_instance* è il nome dello schema della tabella di origine e il nome della tabella di origine nel formato *schema_table*. Se, ad esempio, la tabella **Person. Address** nel database di esempio **AdventureWorks** è abilitata per Change Data Capture, il nome della tabella delle modifiche derivata sarà **CDC.Person_Address_CT**.  
  
 Si consiglia di **non eseguire una query direttamente sulle tabelle di sistema**. Eseguire invece il [cdc.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) e CDC.fn_cdc_get_net_changes_<capture_instance le funzioni [di>.](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)  
  

  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|Numero di sequenza del file di log (LSN) associato alla transazione commit per la modifica.<br /><br /> Tutte le modifiche di cui è stato eseguito il commit nella stessa transazione condividono lo stesso valore LSN di commit. Se, ad esempio, un'operazione di eliminazione nella tabella di origine rimuove due righe, la tabella delle modifiche conterrà due righe, ognuna con lo stesso valore **_ _ $ start_lsn** .|  
|**_ _ $ end_lsn**|**binary(10)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] questa colonna è sempre a NULL.|  
|**__$seqval**|**binary(10)**|Valore di sequenza utilizzato per ordinare le modifiche a una riga all'interno di una transazione.|  
|**__$operation**|**int**|Identifica l'operazione DML (Data Manipulation Language) associata alla modifica. Può essere uno dei valori seguenti:<br /><br /> 1 = eliminazione<br /><br /> 2 = inserimento<br /><br /> 3 = aggiornamento (valori obsoleti)<br /><br /> I dati della colonna includono valori di riga prima dell'esecuzione dell'istruzione di aggiornamento.<br /><br /> 4 = aggiornamento (valori nuovi)<br /><br /> I dati della colonna includono valori di riga dopo l'esecuzione dell'istruzione di aggiornamento.|  
|**__$update_mask**|**varbinary(128)**|Maschera di bit basata su numeri ordinali di colonna della tabella delle modifiche che identificano le colonne modificate.|  
|*\<captured source table columns>*|variabile|Le colonne rimanenti della tabella delle modifiche sono le colonne della tabella di origine identificate come colonne acquisite durante la creazione dell'istanza di acquisizione. Se non è stata specificata alcuna colonna nell'elenco delle colonne acquisite, tutte le colonne della tabella di origine vengono incluse in questa tabella.|  
|**_ _ $ command_id** |**int** |Tiene traccia dell'ordine delle operazioni all'interno di una transazione. |  
  
## <a name="remarks"></a>Osservazioni  

La colonna colonna `__$command_id` was è stata introdotta in un aggiornamento cumulativo nelle versioni da 2012 a 2016. Per informazioni sulla versione e sul download, vedere l'articolo [della Knowledge base 3030352 alla correzione: la tabella delle modifiche è ordinata in modo errato per le righe aggiornate dopo aver abilitato Change Data Capture per un database Microsoft SQL Server](https://support.microsoft.com/help/3030352/fix-the-change-table-is-ordered-incorrectly-for-updated-rows-after-you).  Per ulteriori informazioni, vedere la pagina relativa alla [funzionalità CDC potrebbe interrompersi dopo l'aggiornamento alla versione più recente di cu per SQL Server 2012, 2014 e 2016](/archive/blogs/sql_server_team/cdc-functionality-may-break-after-upgrading-to-the-latest-cu-for-sql-server-2012-2014-and-2016).

## <a name="captured-column-data-types"></a>Tipi di dati delle colonne acquisite  
 Le colonne acquisite incluse in questa tabella hanno lo stesso tipo di dati e valore delle corrispondenti colonne di origine, con le seguenti eccezioni:  
  
-   Le colonne **timestamp** sono definite come **Binary (8)**.  
  
-   Le colonne **Identity** sono definite come **int** o **bigint**.  
  
 Tuttavia, i valori in queste colonne sono uguali ai valori della colonna di origine.  
  
### <a name="large-object-data-types"></a>Tipi di dati per oggetti di grandi dimensioni:  
 Alle colonne con tipo di dati **Image**, **Text** e **ntext** viene sempre assegnato un valore **null** quando _ _ $ operation = 1 o \_ \_ $Operation = 3. Alle colonne con tipo di dati **varbinary (max)**, **varchar (max)** o **nvarchar (max)** viene assegnato un valore **null** quando \_ \_ $Operation = 3 a meno che la colonna non sia stata modificata durante l'aggiornamento. Quando \_ \_ $Operation = 1, a queste colonne viene assegnato il relativo valore al momento dell'eliminazione. Le colonne calcolate incluse in un'istanza di acquisizione hanno sempre un valore **null**.  
  
 Per impostazione predefinita, la dimensione massima che può essere aggiunta a una colonna acquista in una singola istruzione INSERT, UPDATE, WRITETEXT o UPDATETEXT è pari a 65.536 byte o 64 KB Per aumentare questa dimensione per supportare dati LOB di dimensioni maggiori, usare l' [opzione di configurazione del server Configure max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) per specificare una dimensione massima maggiore. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md).  
  
## <a name="data-definition-language-modifications"></a>Modifiche DDL  
 Le modifiche DDL apportate alla tabella di origine, ad esempio l'aggiunta o l'eliminazione di colonne, vengono registrate nella tabella [CDC.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) . Queste modifiche non sono applicate alla tabella delle modifiche, ossia la definizione della tabella delle modifiche rimane costante. Nel caso di inserimento di righe nella tabella delle modifiche, il processo di acquisizione ignora le colonne che non sono visualizzate nell'elenco di colonne acquisite associate alla tabella di origine. Se una colonna è visualizzata nell'elenco di colonne acquisite che non compare più nella tabella di origine, alla colonna viene assegnato un valore Null.  
  
 Anche la modifica del tipo di dati di una colonna nella tabella di origine viene registrata nella tabella [CDC.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md) . Tuttavia, questa modifica non altera la definizione della tabella delle modifiche. Il tipo di dati della colonna acquisita nella tabella delle modifiche viene modificato quando il processo di acquisizione incontra il record del log per la modifica DDL apportata alla tabella di origine.  
  
 Se è necessario modificare il tipo di dati di una colonna acquisita nella tabella di origine in una modalità che decresce la dimensione del tipo di dati, utilizzare la procedura descritta di seguito per assicurasi che la colonna equivalente nella tabella delle modifiche possa essere modificata correttamente.  
  
1.  Nella tabella di origine, aggiornare i valori nella colonna da modificare per adattarli nella dimensione del tipo di dati pianificata. Se ad esempio si modifica il tipo di dati da **int** a **smallint**, aggiornare i valori in base a una dimensione che rientra nell'intervallo **smallint** , da-32.768 a 32.767.  
  
2.  Nella tabella delle modifiche, eseguire la stessa operazione di aggiornamento alla colonna equivalente.  
  
3.  Modificare la tabella di origine specificando il nuovo tipo di dati. La modifica del tipo di dati è propagata correttamente alla tabella delle modifiche.  

## <a name="data-manipulation-language-modifications"></a>Modifiche DML  
 Quando su una tabella di origine abilitata per Change Data Capture vengono eseguite operazioni di inserimento, aggiornamento ed eliminazione, nel log delle transazioni del database viene visualizzato un record relativo a tali operazioni DML. Il processo Change Data Capture recupera le informazioni relative a tali modifiche dal log delle transazioni e aggiunge una o due righe alla tabella delle modifiche per registrare la modifica. Le voci vengono aggiunte alla tabella delle modifiche nello stesso ordine in cui è stato eseguito il relativo commit nella tabella di origine, anche se il commit delle voci della tabella delle modifiche deve essere eseguito in genere per un gruppo di modifiche anziché per una voce singola.  
  
 All'interno della voce della tabella delle modifiche, la colonna **_ _ $ start_lsn** viene utilizzata per registrare il numero LSN di commit associato alla modifica alla tabella di origine e la **colonna _ _ $ seqval** viene utilizzata per ordinare la modifica all'interno della relativa transazione. Nel loro complesso queste colonne di metadati possono essere utilizzate per garantire che venga mantenuto l'ordine di commit delle modifiche di origine. Poiché il processo di acquisizione ottiene le informazioni relative alle modifiche dal log delle transazioni, è importante notare che le voci della tabella delle modifiche non vengono visualizzate in modo sincrono con le modifiche della tabella di origine corrispondenti. Al contrario, le modifiche corrispondenti vengono visualizzate in modo asincrono dopo che il processo di acquisizione ha elaborato le voci di modifica attinenti dal log delle transazioni.  
  
 Per le operazioni di inserimento ed eliminazione, sono impostati tutti i bit della maschera di aggiornamento. Per le operazioni di aggiornamento, la maschera di aggiornamento sia nelle vecchie righe aggiornate obsolete che nelle righe di nuovo aggiornamento sarà modificata per riflettere le colonne modificate durante l'aggiornamento.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_enable_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)  
  
