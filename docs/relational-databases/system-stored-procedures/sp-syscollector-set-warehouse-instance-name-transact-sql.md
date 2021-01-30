---
description: sp_syscollector_set_warehouse_instance_name (Transact-SQL)
title: sp_syscollector_set_warehouse_instance_name (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_set_warehouse_instance_name_TSQL
- sp_syscollector_set_warehouse_instance_name
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_set_warehouse_instance_name
ms.assetid: 5320fcd4-bed1-468f-b784-a5e10fcfaeb6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28a081969b80188f508039e311e55eae1d47e0b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207270"
---
# <a name="sp_syscollector_set_warehouse_instance_name-transact-sql"></a>sp_syscollector_set_warehouse_instance_name (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Specifica il nome dell'istanza per la stringa di connessione utilizzata per connettersi al data warehouse di gestione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syscollector_set_warehouse_instance_name [ @instance_name = ] 'instance_name'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @instance_name =]'*instance_name*'  
 Nome dell'istanza. *instance_name* è di **tipo sysname** e il valore predefinito è l'istanza locale se null.  
  
> **Nota:**_instance_name_ deve essere il nome completo dell'istanza, costituito dal nome del computer e dal nome dell'istanza nel formato *nomecomputer* \\ *NomeIstanza*.    
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario disabilitare l'agente di raccolta dati prima di modificarne la configurazione. La procedura non ha esito positivo se l'agente di raccolta dati è abilitato.  
  
 Per visualizzare il nome dell'istanza corrente, eseguire una query sulla vista di sistema [syscollector_config_store](../../relational-databases/system-catalog-views/syscollector-config-store-transact-sql.md) .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa procedura, è richiesta l'appartenenza al ruolo predefinito del database dc_admin (con autorizzazione EXECUTE) .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene illustrato come configurare l'agente di raccolta dati per utilizzare un'istanza del data warehouse di gestione in un server remoto. In questo esempio il server remoto è denominato `RemoteSERVER` e il database è installato sull'istanza predefinita.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_set_warehouse_instance_name N'RemoteSERVER'; -- the default instance is assumed on the remote server  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure dell'agente di raccolta dati &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_config_store &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-config-store-transact-sql.md)  
  
  
