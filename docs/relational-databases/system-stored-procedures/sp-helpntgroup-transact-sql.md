---
description: sp_helpntgroup (Transact-SQL)
title: sp_helpntgroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpntgroup
- sp_helpntgroup_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpntgroup
ms.assetid: 02b4f7c1-480a-436c-8bae-7a2488be45d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc378f5c07c4aa24c37c0212fd9f84fb6ef39f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210874"
---
# <a name="sp_helpntgroup-transact-sql"></a>sp_helpntgroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sui gruppi di Windows a cui sono associati account nel database corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpntgroup [ [ @ntname= ] 'name' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @ntname = ] 'name'` Nome del gruppo di Windows. *Name* è di **tipo sysname** e il valore predefinito è null. il *nome* deve essere un gruppo di Windows valido con accesso al database corrente. Se il *nome* non è specificato, tutti i gruppi di Windows con accesso al database corrente vengono inclusi nell'output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**NTGroupName**|**sysname**|Nome del gruppo di Windows.|  
|**NTGroupId**|**smallint**|Identificatore (ID) di gruppo.|  
|**SID**|**varbinary(85)**|ID di sicurezza (SID) di **NTGroupName**.|  
|**HasDbAccess**|**int**|1 = Il gruppo di Windows dispone dell'accesso al database.|  
  
## <a name="remarks"></a>Commenti  
 Per visualizzare un elenco dei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ruoli nel database corrente, utilizzare **sp_helprole**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene restituito un elenco dei gruppi di Windows con accesso al database corrente.  
  
```  
EXEC sp_helpntgroup;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_helprole &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_revokedbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
