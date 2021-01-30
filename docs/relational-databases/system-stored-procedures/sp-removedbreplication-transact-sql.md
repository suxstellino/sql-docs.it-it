---
title: sp_removedbreplication (T-SQL)
description: Viene descritto il sp_removedbreplication stored procedure utilizzato per rimuovere tutti gli oggetti di replica nel database di pubblicazione per SQL Server replica.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_removedbreplication
- sp_removedbreplication_TSQL
helpviewer_keywords:
- sp_removedbreplication
ms.assetid: cb98d571-d1eb-467b-91f7-a6e091009672
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39a95fbdfa80cb76e4b73f912ea38de1bc567906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193100"
---
# <a name="sp_removedbreplication-transact-sql"></a>sp_removedbreplication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Questa stored procedure rimuove tutti gli oggetti di replica nel database di pubblicazione dell'istanza del server di pubblicazione di SQL Server nel database di sottoscrizione dell'istanza del sottoscrittore di SQL Server. Avviare l'esecuzione nel database appropriato oppure, se l'esecuzione è nel contesto di un altro database nella stessa istanza, specificare il database in cui gli oggetti di replica devono essere rimossi. Questa procedura non rimuove gli oggetti di altri database, ad esempio il database di distribuzione.  
  
> [!NOTE]  
>  È consigliabile utilizzare questa procedura solo se gli altri metodi di rimozione degli oggetti di replica hanno esito negativo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_removedbreplication [ [ @dbname = ] 'dbname' ]  
    [ , [ @type = ] type ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @dbname = ] 'dbname'` Nome del database. *dbname* è di tipo **sysname** e il valore predefinito è NULL. Quando è NULL, viene utilizzato il database corrente.  
  
`[ @type = ] type` Tipo di replica per cui vengono rimossi gli oggetti di database. il *tipo* è **nvarchar (5)** . i possibili valori sono i seguenti.  
  
|||  
|-|-|  
|**Tran**|Rimuove gli oggetti di pubblicazione correlati alla replica transazionale.|  
|**merge**|Rimuove gli oggetti di pubblicazione correlati alla replica di tipo merge.|  
|**both** (impostazione predefinita)|Rimuove tutti gli oggetti di pubblicazione correlati alla replica.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_removedbreplication** viene utilizzato in tutti i tipi di replica.  
  
 **sp_removedbreplication** è utile quando si ripristina un database replicato privo di oggetti di replica che devono essere ripristinati.  
  
 Impossibile utilizzare **sp_removedbreplication** per un database contrassegnato come di sola lettura.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_removedbreplication**.  
  
## <a name="examples"></a>Esempi

### <a name="a-remove-replication-objects-adventureworks2012replica-subscription-database"></a>R. Rimuovere gli oggetti di replica, database di sottoscrizione AdventureWorks2012Replica
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/sp-removedbreplication-t_1.sql)]  
  
### <a name="b-remove-replication-objects-adventureworksreplica-subscription-database"></a>B. Rimuovere gli oggetti di replica, database di sottoscrizione AdventureWorksReplica
  
```sql
-- Remove replication objects from the subscription database on MYSUB.  
DECLARE @subscriptionDB AS sysname  
SET @subscriptionDB = N'AdventureWorksReplica'  
  
-- Remove replication objects from a subscription database (if necessary).  
USE master  
EXEC sp_removedbreplication @subscriptionDB  
GO  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Disabilitare la pubblicazione e la distribuzione](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
