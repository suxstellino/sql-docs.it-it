---
description: xp_sqlmaint (Transact-SQL)
title: xp_sqlmaint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sqlmaint
- xp_sqlmaint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sqlmaint
ms.assetid: bda66e1b-6bbd-49be-b86e-37efc920e912
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5b83e5e84d5712e9b1cf3e253222d1ef6d212720
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187950"
---
# <a name="xp_sqlmaint-transact-sql"></a>xp_sqlmaint (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Chiama l'utilità **SQLMaint** con una stringa che contiene le opzioni **SQLMaint**. L'utilità **SQLMaint** esegue un set di operazioni di manutenzione in uno o più database.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_sqlmaint 'switch_string'     
```  
  
## <a name="arguments"></a>Argomenti  
 **'** *switch_string* **'**  
 Stringa contenente le opzioni dell'utilità **SQLMaint** . Le opzioni e i relativi valori devono essere separati da uno spazio.  
  
 **-?** opzione non valida per **xp_sqlmaint**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuna. Restituisce un errore se l'utilità **SQLMaint** ha esito negativo.  
  
## <a name="remarks"></a>Commenti  
 Se questa procedura viene chiamata da un utente connesso con SQL Server autenticazione, le opzioni **-U "**_login_ID_*_"_* e **-P "**_password_*_"_* vengono anteposti a *switch_string* prima dell'esecuzione. Se l'utente è connesso con l'autenticazione di Windows, *switch_string* viene passato senza apportare modifiche a **SQLMaint**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente `xp_sqlmaint` viene chiamata da `sqlmaint` per eseguire controlli di integrità, creare un file di report e aggiornare `msdb.dbo.sysdbmaintplan_history`.  
  
```  
EXEC xp_sqlmaint '-D AdventureWorks2012 -PlanID 02A52657-D546-11D1-9D8A-00A0C9054212   
   -Rpt "C:\Program Files\Microsoft SQL Server\MSSQL\LOG\DBMaintPlan2.txt" -WriteHistory  -CkDB -CkAl';   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
The command(s) executed successfully.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Utilità sqlmaint](../../tools/sqlmaint-utility.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
