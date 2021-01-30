---
description: core.sp_add_collector_type (Transact-SQL)
title: core.sp_add_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_collector_type
- sp_add_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- core.sp_add_collector_type stored procedure
- management data warehouse, data collector stored procedures
- sp_add_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 1d981037-2147-464e-a456-7d8e479bce89
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0db58f15641ab57d7eb35782c62334106a4bcdf7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181330"
---
# <a name="coresp_add_collector_type-transact-sql"></a>core.sp_add_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge una nuove voce alla vista core.supported_collector_types nel database del data warehouse di gestione. La procedura deve essere eseguita nel contesto del database del data warehouse di gestione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
core.sp_add_collector_type [ @collector_type_uid = ] 'collector_type_uid'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @collector_type_uid =]'*collector_type_uid*'  
 GUID per il tipo agente di raccolta. *collector_type_uid* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del database di **mdw_admin** (con autorizzazione Execute).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene aggiunto il tipo di agente di raccolta query T-SQL generico alla vista core.supported_collector_types. Per impostazione predefinita, il tipo agente di raccolta query T-SQL generico esiste già. Se pertanto si esegue il codice in un'installazione predefinita, viene visualizzato un messaggio che indica che il tipo di agente di raccolta esiste già.  
  
 Il codice viene eseguito correttamente se il tipo di agente di raccolta query T-SQL generico è stato rimosso utilizzando la stored procedure core.sp_remove_collector_type e quindi si desidera aggiungerlo di nuovo come tipo di agente di raccolta query registrato in grado di caricare i dati nel data warehouse di gestione.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @RC int;  
DECLARE @collector_type_uid uniqueidentifier;  
SELECT @collector_type_uid = (SELECT collector_type_uid FROM msdb.dbo.syscollector_collector_types WHERE name = N'Generic T-SQL Query Collector Type');  
EXECUTE @RC = core.sp_add_collector_type @collector_type_uid;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Data warehouse di gestione](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
