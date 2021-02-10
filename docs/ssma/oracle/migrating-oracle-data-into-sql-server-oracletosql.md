---
title: Migrazione di dati Oracle in SQL Server (OracleToSQL) | Microsoft Docs
description: Informazioni su come eseguire la migrazione dei dati da un database Oracle a SQL Server, dopo aver sincronizzato gli oggetti convertiti, utilizzando l'applicazione SSMA per Oracle.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Data Migration, Client-Side Migration
- Oracle Data Migration,Server-Side Migration
ms.assetid: e23c5268-41ed-4e55-9fe7-a11376202a13
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: a63fcd49d37e47485ed82c3b75514b5c1b7d48cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063326"
---
# <a name="migrating-oracle-data-into-sql-server-oracletosql"></a>Migrazione di dati Oracle a SQL Server (OracleToSQL)
Una volta sincronizzati correttamente gli oggetti convertiti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è possibile eseguire la migrazione dei dati da Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
> Se il motore usato è il motore di migrazione dei dati lato server, prima di poter eseguire la migrazione dei dati, è necessario installare SSMA per Oracle Extension Pack e i provider Oracle nel computer che esegue SSMA. È inoltre necessario che il servizio SQL Server Agent sia in esecuzione. Per ulteriori informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti server (OracleToSQL)](./installing-ssma-components-on-sql-server-oracletosql.md) .  
  
## <a name="setting-migration-options"></a>Impostazione delle opzioni di migrazione  
Prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , rivedere le opzioni di migrazione del progetto nella finestra di dialogo **Impostazioni progetto** .  
  
-   Utilizzando questa finestra di dialogo è possibile impostare opzioni quali le dimensioni del batch di migrazione, il blocco della tabella, il controllo dei vincoli, la gestione dei valori null e la gestione dei valori Identity. Per ulteriori informazioni sulle impostazioni di migrazione del progetto, vedere [Impostazioni progetto (migrazione) (OracleToSQL)](./project-settings-migration-oracletosql.md).  
  
-   Il **motore di migrazione** nella finestra di dialogo **Impostazioni progetto** consente all'utente di eseguire il processo di migrazione utilizzando due tipi di motori di migrazione dei dati:  
  
    1.  Motore di migrazione dati lato client  
  
    2.  Motore di migrazione dei dati lato server  
  
**Migrazione dei dati sul lato client:**  
  
-   Per avviare la migrazione dei dati sul lato client, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .  
  
-   In **Impostazioni progetto** viene impostata l'opzione **motore di migrazione dati lato client** .  
  
    > [!NOTE]  
    > Il **motore di migrazione dei dati lato client** si trova all'interno dell'applicazione SSMA e pertanto non dipende dalla disponibilità del pacchetto di estensione.  
  
**Migrazione dei dati lato server:**  
  
-   Durante la migrazione dei dati sul lato server, il motore risiede nel database di destinazione. Viene installato tramite il pacchetto di estensione. Per ulteriori informazioni su come installare il pacchetto di estensione, vedere [installazione dei componenti server in SQL Server](installing-ssma-components-on-sql-server-oracletosql.md)  
  
-   Per avviare la migrazione sul lato server, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .  
  
## <a name="migrating-data-to-sql-server"></a>Migrazione dei dati a SQL Server  
La migrazione dei dati è un'operazione di caricamento bulk che consente di spostare righe di dati da tabelle Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabelle nelle transazioni. Il numero di righe caricate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in ogni transazione viene configurato nelle impostazioni del progetto.  
  
Per visualizzare i messaggi di migrazione, assicurarsi che il riquadro di output sia visibile. In caso contrario, scegliere **output** dal menu **Visualizza** .  
  
**Per eseguire la migrazione dei dati**  
  
1.  Verificare gli elementi seguenti:  
  
    -   I provider Oracle sono installati nel computer che esegue SSMA.  
  
    -   Gli oggetti convertiti sono stati sincronizzati con il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
2.  In Esplora metadati Oracle selezionare gli oggetti che contengono i dati di cui si desidera eseguire la migrazione:  
  
    -   Per eseguire la migrazione dei dati per tutti gli schemi, selezionare la casella di controllo accanto agli **schemi**.  
  
    -   Per eseguire la migrazione dei dati o omettere singole tabelle, espandere innanzitutto lo schema, espandere **tabelle**, quindi selezionare o deselezionare la casella di controllo accanto alla tabella.  
  
3.  Per eseguire la migrazione dei dati, si verificano due casi:  
  
    **Migrazione dei dati sul lato client:**  
  
    -   Per eseguire la **migrazione dei dati lato client**, selezionare l'opzione **motore di migrazione dati lato client** nella finestra di dialogo **Impostazioni progetto** .  
  
    **Migrazione dei dati lato server:**  
  
    -   Prima di eseguire la migrazione dei dati sul lato server, verificare quanto segue:  
  
        1.  SSMA per Oracle Extension Pack è installato nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
        2.  Il servizio SQL Server Agent è in esecuzione nell'istanza di SQL Server.  
  
    -   Per eseguire la **migrazione dei dati sul lato server**, selezionare l'opzione **motore di migrazione dei dati sul lato server** nella finestra di dialogo **Impostazioni progetto** .  
  
4.  Fare clic con il pulsante destro del mouse su **schemi** in Oracle Metadata Explorer, quindi scegliere **Migrate data**. È inoltre possibile eseguire la migrazione dei dati per singoli oggetti o categorie di oggetti: fare clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre. Selezionare l'opzione **Migrate data** .  
  
    > [!NOTE]  
    > Se il pacchetto di estensione SSMA per Oracle non è installato nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il **motore di migrazione dei dati lato server** è selezionato, durante la migrazione dei dati nel database di destinazione si verifica l'errore seguente: "i componenti di migrazione dei dati di SSMA non sono stati trovati in SQL Server, non sarà possibile eseguire la migrazione dei dati sul lato server. Verificare se il pacchetto di estensione è installato correttamente. Fare clic su **Annulla** per terminare la migrazione dei dati.  
  
5.  Nella finestra di dialogo **Connetti a Oracle** immettere le credenziali di connessione, quindi fare clic su **Connetti**. Per ulteriori informazioni sulla connessione a Oracle, vedere [connettersi a oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md)  
  
    Per la connessione al database di destinazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , immettere le credenziali di connessione nella finestra di dialogo **connetti a SQL Server** e fare clic su **Connetti**. Per altre informazioni sulla connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [connettersi a SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md)  
  
    I messaggi verranno visualizzati nel riquadro di **output** . Al termine della migrazione, viene visualizzato il **report migrazione dei dati** . Se non è stata eseguita la migrazione di dati, fare clic sulla riga che contiene gli errori, quindi fare clic su **Dettagli**. Al termine del report, fare clic su **Chiudi**. Per ulteriori informazioni sul report di migrazione dei dati, vedere [report sulla migrazione dei dati (SSMA Common)](../sybase/data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> Quando si usa SQL Express Edition come database di destinazione, è consentita solo la migrazione dei dati lato client e la migrazione dei dati sul lato server non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
