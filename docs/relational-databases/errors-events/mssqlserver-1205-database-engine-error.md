---
description: MSSQLSERVER_1205
title: MSSQLSERVER_1205 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 1205 (Database Engine error)
ms.assetid: 9fe3f67c-df3c-4642-a3a4-ccc0e138b632
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a4959ec2bd2a1f58008075c4066da92e7dc99c87
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076463"
---
# <a name="mssqlserver_1205"></a>MSSQLSERVER_1205
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1205|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LK_VICTIM|  
|Testo del messaggio|La transazione (ID di processo %d) è stata interrotta a causa di un deadlock delle risorse %.*ls con un altro processo. Ripetere la transazione.|  
  
## <a name="explanation"></a>Spiegazione

È possibile accedere alle risorse in modo in conflitto in transazioni separate, causando un [deadlock](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks). Ad esempio:  
  
- Transaction1 aggiorna **Tabella1. Row1**, mentre Transaction2 aggiorna **Table2. Row2**
- Transaction1 tenta di aggiornare **Table2. Row2** ma è bloccato perché non è ancora stato eseguito il commit di Transaction2  
- Transaction2 tenta ora di aggiornare **Tabella1. Row1** ma è bloccato perché non è stato eseguito il commit di Transaction1
- Si verifica un deadlock perché Transaction1 è in attesa del completamento di Transaction2 e Transaction2 è in attesa a sua volta del completamento di Transaction1.  
  
Il sistema rileverà il deadlock e sceglierà una delle transazioni incluse come ' vittima '. Emetterà quindi questo messaggio di errore, eseguendo il rollback della transazione della vittima.  Per informazioni dettagliate, vedere [deadlock](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks).

## <a name="user-action"></a>Azione dell'utente  

Eseguire nuovamente la transazione. È inoltre possibile modificare l'applicazione per evitare i deadlock. È possibile tentare di eseguire nuovamente la transazione scelta come vittima. In base alle operazioni che verranno eseguite simultaneamente, è probabile che la transazione abbia esito positivo.  
  
Per evitare che si verifichino deadlock, è consigliabile che tutte le transazioni accedano alle righe nello stesso ordine (**Tabella1**, quindi **Table2**). In questo modo, sebbene sia possibile che si verifichi il blocco, verrà evitato un deadlock.  
  
Per ulteriori informazioni, vedere [gestione dei deadlock](../sql-server-transaction-locking-and-row-versioning-guide.md?#handling-deadlocks) e [riduzione dei deadlock](../sql-server-transaction-locking-and-row-versioning-guide.md#deadlock_minimizing).
