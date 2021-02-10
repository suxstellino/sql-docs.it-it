---
title: Installazione dei componenti di SSMA in SQL Server (MySQLToSql) | Microsoft Docs
description: Installare i componenti nel server che esegue SQL Server per supportare la conversione del database MySQL con SSMA, inclusi i provider di estensioni SSMA e MySQL.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA extension pack, Installation
ms.assetid: 6772d0c5-258f-4d7b-afb0-b5f810e71af1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 48c111ab342d7410031851a5635fa73fe143b96b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070196"
---
# <a name="installing-ssma-components-on-sql-server-mysqltosql"></a>Installazione dei componenti di SSMA in SQL Server (MySQLToSql)

Oltre a installare SSMA, è necessario installare anche i componenti nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questi componenti includono il pacchetto di estensioni SSMA, che supporta la migrazione dei dati e i provider MySQL, per abilitare la connettività da server a server.

## <a name="ssma-for-mysql-extension-pack"></a>Pacchetto di estensione SSMA per MySQL

Il pacchetto di estensione SSMA aggiunge un database, **sysdb**, all'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questo database contiene le tabelle e le stored procedure necessarie per eseguire la migrazione dei dati.

Inoltre, quando si esegue la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i processi di Agent, quando viene utilizzato il motore di migrazione dei dati lato server per la migrazione dei dati.

### <a name="prerequisites"></a>Prerequisiti

Prima di installare i componenti di SSMA per MySQL server in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , verificare che il computer soddisfi i requisiti seguenti:

- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versione successiva.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Versione 4.7.2 o successiva. È possibile ottenerlo dal [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882).
- Il provider client MySQL e la connettività al database MySQL di cui si vuole eseguire la migrazione. È possibile installare i provider dal supporto del prodotto MySQL o dal sito web MySQL.
- Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser deve essere in esecuzione durante l'installazione. Viene utilizzato per popolare un elenco delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nell'installazione guidata di. È possibile disabilitare il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser dopo l'installazione.  

  > [!NOTE]
  > Se il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servizio browser è in esecuzione, ma non viene ancora visualizzato un elenco di istanze nel programma di installazione, è necessario sbloccare la porta UDP 1434. È possibile utilizzare Windows Firewall per sbloccare temporaneamente la porta oppure è possibile disabilitare temporaneamente Windows Firewall. Potrebbe anche essere necessario disabilitare temporaneamente il software antivirus. Assicurarsi di abilitare i firewall e il software antivirus dopo l'installazione.

### <a name="installing-the-extension-pack"></a>Installazione del pacchetto di estensione

È possibile installare il pacchetto di estensione in qualsiasi momento prima di eseguire la migrazione dei dati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Per installare il pacchetto di estensione, è necessario essere un membro del ruolo del server **sysadmin** nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Per installare il pacchetto di estensione:

1. Copiare SSMA per **SSMAforMySQLExtensionPack_ *n*. msi**, dove *n* è il numero di build, nel computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Fare doppio clic su **SSMAforMySQLExtensionPack_ *n*. msi**.
3. Nella finestra di dialogo di **benvenuto** fare clic su **Avanti**.
4. Nella finestra di dialogo contratto di licenza con l' **utente finale** leggere il contratto di licenza. Se si accetta, selezionare l'opzione Accetto **il contratto** , quindi fare clic su **Avanti**.
5. Nella finestra di dialogo **Scegli tipo di installazione** fare clic su **tipico**.
6. Nella finestra **di dialogo pronto per l'installazione** fare clic su **Installa**.
7. Nella finestra **di dialogo completato il primo passaggio dell'installazione** fare clic su **Avanti**.

   Verrà visualizzata una nuova finestra di dialogo in cui è possibile selezionare l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'installazione del pacchetto di estensione.
  
8. Selezionare l'istanza di in cui verrà eseguita la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrazione degli schemi MySQL e quindi fare clic su **Avanti**.
  
   L'istanza predefinita ha lo stesso nome del computer. Le istanze denominate saranno seguite da una barra rovesciata e dal nome dell'istanza.

9. Nella pagina connessione selezionare il metodo di autenticazione e quindi fare clic su **Avanti**.
  
    L'autenticazione di Windows utilizzerà le credenziali di Windows per tentare di accedere all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se si seleziona autenticazione server, è necessario immettere un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nome di account di accesso e una password.

10. Per il passaggio successivo è necessario impostare la password per una chiave master che verrà utilizzata per crittografare i dati sensibili archiviati nel database di pacchetto di estensione durante la migrazione dei dati sul lato server. Specificare una password complessa e fare clic su **Avanti**.

11. Nella finestra di dialogo successiva selezionare **Install Utilities database *n* e Install Extension Pack Libraries**, dove *n* è il numero di versione, quindi fare clic su **Avanti**.

    Il database **sysdb** viene creato con le tabelle e le stored procedure necessarie per la migrazione dei dati (tramite il motore di migrazione dei dati sul lato server) in questo database.

12. Per installare le utilità in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , selezionare **Sì** e quindi fare clic su **Avanti**. In alternativa, fare clic su **No** per uscire dalla procedura guidata.

## <a name="see-also"></a>Vedi anche

- [Installazione di SSMA per il client MySQL](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)
- [Migrazione di database MySQL a SQL Server-database SQL di Azure](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
