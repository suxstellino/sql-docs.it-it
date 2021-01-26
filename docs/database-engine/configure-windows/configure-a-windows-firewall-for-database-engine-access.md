---
title: Configurazione di Windows Firewall per l'accesso al Motore di database | Microsoft Docs
description: Informazioni su come configurare Windows Firewall in modo che i computer client possano accedere a un'istanza del motore di database di SQL Server attraverso il firewall.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], firewall systems
- firewall systems, [Database Engine]
- security [SQL Server], firewalls
ms.assetid: 0093b43c-c6b5-4574-9b30-3a0e91e1a1f9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 03f4a469465339664e56aae784c3b734f85ba9b4
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765694"
---
# <a name="configure-a-windows-firewall-for-database-engine-access"></a>Configurazione di Windows Firewall per l'accesso al Motore di database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


  In questo argomento viene descritto come configurare Windows Firewall per l'accesso al Motore di database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite Gestione configurazione SQL Server. I sistemi firewall contribuiscono a impedire l'accesso non autorizzato alle risorse del computer. Per accedere a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] attraverso un firewall, è necessario configurare il firewall nel computer in cui viene eseguito [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in modo che consenta l'accesso.  
  
 Per altre informazioni sulle impostazioni predefinite di Windows Firewall e per una descrizione delle porte TCP che interessano il [!INCLUDE[ssDE](../../includes/ssde-md.md)], Analysis Services, Reporting Services e Integration Services, vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md). Sono disponibili numerosi sistemi firewall. Per informazioni specifiche per il sistema in uso, vedere la documentazione del firewall.  
  
 Di seguito sono elencati i passaggi principali necessari per consentire l'accesso:  
  
1.  Configurare il [!INCLUDE[ssDE](../../includes/ssde-md.md)] per l'utilizzo di una porta TCP/IP specifica. Nell'istanza predefinita del [!INCLUDE[ssDE](../../includes/ssde-md.md)] viene utilizzata la porta 1433, ma questa impostazione può essere modificata. La porta utilizzata dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] è elencata nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nelle istanze di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], [!INCLUDE[ssEW](../../includes/ssew-md.md)] e nelle istanze denominate del [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono utilizzate porte dinamiche. Per configurare queste istanze in modo che usino una porta specifica, vedere [Configurazione di un server per l'attesa su una porta TCP specifica &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
2.  Configurare il firewall in modo che consenta l'accesso a tale porta per utenti o computer autorizzati.  
  
> [!NOTE]  
>  Il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser consente agli utenti di connettersi a istanze di [!INCLUDE[ssDE](../../includes/ssde-md.md)] che non sono in attesa sulla porta 1433, senza conoscere il numero di porta. Per utilizzare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, è necessario aprire la porta UDP 1434. Per promuovere la massima sicurezza per l'ambiente, non avviare il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser e configurare i client per la connessione tramite il numero di porta.  
  
> [!NOTE]  
>  Per impostazione predefinita, in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows è abilitato Windows Firewall, che chiude la porta 1433 per impedire la connessione dei computer Internet a un'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer in uso. Le connessioni all'istanza predefinita tramite TCP/IP non sono possibili se la porta 1433 non viene riaperta. Nelle procedure seguenti vengono illustrati i passaggi di base che consentono di configurare Windows Firewall. Per ulteriori informazioni, vedere la documentazione di Windows.  
  
 In alternativa alla configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per attivare l'attesa su una porta fissa e aprire la porta, è possibile elencare l'eseguibile di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Sqlservr.exe) come un'eccezione ai programmi bloccati. Utilizzare questo metodo quando si desidera continuare a utilizzare le porte dinamiche. In questo modo, è possibile accedere a una sola istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per configurare Windows Firewall per l'accesso al motore di database, usando:**  
  
     [Gestione configurazione SQL Server](#SSMSProcedure)  
  
## <a name="before-you-begin"></a>Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 L'apertura di porte nel firewall potrebbe esporre il server ad attacchi dannosi. Prima di aprire una porta, verificare di comprenderne le implicazioni per un sistema firewall. Per altre informazioni, vedere [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilizzo di Gestione configurazione SQL Server  
 Si applica a Windows Vista, Windows 7 e Windows Server 2008  
  
 Le procedure seguenti consentono di configurare Windows Firewall tramite lo snap-in Windows Firewall con sicurezza avanzata di Microsoft Management Console (MMC). Windows Firewall con sicurezza avanzata consente di configurare solo il profilo corrente. Per altre informazioni su Windows Firewall con protezione avanzata, vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
#### <a name="to-open-a-port-in-the-windows-firewall-for-tcp-access"></a>Per aprire una porta in Windows Firewall per l'accesso TCP  
  
1.  Dal menu **Start** scegliere **Esegui**, digitare **WF.msc**, quindi fare clic su **OK**.  
  
2.  Nel riquadro sinistro di **Windows Firewall con sicurezza avanzata** fare clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola** nel riquadro azioni.  
  
3.  Nella finestra di dialogo **Tipo di regola** selezionare **Porta**, quindi fare clic su **Avanti**.  
  
4.  Nella finestra di dialogo **Protocollo e porte** selezionare **TCP**. Selezionare **Porte locali specifiche**, quindi digitare il numero di porta dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)], ad esempio **1433** per l'istanza predefinita. Fare clic su **Avanti**.  
  
5.  Nella finestra di dialogo **Azione** selezionare **Consenti la connessione**, quindi fare clic su **Avanti**.  
  
6.  Nella finestra di dialogo **Profilo** selezionare tutti i profili che descrivono l'ambiente di connessione del computer quando si desidera eseguire la connessione al [!INCLUDE[ssDE](../../includes/ssde-md.md)], quindi fare clic su **Avanti**.  
  
7.  Nella finestra di dialogo **Nome** digitare un nome e una descrizione per questa regola, quindi fare clic su **Fine**.  
  
#### <a name="to-open-access-to-sql-server-when-using-dynamic-ports"></a>Per aprire l'accesso a SQL Server quando si utilizzano porte dinamiche  
  
1.  Dal menu **Start** scegliere **Esegui**, digitare **WF.msc**, quindi fare clic su **OK**.  
  
2.  Nel riquadro sinistro di **Windows Firewall con sicurezza avanzata** fare clic con il pulsante destro del mouse su **Regole in entrata**, quindi scegliere **Nuova regola** nel riquadro azioni.  
  
3.  Nella finestra di dialogo **Tipo di regola** selezionare **Programma**, quindi fare clic su **Avanti**.  
  
4.  Nella finestra di dialogo **Programma** selezionare **Percorso programma**. Fare clic su **Sfoglia**, spostarsi sull'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui si desidera accedere attraverso il firewall, quindi fare clic su **Apri**. Per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si trova in **C:\Programmi\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Sqlservr.exe**. Fare clic su **Avanti**.  
  
5.  Nella finestra di dialogo **Azione** selezionare **Consenti la connessione**, quindi fare clic su **Avanti**.  
  
6.  Nella finestra di dialogo **Profilo** selezionare tutti i profili che descrivono l'ambiente di connessione del computer quando si desidera eseguire la connessione al [!INCLUDE[ssDE](../../includes/ssde-md.md)], quindi fare clic su **Avanti**.  
  
7.  Nella finestra di dialogo **Nome** digitare un nome e una descrizione per questa regola, quindi fare clic su **Fine**.  
  
## <a name="see-also"></a>Vedere anche  
 [Procedura: Configurare le impostazioni del firewall (database SQL di Azure)](/azure/azure-sql/database/firewall-configure)  
  
