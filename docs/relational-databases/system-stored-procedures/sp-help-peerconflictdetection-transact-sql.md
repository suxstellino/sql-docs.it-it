---
description: sp_help_peerconflictdetection (Transact-SQL)
title: sp_help_peerconflictdetection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_help_peerconflictdetection
- sp_help_peerconflictdetection_TSQL
helpviewer_keywords:
- sp_help_peerconflictdetection
ms.assetid: 59e04107-5eaa-44a1-beb6-ac4f2dbbcb28
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 90cd963b22cffd11c400f18a91f2badefff2e799
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199612"
---
# <a name="sp_help_peerconflictdetection-transact-sql"></a>sp_help_peerconflictdetection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce informazioni sulle impostazioni di rilevamento dei conflitti per una pubblicazione coinvolta in una topologia di replica transazionale peer-to-peer.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_peerconflictdetection [ @publication = ] 'publication'  
    [ ,[ @timeout = ] timeout ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @publication =]'*pubblicazione*'  
 Nome della pubblicazione per cui si desidera restituire le informazioni. *Publication* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
 [ @timeout =] *timeout*  
 Specifica l'intervallo di tempo, espresso in secondi, dopo il quale si verifica il timeout della procedura durante l'attesa di una risposta da ogni nodo nella topologia. Se nella topologia è presente un Sottoscrittore di sola lettura, non sarà possibile specificare un valore di timeout. I Sottoscrittori di sola lettura non rispondono mai a una chiamata da questa procedura. *timeout* è di **tipo int** e il valore predefinito è 60.  
  
## <a name="result-sets"></a>Set di risultati  
 sp_help_peerconflictdetection restituisce tre set di risultati. Questi risultati sono documentati negli argomenti seguenti:  
  
-   [MSpeer_conflictdetectionconfigrequest](../../relational-databases/system-tables/mspeer-conflictdetectionconfigrequest-transact-sql.md)  
  
-   [MSpeer_conflictdetectionconfigresponse](../../relational-databases/system-tables/mspeer-conflictdetectionconfigresponse-transact-sql.md)  
  
-   [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md)  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 sp_help_peerconflictdetection è utilizzato nella replica transazionale peer-to-peer.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server sysadmin o al ruolo predefinito del database db_owner.  
  
## <a name="see-also"></a>Vedere anche  
 [Rilevamento dei conflitti nella replica peer-to-peer](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
