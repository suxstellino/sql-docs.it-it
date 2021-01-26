---
title: Configurare il motore di database per l'attesa su più porte TCP | Microsoft Docs
description: Acquisire familiarità con gli endpoint TDS (Tabular Data Stream). Scoprire come usarli per configurare il motore di database di SQL Server per l'ascolto su più porte TCP.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multiple
- TDS
- listen on multiple ports
- endpoints [SQL Server], TDS
- adding ports
- tabular data stream
- multiple ports
ms.assetid: 8e955033-06ef-403f-b813-3d8241b62f1f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28c73233d8a7b38ec2d14fa92c40f69d9ae0af05
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783282"
---
# <a name="configure-the-database-engine-to-listen-on-multiple-tcp-ports"></a>Configurazione del Motore di database per l'attesa su più porte TCP
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene illustrato come configurare il [!INCLUDE[ssDE](../../includes/ssde-md.md)] per l'ascolto su più porte TCP in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando Gestione configurazione SQL Server. Quando TCP/IP è abilitato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il [!INCLUDE[ssDE](../../includes/ssde-md.md)] è in attesa delle connessioni in ingresso su un punto di connessione composto da un indirizzo IP e dal numero di porta TCP. Le procedure riportate di seguito consentono di creare un endpoint del flusso TDS, in modo che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possa essere in ascolto su una porta TCP aggiuntiva.  
  
 Possibili motivi per la creazione di un secondo endpoint TDS:  
  
-   Aumentare la sicurezza configurando il firewall in modo da limitare l'accesso all'endpoint predefinito ai computer client locali in una subnet specifica. Mantenere l'accesso tramite Internet a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il personale di supporto tecnico, creando un nuovo endpoint che il firewall espone a Internet e autorizzando la connessione all'endpoint al solo personale di supporto tecnico del server.  
  
-   Impostare l'affinità fra connessioni e processori specifici quando si utilizza la configurazione NUMA (Non-Uniform Memory Access).  
  
 La configurazione di un endpoint TDS include i passaggi seguenti, che possono essere eseguiti in qualsiasi ordine:  
  
-   Creare l'endpoint TDS per la porta TCP e ripristinare l'accesso all'endpoint predefinito, ove appropriato.  
  
-   Concedere l'accesso all'endpoint alle entità del server desiderate.  
  
-   Specificare il numero della porta TCP per l'indirizzo IP selezionato.  
  
 Per altre informazioni sulle impostazioni predefinite di Windows Firewall e per una descrizione delle porte TCP che interessano il motore di database, Analysis Services, Reporting Services e Integration Services, vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-create-a-tds-endpoint"></a>Per creare un endpoint TDS  
  
-   Eseguire l'istruzione seguente per creare un endpoint denominato **CustomConnection** per la porta 1500 per tutti gli indirizzi TCP disponibili nel server.  
  
    ```  
    USE master;  
    GO  
    CREATE ENDPOINT [CustomConnection]  
    STATE = STARTED  
    AS TCP  
       (LISTENER_PORT = 1500, LISTENER_IP =ALL)  
    FOR TSQL() ;  
    GO  
    ```  
  
 La creazione di un nuovo endpoint [!INCLUDE[tsql](../../includes/tsql-md.md)] comporta la revoca delle autorizzazioni di connessione **public** nell'endpoint TDS predefinito. Se per l'endpoint predefinito è necessario l'accesso al gruppo **public** , riapplicare l'autorizzazione utilizzando l'istruzione `GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] to [public];` .  
  
#### <a name="to-grant-access-to-the-endpoint"></a>Per concedere l'accesso all'endpoint  
  
-   Eseguire l'istruzione seguente per concedere l'accesso all'endpoint **CustomConnection** al gruppo SQLSupport nel dominio aziendale.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::[CustomConnection] to [corp\SQLSupport] ;  
    GO  
    ```  
  
#### <a name="to-configure-the-sql-server-database-engine-to-listen-on-an-additional-tcp-port"></a>Per configurare il Motore di database di SQL Server per l'attesa su una porta TCP aggiuntiva  
  
1.  In Gestione configurazione SQL Server espandere **Configurazione di rete SQL Server** e quindi fare clic su **Protocolli per** _<nome_istanza>_ .  
  
2.  Espandere **Protocolli per** _<nome_istanza>_ e quindi fare clic su **TCP/IP**.  
  
3.  Nel riquadro di destra, fare clic con il pulsante destro del mouse sugli indirizzi IP disabilitati da attivare, quindi scegliere **lita**.  
  
4.  Fare clic con il pulsante destro del mouse su **IPAll** e quindi scegliere **Proprietà**.  
  
5.  Nella casella **Porta TCP** digitare le porte sulle quali si desidera che [!INCLUDE[ssDE](../../includes/ssde-md.md)] resti in attesa, separate da virgole. Nell'esempio, se è elencata la porta predefinita 1433, digitare **,1500** in modo che il valore nella casella sia **1433,1500** e quindi fare clic su **OK**.  
  
    > [!NOTE]  
    >  Se non si sta abilitando la porta su tutti gli indirizzi IP, configurare la porta aggiuntiva nella casella delle proprietà per il solo indirizzo desiderato. Nel riquadro della console fare quindi clic con il pulsante destro del mouse su **TCP/IP**, scegliere **Proprietà** e nella casella **Attesa su tutti** selezionare **No**.  
  
6.  Nel riquadro di sinistra fare clic su **Servizi di SQL Server**.  
  
7.  Nel riquadro di destra fare clic con il pulsante destro del mouse su **SQL Server** _<nome_istanza>_ e quindi scegliere **Riavvia**.  
  
     Dopo il riavvio di [!INCLUDE[ssDE](../../includes/ssde-md.md)], il log degli errori conterrà l'elenco delle porte sulle quali [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in attesa.  
  
#### <a name="to-connect-to-the-new-endpoint"></a>Per eseguire la connessione al nuovo endpoint  
  
-   Per eseguire la connessione all'endpoint **CustomConnection** dell'istanza predefinita di SQL Server nel server ACCT, usando una connessione trusted e supponendo che l'utente sia membro del gruppo [corp\SQLSupport], eseguire questa istruzione.  
  
    ```  
    sqlcmd -SACCT,1500  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [GRANT Endpoint Permissions &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Eseguire il mapping delle porte TCP/IP ai nodi NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/map-tcp-ip-ports-to-numa-nodes-sql-server.md)  
  
  
