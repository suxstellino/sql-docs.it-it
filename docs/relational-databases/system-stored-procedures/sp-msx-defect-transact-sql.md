---
description: sp_msx_defect (Transact-SQL)
title: sp_msx_defect (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_defect
- sp_msx_defect_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_defect
ms.assetid: 0dfd963a-3bc5-4b58-94f7-aec976da2883
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2ca7f1d044ddfe0730052ff811e6084d5c070b6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174444"
---
# <a name="sp_msx_defect-transact-sql"></a>sp_msx_defect (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esclude il server corrente dalle operazioni multiserver.  
  
> [!CAUTION]  
>  **sp_msx_defect** modifica il registro di sistema. La modifica manuale del Registro di sistema non è un'operazione consigliata, in quanto modifiche inadeguate o non corrette possono causare gravi problemi di configurazione nel sistema. La modifica del Registro di sistema tramite l'editor corrispondente deve essere pertanto eseguita esclusivamente da utenti esperti. Per ulteriori informazioni, vedere la documentazione di Microsoft Windows.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_msx_defect [@forced_defection =] forced_defection  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @forced_defection = ] forced_defection` Specifica se forzare o meno l'esclusione se il SQLServerAgent master è stato perduto definitivamente a causa di un database **msdb** danneggiato in modo irreversibile oppure se non è stato eseguito alcun backup del database **msdb** . *forced_defection* è di **bit** e il valore predefinito è **0**, che indica che non deve verificarsi alcuna esclusione forzata. Il valore **1** impone la defezione.  
  
 Dopo aver forzato un'esclusione eseguendo **sp_msx_defect**, un membro del ruolo predefinito del server **sysadmin** nel SQLServerAgent master deve eseguire il comando seguente per completare l'esclusione:  
  
```  
EXECUTE msdb.dbo.sp_delete_targetserver @server_name = 'tsx-server', @post_defection =  0;  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Quando **sp_msx_defect** viene completata correttamente, viene restituito un messaggio.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa stored procedure, è necessario che gli utenti siano membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="see-also"></a>Vedere anche  
 [sp_msx_enlist &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
