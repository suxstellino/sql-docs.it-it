---
title: Salvare i risultati della traccia in una tabella
titleSuffix: SQL Server Profiler
description: Di seguito viene descritto come usare SQL Server Profiler per salvare i risultati di traccia in una tabella in un database SQL Server e come specificare il numero massimo di righe da salvare.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: edbecf74-683b-4e43-a1ef-7a3d5f5e27f6
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 9e1dbe5def1065874cf840245bfaa3f2676af042
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171593"
---
# <a name="save-trace-results-to-a-table-sql-server-profiler"></a>Salvare i risultati della traccia in una tabella (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento viene descritta la procedura per salvare i risultati della traccia in una tabella di database mediante [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
## <a name="to-save-trace-results-to-a-table"></a>Per salvare i risultati della traccia in una tabella
  
1.  Scegliere **Nuova traccia** dal menu **File** e quindi connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Verrà visualizzata la finestra di dialogo **Proprietà traccia**.  
  
    > [!NOTE]  
    >  Se l'opzione **Avvia traccia non appena viene stabilita una connessione** è selezionata, la finestra di dialogo **Proprietà traccia** non viene visualizzata e viene invece avviata la traccia. Per disabilitare questa impostazione, scegliere **Opzioni** dal menu **Strumenti** e deselezionare la casella di controllo **Avvia traccia non appena viene stabilita una connessione** .  
  
2.  Nella casella **Nome traccia** digitare il nome della traccia e quindi fare clic su **Salva nella tabella**.  
  
3.  Nella finestra di dialogo **Connetti al server** connettersi al database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che conterrà la tabella di traccia.  
  
4.  Nella finestra di dialogo **Tabella di destinazione** selezionare un database dall'elenco **Database**.  
  
5.  Nell'elenco **Proprietario** selezionare il proprietario della traccia.  
  
6.  Nell'elenco **Tabella** digitare o selezionare il nome della tabella per i risultati della traccia. Scegliere **OK.**  
  
7.  Nella finestra di dialogo **Proprietà traccia** selezionare la casella di controllo **Numero massimo di righe (in migliaia)** per specificare il numero massimo di righe da salvare.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
