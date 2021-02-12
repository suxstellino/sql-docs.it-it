---
title: File di configurazione RsReportServer.config | Microsoft Docs
description: Informazioni sul file di configurazione in cui vengono archiviate le impostazioni usate dal servizio Web ReportServer e dall'elaborazione in background.
ms.date: 05/01/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 60e0a0b2-8a47-4eda-a5df-3e5e403dbdbc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7566e003e9b93377f54a3775ae8ca6ecf119c1f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024259"
---
# <a name="rsreportserverconfig-configuration-file"></a>RsReportServer.config Configuration File
Nel file [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**RsReportServer.config** vengono archiviate le impostazioni usate dal servizio Web ReportServer e dall'elaborazione in background. Tutte le applicazioni [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vengono eseguite all'interno di un singolo processo tramite cui è possibile leggere le impostazioni di configurazione archiviate nel file RSReportServer.config. Il file RSReportServer.config viene utilizzato nei server di report sia in modalità nativa, sia in modalità SharePoint. Nelle due modalità non vengono tuttavia utilizzate tutte le stesse impostazioni disponibili nel file di configurazione. La versione per la modalità SharePoint del file è più piccola, poiché molte delle impostazioni per la modalità SharePoint sono archiviate nei database di configurazione di SharePoint anziché nel file. In questo argomento viene descritto il file di configurazione predefinito installato per la modalità nativa e la modalità SharePoint e alcune delle impostazioni e dei comportamenti importanti controllati dal file di configurazione.  

In modalità SharePoint, il file di configurazione contiene le impostazioni che si applicano a tutte le istanze dell'applicazione di servizio in esecuzione nel computer. Il database di configurazione di SharePoint contiene impostazioni di configurazione che si applicano a specifiche applicazioni di servizio. Le impostazioni archiviate nel database di configurazione e gestite tramite le pagine di gestione di SharePoint possono essere diverse per ogni applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Le impostazioni vengono presentate nel contenuto riportato di seguito nell'ordine in cui vengono visualizzate nel file di configurazione installato per impostazione predefinita. Per le istruzioni su come modificare questo file, vedere [Modificare un file di configurazione di Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
 
##  <a name="file-location"></a><a name="bkmk_file_location"></a> Percorso del file  

Il file RSReportServer.config si trova nelle cartelle seguenti, a seconda della modalità del server di report:  


  
### <a name="native-mode-report-server"></a>Server di report in modalità nativa 

 
**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)]

```  
C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
```

**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)]

```  
C:\Program Files\Microsoft SQL Server Reporting Services\SSRS\ReportServer
```  

**[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)]** [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

```  
C:\Program Files\Microsoft Power BI Report Server\PBIRS\ReportServer
```  
  
### <a name="sharepoint-mode-report-server"></a>Server di report in modalità SharePoint 

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.
  
```  
C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting  
```  
 
Per altre informazioni vedere [Modificare un file di configurazione di Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
##  <a name="general-configuration-settings-rsreportserverconfig"></a><a name="bkmk_generalconfiguration"></a> Impostazioni di configurazione generali (rsreportserver.config)  
 Nella tabella seguente vengono fornite le informazioni sulle impostazioni di configurazione generali visualizzate nella prima parte del file. Le impostazioni sono elencate nell'ordine in cui vengono visualizzate nel file di configurazione. Nell'ultima colonna della tabella viene indicato se l'impostazione si applica a un server di report in modalità nativa **(N)** , a un server di report in modalità SharePoint **(S)** o a entrambi.  
  
> [!NOTE]  
>  In questo argomento, con "numero intero massimo" viene fatto riferimento a un valore di INT_MAX pari a 2147483647.  Per altre informazioni, vedere [Integer Limits](https://msdn.microsoft.com/library/296az74e\(v=vs.110\).aspx) (Limiti Integer) (https://msdn.microsoft.com/library/296az74e(v=vs.110).aspx).  
  
|Impostazione|Descrizione|Mode|  
|-------------|-----------------|----------|  
|**Dsn**|Consente di specificare la stringa di connessione al server di database che ospita il database del server di report. Questo valore è crittografato e viene aggiunto al file di configurazione quando si crea il database del server di report. Per SharePoint, le informazioni di connessione al database vengono prese dal database di configurazione di SharePoint.|N, S|  
|**ConnectionType**|Consente di specificare il tipo di credenziali utilizzato dal server di report per la connessione al relativo database. I valori validi sono **Default** e **Impersonate**. Il valore **Default** viene specificato se il server di report è configurato per l'uso dell'account del servizio o di un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la connessione al database del server di report. Il valore **Impersonate** viene specificato se il server di report usa un account di Windows per la connessione al database del server di report.|N|  
|**LogonUser, LogonDomain, LogonCred**|Consente di archiviare il dominio, il nome utente e la password di un account di dominio utilizzato da un server di report per la connessione a un database del server di report. I valori per **LogonUser**, **LogonDomain** e **LogonCred** vengono creati quando la connessione al server di report è configurata per l'uso di un account di dominio. Per altre informazioni sulla connessione di database del server di report, vedere [Configurare una connessione del database del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).|N|  
|**InstanceID**|Identificatore dell'istanza del server di report. I nomi delle istanze del server di report si basano sui nomi delle istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questo valore specifica il nome di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per impostazione predefinita, questo valore è **MSRS12** _\<instancename>_ . Non modificare questa impostazione. Di seguito è riportato un esempio del valore completo: `<InstanceId>MSRS13.MSSQLSERVER</InstanceId>`<br /><br /> Un esempio della modalità SharePoint è il seguente:<br /><br /> `<InstanceId>MSRS12.@Sharepoint</InstanceId>`|N, S|  
|**InstallationID**|Identificatore dell'installazione del server di report creato durante l'installazione. Questo valore è impostato su un GUID. Non modificare questa impostazione.|N|  
|**SecureConnectionLevel**|Specifica il livello in base al quale il servizio Web deve usare Transport Layer Security (TLS), noto in precedenza come Secure Sockets Layer (SSL). Questa impostazione viene usata sia per il servizio Web ReportServer che per il portale Web. Il valore è impostato quando si configura un URL per utilizzare HTTP o HTTPS nello strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . In SQL Server 2008 R2 SecureConnectionLevel diventa un'opzione di attivazione/disattivazione. Per le versioni precedenti a SQL Server 2008 R2 i valori validi sono compresi tra 0 e 3 e 0 è l'opzione meno sicura. Per altre informazioni, vedere [Metodo ConfigurationSetting - SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md), [Uso di metodi del servizio Web protetti](../../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md) e [Configurare connessioni TLS in un server di report in modalità nativa](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).|N, S|
|**DisableSecureFormsAuthenticationCookie**|Il valore predefinito è False.<br /><br /> Specifica se disabilitare l'applicazione del cookie utilizzato affinché l'autenticazione personalizzata e basata su form venga contrassegnata come sicura. A partire da SQL Server 2012, in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] i cookie di autenticazione basata su form utilizzati con estensioni di autenticazione personalizzate verranno contrassegnati automaticamente come sicuri quando inviati al client. Se si modifica questa proprietà, gli amministratori del server di report e gli autori delle estensioni di sicurezza personalizzate possono ripristinare il comportamento precedente in base al quale l'autore dell'estensione di sicurezza personalizzata poteva stabilire se contrassegnare il cookie come sicuro. Si consiglia l'utilizzo di cookie sicuri affinché l'autenticazione basata su form consenta di evitare lo sniffing in rete e attacchi di tipo replay.|N|  
|**CleanupCycleMinutes**|Consente di specificare dopo quanti minuti le sessioni precedenti e gli snapshot scaduti vengono rimossi dai database del server di report. I valori validi sono compresi nell'intervallo da 1 al numero intero massimo. Il valore predefinito è 10. |N, S|  
|**MaxActiveReqForOneUser**|Consente di specificare il numero massimo di report che ogni singolo utente può elaborare contemporaneamente. Quando viene raggiunto il limite impostato, vengono negate ulteriori richieste di elaborazione. I valori validi sono compresi tra 1 e il valore integer massimo. Il valore predefinito è 20.<br /><br /> Si noti che la maggior parte delle richieste viene elaborata in tempi molto rapidi, per cui è improbabile che un singolo utente abbia più di 20 connessioni aperte contemporaneamente. Se gli utenti aprono contemporaneamente più di 15 report con requisiti di elaborazione elevati, potrebbe essere necessario aumentare questo valore.<br /><br /> Questa impostazione viene ignorata per i server di report in esecuzione in modalità integrata SharePoint.|N, S|  
|**MaxActiveReqForAnonymous**|Specifica il numero massimo di richieste anonime che possono essere elaborate contemporaneamente. Quando viene raggiunto il limite, vengono negate ulteriori richieste di elaborazione. I valori validi sono compresi tra 1 e il valore integer massimo. Il valore predefinito è 200.
|**DatabaseQueryTimeout**|Consente di specificare quanti secondi devono trascorrere prima del timeout di una connessione al database del server di report. Questo valore viene passato alla proprietà System.Data.SQLClient.SQLCommand.CommandTimeout. I valori validi sono compresi tra 0 e 2147483647. Il valore predefinito è 120. Un valore uguale a 0 specifica un tempo di attesa illimitato e pertanto non è consigliato.|N|  
|**AlertingCleanupCycleMinutes**|Il valore predefinito è 20.<br /><br /> Determina la frequenza con cui eseguire la pulizia di dati temporanei archiviati nel database di avvisi.|S|  
|**AlertingDataCleanupMinutes**|Il valore predefinito è 360.<br /><br /> Determina per quanto tempo i dati sessione utilizzati per la creazione o la modifica di una definizione di avviso vengono mantenuti nel database di avvisi. Il valore predefinito è 6 ore.|S|  
|**AlertingExecutionLogCleanupMinutes**|Il valore predefinito è 10080.<br /><br /> Determina per quanto tempo mantenere i valori del log di esecuzione degli avvisi. Il valore predefinito è 7 giorni.|S|  
|**AlertingMaxDataRetentionDays**|Il valore predefinito è 180.<br /><br /> Determina per quanto tempo mantenere i dati di avviso necessari per evitare messaggi di avviso duplicati quando i dati per l'avviso non vengono modificati.|S|  
|**RunningRequestsScavengerCycle**|Consente di specificare la frequenza con cui le richieste orfane e le richieste scadute vengono annullate. Il valore è espresso in secondi. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è 60.|N, S|  
|**RunningRequestsDbCycle**|Consente di specificare la frequenza con cui il server di report valuta i processi in esecuzione per stabilire se è stato superato il timeout per l'esecuzione dei report e quando presentare informazioni sui processi in esecuzione nella pagina Gestisci processi del portale Web. Il valore è espresso in secondi. I valori validi sono compresi tra 0 e 2147483647. Il valore predefinito è 60.|N, S|  
|**RunningRequestsAge**|Consente di specificare l'intervallo, espresso in secondi, dopo il quale lo stato di un processo passa da nuovo a in esecuzione. I valori validi sono compresi tra 0 e 2147483647. Il valore predefinito è 30.|N, S|  
|**MaxScheduleWait**|Specifica i secondi di attesa del servizio Windows ReportServer prima dell'aggiornamento di una pianificazione da parte del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent quando l'opzione **Prossima esecuzione** viene richiesta. I valori validi sono compresi tra 1 e 60.<br /><br /> Nel file di configurazione predefinita, MaxScheduleWait è impostato su **5**.<br /><br /> Se è impossibile trovare o leggere il file di configurazione nel server di report, viene utilizzato il valore predefinito 1 per MaxScheduleWait.|N, S|  
|**DisplayErrorLink**|Indica se viene visualizzato un collegamento al sito Guida e supporto tecnico [!INCLUDE[msCoName](../../includes/msconame-md.md)] in caso di errore. Il collegamento viene visualizzato nei messaggi di errore e consente agli utenti di accedere al sito per visualizzare le informazioni aggiornate sugli errori disponibili. I valori validi includono **True** (impostazione predefinita) e **False**.|N, S|  
|**WebServiceuseFileShareStorage**|Consente di specificare se archiviare nel file system i report memorizzati nella cache e gli snapshot temporanei creati dal servizio Web ReportServer per la durata di una sessione utente. I valori validi sono **True** e **False** (impostazione predefinita). Se il valore è impostato su false, i dati temporanei vengono archiviati nel database reportservertempdb.|N, S|  
|**ProcessTimeout**|Consente di specificare il numero di secondi che Process Monitor dovrà attendere prima di completare qualsiasi operazione di attività del servizio prima di poterlo arrestare. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è 150. Questa impostazione è disabilitata per impostazione predefinita e può essere abilitata rimuovendo la sintassi di commento (```<!-- and -->```).|N|  
|**ProcessTimeoutGcExtension**|Consente di specificare il numero di secondi che la funzionalità Process Monitor del server di report dovrà attendere prima di completare un'operazione di attività del servizio prima di poterlo arrestare. Questa impostazione si applica solo se è in esecuzione Garbage Collection .NET e se è stato raggiunto il valore di ProcessTimeout. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è 30. Questa impostazione è disabilitata per impostazione predefinita e può essere abilitata rimuovendo la sintassi di commento (```<!-- and -->```).|N|  
|**WatsonFlags**|Specifica quante informazioni vengono registrate per le condizioni di errore segnalate a [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> 0x0430 = dump completo<br /><br /> 0x0428 = minidump<br /><br /> 0x0002 = nessun dump|N, S|  
|**WatsonDumpOnExceptions**|Specifica un elenco di eccezioni da segnalare in un log degli errori. Ciò si rivela utile in presenza di un problema ricorrente se si desidera creare un dump con informazioni da inviare a [!INCLUDE[msCoName](../../includes/msconame-md.md)] per l'analisi. Poiché la creazione di dump influisce sulle prestazioni, modificare questa impostazione solo durante la diagnosi di un problema.|N, S|  
|**WatsonDumpExcludeIfContainsExceptions**|Specifica un elenco di eccezioni da non segnalare in un log degli errori. Ciò si rivela utile durante la diagnosi di un problema se non si desidera che il server crei dump per eccezioni specifiche.|N, S|  
  
##  <a name="urlreservations-rsreportserverconfig-file"></a><a name="bkmk_URLReservations"></a> URLReservations (file RSReportServer.config)  
 **URLReservations** permette di definire l'accesso HTTP al servizio Web ReportServer e al portale Web per l'istanza corrente. Gli URL sono riservati e vengono archiviati in HTTP.SYS quando si configura il server di report.  
  
> [!WARNING]  
>  Per la modalità SharePoint, le prenotazioni di URL vengono configurate in Amministrazione centrale SharePoint. Per altre informazioni, vedere [Configurare il mapping di accesso alternativo (https://technet.microsoft.com/library/cc263208(office.12).aspx)](https://technet.microsoft.com/library/cc263208\(office.12\).aspx).  
  
 Non modificare direttamente prenotazioni URL nel file di configurazione. Per creare o modificare prenotazioni URL per un server di report in modalità nativa, utilizzare sempre Gestione configurazione [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o il provider WMI del server di report. Se si modificano i valori nel file di configurazione, è possibile danneggiare la prenotazione, provocando errori del server in fase di esecuzione o lasciando prenotazioni orfane in HTTP.SYS che non vengono rimosse se si disinstalla il software. Per altre informazioni, vedere [Configurare gli URL del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md) e [URL nei file di configurazione &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md).  
  
 **URLReservations** è un elemento facoltativo. Se non è presente nel file RSReportServer.config, il server potrebbe non essere configurato. Se viene specificato, tutti gli elementi figlio sono obbligatori, ad eccezione di **AccountName** .  
  
 Nell'ultima colonna della tabella viene indicato se l'impostazione si applica a un server di report in modalità nativa (N), a un server di report in modalità SharePoint (S) o a entrambi.  
  
|Impostazione|Descrizione|Mode|  
|-------------|-----------------|----------|  
|**Applicazione**|Contiene le impostazioni per le applicazioni [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|N|  
|**Nome**|Specifica le applicazioni [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . I valori validi sono ReportServerWebService o ReportManager.|N|  
|**VirtualDirectory**|Specifica il nome della directory virtuale dell'applicazione.|N|  
|**URL**|Contiene una o più prenotazioni URL per l'applicazione.|N|  
|**UrlString**|Specifica la sintassi dell'URL valida per HTTP.SYS. Per altre informazioni sulla sintassi, vedere [Sintassi delle prenotazioni URL &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md).|N|  
|**AccountSid**|Consente di specificare l'ID di sicurezza (SID) dell'account per cui viene creata la prenotazione URL. Deve corrispondere all'account utilizzato per l'esecuzione del servizio del server di report. Se il SID non corrisponde all'account del servizio, il server di report potrebbe non essere in grado di restare in attesa di richieste sull'URL.|N|  
|**AccountName**|Specifica un nome di account leggibile che corrisponde ad **AccountSid**. Questa impostazione non viene utilizzata, ma è presente nel file in modo che sia possibile determinare agevolmente l'account del servizio utilizzato per la prenotazione URL.|N|  
  
##  <a name="authentication-rsreportserverconfig-file"></a><a name="bkmk_Authentication"></a> Authentication (file RSReportServer.config)  
 **Authentication** specifica uno o più tipi di autenticazione accettati dal server di report. Le impostazioni e i valori predefiniti sono un subset di quelli possibili per questa sezione. Solo le impostazioni predefinite vengono aggiunte automaticamente. Per aggiungere altre impostazioni, è necessario utilizzare un editor di testo per aggiungere la struttura dell'elemento al file RSReportServer.config e impostare i valori.  
  
 I valori predefiniti includono **RSWindowsNegotiate** e **RSWindowsNTLM** con **EnableAuthPersistance** impostato su **True**:  
  
```  
   <Authentication>  
      <AuthenticationTypes>  
         <RSWindowsNegotiate/>  
         <RSWindowsNTLM/>  
      </AuthenticationTypes>  
      <EnableAuthPersistence>true</EnableAuthPersistence>  
   </Authentication>  
```  
  
 Tutti gli altri valori devono essere aggiunti manualmente. Per altre informazioni ed esempi, vedere [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md).  
  
 Nell'ultima colonna della tabella riportata di seguito viene indicato se l'impostazione si applica a un server di report in modalità nativa (N), a un server di report in modalità SharePoint (S) o a entrambi.  
  
|Impostazione|Descrizione|Mode|  
|-------------|-----------------|----------|  
|**AuthenticationTypes**|Consente di specificare uno o più tipi di autenticazione. I valori validi sono: **RSWindowsNegotiate**, **RSWindowsKerberos**, **RSWindowsNTLM**, **RSWindowsBasic**, e **Custom**.<br /><br /> I tipi **RSWindows** e **Custom** si escludono a vicenda.<br /><br /> **RSWindowsNegotiate**, **RSWindowsKerberos**, **RSWindowsNTLM** e **RSWindowsBasic** sono cumulativi e possono essere usati insieme, come illustrato in precedenza nell'esempio relativo al valore predefinito in questa sezione.<br /><br /> È necessario specificare più tipi di autenticazione se si prevedono richieste da una varietà di applicazioni client o browser che utilizzano tipi di autenticazione diversi.<br /><br /> Non rimuovere **RSWindowsNTLM**. In caso contrario verrà supportata solo una parte dei tipi di browser possibili. Per altre informazioni, vedere [Supporto browser per Reporting Services e Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).|N|  
|**RSWindowsNegotiate**|Il server di report accetta token di sicurezza Kerberos o NTLM. Si tratta dell'impostazione predefinita quando il server di report viene eseguito in modalità nativa e l'account del servizio utilizzato è Servizio di rete. Questa impostazione viene omessa quando il server di report viene eseguito in modalità nativa e l'account del servizio è configurato come account utente di dominio.<br /><br /> Se un account di dominio è configurato per l'account del servizio del server di report e per il server di report non è configurato alcun nome SPN, questa impostazione potrebbe impedire agli utenti di accedere al server.|N|  
|**RSWindowsNTLM**|Il server accetta token di sicurezza NTLM.<br /><br /> Se si rimuove questa impostazione, il supporto browser sarà limitato per alcuni dei tipi di browser supportati. Per altre informazioni, vedere [Supporto browser per Reporting Services e Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).|N, S|  
|**RSWindowsKerberos**|Il server accetta token di sicurezza Kerberos.<br /><br /> Utilizzare questa impostazione oppure RSWindowsNegotiate quando si utilizza l'autenticazione Kerberos in uno schema di autenticazione della delega vincolata.|N|  
|**RSWindowsBasic**|Il server accetta credenziali di base e invia una sequenza In attesa/Risposta quando viene effettuata una connessione senza credenziali.<br /><br /> L'autenticazione di base passa le credenziali nelle richieste HTTP come testo non crittografato. Se si usa l'autenticazione di base, usare TLS per crittografare il traffico di rete da e verso il server di report. Per visualizzare un esempio di sintassi di configurazione per l'autenticazione di base in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md).|N|  
|**Impostazione personalizzata**|Specificare questo valore se è stata distribuita un'estensione di sicurezza personalizzata nel computer server di report. Per ulteriori informazioni, vedere [Implementing a Security Extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).|N|  
|**LogonMethod**|Con questo valore è possibile specificare il tipo di accesso per **RSWindowsBasic**. Se si specifica **RSWindowsBasic**, questo valore è necessario. I valori validi sono 2 o 3. Ogni valore rappresenta gli elementi seguenti:<br /><br /> **2** = accesso alla rete per server ad alte prestazioni per l'autenticazione di password non crittografate<br /><br /> **3** = accesso non crittografato, che mantiene le credenziali di accesso nel pacchetto di autenticazione inviato con ogni richiesta HTTP, consentendo al server di rappresentare l'utente in caso di connessione ad altri server della rete.<br /><br /> <br /><br /> Nota: I valori 0 (per accesso interattivo) e 1 (per accesso batch) non sono supportati in [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)].|N|  
|**Realm**|Questo valore viene utilizzato per **RSWindowsBasic** e specifica una partizione delle risorse che include funzionalità di autorizzazione e autenticazione utilizzate per controllare l'accesso alle risorse protette nell'organizzazione.|N|  
|**DefaultDomain**|Questo valore viene utilizzato per **RSWindowsBasic** al fine di determinare il dominio utilizzato dal server per autenticare l'utente. Questo valore è facoltativo, ma se non viene specificato il server di report utilizzerà il nome del computer come dominio. Se il server di report è stato installato in un controller di dominio, il dominio utilizzato è quello controllato dal computer.|N|  
|**RSWindowsExtendedProtectionLevel**|Il valore predefinito è **off**. Per altre informazioni, vedere [Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)|N|  
|**RSWindowsExtendedProtectionScenario**|Il valore predefinito è **Proxy**|N|  
|**EnableAuthPersistence**|Determina se l'autenticazione viene eseguita sulla connessione o per ogni richiesta.<br /><br /> I valori validi sono **True** (impostazione predefinita) e **False**. Se il valore è impostato su **True**, per le richieste successive provenienti dalla stessa connessione viene usato il contesto di rappresentazione della prima richiesta.<br /><br /> Se si usa software per server proxy (ad esempio ISA Server) per accedere al server di report, il valore deve essere impostato su **False** . Un server proxy consente a più utenti di utilizzare una sola connessione dal server stesso. Per questo scenario, è necessario disabilitare la persistenza dell'autenticazione affinché sia possibile autenticare separatamente la richiesta di ogni utente. Se non si imposta **EnableAuthPersistence** su **False**, tutti gli utenti si connetteranno usando il contesto di rappresentazione della prima richiesta.|N, S|  
  
##  <a name="service-rsreportserverconfig-file"></a><a name="bkmk_service"></a> Service (file RSReportServer.config)  
 **Service** specifica le impostazioni dell'applicazione da applicare nel complesso al servizio.  
  
 Nell'ultima colonna della tabella riportata di seguito viene indicato se l'impostazione si applica a un server di report in modalità nativa (N), a un server di report in modalità SharePoint (S) o a un server di report di Power BI (P).  

  
|Impostazione|Descrizione|Mode|  
|-------------|-----------------|----------|  
|**IsSchedulingService**|Specifica se tramite il server di report è possibile gestire un set di processi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent che corrispondono a pianificazioni e sottoscrizioni create dagli utenti di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . I valori validi includono **True** (impostazione predefinita) e **False**.<br /><br /> Questa impostazione viene modificata quando si abilitano o disabilitano le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usando il facet Configurazione superficie di attacco per Reporting Services della gestione basata sui criteri. Per altre informazioni, vedere [Avviare e arrestare il servizio del server di report](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsNotificationService**|Consente di specificare se il server di report elabora notifiche e recapiti. I valori validi includono **True** (impostazione predefinita) e **False**. Quando il valore è **False**, le sottoscrizioni non vengono recapitate.<br /><br /> Questa impostazione viene modificata quando si abilitano o disabilitano le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usando il facet Configurazione superficie di attacco per Reporting Services della gestione basata sui criteri. Per altre informazioni, vedere [Avviare e arrestare il servizio del server di report](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsEventService**|Consente di specificare se i processi dei servizi sono eventi nella coda degli eventi. I valori validi includono **True** (impostazione predefinita) e **False**. Quando il valore è **False**, il server di report non esegue operazioni per pianificazioni o sottoscrizioni.<br /><br /> Questa impostazione viene modificata quando si abilitano o disabilitano le funzionalità di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usando il facet Configurazione superficie di attacco per Reporting Services della gestione basata sui criteri. Per altre informazioni, vedere [Avviare e arrestare il servizio del server di report](../../reporting-services/report-server/start-and-stop-the-report-server-service.md).|N,S,P|  
|**IsAlertingService**|Il valore predefinito è **True**.|S|  
|**PollingInterval**|Consente di specificare l'intervallo, espresso in secondi, tra i polling della tabella eventi da parte del server di report. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è 10.|N,S,P|  
|**IsDataModelRefreshService**|Consente di specificare se il servizio elabora gli eventi di aggiornamento del modello di dati pianificato per i report Power BI. I valori validi includono **True** (impostazione predefinita) e **False**. Quando il valore è **False**, il server di report non esegue operazioni per l'aggiornamento del modello di dati pianificato.|N|  
|**WindowsServiceUseFileShareStorage**|Consente di specificare se archiviare nel file system i report memorizzati nella cache e gli snapshot temporanei creati dal servizio del server di report per la durata di una sessione utente. I valori validi sono **True** e **False** (impostazione predefinita).|N,S,P|  
|**MemorySafetyMargin**|Consente di specificare una percentuale di **WorkingSetMaximum** che definisce il limite tra scenari relativi a un utilizzo basso e medio della memoria. Il valore predefinito è 80. Per altre informazioni su **WorkingSetMaximum** e sulla configurazione della memoria disponibile, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**MemoryThreshold**|Consente di specificare una percentuale di **WorkingSetMaximum** che definisce il limite tra scenari relativi a un utilizzo medio e alto della memoria. Il valore predefinito è **90**. Questo valore deve essere maggiore del valore impostato per **MemorySafetyMargin**. Per altre informazioni, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**WorkingSetMaximum**|Consente di specificare una soglia di memoria oltre la quale non viene concessa alcuna allocazione di memoria alle applicazione del server di report che la richiedano. Per impostazione predefinita, il server di report imposta WorkingSetMaximum sulla quantità di memoria disponibile nel computer. Questo valore viene rilevato al momento dell'avvio del servizio. Questa impostazione non è presente nel file RSReportServer.config a meno che non venga aggiunta manualmente. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Questo valore è espresso in KB. Per altre informazioni, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N|  
|**WorkingSetMinimum**|Consente di specificare un limite inferiore per l'uso della memoria. Se la quantità di memoria complessiva usata è inferiore a questo limite, il server di report non rilascerà memoria. Per impostazione predefinita, il valore viene calcolato all'avvio del servizio e la richiesta di allocazione di memoria iniziale è per il 60% di **WorkingSetMaximum**. Questa impostazione non è presente nel file RSReportServer.config a meno che non venga aggiunta manualmente. Se si vuole personalizzare questo valore, è necessario aggiungere l'elemento **WorkingSetMaximum** al file RSReportServer.config. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Questo valore è espresso in KB. Per altre informazioni, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N| 
|**RecycleTime**|Consente di specificare il tempo di riciclo per il dominio dell'applicazione, espresso in minuti. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è 720.|N, S|  
|**MaxAppDomainUnloadTime**|Consente di specificare un intervallo durante il quale il dominio applicazione può essere scaricato durante un'operazione di riciclo. Se il riciclo non viene completato entro questo periodo di tempo, viene arrestata qualsiasi elaborazione nel dominio dell'applicazione. Per ulteriori informazioni, vedere [Application Domains for Report Server Applications](../../reporting-services/report-server/application-domains-for-report-server-applications.md).<br /><br /> Il valore è espresso in minuti. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è **30**.|N,S,P|  
|**MaxQueueThreads**|Consente di specificare il numero di thread utilizzati dal servizio Windows ReportServer per elaborare contemporaneamente sottoscrizioni e notifiche. I valori validi sono compresi nell'intervallo da 0 al numero intero massimo. Il valore predefinito è 0. Se si sceglie 0, il numero massimo di thread verrà determinato dal server di report. Se si specifica un valore integer, il valore specificato verrà utilizzato per impostare il limite superiore relativo al numero di thread che è possibile creare contemporaneamente. Per altre informazioni su come il servizio Windows ReportServer gestisce la memoria per i processi in esecuzione, vedere [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).|N,S,P|  
|**UrlRoot**|Viene usato dalle estensioni per il recapito del server di report per creare URL usati dai report recapitati in sottoscrizioni tramite posta elettronica o condivisione file. Viene usato anche dallo strumento di elaborazione dei report quando si risolvono espressioni tramite Globals!ReportServerUrl. È necessario che il valore sia un indirizzo URL valido per il server di report dal quale si accede al report pubblicato. Viene utilizzato dal server di report per generare URL per l'accesso offline o automatico. Tali URL vengono utilizzati nei report esportati e dalle estensioni per il recapito per creare un URL incluso in messaggi di recapito, ad esempio collegamenti in messaggi di posta elettronica. Il server di report determina gli URL nei report in base al comportamento seguente:<br /><br /> Quando **UrlRoot** non è specificato (valore predefinito) e sono presenti prenotazioni URL, il server di report determina automaticamente gli URL allo stesso modo in cui vengono generati gli URL per il metodo ListReportServerUrls. Viene utilizzato il primo URL restituito dal metodo ListReportServerUrls. In alternativa, se SecureConnectionLevel è maggiore di zero (0), viene usato il primo URL TLS.<br /><br /> Quando **UrlRoot** è impostato su un valore specifico, viene utilizzato il valore esplicito.<br /><br /> Quando **UrlRoot** non è specificato e non è configurata alcuna prenotazione URL, gli URL nei report visualizzati e nei collegamenti di posta elettronica non sono corretti.|N,S,P|  
|**UnattendedExecutionAccount**|Consente di specificare il nome utente, la password e il dominio usati dal server di report per l'esecuzione di un report. Questi valori sono crittografati. Per impostare questi valori, usare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o l'utilità **rsconfig** . Per altre informazioni, vedere [Configurare l'account di esecuzione automatica &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).<br /><br /> Per la modalità SharePoint, è possibile impostare l'account di esecuzione per un'applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilizzando Amministrazione centrale SharePoint. Per altre informazioni, vedere [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)|N,P|  
|**PolicyLevel**|Indica il file di configurazione dei criteri di sicurezza. Il valore valido è Rssrvrpolicy.config. Per ulteriori informazioni, vedere [Using Reporting Services Security Policy Files](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).|N,S,P|  
|**IsWebServiceEnabled**|Consente di specificare se il servizio Web ReportServer risponde a richieste di accesso SOAP e URL. Questo valore viene impostato quando si abilita o si disabilita il servizio utilizzando il facet Configurazione superficie di attacco per Reporting Services della gestione basata sui criteri.|N, S|  
|**IsReportManagerEnabled**|Questa impostazione è stata deprecata a partire dall'aggiornamento cumulativo 2 di SQL Server 2016 Reporting Services. Il portale Web verrà sempre abilitato.|N|  
|**FileShareStorageLocation**|Consente di specificare una cartella del file system per l'archiviazione degli snapshot temporanei. Benché sia possibile, non è consigliabile specificare un percorso UNC. Il valore predefinito è vuoto.<br /><br /> `<FileShareStorageLocation>`<br /><br /> `<Path>`<br /><br /> `</Path>`<br /><br /> `</FileShareStorageLocation>`|N,S,P|  
|**IsRdceEnabled**|Specifica se l'estensione RDCE (Report Definition Customization Extension) è abilitata. I valori validi sono **True** e **False**.|N,S,P|
|**IsDataModelRefreshService**|Consente di specificare se il server deve elaborare gli aggiornamenti dei report di Power BI. I valori validi sono **True** e **False**.|P|
|**MaxCatalogConnectionPoolSizePerProcess**|Consente di specificare la dimensione massima del pool di connessioni durante la connessione al catalogo del server. Il valore predefinito è 0. Se si sceglie 0, il server di report determina il numero massimo di connessioni per il processo reportingservices.exe, per gli altri processi è .[Sql Client Default](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.maxpoolsize).|P|  

  
##  <a name="ui-rsreportserverconfig-file"></a><a name="bkmk_UI"></a> UI (file RSReportServer.config)  
 **UI** specifica le impostazioni di configurazione valide per l'applicazione portale Web.  
  
 Nell'ultima colonna della tabella riportata di seguito viene indicato se l'impostazione si applica a un server di report in modalità nativa (N), a un server di report in modalità SharePoint (S) o a entrambi.  
  
|Impostazione|Descrizione|Mode|  
|-------------|-----------------|----------|  
|**ReportServerUrl**|Consente di specificare l'URL del server di report cui il portale Web si connette. Modificare questo valore solo se si configura il portale Web per connettersi a un server di report in un'altra istanza o in un computer remoto.|N, S|  
|**ReportBuilderTrustLevel**|Non modificare questo valore poiché non è configurabile. In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e versioni successive Generatore report viene eseguito solo in **FullTrust**. Per altre informazioni sulla modalità di attendibilità parziale non più disponibile, vedere [Funzionalità non più disponibili in SQL Server Reporting Services in SQL Server 2016](../../reporting-services/discontinued-functionality-to-sql-server-reporting-services-in-sql-server.md).|N, S|  
|**PageCountMode**|Solo per il portale Web, questa impostazione consente di specificare se il server di report calcola il conteggio delle pagine prima che sia stato eseguito il rendering del report o nel momento in cui viene visualizzato. I valori validi sono **Estimate** (impostazione predefinita) e **Actual**. Usare **Estimate** per calcolare le informazioni sul conteggio delle pagine nel momento in cui l'utente visualizza il report. Inizialmente il conteggio delle pagine è impostato su 2 (la pagina corrente più una pagina aggiuntiva), ma aumenta man mano che l'utente si sposta tra le pagine del report. Usare **Actual** per calcolare il conteggio delle pagine in anticipo prima che il report venga visualizzato. **Actual** viene fornito per compatibilità con le versioni precedenti. Si noti che se si imposta **PageCountMode** su **Actual**, per ottenere un conteggio delle pagine valido è necessario elaborare l'intero report, aumentando in questo modo il tempo di attesa prima che il report venga visualizzato.|N, S|  
  
##  <a name="extensions-rsreportserverconfig-file-native-mode"></a><a name="bkmk_extensions"></a> Extensions (file RSReportServer.config) per la modalità nativa  
 La sezione Extensions viene visualizzata nel file rsreportserver.config solo per server di report in **modalità nativa** . Le informazioni sulle estensioni per i server di report in modalità SharePoint sono archiviate nel database di configurazione di SharePoint e vengono configurate per l'applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 **Extensions** specifica le impostazioni di configurazione per i moduli estendibili riportati di seguito di un'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Estensioni per il recapito  
  
-   Estensioni per DeliveryUI  
  
-   Estensioni per il rendering  
  
-   Estensioni per l'elaborazione dati  
  
-   Estensioni delle query semantiche (solo per uso interno)  
  
-   Estensioni per la generazione del modello (solo per uso interno)  
  
-   Estensioni di sicurezza  
  
-   Estensioni di autenticazione  
  
-   Estensioni per l'elaborazione di eventi (solo per uso interno)  
  
-   Report Definition Customization Extension  
  
 Alcune di queste estensioni sono destinate esclusivamente all'uso interno del server di report. Le impostazioni di configurazione per le estensioni di solo uso interno non sono documentate. Nelle sezioni seguenti vengono descritte le impostazioni di configurazione per le estensioni predefinite. Se si utilizza un server di report che dispone di estensioni personalizzate, i file di configurazione potrebbero contenere impostazioni non descritte di seguito. In questa sezione le estensioni vengono elencate nell'ordine in cui appaiono. Le impostazioni utilizzate ripetutamente per più istanze dello stesso tipo di estensione vengono descritte solo una volta.  
  
###  <a name="delivery-extensions-general-configuration"></a><a name="bkmk_extensionsgeneral"></a> Configurazione generale delle estensioni per il recapito  
 Consente di specificare le estensioni per il recapito predefinite ed eventualmente quelle personalizzate utilizzate per recapitare report tramite sottoscrizione. Nel file RSReportServer.config sono incluse impostazioni dell'applicazione per quattro estensioni per il recapito:  
  
1.  Posta elettronica del server di report.  
  
2.  Recapito tramite condivisione file.  
  
3.  Raccolta documenti del server di report utilizzata per un server di report in esecuzione in modalità integrata SharePoint.  
  
4.  Provider recapito Null utilizzato per precaricare la cache del report.  
  
 Per altre informazioni sulle estensioni per il recapito, vedere [Sottoscrizioni e recapito &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)  
  
 A tutte le estensioni per il recapito sono associate le impostazioni **Extension Name**, **MaxRetries**, **SecondsBeforeRetry** e **Configuration**. Vengono descritte innanzitutto queste impostazioni condivise, mentre le impostazioni specifiche delle estensioni vengono descritte in una tabella successiva.  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**Extension Name**|Consente di specificare un nome descrittivo e un assembly dell'estensione per il recapito. Non modificare questo valore.|  
|**MaxRetries**|Consente di specificare il numero massimo di tentativi di recapito da parte di un server di report se il primo tentativo non viene eseguito in modo corretto. Il valore predefinito è 3.|  
|**SecondsBeforeRetry**|Consente di specificare l'intervallo di tempo (espresso in secondi) tra due tentativi successivi. Il valore predefinito è 900.|  
|**Configuration**|Contiene le impostazioni di configurazione specifiche per ogni estensione per il recapito.|  
  
####  <a name="file-share-delivery-extension-configuration-settings"></a><a name="bkmk_fileshare_extension"></a> Impostazioni di configurazione dell'estensione per il recapito tramite la condivisione file  
 Il recapito tramite la condivisione file consente di inviare un report esportato in un formato del file dell'applicazione a una cartella condivisa sulla rete. Per ulteriori informazioni, vedere [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**ExcludedRenderFormats**, **RenderingExtension**|Queste impostazioni vengono utilizzate per escludere intenzionalmente formati di esportazione che non funzionano in modo corretto con il recapito tramite la condivisione file. Questi formati vengono utilizzati in genere per la creazione interattiva di report, la visualizzazione in anteprima o il precaricamento della cache del report e non producono file dell'applicazione facilmente visualizzabili da un'applicazione desktop.<br /><br /> HTMLOWC<br /><br /> RGDI<br /><br /> Null|  
  
####  <a name="report-server-e-mail-extension-configuration-settings"></a><a name="bkmk_email_extension"></a> Impostazioni di configurazione dell'estensione per la posta elettronica del server di report  
 La posta elettronica del server di report utilizza un dispositivo di rete di SMTP per inviare report agli indirizzi di posta elettronica. Prima che sia possibile utilizzare questa estensione per il recapito, è necessario configurarla. Per altre informazioni, vedere [Recapito tramite posta elettronica in Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md).  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**SMTPServer**|Consente di specificare un valore stringa che indica l'indirizzo di un server SMTP remoto o di un server d'inoltro. Questo valore è obbligatorio per il servizio SMTP remoto. Può essere un indirizzo IP, un nome UNC di un computer nella Intranet aziendale o un nome di dominio completo.|  
|**SMTPServerPort**|Consente di specificare un valore integer che indica la porta cui il servizio SMTP invia la posta in uscita. Di norma per l'invio della posta elettronica viene utilizzata la porta 25.|  
|**SMTPAccountName**|Contiene un valore stringa che assegna il nome dell'account di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Outlook Express. Se il server SMTP è configurato per utilizzare questo valore, è possibile impostarlo, altrimenti è possibile lasciare vuota l'impostazione. Usare **From** per specificare un account di posta elettronica per l'invio dei report.|  
|**SMTPConnectionTimeout**|Consente di specificare un valore integer che indica il numero di secondi di attesa per una connessione socket valida con il servizio SMTP, trascorsi i quali si verifica il timeout. Il valore predefinito è 30 secondi, ma viene ignorato se l'elemento **SendUsing** è impostato su 2.|  
|**SMTPServerPickupDirectory**|Consente di specificare un valore stringa che indica la directory di prelievo per il servizio SMTP locale. Questo valore deve essere il percorso completo di una cartella locale, ad esempio d:\rs-emails.|  
|**SMTPUseSSL**|Consente di specificare un valore booleano che può essere impostato per usare TLS (Secure Sockets Layer), protocollo precedentemente noto come Secure Sockets Layer (SSL), quando si invia un messaggio SMTP tramite la rete. Il valore predefinito è 0 (o false). Questa impostazione può essere utilizzata se l'elemento **SendUsing** è impostato su 2.|  
|**SendUsing**|Consente di specificare il metodo da utilizzare per l'invio dei messaggi. I valori validi sono:<br /><br /> 1=il messaggio viene inviato dalla directory di prelievo del servizio SMTP locale.<br /><br /> 2=il messaggio viene inviato dal servizio SMTP della rete.|  
|**SMTPAuthenticate**|Consente di specificare un valore integer che indica il tipo di autenticazione da utilizzare per l'invio di messaggi a un servizio SMTP tramite una connessione TCP/IP. I valori validi sono:<br /><br /> 0=nessuna autenticazione.<br /><br /> 1= (non supportato).<br /><br /> 2= autenticazione NTLM (NT LanMan). Per la connessione al server SMTP di rete viene utilizzato il contesto di sicurezza del servizio Windows ReportServer.|  
|**From**|Consente di specificare l'indirizzo di posta elettronica da cui vengono inviati i report, nel formato *abc@host.xyz* . L'indirizzo viene visualizzato nella riga **Da** di un messaggio di posta elettronica in uscita. Questo valore è obbligatorio se viene utilizzato un server SMTP remoto. Deve essere un account di posta elettronica valido con l'autorizzazione all'invio di posta.|  
|**EmbeddedRenderFormats, RenderingExtension**|Consente di specificare il formato di rendering utilizzato per incapsulare un report nel corpo di un messaggio di posta elettronica. Le immagini presenti nel report vengono incorporate all'interno del report. I valori validi sono MHTML e HTML4.0.|  
|**PrivilegedUserRenderFormats**|Consente di specificare i formati di rendering che un utente può selezionare per una sottoscrizione, purché le sottoscrizioni siano state abilitate tramite l'attività "Gestione di tutte le sottoscrizioni". Se questo valore non viene impostato, risultano disponibili tutti i formati di rendering che non siano stati esclusi in modo esplicito.|  
|**ExcludedRenderFormats, RenderingExtension**|Consente di escludere i formati non adatti per un'estensione per il recapito specificata. Non è possibile escludere più istanze della stessa estensione per il rendering. In questo caso, si verificherà un errore durante la lettura del file di configurazione da parte del server di report. Per impostazione predefinita, le estensioni seguenti sono escluse per il recapito tramite posta elettronica:<br /><br /> HTMLOWC<br /><br /> Null<br /><br /> RGDI|  
|**SendEmailToUserAlias**|Questo valore funziona con **DefaultHostName**.<br /><br /> Se **SendEmailToUserAlias** è impostato su **True**, gli utenti che definiscono le singole sottoscrizioni vengono specificati automaticamente come destinatari del report. Il campo **A** risulta nascosto. Se questo valore è impostato su **False**, il campo **A** è visibile. Impostare questo valore su **True** se si vuole esercitare il massimo controllo sulla distribuzione dei report. I valori validi sono i seguenti:<br /><br /> **True**: viene usato l'indirizzo di posta elettronica dell'utente che crea la sottoscrizione. Si tratta del valore predefinito.<br /><br /> **False**: è possibile specificare qualsiasi indirizzo di posta elettronica.|  
|**DefaultHostName**|Questo valore interagisce con **SendEmailToUserAlias**.<br /><br /> Consente di specificare un valore stringa che indica il nome host da aggiungere all'alias dell'utente quando **SendEmailToUserAlias** è impostata su true. Questo valore può essere un nome DNS (Domain Name System) o un indirizzo IP.|  
|**PermittedHosts**|Consente di limitare la distribuzione dei report specificando in modo esplicito quali host possono ricevere i recapiti tramite posta elettronica. In **PermittedHosts** ogni host è specificato come elemento **HostName** , dove il valore è un indirizzo IP o un nome DNS.<br /><br /> Sono destinatari validi solo gli account definiti per l'host. Se è stato specificato **DefaultHostName**, è importante includere quell'host come elemento **HostName** di **PermittedHosts**. Il valore deve corrispondere a uno o più nomi DNS o indirizzi IP. Per impostazione predefinita, questo valore non è impostato. Se il valore non è impostato, non vi sono restrizioni in merito a chi può ricevere i report tramite posta elettronica.|  
  
####  <a name="report-server-sharepoint-document-library-extension-configuration"></a><a name="bkmk_documentlibrary_extension"></a> Configurazione dell'estensione per la raccolta documenti SharePoint del server di report  
 La raccolta documenti del server di report consente di inviare un report esportato in un formato del file dell'applicazione a una raccolta documenti. Questa estensione per il recapito può essere utilizzata solo da un server di report configurato per essere eseguito in modalità integrata SharePoint. Per ulteriori informazioni, vedere [SharePoint Library Delivery in Reporting Services](../../reporting-services/subscriptions/sharepoint-library-delivery-in-reporting-services.md).  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**ExcludedRenderFormats, RenderingExtension**|Queste impostazioni vengono utilizzate per escludere intenzionalmente formati di esportazione che non funzionano in modo corretto con il recapito tramite la raccolta documenti. Le estensioni per il recapito HTMLOWC, RGDI e Null sono escluse. Questi formati vengono utilizzati in genere per la creazione interattiva di report, la visualizzazione in anteprima o il precaricamento della cache del report e non producono file dell'applicazione facilmente visualizzabili da un'applicazione desktop.|  
  
####  <a name="null-delivery-extension-configuration"></a><a name="bkmk_null_extension"></a> Configurazione dell'estensione per il recapito NULL  
 Il provider di recapito NULL viene utilizzato per precaricare la cache con report pre-generati per utenti singoli. Per questa estensione per il recapito non esistono impostazioni di configurazione. Per altre informazioni, vedere [Memorizzazione dei report nella cache &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md).  
  
###  <a name="delivery-ui-extensions-general-configuration"></a><a name="bkmk_ui"></a> Configurazione generale delle estensioni per l'interfaccia utente per il recapito  
 Specifica estensioni per il recapito che contengono un componente dell'interfaccia utente visualizzato nelle pagine di definizione della sottoscrizione usate nella specifica di sottoscrizioni singole nel portale Web. Se si crea e si distribuisce un'estensione per il recapito personalizzata che include opzioni definite dall'utente e si desidera usare il portale Web, è necessario registrare tale estensione in questa sezione. Per impostazione predefinita, sono presenti impostazioni di configurazione per la posta elettronica e per la condivisione file del server di report. Le estensioni per il recapito utilizzate solo nelle sottoscrizioni guidate dai dati o nelle pagine dell'applicazione SharePoint non dispongono di impostazioni descritte in questa sezione.  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**DefaultDeliveryExtension**|Questa impostazione consente di determinare se l'estensione per il recapito viene visualizzata per prima nell'elenco dei tipi di recapito nella pagina di definizione della sottoscrizione. Solo un estensione per il recapito può contenere questa impostazione. I valori validi includono **True** e **False**. Quando questo valore è impostato su **True**, tale estensione è l'opzione predefinita.|  
|**Configuration**|Consente di specificare le opzioni di configurazione per un'estensione per il recapito. È possibile impostare un formato di rendering predefinito per ogni estensione per il recapito. I valori validi sono i nomi delle estensioni per il rendering indicati nella sezione relativa al rendering del file rsreportserver.config.|  
|**DefaultRenderingExtension**|Consente di specificare se un'estensione per il recapito è quella predefinita. L'estensione per il recapito predefinita è la posta elettronica del server di report. I valori validi includono **True** e **False**. Se il valore **True** viene specificato per più estensioni, la prima estensione verrà considerata quella predefinita.|  
  
###  <a name="rendering-extensions-general-configuration"></a><a name="bkmk_rendering"></a> Configurazione generale delle estensioni per il rendering  
 Consente di specificare le estensioni per il rendering predefinite ed eventualmente personalizzate utilizzate per la presentazione dei report.  
  
 Non modificare questa sezione a meno che non si distribuisca un'estensione per il rendering personalizzata. Per ulteriori informazioni, vedere [Implementing a Rendering Extension](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md).  
  
 Tra le estensioni per il rendering predefinite sono incluse le seguenti:  
  
-   XML  
  
-   Null  
  
-   CSV  
  
-   PDF  
  
-   RGDI  
  
-   HTML4.0  
  
-   MHTML  
  
-   EXCEL  
  
-   RPL  
  
-   IMAGE  
  
 A partire dalla versione [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , nei rendering MHTML e HTML 4.0 è inclusa, per impostazione predefinita, la seguente impostazione relativa alle informazioni sui dispositivi per controllare il comportamento di ridimensionamento delle visualizzazioni dei dati.  
  
```  
<DeviceInfo><DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing></DeviceInfo>  
```  
  
 Per ulteriori informazioni sulle impostazioni DeviceInfo, vedere quanto riportato di seguito:  
  
-   [Impostazioni relative alle informazioni sul dispositivo MHTML](../../reporting-services/mhtml-device-information-settings.md)  
  
-   [Impostazioni relative alle informazioni sul dispositivo HTML](../../reporting-services/html-device-information-settings.md)  
  
-   [Impostazioni relative alle informazioni sul dispositivo per le estensioni per il rendering &#40;Reporting Services&#41;](../../reporting-services/device-information-settings-for-rendering-extensions-reporting-services.md)  
  
 Per informazioni sugli attributi per l'elemento figlio **\<Extension>** in **\<Render>** , vedere quanto riportato di seguito:  
  
-   [Personalizzare i parametri di estensione per il rendering in RSReportServer.config.](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)  
  
-   [Distribuzione di un'estensione per il rendering](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
 Non modificare questa sezione a meno che non si distribuisca un'estensione per il rendering personalizzata. Per ulteriori informazioni, vedere [Implementing a Rendering Extension](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md).  
  
###  <a name="data-extensions-general-configuration"></a><a name="bkmk_data"></a> Configurazione generale delle estensioni per i dati  
 Consente di specificare le estensioni per l'elaborazione dati predefinite ed eventualmente personalizzate utilizzate per l'elaborazione delle query. Tra le estensioni per l'elaborazione dati predefinite sono incluse le seguenti:  
  
-   SQL  
  
-   SQLAZURE  
  
-   SQLPDW  
  
-   OLEDB  
  
-   OLEDB-MD  
  
-   ORACLE  
  
-   ODBC  
  
-   XML  
  
-   SHAREPOINTLIST  
  
-   SAPBW  
  
-   ESSBASE  
  
-   TERADATA  
  
 Non modificare questa sezione a meno che non si aggiungano estensioni per l'elaborazione dati personalizzate. Per ulteriori informazioni, vedere [Implementing a Data Processing Extension](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
###  <a name="semantic-query-extensions-general-configuration"></a><a name="bkmk_semantic"></a> Configurazione generale delle estensioni per le query semantiche  
 Consente di specificare l'estensione per l'elaborazione della query semantica utilizzata per elaborare i modelli di report. Le estensioni per l'elaborazione delle query semantiche disponibili in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] forniscono supporto per dati relazionali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Oracle e dati multidimensionali di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Non modificare questa sezione. L'elaborazione della query non è estendibile.  
  
###  <a name="model-generation-configuration"></a><a name="bkmk_model"></a> Configurazione relativa alla generazione del modello  
 Consente di specificare un'estensione di generazione del modello utilizzata per creare modelli di report da un'origine dati condivisa già pubblicata su un server di report. È possibile generare modelli per data relazionali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Oracle e origini dati multidimensionali di [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Non modificare questa sezione. La generazione del modello non è estendibile.  
  
###  <a name="security-extension-configuration"></a><a name="bkmk_security"></a> Configurazione dell'estensione di sicurezza  
 Consente di specificare il componente di autorizzazione utilizzato da [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Questo componente viene usato dall'estensione di autenticazione registrata nell'elemento **Authentication** del file RSReportServer.config. Non modificare questa sezione a meno che non si implementi un'estensione dell'autenticazione personalizzata. Per ulteriori informazioni sull'aggiunta delle funzionalità di sicurezza personalizzate, vedere [Implementing a Security Extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md). Per ulteriori informazioni sull'autorizzazione, vedere [Authorization in Reporting Services](../../reporting-services/extensions/security-extension/authorization-in-reporting-services.md).  
  
###  <a name="authentication-extension-configuration"></a><a name="bkmk_authentication"></a> Configurazione dell'estensione di autenticazione  
 Consente di specificare le estensioni predefinite e personalizzate dell'autenticazione utilizzate dal server di report. L'estensione predefinita si basa sull'autenticazione di Windows. Non modificare questa sezione a meno che non si implementi un'estensione dell'autenticazione personalizzata. Per altre informazioni sull'autenticazione in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vedere [Autenticazione in Reporting Services](../../reporting-services/extensions/security-extension/authentication-in-reporting-services.md) e [Autenticazione con il server di report](../../reporting-services/security/authentication-with-the-report-server.md). Per ulteriori informazioni sull'aggiunta delle funzionalità di sicurezza personalizzate, vedere [Implementing a Security Extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).  
  
###  <a name="event-processing"></a><a name="bkmk_eventprocessing"></a> Elaborazione di eventi  
 Consente di specificare i gestori eventi predefiniti. Non modificare questa sezione. Questa sezione non è estendibile.  
  
###  <a name="report-definition-customization"></a><a name="bkmk_reportdefinition"></a> Personalizzazione della definizione del report  
 Consente di specificare il nome e il tipo di un'estensione personalizzata che modifica una definizione di report.  
  
###  <a name="rdlsandboxing"></a><a name="bkmk_rdlsandboxing"></a> RDLSandboxing  
 Specifica una modalità RDL (Report Definition Language) che consente di rilevare e limitare l'utilizzo di tipi specifici di risorse del report in base a singoli titolari in uno scenario in cui più titolari condividono una sola Web farm di server di report. Per altre informazioni, vedere [Enable and Disable RDL Sandboxing](../../reporting-services/report-server-sharepoint/enable-and-disable-rdl-sandboxing.md).  
  
##  <a name="maptileserverconfiguration-rsreportserverconfig-file"></a><a name="bkmk_MapTileServer"></a> MapTileServerConfiguration (RSReportServer.config file)  
 **MapTileServerConfiguration** definisce le impostazioni di configurazione per i servizi Web di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing Maps che forniscono uno sfondo a sezioni per un elemento del report della mappa in un report pubblicato in un server di report. Tutti gli elementi figlio sono obbligatori.  
  
|Impostazione|Descrizione|  
|-------------|-----------------|  
|**MaxConnections**|Specifica il numero massimo di connessioni ai servizi Web di Bing Maps.|  
|**Timeout**|Specifica il timeout in secondi per l'attesa di una risposta dai servizi Web di Bing Maps.|  
|**AppID**|Specifica l'identificatore dell'applicazione (AppID) da utilizzare per i servizi Web di Bing Maps. **(Default)** specifica l'AppID predefinito di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .<br /><br /> Per altre informazioni sull'utilizzo delle tessere mappa di Bing nel report, vedere [Ulteriori condizioni di utilizzo](https://go.microsoft.com/fwlink/?LinkId=151371).<br /><br /> Non modificare questo valore a meno che non sia necessario specificare un AppID personalizzato per il contratto di licenza di Bing Maps. Quando si modifica l'AppID, non è necessario riavviare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per rendere effettiva la modifica.|  
|**CacheLevel**|Specifica un valore dell'enumerazione HttpRequestCacheLevel di System.Net.Cache. Il valore predefinito è **Default**. Per ulteriori informazioni, vedere la pagina relativa all' [enumerazione HttpRequestCacheLevel](/dotnet/api/system.net.cache.httprequestcachelevel).|  
  
##  <a name="default-configuration-file-for-a-native-mode-report-server"></a><a name="bkmk_nativedefaultfile"></a> File di configurazione predefinito per un server di report in modalità nativa  
 Per impostazione predefinita, il file rsreportserver.config è installato nel percorso seguente:  
  
 **C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer**  
  
```  
<Configuration>
    <Dsn>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAR58DMGebHUeMvyR6HR04kQQAAAAiAAAAUgBlAHAAbwBy
AHQAaQBuAGcAIABTAGUAcgB2AGUAcgAAAANmAADAAAAAEAAAADczfLRgZ4GF44iBHkLrKY4AAAAA
BIAAAKAAAAAQAAAAJ9wQOmDNauH+LS30rboJ2OAAAAAp0kiFFBrc3r3ypKaldZJtjCORX9LTZRzt
0/JCSVIZc4GXx0peGKqd+f85UyrY/KOyUSHogOC/XoBp9Ppxv6ITbdunsS/LXEcMUBVqEdQD4ylh
x6K1NTC/u8hl9v0MgK+xMQKaiV7BuNYbgGgkaViABcNH0xVzcc5rMTHUkrABbGDFGKyAFniGQ1qu
/rqHibNNyvYbP/2uiqvgC0tQl6u8VkVbXpWrkvO+bFCqxlaJlCoDc2f3rIO321SZEvoFbsYNgPLd
+mIAkSCnH3Z3gm/bI8bqVkFaHblKyQuSfFsi6RQAAACb87b26dV0GjHmMJnE0Tk8CzNmhg==</Dsn>
    <ConnectionType>Default</ConnectionType>
    <LogonUser></LogonUser>
    <LogonDomain></LogonDomain>
    <LogonCred></LogonCred>
    <InstanceId>MSRS13.MSSQLSERVER</InstanceId>
    <InstallationID>{cd920604-a5c7-4554-b2a0-aadc04312fe5}</InstallationID>
    <Add Key="SecureConnectionLevel" Value="0"/>
    <Add Key="DisableSecureFormsAuthenticationCookie" Value="false"/>
    <Add Key="CleanupCycleMinutes" Value="10"/>
    <Add Key="MaxActiveReqForOneUser" Value="20"/>
    <Add Key="DatabaseQueryTimeout" Value="120"/>
    <Add Key="RunningRequestsScavengerCycle" Value="60"/>
    <Add Key="RunningRequestsDbCycle" Value="60"/>
    <Add Key="RunningRequestsAge" Value="30"/>
    <Add Key="MaxScheduleWait" Value="5"/>
    <Add Key="DisplayErrorLink" Value="true"/>
    <Add Key="WebServiceUseFileShareStorage" Value="false"/>
    <!--  <Add Key="ProcessTimeout" Value="150" /> -->
    <!--  <Add Key="ProcessTimeoutGcExtension" Value="30" /> -->
    <!--  <Add Key="WatsonFlags" Value="0x0430" /> full dump-->
    <!--  <Add Key="WatsonFlags" Value="0x0428" /> minidump -->
    <!--  <Add Key="WatsonFlags" Value="0x0002" /> no dump-->
    <Add Key="WatsonFlags" Value="0x0428"/>
    <Add Key="WatsonDumpOnExceptions" Value="Microsoft.ReportingServices.Diagnostics.Utilities.InternalCatalogException,Microsoft.ReportingServices.Modeling.InternalModelingException,Microsoft.ReportingServices.ReportProcessing.UnhandledReportRenderingException"/>
    <Add Key="WatsonDumpExcludeIfContainsExceptions" Value="System.Threading.ThreadAbortException,System.Web.UI.ViewStateException,System.OutOfMemoryException,System.Web.HttpException,System.IO.IOException,System.IO.FileLoadException,Microsoft.SharePoint.SPException,Microsoft.ReportingServices.WmiProvider.WMIProviderException,System.AppDomainUnloadedException"/>
    <URLReservations>
        <Application>
            <Name>ReportServerWebService</Name>
            <VirtualDirectory>ReportServer</VirtualDirectory>
            <URLs>
                <URL>
                    <UrlString>https://+:80</UrlString>
                    <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>
                    <AccountName>NT SERVICE\ReportServer</AccountName>
                </URL>
            </URLs>
        </Application>
        <Application>
            <Name>ReportServerWebApp</Name>
            <VirtualDirectory>Reports</VirtualDirectory>
            <URLs>
                <URL>
                    <UrlString>https://+:80</UrlString>
                    <AccountSid>S-1-5-80-2885764129-887777008-271615777-1616004480-2722851051</AccountSid>
                    <AccountName>NT SERVICE\ReportServer</AccountName>
                </URL>
            </URLs>
        </Application>
    </URLReservations>
    <Authentication>
        <AuthenticationTypes>
            <RSWindowsNTLM/>
        </AuthenticationTypes>
        <RSWindowsExtendedProtectionLevel>Off</RSWindowsExtendedProtectionLevel>
        <RSWindowsExtendedProtectionScenario>Proxy</RSWindowsExtendedProtectionScenario>
        <EnableAuthPersistence>true</EnableAuthPersistence>
    </Authentication>
    <Service>
        <IsSchedulingService>True</IsSchedulingService>
        <IsNotificationService>True</IsNotificationService>
        <IsEventService>True</IsEventService>
        <PollingInterval>10</PollingInterval>
        <WindowsServiceUseFileShareStorage>False</WindowsServiceUseFileShareStorage>
        <MemorySafetyMargin>80</MemorySafetyMargin>
        <MemoryThreshold>90</MemoryThreshold>
        <RecycleTime>720</RecycleTime>
        <MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>
        <MaxQueueThreads>0</MaxQueueThreads>
        <UrlRoot>
        </UrlRoot>
        <UnattendedExecutionAccount>
            <UserName></UserName>
            <Password></Password>
            <Domain></Domain>
        </UnattendedExecutionAccount>
        <PolicyLevel>rssrvpolicy.config</PolicyLevel>
        <IsWebServiceEnabled>True</IsWebServiceEnabled>
        <IsReportManagerEnabled>True</IsReportManagerEnabled>
        <FileShareStorageLocation>
            <Path>
            </Path>
        </FileShareStorageLocation>
        <DefaultFileShareAccount>
            <Domain></Domain>
            <UserName></UserName>
            <Password></Password>
        </DefaultFileShareAccount>
    </Service>
    <UI>
        <ReportServerUrl>
        </ReportServerUrl>
        <PageCountMode>Estimate</PageCountMode>
    </UI>
    <Extensions>
        <Delivery>
            <Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareProvider,ReportingServicesFileShareDeliveryProvider">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <FileShareConfiguration>
                        <ExcludedRenderFormats>
                            <RenderingExtension>HTMLOWC</RenderingExtension>
                            <RenderingExtension>NULL</RenderingExtension>
                            <RenderingExtension>RGDI</RenderingExtension>
                        </ExcludedRenderFormats>
                    </FileShareConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailProvider,ReportingServicesEmailDeliveryProvider">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <RSEmailDPConfiguration>
                        <SMTPServer></SMTPServer>
                        <SMTPServerPort>
                        </SMTPServerPort>
                        <SMTPAccountName>
                        </SMTPAccountName>
                        <SMTPConnectionTimeout>
                        </SMTPConnectionTimeout>
                        <SMTPServerPickupDirectory>
                        </SMTPServerPickupDirectory>
                        <SMTPUseSSL>False</SMTPUseSSL>
                        <SendUsing>2</SendUsing>
                        <SMTPAuthenticate>0</SMTPAuthenticate>
                        <SendUserName></SendUserName>
                        <SendPassword></SendPassword>
                        <From></From>
                        <EmbeddedRenderFormats>
                            <RenderingExtension>MHTML</RenderingExtension>
                        </EmbeddedRenderFormats>
                        <PrivilegedUserRenderFormats>
                        </PrivilegedUserRenderFormats>
                        <ExcludedRenderFormats>
                            <RenderingExtension>HTMLOWC</RenderingExtension>
                            <RenderingExtension>NULL</RenderingExtension>
                            <RenderingExtension>RGDI</RenderingExtension>
                        </ExcludedRenderFormats>
                        <SendEmailToUserAlias>True</SendEmailToUserAlias>
                        <DefaultHostName>
                        </DefaultHostName>
                        <PermittedHosts>
                        </PermittedHosts>
                    </RSEmailDPConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="Report Server DocumentLibrary" Type="Microsoft.ReportingServices.SharePoint.SharePointDeliveryExtension.DocumentLibraryProvider,ReportingServicesSharePointDeliveryExtension">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <DocumentLibraryConfiguration>
                        <ExcludedRenderFormats>
                            <RenderingExtension>HTMLOWC</RenderingExtension>
                            <RenderingExtension>NULL</RenderingExtension>
                            <RenderingExtension>RGDI</RenderingExtension>
                        </ExcludedRenderFormats>
                    </DocumentLibraryConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="NULL" Type="Microsoft.ReportingServices.NullDeliveryProvider.NullProvider,ReportingServicesNullDeliveryProvider"/>
            <Extension Name="Report Server PowerBI" Type="Microsoft.ReportingServices.PowerBIDeliveryProvider.PowerBIDeliveryProvider,ReportingServicesPowerBIDeliveryProvider">
                <MaxRetries>3</MaxRetries>
                <SecondsBeforeRetry>900</SecondsBeforeRetry>
                <Configuration>
                    <PowerBIDeliveryConfiguration>
                    </PowerBIDeliveryConfiguration>
                </Configuration>
            </Extension>
        </Delivery>
        <DeliveryUI>
            <Extension Name="Report Server Email" Type="Microsoft.ReportingServices.EmailDeliveryProvider.EmailDeliveryProviderControl,ReportingServicesEmailDeliveryProvider">
                <DefaultDeliveryExtension>True</DefaultDeliveryExtension>
                <Configuration>
                    <RSEmailDPConfiguration>
                        <DefaultRenderingExtension>MHTML</DefaultRenderingExtension>
                    </RSEmailDPConfiguration>
                </Configuration>
            </Extension>
            <Extension Name="Report Server FileShare" Type="Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareUIControl,ReportingServicesFileShareDeliveryProvider"/>
            <Extension Name="Report Server PowerBI" Type="Microsoft.ReportingServices.PowerBIDeliveryProvider.PowerBIDeliveryUIControl,ReportingServicesPowerBIDeliveryProvider"/>
        </DeliveryUI>
        <Render>
            <Extension Name="WORDOPENXML" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordOpenXmlRenderer.WordOpenXmlDocumentRenderer,Microsoft.ReportingServices.WordRendering"/>
            <Extension Name="WORD" Type="Microsoft.ReportingServices.Rendering.WordRenderer.WordDocumentRenderer,Microsoft.ReportingServices.WordRendering" Visible="false"/>
            <Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>
            <Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>
            <Extension Name="PPTX" Type="Microsoft.ReportingServices.Rendering.PowerPointRendering.PptxRenderingExtension,Microsoft.ReportingServices.PowerPointRendering"/>
            <Extension Name="PDF" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.PDFRenderer,Microsoft.ReportingServices.ImageRendering"/>
            <Extension Name="IMAGE" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.ImageRenderer,Microsoft.ReportingServices.ImageRendering"/>
            <Extension Name="MHTML" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.MHtmlRenderingExtension,Microsoft.ReportingServices.HtmlRendering">
                <Configuration>
                    <DeviceInfo>
                        <DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
                    </DeviceInfo>
                </Configuration>
            </Extension>
            <Extension Name="CSV" Type="Microsoft.ReportingServices.Rendering.DataRenderer.CsvReport,Microsoft.ReportingServices.DataRendering"/>
            <Extension Name="XML" Type="Microsoft.ReportingServices.Rendering.DataRenderer.XmlDataReport,Microsoft.ReportingServices.DataRendering"/>
            <Extension Name="ATOM" Type="Microsoft.ReportingServices.Rendering.DataRenderer.AtomDataReport,Microsoft.ReportingServices.DataRendering"/>
            <Extension Name="NULL" Type="Microsoft.ReportingServices.Rendering.NullRenderer.NullReport,Microsoft.ReportingServices.NullRendering" Visible="false"/>
            <Extension Name="RGDI" Type="Microsoft.ReportingServices.Rendering.ImageRenderer.RGDIRenderer,Microsoft.ReportingServices.ImageRendering" Visible="false"/>
            <Extension Name="HTML4.0" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.Html40RenderingExtension,Microsoft.ReportingServices.HtmlRendering" Visible="false">
                <Configuration>
                    <DeviceInfo>
                        <DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
                    </DeviceInfo>
                </Configuration>
            </Extension>
            <Extension Name="HTML5" Type="Microsoft.ReportingServices.Rendering.HtmlRenderer.Html5RenderingExtension,Microsoft.ReportingServices.HtmlRendering" Visible="false">
                <Configuration>
                    <DeviceInfo>
                        <DataVisualizationFitSizing>Approximate</DataVisualizationFitSizing>
                    </DeviceInfo>
                </Configuration>
            </Extension>
            <Extension Name="RPL" Type="Microsoft.ReportingServices.Rendering.RPLRendering.RPLRenderer,Microsoft.ReportingServices.RPLRendering" Visible="false" LogAllExecutionRequests="false"/>
        </Render>
        <!--
        For the SQLPDW extension to work, install the SQL Server PDW Client Tools on the report server.
        NOTE: The SQLPDW extension is deprecated. It supports old versions of SQL Server Parallel Data Warehouse (PDW).        
        To connect to Analytics Platform System, use the SQL (SQL Server) extension.        
        For the ORACLE extension to work, install the Oracle Data Provider for NET (ODP.NET) on the report server.
        For TERADATA extension to work, install the .NET Provider for Teradata on the report server.
      -->
        <Data>
            <Extension Name="SQL" Type="Microsoft.ReportingServices.DataExtensions.SqlConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.DataExtensions.SqlAzureConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="SQLPDW" Type="Microsoft.ReportingServices.DataExtensions.SqlDwConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="OLEDB-MD" Type="Microsoft.ReportingServices.DataExtensions.AdoMdConnection,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="SHAREPOINTLIST" Type="Microsoft.ReportingServices.DataExtensions.SharePointList.SPListConnection,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="ORACLE" Type="Microsoft.ReportingServices.DataExtensions.OracleClientConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="ESSBASE" Type="Microsoft.ReportingServices.DataExtensions.Essbase.EssbaseConnection,Microsoft.ReportingServices.DataExtensions.Essbase"/>
            <Extension Name="SAPBW" Type="Microsoft.ReportingServices.DataExtensions.SapBw.SapBwConnection,Microsoft.ReportingServices.DataExtensions.SapBw"/>
            <Extension Name="TERADATA" Type="Microsoft.ReportingServices.DataExtensions.TeradataConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="OLEDB" Type="Microsoft.ReportingServices.DataExtensions.OleDbConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="ODBC" Type="Microsoft.ReportingServices.DataExtensions.OdbcConnectionWrapper,Microsoft.ReportingServices.DataExtensions"/>
            <Extension Name="XML" Type="Microsoft.ReportingServices.DataExtensions.XmlDPConnection,Microsoft.ReportingServices.DataExtensions"/>
        </Data>
        <SemanticQuery>
            <Extension Name="SQL" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MSSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>False</EnableMathOpCasting>
                </Configuration>
            </Extension>
            <Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MSSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>False</EnableMathOpCasting>
                </Configuration>
            </Extension>
            <Extension Name="SQLPDW" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQLADW.MSSqlAdwSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>False</EnableMathOpCasting>
                </Configuration>
            </Extension>
            <Extension Name="ORACLE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Oracle.OraSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>True</EnableMathOpCasting>
                    <DisableNO_MERGEInLeftOuters>False</DisableNO_MERGEInLeftOuters>
                    <EnableUnistr>False</EnableUnistr>
                    <DisableTSTruncation>False</DisableTSTruncation>
                </Configuration>
            </Extension>
            <Extension Name="TERADATA" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Teradata.TdSqlSQCommand,Microsoft.ReportingServices.SemanticQueryEngine">
                <Configuration>
                    <EnableMathOpCasting>True</EnableMathOpCasting>
                    <ReplaceFunctionName>oREPLACE</ReplaceFunctionName>
                </Configuration>
            </Extension>
            <Extension Name="OLEDB-MD" Type="Microsoft.AnalysisServices.Modeling.QueryExecution.ASSemanticQueryCommand,Microsoft.AnalysisServices.Modeling"/>
        </SemanticQuery>
        <ModelGeneration>
            <Extension Name="SQL" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MsSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="SQLAZURE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.MSSQL.MsSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="ORACLE" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Oracle.OraSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="TERADATA" Type="Microsoft.ReportingServices.SemanticQueryEngine.Sql.Teradata.TdSqlModelGenerator,Microsoft.ReportingServices.SemanticQueryEngine"/>
            <Extension Name="OLEDB-MD" Type="Microsoft.AnalysisServices.Modeling.Generation.ModelGeneratorExtention,Microsoft.AnalysisServices.Modeling"/>
        </ModelGeneration>
        <Security>
            <Extension Name="Windows" Type="Microsoft.ReportingServices.Authorization.WindowsAuthorization, Microsoft.ReportingServices.Authorization"/>
        </Security>
        <Authentication>
            <Extension Name="Windows" Type="Microsoft.ReportingServices.Authentication.WindowsAuthentication, Microsoft.ReportingServices.Authorization"/>
        </Authentication>
        <EventProcessing>
            <Extension Name="SnapShot Extension" Type="Microsoft.ReportingServices.Library.HistorySnapShotCreatedHandler,ReportingServicesLibrary">
                <Event>
                    <Type>ReportHistorySnapshotCreated</Type>
                </Event>
            </Extension>
            <Extension Name="Timed Subscription Extension" Type="Microsoft.ReportingServices.Library.TimedSubscriptionHandler,ReportingServicesLibrary">
                <Event>
                    <Type>TimedSubscription</Type>
                </Event>
            </Extension>
            <Extension Name="Cache Refresh Plan Extension" Type="Microsoft.ReportingServices.Library.CacheRefreshPlanHandler,ReportingServicesLibrary">
                <Event>
                    <Type>RefreshCache</Type>
                </Event>
            </Extension>
            <Extension Name="Shared Dataset Cache Update Extension" Type="Microsoft.ReportingServices.Library.SharedDatasetCacheUpdatePlanHandler,ReportingServicesLibrary">
                <Event>
                    <Type>SharedDatasetCacheUpdate</Type>
                </Event>
            </Extension>
            <Extension Name="Cache Update Extension" Type="Microsoft.ReportingServices.Library.ReportExecutionSnapshotUpdateEventHandler,ReportingServicesLibrary">
                <Event>
                    <Type>SnapshotUpdated</Type>
                </Event>
            </Extension>
        </EventProcessing>
    </Extensions>
    <MapTileServerConfiguration>
        <MaxConnections>2</MaxConnections>
        <Timeout>10</Timeout>
        <AppID>(Default)</AppID>
        <CacheLevel>Default</CacheLevel>
    </MapTileServerConfiguration>
</Configuration> 
```  
  
##  <a name="default-configuration-file-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepointdefaultfile"></a> File di configurazione predefinito per un server di report in modalità SharePoint  
 Per impostazione predefinita, il file rsreportserver.config è installato nel percorso seguente:  
  
 **C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting**  
  
```  
<Configuration>  
  <Dsn />  
  <ConnectionType>Default</ConnectionType>  
  <LogonUser>  
  </LogonUser>  
  <LogonDomain>  
  </LogonDomain>  
  <LogonCred>  
  </LogonCred>  
  <InstanceId>MSRS12.@Sharepoint</InstanceId>  
  <Add Key="SecureConnectionLevel" Value="0" />  
  <Add Key="CleanupCycleMinutes" Value="10" />  
  <Add Key="MaxActiveReqForOneUser" Value="20" />  
  <Add Key="AlertingCleanupCycleMinutes" Value="20" />  
  <Add Key="AlertingDataCleanupMinutes" Value="360" />  
  <Add Key="AlertingExecutionLogCleanupMinutes" Value="10080" />  
  <Add Key="AlertingMaxDataRetentionDays" Value="180" />  
  <Add Key="RunningRequestsScavengerCycle" Value="60" />  
  <Add Key="RunningRequestsDbCycle" Value="60" />  
  <Add Key="RunningRequestsAge" Value="30" />  
  <Add Key="MaxScheduleWait" Value="5" />  
  <Add Key="DisplayErrorLink" Value="true" />  
  <Add Key="WebServiceUseFileShareStorage" Value="false" />  
  <!--  <Add Key="ProcessTimeout" Value="150" /> -->  
  <!--  <Add Key="ProcessTimeoutGcExtension" Value="30" /> -->  
  <!--  <Add Key="WatsonFlags" Value="0x0430" /> full dump-->  
  <!--  <Add Key="WatsonFlags" Value="0x0428" /> minidump -->  
  <!--  <Add Key="WatsonFlags" Value="0x0002" /> no dump-->  
  <Add Key="WatsonFlags" Value="0x0428" />  
  <Add Key="WatsonDumpOnExceptions" Value="Microsoft.ReportingServices.Diagnostics.Utilities.InternalCatalogException,Microsoft.ReportingServices.Modeling.InternalModelingException,Microsoft.ReportingServices.ReportProcessing.UnhandledReportRenderingException" />  
  <Add Key="WatsonDumpExcludeIfContainsExceptions" Value="System.Threading.ThreadAbortException,System.Web.UI.ViewStateException,System.OutOfMemoryException,System.Web.HttpException,System.IO.IOException,System.IO.FileLoadException,Microsoft.SharePoint.SPException,Microsoft.ReportingServices.WmiProvider.WMIProviderException" />  
  <RStrace>  
    <add name="FileName" value="ReportServerService" />  
    <add name="FileSizeLimitMb" value="32" />  
    <add name="KeepFilesForDays" value="14" />  
    <add name="Prefix" value="tid, time" />  
    <add name="TraceListeners" value="file" />  
    <add name="TraceFileMode" value="unique" />  
    <add name="Components" value="all:3" />  
  </RStrace>  
  <URLReservations>  
    <Application>  
      <Name>ReportServerWebService</Name>  
      <VirtualDirectory>ReportServer</VirtualDirectory>  
      <URLs>  
        <URL>  
          <UrlString>https://+:80</UrlString>  
          <AccountSid>  
          </AccountSid>  
          <AccountName>  
          </AccountName>  
        </URL>  
      </URLs>  
    </Application>  
    <Application>  
      <Name>ReportManager</Name>  
      <VirtualDirectory>Reports</VirtualDirectory>  
      <URLs>  
        <URL>  
          <UrlString>https://+:80</UrlString>  
          <AccountSid>  
          </AccountSid>  
          <AccountName>  
          </AccountName>  
        </URL>  
      </URLs>  
    </Application>  
  </URLReservations>  
  <Authentication>  
    <AuthenticationTypes>  
      <RSWindowsNTLM />  
    </AuthenticationTypes>  
    <EnableAuthPersistence>true</EnableAuthPersistence>  
  </Authentication>  
  <Service>  
    <IsSchedulingService>True</IsSchedulingService>  
    <IsNotificationService>True</IsNotificationService>  
    <IsEventService>True</IsEventService>  
    <IsAlertingService>True</IsAlertingService>  
    <PollingInterval>10</PollingInterval>  
    <WindowsServiceUseFileShareStorage>False</WindowsServiceUseFileShareStorage>  
    <MemorySafetyMargin>80</MemorySafetyMargin>  
    <MemoryThreshold>90</MemoryThreshold>  
    <RecycleTime>720</RecycleTime>  
    <MaxAppDomainUnloadTime>30</MaxAppDomainUnloadTime>  
    <MaxQueueThreads>0</MaxQueueThreads>  
    <UrlRoot>  
    </UrlRoot>  
    <PolicyLevel>rssrvpolicy.config</PolicyLevel>  
    <IsWebServiceEnabled>True</IsWebServiceEnabled>    
    <FileShareStorageLocation>  
      <Path>  
      </Path>  
    </FileShareStorageLocation>  
  </Service>  
  <UI>  
    <ReportServerUrl>  
    </ReportServerUrl>  
    <PageCountMode>Estimate</PageCountMode>  
  </UI>  
  <MapTileServerConfiguration>  
    <MaxConnections>2</MaxConnections>  
    <Timeout>10</Timeout>  
    <AppID>(Default)</AppID>  
    <CacheLevel>Default</CacheLevel>  
  </MapTileServerConfiguration>  
</Configuration>  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Modificare un file di configurazione di Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurare la memoria disponibile per applicazioni del server di report](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [File di configurazione di Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Inizializzare un server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md)   
 [Archiviare i dati crittografati del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md)   
 [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

 Altre domande? [Visitare il forum su Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
  
