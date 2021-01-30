---
title: sys.sp_rda_reconcile_indexes (Transact-SQL) | Microsoft Docs
description: Informazioni sulle sys.sp_rda_reconcile_indexes. Vedere come utilizzare questo stored procedure Transact-SQL per accodare un'attività dello schema per riconciliare gli indici in una tabella remota.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sp_rda_reconcile_indexes
- sp_rda_reconcile_indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_indexes stored procedure
ms.assetid: 96b31ab9-bf84-46d6-9990-81f5c51f885a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28152ad8d8ed10eff6f0576a6fd9f508f0e8fc98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211740"
---
# <a name="syssp_rda_reconcile_indexes-transact-sql"></a>sys.sp_rda_reconcile_indexes (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Accoda un'attività dello schema per riconciliare gli indici nella tabella remota. Al termine di questa attività, la tabella remota presenta gli stessi indici presenti nella tabella abilitata per l'estensione locale.  
  
 Se è presente un'altra attività in coda per riconciliare gli indici quando si chiama **sp_rda_reconcile_indexes**, questo stored procedure non accoda un'attività duplicata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_rda_reconcile_indexes [@objname = ] 'objname'  
  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @objname =] *' objname '*  
 Nome completo o non qualificato della tabella abilitata per l'estensione per la quale si desidera riconciliare gli indici. Le virgolette sono necessarie solo se si specifica un oggetto completo.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o >0 (esito negativo)  
  
## <a name="see-also"></a>Vedere anche  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
