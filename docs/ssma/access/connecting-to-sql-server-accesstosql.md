---
title: Connessione a SQL Server (AccessToSQL) | Microsoft Docs
description: Informazioni su come connettersi a un'istanza di destinazione del database SQL per eseguire la migrazione dei database di Access. SSMA ottiene i metadati relativi ai database nel database SQL.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- authentication
- instance of SQL Server
- metadata, refreshing
- ports
- refreshing metadata
- spaces in database names
- special characters
- SQL Server
- SQL Server, connecting
- SQL Server, connecting to
- SQL Server, reconnecting
ms.assetid: f84cf007-ddf1-4396-a07c-3e0729abc769
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 59583057641bb167aae197dd9fae3bf6e5833302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076192"
---
# <a name="connecting-to-sql-server-accesstosql"></a>Connessione a SQL Server (AccessToSQL)

Per eseguire la migrazione dei database di Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario connettersi all'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando si esegue la connessione, SSMA ottiene i metadati relativi ai database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e Visualizza i metadati del database in **SQL Server Esplora metadati**. SSMA archivia informazioni sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è connessi, ma non archivia le password.

La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e si migrano i dati.

I metadati relativi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono sincronizzati automaticamente. Per aggiornare i metadati in SQL Server Metadata Explorer, è invece necessario aggiornare manualmente i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadati. Per ulteriori informazioni, vedere la sezione "sincronizzazione dei metadati di SQL Server" più avanti in questo argomento.

## <a name="required-sql-server-permissions"></a>Autorizzazioni SQL Server richieste

L'account utilizzato per la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede autorizzazioni diverse a seconda delle azioni eseguite dall'account:

- Per convertire gli oggetti di accesso in [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi, per aggiornare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i metadati da o per salvare la sintassi convertita in script, l'account deve disporre dell'autorizzazione per accedere all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Per caricare oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'account deve essere un membro del ruolo del database **db_ddladmin** .

- Per eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'account deve essere un membro del ruolo del database **db_owner** .

## <a name="establishing-a-sql-server-connection"></a>Creazione di una connessione SQL Server

Prima di convertire gli oggetti di database di Access nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi, è necessario stabilire una connessione all'istanza di in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cui si desidera eseguire la migrazione dei database di Access.

Quando si definiscono le proprietà di connessione, è inoltre necessario specificare il database in cui verrà eseguita la migrazione degli oggetti e dei dati. È possibile personalizzare questo mapping a livello di database di Access dopo la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [mapping di database di origine e di destinazione](mapping-source-and-target-databases-accesstosql.md).

> [!IMPORTANT]
> Prima di connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia in esecuzione e che sia in grado di accettare le connessioni.

Per connettersi al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

1. Scegliere **Connetti a SQL Server** dal menu **file** .
   Se in precedenza si è connessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il nome del comando verrà **riconnesso a SQL Server**.

2. Nella casella **nome server** immettere o selezionare il nome dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Se ci si connette all'istanza predefinita nel computer locale, è possibile immettere `localhost` o un punto ( `.` ).
   - Se ci si connette all'istanza predefinita in un altro computer, immettere il nome del computer.
   - Se ci si connette a un'istanza denominata, immettere il nome del computer, una barra rovesciata e il nome dell'istanza. Ad esempio: `MyServer\MyInstance`.
   - Per connettersi a un'istanza utente attiva di [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] , connettersi utilizzando il protocollo Named Pipes e specificando il nome della pipe, ad esempio `\\.\pipe\sql\query` . Per altre informazioni, vedere la documentazione di [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)].

3. Se l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è configurata in modo da accettare connessioni su una porta non predefinita, immettere il numero di porta utilizzato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le connessioni nella casella **porta server** . Per l'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il numero di porta predefinito è 1433. Per le istanze denominate, SSMA proverà a ottenere il numero di porta dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser.

4. Nella casella **database** immettere il nome del database di destinazione per la migrazione di oggetti e dati.
   Questa opzione non è disponibile quando si esegue la riconnessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   Il nome del database di destinazione non può contenere spazi o caratteri speciali. È ad esempio possibile eseguire la migrazione dei database di Access a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database denominato `abc` . Non è tuttavia possibile eseguire la migrazione dei database di Access a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database denominato `a b-c` .
   È possibile personalizzare questo mapping per ogni database dopo la connessione. Per ulteriori informazioni, vedere [mapping di database di origine e di destinazione](mapping-source-and-target-databases-accesstosql.md)

5. Nel menu a discesa **autenticazione** selezionare il tipo di autenticazione da utilizzare per la connessione. Per utilizzare l'account di Windows corrente, selezionare **autenticazione di Windows**. Per usare un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account di accesso, selezionare **SQL Server autenticazione** e quindi specificare un nome utente e una password.

6. Per la connessione sicura vengono aggiunti due controlli, la casella di controllo **Crittografa connessione** e la casella di controllo **TrustServerCertificate** . È visibile solo quando è selezionata la casella di controllo **Crittografa connessione** **TrustServerCertificate** . Quando la **connessione Encrypt** è selezionata (true) e **TrustServerCertificate** è deselezionata (false), convaliderà il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] certificato SSL. La convalida del certificato del server fa parte dell'handshake SSL e assicura che il server a cui si esegue la connessione sia quello corretto. Per garantire questo problema, è necessario installare un certificato sul lato client e sul lato server.

7. Fare clic su **Connect** (Connetti).

> [!IMPORTANT]
> Sebbene sia possibile connettersi a una versione successiva di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , rispetto alla versione scelta al momento della creazione del progetto di migrazione, la conversione degli oggetti di database è determinata dalla versione di destinazione del progetto e non dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si è connessi.

## <a name="synchronizing-sql-server-metadata"></a>Sincronizzazione di metadati di SQL Server

Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gli schemi cambiano dopo la connessione, è possibile sincronizzare i metadati con il server.

Per sincronizzare i metadati di SQL Server, **SQL Server Esplora metadati**, fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Sincronizza con database**.

## <a name="reconnecting-to-sql-server"></a>Riconnessione a SQL Server

La connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimane attiva fino a quando non si chiude il progetto. Quando si riapre il progetto, è necessario riconnettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se si desidera una connessione attiva al server. È possibile lavorare offline fino a quando non si caricano oggetti di database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e si migrano i dati.

La procedura per la riconnessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è identica alla procedura per stabilire una connessione.

## <a name="next-steps"></a>Passaggi successivi

Se si desidera personalizzare il mapping tra i database di origine e di destinazione, vedere [mapping di database di origine e di destinazione](mapping-source-and-target-databases-accesstosql.md) in caso contrario, il passaggio successivo consiste nel convertire gli oggetti di database nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintassi utilizzando [Converti oggetti di database](converting-access-database-objects-accesstosql.md).

## <a name="see-also"></a>Vedere anche

[Migrazione dei database di Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
