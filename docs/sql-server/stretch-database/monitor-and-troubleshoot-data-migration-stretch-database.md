---
description: Monitoraggio e risoluzione dei problemi di migrazione dei dati (Stretch Database)
title: Monitor and troubleshoot data migration (Monitorare e risolvere i problemi relativi alla migrazione dei dati)
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, monitoring
- monitoring Stretch Database
ms.assetid: 06950858-8c02-4ec6-9c59-42b787316a2d
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f4d46ccfca9642a1be2f9e60d7f3382b4c49e90d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079902"
---
# <a name="monitor-and-troubleshoot-data-migration-stretch-database"></a>Monitoraggio e risoluzione dei problemi di migrazione dei dati (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Per monitorare la migrazione dei dati in Stretch Database Monitor selezionare **Attività | Stretch | Monitoraggio** per un database in SQL Server Management Studio.  
  
## <a name="check-the-status-of-data-migration-in-the-stretch-database-monitor"></a>Controllare lo stato della migrazione dei dati in Stretch Database Monitor  
 Selezionare **Attività | Stretch | Monitoraggio** per un database in SQL Server Management Studio per aprire Stretch Database Monitor e monitorare la migrazione dei dati.  
  
-   Nella parte superiore della finestra di monitoraggio vengono visualizzate informazioni generali sul database SQL Server abilitato per l'estensione e sul database Azure remoto.  
  
-   Nella parte inferiore viene visualizzato lo stato della migrazione dei dati per ogni tabella abilitata per l'estensione nel database.  
  
 ![Monitoraggio di Stretch Database](../../sql-server/stretch-database/media/stretch-monitor.PNG "Monitoraggio di Stretch Database")  
  
##  <a name="check-the-status-of-data-migration-in-a-dynamic-management-view"></a><a name="Migration"></a> Controllare lo stato della migrazione dei dati in una DMV  
 Aprire la DMV **sys.dm_db_rda_migration_status** per visualizzare il numero di batch e righe di dati di cui è stata eseguita la migrazione. Per altre informazioni, vedere [sys.dm_db_rda_migration_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
##  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> Risolvere i problemi relativi alla migrazione dei dati  
 **Non viene eseguita la migrazione in Azure delle righe dalla tabella abilitata per l'estensione. Qual è il problema?**  
 Esistono diversi problemi che possono influire sulla migrazione. Verificare quanto segue.  
  
-   Verificare la connettività di rete per il computer SQL Server.  
  
-   Verificare che il firewall di Azure non stia bloccando la connessione di SQL Server all'endpoint remoto.  
  
-   Verificare lo stato dell'ultimo batch nella DMV **sys.dm_db_rda_migration_status** . Se si è verificato un errore, controllare i valori error_number, error_state, ed error_severity per il batch.  
  
    -   Per altre informazioni sulla vista, vedere [sys.dm_db_rda_migration_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
    -   Per altre informazioni sul contenuto di un messaggio di errore di SQL Server, vedere [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md).  
  
 **Il firewall di Azure blocca le connessioni dal server locale.**  
 Potrebbe essere necessario aggiungere una regola nelle impostazioni del firewall di Azure per il server Azure per consentire a SQL Server di comunicare con il server Azure remoto.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire e risolvere i problemi di Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
  
