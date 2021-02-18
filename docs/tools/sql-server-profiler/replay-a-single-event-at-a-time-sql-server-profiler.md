---
title: Riprodurre un solo evento alla volta
titleSuffix: (SQL Server Profiler
description: Informazioni su come riprodurre un evento di traccia alla volta in SQL Server Profiler eseguendo un file di traccia o una tabella di riproduzione.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 220fb192-9636-41a2-b15c-62af6cab8fff
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: b9000da1493b61383f301308d5269fe4349c2e76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353381"
---
# <a name="replay-a-single-event-at-a-time-sql-server-profiler"></a>Riprodurre un solo evento alla volta (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento viene illustrato come riprodurre un evento alla volta in un file o tabella di traccia di riproduzione utilizzando [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-replay-a-single-event-at-a-time"></a>Per riprodurre un solo evento alla volta  
  
1.  Aprire il file o la tabella di traccia che si desidera riprodurre. Per altre informazioni, vedere [Aprire un file di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) o Ottimizzazione guidata [Aprire una tabella di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md).  
  
     Verificare che il file o la tabella di traccia aperta contenga le classi di evento necessarie per la riproduzione. Per altre informazioni, vedere [Requisiti per la riproduzione](../../tools/sql-server-profiler/replay-requirements.md).  
  
2.  Scegliere **Passaggio** dal menu **Riproduci** e quindi connettersi all'istanza del server in cui si vuole riprodurre la traccia.  
  
3.  Nella finestra di dialogo **Configurazione riproduzione** verificare le impostazioni e quindi fare clic su **OK**. Per altre informazioni sulla specifica delle impostazioni nella finestra di dialogo **Configurazione riproduzione**, vedere [Riprodurre un file di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md) o [Riprodurre una tabella di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md).  
  
4.  Per riprodurre il primo evento, fare clic su **OK** nella finestra di dialogo **Configurazione riproduzione** .  
  
5.  Per riprodurre gli eventi successivi, scegliere **Passaggio** dal menu **Riproduci** oppure premere F10. Fare di nuovo clic su **Passaggio** oppure premere di nuovo F10 per ogni evento.  
  
## <a name="see-also"></a>Vedere anche  
 [Riprodurre le tracce](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
