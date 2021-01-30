---
description: MSSQLSERVER_1458
title: MSSQLSERVER_1458 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1458 (Database Engine error)
ms.assetid: adc78c59-a6f2-432b-9a07-fdd1dc2b9026
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c9869ae430c0d1b669ced02202642e5eee7a7b58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196991"
---
# <a name="mssqlserver_1458"></a>MSSQLSERVER_1458
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1458|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBM_FAILREDO_ON_PRIMARY|  
|Testo del messaggio|La copia principale del database '%.*ls' ha rilevato l'errore % d, con stato% d e gravità% d. Il mirroring del database è stato sospeso. Risolvere il problema e riprendere il mirroring.|  
  
## <a name="explanation"></a>Spiegazione  
Il messaggio indica che il database principale ha rilevato un errore che ha causato la sospensione del mirroring del database.  
  
## <a name="user-action"></a>Azione dell'utente  
Nella maggior parte dei casi per la risoluzione di questo errore non è richiesto l'intervento dell'utente. Se il problema persiste, è in genere sufficiente riavviare l'istanza del database o del server per correggerlo. Per ulteriori informazioni, ricercare nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di ogni partner l'errore associato che ha preceduto la visualizzazione del messaggio.  
  
## <a name="see-also"></a>Vedere anche  
[Monitoraggio del mirroring del database &#40;SQL Server&#41;](~/database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
