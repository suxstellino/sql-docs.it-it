---
title: Usare il profiler XEvent di SQL Server Management Studio
description: Il profiler XEvent apre un visualizzatore live di eventi estesi. Vengono illustrati i vantaggi di questo profiler, le funzionalità principali e come iniziare a visualizzare gli eventi estesi.
ms.date: 10/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: genemi
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- extended events [SQL Server], system health session
- extended events [SQL Server], system_health session
- system_health session [SQL Server extended events]
- system health session [SQL Server extended events]
ms.assetid: 1e1fad43-d747-4775-ac0d-c50648e56d78
author: yualan
ms.author: alayu
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 0cfc93c0e35ccfa81b6570187056dc8e2b500741
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355268"
---
# <a name="use-the-ssms-xevent-profiler"></a>Usare il profiler XEvent di SQL Server Management Studio

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Il profiler XEvent è una funzionalità di SQL Server Management Studio (SSMS) che apre una finestra del visualizzatore degli eventi estesi. Questa panoramica descrive i motivi di utilizzo del profiler, le funzionalità principali e le istruzioni per iniziare a visualizzare gli eventi estesi.

## <a name="why-would-i-use-the-xevent-profiler"></a>Perché usare il profiler XEvent?
A differenza del profiler SQL, il profiler XEvent è integrato direttamente in SSMS ed è basato sulla tecnologia degli eventi estesi del motore SQL. Questa funzionalità consente di accedere rapidamente a una vista streaming live degli eventi di diagnostica del server SQL. Questa vista può essere personalizzata e le personalizzazioni possono essere condivise con altri utenti di SQL Server Management Studio come file con estensione viewsettings. La sessione creata dal profiler XE è meno intrusiva per il server SQL in esecuzione rispetto a una traccia SQL analoga con l'uso del profiler SQL. Questa sessione può essere personalizzata anche dall'utente tramite l'interfaccia utente delle proprietà della sessione XE esistente o tramite TSQL.

## <a name="prerequisites"></a>Prerequisiti
Questa funzionalità è disponibile solo in SQL Server Management Studio (SSMS) v17.3 o versioni successive. Verificare che sia in uso la versione più recente. La versione più recente è disponibile [qui](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="getting-started"></a><a id="getting-started"></a>Introduzione
Per accedere al profiler XEvent, seguire questa procedura:

1. Aprire **SQL Server Management Studio**.

2. Connettersi a un'istanza del motore di database di SQL Server o a localhost.

3. In Esplora oggetti individuare la voce di menu Profiler XE ed espanderla facendo clic sul segno '+'.

   ![Menu Profiler XE](media/xevents-xe-profiler-menu.png)

4. Fare doppio clic su **Standard** per visualizzare tutti gli eventi estesi nella sessione. Fare clic su **T-SQL** per visualizzare le istruzioni SQL. Se non è già stata creata, viene creata automaticamente una sessione.

   ![Sessione del profiler XE](media/xevents-xe-profiler-start-session.png)

5. È ora possibile visualizzare gli eventi estesi.

   ![Visualizzatore del profiler XE](media/xevents-xe-profiler-start-viewer.png)

## <a name="see-also"></a>Vedere anche
[Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
[Strumenti degli eventi estesi](../../relational-databases/extended-events/extended-events-tools.md)  
  
