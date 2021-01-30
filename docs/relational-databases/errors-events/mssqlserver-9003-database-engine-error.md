---
description: MSSQLSERVER_9003
title: MSSQLSERVER_9003 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9003 (Database Engine error)
ms.assetid: 7fdfb391-5c6f-428b-b434-6c3d0b30fd7b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 37070cdc53894b703674e4bd696cd162796351fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161973"
---
# <a name="mssqlserver_9003"></a>MSSQLSERVER_9003
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|9003|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LOG_INVALID_LSN|  
|Testo del messaggio|Il numero di analisi log %S_LSN passato alla funzione di analisi dei log nel database '%.*ls' non è valido. L'errore può indicare la presenza di dati danneggiati o un problema di mancata corrispondenza tra il file di log (con estensione ldf) e il file di dati (con estensione mdf). Se l'errore si è verificato durante la replica, ricreare la pubblicazione. Se invece il problema provoca un errore all'avvio, eseguire il ripristino da un backup.|  
  
## <a name="explanation"></a>Spiegazione  
Un componente ha passato un numero di sequenza del file di log (LSN) non valido allo strumento di gestione del log per il database. L'errore potrebbe verificarsi durante la replica oppure in presenza di dati danneggiati o di incoerenza tra il file di dati primario e il log.  
  
## <a name="user-action"></a>Azione dell'utente  
Se l'errore si è verificato durante la replica, ricreare la pubblicazione. In caso contrario, eseguire un ripristino dal backup.  
  
