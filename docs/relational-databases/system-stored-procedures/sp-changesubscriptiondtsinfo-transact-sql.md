---
description: sp_changesubscriptiondtsinfo (Transact-SQL)
title: sp_changesubscriptiondtsinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriptiondtsinfo
- sp_changesubscriptiondtsinfo_TSQL
helpviewer_keywords:
- sp_changesubscriptiondtsinfo
ms.assetid: 64fc085f-f81b-493b-b59a-ee6192d9736d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 63762571085653afea03470cfc1e453a85e8adad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194416"
---
# <a name="sp_changesubscriptiondtsinfo-transact-sql"></a>sp_changesubscriptiondtsinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le proprietà dei pacchetti DTS di una sottoscrizione. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_changesubscriptiondtsinfo [ [ @job_id = ] job_id ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @job_id = ] job_id` ID del processo di agente di distribuzione per la sottoscrizione push. *job_id* è di tipo **varbinary (16)** e non prevede alcun valore predefinito. Per trovare l'ID del processo di distribuzione, eseguire **sp_helpsubscription** o **sp_helppullsubscription**.  
  
`[ @dts_package_name = ] 'dts_package_name'` Specifica il nome del pacchetto DTS. *dts_package_name* è di **tipo sysname** e il valore predefinito è null. Ad esempio, per specificare un pacchetto denominato **DTSPub_Package**, è necessario specificare `@dts_package_name = N'DTSPub_Package'` .  
  
`[ @dts_package_password = ] 'dts_package_password'` Consente di specificare la password per il pacchetto. *dts_package_password* è di **tipo sysname** e il valore predefinito è null, che indica che la proprietà della password deve essere lasciata invariata.  
  
> [!NOTE]  
>  A ogni pacchetto DTS deve essere associata una password.  
  
`[ @dts_package_location = ] 'dts_package_location'` Specifica la posizione del pacchetto. *dts_package_location* è di **tipo nvarchar (12)** e il valore predefinito è null, che specifica che il percorso del pacchetto deve rimanere invariato. Il percorso del pacchetto può essere modificato nel database di **distribuzione** o nel **Sottoscrittore**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_changesubscriptiondtsinfo** viene utilizzato per la replica snapshot e la replica transazionale che sono solo sottoscrizioni push.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_changesubscriptiondtsinfo** solo i membri del ruolo predefinito del server **sysadmin** , **db_owner** ruolo predefinito del database o l'autore della sottoscrizione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
