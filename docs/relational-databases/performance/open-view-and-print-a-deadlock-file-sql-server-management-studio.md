---
title: Aprire, visualizzare e stampare un file deadlock (SSMS)
description: Informazioni su come acquisire le informazioni sui deadlock generate da SQL Server Profiler e su come visualizzarle in SQL Server Management Studio.
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- deadlocks [SQL Server], printing files
- deadlocks [SQL Server], opening files
- opening deadlock files
- printing deadlock files
ms.assetid: 5061b13f-2cb7-457a-b8d0-fbd437b510ab
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f06fbda4382042ffa51afa0eaa14eb18175874f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483102"
---
# <a name="open-view-and-print-a-deadlock-file-in-sql-server-management-studio-ssms"></a>Aprire, visualizzare e stampare un file deadlock in SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Quando tramite [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] viene generato un deadlock, è possibile acquisire e salvare le informazioni del deadlock in un file. Dopo avere salvato il file deadlock, è possibile aprirlo in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per visualizzarlo o stamparlo.  
  
## <a name="open-and-view-a-deadlock-file"></a>Aprire e visualizzare un file deadlock  
  
1. Scegliere **Apri** dal menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]File **di** e quindi selezionare **File**.  
  
2. Nella finestra di dialogo **Apri file** selezionare il tipo di file con estensione xdl nella casella **Tipo file** . Si ottiene un elenco filtrato contenente solo i file deadlock.  
  
## <a name="print-a-deadlock-file"></a>Stampare un file deadlock  
  
1. Scegliere **Apri** dal menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]File **di** e quindi selezionare **File**.  
  
2. Nella finestra di dialogo **Apri file** selezionare il tipo di file con estensione xdl nella casella **Tipo file** . Si ottiene un elenco filtrato contenente solo i file deadlock.  
  
3. Selezionare il file deadlock che si vuole stampare e quindi scegliere **Apri**.  
  
4. Scegliere **Stampa** dal menu **File**.  
  
## <a name="see-also"></a>Vedere anche  
 [Salvare eventi Deadlock Graph &#40;SQL Server Profiler&#41;](../../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md)  
  
  
