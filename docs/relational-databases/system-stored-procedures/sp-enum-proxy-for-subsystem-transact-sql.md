---
description: sp_enum_proxy_for_subsystem (Transact-SQL)
title: sp_enum_proxy_for_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_proxy_for_subsystem_TSQL
- sp_enum_proxy_for_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_proxy_for_subsystems
ms.assetid: 580cc3be-1068-4a96-8d15-78ca3a5bb719
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: bac523afb9f802e3f6feb8a23c47835c82bac807
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157071"
---
# <a name="sp_enum_proxy_for_subsystem-transact-sql"></a>sp_enum_proxy_for_subsystem (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Visualizza un elenco delle autorizzazioni per i proxy di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per accedere ai sottosistemi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_enum_proxy_for_subsystem  
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @proxy_id = ] proxy_id` Numero di identificazione del proxy per cui elencare le informazioni. Il *proxy_id* è di **tipo int** e il valore predefinito è null. È possibile specificare l' *ID* o il *proxy_name* .  
  
`[ @proxy_name = ] 'proxy_name'` Nome del proxy per cui elencare le informazioni. Il *proxy_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare l' *ID* o il *proxy_name* .  
  
`[ @subsystem_id = ] subsystem_id` Numero di identificazione del sottosistema per cui elencare le informazioni. Il *subsystem_id* è di **tipo int** e il valore predefinito è null. È possibile specificare il *subsystem_id* o l' *subsystem_name* .  
  
`[ @subsystem_name = ] 'subsystem_name'` Nome del sottosistema per cui elencare le informazioni. Il *subsystem_name* è di **tipo sysname** e il valore predefinito è null. È possibile specificare il *subsystem_id* o l' *subsystem_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Numero di identificazione del sottosistema.|  
|**subsystem_name**|**sysname**|Nome del sottosistema.|  
|**proxy_id**|**int**|Numero di identificazione del proxy.|  
|**proxy_name**|**sysname**|Nome del proxy.|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Commenti  
 Se non vengono specificati parametri, **sp_enum_proxy_for_subsystem** elenca le informazioni su tutti i proxy nell'istanza di per ogni sottosistema.  
  
 Quando viene fornito un nome proxy o un ID proxy, **sp_enum_proxy_for_subsystem** elenca i sottosistemi a cui il proxy ha accesso. Quando viene fornito un ID sottosistema o un nome di sottosistema, **sp_enum_proxy_for_subsystem** elenca i proxy che hanno accesso a tale sottosistema.  
  
 Quando vengono specificate sia le informazioni sul proxy che sul sottosistema, il set di risultati restituisce una riga se il proxy specificato può accedere al sottosistema specificato.  
  
 Questo stored procedure si trova nel **database msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-listing-all-associations"></a>R. Visualizzazione di un elenco di tutte le associazioni  
 Nell'esempio seguente viene visualizzato un elenco di tutte le autorizzazioni stabilite tra proxy e sottosistemi nell'istanza corrente.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem ;  
GO  
```  
  
### <a name="b-determining-if-a-proxy-has-access-to-a-specific-subsystem"></a>B. Stabilire se un proxy ha accesso a un sottosistema specificato  
 Nell'esempio seguente viene restituita una riga se il proxy `Catalog application proxy` ha accesso al sottosistema `ActiveScripting`. In caso contrario, viene restituito un set di risultati vuoto.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem  
    @subsystem_name = 'ActiveScripting',  
    @proxy_name = 'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_grant_proxy_to_subsystem &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
