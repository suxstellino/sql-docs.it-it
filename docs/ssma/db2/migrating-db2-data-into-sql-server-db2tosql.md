---
title: Migrazione di dati DB2 in SQL Server (DB2ToSQL) | Microsoft Docs
description: Informazioni su come eseguire la migrazione dei dati da un database DB2 a SQL Server o al database SQL di Azure, dopo aver sincronizzato gli oggetti convertiti.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 86cbd39f-6dac-409a-9ce1-7dd54403f84b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: cd55ac5432784cb3af727fb923397fa9717dab40
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017801"
---
# <a name="migrating-db2-data-into-sql-server-db2tosql"></a>Migrazione di dati DB2 in SQL Server (DB2ToSQL)
Una volta sincronizzati correttamente gli oggetti convertiti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile migrare i dati da DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
> Se il motore usato è il motore di migrazione dei dati lato server, prima di poter eseguire la migrazione dei dati, è necessario installare il pacchetto di estensione SSMA per DB2 e i provider DB2 nel computer che esegue SSMA. È inoltre necessario che il servizio SQL Server Agent sia in esecuzione. Per ulteriori informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti di SSMA su SQL Server](./installing-ssma-components-on-sql-server-db2tosql.md)  
  
## <a name="setting-migration-options"></a>Impostazione delle opzioni di migrazione  
Prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , rivedere le opzioni di migrazione del progetto nella finestra di dialogo **Impostazioni progetto** .  
  
-   Utilizzando questa finestra di dialogo è possibile impostare opzioni quali le dimensioni del batch di migrazione, il blocco della tabella, il controllo dei vincoli, la gestione dei valori null e la gestione dei valori Identity. Per ulteriori informazioni sulle impostazioni di migrazione del progetto, vedere [Impostazioni progetto (migrazione)](./project-settings-migration-db2tosql.md).  
  
-   Il **motore di migrazione** nella finestra di dialogo **Impostazioni progetto** consente all'utente di eseguire il processo di migrazione utilizzando due tipi di motori di migrazione dei dati:  
  
    1.  Motore di migrazione dati lato client  
  
    2.  Motore di migrazione dei dati lato server  
  
**Migrazione dei dati sul lato client:**  
  
-   Per avviare la migrazione dei dati sul lato client, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .  
  
-   In **Impostazioni progetto** viene impostata l'opzione **motore di migrazione dati lato client** .  
  
    > [!NOTE]  
    > Il **motore di migrazione dei dati lato client** si trova all'interno dell'applicazione SSMA e pertanto non dipende dalla disponibilità del pacchetto di estensione.  
  
**Migrazione dei dati lato server:**  
  
-   Durante la migrazione dei dati sul lato server, il motore risiede nel database di destinazione. Viene installato tramite il pacchetto di estensione. Per ulteriori informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti di SSMA su SQL Server](./installing-ssma-components-on-sql-server-db2tosql.md)  
  
-   Per avviare la migrazione sul lato server, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .  
  
## <a name="migrating-data-to-sql-server"></a>Migrazione dei dati a SQL Server  
La migrazione dei dati è un'operazione di caricamento bulk che sposta le righe di dati dalle tabelle DB2 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabelle nelle transazioni. Il numero di righe caricate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in ogni transazione viene configurato nelle impostazioni del progetto.  
  
Per visualizzare i messaggi di migrazione, assicurarsi che il riquadro di output sia visibile. In caso contrario, scegliere **output** dal menu **Visualizza** .  
  
**Per eseguire la migrazione dei dati**  
  
1.  Verificare gli elementi seguenti:  
  
    -   I provider DB2 sono installati nel computer che esegue SSMA.  
  
    -   Gli oggetti convertiti sono stati sincronizzati con il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
2.  In DB2 Metadata Explorer selezionare gli oggetti che contengono i dati di cui si vuole eseguire la migrazione:  
  
    -   Per eseguire la migrazione dei dati per tutti gli schemi, selezionare la casella di controllo accanto agli **schemi**.  
  
    -   Per eseguire la migrazione dei dati o omettere singole tabelle, espandere innanzitutto lo schema, espandere **tabelle**, quindi selezionare o deselezionare la casella di controllo accanto alla tabella.  
  
3.  Per eseguire la migrazione dei dati, si verificano due casi:  
  
    **Migrazione dei dati sul lato client:**  
  
    -   Per eseguire la **migrazione dei dati lato client**, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .  
  
    **Migrazione dei dati lato server:**  
  
    -   Prima di eseguire la migrazione dei dati sul lato server, verificare quanto segue:  
  
        1.  SSMA per DB2 Extension Pack è installato nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        2.  Il servizio SQL Server Agent è in esecuzione nell'istanza di SQL Server.  
  
    -   Per eseguire la **migrazione dei dati sul lato server**, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .  
  
4.  In DB2 Metadata Explorer fare clic con il pulsante destro del mouse su **schemi** e quindi scegliere **Migrate data**. È inoltre possibile eseguire la migrazione dei dati per singoli oggetti o categorie di oggetti: fare clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre. Selezionare l'opzione **Migrate data** .  
  
    > [!NOTE]  
    > Se il pacchetto di estensioni SSMA per DB2 non è installato nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il **motore di migrazione dei dati lato server** è selezionato, durante la migrazione dei dati nel database di destinazione si verifica l'errore seguente: "i componenti di migrazione dei dati di SSMA non sono stati trovati in SQL Server, non sarà possibile eseguire la migrazione dei dati sul lato server. Verificare se il pacchetto di estensione è installato correttamente. Fare clic su **Annulla** per terminare la migrazione dei dati.  
  
5.  Nella finestra di dialogo **Connetti a DB2** immettere le credenziali di connessione, quindi fare clic su **Connetti**. Per ulteriori informazioni sulla connessione a DB2, vedere [connessione al database db2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
  
    Per la connessione al database di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , immettere le credenziali di connessione nella finestra di dialogo **connetti a SQL Server** e fare clic su **Connetti**. Per ulteriori informazioni sulla connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere la pagina relativa [alla connessione a SQL Server](./connecting-to-sql-server-db2tosql.md)  
  
    I messaggi verranno visualizzati nel riquadro di **output** . Al termine della migrazione, viene visualizzato il **report migrazione dei dati** . Se non è stata eseguita la migrazione di dati, fare clic sulla riga che contiene gli errori, quindi fare clic su **Dettagli**. Al termine del report, fare clic su **Chiudi**. Per ulteriori informazioni sul report di migrazione dei dati, vedere [report sulla migrazione dei dati (SSMA Common)](../sybase/data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> Quando si usa SQL Express Edition come database di destinazione, è consentita solo la migrazione dei dati lato client e la migrazione dei dati sul lato server non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di dati DB2 in SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
