---
description: GET_TRANSMISSION_STATUS (Transact-SQL)
title: GET_TRANSMISSION_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STATUS_TSQL
- TRANSMISSION
- TRANSMISSION_TSQL
- GET_TRANSMISSION_STATUS
- STATUS
- GET_TRANSMISSION_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], transmission status
- Service Broker errors, transmission status
- transmission status information
- status information [SQL Server], conversations
- GET_TRANSMISSION_STATUS statement
ms.assetid: 621805d5-49ed-4764-b3cb-2ae4a3bf797e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f5a59676494b664e11b3dbd0ad86a2c9d4d9f434
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091846"
---
# <a name="get_transmission_status-transact-sql"></a>GET_TRANSMISSION_STATUS (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce lo stato dell'ultima trasmissione per un lato di una conversazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GET_TRANSMISSION_STATUS ( conversation_handle )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *conversation_id*  
 Handle della conversazione. Questo parametro è di tipo **uniqueidentifier**.  
  
## <a name="return-types"></a>Tipi restituiti  
 **nchar**  
  
## <a name="remarks"></a>Osservazioni  
 Restituisce una stringa che descrive lo stato dell'ultimo tentativo di trasmissione per la conversazione specificata. Restituisce una stringa vuota se l'ultimo tentativo di trasmissione ha avuto esito positivo, se non è ancora stato effettuato alcun tentativo di trasmissione oppure se *conversation_handle* non esiste.  
  
 Le informazioni restituite da questa funzione corrispondono alle stesse informazioni visualizzate nella colonna last_transmission_error della vista di gestione sys.transmission_queue. Tuttavia, questa funzione può essere utilizzata per ricercare lo stato di trasmissione delle conversazioni che non includono messaggi nella coda di trasmissione.  
  
> [!NOTE]  
>  GET_TRANSMISSION_STATUS non restituisce informazioni per i messaggi che non dispongono di un endpoint di conversazione nell'istanza corrente, ovvero non sono disponibili informazioni per i messaggi da inoltrare.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene visualizzato lo stato di trasmissione della conversazione con handle di conversazione `58ef1d2d-c405-42eb-a762-23ff320bddf0`.  
  
```sql  
SELECT Status =  
    GET_TRANSMISSION_STATUS('58ef1d2d-c405-42eb-a762-23ff320bddf0') ;  
```  
  
 Quello che segue è un set di risultati di esempio, modificato per adattarlo alla lunghezza di riga.  
  
 ```
 Status  
 ------------------------------- 
 The Service Broker protocol transport is disabled or not configured.
 ```  
  
 In questo caso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è configurato in modo da consentire le comunicazioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)] nella rete.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)  
  
  
