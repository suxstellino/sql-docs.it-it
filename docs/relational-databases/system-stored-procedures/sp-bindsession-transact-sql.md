---
description: sp_bindsession (Transact-SQL)
title: sp_bindsession (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindsession
- sp_bindsession_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindsession
ms.assetid: 1436fe21-ad00-4a98-aca1-1451a5e571d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64f6b6b4134c5f29dea38d87e7d75e43910ca530
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206603"
---
# <a name="sp_bindsession-transact-sql"></a>sp_bindsession (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Associa o Annulla l'associazione di una sessione ad altre sessioni nella stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] . L'associazione di sessioni consente a due o più sessioni di partecipare alla stessa transazione e condividere i blocchi fino a quando non viene eseguita un'istruzione ROLLBACK TRANSACTION o COMMIT TRANSACTION.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilizzare invece MARS (Multiple Active Results Sets) o transazioni distribuite. Per altre informazioni vedere [Uso di MARS &#40;Multiple Active Result Set&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_bindsession { 'bind_token' | NULL }  
```  
  
## <a name="arguments"></a>Argomenti  
 **'** *bind_token* **'**  
 Token che identifica la transazione ottenuta in origine tramite **sp_getbindtoken** o la funzione di **Srv_getbindtoken** Open Data Services. *bind_token* è di tipo **varchar (255)**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Due sessioni associate condividono esclusivamente la transazione e i blocchi. Ogni sessione mantiene il livello di isolamento specifico impostato e se si imposta un diverso livello di isolamento per una sessione, tale modifica non influisce sul livello di isolamento dell'altra sessione. Ogni sessione è identificata da un account di sicurezza specifico e consente l'accesso solo alle risorse del database per cui l'account in questione dispone delle autorizzazioni.  
  
 **sp_bindsession** utilizza un token di associazione per associare due o più sessioni client esistenti. Tali sessioni devono essere incluse nella stessa istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] da cui è stato ottenuto il token di associazione. Una sessione è un client che esegue un comando. Le sessioni di database associate condividono una transazione e uno spazio di blocco.  
  
 Non è possibile utilizzare un token di associazione ottenuto da un'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] per una sessione client connessa a un'altra istanza, anche nel caso di transazioni DTC. Un token di associazione è valido solo a livello locale all'interno di ogni istanza e non può essere condiviso tra più istanze. Per associare le sessioni client in un'altra istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] , è necessario ottenere un token di associazione diverso eseguendo **sp_getbindtoken**.  
  
 **sp_bindsession** avrà esito negativo con un errore se usa un token non attivo.  
  
 Annullare l'associazione da una sessione utilizzando **sp_bindsession** senza specificare *BIND_TOKEN* o passando null in *bind_token*.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene associato alla sessione corrente il token di associazione specificato.  
  
> [!NOTE]  
>  Il token di associazione illustrato nell'esempio è stato ottenuto eseguendo **sp_getbindtoken** prima di eseguire **sp_bindsession**.  
  
```  
USE master;  
GO  
EXEC sp_bindsession 'BP9---5---->KB?-V'<>1E:H-7U-]ANZ';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_getbindtoken &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getbindtoken-transact-sql.md)   
 [srv_getbindtoken &#40;API stored procedure estesa&#41;](../../relational-databases/extended-stored-procedures-reference/srv-getbindtoken-extended-stored-procedure-api.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
