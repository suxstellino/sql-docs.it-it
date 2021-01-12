---
description: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
title: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT
- SET_QUERY_GOVERNOR_COST_LIMIT_TSQL
- QUERY_GOVERNOR_COST_LIMIT
- QUERY_GOVERNOR_COST_LIMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT statement
- connections [SQL Server], overriding
- QUERY_GOVERNOR_COST_LIMIT option
- overriding connection values
ms.assetid: 3424bb44-6915-462d-a8d7-fe834af81387
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fb2a2c1dda36b6e2378c6197e1e87cc9190a535b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100733"
---
# <a name="set-query_governor_cost_limit-transact-sql"></a>SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Sostituisce il valore dell'opzione **query governor cost limit** configurato per la connessione corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET QUERY_GOVERNOR_COST_LIMIT value  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *value*  
 Valore numerico o integer che specifica la periodo massimo consentito per l'esecuzione di una query. I valori vengono arrotondati per difetto al valore intero più vicino. I valori negativi vengono arrotondati a 0. Query Governor non consente l'esecuzione delle query il cui costo stimato supera tale valore. Se si specifica 0 (valore predefinito), Query Governor verrà disabilitato e sarà possibile eseguire tutte le query senza limiti di tempo.  
  
 Il costo delle query equivale al tempo trascorso, in secondi, stimato per l'esecuzione di una query in una configurazione hardware specifica.  
  
## <a name="remarks"></a>Osservazioni  
 L'opzione SET QUERY_GOVERNOR_COST_LIMIT viene utilizzata solo per la connessione corrente e solo per la durata della connessione corrente. Usare l'opzione [Configurare l'opzione di configurazione del server query governor cost limit](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) di **sp_configure** per modificare il valore del limite di costo di Query Governor per l'intero server. Per altre informazioni sulla configurazione di questa opzione, vedere [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) e [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
 L'opzione SET QUERY_GOVERNOR_COST_LIMIT viene impostata in fase di esecuzione, non in fase di analisi.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
