---
description: sp_settriggerorder (Transact-SQL)
title: sp_settriggerorder (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_settriggerorder
- sp_settriggerorder_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_settriggerorder
ms.assetid: 8b75c906-7315-486c-bc59-293ef12078e8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f7977873f4b2887b736ffc16f5592340f5b87dee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207366"
---
# <a name="sp_settriggerorder-transact-sql"></a>sp_settriggerorder (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Specifica il primo o l'ultimo trigger AFTER attivato. I trigger AFTER compresi fra il primo e l'ultimo vengono eseguiti in base a un ordine non definito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_settriggerorder [ @triggername = ] '[ triggerschema. ] triggername'   
    , [ @order = ] 'value'   
    , [ @stmttype = ] 'statement_type'   
    [ , [ @namespace = ] { 'DATABASE' | 'SERVER' | NULL } ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @triggername = ] '[ _triggerschema.] _triggername'` Nome del trigger e schema a cui appartiene, se applicabile, il cui ordine deve essere impostato o modificato. [_triggerschema_**.**] *triggername* è di **tipo sysname**. Se il nome specificato non corrisponde a un trigger oppure corrisponde a un trigger INSTEAD OF, viene restituito un errore. Impossibile specificare *triggerschema* per i trigger DDL o Logon.  
  
`[ @order = ] 'value'` Impostazione del nuovo ordine del trigger. *value* è di tipo **varchar (10)** e può essere uno dei valori seguenti.  
  
> [!IMPORTANT]  
>  Il **primo** e l' **ultimo** trigger devono essere due trigger diversi.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**Primo**|Trigger avviato per primo.|  
|**Ultimo**|Trigger avviato per ultimo.|  
|**Nessuno**|Il trigger viene attivato in base a un ordine non definito.|  
  
`[ @stmttype = ] 'statement_type'` Specifica l'istruzione SQL che attiva il trigger. *statement_type* è di tipo **varchar (50)** ed è possibile inserire, aggiornare, eliminare, accedere o qualsiasi [!INCLUDE[tsql](../../includes/tsql-md.md)] evento di istruzione elencato negli [eventi DDL](../../relational-databases/triggers/ddl-events.md). Non è possibile specificare gruppi di eventi.  
  
 Un trigger può essere designato come **primo** o **ultimo** trigger per un tipo di istruzione solo dopo che il trigger è stato definito come trigger per tale tipo di istruzione. Ad esempio, il trigger **TR1** può essere designato per **primo** per INSERT nella tabella **T1** se **TR1** è definito come trigger di inserimento. [!INCLUDE[ssDE](../../includes/ssde-md.md)]Restituisce un errore se **TR1**, che è stato definito solo come trigger di inserimento, viene impostato come **primo** o **ultimo** trigger per un'istruzione Update. Per altre informazioni, vedere la sezione Osservazioni.  
  
 **\@ namespace =** { **' database '**  |  **' server '** | NULL  
 Quando *triggername* è un trigger DDL, **\@ lo spazio dei nomi** specifica se *triggername* è stato creato con ambito database o ambito server. Se *triggername* è un trigger LOGON, è necessario specificare il server. Per ulteriori informazioni sull'ambito del trigger DDL, vedere [trigger DDL](../../relational-databases/triggers/ddl-triggers.md). Se non è specificato o se viene specificato NULL, *triggername* è un trigger DML.  
  
* Il SERVER si applica a: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o 1 (esito negativo)  
  
## <a name="remarks"></a>Commenti  
  
## <a name="dml-triggers"></a>Trigger DML  
 Può essere presente un solo **primo** e un **ultimo** trigger per ogni istruzione in una singola tabella.  
  
 Se un **primo** trigger è già definito nella tabella, nel database o nel server, non è possibile designare un nuovo trigger come **primo** per la stessa tabella, database o server per lo stesso *statement_type*. Questa restrizione applica anche gli **ultimi** trigger.  
  
 La replica genera automaticamente un primo trigger per ogni tabella inclusa in una sottoscrizione ad aggiornamento in coda o ad aggiornamento immediato. La replica richiede che il proprio trigger sia il primo trigger. La replica genera un errore se si cerca di includere una tabella con un primo trigger in una sottoscrizione ad aggiornamento immediato o ad aggiornamento in coda. Se si prova a impostare un trigger come primo dopo l'inclusione di una tabella in una sottoscrizione, **sp_settriggerorder** restituisce un errore. Se si utilizza ALTER TRIGGER sul trigger di replica oppure si utilizza **sp_settriggerorder** per modificare il trigger di replica in un trigger **Last** o **None** , la sottoscrizione non funzionerà correttamente.  
  
## <a name="ddl-triggers"></a>Trigger DDL  
 Se un trigger DDL con ambito database e un trigger DDL con ambito server esistono nello stesso evento, è possibile specificare che entrambi i trigger siano un **primo** trigger o un **ultimo** trigger. Tuttavia, i trigger con ambito server vengono sempre generati per primi. In generale, l'ordine di esecuzione dei trigger DDL che esistono sullo stesso evento è il seguente:  
  
1.  Trigger a livello di server contrassegnato come **primo**.  
  
2.  Altri trigger a livello del server  
  
3.  Trigger a livello di server contrassegnato come **ultimo**.  
  
4.  Trigger a livello di database contrassegnato per **primo**.  
  
5.  Altri trigger a livello del database  
  
6.  Trigger a livello di database contrassegnato come **ultimo**.  
  
## <a name="general-trigger-considerations"></a>Considerazioni generali sui trigger  
 Se un'istruzione ALTER TRIGGER modifica un primo o un ultimo trigger, il **primo** o l' **ultimo** attributo impostato in origine sul trigger viene eliminato e il valore viene sostituito da **None**. Il valore dell'ordine deve essere reimpostato utilizzando **sp_settriggerorder**.  
  
 Se lo stesso trigger deve essere designato come primo o ultimo ordine per più di un tipo di istruzione, è necessario eseguire **sp_settriggerorder** per ogni tipo di istruzione. Inoltre, il trigger deve essere definito per primo per un tipo di istruzione prima di poter essere designato come **primo** o **ultimo** trigger da attivare per tale tipo di istruzione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per impostare l'ordine di un trigger DDL con ambito server (creato ON ALL SERVER) o un trigger LOGON è necessaria l'autorizzazione CONTROL SERVER nel server.  
  
 Per impostare l'ordine di attivazione di un trigger DDL nell'ambito del database (creato tramite l'istruzione ON DATABASE), è necessario disporre dell'autorizzazione ALTER ANY DATABASE DDL TRIGGER.  
  
 Per impostare l'ordine di attivazione di un trigger DML, è necessario disporre dell'autorizzazione ALTER per la tabella o vista in cui è stato definito il trigger.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-setting-the-firing-order-for-a-dml-trigger"></a>R. Impostazione dell'ordine di attivazione per un trigger DML  
 Nell'esempio seguente il trigger `uSalesOrderHeader` viene definito come primo trigger da attivare dopo l'esecuzione di un'operazione `UPDATE` nella tabella `Sales.SalesOrderHeader`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'Sales.uSalesOrderHeader', @order='First', @stmttype = 'UPDATE';  
```  
  
### <a name="b-setting-the-firing-order-for-a-ddl-trigger"></a>B. Impostazione dell'ordine di attivazione per un trigger DDL  
 Nell'esempio seguente il trigger `ddlDatabaseTriggerLog` viene definito come primo trigger da attivare dopo che si è verificato un evento `ALTER_TABLE` nel database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'ddlDatabaseTriggerLog', @order='First', @stmttype = 'ALTER_TABLE', @namespace = 'DATABASE';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
  
