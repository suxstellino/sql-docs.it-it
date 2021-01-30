---
description: sp_changedistpublisher (Transact-SQL)
title: sp_changedistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changedistpublisher_TSQL
- sp_changedistpublisher
helpviewer_keywords:
- sp_changedistpublisher
ms.assetid: 7ef5c89d-faaa-4f8e-aef7-00649ebc8bc9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d48757f7404ba4f915e7b92609c9077ab702adb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159033"
---
# <a name="sp_changedistpublisher-transact-sql"></a>sp_changedistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà del server di pubblicazione della distribuzione. Questa stored procedure viene eseguita in qualsiasi database del server di distribuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changedistpublisher [ @publisher = ] 'publisher'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @storage_connection_string = ] 'storage_connection_string']
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @property = ] 'property'` Proprietà da modificare per il server di pubblicazione specificato. *Property* è di **tipo sysname** . i possibili valori sono i seguenti.  
  
`[ @value = ] 'value'` Valore per la proprietà specificata. *value* è di **tipo nvarchar (255)** e il valore predefinito è null.  
  
`[ @storage_connection_string = ] 'storage_connection_string'` È obbligatorio per SQL Istanza gestita, deve corrispondere alla chiave di accesso per il volume di archiviazione del database SQL di Azure. 


 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]
 
 Nella tabella seguente vengono descritte le proprietà valide dei server di pubblicazione e i valori corrispondenti.  
  
|Proprietà|Valori|Descrizione|  
|--------------|------------|-----------------|  
|**active**|**true**|Attiva il server di pubblicazione.|  
||**false**|Disattiva il server di pubblicazione.|  
|**distribution_db**||Nome del database di distribuzione.|  
|**accesso**||Nome dell'account di accesso.|  
|**password**||Password complessa per l'account di accesso fornito.|  
|**security_mode**|**1**|Esegue la connessione al server di pubblicazione utilizzando l'autenticazione di Windows. *Non è possibile modificare questo valore per un non* [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *server di pubblicazione.*|  
||**0**|Esegue la connessione al server di pubblicazione utilizzando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Non è possibile modificare questo valore per un non* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *server di pubblicazione.*|  
|**working_directory**||Directory di lavoro utilizzata per archiviare i file dei dati e di schema per la pubblicazione.|  
|NULL (predefinito)||Verranno stampate tutte le opzioni di *Proprietà* disponibili.| 
|**storage_connection_string**| Chiave di accesso | Chiave di accesso per la directory di lavoro quando il database è Istanza gestita di Azure SQL. 
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changedistpublisher** viene utilizzato in tutti i tipi di replica.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** possono eseguire **sp_changedistpublisher**.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
