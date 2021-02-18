---
title: Creare un modello di traccia
titleSuffix: SQL Server Profiler
description: Informazioni su come creare un nuovo modello di traccia in SQL Server Profiler. Informazioni su come aggiungere filtri al modello e su come aggiungere o rimuovere eventi e colonne di dati.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 025868b0-3790-4cda-8757-5a58327bfba0
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 453989e908808e0444807bcfb9dd817b3f79c436
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349360"
---
# <a name="create-a-trace-template-sql-server-profiler"></a>Creare un modello di traccia (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come creare un nuovo modello di traccia utilizzando [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-create-a-trace-template"></a>Per creare un modello di traccia  
  
1.  Scegliere **Modelli** dal menu **File** e quindi fare clic su **Nuovo modello**.  
  
2.  Nella finestra di dialogo **Proprietà modello di traccia** selezionare il tipo di server nell'elenco **Tipo server**.  
  
3.  Nella casella **Nome nuovo modello** immettere un nome per il modello.  
  
4.  Facoltativamente, selezionare la casella di controllo **Basa il nuovo modello sul seguente modello esistente** e quindi fare clic su un modello nell'elenco.  
  
     Tutti gli eventi, le colonne di dati e i filtri vengono impostati inizialmente come specificato nel modello selezionato.  
  
5.  Facoltativamente, selezionare la casella di controllo **Usa come modello predefinito per il tipo di server selezionato**.  
  
6.  Nella scheda **Selezione eventi** aggiungere, rimuovere o modificare eventi, colonne di dati o filtri.  
  
7.  Nella scheda **Selezione eventi** usare il controllo griglia per aggiungere o rimuovere eventi e colonne di dati dal file di traccia, nel modo seguente:  
  
    -   Per aggiungere un evento, espandere la categoria di eventi appropriata nella colonna **Eventi** e quindi selezionare il nome dell'evento.  
  
    -   Quando si aggiunge un evento, tutte le colonne di dati significative vengono incluse per impostazione predefinita. Per rimuovere una colonna di dati per un evento da una traccia, deselezionare la casella di controllo nella colonna di dati per l'evento.  
  
    -   Per aggiungere i filtri, fare clic sul nome della colonna di dati e specificare i criteri del filtro nella finestra di dialogo **Modifica filtro** . È anche possibile fare clic con il pulsante destro del mouse sul nome della colonna di dati e scegliere **Modifica filtro colonne** per accedere alla finestra di dialogo **Modifica filtro** . Fare clic su **OK** per aggiungere il filtro.  
  
8.  Fare clic su **Salva**.  
  
## <a name="see-also"></a>Vedere anche  
 [Specificare eventi e colonne di dati per un file di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)   
 [Derivare un modello da una traccia in esecuzione &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)   
 [Derivare un modello da un file di traccia o da una tabella di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)   
 [Modelli e autorizzazioni di SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
