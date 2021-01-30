---
description: sp_dropdevice (Transact-SQL)
title: sp_dropdevice (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropdevice_TSQL
- sp_dropdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], deleting
- sp_dropdevice
ms.assetid: c8b07189-7c35-414b-acc1-45bd6e7e17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df2e24fa3ca61cb235153e0157dbb953a828629
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200663"
---
# <a name="sp_dropdevice-transact-sql"></a>sp_dropdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina un dispositivo di database o un dispositivo di backup da un'istanza del [!INCLUDE[ssDEversion2005](../../includes/ssdeversion2005-md.md)] , eliminando la voce da **master.dbo.sysdispositivi**.  
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropdevice [ @logicalname = ] 'device'   
    [ , [ @delfile = ] 'delfile' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @logicalname = ] 'device'` Nome logico del dispositivo di database o del dispositivo di backup elencato in **master.dbo.sysDevices.Name**. il *dispositivo* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @delfile = ] 'delfile'` Specifica se il file del dispositivo di backup fisico deve essere eliminato. *delfile* è di tipo **varchar (7)**. Se specificato come **delfile**, il file su disco del dispositivo di backup fisico viene eliminato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Impossibile utilizzare **sp_dropdevice** all'interno di una transazione.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **diskadmin** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente il dispositivo di dump su nastro `tapedump1` viene eliminato da [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
EXEC sp_dropdevice 'tapedump1';  
```  
  
## <a name="see-also"></a>Vedi anche  
 [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Eliminare un dispositivo di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_helpdb &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpdevice &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdevice-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
