---
description: Connessione a SQL Server (SybaseToSQL)
title: Connessione a SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to SQL Server
ms.assetid: dd368a1a-45b0-40e9-b4d3-5cdb48c26606
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5637ee7524758dc8d527946c69a2edc9af948d8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056906"
---
# <a name="connecting-to-sql-server-sybasetosql"></a>Connessione a SQL Server (SybaseToSQL)

Per eseguire la migrazione dei database di Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario connettersi all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti i database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Visualizza i metadati del database in **esplora metadati SQL Server**. SSMA archivia informazioni sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è connessi, ma non archivia le password.

La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e si migrano i dati.

I metadati relativi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono sincronizzati automaticamente. Se invece si desidera aggiornare i metadati in **SQL Server Esplora metadati**, è necessario aggiornare manualmente i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadati, come descritto nella sezione "sincronizzazione dei metadati SQL Server" più avanti in questo argomento.

## <a name="required-sql-server-permissions"></a>Autorizzazioni SQL Server richieste

L'account utilizzato per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede autorizzazioni diverse a seconda delle azioni eseguite dall'account:

- Per convertire gli oggetti dell'ambiente del servizio app in [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi, per aggiornare i metadati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o per salvare la sintassi convertita in script, l'account deve disporre dell'autorizzazione per accedere all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Per caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'account deve essere un membro del ruolo del database **db_ddladmin** .

- Per eseguire la migrazione dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'account deve essere:
  - Membro del ruolo del database **db_owner** , se si utilizza il motore di migrazione dei dati sul lato client.
  - Membro del ruolo del server **sysadmin** , se si utilizza il motore di migrazione dei dati sul lato server. Questa operazione è necessaria per creare il `CmdExec` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] passaggio del processo di Agent durante la migrazione dei dati per eseguire lo strumento per la copia bulk SSMA.
    > [!NOTE]
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gli account proxy di Agent non sono supportati dalla migrazione dei dati sul lato server.

- Per eseguire il codice generato da SSMA, è necessario che l'account disponga `EXECUTE` delle autorizzazioni per tutte le funzioni definite dall'utente nello schema **ssma_syb** del database di destinazione. Queste funzioni forniscono funzionalità equivalenti delle funzioni di sistema dell'ambiente del servizio app e vengono usate dagli oggetti convertiti.

## <a name="establishing-a-sql-server-connection"></a>Creazione di una connessione SQL Server

Prima di convertire gli oggetti di database dell'ambiente del servizio app nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi, è necessario stabilire una connessione all'istanza di in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui si vuole eseguire la migrazione del database o dei database dell'ambiente del servizio app.

Quando si definiscono le proprietà di connessione, è inoltre necessario specificare il database in cui verrà eseguita la migrazione degli oggetti e dei dati. È possibile personalizzare questo mapping a livello di schema dell'ambiente del servizio app dopo la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere mapping di schemi dell'ambiente del servizio app [Sybase a schemi SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).

> [!IMPORTANT]
> Prima di provare a connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia in esecuzione e che sia in grado di accettare le connessioni.

Per connettersi al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:
  
1. Scegliere **Connetti a SQL Server** dal menu **file** .
   Se in precedenza si è connessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il nome del comando verrà **riconnesso a SQL Server**.

2. Nella finestra di dialogo connessione immettere o selezionare il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
   - Se ci si connette all'istanza predefinita nel computer locale, è possibile immettere `localhost` o un punto ( `.` ).
   - Se ci si connette all'istanza predefinita in un altro computer, immettere il nome del computer.
   - Se ci si connette a un'istanza denominata in un altro computer, immettere il nome del computer seguito da una barra rovesciata e quindi dal nome dell'istanza, ad esempio `MyServer\MyInstance` .

3. Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata in modo da accettare connessioni su una porta non predefinita, immettere il numero di porta utilizzato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le connessioni nella casella **porta server** . Per l'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il numero di porta predefinito è 1433. Per le istanze denominate, SSMA proverà a ottenere il numero di porta dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser.

4. Nella casella **database** immettere il nome del database di destinazione.
   Questa opzione non è disponibile quando si esegue la riconnessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. Nella casella **autenticazione** selezionare il tipo di autenticazione da utilizzare per la connessione. Per utilizzare l'account di Windows corrente, selezionare **autenticazione di Windows**. Per usare un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso, selezionare **SQL Server autenticazione** e quindi specificare il nome e la password dell'account di accesso.

6. Per una connessione protetta vengono aggiunti due controlli, ovvero le caselle di controllo **Encrypt connection** e **TrustServerCertificate** . La casella di controllo **TrustServerCertificate** è visibile solo quando è selezionata l'opzione **Crittografa connessione** . Quando la **connessione Encrypt** è selezionata (true) e **TrustServerCertificate** è deselezionata (false), il certificato SSL verrà convalidato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La convalida del certificato del server fa parte dell'handshake SSL e assicura che il server a cui si esegue la connessione sia quello corretto. Per garantire questo problema, è necessario installare un certificato sul lato client e sul lato server.

7. Fare clic su **Connect** (Connetti).

> [!IMPORTANT]
> Sebbene sia possibile connettersi a una versione successiva di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , rispetto alla versione scelta al momento della creazione del progetto di migrazione, la conversione degli oggetti di database è determinata dalla versione di destinazione del progetto e non dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è connessi.

## <a name="reconnecting-to-sql-server"></a>Riconnessione a SQL Server

La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si aggiornano i metadati, si caricano oggetti di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e si esegue la migrazione dei dati

La procedura per la riconnessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è identica alla procedura per stabilire una connessione.

## <a name="synchronizing-sql-server-metadata"></a>Sincronizzazione di metadati di SQL Server

I metadati relativi ai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database non vengono aggiornati automaticamente. I metadati in **SQL Server Esplora metadati** sono uno snapshot dei metadati al momento della prima connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure l'ultima volta che i metadati sono stati aggiornati manualmente. È possibile aggiornare manualmente i metadati per tutti i database o per qualsiasi singolo database o oggetto di database. Per sincronizzare i metadati:

1. Assicurarsi di essere connessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

2. In **SQL Server Esplora metadati**, selezionare la casella di controllo accanto allo schema del database o del database che si desidera aggiornare.
   Ad esempio, per aggiornare i metadati per tutti i database, selezionare la casella accanto a **database**.

3. Fare clic con il pulsante destro del mouse su **database** o sul database singolo o sullo schema del database, quindi scegliere **Sincronizza con database**.

## <a name="next-step"></a>passaggio successivo

Il passaggio successivo della migrazione dipende dalle esigenze del progetto:

- Se si vuole personalizzare il mapping tra i database dell'ambiente del servizio app e gli schemi e i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database e gli schemi, vedere mapping degli schemi dell'ambiente del servizio app [Sybase a schemi SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).
- Se si desidera personalizzare le opzioni di configurazione per i progetti, vedere [impostazione delle opzioni del progetto &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md).
- Se si vuole personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere Mapping dei tipi di [dati di Sybase ASE e SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).
- Se non è necessario eseguire nessuna di queste operazioni, è possibile convertire le definizioni degli oggetti di database Sybase ASE in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definizioni di oggetti. Per altre informazioni, vedere [conversione di oggetti di database Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).

## <a name="see-also"></a>Vedere anche

[Migrazione dei database Sybase ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
