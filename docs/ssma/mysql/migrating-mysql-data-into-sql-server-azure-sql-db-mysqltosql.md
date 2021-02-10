---
description: Migrazione dei dati MySQL in SQL Server-database SQL di Azure (MySQLToSQL)
title: Migrazione dei dati MySQL in SQL Server-database SQL di Azure (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Data Migration, server side data migration
- Data Migration,client side data migration
ms.assetid: a6a7f4d6-68aa-4a38-93bf-53eba0d7dc82
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3207dc38dd777ee6ccc36e37e9b18414dbea085e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017528"
---
# <a name="migrating-mysql-data-into-sql-server---azure-sql-database-mysqltosql"></a>Migrazione dei dati MySQL in SQL Server-database SQL di Azure (MySQLToSQL)
Dopo aver sincronizzato correttamente gli oggetti convertiti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è possibile eseguire la migrazione dei dati da MySQL a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
> [!IMPORTANT]  
> Se il motore usato è il motore di migrazione dei dati lato server, prima di eseguire la migrazione dei dati, è necessario installare il pacchetto di estensione SSMA per MySQL e i provider MySQL nel computer che esegue SSMA. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]È inoltre necessario che il servizio Agent sia in esecuzione. Per ulteriori informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti SSMA in SQL Server (da MySQL a SQL)](./installing-ssma-components-on-sql-server-mysqltosql.md) .  
  
## <a name="setting-migration-options"></a>Impostazione delle opzioni di migrazione  
Prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, esaminare le opzioni di migrazione del progetto nella finestra di dialogo **Impostazioni progetto** .  
  
-   Utilizzando questa finestra di dialogo è possibile impostare opzioni quali le dimensioni del batch di migrazione, il blocco della tabella, il controllo dei vincoli, la gestione dei valori null e la gestione dei valori Identity. Per ulteriori informazioni sulle impostazioni di migrazione del progetto, vedere [Impostazioni progetto (migrazione)](./project-settings-migration-mysqltosql.md).  
  
    Per altre informazioni sulle **impostazioni di migrazione dei dati estese**, vedere [impostazioni di migrazione dei dati](data-migration-settings-mysqltosql.md)  
  
-   Il **motore di migrazione** nella finestra di dialogo **Impostazioni progetto** consente all'utente di eseguire il processo di migrazione utilizzando due tipi di motori di migrazione dei dati:  
  
    1.  Motore di migrazione dati lato client  
  
    2.  Motore di migrazione dei dati lato server  
  
**Migrazione dei dati sul lato client:**  
  
-   Per avviare la migrazione dei dati sul lato client, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .  
  
-   In **Impostazioni progetto** viene impostata l'opzione **motore di migrazione dati lato client** .  
  
    > [!NOTE]  
    > Il **motore di migrazione dei dati lato client** si trova all'interno dell'applicazione SSMA e pertanto non dipende dalla disponibilità del pacchetto di estensione.  
  
**Migrazione dei dati lato server:**  
  
-   Durante la migrazione dei dati sul lato server, il motore risiede nel database di destinazione. Viene installato tramite il pacchetto di estensione. Per altre informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti SSMA in SQL Server (da MySQL a SQL)](./installing-ssma-components-on-sql-server-mysqltosql.md) .  
  
-   Per avviare la migrazione sul lato server, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .  
  
> [!IMPORTANT]  
> L'opzione di **migrazione dati lato client** è disponibile solo per SQL Azure.  
  
## <a name="migrating-data-to-sql-server-or-sql-azure"></a>Migrazione dei dati a SQL Server o SQL Azure  
La migrazione dei dati è un'operazione di caricamento bulk che sposta le righe di dati dalle tabelle MySQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure tabelle nelle transazioni. Il numero di righe caricate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in ogni transazione viene configurato nelle impostazioni del progetto.  
  
Per visualizzare i messaggi di migrazione, assicurarsi che il riquadro di output sia visibile. In caso contrario, scegliere **output** dal menu **Visualizza** .  
  
**Per eseguire la migrazione dei dati**  
  
1.  Verificare gli elementi seguenti:  
  
    -   I provider MySQL sono installati nel computer che esegue SSMA.  
  
    -   Gli oggetti convertiti sono stati sincronizzati con il database di destinazione (SQL Server/SQL Azure).  
  
2.  In MySQL Metadata Explorer selezionare gli oggetti che contengono i dati di cui si vuole eseguire la migrazione:  
  
    -   Per eseguire la migrazione dei dati per tutti gli schemi, selezionare la casella di controllo accanto agli **schemi**.  
  
    -   Per eseguire la migrazione dei dati o omettere singole tabelle, espandere innanzitutto lo schema, espandere **tabelle**, quindi selezionare o deselezionare la casella di controllo accanto alla tabella.  
  
3.  Per eseguire la migrazione dei dati, si verificano due casi:  
  
    **Migrazione dei dati sul lato client:**  
  
    -   Per eseguire la **migrazione dei dati lato client**, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .  
  
    **Migrazione dei dati lato server:**  
  
    -   Prima di eseguire la migrazione dei dati sul lato server, verificare quanto segue:  
  
        1.  Il pacchetto di estensione SSMA per MySQL è installato nell'istanza di SQL Server.  
  
        2.  Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio Agent è in esecuzione nell'istanza di SQL Server  
  
    -   Per eseguire la **migrazione dei dati sul lato server**, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .  
  
4.  Fare clic con il pulsante destro del mouse su **schemi** in MySQL Metadata Explorer, quindi scegliere **Migrate data**. È inoltre possibile eseguire la migrazione dei dati per singoli oggetti o categorie di oggetti: fare clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre. Selezionare l'opzione **Migrate data** .  
  
    > [!NOTE]  
    > Se il pacchetto di estensione SSMA per MySQL non è installato nell'istanza di SQL Server e se è selezionato **motore di migrazione dei dati lato server** , durante la migrazione dei dati nel database di destinazione si verifica l'errore seguente: "i componenti di migrazione dei dati di SSMA non sono stati trovati nel SQL Server, non sarà possibile eseguire la migrazione dei dati sul lato server. Verificare se il pacchetto di estensione è installato correttamente. Fare clic su **Annulla** per terminare la migrazione dei dati.  
  
5.  Nella finestra di dialogo **Connetti a MySQL** immettere le credenziali di connessione e quindi fare clic su **Connetti**. Per altre informazioni sulla connessione a MySQL, vedere [connettersi a mysql &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
    Se il database di destinazione è SQL Server, immettere le credenziali di connessione nella finestra di dialogo **Connetti a SQL Server** , quindi fare clic su **Connetti**. Per altre informazioni sulla connessione a SQL Server, vedere [connettersi a SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md)  
  
    Se il database di destinazione è SQL Azure, immettere le credenziali di connessione nella finestra di dialogo **Connetti a SQL Azure** , quindi fare clic su **Connetti**. Per altre informazioni sulla connessione a SQL Azure, vedere [connettersi al database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-azure-sql-db-mysqltosql.md)  
  
    I messaggi verranno visualizzati nel riquadro di **output** . Al termine della migrazione, viene visualizzato il **report migrazione dei dati** . Se non è stata eseguita la migrazione di dati, fare clic sulla riga che contiene gli errori, quindi fare clic su **Dettagli**. Al termine del report, fare clic su **Chiudi**. Per ulteriori informazioni sul report di migrazione dei dati, vedere [report sulla migrazione dei dati (SSMA Common)](../sybase/data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> Quando si usa SQL Express Edition come database di destinazione, è consentita solo la migrazione dei dati lato client e la migrazione dei dati sul lato server non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
