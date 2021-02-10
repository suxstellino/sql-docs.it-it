---
title: Connessione a SQL Server (MySQLToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un'istanza di destinazione di SQL Server per eseguire la migrazione di database MySQL. SSMA ottiene i metadati relativi ai database in SQL Server.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- connecting to SQL Server 2008, SQL Server permission
- connecting to SQL Server 2008, synchronization
ms.assetid: 08233267-693e-46e6-9ca3-3a3dfd3d2be7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9373f0272fd39ee515c63e0c1308d2218fcf9de2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069046"
---
# <a name="connecting-to-sql-server-mysqltosql"></a>Connessione a SQL Server (MySQLToSQL)

Per eseguire la migrazione dei database MySQL a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario connettersi all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando si esegue la connessione, SSMA ottiene i metadati relativi a tutti i database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Visualizza i metadati del database in **esplora metadati SQL Server**. SSMA archivia le informazioni dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è connessi, ma non archivia le password.

La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e si migrano i dati.

I metadati relativi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono sincronizzati automaticamente. Per aggiornare i metadati in **SQL Server Metadata Explorer**, è invece necessario aggiornare manualmente i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadati. Per ulteriori informazioni, vedere la sezione "sincronizzazione dei metadati di SQL Server" più avanti in questo argomento.

## <a name="required-sql-server-permissions"></a>Autorizzazioni SQL Server richieste

L'account utilizzato per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede autorizzazioni diverse a seconda delle azioni eseguite dall'account:

- Per convertire gli oggetti MySQL in [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi, per aggiornare i metadati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o per salvare la sintassi convertita in script, l'account deve disporre dell'autorizzazione per accedere all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Per caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'account deve essere un membro del ruolo del database **db_ddladmin** .

- Per eseguire la migrazione dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'account deve essere:
  - Membro del ruolo del database **db_owner** , se si utilizza il motore di migrazione dei dati sul lato client.
  - Membro del ruolo del server **sysadmin** , se si utilizza il motore di migrazione dei dati sul lato server. Questa operazione è necessaria per creare il `CmdExec` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] passaggio del processo di Agent durante la migrazione dei dati per eseguire lo strumento per la copia bulk SSMA.
    > [!NOTE]
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gli account proxy di Agent non sono supportati dalla migrazione dei dati sul lato server.

## <a name="establishing-a-sql-server-connection"></a>Creazione di una connessione SQL Server

Prima di convertire gli oggetti di database MySQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi, è necessario stabilire una connessione all'istanza di in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui si vuole eseguire la migrazione del database o dei database MySQL.

Quando si definiscono le proprietà di connessione, è inoltre necessario specificare il database in cui verrà eseguita la migrazione degli oggetti e dei dati. È possibile personalizzare questo mapping a livello di schema MySQL dopo la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [mapping di database MySQL a schemi SQL Server &#40;&#41;MySQLToSQL ](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).

> [!IMPORTANT]
> Prima di provare a connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia in esecuzione e che sia in grado di accettare le connessioni.

Per connettersi a SQL Server:

1. Scegliere **Connetti a SQL Server** dal menu **file** . questa opzione è abilitata dopo la creazione di un progetto.
   Se si è precedentemente connessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il nome del comando verrà **riconnesso a SQL Server**.

2. Nella finestra di dialogo connessione immettere o selezionare il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Se ci si connette all'istanza predefinita nel computer locale, è possibile immettere `localhost` o un punto ( `.` ).
   - Se ci si connette all'istanza predefinita in un altro computer, immettere il nome del computer.
   - Se ci si connette a un'istanza denominata in un altro computer, immettere il nome del computer seguito da una barra rovesciata e quindi dal nome dell'istanza, ad esempio `MyServer\MyInstance` .

3. Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata in modo da accettare connessioni su una porta non predefinita, immettere il numero di porta utilizzato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le connessioni nella casella **porta server** . Per l'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il numero di porta predefinito è 1433. Per le istanze denominate, SSMA proverà a ottenere il numero di porta dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser.

4. Nella casella **autenticazione** selezionare il tipo di autenticazione da utilizzare per la connessione. Per utilizzare l'account di Windows corrente, selezionare **autenticazione di Windows**. Per usare un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso, selezionare **SQL Server autenticazione** e quindi specificare il nome e la password dell'account di accesso.

5. Per una connessione protetta vengono aggiunti due controlli, ovvero le caselle di controllo **Encrypt connection** e **TrustServerCertificate** . La casella di controllo **TrustServerCertificate** è visibile solo quando è selezionata l'opzione **Crittografa connessione** . Quando la **connessione Encrypt** è selezionata (true) e **TrustServerCertificate** è deselezionata (false), il certificato SSL verrà convalidato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La convalida del certificato del server fa parte dell'handshake SSL e assicura che il server a cui si esegue la connessione sia quello corretto. Per garantire questo problema, è necessario installare un certificato sul lato client e sul lato server.

6. Fare clic su **Connect** (Connetti).

> [!IMPORTANT]
> Sebbene sia possibile connettersi a una versione successiva di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , rispetto alla versione scelta al momento della creazione del progetto di migrazione, la conversione degli oggetti di database è determinata dalla versione di destinazione del progetto e non dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è connessi.

## <a name="synchronizing-sql-server-metadata"></a>Sincronizzazione di metadati di SQL Server

I metadati relativi ai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database non vengono aggiornati automaticamente. I metadati in **SQL Server Esplora metadati** sono uno snapshot dei metadati al momento della prima connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oppure l'ultima volta che i metadati sono stati aggiornati manualmente. È possibile aggiornare manualmente i metadati per tutti i database o per qualsiasi singolo database o oggetto di database. Per sincronizzare i metadati:

1. Assicurarsi di essere connessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

2. In **SQL Server Esplora metadati**, selezionare la casella di controllo accanto allo schema del database o del database che si desidera aggiornare.
   Ad esempio, per aggiornare i metadati per tutti i database, selezionare la casella accanto a **database**.

3. Fare clic con il pulsante destro del mouse su **database** o sul database singolo o sullo schema del database, quindi scegliere **Sincronizza con database**.

## <a name="next-step"></a>passaggio successivo

Il passaggio successivo della migrazione dipende dalle esigenze del progetto:

- Per personalizzare il mapping tra gli schemi di MySQL e SQL Server database e schemi, vedere [mapping di database MySQL a SQL Server schemi &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).
- Per personalizzare le opzioni di configurazione per i progetti, vedere [impostazione delle opzioni del progetto &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).
- Per personalizzare il mapping dei tipi di dati di origine e di destinazione, vedere [mapping di tipi di dati MySQL e SQL Server &#40;&#41;MySQLToSQL ](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).
- Se non è necessario eseguire nessuna di queste attività, è possibile convertire le definizioni degli oggetti di database MySQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definizioni di oggetti. Per altre informazioni, vedere [conversione di database MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md).

## <a name="see-also"></a>Vedere anche

[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
