---
description: MSSQLSERVER_12304
title: MSSQLSERVER_12304 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 12304 (Database Engine error)
ms.assetid: a2c252c2-e815-4ac8-a101-7af5b32e3233
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 484046de79a34997f4e0c77cc0ff56f9c2c2452c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197096"
---
# <a name="mssqlserver_12304"></a>MSSQLSERVER_12304
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|12304|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|HK_UNSUPPORTED_IDENTITY_TABLE_VAR|  
|Testo del messaggio|L'utilizzo di un tipo di tabella con ottimizzazione per la memoria in cui viene utilizzata la proprietà IDENTITY con una delle relative colonne non è supportato quando si utilizza il tipo al di fuori del contesto di una stored procedure compilata in modo nativo.|  
  
## <a name="user-action"></a>Azione dell'utente  
Non utilizzare un tipo di tabella ottimizzata per la memoria in cui viene utilizzata la proprietà Identity con una delle relative colonne.  
  
## <a name="see-also"></a>Vedere anche  
[OLTP in memoria &#40;ottimizzazione per la memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
