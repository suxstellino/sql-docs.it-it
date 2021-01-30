---
title: sys.sp_rda_set_query_mode (Transact-SQL) | Microsoft Docs
description: Utilizzare sys.sp_rda_set_query_mode per specificare se le query eseguite sul database corrente con estensione abilitato e sulle relative tabelle restituiscono dati locali e remoti o solo dati locali.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_set_query_mode
- sys.sp_rda_set_query_mode_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_set_query_mode stored procedure
ms.assetid: 65a0b390-cf87-4db7-972a-1fdf13456c88
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed9e5afda379b094a61c9f6d3130b986e0b83ca3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211731"
---
# <a name="syssp_rda_set_query_mode-transact-sql"></a>sys.sp_rda_set_query_mode (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Specifica se le query eseguite sul database corrente abilitato per l'estensione e sulle relative tabelle restituiscono dati locali e remoti (impostazione predefinita) o solo dati locali.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_rda_set_query_mode [ @mode = ] @mode   
    [ , [ @force = ]  @force ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @mode =] *\@ modalità*  
 È uno dei valori seguenti.  
  
-   **Disabilitato** Tutte le query sulle tabelle abilitate per l'estensione hanno esito negativo.  
  
-   **LOCAL_ONLY** Le query sulle tabelle abilitate per l'estensione restituiscono solo dati locali.  
  
-   **LOCAL_AND_REMOTE** Le query sulle tabelle abilitate per l'estensione restituiscono dati locali e remoti. Comportamento predefinito.  
  
 [ @force =] *\@ forza*  
 È un valore di bit facoltativo che è possibile impostare su 1 se si desidera modificare la modalità query senza convalida.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o >0 (esito negativo)  
  
## <a name="permissions"></a>Autorizzazioni  
 Richiede autorizzazioni db_owner.  
  
## <a name="remarks"></a>Commenti  
 Le stored procedure estese seguenti consentono inoltre di impostare la modalità di query per un database abilitato per l'estensione.  
  
-   **sp_rda_deauthorize_db**  
  
     Dopo l'esecuzione di **sp_rda_deauthorize_db** , tutte le query sulle tabelle e i database abilitati per l'estensione hanno esito negativo. Ovvero la modalità query è impostata su DISABLEd. Per uscire da questa modalità, eseguire una delle operazioni seguenti.  
  
    -   Eseguire [sys.sp_rda_reauthorize_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) per riconnettersi al database di Azure remoto. Questa operazione Reimposta automaticamente la modalità di query su LOCAL_AND_REMOTE, che rappresenta il comportamento predefinito per Stretch Database. Ovvero le query restituiscono i risultati dai dati locali e remoti.  
  
    -   Eseguire [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) con l'argomento LOCAL_ONLY per consentire l'esecuzione delle query solo sui dati locali.  
  
-   **sp_rda_reauthorize_db**  
  
     Quando si esegue [sys.sp_rda_reauthorize_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) per riconnettersi al database di Azure remoto, questa operazione Reimposta automaticamente la modalità di query su LOCAL_AND_REMOTE, che è il comportamento predefinito per Stretch database. Ovvero le query restituiscono i risultati dai dati locali e remoti.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.sp_rda_deauthorize_db &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)   
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
