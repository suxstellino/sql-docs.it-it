---
description: sp_adjustpublisheridentityrange (Transact-SQL)
title: sp_adjustpublisheridentityrange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adjustpublisheridentityrange_TSQL
- sp_adjustpublisheridentityrange
helpviewer_keywords:
- sp_adjustpublisheridentityrange
ms.assetid: 64f111fd-fb7d-4459-93f7-65f0f8dd7efe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5855ffeceff68c10e0eac2089a95d2b0184cd327
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198351"
---
# <a name="sp_adjustpublisheridentityrange-transact-sql"></a>sp_adjustpublisheridentityrange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Regola l'intervallo di valori Identity in una pubblicazione e riassegna nuovi intervalli in base al valore soglia previsto per la pubblicazione. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_adjustpublisheridentityrange [ [ @publication = ] 'publication' ]  
    [ , [ @table_name = ] 'table_name' ]  
    [ , [ @table_owner= ] 'table_owner' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione in cui vengono riallocati i nuovi intervalli di valori Identity. *Publication* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_name = ] 'table_name'` Nome della tabella in cui vengono riallocati i nuovi intervalli di valori Identity. *table_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @table_owner = ] 'table_owner'` Proprietario della tabella nel server di pubblicazione. *TABLE_OWNER* è di **tipo sysname** e il valore predefinito è null. Se *TABLE_OWNER* viene omesso, viene utilizzato il nome dell'utente corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_adjustpublisheridentityrange** viene utilizzato in tutti i tipi di replica.  
  
 Nel caso di una pubblicazione per la quale è attivata la gestione automatica di intervalli di valori Identity, l'agente di distribuzione o di merge è responsabile della regolazione automatica dell'intervallo di valori Identity in una pubblicazione in base al valore soglia corrispondente. Tuttavia, se per qualche motivo il agente di distribuzione o agente di merge non è stato eseguito per un determinato periodo di tempo e la risorsa dell'intervallo di valori Identity è stata utilizzata in modo significativo fino al punto di soglia, gli utenti possono chiamare **sp_adjustpublisheridentityrange** per allocare un nuovo intervallo di valori per un server di pubblicazione.  
  
 Quando si esegue **sp_adjustpublisheridentityrange**, è necessario specificare la *pubblicazione* o la *table_name* . Se vengono specificati entrambi oppure viene omesso uno dei due, viene restituito un errore.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_adjustpublisheridentityrange**.  
  
## <a name="see-also"></a>Vedere anche  
 [Replica colonne Identity](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
