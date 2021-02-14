---
title: Configurare una connessione del database del server di report (Gestione configurazione) | Microsoft Docs
description: Ogni istanza del server di report richiede una connessione al database del server di report in cui sono archiviati report, origini dei dati condivise, risorse e metadati gestiti dal server.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 01/04/2020
ms.openlocfilehash: fdbf12310fb46add3ce9e3240d7c249028eda606
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057296"
---
# <a name="configure-a-report-server-database-connection-report-server-configuration-manager"></a>Configurare una connessione del database del server di report (Gestione configurazione del server di report)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Ogni istanza del server di report richiede una connessione al database del server di report in cui sono archiviati report, origini dei dati condivise, risorse e metadati gestiti dal server. La connessione iniziale può essere creata durante l'installazione del server di report, se si sta installando la configurazione predefinita. Nella maggior parte dei casi è possibile utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per configurare la connessione al termine dell'installazione. È possibile modificare la connessione in qualsiasi momento per cambiare il tipo di account o reimpostare le credenziali. Per istruzioni dettagliate su come creare il database e configurare la connessione, vedere [Creare un database del server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).

È necessario configurare un database del server di report nei casi seguenti:  

- Configurazione di un server di report per il primo utilizzo.  

- Configurazione di un server di report per utilizzare un database diverso del server di report.  

- Modifica dell'account utente o della password utilizzati per la connessione al database. È necessario aggiornare la connessione al database solo quando le informazioni dell'account sono archiviate nel file RSReportServer.config. Se la connessione viene eseguita tramite l'account del servizio, che utilizza la sicurezza integrata di Windows come tipo di credenziali, la password non viene archiviata e non è quindi necessario aggiornare le informazioni di connessione. Per altre informazioni sulla modifica degli account, vedere [Configurare l'account del servizio del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  

- Configurazione della distribuzione con scalabilità orizzontale di un server di report. Per configurare una distribuzione con scalabilità orizzontale è necessario creare più connessioni a un database del server di report. Per altre informazioni su come eseguire questa operazione in più passaggi, vedere [Configurare una distribuzione con scalabilità orizzontale di un server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  

## <a name="how-reporting-services-connects-to-the-database-engine"></a>Modalità di connessione di Reporting Services al Motore di database

L'accesso del server di report a un database del server di report dipende dalle credenziali e dalle informazioni di connessione, nonché dalle chiavi di crittografia valide per l'istanza del server di report che utilizza quel database. Per archiviare e recuperare dati sensibili è necessario disporre di chiavi di crittografia valide. Le chiavi di crittografia vengono create automaticamente alla prima configurazione del database. In seguito alla creazione delle chiavi, è necessario aggiornarle se si modifica l'identità del servizio del server di report. Per altre informazioni sull'uso delle chiavi di crittografia, vedere [Configurare e gestire chiavi di crittografia &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  

Il database del server di report è un componente interno, a cui accede solo il server di report. Le credenziali e le informazioni di connessione specificate per il database del server di report vengono utilizzate esclusivamente dal server di report. Gli utenti che richiedono i report, non devono disporre di autorizzazioni per il database o di un account di accesso al database per il database del server di report.  

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa **System.Data.SqlClient** per connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)] che ospita il database del server di report. Se si utilizza un'istanza locale del [!INCLUDE[ssDE](../../includes/ssde-md.md)], il server di report stabilirà la connessione tramite memoria condivisa. Se si utilizza un server di database remoto per il database del server di report, a seconda dell'edizione utilizzata potrebbe essere necessario abilitare le connessioni remote. Se si usa l'edizione Enterprise Edition, le connessioni remote sono abilitate per TCP/IP per impostazione predefinita.  

Per verificare che l'istanza accetti connessioni remote, fare clic sul menu **Start**, scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, **Gestione configurazione SQL Server**, quindi verificare che il protocollo TCP/IP sia abilitato per ogni servizio.  

Quando si attivano le connessioni remote, vengono abilitati anche i protocolli client e server. Per verificare che i protocolli siano abilitati, fare clic sul menu **Start**, scegliere **Tutti i programmi**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Strumenti di configurazione**, **Gestione configurazione SQL Server**, **Configurazione di rete SQL Server**, quindi **Protocolli per MSSQLSERVER**. Per altre informazioni, vedere [Abilitare o disabilitare un protocollo di rete del server](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="defining-a-report-server-database-connection"></a>Definizione della connessione a un database del server di report

Per configurare la connessione, è necessario utilizzare Gestione configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o l'utilità della riga di comando **rsconfig** . Un server di report richiede le informazioni seguenti sulla connessione:  

- Nome dell'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] che ospita il database del server di report.  

- Nome del database del server di report. Quando si crea una connessione per la prima volta, è possibile creare un nuovo database del server di report oppure selezionare un database esistente. Per altre informazioni, vedere [Creare un database del server di report &#41;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md).  

- Tipo di credenziali. È possibile utilizzare gli account di servizio, un account di dominio di Windows o un account di accesso al database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

- Nome utente e password, necessari solo se si utilizza un account di dominio di Windows o un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Alle credenziali fornite deve essere concesso l'accesso al database del server di report. Se si utilizza lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , questo passaggio verrà eseguito automaticamente. Per ulteriori informazioni sulle autorizzazioni necessarie per accedere al database, vedere la sezione "Autorizzazioni per il database" di questo argomento.  

### <a name="storing-database-connection-information"></a>Archiviazione delle informazioni di connessione al database

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] le informazioni di connessione vengono archiviate e crittografate nelle impostazioni di RSreportserver.config seguenti. Per creare valori crittografati per queste impostazioni, è necessario utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o l'utilità rsconfig.  

Non tutti i valori vengono impostati per ogni tipo di connessione. Se si configura la connessione mediante i valori predefiniti, ovvero si stabilisce la connessione mediante gli account del servizio, \<**LogonUser**>, \<**LogonDomain**> e \<**LogonCred**> risulteranno vuoti, come indicato di seguito:  

```
<Dsn></Dsn>  
<ConnectionType></ConnectionType>  
<LogonUser></LogonUser>  
<LogonDomain></LogonDomain>  
<LogonCred></LogonCred>  
```

Se si configura la connessione per l'utilizzo di un account di Windows o un account di accesso al database specifico, è necessario ricordarsi di aggiornare i valori archiviati, se in un secondo momento si modifica l'account di Windows o quello di accesso.  

### <a name="choosing-a-credential-type"></a>Scelta del tipo di credenziali

In una connessione a un database del server di report è possibile utilizzare tre tipi di credenziali:  

- Sicurezza integrata di Windows tramite l'account del servizio del server di report. Poiché il server di report è implementato come singolo servizio, solo l'account utilizzato per l'esecuzione del servizio deve disporre di accesso al database.  
  
- Account utente di Windows. Se il server di report e il relativo database sono installati sullo stesso computer, è possibile utilizzare un account locale. In caso contrario, è necessario utilizzare un account di dominio.  
  
- Account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
> [!NOTE]
> Non è possibile utilizzare un'estensione di autenticazione personalizzata per connettersi a un database del server di report. Le estensioni di autenticazione personalizzate sono utilizzate solo per autenticare un'entità a un server di report. Tali estensioni non influiscono sulle connessioni al database del server di report o alle origini dati esterne che forniscono contenuto ai report.
  
Se l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] è configurata per l'autenticazione di Windows e si trova nello stesso dominio o in un dominio trusted con il computer server di report, è possibile configurare la connessione per l'utilizzo dell'account del servizio o di un account utente di dominio da gestire come proprietà di connessione tramite lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Se il server di database si trova in un dominio diverso o si utilizza la sicurezza dei gruppi di lavoro, è necessario configurare la connessione per l'utilizzo di un account di accesso al database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In questo caso, assicurarsi di crittografare la connessione.  

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Se si usa Istanza gestita di SQL di Azure per ospitare i database del server di report, l'autenticazione di SQL Server è l'unico tipo di credenziali supportato. È importante ricordare, inoltre, che Istanza gestita non può ospitare l'istanza del server di report.

::: moniker-end

#### <a name="using-service-accounts-and-integrated-security"></a>Utilizzo di account di servizio e sicurezza integrata

È possibile utilizzare la sicurezza integrata di Windows per connettersi tramite l'account di servizio del server di report. All'account vengono concessi diritti di accesso al database del server di report. Si tratta del tipo di credenziali predefinito scelto dal programma di installazione se [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] viene installato nella configurazione predefinita.  

L'account del servizio è un account attendibile che consente di adottare un approccio con interventi minimi di manutenzione per la gestione di una connessione al database del server di report. Poiché l'account del servizio utilizza la sicurezza integrata di Windows per stabilire la connessione, non è necessario archiviare le credenziali. Se in seguito, tuttavia, si modifica la password dell'account del servizio o l'identità, ad esempio passando da un account predefinito a un account di dominio, assicurarsi di utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per apportare la modifica. Lo strumento aggiorna automaticamente le autorizzazioni per il database per utilizzare le informazioni sull'account modificate. Per altre informazioni, vedere [Configurare l'account del servizio del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  

Se si configura la connessione di database per l'utilizzo dell'account del servizio, l'account dovrà disporre di autorizzazione di rete se il database del server di report si trova in un computer remoto. Non utilizzare l'account di servizio se il database del server di report si trova in un dominio diverso, dietro un firewall o se si utilizza la sicurezza dei gruppi di lavoro anziché la sicurezza di dominio. Utilizzare invece un account utente del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

#### <a name="using-a-domain-user-account"></a>Utilizzo di un account utente di dominio

È possibile specificare un account utente di Windows per la connessione del server di report al database del server di report. Se si utilizza un account locale o di dominio, è necessario aggiornare la connessione al database del server di report ogni volta che si modifica la password o l'account. Per aggiornare la connessione, utilizzare sempre lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  

#### <a name="using-a-sql-server-login"></a>Utilizzo di un account di accesso di SQL Server

È possibile specificare un singolo account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al database del server di report. Se si usa l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il database del server di report si trova in un computer remoto, usare IPSec per proteggere la trasmissione dei dati tra i server. Se si utilizza un account di accesso al database, è necessario aggiornare la connessione al database del server di report ogni volta che si modifica la password o l'account.

### <a name="database-permissions"></a>Autorizzazioni per il database

Agli account utilizzati per connettersi al database del server di report vengono concessi i ruoli seguenti:  

- Ruoli **public** e **RSExecRole** per il database **ReportServer** .  

- Ruolo **RSExecRole** per i database **master**, **msdb** e **ReportServerTempDB** .  

Quando si utilizza lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per creare o modificare la connessione, queste autorizzazioni vengono concesse automaticamente. Se si utilizza l'utilità rsconfig e si specifica un account diverso per la connessione, è necessario aggiornare l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il nuovo account. È possibile creare file script nello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per aggiornare l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il server di report.  

### <a name="verifying-the-database-name"></a>Verifica del nome di database

Utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per determinare il database del server di report utilizzato da un'istanza del server di report specifica. Per individuare il nome, connettersi all'istanza del server di report e aprire la pagina Impostazioni database.  

## <a name="using-a-different-report-server-database-or-moving-a-report-server-database"></a>Utilizzo di un database diverso del server di report o spostamento di un database del server di report

È possibile configurare un'istanza del server di report in modo da utilizzare un database diverso del server di report modificando le informazioni di connessione. In genere, si rende necessario cambiare database quando si distribuisce un server di report di produzione. In questo caso si passa da un database del server di report di prova a un database del server di report di produzione. È inoltre possibile spostare un database del server di report in un altro computer. Per altre informazioni, vedere [Eseguire aggiornamento e migrazione di Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="configuring-multiple-reports-servers-to-use-the-same-report-server-database"></a>Configurazione di più server di report per garantire l'utilizzo dello stesso database del server di report

È possibile configurare più server di report in modo che utilizzino lo stesso database del server di report. Questa configurazione di distribuzione è denominata distribuzione con scalabilità orizzontale e costituisce un prerequisito se si desidera eseguire più server di report in un cluster di server. È tuttavia possibile utilizzare tale configurazione se si desidera segmentare le applicazioni del servizio o eseguire il test dell'installazione e delle impostazioni di una nuova istanza del server di report per confrontarla con un'installazione del server di report esistente. Per altre informazioni, vedere [Configurare una distribuzione con scalabilità orizzontale di un server di report in modalità nativa &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  

## <a name="next-steps"></a>Passaggi successivi

- [Creare un database del server di report](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
- [Gestire un server di report in modalità nativa di Reporting Services](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
- [Configurare l'account del servizio del server di report](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)

Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
