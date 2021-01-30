---
description: MSSQLSERVER_2508
title: MSSQLSERVER_2508 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2508 (Database Engine error)
ms.assetid: c37d40e5-c665-4d66-a727-5cb845634fcc
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8a9779c3d55afed57d00835748ad73b3eaab410b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208494"
---
# <a name="mssqlserver_2508"></a>MSSQLSERVER_2508
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2508|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC_OUT_OF_DATE_PAGE_OR_ROW_COUNT|  
|Testo del messaggio|Il conteggio %.*ls per l'oggetto "%.\*ls", con ID di indice %d, ID di partizione %I64d e ID di unità di allocazione %I64d (tipo %.\*ls), non è corretto. Eseguire DBCC UPDATEUSAGE.|  
  
## <a name="explanation"></a>Spiegazione  
Nelle versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedenti a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] i valori relativi al conteggio delle righe delle tabelle e degli indici e i numeri delle pagine possono essere errati. Nei database creati con versioni precedenti a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] i conteggi potrebbero non essere corretti. Il comando DBCC CHECKDB è stato migliorato per rilevare tali errori e restituisce questo messaggio di avviso quando si verifica un errore di questo tipo.  
  
## <a name="user-action"></a>Azione dell'utente  
Eseguire DBCC UPDATEUSAGE sull'oggetto o l'indice specificato oppure sul database in cui è contenuto l'oggetto per correggere i conteggi non validi.  
  
