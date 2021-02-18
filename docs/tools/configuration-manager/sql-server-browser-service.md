---
title: SQL Server Browser Service
description: Di seguito viene descritto come usare SQL Server Browser, un servizio che resta in ascolto delle richieste di risorse di SQL Server e fornisce informazioni sulle istanze di SQL Server installate.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.browseservers.local.f1
- sql13.swb.browseservers.network.f1
helpviewer_keywords:
- services [SQL Server], security
- SQL Browser service (See SQL Server Browser Service)
- Browser Service
- SQL Server Browser service
ms.assetid: 3cc00d3a-487c-4cd9-a155-655f02485fa0
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a4c58894434c503f92c8bcf4df4f74cf9fa2c688
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345191"
---
# <a name="sql-server-browser-service"></a>SQL Server Browser Service
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Il programma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Browser viene eseguito come servizio Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser rimane in attesa delle richieste in entrata di risorse di [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e fornisce informazioni sulle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installate nel computer. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser consente di eseguire le azioni seguenti:  
  
-   Esplorazione di un elenco di server disponibili  
  
-   Connessione all'istanza del server corretta  
  
-   Connessione a endpoint della connessione amministrativa dedicata (DAC)  
  
 Per ogni istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e di [!INCLUDE[ssAS](../../includes/ssas-md.md)], il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser (sqlbrowser) fornisce il nome dell'istanza e il numero di versione. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene installato con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser può essere configurato durante l'installazione o mediante Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per impostazione predefinita, il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene avviato automaticamente:  
  
-   Durante l'aggiornamento di un'installazione.  
  
-   Durante l'installazione in un cluster.  
  
-   Durante l'installazione di un'istanza denominata del [!INCLUDE[ssDE](../../includes/ssde-md.md)] che include tutte le istanze di SQL Server Express.  
  
-   Durante l'installazione di un'istanza denominata di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="background"></a>Background  
 Nelle versioni precedenti a [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]è possibile installare solo un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un computer. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] resta in attesa delle richieste in entrata sulla porta 1433, assegnata a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dall'autorità ufficiale IANA (Internet Assigned Numbers Authority). Poiché solo un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare una porta, quando in [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] è stato introdotto il supporto per più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è stato sviluppato il protocollo SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol) per l'ascolto sulla porta UDP 1434. Il servizio listener risponde alle richieste del client con i nomi delle istanze installate e le porte o le named pipe usate dall'istanza. Per risolvere i problemi connessi ai suoi limiti, in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] il sistema SSRP è stato sostituito dal servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser.  
  
## <a name="how-sql-server-browser-works"></a>Funzionamento di SQL Server Browser  
 Quando viene avviata un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se il protocollo TCP/IP è abilitato per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], al server viene assegnata una porta TCP/IP. Se è abilitato il protocollo Named Pipes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rimane in attesa su una named pipe specifica. Questa porta, o "pipe", viene usata dall'istanza specifica per scambiare dati con le applicazioni client. Durante l'installazione, la porta TCP 1433 e la pipe `\sql\query` vengono assegnate all'istanza predefinita, ma possono essere cambiate in seguito dall'amministratore del server usando Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Poiché solo un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può usare una porta o una pipe, vengono assegnati diversi numeri di porta e nomi di pipe alle istanze denominate, incluso [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Per impostazione predefinita, quando sono abilitati, sia le istanze denominate che [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] sono configurati per l'utilizzo di porte dinamiche, ovvero viene assegnata una porta disponibile quando viene avviato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se lo si desidera, è possibile assegnare una porta specifica a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Durante la connessione, i client possono specificare una determinata porta, ma se la porta viene assegnata in modo dinamico, il numero di porta può essere modificato a ogni riavvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e, pertanto, il numero di porta corretto non è noto al client.  
  
 All'avvio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene avviato e richiede la porta UDP 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser legge il Registro di sistema, identifica tutte le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer e rileva le porte e le named pipe usate. Quando in un server sono installate due o più schede di rete, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser restituisce la prima porta abilitata rilevata per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser supporta ipv6 e ipv4.  
  
 Quando i client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiedono le risorse [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la libreria di rete client invia un messaggio UDP al server usando la porta 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser risponde con la porta TCP/IP o la named pipe dell'istanza richiesta. La libreria di rete dell'applicazione client completa quindi la connessione inviando una richiesta al server tramite la porta o la named pipe dell'istanza desiderata.  
  
 Per informazioni sull'avvio e l'arresto del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, vedere la documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="using-sql-server-browser"></a>Utilizzo di SQL Server Browser  
 Se il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser non è in esecuzione, è comunque possibile connettersi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indicando la named pipe o il numero di porta corretto. È ad esempio possibile connettersi all'istanza predefinita di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con TCP/IP se l'istanza è in esecuzione nella porta 1433.  
  
 Se tuttavia il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser non è in esecuzione, non funzionano le connessioni seguenti:  
  
-   Qualsiasi componente che tenta di eseguire la connessione a un'istanza denominata senza specificare completamente i parametri, ad esempio la porta TCP/IP o la named pipe.  
  
-   Qualsiasi componente che genera o passa informazioni server/istanza che potrebbero essere usate in seguito da altri componenti per eseguire nuovamente la connessione.  
  
-   Connessione a un'istanza denominata senza indicare il numero di porta o la pipe.  
  
-   Connessione amministrativa dedicata (DAC) a un'istanza denominata o all'istanza predefinita se non si usano la porta TCP/IP 1433.  
  
-   Servizio redirector OLAP.  
  
-   Enumerazione dei server in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], Enterprise Manager o Query Analyzer.  
  
 Se si usano [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in uno scenario client-server, ad esempio quando l'applicazione accede a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] su una rete, e si arresta o disabilita il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser, è necessario assegnare un numero di porta specifico a ogni istanza e scrivere il codice dell'applicazione client affinché venga sempre usato il numero di porta assegnato. Questo approccio comporta i problemi indicati di seguito.  
  
-   È necessario aggiornare e mantenere aggiornato il codice dell'applicazione client per assicurarsi che si connetta alla porta corretta.  
  
-   La porta scelta per ogni istanza potrebbe essere usata da un altro servizio o un'altra applicazione nel server, con conseguente non disponibilità dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="clustering"></a>Clustering  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser non è una risorsa cluster e non supporta il failover tra nodi del cluster. Nel caso di un cluster, è pertanto consigliabile installare e abilitare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser per ogni nodo del cluster. Sui cluster [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser è in attesa su IP_ANY.  
  
> [!NOTE]  
>  Durante l'attesa su IP_ANY, quando si abilita l'attesa su IP specifici, l'utente deve configurare la stessa porta TCP in ogni IP, poiché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser restituisce la prima coppia IP/porta rilevata.  
  
## <a name="installing-uninstalling-and-running-from-the-command-line"></a>Installazione, disinstallazione ed esecuzione dalla riga di comando  
 Per impostazione predefinita, il programma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene installato in C:\Programmi(x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe.  
  
 Il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene disinstallato quando viene rimossa l'ultima istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser può essere avviato dal prompt dei comandi, ai fini della risoluzione dei problemi, usando l'opzione **-c** :  
  
```  
<drive>\<path>\sqlbrowser.exe -c  
```  
  
## <a name="security"></a>Sicurezza  
  
### <a name="account-privileges"></a>Privilegi dell'account  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser rimane in attesa su una porta UDP e accetta le richieste non autenticate tramite il protocollo SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser deve essere eseguito nel contesto di sicurezza di un utente con pochi privilegi per ridurre l'esposizione agli attacchi da parte di utenti malintenzionati. È possibile modificare l'account di accesso usando Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Di seguito vengono indicati i diritti utente minimi per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser:  
  
-   Nega accesso al computer dalla rete  
  
-   Nega accesso locale  
  
-   Nega accesso come processo batch  
  
-   Nega accesso tramite Servizi terminal  
  
-   Accedi come servizio  
  
-   Lettura e scrittura per le chiavi del Registro di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correlate alle comunicazioni di rete (porte e pipe)  
  
### <a name="default-account"></a>Account predefinito  
 Tramite il programma di installazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser viene configurato per l'utilizzo dell'account selezionato per i servizi durante l'installazione. Tra gli altri account possibili sono inclusi i seguenti:  
  
-   Tutti gli account **di dominio\locali**  
  
-   Account **Servizio locale**  
  
-   Account **Sistema locale** (sconsigliabile perché include privilegi non necessari)  
  
### <a name="hiding-sql-server"></a>Istanze nascoste di SQL Server  
 Le istanze nascoste sono istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che supportano solo connessioni della memoria condivisa. Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], impostare il flag `HideInstance` per indicare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser non deve rispondere inviando alcuna informazione su questa istanza del server.  
  
### <a name="using-a-firewall"></a>Utilizzo di un firewall  
 Per comunicare con il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser in un server protetto da un firewall, aprire la porta UDP 1434 oltre alla porta TCP usata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio la porta 1433. Per informazioni sull'utilizzo di un firewall, vedere "Procedura: Configurare un firewall per l'accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Protocolli e librerie di rete](../../sql-server/install/network-protocols-and-network-libraries.md)  
  
  
