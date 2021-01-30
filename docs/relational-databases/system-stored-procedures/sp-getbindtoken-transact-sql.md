---
description: sp_getbindtoken (Transact-SQL)
title: sp_getbindtoken (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_getbindtoken
- sp_getbindtoken_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_getbindtoken
ms.assetid: 5db87d77-85fa-45a3-a23a-3ea500f9a5ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4613c83384aa7ff2cd36512f6c172a00fbbb047e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185752"
---
# <a name="sp_getbindtoken-transact-sql"></a>sp_getbindtoken (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un identificatore univoco per la transazione, ovvero una stringa utilizzata per associare le sessioni tramite sp_bindsession.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilizzare invece MARS (Multiple Active Results Sets) o transazioni distribuite. Per altre informazioni vedere [Uso di MARS &#40;Multiple Active Result Set&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_getbindtoken [@out_token =] 'return_value' OUTPUT   
```  
  
## <a name="arguments"></a>Argomenti  
 [ @out_token =]'*RETURN_VALUE*'  
 Token da utilizzare per associare le sessioni. *RETURN_VALUE* è di tipo **varchar (255)** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 sp_getbindtoken restituirà un token valido solo quando il stored procedure viene eseguito all'interno di una transazione attiva. In caso contrario, [!INCLUDE[ssDE](../../includes/ssde-md.md)] restituisce un errore. Ad esempio:  
  
```  
-- Declare a variable to hold the bind token.  
-- No active transaction.  
DECLARE @bind_token varchar(255);  
-- Trying to get the bind token returns an error 3921.  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
Server: Msg 3921, Level 16, State 1, Procedure sp_getbindtoken, Line 4  
Cannot get a transaction token if there is no transaction active.  
Reissue the statement after a transaction has been started.  
```  
  
 Quando sp_getbindtoken viene utilizzata per integrare una connessione di transazione distribuita all'interno di una transazione aperta, in viene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituito lo stesso token. Ad esempio:  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @bind_token varchar(255);  
  
BEGIN TRAN;  
  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
  
BEGIN DISTRIBUTED TRAN;  
  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
```  
  
 Entrambe le istruzioni `SELECT` restituiscono lo stesso token:  
  
```  
Token  
-----  
PKb'gN5<9aGEedk_16>8U=5---/5G=--  
(1 row(s_) affected)  
  
Token  
-----  
PKb'gN5<9aGEedk_16>8U=5---/5G=--  
(1 row(s_) affected)  
```  
  
 È possibile utilizzare il token di associazione con sp_bindsession per associare nuove sessioni alla stessa transazione. Il token di associazione è valido solo a livello locale in ogni istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] e non può essere condiviso tra più istanze.  
  
 Per ottenere e passare un token di associazione, è necessario eseguire sp_getbindtoken prima di eseguire sp_bindsession per la condivisione dello stesso spazio di blocco. Se si ottiene un token di associazione, la stored procedure sp_bindsession viene eseguita correttamente.  
  
> [!NOTE]  
>  È consigliabile utilizzare l'API ODS srv_getbindtoken per ottenere un token di associazione da utilizzare in una stored procedure estesa.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene recuperato un token di associazione, di cui viene quindi visualizzato il nome.  
  
```  
DECLARE @bind_token varchar(255);  
BEGIN TRAN;  
EXECUTE sp_getbindtoken @bind_token OUTPUT;  
SELECT @bind_token AS Token;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Token`  
  
 `----------------------------------------------------------`  
  
 `\0]---5^PJK51bP<1F<-7U-]ANZ`  
  
## <a name="see-also"></a>Vedere anche  
 [sp_bindsession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [srv_getbindtoken &#40;API stored procedure estesa&#41;](../../relational-databases/extended-stored-procedures-reference/srv-getbindtoken-extended-stored-procedure-api.md)  
  
  
