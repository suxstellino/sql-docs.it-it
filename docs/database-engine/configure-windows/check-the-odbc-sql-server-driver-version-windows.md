---
title: Controllare la versione dei driver ODBC di SQL Server (Windows) | Microsoft Docs
description: Scoprire come usare Amministratore origine dati ODBC di Windows per verificare la versione dei driver ODBC installati nel computer.
ms.custom: ''
ms.date: 11/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- driver version number [ODBC]
- ODBC drivers, version number
ms.assetid: 43451080-a562-4231-b1d4-1ba35ca0ea79
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 885984563072d8673aaf687452c35f8f923414a4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748183"
---
# <a name="check-the-odbc-sql-server-driver-version-windows"></a>Verifica della versione dei driver ODBC di SQL Server (Windows)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  È possibile che nel computer siano installati numerosi driver ODBC di [!INCLUDE[msCoName](../../includes/msconame-md.md)] e di altre società. Questo argomento descrive come usare **Amministratore origine dati ODBC** di Windows per verificare la versione dei driver ODBC installati.  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-check-the-odbc-sql-server-driver-version-32-bit-odbc"></a>Per verificare la versione dei driver ODBC di SQL Server (ODBC a 32 bit)  
  
-   In **Amministratore origine dati ODBC** selezionare la scheda **Driver** .  
  
     Le informazioni relative alla voce Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verranno visualizzate nella colonna **Versione** .  


> [!NOTE]  
>  Per le connessioni all'autenticazione di Azure Active Directory per il database SQL, installare il driver più recente, ad esempio il [driver ODBC 17 per SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md).   

  
## <a name="see-also"></a>Vedere anche  
 [Apertura di Amministratore origine dati ODBC](../../database-engine/configure-windows/open-the-odbc-data-source-administrator.md)  
  
