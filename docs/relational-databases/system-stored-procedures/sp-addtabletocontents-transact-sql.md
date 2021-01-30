---
description: sp_addtabletocontents (Transact-SQL)
title: sp_addtabletocontents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 469d9255813e1a072539e6aa8ca7035378ce2b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198404"
---
# <a name="sp_addtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inserisce riferimenti nelle tabelle di rilevamento per le operazioni di merge per tutte le righe di una tabella di origine non incluse nelle tabelle di rilevamento. Utilizzare questa opzione se è stata caricata in blocco una grande quantità di dati utilizzando **bcp**, che non attiverà i trigger di rilevamento di tipo merge. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table_name = ] 'table_name'` Nome della tabella. *table_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @owner_name = ] 'owner_name'` Nome del proprietario della tabella. *owner_name* è di **tipo sysname** e il valore predefinito è null.  
  
`[ @filter_clause = ] 'filter_clause'` Specifica una clausola di filtro che controlla quali righe dei dati appena caricati devono essere aggiunte alle tabelle di rilevamento di tipo merge. *filter_clause* è di **tipo nvarchar (4000)** e il valore predefinito è null. Se *filter_clause* è **null**, vengono aggiunte tutte le righe caricate in blocco.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_addtabletocontents** viene utilizzata solo nella replica di tipo merge.  
  
 Alle righe della *table_name* viene fatto riferimento tramite **ROWGUIDCOL** e i riferimenti vengono aggiunti alle tabelle di rilevamento di tipo merge. **sp_addtabletocontents** deve essere utilizzata dopo la copia bulk dei dati in una tabella pubblicata mediante la replica di tipo merge. La stored procedure inizia il rilevamento delle righe copiate e include le nuove righe nella sincronizzazione successiva.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_addtabletocontents**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
