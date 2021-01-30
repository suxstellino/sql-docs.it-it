---
description: ALTER MESSAGE TYPE (Transact-SQL)
title: ALTER MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_MESSAGE_TYPE_TSQL
- ALTER MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER MESSAGE TYPE statement
- modifying message types
- message types [Service Broker], modifying
ms.assetid: 98c94176-2bdf-4725-b4bc-d33b6b14817d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 400ba882af76481c272a240cabedadc6a03f4202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204153"
---
# <a name="alter-message-type-transact-sql"></a>ALTER MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà di un tipo di messaggio.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql 
ALTER MESSAGE TYPE message_type_name  
   VALIDATION =  
    {  NONE   
     | EMPTY   
     | WELL_FORMED_XML   
     | VALID_XML WITH SCHEMA COLLECTION schema_collection_name }  
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *message_type_name*  
 Nome del messaggio da modificare. Non è possibile specificare i nomi del server, del database e dello schema.  
  
 VALIDATION  
 Specifica la modalità con cui [!INCLUDE[ssSB](../../includes/sssb-md.md)] convalida il corpo dei messaggi di questo tipo.  
  
 NONE  
 Non viene eseguita alcuna convalida. Il corpo del messaggio può contenere dati oppure può essere NULL.  
  
 EMPTY  
 Il corpo del messaggio deve essere NULL.  
  
 WELL_FORMED_XML  
 Il corpo del messaggio deve contenere XML ben formato.  
  
 VALID_XML_WITH_SCHEMA = *schema_collection_name*  
 Il corpo del messaggio deve contenere dati XML conformi a uno schema incluso nella raccolta di schemi specificata. *schema_collection_name* deve corrispondere al nome di una raccolta di XML Schema esistente.  
  
## <a name="remarks"></a>Commenti  
 La modifica del tipo di messaggio non influisce sui messaggi che sono già stati recapitati a una coda.  
  
 Per modificare AUTHORIZATION per un tipo di messaggio, utilizzare l'istruzione ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per modificare un tipo di messaggio viene assegnata per impostazione predefinita al proprietario del tipo di messaggio, ai membri del ruolo predefinito del database **db_ddladmin** o **db_owner** e ai membri del ruolo predefinito del server **sysadmin**.  
  
 Quando l'istruzione ALTER MESSAGE TYPE specifica una raccolta di schemi, l'utente che esegue l'istruzione deve disporre dell'autorizzazione REFERENCES nella raccolta di schemi specificata.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene modificato il tipo di messaggio `//Adventure-Works.com/Expenses/SubmitExpense` in modo da richiedere che il corpo del messaggio contenga un documento XML in formato corretto.  
  
```sql  
ALTER MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = WELL_FORMED_XML ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
