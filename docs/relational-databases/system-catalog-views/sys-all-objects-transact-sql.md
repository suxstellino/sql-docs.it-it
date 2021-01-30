---
description: sys.all_objects (Transact-SQL)
title: sys.all_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.all_objects
- all_objects_TSQL
- all_objects
- sys.all_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_objects catalog view
ms.assetid: 547e4be4-a8e4-48ce-9d8d-37b169985081
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d6d4656dc6db736c319b15e8b02cfbc859741a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209046"
---
# <a name="sysall_objects-transact-sql"></a>sys.all_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Visualizza l'operatore UNION di tutti gli oggetti definiti dall'utente a livello di ambito di schema e gli oggetti di sistema.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nome dell'oggetto.|  
|object_id|**int**|Numero di identificazione dell'oggetto. Valore univoco all'interno di un database.|  
|principal_id|**int**|ID del singolo proprietario se diverso dal proprietario dello schema. Per impostazione predefinita, gli oggetti contenuti nello schema appartengono al proprietario dello schema stesso. È tuttavia possibile specificare un altro proprietario tramite l'istruzione ALTER AUTHORIZATION per modificare la proprietà.<br /><br /> NULL se non esiste alcun proprietario alternativo.<br /><br /> È NULL se il tipo di oggetto è uno dei seguenti:<br /><br /> C = vincolo CHECK<br /><br /> D = DEFAULT (vincolo o valore autonomo)<br /><br /> F = vincolo FOREIGN KEY<br /><br /> PK = vincolo PRIMARY KEY<br /><br /> R = regola (tipo obsoleto, autonoma)<br /><br /> TA = Trigger di assembly (CLR)<br /><br /> TR = trigger SQL<br /><br /> UQ = vincolo UNIQUE|  
|schema_id|**int**|ID dello schema che include l'oggetto.<br /><br /> Per tutti gli oggetti di sistema definiti a livello di ambito dello schema inclusi in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], questo valore è sempre incluso in (schema_id('sys'), schema_id('INFORMATION_SCHEMA')).|  
|parent_object_id|**int**|ID dell'oggetto a cui appartiene l'oggetto.<br /><br /> 0 = non è un oggetto figlio.|  
|tipo|**char(2)**|Tipo di oggetto:<br /><br /> AF = funzione di aggregazione (CLR)<br /><br /> C = vincolo CHECK<br /><br /> D = DEFAULT (vincolo o valore autonomo)<br /><br /> F = vincolo FOREIGN KEY<br /><br /> FN = funzione scalare SQL<br /><br /> FS = funzione scalare di assembly (CLR)<br /><br /> FT = funzione valutata a livello di tabella assembly (CLR)<br /><br /> IF = funzione SQL inline valutata a livello di tabella<br /><br /> IT = tabella interna<br /><br /> P = stored procedure SQL<br /><br /> PC = stored procedure di assembly (CLR)<br /><br /> PG = guida di piano<br /><br /> PK = vincolo PRIMARY KEY<br /><br /> R = regola (tipo obsoleto, autonoma)<br /><br /> RF = procedura-filtro-replica<br /><br /> S = tabella di base di sistema<br /><br /> SN = sinonimo<br /><br /> SO = oggetto sequenza<br /><br /> SQ = coda di servizio<br /><br /> TA = trigger DML assembly (CLR)<br /><br /> TF = funzione valutata a livello di tabella SQL<br /><br /> TR = trigger DML SQL<br /><br /> TT = tipo tabella<br /><br /> U = tabella (definita dall'utente)<br /><br /> UQ = vincolo UNIQUE<br /><br /> V = vista<br /><br /> X = stored procedure estesa|  
|type_desc|**nvarchar(60)**|Descrizione del tipo di oggetto. AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> DEFAULT_CONSTRAINT<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> INTERNAL_TABLE<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> RULE<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> SYSTEM_TABLE<br /><br /> SYNONYM<br /><br /> SERVICE_QUEUE<br /><br /> CLR_TRIGGER<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> TABLE_TYPE<br /><br /> USER_TABLE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> VIEW<br /><br /> EXTENDED_STORED_PROCEDURE|  
|create_date|**datetime**|Data di creazione dell'oggetto.|  
|modify_date|**datetime**|Data dell'ultima modifica apportata all'oggetto mediante un'istruzione ALTER. Se l'oggetto è una tabella o una vista, modify_date cambia anche quando si crea o si modifica un indice nella tabella o nella vista.|  
|is_ms_shipped|**bit**|Oggetto creato da un componente interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_published|**bit**|L'oggetto viene pubblicato.|  
|is_schema_published|**bit**|Viene pubblicato solo lo schema dell'oggetto.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.system_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)  
  
  
