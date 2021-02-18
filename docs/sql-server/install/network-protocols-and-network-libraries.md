---
title: Protocolli e librerie di rete | Microsoft Docs
description: Un server può essere configurato per il monitoraggio di più protocolli di rete. È possibile modificare la configurazione usando Gestione configurazione SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server]
- configuration options [SQL Server], protocols
- network libraries [SQL Server]
- protocols [SQL Server], about network protocols
- pipes [SQL Server]
- network protocols [SQL Server]
- default SQL Server configurations
- library [SQL Server]
- network protocols [SQL Server], about network protocols
- configuration options [SQL Server], libraries
ms.assetid: 8cd437f6-9af1-44ce-9cb0-4d10c83da9ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81bf039f0a63aaad7a2214c48bb5948618e7ce7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336161"
---
# <a name="network-protocols-and-network-libraries"></a>Protocolli e librerie di rete
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un server può consentire di restare in attesa su più protocolli di rete contemporaneamente o di monitorarli. È tuttavia necessario configurare ogni protocollo. Se un particolare protocollo non è configurato, il server non può restare in attesa su tale protocollo. Le configurazioni dei protocolli possono essere modificate dopo l'installazione utilizzando Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="default-sql-server-network-configuration"></a>Configurazione di rete predefinita di SQL Server  
 Un'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene configurata per la porta TCP/IP 1433 e la named pipe \\\\\pipe\sql\query. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono configurate per le porte dinamiche TCP, con un numero di porta assegnato dal sistema operativo.  
  
 Se non è possibile utilizzare indirizzi di porta dinamici (ad esempio, quando le connessioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devono passare tramite un server firewall configurato per l'utilizzo di indirizzi di porte specifici). Selezionare un numero di porta non assegnato. Le assegnazioni dei numeri di porta vengono gestite da IANA (Internet Assigned Numbers Authority) e sono elencate in [https://www.iana.org](https://go.microsoft.com/fwlink/?LinkId=48844).  
  
 Per migliorare la sicurezza, la connettività di rete non viene abilitata completamente durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per abilitare, disabilitare e configurare i protocolli di rete al termine dell'installazione, utilizzare l'area Configurazione di rete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="server-message-block-protocol"></a>Protocollo Server Message Block  
 È necessario disabilitare tutti i protocolli non richiesti, incluso il protocollo SMB (Server Message Block), dei server della rete perimetrale. Per i server Web e i server DNS (Domain Name System) non è necessario SMB. È necessario disabilitare questo protocollo per proteggersi dal rischio di enumerazione degli utenti.  
  
> [!WARNING]
>  Tramite la disabilitazione di SMB (Server Message Block) verrà bloccato il servizio cluster di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Windows, impedendo l'accesso alla condivisione file remota. Non disabilitare SMB se si esegue o si pianifica di eseguire una delle operazioni seguenti:  
> 
>  -   Utilizzare la modalità di quorum di tipo Maggioranza dei nodi del cluster e delle condivisioni file di Windows  
> -   Specificare una condivisione file SMB come directory dei dati durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
> -   Creare un file di database in una condivisione file SMB  
  
#### <a name="to-disable-smb"></a>Per disabilitare SMB  
  
1.  Scegliere **Impostazioni** dal menu **Start** e quindi fare clic su **Rete e connessioni remote**.  
  
     Fare clic con il pulsante destro del mouse sulla connessione Internet e quindi scegliere **Proprietà**.  
  
2.  Selezionare la casella di controllo **Client per reti Microsoft** e quindi fare clic su **Disinstalla**.  
  
3.  Eseguire la procedura di disinstallazione.  
  
4.  Selezionare **Condivisione file e stampanti per reti Microsoft** e quindi fare clic su **Disinstalla**.  
  
5.  Eseguire la procedura di disinstallazione.  
  
#### <a name="to-disable-smb-on-servers-accessible-from-the-internet"></a>Per disabilitare SMB nei server accessibili da Internet  
  
-   In Connessione alla rete locale (LAN) usare la finestra di dialogo **Proprietà TCP/IP** per rimuovere **Condivisione file e stampanti per reti Microsoft** e **Client per reti Microsoft**.  
  
## <a name="endpoints"></a>Endpoint  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introduce un nuovo concetto in base al quale le connessioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono rappresentate sul lato server da un [!INCLUDE[tsql](../../includes/tsql-md.md)]*endpoint*. È possibile concedere, revocare o negare le autorizzazioni per gli endpoint [!INCLUDE[tsql](../../includes/tsql-md.md)] . Per impostazione predefinita, tutti gli utenti dispongono delle autorizzazioni di accesso a un endpoint a meno che tali autorizzazioni non vengano negate o revocate da un membro del gruppo sysadmin o dal proprietario dell'endpoint. La sintassi GRANT, REVOKE e DENY ENDPOINT utilizza un ID di endpoint che l'amministratore deve ottenere dalla vista del catalogo dell'endpoint.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea endpoint [!INCLUDE[tsql](../../includes/tsql-md.md)] per tutti i protocolli di rete supportati, nonché per la connessione amministrativa dedicata.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] creati dal programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono i seguenti:  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] computer locale  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] named pipe  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] TCP predefinito  
  
 Per altre informazioni sugli endpoint, vedere [Configurazione del Motore di database per l'attesa su più porte TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) e [Viste del catalogo degli endpoint &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md).  
  
 Per altre informazioni sulle configurazioni di rete di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere gli articoli seguenti nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   [Configurazione di rete del server](../../database-engine/configure-windows/server-network-configuration.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Configurazione superficie di attacco](../../relational-databases/security/surface-area-configuration.md)   
 [Considerazioni sulla sicurezza per un'installazione di SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [Pianificazione di un'installazione di SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)  
  
  
