---
description: MSSQLSERVER_7308
title: MSSQLSERVER_7308 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7308 (Database Engine error)
- single-threaded apartment mode
ms.assetid: cca9eab9-afb9-463d-abfd-0802257e2c99
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f01b6e1fb5d73daf947eb7d0f3e3bfd249dbd17f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209162"
---
# <a name="mssqlserver_7308"></a>MSSQLSERVER_7308
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|7308|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|RMT_STA_DISABLED|  
|Testo del messaggio|Il provider OLE DB '%ls' è configurato per l'esecuzione in modalità apartment a thread singolo. Impossibile utilizzarlo per le query distribuite.|  
  
## <a name="explanation"></a>Spiegazione  
È stato utilizzato un provider OLE DB configurato per l'esecuzione in modalità apartment a thread singolo. I provider OLE DB in esecuzione in modalità apartment a thread singolo non possono essere utilizzati per le query distribuite.  
  
## <a name="user-action"></a>Azione dell'utente  
Per risolvere l'errore, configurare il provider per l'esecuzione in modalità apartment a thread multipli. Se il provider non supporta la modalità MTA e non può essere aggiornato a una versione che la supporta, configurarlo per l'esecuzione out-of-process. Il fornitore del provider deve essere in grado di indicare se il provider supporta la modalità MTA o l'esecuzione out-of-process.  
  
