---
description: sp_fulltext_table (Transact-SQL)
title: sp_fulltext_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18f807181005e20e94a15a8c4c95b43e82ecbb44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211324"
---
# <a name="sp_fulltext_table-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Contrassegna una tabella per l'indicizzazione full-text oppure elimina tale contrassegno.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilizzare in alternativa [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)e [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tabname = ] 'qualified_table_name'` È un nome di tabella in una o due parti. La tabella deve esistere nel database corrente *qualified_table_name* è di **tipo nvarchar (517)** e non prevede alcun valore predefinito.  
  
`[ @action = ] 'action'` Azione da eseguire. *Action* è di **tipo nvarchar (50)** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**Creare**|Crea i metadati per un indice full-text per la tabella a cui fa riferimento *qualified_table_name* e specifica che i dati dell'indice full-text per questa tabella devono risiedere nella *fulltext_catalog_name*. Questa azione indica inoltre l'utilizzo di *unique_index_name* come colonna chiave full-text. Questo indice univoco deve essere già presente e definito in una colonna della tabella.<br /><br /> Nella tabella sarà possibile eseguire una ricerca full-text solo dopo il popolamento del catalogo full-text.|  
|**Goccia**|Elimina i metadati nell'indice full-text per *qualified_table_name*. Se l'indice full-text è attivo, viene disattivato automaticamente prima dell'eliminazione. Non è necessario rimuovere le colonne prima di eliminare l'indice full-text.|  
|**Attiva**|Attiva la possibilità di raccogliere i dati dell'indice full-text per *qualified_table_name*, dopo che sono stati disattivati. Per poter attivare un indice full-text, è necessario che l'indice includa almeno una colonna.<br /><br /> Un indice full-text viene attivato automaticamente per il popolamento non appena si aggiunge la prima colonna per l'indicizzazione. Se si elimina l'ultima colonna dell'indice, l'indice diventa inattivo. Se il rilevamento delle modifiche è attivato, l'attivazione di un indice non attivo comporta l'avvio di un nuovo processo di popolamento.<br /><br /> Si noti che in questo modo l'indice full-text non viene popolato, ma viene semplicemente registrata la tabella nel catalogo full-text del file system in modo che sia possibile recuperare le righe da *qualified_table_name* durante il popolamento dell'indice full-text successivo.|  
|**Disattivare**|Disattiva l'indice full-text per *qualified_table_name* in modo che non sia più possibile raccogliere i dati dell'indice full-text per l' *qualified_table_name*. I metadati dell'indice full-text tuttavia vengono conservati e la tabella può essere riattivata.<br /><br /> Se il rilevamento delle modifiche è attivato, la disattivazione di un indice attivo comporta il blocco dello stato dell'indice, ovvero vengono arrestati i processi di popolamento in corso e la distribuzione delle modifiche nell'indice.|  
|**start_change_tracking**|Avvia un popolamento incrementale dell'indice full-text. Se la tabella non include una colonna di tipo timestamp, viene avviato un popolamento completo dell'indice full-text e il rilevamento delle modifiche apportate alla tabella.<br /><br /> Il rilevamento delle modifiche full-text non tiene traccia delle operazioni WRITETEXT o UPDATETEXT eseguite su colonne con indicizzazione full-text di tipo **Image**, **Text** o **ntext**.|  
|**stop_change_tracking**|Arresta il rilevamento delle modifiche apportate alla tabella.|  
|**update_index**|Propaga nell'indice full-text il set di modifiche rilevate.|  
|**Start_background_updateindex**|Avvia la propagazione delle modifiche rilevate nell'indice full-text.|  
|**Stop_background_updateindex**|Arresta la propagazione delle modifiche rilevate nell'indice full-text.|  
|**start_full**|Avvia un popolamento completo dell'indice full-text per la tabella.|  
|**start_incremental**|Avvia un popolamento incrementale dell'indice full-text per la tabella.|  
|**Stop**|Arresta un popolamento completo o incrementale.|  
  
`[ @ftcat = ] 'fulltext_catalog_name'` Nome del catalogo full-text esistente valido per un'azione **create** . Per tutte le altre azioni questo parametro deve essere NULL. *fulltext_catalog_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @keyname = ] 'unique_index_name'` È una colonna a chiave singola valida, ovvero un indice non null univoco per *qualified_table_name* per un'azione **create** . Per tutte le altre azioni questo parametro deve essere NULL. *unique_index_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Dopo la disattivazione di un indice full-text per una determinata tabella, l'indice full-text esistente rimane attivo fino al popolamento completo successivo. Questo indice non viene tuttavia utilizzato perché [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] blocca le query sulle tabelle disattivate.  
  
 Se la tabella viene riattivata e l'indice non viene ripopolato, l'indice precedente rimane disponibile per le query nelle colonne full-text attivate rimanenti, escluse quelle nuove. I dati provenienti da colonne eliminate vengono recuperati nelle query che specificano una ricerca su tutte le colonne full-text.  
  
 Dopo aver definito una tabella per l'indicizzazione full-text, il passaggio della colonna chiave univoca full-text da un tipo di dati a un altro modificando il tipo di dati della colonna o modificando la chiave univoca full-text da una colonna a un'altra, senza un ripopolamento completo, potrebbe verificarsi un errore durante una query successiva e viene restituito il messaggio di errore: *"* Impossibile eseguire la conversione nel tipo data_type *per* il valore della chiave di key_value Per evitare questo problema, eliminare la definizione full-text per questa tabella utilizzando l'azione di **rilascio** di **sp_fulltext_table** e ridefinirla utilizzando **sp_fulltext_table** e **sp_fulltext_column**.  
  
 Il valore massimo definito per le dimensioni della colonna chiave full-text deve essere 900 byte. È consigliabile che le dimensioni della colonna chiave siano ridotte al massimo per motivi di prestazioni.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** , **db_owner** e **db_ddladmin** ruoli predefiniti del database o un utente con autorizzazioni di riferimento sul catalogo full-text possono eseguire **sp_fulltext_table**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>R. Abilitazione di una tabella per l'indicizzazione full-text  
 Nell'esempio seguente vengono creati i metadati di un indice full-text per la tabella `Document` del database `AdventureWorks`. `Cat_Desc` è un catalogo full-text. `PK_Document_DocumentID` è un indice univoco a colonna singola nella tabella `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>B. Attivazione e propagazione delle modifiche rilevate  
 Nell'esempio seguente viene attivata e avviata la propagazione delle modifiche rilevate nell'indice full-text, man mano che si verificano.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>C. Rimozione di un indice full-text  
 In questo esempio vengono rimossi i metadati di un indice full-text per la tabella `Document` del database `AdventureWorks`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
