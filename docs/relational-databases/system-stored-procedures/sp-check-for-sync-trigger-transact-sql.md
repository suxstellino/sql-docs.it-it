---
description: sp_check_for_sync_trigger (Transact-SQL)
title: sp_check_for_sync_trigger (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- filter_TSQL
- sp_check_for_sync_trigger
helpviewer_keywords:
- sp_check_for_sync_trigger
ms.assetid: 54a1e2fd-c40a-43d4-ac64-baed28ae4637
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cb15d74c1ac4845b7ff60bd12983b922008b27c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206565"
---
# <a name="sp_check_for_sync_trigger-transact-sql"></a>sp_check_for_sync_trigger (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Determina se una stored procedure definita dall'utente o un trigger definito dall'utente viene chiamato nel contesto di un trigger di replica utilizzato per sottoscrizioni ad aggiornamento immediato. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione o nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_check_for_sync_trigger [ @tabid = ] 'tabid'   
    [ , [ @trigger_op = ] 'trigger_output_parameters' OUTPUT ]  
    [ , [ @fonpublisher = ] fonpublisher ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [**@tabid =** ]'*tabid*'  
 ID di oggetto della tabella in cui vengono controllati i trigger per l'aggiornamento immediato. *tabid* è di **tipo int** e non prevede alcun valore predefinito.  
  
 [**@trigger_op =** ] output '*trigger_output_parameters*'  
 Specifica se il parametro di output restituisce il tipo di trigger da cui viene richiamato. *trigger_output_parameters* è **char (10)** . i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**In**|Trigger INSERT|  
|**Upd**|Trigger UPDATE|  
|**CANC**|Trigger DELETE|  
|NULL (predefinito)||  
  
`[ @fonpublisher = ] fonpublisher` Specifica il percorso in cui viene eseguita la stored procedure. *fonpublisher* è di **bit** e il valore predefinito è 0. 0 indica che l'esecuzione avviene nel Sottoscrittore, mentre 1 indica che avviene nel server di pubblicazione.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 indica che la stored procedure non viene richiamata nel contesto di un trigger per l'aggiornamento immediato. 1 indica che è in corso la chiamata nel contesto di un trigger ad aggiornamento immediato ed è il tipo di trigger restituito in *\@ trigger_op*.  
  
## <a name="remarks"></a>Commenti  
 **sp_check_for_sync_trigger** viene utilizzata nella replica snapshot e nella replica transazionale.  
  
 **sp_check_for_sync_trigger** viene utilizzato per coordinare la replica e i trigger definiti dall'utente. Questa stored procedure determina se viene richiamata nel contesto di un trigger di replica. Ad esempio, è possibile chiamare la stored procedure **sp_check_for_sync_trigger** nel corpo di un trigger definito dall'utente. Se **sp_check_for_sync_trigger** restituisce **0**, l'elaborazione del trigger definito dall'utente continua. Se **sp_check_for_sync_trigger** restituisce **1**, il trigger definito dall'utente viene chiuso. Ciò garantisce che il trigger definito dall'utente non venga attivato quando il trigger di replica aggiorna la tabella.  
  
## <a name="examples"></a>Esempi

### <a name="a-add-code-to-a-trigger-on-a-subscriber-table"></a>R. Aggiungere codice a un trigger in una tabella del Sottoscrittore
 Nell'esempio seguente viene illustrato il codice che può essere utilizzato in un trigger in una tabella del Sottoscrittore.  
  
```  
DECLARE @retcode int, @trigger_op char(10), @table_id int  
SELECT @table_id = object_id('tablename')  
EXEC @retcode = sp_check_for_sync_trigger @table_id, @trigger_op OUTPUT  
IF @retcode = 1  
RETURN  
```  
  
### <a name="b-add-code-to-a-trigger-on-a-publisher-table"></a>B. Aggiungere codice a un trigger in una tabella del server di pubblicazione
 Il codice può essere aggiunto anche a un trigger in una tabella nel server di pubblicazione. il codice è simile, ma la chiamata a **sp_check_for_sync_trigger** include un parametro aggiuntivo.  
  
```  
DECLARE @retcode int, @trigger_op char(10), @table_id int, @fonpublisher int  
SELECT @table_id = object_id('tablename')  
SELECT @fonpublisher = 1  
EXEC @retcode = sp_check_for_sync_trigger @table_id, @trigger_op OUTPUT, @fonpublisher  
IF @retcode = 1  
RETURN  
```  
  
## <a name="permissions"></a>Autorizzazioni  
 **sp_check_for_sync_trigger** stored procedure può essere eseguita da qualsiasi utente con autorizzazioni SELECT nella vista di sistema [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) .  
  
## <a name="see-also"></a>Vedere anche  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
