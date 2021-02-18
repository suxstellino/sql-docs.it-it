---
title: Filtrare gli eventi in base all'ora di fine
titleSuffix: SQL Server Profiler
description: Filtrare gli eventi in base all'ora di fine durante una traccia. Di seguito viene descritto come configurare un filtro per l'ora di fine dell'evento in SQL Server Profiler.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 74628f9e-2d39-496a-a443-0a3887db223d
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4306910b10fe1ac014226b9013af41b235b5e628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345837"
---
# <a name="filter-events-based-on-the-event-end-time-sql-server-profiler"></a>Filtrare eventi in base all'ora di fine (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrata la procedura per filtrare gli eventi di traccia in base all'ora di fine dell'evento tramite [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-filter-events-based-on-the-event-end-time"></a>Per filtrare gli eventi in base all'ora di fine  
  
1.  Scegliere **Nuova traccia** dal menu **File** e quindi connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Verrà visualizzata la finestra di dialogo **Proprietà traccia**.  
  
    > [!NOTE]  
    >  Se l'opzione **Avvia traccia non appena viene stabilita una connessione** è selezionata, la finestra di dialogo **Proprietà traccia** non viene visualizzata e viene invece avviata la traccia. Per disabilitare questa impostazione, scegliere **Opzioni** dal menu **Strumenti** e deselezionare la casella di controllo **Avvia traccia non appena viene stabilita una connessione** .  
  
2.  Nella finestra di dialogo **Proprietà traccia** verificare che sia selezionata la scheda **Generale** e quindi immettere un nome nella casella di testo **Nome traccia** .  
  
3.  Selezionare un modello di traccia nell'elenco **Modello**.  
  
4.  Facoltativamente, è possibile specificare un file o una tabella di destinazione in cui salvare i risultati della traccia.  
  
5.  Nella scheda **Selezione eventi** fare clic sulla colonna di dati **EndTime** per aprire la finestra di dialogo **Modifica filtro** . È anche possibile fare clic con il pulsante destro del mouse sull'intestazione di colonna e scegliere **Modifica filtro colonne**.  
  
6.  Espandere **Maggiore di** o **Minore di** e immettere un valore **datetime** nel campo visualizzato sotto l'operatore di confronto.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
