---
description: sp_resetsnapshotdeliveryprogress (Transact-SQL)
title: sp_resetsnapshotdeliveryprogress (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_resetsnapshotdeliveryprogress
- sp_resetsnapshotdeliveryprogress_TSQL
helpviewer_keywords:
- sp_resetsnapshotdeliveryprogress
ms.assetid: 5df7d86b-d343-4d9b-88b1-74429ed092e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4b0d858fdaf03a50a4d87447dc57c09f4a79792c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158419"
---
# <a name="sp_resetsnapshotdeliveryprogress-transact-sql"></a>sp_resetsnapshotdeliveryprogress (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Reimposta il processo di recapito degli snapshot per una sottoscrizione pull in modo che il recapito degli snapshot possa essere riavviato. Questa stored procedure viene eseguita nel database di sottoscrizione del Sottoscrittore.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_resetsnapshotdeliveryprogress [ [ @verbose_level = ] verbose_level ]  
    [ , [ @drop_table = ] 'drop_table' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @verbose_level = ] verbose_level` Specifica la quantità di informazioni restituite. *verbose_level* è di **tipo int** e il valore predefinito è **1**. Il valore **1** indica che viene restituito un errore se non è possibile ottenere i blocchi necessari nella tabella **MSsnapshotdeliveryprogress** , mentre **0** indica che non viene restituito alcun errore.  
  
`[ @drop_table = ] 'drop_table'` Indica se eliminare o troncare la tabella contenente le informazioni sullo stato di avanzamento dello snapshot. *drop_table* è di **tipo nvarchar (5)** e il valore predefinito è **false**. FALSE indica che la tabella viene troncata, mentre TRUE indica che la tabella è stata eliminata.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 **sp_resetsnapshotdeliveryprogress** vengono rimosse tutte le righe nella tabella **MSsnapshotdeliveryprogress** . Vengono effettivamente rimossi tutti i metadati rimasti nel database di sottoscrizione in seguito alle precedenti fasi dei processi di recapito degli snapshot.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** possono eseguire **sp_resetsnapshotdeliveryprogress**.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
