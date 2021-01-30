---
description: sys.sp_cdc_get_captured_columns (Transact-SQL)
title: sys.sp_cdc_get_captured_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_get_captured_columns
- sys.sp_cdc_get_captured_columns
- sys.sp_cdc_get_captured_columns_TSQL
- sp_cdc_get_captured_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_get_captured_columns
- sp_cdc_get_captured_columns
- change data capture [SQL Server], querying metadata
ms.assetid: d9e680be-ab9b-4e0c-b63a-90658f241df8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 90476faf2182ad92259d85dd97cc900153ec53c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205967"
---
# <a name="syssp_cdc_get_captured_columns-transact-sql"></a>sys.sp_cdc_get_captured_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce le informazioni sui metadati di Change Data Capture per le colonne di origine acquisite registrate dall'istanza di acquisizione specificata. Change Data Capture non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_get_captured_columns   
    [ @capture_instance = ] 'capture_instance'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @capture_instance =]'*capture_instance*'  
 Nome dell'istanza di acquisizione associata a una tabella di origine. *capture_instance* è di **tipo sysname** e non può essere null.  
  
 Per creare un report sulle istanze di acquisizione per la tabella, eseguire il [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) stored procedure.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nome dello schema della tabella di origine.|  
|source_table|**sysname**|Nome della tabella di origine.|  
|capture_instance|**sysname**|Nome dell'istanza di acquisizione.|  
|column_name|**sysname**|Nome della colonna di origine acquisita.|  
|column_id|**int**|ID della colonna della tabella di origine.|  
|column_ordinal|**int**|Posizione della colonna all'interno della tabella di origine.|  
|data_type|**sysname**|Tipo di dati della colonna.|  
|character_maximum_length|**int**|Lunghezza massima in caratteri della colonna se questa è basata su caratteri. In caso contrario il valore è NULL.|  
|numeric_precision|**tinyint**|Precisione della colonna se questa è basata su valori numerici. In caso contrario il valore è NULL.|  
|numeric_precision_radix|**smallint**|Radice di precisione della colonna se questa è basata su valori numerici. In caso contrario il valore è NULL.|  
|numeric_scale|**int**|Scala della colonna se questa è basata su valori numerici. In caso contrario il valore è NULL.|  
|datetime_precision|**smallint**|Precisione della colonna se questa è basata su valori datetime. In caso contrario il valore è NULL.|  
  
## <a name="remarks"></a>Commenti  
 Utilizzare sys.sp_cdc_get_captured_columns per ottenere informazioni sulle colonne acquisite restituite eseguendo una query sulle funzioni di query dell'istanza di acquisizione [cdc.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) [o CDC.fn_cdc_get_net_changes_<capture_instance>. ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md) I nomi di colonna, gli ID e la posizione rimangono costanti per tutta la durata dell'istanza di acquisizione. Solo il tipo di dati delle colonne cambia quando cambia il tipo di dati delle colonne di origine sottostanti nella tabella registrata. Le colonne aggiunte o eliminate da una tabella di origine non influiscono sulle colonne acquisite di istanze di acquisizione esistenti.  
  
 Utilizzare [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md) per ottenere informazioni sulle istruzioni Data Definition Language (DDL) applicate a una tabella di origine. Le modifiche DDL che influiscono sulla struttura di una colonna di origine registrata vengono restituite nel set di risultati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner. Per tutti gli altri utenti, è richiesta l'autorizzazione SELECT su tutte le colonne acquisite nella tabella di origine e, se è stato definito un ruolo di controllo per l'istanza di acquisizione, l'appartenenza a tale ruolo del database. Se il chiamante non dispone delle autorizzazioni per visualizzare i dati di origine, la funzione restituisce l'errore 22981 (Oggetto inesistente o accesso negato).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente sono restituite le informazioni sulle colonne acquisite nell'istanza di acquisizione `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_get_captured_columns   
    @capture_instance = N'HumanResources_Employee';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)  
  
