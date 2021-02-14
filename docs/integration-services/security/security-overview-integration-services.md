---
description: Panoramica sulla sicurezza (Integration Services)
title: Panoramica sulla sicurezza (Integration Services) | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SSIS packages, security
- Integration Services, security
- security [Integration Services], about security
- passwords [Integration Services]
- packages [Integration Services], security
- SQL Server Integration Services, security
- SSIS, security
- Integration Services packages, security
- SQL Server Integration Services packages, security
ms.assetid: 01aa0b88-d477-4581-9a3b-2efc3de2b133
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6b757c7c0a60ab8cff5c496c2a0774646da4c7f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340768"
---
# <a name="security-overview-integration-services"></a>Panoramica sulla sicurezza (Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] la sicurezza è costituita da diversi livelli che forniscono un ambiente di sicurezza protetto e flessibile. Questi livelli di sicurezza includono l'uso di firme digitali, proprietà dei pacchetti, ruoli di database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e autorizzazioni del sistema operativo. La maggior parte di queste caratteristiche di sicurezza rientra nelle categorie di identità e di controllo dell'accesso.  

## <a name="threat-and-vulnerability-mitigation"></a>Attenuazione di minacce e vulnerabilità
  Sebbene in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] siano inclusi diversi meccanismi di sicurezza, i pacchetti e i file che da questi vengono creati o usati potrebbero essere usati da utenti malintenzionati.  
  
 Nelle tabelle seguenti vengono illustrati i rischi e le azioni che è possibile intraprendere per ridurli.  
  
|Minaccia o vulnerabilità|Definizione|Strategia di riduzione del rischio|  
|-----------------------------|----------------|----------------|  
|Origine pacchetto|L'origine di un pacchetto è la persona o l'organizzazione che ha creato il pacchetto. L'esecuzione di un pacchetto da un'origine ignota o non attendibile potrebbe essere rischiosa.|Identificare l'origine di un pacchetto utilizzando una firma digitale ed eseguire pacchetti provenienti solo da origini note e attendibili. Per altre informazioni, vedere [Identificazione dell'origine dei pacchetti con firme digitali](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).|  
|Contenuto del pacchetto|Nel pacchetto sono inclusi gli elementi del pacchetto e le rispettive proprietà. Tali proprietà possono contenere dati sensibili, ad esempio una password o una stringa di connessione. Gli elementi del pacchetto, ad esempio un'istruzione SQL, possono rivelare la struttura del database.|Controllare l'accesso a un pacchetto e al relativo contenuto eseguendo i passaggi seguenti:<br /><br /> 1) Per controllare l'accesso al pacchetto stesso, applicare caratteristiche di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ai pacchetti salvati nel database **msdb** in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ai pacchetti salvati nel file system applicare le caratteristiche di sicurezza del file system, ad esempio elenchi di controllo di accesso (ACL).<br /><br /> 2) Per controllare l'accesso al contenuto del pacchetto, impostarne il livello di protezione.<br /><br /> Per altre informazioni, vedere [Panoramica sulla sicurezza &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md) e [Controllo dell'accesso per dati sensibili nei pacchetti](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).|  
|Output di pacchetto|Quando si configura un pacchetto per l'utilizzo di configurazioni, checkpoint e registrazione, le informazioni vengono archiviate all'esterno del pacchetto stesso. Tali informazioni potrebbero contenere dati sensibili.|Per proteggere le configurazioni e le registrazioni che il pacchetto salva nelle tabelle di database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , usare le caratteristiche di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Per controllare l'accesso ai file, utilizzare gli elenchi di controllo di accesso (ACL) disponibili nel file system.<br /><br /> Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](#files)|  
  
## <a name="identity-features"></a>Caratteristiche di identità  
 Implementando le caratteristiche di identità nei pacchetti, è possibile raggiungere l'obiettivo seguente:  
  
 **Aprire ed eseguire i pacchetti solo da origini attendibili**.  
  
 Per aprire ed eseguire i pacchetti solo da origini attendibili, è necessario identificare innanzitutto l'origine dei pacchetti. È possibile identificare l'origine dei pacchetti tramite la firma con certificati. Quando si aprono o si eseguono i pacchetti, è possibile impostare [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in modo che verifichi la presenza e la validità delle firme digitali. Per altre informazioni, vedere [Identificazione dell'origine dei pacchetti con firme digitali](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
## <a name="access-control-features"></a>Caratteristiche di controllo dell'accesso  
 Implementando le caratteristiche di identità nei pacchetti, è possibile raggiungere l'obiettivo seguente:  
  
 **Assicurarsi che i pacchetti vengano aperti ed eseguiti solo da utenti autorizzati**.  
  
 Per assicurarsi che i pacchetti vengano aperti ed eseguiti solo da utenti autorizzati, è necessario controllare l'accesso alle informazioni seguenti:  
  
-   Controllare l'accesso al contenuto dei pacchetti, soprattutto ai dati sensibili.  
  
-   Controllare l'accesso ai pacchetti e alle configurazioni dei pacchetti archiviate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Controllare l'accesso ai pacchetti e ai file correlati, quali configurazioni, log e file di checkpoint, archiviati nel file system.  
  
-   Controllare l'accesso al servizio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e alle informazioni sui pacchetti visualizzate dal servizio in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="controlling-access-to-the-contents-of-packages"></a>Controllo dell'accesso al contenuto dei pacchetti  
 Per limitare l'accesso al contenuto di un pacchetto, è possibile crittografare i pacchetti impostando la proprietà ProtectionLevel del pacchetto. Questa proprietà può essere impostata sul livello di protezione richiesto dal pacchetto. Ad esempio, nell'ambiente di sviluppo di un gruppo di lavoro è possibile crittografare un pacchetto impostando una password nota solo ai membri del gruppo che lavorano al pacchetto.  
  
 Quando si imposta la proprietà ProtectionLevel di un pacchetto, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] individua automaticamente le proprietà riservate e le gestisce in base al livello di protezione pacchetto specificato. Impostare ad esempio la proprietà ProtectionLevel per un pacchetto su un livello che crittografa le informazioni riservate con una password. Per questo pacchetto, in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vengono crittografati automaticamente i valori di tutte le proprietà riservate e i dati corrispondenti verranno visualizzati solo se viene fornita la password corretta.  
  
 In genere, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] identifica le proprietà come riservate se contengono informazioni, ad esempio una password o una stringa di connessione, o se corrispondono a variabili o a nodi XML generati dall'attività. In [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] una proprietà viene considerata riservata a seconda se lo sviluppatore del componente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ad esempio una gestione connessione o un'attività, abbia definito la proprietà come riservata. Gli utenti non possono aggiungere proprietà all'elenco di proprietà considerate riservate, né possono rimuoverle. Se si scrivono attività, gestioni connessione o componenti del flusso di dati personalizzati, è possibile specificare le proprietà che devono essere considerate riservate in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Per altre informazioni, vedere [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
### <a name="controlling-access-to-packages"></a>Controllo dell'accesso ai pacchetti  
 I pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] possono essere salvati nel database msdb in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o nel file system come file XML con estensione dtsx. Per altre informazioni, vedere [Salvataggio di pacchetti](../../integration-services/save-packages.md).  
  
#### <a name="saving-packages-to-the-msdb-database"></a>Salvataggio dei pacchetti nel database msdb  
 Con il salvataggio dei pacchetti nel database msdb la sicurezza viene implementata a livello di server, di database e di tabella. Nel database msdb i pacchetti [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vengono archiviati nella tabella sysssispackages. Poiché i pacchetti vengono salvati nelle tabelle sysssispackages e sysdtspackages del database msdb, quando si esegue il backup del database msdb automaticamente viene eseguito il backup dei pacchetti.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - I pacchetti archiviati nel database msdb possono essere protetti anche applicando i ruoli a livello di database di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] include i tre ruoli predefiniti a livello di database db_ssisadmin, db_ssisltduser e db_ssisoperator per il controllo dell'accesso ai pacchetti. A ogni pacchetto è possibile associare un ruolo di lettore e un ruolo di scrittore. È inoltre possibile definire ruoli personalizzati a livello di database da usare nei pacchetti di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . I ruoli possono essere implementati solo nei pacchetti salvati nel database msdb in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Ruoli Integration Services &#40;servizio SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
#### <a name="saving-packages-to-the-file-system"></a>Salvataggio di pacchetti nel file system  
 Se i pacchetti vengono archiviati nel file system anziché nel database msdb, è necessario proteggere i file di pacchetto e le cartelle che li contengono.  
  
### <a name="controlling-access-to-files-used-by-packages"></a>Controllo dell'accesso ai file utilizzati dai pacchetti  
 I pacchetti configurati per l'utilizzo di configurazioni, checkpoint e registrazione generano informazioni che vengono archiviate all'esterno dei pacchetti stessi. Tali informazioni potrebbero essere riservate e devono essere pertanto protette. I file di checkpoint possono essere salvati solo nel file system, mentre le configurazione e i log possono essere salvati sia nel file system che nelle tabelle di un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le configurazioni e i log salvati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono protetti tramite la sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ma per le informazioni scritte ne file system è necessario implementare livelli di sicurezza aggiuntivi.  
  
 Per altre informazioni, vedere [Accesso ai file utilizzati dai pacchetti](#files).  
  
#### <a name="storing-package-configurations-securely"></a>Archiviazione sicura delle configurazioni del pacchetto  
 Le configurazioni dei pacchetti possono essere salvate in una tabella in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel file system.  
  
 Le configurazioni possono essere salvate in qualsiasi database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , non solo nel database msdb. È quindi possibile specificare quale database utilizzare come repository delle configurazioni dei pacchetti. È inoltre possibile specificare il nome della tabella in cui includere le configurazioni, la quale verrà creata automaticamente in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con la struttura corretta. Con il salvataggio delle configurazioni in una tabella la sicurezza viene implementata a livello di server, di database e di tabella. Inoltre, per le configurazioni salvate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito automaticamente il backup quando si esegue il backup del database.  
  
 Se le configurazioni vengono archiviate nel file system anziché in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario proteggere le cartelle contenenti i file di configurazione dei pacchetti.  
  
 Per ulteriori informazioni sulle configurazioni, vedere [Package Configurations](../packages/legacy-package-deployment-ssis.md).  
  
### <a name="controlling-access-to-the-integration-services-service"></a>Controllo dell'accesso al servizio Integration Services  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] l'elenco dei pacchetti archiviati viene visualizzato tramite il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per impedire agli utenti non autorizzati di visualizzare informazioni sui pacchetti archiviati in computer locali e remoti, acquisendo quindi informazioni private, limitare l'accesso ai computer in cui viene eseguito il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Per altre informazioni, vedere [Accesso al servizio Integration Services](#service).  

## <a name="access-to-files-used-by-packages"></a><a name="files"></a> Accesso ai file utilizzati dai pacchetti
  Il livello di protezione del pacchetto non protegge i file archiviati al di fuori del pacchetto. ovvero i file seguenti:  
  
-   File di configurazione  
  
-   file del checkpoint  
  
-   File di log  
  
 Questi file devono essere protetti separatamente, soprattutto se includono informazioni riservate.  
  
### <a name="configuration-files"></a>File di configurazione  
 Se un file di configurazione contiene dati riservati, come informazioni su account di accesso e password, è consigliabile salvare tale configurazione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]oppure usare un elenco di controllo di accesso (ACL) per limitare l'accesso al percorso o alla cartella in cui sono archiviati i file e consentire l'accesso solo a determinati account. L'accesso viene in genere concesso agli account autorizzati all'esecuzione dei pacchetti e agli account utilizzati per la gestione e la risoluzione dei problemi dei pacchetti, che possono comportare la revisione dei contenuti dei file di configurazione, del checkpoint e di log. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispone di un'archiviazione più sicura perché la protezione viene garantita ai livelli del server e del database. Per salvare le configurazioni in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario usare il tipo di configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mentre per salvarle nel file system è necessario utilizzare il tipo di configurazione XML.  
  
 Per altre informazioni, vedere [Configurazioni di pacchetto](../packages/legacy-package-deployment-ssis.md), [Creazione di configurazioni dei pacchetti](../packages/legacy-package-deployment-ssis.md)e [Considerazioni sulla sicurezza per un'installazione SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
### <a name="checkpoint-files"></a>file del checkpoint  
 In modo analogo, è consigliabile utilizzare un elenco di controllo di accesso del sistema operativo per la sicurezza del percorso o della cartella se il file del checkpoint utilizzato dal pacchetto contiene informazioni riservate. Nei file del checkpoint vengono salvate informazioni aggiornate sullo stato del pacchetto e sui valori correnti delle variabili. Il pacchetto potrebbe includere, ad esempio, una variabile personalizzata per la memorizzazione di un numero di telefono. Per ulteriori informazioni, vedere [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
### <a name="log-files"></a>File di log  
 Anche le voci di log scritte nel file system dovrebbero essere protette tramite un elenco di controllo di accesso. È inoltre possibile archiviare le voci di log nelle tabelle di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e proteggerle con gli strumenti di sicurezza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le voci di log possono infatti includere informazioni riservate. Se ad esempio il pacchetto contiene un'attività Esegui SQL che genera un'istruzione SQL che fa riferimento a un numero di telefono, la voce di log per l'istruzione SQL conterrà tale numero di telefono. L'istruzione SQL potrebbe inoltre esporre informazioni private sui nomi di tabelle e colonne nei database. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  

## <a name="access-to-the-integration-services-service"></a><a name="service"></a> Accesso al servizio Integration Services
  Tramite i livelli di protezione dei pacchetti è possibile limitare gli utenti a cui è consentito modificare ed eseguire un pacchetto. Per limitare gli utenti a cui è consentito visualizzare l'elenco di pacchetti attualmente in esecuzione in un server e arrestare i pacchetti attualmente in esecuzione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]è necessaria una protezione aggiuntiva.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usa il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per visualizzare l'elenco dei pacchetti in esecuzione. I membri del gruppo Administrators di Windows possono visualizzare e arrestare tutti i pacchetti in esecuzione. Gli utenti che non appartengono a questo gruppo possono visualizzare e arrestare solo i pacchetti che hanno avviato personalmente.  
  
 È importante limitare l'accesso ai computer che eseguono un servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , soprattutto se si tratta di un servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che consente l'enumerazione di cartelle remote. Gli utenti autenticati possono richiedere l'enumerazione di pacchetti. Anche se il servizio non viene individuato, le cartelle vengono comunque enumerate dal servizio stesso. Questi nomi di cartella potrebbero essere sfruttati da un utente malintenzionato. Se un amministratore ha configurato il servizio in modo da consentire l'enumerazione di cartelle in un computer remoto, agli utenti potrebbero essere visualizzati anche i nomi di cartella in genere non visibili.  

## <a name="related-tasks"></a>Attività correlate  
 Nell'elenco seguente sono contenuti collegamenti ad argomenti che illustrano come eseguire un'attività relativa alla sicurezza.  
  
-   [Creazione di un ruolo definito dall'utente](../../integration-services/security/integration-services-roles-ssis-service.md#create)  
  
-   [Assegnazione di un ruolo lettura e scrittura a un pacchetto](../../integration-services/security/integration-services-roles-ssis-service.md#assign)  
  
-   [Implementazione di criteri per le firme impostando un valore del Registro di sistema](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#registry)  
  
-   [Firma di un pacchetto tramite certificato digitale](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#cert)  
  
-   [Impostazione o modifica del livello di protezione dei pacchetti](../../integration-services/security/access-control-for-sensitive-data-in-packages.md#set_protection)