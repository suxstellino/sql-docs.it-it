---
title: Avviare una traccia
titleSuffix: SQL Server Profiler
description: Informazioni su come avviare una traccia e individuare i dati dopo aver definito una nuova traccia o creato un modello in SQL Server Profiler.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: aeeb38eb-229a-4c8b-ad66-57e9ce45fb6a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: df7bc3602d03cf6467a022f060db6b5a63f1c747
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341997"
---
# <a name="start-a-trace"></a>Avviare una traccia

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Dopo aver definito una nuova traccia o creato un modello con [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], è possibile avviare, sospendere o arrestare l'acquisizione dei dati in base alla nuova definizione di traccia o modello.  
  
## <a name="starting-a-trace"></a>Avvio di una traccia

Quando viene avviata una traccia e l'origine definita è un'istanza di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] o [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea una coda come posizione temporanea per gli eventi del server acquisiti.  
  
Se si utilizza [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per accedere a Traccia SQL, l'avvio di una traccia comporta l'apertura di una nuova finestra di traccia (se non vi è una finestra già aperta) e i dati vengono acquisiti immediatamente.  
  
Quando si utilizzano le stored procedure di sistema di [!INCLUDE[tsql](../../includes/tsql-md.md)] per l'accesso a Traccia SQL, perché i dati vengano acquisiti è necessario avviare una traccia a ogni avvio di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dopo l'avvio di una traccia è possibile modificarne unicamente il nome.  
  
> [!NOTE]  
>  Quando si utilizzano tracce esistenti, è possibile visualizzare le proprietà ma non modificarle. Per modificare le proprietà, arrestare o sospendere la traccia.  
  
## <a name="see-also"></a>Vedere anche

[Avviare una traccia automaticamente dopo la connessione a un server &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)   

[Sospendere una traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)   

[Arrestare in pausa una traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)   

[Cancellare il contenuto di una finestra di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)   

[Eseguire una traccia dopo la sospensione o l'arresto &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)