---
description: Ripristino di database abilitati per Stretch (Stretch Database)
title: Restore Stretch-enabled databases (Ripristinare database abilitati per l'estensione)
ms.date: 07/06/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: cebc1f6d-d5ea-460d-ae60-d047d29c2723
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 18c4d602edea0f414863e3fc54bfa58d1e6ca2cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079792"
---
# <a name="restore-stretch-enabled-databases-stretch-database"></a>Ripristino di database abilitati per Stretch (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Ripristinare un database di cui è stato eseguito il backup quando necessario, per il ripristino da molti tipi di errori e guasti.
  
  Per altre informazioni sul backup, vedere [Backup di database abilitati per l'estensione](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md).

> [!TIP]
> Il backup è solo una parte di una soluzione di continuità aziendale e a disponibilità elevata completa. Per altre informazioni sulla disponibilità elevata, vedere [Soluzioni a disponibilità elevata](../../database-engine/sql-server-business-continuity-dr.md).

## <a name="restore-your-sql-server-data"></a>Ripristinare i dati di SQL Server
Per eseguire il ripristino da errori hardware o danneggiamenti, ripristinare il database di SQL Server abilitato per l'estensione da un backup. È possibile continuare a usare i metodi di ripristino di SQL Server già in uso. Per altre informazioni, vedere [Panoramica del ripristino e del recupero](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

Dopo aver ripristinato il database di SQL Server, è necessario eseguire la stored procedure **sys.sp_rda_reauthorize_db** per ristabilire la connessione tra il database di SQL Server abilitato per l'estensione e il database di Azure remoto. Per altre informazioni, vedere [Ripristinare la connessione tra il database di SQL Server e il database di Azure remoto](#reconnect).

## <a name="restore-your-remote-azure-data"></a>Ripristinare i dati di Azure remoti

### <a name="recover-a-live-azure-database"></a>Ripristinare un database di Azure attivo
Il servizio SQL Server Stretch Database su Azure esegue gli snapshot di tutti i dati dinamici almeno ogni 8 ore usando gli snapshot di Archiviazione di Azure. Questi snapshot sono mantenuti per 7 giorni. Questo consente di ripristinare i dati in uno degli almeno 21 punti temporali negli ultimi 7 giorni fino all'ora di creazione dell'ultimo snapshot.

Per ripristinare un database di Azure attivo in un punto temporale precedente tramite il portale di Azure, effettuare le operazioni seguenti.

1. Accedere al [Portale di Azure][].
2. Sul lato sinistro della schermata selezionare **Sfoglia** e quindi selezionare **Database SQL**.
3. Passare al database e selezionarlo.
4. Nella parte superiore del pannello del database, fare clic su **Ripristina**.
5. Specificare un nuovo **Nome database**, selezionare un **Punto di ripristino** e quindi fare clic su **Crea**.
6. Il processo di ripristino del database inizierà e potrà essere monitorato tramite **Notifiche**.

### <a name="recover-a-deleted-azure-database"></a>Ripristinare un database di Azure eliminato
Prima dell'eliminazione del database, il servizio SQL Server Stretch Database in Azure esegue uno snapshot del database e lo conserva per 7 giorni. Dopo l'eliminazione, gli snapshot del database attivo non vengono più conservati. Ciò consente di ripristinare un database eliminato nel punto in cui è stato eliminato.

Per ripristinare un database di Azure eliminato nel punto in cui è stato eliminato tramite il portale di Azure, effettuare le operazioni seguenti.

1. Accedere al [Portale di Azure][].
2. Sul lato sinistro della schermata selezionare **Sfoglia** e quindi selezionare **SQL Server**.
3. Passare al server e selezionarlo.
4. Scorrere verso il basso fino a visualizzare Operazioni nel pannello del server, quindi fare clic sul riquadro **Database eliminati** .
5. Selezionare il database eliminato che si desidera ripristinare.
5. Specificare un nuovo **Nome database** e fare clic su **Crea**.
6. Il processo di ripristino del database inizierà e potrà essere monitorato tramite **Notifiche**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a><a name="reconnect"></a>Ripristinare la connessione tra il database di SQL Server e il database di Azure remoto

1.  Se si prevede di connettersi a un database di Azure ripristinato con un nome diverso o in un'area diversa, eseguire la stored procedure [sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) per disconnettersi dal database di Azure precedente.  
  
2.  Eseguire la stored procedure [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) per riconnettere il database locale abilitato per l'estensione al database di Azure.  
  
    -   Specificare le credenziali con ambito database come valore sysname o varchar(128). Non usare varchar(max). È possibile cercare il nome delle credenziali nella vista **sys.database_scoped_credentials**.  
  
    -   Specificare se creare una copia dei dati remoti e connettersi alla copia (scelta consigliata).  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO  
    ```  
    
  ## <a name="see-also"></a>Vedere anche  
 [Eseguire il backup di database con estensione abilitata](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
 [Gestione e risoluzione dei problemi di Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)   
 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 
 [sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)  
 [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
 
 [Azure portal]: https://portal.azure.com/
 
