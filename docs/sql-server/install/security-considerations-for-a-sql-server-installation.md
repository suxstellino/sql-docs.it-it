---
title: Considerazioni relative alla sicurezza
description: Questo articolo illustra alcune procedure consigliate per la sicurezza da prendere in considerazione prima e dopo l'installazione di SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- firewall systems [SQL Server]
- server message blocks [SQL Server]
- disabling protocols
- security [SQL Server], installations
- protocols [SQL Server], disabling
- NetBIOS [SQL Server]
- services [SQL Server], security
- SMB [SQL Server]
- isolating services [SQL Server]
- Setup [SQL Server], security
- low SQL Server installation privileges
- NTFS [SQL Server]
- physical security [SQL Server]
- file system security [SQL Server]
- installing SQL Server, security
ms.assetid: cf96155f-30a8-48b7-8d6b-24ce90dafdc7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f8a540cf1f740ad2f9cb712db4bdd51f6c1a623
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336118"
---
# <a name="security-considerations-for-a-sql-server-installation"></a>Considerazioni sulla sicurezza per un'installazione di SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

 La sicurezza rappresenta un fattore importante per ogni prodotto e azienda. Con alcune semplici procedure consigliate è possibile evitare molte vulnerabilità di sicurezza. Questo articolo illustra alcune procedure consigliate per la sicurezza da prendere in considerazione prima dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le indicazioni sulla sicurezza per specifiche funzionalità sono riportate negli articoli di riferimento relativi a tali funzionalità.  
  
## <a name="before-installing-ssnoversion"></a>Prima dell'installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Procedure consigliate da eseguire durante la configurazione dell'ambiente server:  
  
-   [Ottimizzazione della sicurezza fisica](#physical_security)  
  
-   [Utilizzo di firewall](#firewalls)  
  
-   [Isolamento dei servizi](#isolated_services)  
  
-   [Configurazione di un file system protetto](#sa_with_least_privileges)  
  
-   [Disabilitazione di NetBIOS e SMB](#disabled_protocols)  
  
-   [Installazione di SQL Server in un controller di dominio](../../sql-server/install/security-considerations-for-a-sql-server-installation.md#Install_DC)  
  
###  <a name="enhance-physical-security"></a><a name="physical_security"></a> Enhance Physical Security  
 La sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è basata sull'isolamento fisico e logico. Per ottimizzare la sicurezza fisica dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , eseguire le operazioni seguenti:  
  
-   Collocare il server in un'area accessibile solo a persone autorizzate.  
  
-   Posizionare i computer che ospitano un database in un luogo fisicamente protetto. La soluzione ideale è rappresentata da una sala computer chiusa, con monitoraggio e rilevamento di allagamenti e sistemi di rilevamento e spegnimento incendi.  
  
-   Installare i database in un'area protetta della rete Intranet aziendale e non connettere i computer SQL Server direttamente a Internet.  
  
-   Eseguire regolarmente il backup di tutti i dati e archiviare tali backup in un luogo protetto fuori sede.  
  
###  <a name="use-firewalls"></a><a name="firewalls"></a> Use Firewalls  
 I firewall costituiscono un elemento importante ai fini della protezione dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e garantiscono la massima efficienza se vengono rispettate le linee guida seguenti:  
  
-   Inserire un firewall tra il server e Internet. Abilitare il firewall. Se il firewall è disabilitato, abilitarlo. Se il firewall è abilitato, non disabilitarlo.  
  
-   Dividere la rete in aree di sicurezza separate da firewall. Bloccare tutto il traffico e quindi ammettere selettivamente soltanto il traffico necessario.  
  
-   In un ambiente multilivello utilizzare più firewall per creare subnet schermate.  
  
-   Se il server viene installato all'interno di un dominio Windows, configurare firewall interni per consentire l'autenticazione di Windows.  
  
-   Se l'applicazione utilizza transazioni distribuite, potrebbe essere necessario configurare il firewall in modo da consentire il flusso del traffico [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) tra istanze MS DTC separate. Sarà inoltre necessario configurare il firewall per consentire il flusso del traffico tra MS DTC e strumenti di gestione delle risorse, ad esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per altre informazioni sulle impostazioni predefinite di Windows Firewall e per una descrizione delle porte TCP che interessano [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]e [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Configurare Windows Firewall per consentire l'accesso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
###  <a name="isolate-services"></a><a name="isolated_services"></a> Isolate Services  
 L'isolamento dei servizi riduce il rischio che un servizio compromesso venga utilizzato per comprometterne altri. Per isolare i servizi, osservare le linee guida seguenti:  
  
-   Eseguire servizi separati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con account di Windows separati. Laddove possibile, utilizzare account di Windows o account utente locale con diritti minimi distinti per ogni servizio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
###  <a name="configure-a-secure-file-system"></a><a name="sa_with_least_privileges"></a> Configure a Secure File System  
 L'utilizzo del file system corretto garantisce una maggiore sicurezza. Per le installazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è necessario effettuare le operazioni seguenti:  
  
-   Utilizzare il file system NTFS. NTFS è il file system più appropriato per le installazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in quanto è più stabile e recuperabile rispetto ai file system FAT. NTFS supporta inoltre opzioni di sicurezza quali gli elenchi di controllo di accesso (ACL) a file e directory e la crittografia file Encrypting File System (EFS). Durante l'installazione, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] imposterà elenchi di controllo di accesso appropriati nelle chiavi del Registro di sistema e nei file se rileva il file system NTFS. Queste autorizzazioni non devono essere modificate. Per le versioni successive di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe non essere supportata l'installazione nei computer con file system di tipo FAT.  
  
    > [!NOTE]  
    >  Se si utilizza EFS, i file di database verranno crittografati utilizzando l'identità dell'account che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Solo questo account potrà decrittografare i file. Se è necessario modificare l'account che esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è innanzitutto necessario decrittografare i file utilizzando l'account precedente e quindi crittografarli nuovamente con il nuovo account.  
  
-   Utilizzare un Redundant Array of Independent Disks (RAID) per i file di dati critici.  
  
###  <a name="disable-netbios-and-server-message-block"></a><a name="disabled_protocols"></a> Disable NetBIOS and Server Message Block  
 È consigliabile disabilitare tutti i protocolli non necessari sui server della rete perimetrale, ad esempio NetBIOS e SMB (Server Message Block).  
  
 NetBIOS utilizza le porte seguenti:  
  
-   UDP/137 (servizio nomi NetBIOS)  
  
-   UDP/138 (servizio datagrammi NetBIOS)  
  
-   TCP/139 (servizio di sessione NetBIOS)  
  
 SMB utilizza le porte seguenti:  
  
-   TCP/139  
  
-   TCP/445  
  
 I server Web e DNS (Domain Name System) non necessitano di NetBIOS o SMB. In tali server disabilitare entrambi i protocolli per ridurre il rischio di enumerazione degli utenti.  
  
###  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="Install_DC"></a> Installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio  
 Per motivi di sicurezza, è consigliabile non installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ma verranno applicate le limitazioni seguenti:  
  
-   Non è possibile eseguire servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio utilizzando un account Servizio locale.  
  
-   Al termine dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer, non è possibile modificare il computer da membro di dominio a controller di dominio. Prima di modificare il computer host in controller di dominio, è necessario disinstallare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Al termine dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer, non è possibile modificare il computer da controller di dominio a membro di dominio. Prima di modificare il computer host in membro di dominio, è necessario disinstallare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Le istanze del cluster di failover di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sono supportate quando i nodi del cluster sono controller di dominio.  
  
-   Tramite il programma di installazione di[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sarà possibile creare gruppi di sicurezza o fornire account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un controller di dominio di sola lettura. In questo scenario il programma di installazione non verrà completato.  
  
## <a name="during-or-after-installation-of-ssnoversion"></a>Durante o dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Dopo l'installazione, è possibile ottimizzare la sicurezza dell'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguendo le procedure consigliate seguenti per gli account e le modalità di autenticazione.  
  
 **Account di servizio**  
  
-   Eseguire i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzando autorizzazioni quanto più restrittive possibili.  
  
-   Associare i servizi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ad account utente locali di Windows con privilegi minimi o ad account utente di dominio.  
  
-   Per altre informazioni, vedere [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
 **Modalità di autenticazione**  
  
-   Richiedere l'autenticazione di Windows per le connessioni a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Utilizzo dell'autenticazione Kerberos. Per altre informazioni, vedere [Registrare un nome dell'entità servizio per le connessioni Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  
 **Password complesse**  
  
-   Assegnare sempre una password complessa all'account **sa** .  
  
-   Abilitare sempre il controllo dei criteri password che consente di verificare la complessità e la scadenza della password.  
  
-   Utilizzare sempre password complesse per tutti gli account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Durante l'installazione di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] viene aggiunto un account di accesso per il gruppo BUILTIN\Users. In questo modo, tutti gli utenti autenticati del computer possono accedere all'istanza di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] come un membro del ruolo pubblico. L'account di accesso BUILTIN\Users può essere rimosso in sicurezza per limitare l'accesso del [!INCLUDE[ssDE](../../includes/ssde-md.md)] agli utenti di computer che dispongono di account di accesso singoli o sono membri di altri gruppi di Windows con account di accesso.  
  
## <a name="see-also"></a>Vedere anche  
 [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Protocolli e librerie di rete](../../sql-server/install/network-protocols-and-network-libraries.md)   
 [Registrazione di un nome dell'entità servizio per le connessioni Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
  
  
