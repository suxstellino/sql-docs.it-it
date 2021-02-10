---
title: Connessione a SAP ASE (SybaseToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un server adattivo per eseguire la migrazione di un database di SAP Adaptive Server Enterprise (ASE) a SQL Server o al database SQL di Azure.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Sybase
ms.assetid: a45a2330-9175-4c9e-af38-ef920e350614
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7afb893d194b874da4cbb6482df4ba8a16e8c0a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056866"
---
# <a name="connecting-to-sap-ase-sybasetosql"></a>Connessione a SAP ASE (SybaseToSQL)

Per eseguire la migrazione dei database di SAP Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, è necessario connettersi al server adattivo che contiene i database di cui si desidera eseguire la migrazione. Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti i database nel server adattivo e Visualizza i metadati del database nel riquadro di Esplora metadati Sybase. SSMA archivia le informazioni sul server di database, ma non archivia le password.  
  
La connessione all'ambiente del servizio app rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi all'ambiente del servizio app se si vuole una connessione attiva al server.  
  
I metadati relativi al server adattivo non vengono aggiornati automaticamente. Se invece si desidera aggiornare i metadati in Sybase Metadata Explorer, è necessario aggiornare manualmente i metadati, come descritto nella sezione "aggiornamento dei metadati di Sybase ASE" più avanti in questo argomento.  
  
## <a name="required-ase-permissions"></a>Autorizzazioni ASE richieste

L'account usato per la connessione all'ambiente del servizio app deve avere almeno l'accesso **pubblico** al database master e ai database di origine di cui eseguire la migrazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Inoltre, per selezionare le autorizzazioni per le tabelle di cui viene eseguita la migrazione, l'utente deve disporre delle autorizzazioni SELECT per le tabelle di sistema seguenti:  
  
- oggetti [source_db] .dbo.sys  
- [source_db] .dbo.syscolonne  
- [source_db] .dbo.sysutenti  
- tipi di .dbo.sys[source_db]  
- [source_db] vincoli .dbo.sys  
- [source_db] Commenti .dbo.sys  
- [source_db] .dbo.sysindici  
- [source_db] .dbo.sysriferimenti  
- Database master.dbo.sys  
  
## <a name="establishing-a-connection-to-ase"></a>Stabilire una connessione all'ambiente del servizio app

Quando ci si connette a un server adattivo, SSMA legge i metadati del database nel server di database e quindi aggiunge i metadati al file di progetto. Questi metadati vengono usati da SSMA quando converte gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure sintassi e quando esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. È possibile esplorare questi metadati nel riquadro di Esplora metadati Sybase ed esaminare le proprietà dei singoli oggetti di database.  
  
> [!IMPORTANT]  
> Prima di provare a connettersi al server di database, verificare che il server di database sia in esecuzione e che sia in grado di accettare le connessioni.  
  
**Per connettersi a Sybase ASE**
  
1. Scegliere **Connetti a Sybase** dal menu **file** .  
  
   Se in precedenza si è connessi a Sybase, il nome del comando verrà **riconnesso a Sybase**.  
  
2. Nella casella **provider** selezionare uno dei provider installati nel computer per la connessione al Server Sybase.  
  
3. Nella casella **modalità** selezionare **modalità standard** o **modalità avanzata**.  
  
   Utilizzare la modalità standard per specificare il nome del server, la porta, il nome utente e la password. Utilizzare la modalità avanzata per specificare una stringa di connessione. Questa modalità viene in genere usata solo per la risoluzione dei problemi o per l'uso del supporto tecnico.  
  
4. Se si seleziona la **modalità standard**, fornire i valori seguenti:  
  
    1. Nella casella **nome server** immettere o selezionare il nome o l'indirizzo IP del server di database.  
    2. Se il server di database non è configurato per accettare le connessioni sulla porta predefinita (5000), immettere il numero di porta usato per le connessioni Sybase nella casella **porta server** .  
    3. Nella casella **nome utente** immettere un account Sybase con le autorizzazioni necessarie.  
    4. Nella casella **password** immettere la password per il nome utente specificato.  
  
5. Se si seleziona la **modalità avanzata**, specificare una stringa di connessione nella casella **stringa di connessione** .  
  
    Di seguito sono riportati alcuni esempi di diverse stringhe di connessione:  
  
    1. **Stringhe di connessione per il provider di OLE DB Sybase:**  
  
        Per Sybase ASE OLE DB 12,5, una stringa di connessione di esempio è la seguente:  
  
        `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`  
  
        Per Sybase ASE OLE DB 15, una stringa di connessione di esempio è la seguente:  
  
        `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider= ASEOLEDB;Port=5000;`  
  
    2. **Stringa di connessione per il provider ODBC Sybase:**  
  
       `Driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`  
  
    3. **Stringa di connessione per il provider ADO.NET Sybase:**  
  
       `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`  
  
    Per altre informazioni, vedere [connettersi a Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md).  
  
## <a name="reconnecting-to-sybase-ase"></a>Riconnessione a Sybase ASE

La connessione al server di database rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi se si desidera una connessione attiva al server adattivo. È possibile lavorare offline fino a quando non si desidera aggiornare i metadati, caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure ed eseguire la migrazione dei dati.  
  
## <a name="refreshing-sybase-ase-metadata"></a>Aggiornamento dei metadati di Sybase ASE

I metadati relativi ai database dell'ambiente del servizio app non vengono aggiornati automaticamente. I metadati in Sybase Metadata Explorer sono uno snapshot dei metadati al momento della prima connessione al server adattivo o dell'ultimo aggiornamento manuale dei metadati. È possibile aggiornare manualmente i metadati per un singolo database, uno schema di database singolo o tutti i database.  
  
**Per aggiornare i metadati**
  
1. Assicurarsi di essere connessi al server adattivo.  
  
2. In Sybase Metadata Explorer selezionare la casella di controllo accanto allo schema del database o del database che si desidera aggiornare.  
  
3. Fare clic con il pulsante destro del mouse su database o sul database singolo o sullo schema del database, quindi scegliere **Aggiorna da database**.  
  
4. Se viene richiesto di controllare l'oggetto corrente, fare clic su **Sì**.  
  
## <a name="next-step"></a>passaggio successivo  
  
- Il passaggio successivo del processo di migrazione consiste nel [connettersi a un'istanza di SQL Server](connecting-to-sql-server-sybasetosql.md)la  /  [connessione a un'istanza di SQL Azure](connecting-to-azure-sql-db-sybasetosql.md)  
  
## <a name="see-also"></a>Vedere anche

[Migrazione dei database Sybase ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
