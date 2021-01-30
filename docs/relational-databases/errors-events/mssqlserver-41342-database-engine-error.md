---
description: MSSQLSERVER_41342
title: MSSQLSERVER_41342 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41342 (Database Engine error)
ms.assetid: 28270d98-c543-4e7d-b40c-2200e38dce1c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 638f0758a28a7a2a03a9f9abcbbfb82181948548
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179565"
---
# <a name="mssqlserver_41342"></a>MSSQLSERVER_41342
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID evento|41342|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|HK_HW_NOT_SUPPORTED|  
|Testo del messaggio|Il modello del processore del sistema non supporta la creazione di *costrutto*. Questo errore si verifica in genere nei processori meno recenti.|  
  
## <a name="explanation"></a>Spiegazione  
Per le tabelle con ottimizzazione per la memoria è richiesto un modello di processore che supporta le operazioni di scambio e confronto atomiche su valori a 128 bit, che richiedono l'istruzione di assembly CMPXCHG16B. Alcuni modelli di processore AMD meno recenti non supportano l'istruzione CMPXCHG16B. Inoltre, alcuni ambienti di virtualizzazione non consentono l'utilizzo di questa istruzione per impostazione predefinita.  
  
## <a name="user-action"></a>Azione dell'utente  
Aggiornare il processore. Se il processore supporta l'istruzione e si sta eseguendo SQL Server in una macchina virtuale, modificare la configurazione per supportare l'istruzione CMPXCHG16B.  
  
## <a name="see-also"></a>Vedere anche  
[OLTP in memoria &#40;ottimizzazione per la memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
