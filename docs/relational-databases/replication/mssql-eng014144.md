---
description: MSSQL_ENG014144
title: MSSQL_ENG014144 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014144 error
ms.assetid: fdc744d5-530e-48c4-9420-cca032fd482b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6469373e327d4b7eef48819f3f266c36a10a3237
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469112"
---
# <a name="mssql_eng014144"></a>MSSQL_ENG014144
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|14144|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Al Sottoscrittore '%s' sono associate sottoscrizioni nel database di pubblicazione '%s'. Impossibile eliminarlo.|  
  
## <a name="explanation"></a>Spiegazione  
 Non è possibile rimuovere dal ruolo del Sottoscrittore un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che è configurata come Sottoscrittore fino a che vi sono sottoscrizioni attive configurate per quell'istanza.  
  
## <a name="user-action"></a>Azione dell'utente  
 Eliminare tutte le sottoscrizioni associate prima di tentare di modificare lo stato del Sottoscrittore dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
1.  Per trovare le sottoscrizioni, eseguire [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md) nel database di pubblicazione sul server di pubblicazione.  
  
2.  Per eliminare le sottoscrizioni, eseguire [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md) nel database di pubblicazione.  

## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Sottoscrizione delle pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
