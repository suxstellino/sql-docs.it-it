---
description: MSSQL_ENG020598
title: MSSQL_ENG020598 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG020598 error
ms.assetid: 1c3032f2-23d1-4ead-94ee-16ac4d75cd0c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 09959b146ba41bdadee12e59729c1dbc293eac86
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460224"
---
# <a name="mssql_eng020598"></a>MSSQL_ENG020598
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|20598|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Impossibile trovare la riga nel Sottoscrittore durante l'applicazione del comando replicato.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore viene generato nella replica transazionale se l'agente di distribuzione tenta di aggiornare una riga nel Sottoscrittore mentre la riga è stata eliminata o la chiave primaria della riga è stata modificata. Per impostazione predefinita, i Sottoscrittori di pubblicazioni transazionali devono essere considerati di sola lettura poiché le modifiche non vengono trasferite al server di pubblicazione. Nella replica transazionale, gli utenti possono eseguire modifiche nel Sottoscrittore solo se utilizzano sottoscrizioni aggiornabili o la replica peer-to-peer. Per informazioni sulle opzioni menzionate, vedere [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md) e [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
## <a name="user-action"></a>Azione dell'utente  
 **Per risolvere il problema:**  
  
1.  Se si desidera che la replica continui mentre si identifica l'origine dell'errore, specificare il parametro **-SkipErrors 20598** per l'agente di distribuzione. In questo modo l'agente ignorerà le modifiche che generano l'errore 20598 e consentirà la replica delle altre modifiche.  
  
2.  Identificare le righe eliminate o che presentano una chiave primaria diversa nel Sottoscrittore rispetto alle righe corrispondenti nel server di pubblicazione. È possibile utilizzare la [tablediff Utility](../../tools/tablediff-utility.md) per determinare quali righe risultano diverse nei database di pubblicazione e di sottoscrizione. Per informazioni sull'uso di questa utilità con database di replica, vedere [Confrontare tabelle replicate al fine di individuare le differenze &#40;programmazione della replica&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
3.  Correggere le righe nel Sottoscrittore utilizzando l'utilità **tablediff** o un altro metodo.  
  
4.  (Facoltativo) Rimuovere il parametro **-SkipErrors** .  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
