---
description: Installazione dei componenti di SSMA in SQL Server (SybaseToSQL)
title: Installazione dei componenti di SSMA in SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5ad9e12c-2cdb-4dd2-8703-05a23242d19d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 82f5d7eef228326f0dba9b194b87bffce4bb5eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078232"
---
# <a name="installing-ssma-components-on-sql-server-sybasetosql"></a>Installazione dei componenti di SSMA in SQL Server (SybaseToSQL)

Oltre all'installazione di SSMA, per l'utilizzo della migrazione dei dati sul lato server, è necessario installare anche i componenti di nel computer che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti includono il pacchetto di estensioni SSMA, che supporta la migrazione dei dati e i provider Sybase per abilitare la connettività da server a server.

## <a name="ssma-for-sybase-extension-pack"></a>SSMA per Sybase Extension Pack

Il pacchetto di estensione SSMA aggiunge i database **sysdb** e **ssmatesterdb_syb** all'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il database **sysdb** contiene le tabelle e le stored procedure necessarie per eseguire la migrazione dei dati. Il database **ssmatester_syb** contiene lo schema **ssma_sybase_utilities**, in cui vengono creati gli oggetti (tabelle, trigger, viste) usati dal componente SSMA tester.

Inoltre, quando si esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processi di Agent quando viene utilizzato il motore di migrazione dei dati lato server per la migrazione dei dati.

### <a name="prerequisites"></a>Prerequisiti

Prima di installare SSMA per i componenti del Server Sybase in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che il sistema soddisfi i requisiti seguenti:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è installata l'istanza di.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Versione 4.7.2 o successiva. È possibile ottenerlo dal [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882).
- Il provider OLE DB/ADO.Net/ODBC Sybase e la connettività al server di database SAP ASE che contiene i database di cui si desidera eseguire la migrazione. È possibile installare i provider dal supporto del prodotto SAP ASE. Per informazioni sulla connettività, vedere [connessione a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).
- Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser deve essere in esecuzione durante l'installazione. Viene utilizzato per popolare un elenco delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nell'installazione guidata di. È possibile disabilitare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser dopo l'installazione.

  > [!NOTE]
  > Se il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser è in esecuzione, ma non viene ancora visualizzato un elenco di istanze nel programma di installazione, è necessario sbloccare la porta UDP 1434. È possibile utilizzare Windows Firewall per sbloccare temporaneamente la porta oppure è possibile disabilitare temporaneamente Windows Firewall. Potrebbe anche essere necessario disabilitare temporaneamente il software antivirus. Assicurarsi di abilitare i firewall e il software antivirus dopo l'installazione.

### <a name="installing-the-extension-pack"></a>Installazione del pacchetto di estensione

È possibile installare il pacchetto di estensione in qualsiasi momento prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Per installare il pacchetto di estensione, è necessario essere un membro del ruolo del server sysadmin nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Per installare il pacchetto di estensione:

1. Copiare **SSMAforSybaseExtensionPack_ *n*. msi**, dove *n* è il numero di build, nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Fare doppio clic su **SSMAforSybaseExtensionPack_ *n*. msi**.
3. Nella pagina **Benvenuti** fare clic su **Avanti**.
4. Nella pagina **contratto di licenza con l'utente finale** leggere il contratto di licenza. Se si accetta, selezionare l'opzione Accetto **il contratto** , quindi fare clic su **Avanti**.
5. Nella pagina **Selezione tipo di installazione** fare clic su **tipico**.
6. Nella pagina **Pronto per l'installazione**, fare clic su **Installa**.
7. Nella pagina **completamento del primo passaggio dell'installazione** fare clic su **Avanti**.

   Verrà visualizzata una nuova finestra di dialogo in cui è possibile selezionare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'installazione del pacchetto di estensione.

8. Selezionare l'istanza di in cui verrà eseguita la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrazione dei database di SAP ASE, quindi fare clic su **Avanti**.

   L'istanza predefinita ha lo stesso nome del computer. Le istanze denominate saranno seguite da una barra rovesciata e dal nome dell'istanza.

9. Nella pagina connessione selezionare il metodo di autenticazione e quindi fare clic su **Avanti**.

   L'autenticazione di Windows utilizzerà le credenziali di Windows per tentare di accedere all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si seleziona autenticazione server, è necessario immettere un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nome di account di accesso e una password.

10. Per il passaggio successivo è necessario impostare la password per una chiave master che verrà utilizzata per crittografare i dati sensibili archiviati nel database di pacchetto di estensione durante la migrazione dei dati sul lato server. Specificare una password complessa e fare clic su **Avanti**.

11. Nella pagina successiva selezionare **Install Utilities database *n* e Install Extension Pack Libraries**, dove *n* è il numero di versione. Se si prevede di usare la funzionalità tester, selezionare la casella di controllo **Installa database tester** , quindi fare clic su **Avanti**.

    Il database **sysdb** viene creato con le tabelle e le stored procedure necessarie per la migrazione dei dati (tramite il motore di migrazione dei dati sul lato server) in questo database.

    Se è selezionata l'opzione **Installa database tester** , verrà creato il database **ssmatesterdb_syb** .

12. Al termine dell'installazione, verrà visualizzato un messaggio in cui viene chiesto se si desidera installare il database Utilities in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , selezionare **Sì** e quindi fare clic su **Avanti**. per uscire dalla procedura guidata, selezionare **No** , quindi scegliere **Esci**.

### <a name="sql-server-database-objects"></a>Oggetti di database SQL Server

Dopo aver installato il pacchetto di estensione, viene visualizzata una tabella **ssma_syb. bcp _migration_packages** nel database **sysdb** . Vengono inoltre visualizzate le stored procedure seguenti:

- `bcp_clean_migration_data`
- `bcp_ensure_message_table`
- `bcp_insert_new_message`
- `bcp_post_process`
- `bcp_read_new_migration_messages`
- `bcp_save_migration_package`
- `bcp_smart_truncate`
- `bcp_start_migration_process`
- `get_jobstep_info`
- `stop_agent_process`

Ogni volta che si esegue la migrazione dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processo di Agent. Questi processi sono denominati **ssma_syb pacchetto di migrazione dei dati {GUID}** e sono visibili nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nodo Agent di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] nella cartella Jobs.  

## <a name="sybase-providers"></a>Provider Sybase

Quando si usa la migrazione dei dati sul lato server per spostare i dati da SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , i dati vengono migrati direttamente tra SAP ASE e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Non passa attraverso SSMA perché questo rallenta la migrazione dei dati.

### <a name="installing-the-sybase-providers"></a>Installazione dei provider Sybase

Le istruzioni seguenti forniscono i passaggi di installazione di base per l'installazione dei provider Sybase. Le istruzioni esatte variano a seconda della versione del programma di installazione di Sybase.

> [!IMPORTANT]
> Prima di eseguire il programma di installazione, verificare di non violare i contratti di licenza.

1. Eseguire il programma di installazione di Sybase ASE.
2. Selezionare installazione personalizzata.
3. Nella pagina Selezione funzionalità selezionare i provider di dati ODBC, OLE DB e ADO.NET.
4. Verificare le funzionalità selezionate, quindi fare clic su **fine** per installare il provider di dati.

## <a name="see-also"></a>Vedi anche

- [Installazione di SSMA per il client Sybase](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)
- [Migrazione di database Sybase ASE a SQL Server-database SQL di Azure](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
