---
description: sys.sp_cdc_help_change_data_capture (Transact-SQL)
title: sys.sp_cdc_help_change_data_capture (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_help_change_data_capture_TSQL
- sys.sp_cdc_help_change_data_capture_TSQL
- sp_cdc_help_change_data_capture
- sys.sp_cdc_help_change_data_capture
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], querying metadata
- sys.sp_cdc_help_change_data_capture
- sp_cdc_help_change_data_capture
ms.assetid: 91fd41f5-1b4d-44fe-a3b5-b73eff65a534
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 79c12ab4b1bdfb3a955c83ef3d724a026f24f1ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159777"
---
# <a name="syssp_cdc_help_change_data_capture-transact-sql"></a>sys.sp_cdc_help_change_data_capture (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce la configurazione dell'acquisizione dei dati delle modifiche per ogni tabella abilitata per la modifica dell'acquisizione di dati nel database corrente. Possono essere restituite fino a due righe per ogni tabella di origine, una riga per ogni istanza di acquisizione. Change Data Capture non è disponibile in tutte le edizioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.sp_cdc_help_change_data_capture   
  [ [ @source_schema = ] 'source_schema' ]  
  [, [ @source_name = ] 'source_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @source_schema =]'*source_schema*'  
 Nome dello schema a cui appartiene la tabella di origine. *source_schema* è di **tipo sysname** e il valore predefinito è null. Quando si specifica *source_schema* , è necessario specificare anche *source_name* .  
  
 Se è diverso da NULL, *source_schema* deve esistere nel database corrente.  
  
 Se *source_schema* è diverso da null, anche *source_name* deve essere non null.  
  
 [ @source_name =]'*source_name*'  
 Nome della tabella di origine. *source_name* è di **tipo sysname** e il valore predefinito è null. Quando si specifica *source_name* , è necessario specificare anche *source_schema* .  
  
 Se è diverso da NULL, *source_name* deve esistere nel database corrente.  
  
 Se *source_name* è diverso da null, anche *source_schema* deve essere non null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|source_schema|**sysname**|Nome dello schema della tabella di origine.|  
|source_table|**sysname**|Nome della tabella di origine.|  
|capture_instance|**sysname**|Nome dell'istanza di acquisizione.|  
|object_id|**int**|ID della tabella delle modifiche associata alla tabella di origine.|  
|source_object_id|**int**|ID della tabella di origine.|  
|start_lsn|**binary(10)**|Numero di sequenza del file di log (LSN) che rappresenta l'endpoint inferiore per l'esecuzione di query sulla tabella delle modifiche.<br /><br /> NULL = l'endpoint inferiore non è stato stabilito.|  
|end_lsn|**binary(10)**|Il numero LSN rappresenta l'endpoint superiore per l'esecuzione di query sulla tabella delle modifiche. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] questa colonna è sempre a NULL.|  
|supports_net_changes|**bit**|Il supporto delle modifiche totali è abilitato.|  
|has_drop_pending|**bit**|Non utilizzato in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].|  
|role_name|**sysname**|Nome del ruolo del database utilizzato per controllare l'accesso ai dati delle modifiche.<br /><br /> NULL = non è utilizzato un ruolo.|  
|index_name|**sysname**|Nome dell'indice utilizzato per identificare in modo univoco le righe nella tabella di origine.|  
|filegroup_name|**sysname**|Nome del database filegroup contenente la tabella delle modifiche specificata.<br /><br /> NULL = la tabella delle modifiche si trova nel filegroup predefinito del database.|  
|create_date|**datetime**|Data in cui l'istanza di acquisizione è stata abilitata.|  
|index_column_list|**nvarchar(max)**|Elenco delle colonne dell'indice utilizzato per identificare in modo univoco le righe nella tabella di origine.|  
|captured_column_list|**nvarchar(max)**|Elenco delle colonne di origine acquisite.|  
  
## <a name="remarks"></a>Commenti  
 Quando sia *source_schema* che *source_name* il valore predefinito è null o viene impostato in modo esplicito su null, questo stored procedure restituisce informazioni per tutte le istanze di acquisizione del database a cui il chiamante ha accesso SELECT. Quando *source_schema* e *SOURCE_NAME* sono non null, vengono restituite solo le informazioni sulla tabella abilitata specificata specifica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Quando *source_schema* e *source_name* sono null, l'autorizzazione del chiamante determina quali tabelle abilitate sono incluse nel set di risultati. I chiamanti devono disporre dell'autorizzazione SELECT in tutte le colonne acquisite dell'istanza di acquisizione nonché dell'appartenenza a qualsiasi ruolo di controllo definito per le informazioni di tabella da includere. I membri del ruolo del database db_owner possono visualizzare le informazioni su tutte le istanze di acquisizione definite. Se vengono richieste informazioni per una tabella abilitata specifica, alla tabella denominata vengono applicati gli stessi criteri SELECT e di appartenenza.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-change-data-capture-configuration-information-for-a-specified-table"></a>R. Restituzione delle informazioni di configurazione di Change Data Capture per una tabella specifica  
 L'esempio seguente restituisce la configurazione dell'acquisizione dei dati delle modifiche per la tabella `HumanResources.Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_help_change_data_capture   
    @source_schema = N'HumanResources',   
    @source_name = N'Employee';  
GO  
```  
  
### <a name="b-returning-change-data-capture-configuration-information-for-all-tables"></a>B. Restituzione delle informazioni di configurazione di Change Data Capture per tutte le tabelle  
 Nell'esempio seguente vengono restituite le informazioni di configurazione per tutte le tabelle abilitate nel database contenenti dati delle modifiche a cui il chiamante è autorizzato ad accedere.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_help_change_data_capture;  
GO  
```  
  
