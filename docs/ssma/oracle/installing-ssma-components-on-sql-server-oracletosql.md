---
title: Installazione dei componenti di SSMA in SQL Server (OracleToSQL) | Microsoft Docs
description: Informazioni su come installare il pacchetto di estensione SSMA e i provider Oracle nel computer che esegue SQL Server per supportare la conversione del database Oracle.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing the extension pack
- SQL Server Database Objects
ms.assetid: 33070e5f-4e39-4b70-ae81-b8af6e4983c5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 81d3f542e64030dbf6a0bf70067307222bef5a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344210"
---
# <a name="installing-ssma-components-on-sql-server-oracletosql"></a>Installazione dei componenti di SSMA in SQL Server (OracleToSQL)

Oltre a installare SSMA, è necessario installare anche i componenti nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti includono il pacchetto di estensioni SSMA, che supporta la migrazione dei dati e i provider Oracle per abilitare la connettività tra server.

## <a name="ssma-for-oracle-extension-pack"></a>SSMA per Oracle Extension Pack

Il pacchetto di estensione SSMA distribuisce stored procedure estese e aggiunge i database **sysdb** e **ssmatesterdb** all'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le stored procedure estese forniscono la funzionalità necessaria per emulare le funzionalità e behaiov di Oracle, mentre il database **sysdb** contiene le tabelle e le stored procedure necessarie per eseguire la migrazione dei dati. Il database **ssmatesterdb** contiene le tabelle e le procedure richieste dal componente tester (se installato).

Inoltre, quando si esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processi di Agent quando viene utilizzato il motore di migrazione dei dati sul lato server per la migrazione dei dati.

### <a name="prerequisites"></a>Prerequisiti

Prima di installare i componenti di SSMA per Oracle Server in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che il sistema soddisfi i requisiti seguenti:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è installata l'istanza di.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Versione 4.7.2 o successiva. È possibile ottenerlo dal [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882).
- Il provider di OLE DB per Oracle (se si utilizza OLE DB) e la connettività al database Oracle di cui si desidera eseguire la migrazione. È possibile installare i provider dal supporto del prodotto Oracle o dal sito Web Oracle.
- Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser deve essere in esecuzione durante l'installazione. Viene utilizzato per popolare un elenco delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nell'installazione guidata di. È possibile disabilitare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser dopo l'installazione.

  > [!NOTE]
  > Se il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser è in esecuzione, ma non viene ancora visualizzato un elenco di istanze nel programma di installazione, è necessario sbloccare la porta UDP 1434. È possibile utilizzare Windows Firewall per sbloccare temporaneamente la porta oppure è possibile disabilitare temporaneamente Windows Firewall. Potrebbe anche essere necessario disabilitare temporaneamente il software antivirus. Assicurarsi di abilitare i firewall e il software antivirus dopo l'installazione.

### <a name="installing-the-extension-pack"></a>Installazione del pacchetto di estensione

È possibile installare il pacchetto di estensione in qualsiasi momento prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Per installare il pacchetto di estensione, è necessario essere un membro del ruolo del server **sysadmin** nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Per installare il pacchetto di estensione:

1. Copiare **SSMAforOracleExtensionPack_ *n*. msi** (dove *n* è il numero di Build) nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Fare doppio clic su **SSMAforOracleExtensionPack_ *n*. msi**.
3. Nella pagina **Benvenuti** fare clic su **Avanti**.
4. Nella pagina **contratto di licenza con l'utente finale** leggere il contratto di licenza. Se si accettano le condizioni, selezionare Accetto **l'opzione del contratto** , quindi fare clic su **Avanti**.
5. Nella pagina Selezione **tipo di installazione** selezionare **tipico**.
6. Nella pagina **Inizio installazione** fare clic su **Installa**.
7. Nella pagina **completamento del primo passaggio dell'installazione** fare clic su **Avanti**.
  
   Verrà visualizzata una nuova finestra di dialogo. Selezionare il tipo di pacchetto di estensione.
  
8. Selezionare il tipo di installazione desiderato e fare clic su **Avanti**.

   > [!IMPORTANT]
   > È consigliabile usare l'opzione remote solo quando si installa il pacchetto di estensione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esecuzione in Linux o quando la destinazione è [!INCLUDE[ssAzureMi](../../includes/ssazuremi_md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per le installazioni in esecuzione in Windows è sempre necessario che il pacchetto di estensione sia installato localmente. [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] e Azure sinapsi Analytics non supportano il pacchetto di estensione.

   Se si sta installando il pacchetto di estensione in un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza locale, la pagina successiva consente di scegliere un'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si eseguirà la migrazione degli schemi Oracle. Scegliere un'istanza nell'elenco a discesa, quindi fare clic su **Avanti**.

   L'istanza predefinita ha lo stesso nome del computer. Le istanze denominate saranno seguite da una barra rovesciata e dal nome dell'istanza.

9. Nella pagina connessione selezionare il metodo di autenticazione e quindi fare clic su **Avanti**.

   L'autenticazione di Windows utilizzerà le credenziali di Windows per tentare di accedere all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si seleziona autenticazione server, è necessario immettere un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nome di account di accesso e una password.

10. Per il passaggio successivo è necessario impostare la password per una chiave master che verrà utilizzata per crittografare i dati sensibili archiviati nel database di pacchetto di estensione durante la migrazione dei dati sul lato server. Specificare una password complessa e fare clic su **Avanti**.

11. Nella pagina successiva selezionare **Install Utilities database *n* e Install Extension Pack Libraries**, dove *n* è il numero di versione. Se si prevede di usare la funzionalità tester, selezionare la casella di controllo **Installa database tester** , quindi fare clic su **Avanti**.

    Il database **sysdb** viene creato con le tabelle e le stored procedure necessarie per la migrazione dei dati (tramite il motore di migrazione dei dati sul lato server) in questo database.

    Se l'opzione **Installa database tester** è selezionata, verrà creato il database **ssmatesterdb** .

12. Al termine dell'installazione, verrà visualizzato un messaggio in cui viene chiesto se si desidera installare il database Utilities in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , selezionare **Sì** e quindi fare clic su **Avanti**. per uscire dalla procedura guidata, selezionare **No** , quindi scegliere **Esci**.

13. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o tramite l' `sqlcmd` Utilità, eseguire lo script seguente per abilitare CLR:

    ```sql
    sp_configure 'clr enabled', 1
    GO
    RECONFIGURE
    GO
    ```

    Se CLR non è abilitato, si riceverà l'errore seguente quando SSMA si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

    > SSMA non è riuscito a recuperare le informazioni sulla versione dell'assembly del pacchetto di estensione. Reinstallare il pacchetto di estensione nel server di database.

### <a name="sql-server-database-objects"></a>Oggetti di database SQL Server

Dopo aver installato il pacchetto di estensione, viene visualizzata una tabella **ssma_oracle. bcp _migration_packages** nel database **sysdb** .

Ogni volta che si esegue la migrazione dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processo di Agent. Questi processi sono denominati **ssma_oracle pacchetto di migrazione dei dati {GUID}** e sono visibili nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nodo Agent di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] nella cartella Jobs.

Verranno inoltre aggiunte al database **Master** le stored procedure estese seguenti:

- `xp_ora2ms_exec2`
- `xp_ora2ms_exec2_ex`
- `xp_ora2ms_versioninfo2`

## <a name="see-also"></a>Vedi anche

- [Installazione di SSMA per il client Oracle](../../ssma/oracle/installing-ssma-for-oracle-client-oracletosql.md)
- [Migrazione di database Oracle a SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
