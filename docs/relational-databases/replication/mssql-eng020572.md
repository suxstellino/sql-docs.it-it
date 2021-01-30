---
description: MSSQL_ENG020572
title: MSSQL_ENG020572 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG020572 error
ms.assetid: 636566db-ffcf-4109-8c11-15b8c7cb9cd9
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 72b71abce767a2e988f8608757e34d76dd5fff96
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205583"
---
# <a name="mssql_eng020572"></a>MSSQL_ENG020572
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|20572|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|La sottoscrizione del Sottoscrittore '%s' dell'articolo '%s' della pubblicazione '%s' è stata reinizializzata dopo un errore di convalida.|  
  
## <a name="explanation"></a>Spiegazione  
 I dati nel Sottoscrittore sono stati convalidati in base a quelli nel server di pubblicazione e non corrispondono, pertanto la convalida non è stata eseguita. Quando è stato specificato che è necessario eseguire la convalida, è stata selezionata l'opzione che prevede la reinizializzazione di una sottoscrizione in caso di mancata convalida. La reinizializzazione di una sottoscrizione implica l'applicazione di un nuovo snapshot nel Sottoscrittore. Per ulteriori informazioni sulla convalida, vedere [Validate Replicated Data](../../relational-databases/replication/validate-data-at-the-subscriber.md).  
  
## <a name="user-action"></a>Azione dell'utente  
 I dati nel server di pubblicazione e nel Sottoscrittore corrisponderanno dopo l'applicazione del nuovo snapshot nel Sottoscrittore.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
