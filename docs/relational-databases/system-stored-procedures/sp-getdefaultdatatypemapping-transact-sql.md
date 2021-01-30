---
description: sp_getdefaultdatatypemapping (Transact-SQL)
title: sp_getdefaultdatatypemapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getdefaultdatatypemapping_TSQL
- sp_getdefaultdatatypemapping
helpviewer_keywords:
- sp_getdefaultdatatypemapping
ms.assetid: b8401de1-f135-41d0-ba79-ce8fe1f48c00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64345f8471caf5d757e0c8fbdbc843b2a18680ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183160"
---
# <a name="sp_getdefaultdatatypemapping-transact-sql"></a>sp_getdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sul mapping predefinito per il tipo di dati specificato tra [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e un sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestione non di database (DBMS). Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_getdefaultdatatypemapping [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
        , [ @source_type = ] 'source_type'    
    [ , [ @source_length = ] source_length ]  
    [ , [ @source_precision = ] source_precision ]  
    [ , [ @source_scale = ] source_scale ]  
    [ , [ @source_nullable = ] source_nullable ]  
        , [ @destination_dbms = ] 'destination_dbms'   
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' OUTPUT ]  
    [ , [ @destination_length = ] destination_length OUTPUT ]  
    [ , [ @destination_precision = ] destination_precision OUTPUT ]  
    [ , [ @destination_scale = ] destination_scale OUTPUT ]  
    [ , [ @destination_nullable = ] source_nullable OUTPUT ]  
    [ , [ @dataloss = ] dataloss OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @source_dbms = ] 'source_dbms'` Nome del sistema DBMS da cui viene eseguito il mapping dei tipi di dati. *source_dbms* è di **tipo sysname**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**|L'origine è un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|L'origine è un database Oracle.|  
  
 Questo parametro è obbligatorio.  
  
`[ @source_version = ] 'source_version'` È il numero di versione del sistema DBMS di origine. *source_version* è di tipo **varchar (10)** e il valore predefinito è null.  
  
`[ @source_type = ] 'source_type'` Tipo di dati nel sistema DBMS di origine. *source_type* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @source_length = ] source_length` Lunghezza del tipo di dati nel sistema DBMS di origine. *source_length* è di tipo **bigint** e il valore predefinito è null.  
  
`[ @source_precision = ] source_precision` Precisione del tipo di dati nel sistema DBMS di origine. *source_precision* è di tipo **bigint** e il valore predefinito è null.  
  
`[ @source_scale = ] source_scale` Scala del tipo di dati nel sistema DBMS di origine. *source_scale* è di **tipo int** e il valore predefinito è null.  
  
`[ @source_nullable = ] source_nullable` Indica se il tipo di dati nel sistema DBMS di origine supporta un valore NULL. *source_nullable* è di **bit** e il valore predefinito è **1**, il che significa che i valori null sono supportati.  
  
`[ @destination_dbms = ] 'destination_dbms'` Nome del sistema DBMS di destinazione. *destination_dbms* è di **tipo sysname**. i possibili valori sono i seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**|La destinazione è un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|La destinazione è un database Oracle.|  
|**DB2**|La destinazione è un database IBM DB2.|  
|**SYBASE**|La destinazione è un database Sybase.|  
  
 Questo parametro è obbligatorio.  
  
`[ @destination_version = ] 'destination_version'` È la versione del prodotto del sistema DBMS di destinazione. *destination_version* è di tipo **varchar (10)** e il valore predefinito è null.  
  
`[ @destination_type = ] 'destination_type' OUTPUT` Tipo di dati elencato nel sistema DBMS di destinazione. *destination_type* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @destination_length = ] destination_length OUTPUT` Lunghezza del tipo di dati nel sistema DBMS di destinazione. *destination_length* è di tipo **bigint** e il valore predefinito è null.  
  
`[ @destination_precision = ] destination_precision OUTPUT` Precisione del tipo di dati nel sistema DBMS di destinazione. *destination_precision* è di tipo **bigint** e il valore predefinito è null.  
  
`[ @destination_scale = ] _destination_scaleOUTPUT` Scala del tipo di dati nel sistema DBMS di destinazione. *destination_scale* è di **tipo int** e il valore predefinito è null.  
  
`[ @destination_nullable = ] _destination_nullableOUTPUT` Indica se il tipo di dati nel sistema DBMS di destinazione supporta il valore NULL. *destination_nullable* è di **bit** e il valore predefinito è null. **1** indica che i valori null sono supportati.  
  
`[ @dataloss = ] _datalossOUTPUT` Indica se il mapping può causare la perdita di dati. *dataloss* è di **bit** e il valore predefinito è null. **1** indica che è possibile che si verifichi una perdita di dati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_getdefaultdatatypemapping** viene utilizzato in tutti i tipi di replica tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e un non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DBMS.  
  
 **sp_getdefaultdatatypemapping** restituisce il tipo di dati di destinazione predefinito che rappresenta la corrispondenza più vicina al tipo di dati di origine specificato.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_getdefaultdatatypemapping**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_helpdatatypemap &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)  
  
  
