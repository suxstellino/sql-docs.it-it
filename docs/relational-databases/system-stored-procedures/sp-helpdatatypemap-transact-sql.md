---
description: sp_helpdatatypemap (Transact-SQL)
title: sp_helpdatatypemap (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdatatypemap
- sp_helpdatatypemap_TSQL
helpviewer_keywords:
- sp_helpdatatypemap
ms.assetid: 800c9c65-723e-4961-a63d-327987f129f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 46750f82e08a9a3dcb936320614390bc6f359c4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176452"
---
# <a name="sp_helpdatatypemap-transact-sql"></a>sp_helpdatatypemap (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce informazioni sui mapping dei tipi di dati definiti tra [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i sistemi di gestione non di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database (DBMS). Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpdatatypemap [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @defaults_only = ] defaults_only ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @source_dbms = ] 'source_dbms'` Nome del sistema DBMS da cui viene eseguito il mapping dei tipi di dati. *source_dbms* è di **tipo sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**|L'origine è un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|L'origine è un database Oracle.|  
  
`[ @source_version = ] 'source_version'` È la versione del prodotto del sistema DBMS di origine. *source_version* è di tipo **varchar (10)** e, se non è specificato, vengono restituiti i mapping dei tipi di dati per tutte le versioni del sistema DBMS di origine. Consente al set dei risultati di venire filtrato in base alla versione di origine del sistema DBMS.  
  
`[ @source_type = ] 'source_type'` Tipo di dati elencato nel sistema DBMS di origine. *source_type* è di **tipo sysname**. se non viene specificato, vengono restituiti i mapping per tutti i tipi di dati nel sistema DBMS di origine. Consente al set di risultati di venire filtrato in base al tipo di dati nel sistema DBMS di origine.  
  
`[ @destination_dbms = ] 'destination_dbms'` Nome del sistema DBMS di destinazione. *destination_dbms* è di **tipo sysname**. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**MSSQLSERVER**|La destinazione è un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|La destinazione è un database Oracle.|  
|**DB2**|La destinazione è un database IBM DB2.|  
|**SYBASE**|La destinazione è un database Sybase.|  
  
`[ @destination_version = ] 'destination_version'` È la versione del prodotto del sistema DBMS di destinazione. *destination_version* è di tipo **varchar (10)** e, se non è specificato, vengono restituiti i mapping per tutte le versioni del sistema DBMS di destinazione. Consente al set dei risultati di venire filtrato in base alla versione di destinazione del sistema DBMS.  
  
`[ @destination_type = ] 'destination_type'` Tipo di dati elencato nel sistema DBMS di destinazione. *destination_type* è di **tipo sysname**. se non viene specificato, vengono restituiti i mapping per tutti i tipi di dati nel sistema DBMS di destinazione. Consente al set di risultati di venire filtrato in base al tipo di dati nel sistema DBMS di destinazione.  
  
`[ @defaults_only = ] defaults_only` Indica se vengono restituiti solo i mapping dei tipi di dati predefiniti. *defaults_only* è di **bit** e il valore predefinito è **0**. **1** indica che vengono restituiti solo i mapping dei tipi di dati predefiniti. **0** indica che vengono restituiti i mapping predefiniti e i mapping dei tipi di dati definiti dall'utente.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**mapping_id**|Identifica il mapping dei tipi di dati.|  
|**source_dbms**|Nome e numero di versione del sistema DBMS di origine.|  
|**source_type**|Tipo di dati nel sistema DBMS di origine.|  
|**destination_dbms**|Nome del sistema DBMS di destinazione.|  
|**destination_type**|Tipo di dati nel sistema DBMS di destinazione.|  
|**is_default**|Indica il mapping predefinito o un mapping alternativo. Il valore **0** indica che il mapping è definito dall'utente.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpdatatypemap** definisce i mapping dei tipi di dati da autori non SQL Server e da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autori a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.  
  
 Quando la combinazione specificata di DBMS di origine e di destinazione non è supportata, **sp_helpdatatypemap** restituisce un set di risultati vuoto.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** nel server di distribuzione o i membri del ruolo predefinito del database **db_owner** nel database di distribuzione possono eseguire **sp_helpdatatypemap**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_getdefaultdatatypemapping &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)  
  
  
