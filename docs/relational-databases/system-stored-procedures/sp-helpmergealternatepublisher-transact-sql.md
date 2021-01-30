---
description: sp_helpmergealternatepublisher (Transact-SQL)
title: sp_helpmergealternatepublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergealternatepublisher_TSQL
- sp_helpmergealternatepublisher
helpviewer_keywords:
- sp_helpmergealternatepublisher
ms.assetid: a96e365f-5967-4580-9d79-5bacf2d12211
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7448059958f07ba1aab7e63a1a857e113e21432
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183107"
---
# <a name="sp_helpmergealternatepublisher-transact-sql"></a>sp_helpmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un elenco di tutti i server abilitati come server di pubblicazione alternativi per le pubblicazioni di tipo merge. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpmergealternatepublisher [ @publisher = ] 'publisher', [ @publisher_db = ] 'publisher_db', [ @publication = ] 'publication'  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publisher = ] 'publisher'` Nome del server di pubblicazione alternativo. *Publisher* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publisher_db = ] 'publisher_db'` Nome del database di pubblicazione. *publisher_db* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**alternate_publisher**|**sysname**|Nome del server di pubblicazione alternativo.|  
|**alternate_publisher_db**|**sysname**|Nome del database di pubblicazione.|  
|**alternate_publication**|**sysname**|Nome della pubblicazione.|  
|**alternate_distributor**|**sysname**|Nome del server di distribuzione.|  
|**friendly_name**|**nvarchar(255)**|Descrizione del server di pubblicazione alternativo.|  
|**abilitato**|**bit**|Specifica se il server è un server di pubblicazione alternativo. **1** specifica che il server di pubblicazione è abilitato come server di pubblicazione alternativo. **0** indica che non è abilitato.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_helpmergealternatepublisher** viene utilizzata nella replica di tipo merge.  
  
 Durante ogni sessione di merge viene eseguita una ricerca dell'elenco dei server di pubblicazione alternativi sia nel server di pubblicazione che nel Sottoscrittore. Il processo di merge aggiunge o elimina voci nell'elenco dei server di pubblicazione alternativi, in modo che entrambi gli elenchi nel Sottoscrittore e nel server di pubblicazione corrispondano.  
  
## <a name="permissions"></a>Autorizzazioni  
 È possibile eseguire **sp_helpmergealternatepublisher** solo i membri dell'elenco di accesso alla pubblicazione per la pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
