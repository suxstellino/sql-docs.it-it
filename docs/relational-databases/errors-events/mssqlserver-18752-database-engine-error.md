---
description: MSSQLSERVER_18752
title: MSSQLSERVER_18752 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18752 (Database Engine error)
ms.assetid: 234c58d8-7a1e-4b07-a64b-32a311527980
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 51d1c0e130e0bada6601a9ef10875108da2f233a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196485"
---
# <a name="mssqlserver_18752"></a>MSSQLSERVER_18752
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|18752|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|REPL_INUSE|  
|Testo del messaggio|A un database può connettersi un solo agente di lettura log o una sola procedura correlata ai log (sp_repldone, sp_replcmds e sp_replshowcmds) alla volta. Se è stata eseguita una procedura correlata ai log, eliminare la connessione utilizzata per eseguire la procedura oppure eseguire sp_replflush tramite tale connessione prima di avviare l'agente di lettura log o di eseguire un'altra procedura relativa ai log.|  
  
## <a name="explanation"></a>Spiegazione  
A un database può connettersi solo un agente di lettura log o una sola procedura correlata ai log alla volta.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare che non sia in esecuzione alcun altro agente di lettura log per lo stesso database di pubblicazione e che in nessuna connessione attiva sia stata eseguita in precedenza sp_replcmds, sp_repltrans o sp_repldone senza la successiva esecuzione di sp_replflush o senza disconnessione.  
  
