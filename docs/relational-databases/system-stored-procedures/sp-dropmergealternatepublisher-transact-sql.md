---
description: sp_dropmergealternatepublisher (Transact-SQL)
title: sp_dropmergealternatepublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergealternatepublisher
- sp_dropmergealternatepublisher_TSQL
helpviewer_keywords:
- sp_dropmergealternatepublisher
ms.assetid: a7dee4e2-2a60-41da-9d1d-6f991d7e2c5e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d787648f197c54623ba305503ddd878a3a7061d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208278"
---
# <a name="sp_dropmergealternatepublisher-transact-sql"></a>sp_dropmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rimuove un server di pubblicazione alternativo da una pubblicazione di tipo merge. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropmergealaternatepublisher [ @publisher = ] 'publisher'    , [ @publisher_db = ] 'publisher_db'    , [ @publication = ] 'publication'    , [ @alternate_publisher = ] 'alternate_publisher'    , [ @alternate_publisher_db = ] 'alternate_publisher_db'    , [ @alternate_publication = ] 'alternate_publication'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione corrente. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione corrente. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione corrente. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_publisher = ] 'alternate_publisher'` Nome del server di pubblicazione alternativo da eliminare come partner di sincronizzazione alternativo. *alternate_publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_publisher_db = ] 'alternate_publisher_db'` Nome del database di pubblicazione da eliminare come database di pubblicazione partner alternativo per la sincronizzazione. *alternate_publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @alternate_publication = ] 'alternate_publication'` Nome della pubblicazione da eliminare come pubblicazione del partner di sincronizzazione alternativo. *alternate_publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_dropmergealternatepublisher** viene utilizzata nella replica di tipo merge.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_dropmergelternatepublisher**.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_addmergealternatepublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergealternatepublisher-transact-sql.md)  
  
  
