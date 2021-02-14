---
title: Usare SSMS per gestire SQL Server in Linux
description: Questo articolo introduce SQL Server Management Studio, un ambiente integrato per l'accesso, la configurazione, la gestione, l'amministrazione e lo sviluppo di componenti di SQL Server.
author: VanMSFT
ms.author: vanto
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b2fcf858-21c3-462a-8d49-50c85647d092
ms.openlocfilehash: 758c4e8165efd9fd24a979bfd3a1390e88b765ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345639"
---
# <a name="use-sql-server-management-studio-on-windows-to-manage-sql-server-on-linux"></a>Usare SQL Server Management Studio in Windows per gestire SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo presenta [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) e illustra alcune attività comuni. Poiché SSMS è un'applicazione Windows, usare SSMS quando si ha un computer Windows in grado di connettersi a un'istanza di SQL Server remota in Linux.

> [!TIP]
> Se non si ha un computer Windows su cui eseguire SSMS, prendere in considerazione il nuovo [Azure Data Studio](../azure-data-studio/index.yml), che offre uno strumento grafico per la gestione di SQL Server e viene eseguito sia in Linux che in Windows.

[SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) fa parte di una suite di strumenti SQL gratuiti offerti da Microsoft per esigenze di sviluppo e gestione. SSMS è un ambiente integrato per l'accesso, la configurazione, la gestione, l'amministrazione e lo sviluppo di tutti i componenti di SQL Server. Può connettersi a SQL Server in esecuzione su qualsiasi piattaforma locale, in contenitori Docker e nel cloud. Si connette anche al database SQL di Azure e ad Azure Synapse Analytics. SSMS integra un'ampia gamma di strumenti grafici con numerosi editor di script avanzati per consentire l'accesso a SQL Server a sviluppatori e amministratori di qualsiasi livello di competenza.

SSMS offre un'ampia gamma di funzionalità di sviluppo e gestione per SQL Server, inclusi gli strumenti per:

- Configurare, monitorare e amministrare una o più istanze di SQL Server
- Distribuire, monitorare e aggiornare i componenti del livello dati, ad esempio i database e le data warehouse
- Eseguire il backup e ripristino dei database
- Compilare ed eseguire query e script T-SQL e visualizzare i risultati
- Generare script T-SQL per oggetti di database
- Visualizzare e modificare dati nei database
- Progettare visivamente query T-SQL e oggetti di database quali viste, tabelle e stored procedure

Per altre informazioni su SSMS, vedere [Che cos'è SSMS?](../ssms/sql-server-management-studio-ssms.md)

## <a name="install-the-newest-version-of-sql-server-management-studio-ssms"></a>Installare la versione più recente di SQL Server Management Studio (SSMS)

Quando si lavora con SQL Server, è consigliabile usare sempre la versione più recente di SQL Server Management Studio (SSMS). La versione più recente di SSMS viene continuamente aggiornata e ottimizzata e attualmente funziona con SQL Server in Linux. Per scaricare e installare la versione più recente, vedere [Scaricare SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Per rimanere sempre aggiornati, la versione più recente di SSMS richiede di confermare il download quando è disponibile una nuova versione.

> [!NOTE]
> Prima di usare SSMS per gestire Linux, vedere i [problemi noti](sql-server-linux-release-notes.md) di SSMS in Linux.

## <a name="connect-to-sql-server-on-linux"></a>Connettersi a SQL Server in Linux

Per connettersi, seguire questa procedura di base:

1. Avviare SSMS digitando **Microsoft SQL Server Management Studio** nella casella di ricerca di Windows e quindi fare clic sull'applicazione desktop.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png)

1. Nella finestra **Connetti al server** immettere le informazioni seguenti (se SSMS è già in esecuzione, fare clic su **Connetti > Motore di database** per aprire la finestra **Connetti al server**):

   | Impostazione | Descrizione |
   |-----|-----|
   | **Tipo di server** | L'impostazione predefinita è il motore di database. Non modificare questo valore. |
   | **Nome server** | Immettere il nome del computer SQL Server Linux di destinazione o l'indirizzo IP e la porta nel formato `IP,port`. |
   | **autenticazione** | Per SQL Server in Linux, usare **Autenticazione di SQL Server**. |
   | **Accesso** | Immettere il nome di un utente con accesso a un database sul server (ad esempio, l'account **SA** predefinito creato durante l'installazione). |
   | **Password** | Immettere la password per l'utente specificato (per l'account **SA** creato durante l'installazione). |

    ![SQL Server Management Studio: connettersi al server di database SQL](./media/sql-server-linux-manage-ssms/connect.png)

1. Fare clic su **Connetti**.

    > [!TIP]
    > Se si verifica un errore di connessione, provare a diagnosticare il problema dal messaggio di errore. Rivedere poi i [consigli per la risoluzione dei problemi di connessione](sql-server-linux-troubleshooting-guide.md#connection).
 
1. Dopo la connessione a SQL Server, si apre **Esplora oggetti** ed è possibile accedere al database per eseguire attività amministrative o query sui dati.

## <a name="run-transact-sql-queries"></a>Eseguire le query Transact-SQL

Dopo la connessione al server, è possibile connettersi a un database ed eseguire query Transact-SQL. È possibile usare query Transact-SQL per quasi tutte le attività di database.

1. In **Esplora oggetti** passare al database di destinazione sul server. Ad esempio, espandere **Database di sistema** per usare il database **master**.

1. Fare clic con il pulsante destro del mouse sul database e quindi scegliere **Nuova query**.

1. Nella finestra della query scrivere una query Transact-SQL per selezionare e restituire i nomi di tutti i database nel server.

   ```sql
   SELECT [Name]
   FROM sys.Databases
   ```

   Se non si ha familiarità con la scrittura di query, vedere [Scrittura di istruzioni Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).

1. Fare clic sul pulsante **Esegui** per eseguire la query e visualizzare i risultati.

   ![Esito positivo. Connettersi al server di database SQL: SQL Server Management Studio](./media/sql-server-linux-manage-ssms/execute-query.png)

Nonostante sia possibile eseguire quasi tutte le attività di gestione con le query Transact-SQL, SSMS è uno strumento grafico che rende più semplice la gestione di SQL Server. Le sezioni seguenti forniscono alcuni esempi dell'uso dell'interfaccia utente grafica.

## <a name="create-and-manage-databases"></a>Creare e gestire database

Mentre si è connessi al database *master*, è possibile creare database sul server e modificare o eliminare i database esistenti. I passaggi seguenti illustrano come eseguire diverse attività di gestione di database comuni tramite Management Studio. Per eseguire queste attività, verificare di essere connessi al database *master* con l'account di accesso dell'entità di livello server creato durante la configurazione di SQL Server in Linux.

### <a name="create-a-new-database"></a>Creare un nuovo database

1. Avviare SSMS e connettersi al server in SQL Server in Linux

2. In Esplora oggetti fare clic con il pulsante destro del mouse sulla cartella *Database* e quindi scegliere *Nuovo database"

3. Nella finestra di dialogo *Nuovo database* immettere un nome per il nuovo database e quindi fare clic su *OK*

Il nuovo database viene creato correttamente nel server. Se si preferisce creare un nuovo database usando T-SQL, vedere [CREATE DATABASE (Transact-SQL di SQL Server)](../t-sql/statements/create-database-transact-sql.md).

### <a name="drop-a-database"></a>Eliminare un database

1. Avviare SSMS e connettersi al server in SQL Server in Linux

2. In Esplora oggetti espandere la cartella *Database* per visualizzare un elenco di tutti i database sul server.

3. In Esplora oggetti fare clic con il pulsante destro del mouse sul database che si vuole eliminare e quindi scegliere *Elimina*

4. Nella finestra di dialogo *Elimina oggetto* selezionare *Chiudi connessioni esistenti* e quindi fare clic su *OK*

Il database viene eliminato correttamente dal server. Se si preferisce eliminare un database usando T-SQL, vedere [DROP DATABASE (Transact-SQL di SQL Server)](../t-sql/statements/drop-database-transact-sql.md).

## <a name="use-activity-monitor-to-see-information-about-sql-server-activity"></a>Usare Monitoraggio attività per visualizzare informazioni sull'attività di SQL Server

Lo strumento [Monitoraggio attività](../relational-databases/performance-monitor/activity-monitor.md), integrato in SQL Server Management Studio (SSMS), visualizza informazioni sui processi di SQL Server e mostra come questi processi influiscono sull'istanza corrente di SQL Server.

1. Avviare SSMS e connettersi al server in SQL Server in Linux

1. In Esplora oggetti fare clic con il pulsante destro del mouse sul nodo *server* e scegliere *Monitoraggio attività*

Monitoraggio attività include riquadri espandibili e comprimibili con le informazioni seguenti:

- Panoramica
- Processi
- Attesa di risorse
- I/O file di dati
- Query recenti con costo elevato
- Query attive con costo elevato

Quando un riquadro è espanso, Monitoraggio attività esegue una query sull'istanza per ottenere informazioni. Quando un riquadro è compresso, significa che tutte le relative attività di query sono arrestate. È possibile espandere uno o più riquadri contemporaneamente per visualizzare diversi tipi di attività sull'istanza.

## <a name="see-also"></a>Vedere anche
- [Che cos'è SSMS?](../ssms/sql-server-management-studio-ssms.md)
- [Esportare e importare un database con SSMS](sql-server-linux-migrate-ssms.md)
- [Esercitazione: SQL Server Management Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)
- [Esercitazione: Scrittura di istruzioni Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Monitoraggio delle prestazioni e dell'attività del server](../relational-databases/performance/server-performance-and-activity-monitoring.md)