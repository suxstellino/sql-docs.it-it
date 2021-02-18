---
title: Impostare le dimensioni massime di un tabella di traccia
titleSuffix: SQL Server Profiler
description: Informazioni su come limitare le dimensioni di una tabella di traccia. Usare SQL Server Profiler per specificare il numero massimo di righe consentite per la tabella.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: d0ae83e5-1c88-4a2e-be05-2c341280b978
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 563e01d364c82a4d0c7a58e909ff0e31e875da36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345767"
---
# <a name="set-a-maximum-table-size-for-a-trace-table-sql-server-profiler"></a>Impostare le dimensioni massime di una tabella di traccia (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento viene descritta la procedura per l'impostazione delle dimensioni massime delle tabelle di traccia tramite [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-set-a-maximum-table-size-for-a-trace-table"></a>Per impostare le dimensioni massime di una tabella di traccia  
  
1.  Scegliere **Nuova traccia** dal menu **File** e quindi connettersi a un'istanza di SQL Server.  
  
     Verrà visualizzata la finestra di dialogo **Proprietà traccia**.  
  
    > [!NOTE]  
    >  Se l'opzione **Avvia traccia non appena viene stabilita una connessione** è selezionata, la finestra di dialogo **Proprietà traccia** non viene visualizzata e viene invece avviata la traccia. Per disabilitare questa impostazione, scegliere **Opzioni** dal menu **Strumenti** e deselezionare la casella di controllo **Avvia traccia non appena viene stabilita una connessione** .  
  
2.  Nella casella **Nome traccia** digitare un nome per la traccia.  
  
3.  Nell'elenco **Nome del modello** selezionare un modello di traccia.  
  
4.  Selezionare la casella di controllo **Salva nella tabella**.  
  
5.  Connettersi al server nel quale si desidera archiviare la traccia.  
  
     Verrà visualizzata la finestra di dialogo **Tabella di destinazione** .  
  
6.  Selezionare un database per la traccia dall'elenco **Database** .  
  
7.  Nella casella **Tabella** digitare o selezionare il nome di una tabella.  
  
8.  Selezionare la casella di controllo **Numero massimo di righe** e specificare il numero massimo di righe della tabella di traccia.  
  
    > [!NOTE]  
    >  Quando il numero di righe della tabella supererà il valore massimo specificato, gli eventi di traccia non verranno più registrati. L'esecuzione della traccia, tuttavia, proseguirà.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
