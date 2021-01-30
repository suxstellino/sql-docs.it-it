---
description: sp_markpendingschemachange (Transact-SQL)
title: sp_markpendingschemachange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_markpendingschemachange
- sp_markpendingschemachange_TSQL
helpviewer_keywords:
- sp_markpendingschemachange
ms.assetid: 01100309-7bef-4154-85bf-f18489577e37
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 412beb1a5afa4fdb24cab38df9e6251c6b4d5ed2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185374"
---
# <a name="sp_markpendingschemachange-transact-sql"></a>sp_markpendingschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stored procedure utilizzata per un migliore supporto delle pubblicazioni di tipo merge, in quanto consente agli amministratori di selezionare le modifiche dello schema in sospeso da ignorare, in modo che non vengano replicate. Questa stored procedure viene eseguita nel database di pubblicazione del server di pubblicazione.  
  
> [!CAUTION]  
>  Con l'esecuzione di questa stored procedure è possibile che modifiche dello schema non vengano replicate. È pertanto consigliabile utilizzarla solo per risolvere problemi non risolti con altri metodi, come la reinizializzazione, oppure quando le soluzioni alternative disponibili sono troppo onerose in termini di prestazioni.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_markpendingschemachange [@publication = ] 'publication'  
    [ , [ @schemaversion = ] schemaversion ]  
    [ , [ @status = ] 'status' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @publication = ] 'publication'` Nome della pubblicazione. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
`[ @schemaversion = ] schemaversion` Identifica una modifica dello schema in sospeso. *SchemaVersion* è di **tipo int** e il valore predefinito è **0**. Utilizzare [sp_enumeratependingschemachanges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-enumeratependingschemachanges-transact-sql.md) per elencare le modifiche dello schema in sospeso per la pubblicazione.  
  
`[ @status = ] 'status'` Indica se una modifica dello schema in sospeso verrà ignorata. *status* è di **tipo nvarchar (10)** e il valore predefinito è **Active**. Se il valore di *status* viene **ignorato**, la modifica dello schema selezionata non verrà replicata.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_markpendingschemachange** viene utilizzato con la replica di tipo merge.  
  
 **sp_markpendingschemachange** è un stored procedure progettato per supportare la replica di tipo merge e deve essere utilizzato solo quando altre azioni correttive, ad esempio la reinizializzazione, non riescono a correggere la situazione o sono troppo onerose in termini di prestazioni.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_markpendingschemachange**.  
  
## <a name="see-also"></a>Vedere anche  
 [sysmergeschemachange &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
