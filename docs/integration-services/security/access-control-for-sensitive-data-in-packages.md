---
description: Controllo dell'accesso per dati sensibili nei pacchetti
title: Controllo dell'accesso per dati sensibili nei pacchetti | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.packageprotectionlevel.f1
- sql13.ssis.bids.projectprotectionlevel.f1
- sql13.dts.designer.packagepassword.f1
- sql13.ssis.bids.projectpassword.f1
helpviewer_keywords:
- passwords [Integration Services]
- packages [Integration Services], security
- protection levels for packages [Integration Services]
- configuration files [Integration Services]
- encryption [Integration Services]
- cryptography [Integration Services]
- security [Integration Services], protection levels
ms.assetid: d4b073c4-4238-41fc-a258-4e114216e185
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b85dc4949e363a8e62ca3d992a04365f7ce414f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353008"
---
# <a name="access-control-for-sensitive-data-in-packages"></a>Controllo dell'accesso per dati sensibili nei pacchetti

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Per proteggere i dati in un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , è possibile impostare un livello di protezione in modo da proteggere i dati sensibili o tutti i dati all'interno del pacchetto. Inoltre, è possibile crittografare questi dati con una password o una chiave utente o utilizzare il sistema di crittografia del database. Inoltre, il livello di protezione che si utilizza per un pacchetto non è necessariamente statico, ma cambia durante tutto il ciclo di vita del pacchetto. Spesso si imposta un livello di protezione durante lo sviluppo e un altro appena si distribuisce il pacchetto.  
  
> [!NOTE]  
>  Oltre ai livelli di protezione descritti in questo argomento, è possibile usare i ruoli predefiniti a livello di database per proteggere i pacchetti salvati nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
## <a name="definition-of-sensitive-information"></a>Definizione di informazioni riservate  
 In un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] le informazioni seguenti sono definite *riservate*:  
  
-   La password di una stringa di connessione. Se, tuttavia, si seleziona un'opzione che implica la crittografia dell'intero pacchetto, viene considerata riservata l'intera stringa di connessione.  
  
-   I nodi XML generati dall'attività contrassegnati come riservati. L'aggiunta di tag ai nodi XML viene controllata da [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] e non può essere modificata dagli utenti.  
  
-   Qualsiasi variabile contrassegnata come riservata. Le variabili vengono contrassegnate come riservate da [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 In [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] una proprietà viene considerata riservata a seconda se lo sviluppatore del componente di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ad esempio una gestione connessione o un'attività, abbia definito la proprietà come riservata. Gli utenti non possono aggiungere proprietà all'elenco di proprietà considerate riservate, né possono rimuoverle.  
  
## <a name="encryption"></a>Crittografia  
 La crittografia applicata con i livelli di protezione dei pacchetti viene eseguita tramite l'API [!INCLUDE[msCoName](../../includes/msconame-md.md)] Data Protection (DPAPI), che fa parte dell'API Cryptography (CryptoAPI).  
  
 Con i livelli di protezione dei pacchetti che implicano la crittografia con password anche l'utente autorizzato a modificare l'origine deve specificare una password. Se tale utente sostituisce un livello di protezione che non richiede la password con un livello che la richiede, verrà richiesto di specificare una password.  
  
 Inoltre, per i livelli di protezione che prevedono una password, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa l'algoritmo di crittografia Triple DES con lunghezza di chiave di 192 bit, disponibile nella libreria di classi [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (FCL).  
  
## <a name="protection-levels"></a>Livelli di protezione  
 Nella tabella seguente vengono descritti i livelli di protezione disponibili in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . I valori tra parentesi derivano dall'enumerazione <xref:Microsoft.SqlServer.Dts.Runtime.DTSProtectionLevel> . e sono visualizzati nella finestra Proprietà utilizzata per la configurazione delle proprietà del pacchetto quando si utilizzano i pacchetti in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
|Livello di protezione|Descrizione|  
|----------------------|-----------------|  
|Non salva i dati sensibili (**DontSaveSensitive**)|Quando si salva il pacchetto, i valori delle proprietà riservate vengono eliminati. Con questo livello di protezione i dati non vengono crittografati, ma le proprietà contrassegnate come riservate non vengono salvate insieme al pacchetto. Di conseguenza i dati riservati non sono disponibili ad altri utenti, i quali dovranno specificarli quando aprono il pacchetto.<br /><br /> Se usato con l'utilità **dtutil** (dtutil.exe), questo livello di protezione corrisponde al valore 0.|  
|Crittografa tutti i dati con una password (**EncryptAllWithPassword**)|Viene utilizzata una password per crittografare l'intero pacchetto. Il pacchetto viene crittografato con una password specificata dall'utente in fase di creazione o di esportazione del pacchetto. Per aprire il pacchetto in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o per eseguirlo con l'utilità del prompt dei comandi **dtexec** , l'utente deve specificare la password del pacchetto. Se non specifica la password corretta, l'utente non può né aprire né eseguire il pacchetto.<br /><br /> Se usato con l'utilità **dtutil** , questo livello di protezione corrisponde al valore 3.|  
|Crittografa tutti i dati con una chiave utente (**EncryptAllWithUserKey**)|Viene utilizzata una chiave basata sul profilo utente corrente per crittografare l'intero pacchetto. Il pacchetto può essere aperto in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o eseguito con l'utilità del prompt dei comandi **dtexec** solo dall'utente che lo ha creato o esportato.<br /><br /> Se usato con l'utilità **dtutil** , questo livello di protezione corrisponde al valore 4.<br /><br /> Nota: per i livelli di protezione che prevedono una chiave utente, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa gli standard DPAPI. Per altre informazioni su DPAPI, vedere MSDN Library all'indirizzo [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Crittografa tutti i dati sensibili con una password (**EncryptSensitiveWithPassword**)|Viene utilizzata una password per crittografare solo i valori delle proprietà riservate nel pacchetto. Per la crittografia viene utilizzato DPAPI. I dati riservati vengono salvati come parte del pacchetto, ma crittografati tramite una password specificata dall'utente corrente in fase di creazione o di esportazione del pacchetto. Per poter aprire il pacchetto in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , l'utente deve specificare la password. Se non specifica la password, il pacchetto viene aperto senza i dati riservati e l'utente corrente deve specificare nuovi valori per questi dati. I tentativi di esecuzione del pacchetto senza specificare la password hanno esito negativo. Per altre informazioni sulle password e sull'esecuzione dell'utilità della riga di comando, vedere [Utilità dtexec](../../integration-services/packages/dtexec-utility.md).<br /><br /> Se usato con l'utilità **dtutil** , questo livello di protezione corrisponde al valore 2.|  
|Crittografa tutti i dati sensibili con una chiave utente (**EncryptSensitiveWithPassword**)|Viene utilizzata una chiave basata sul profilo utente corrente per crittografare solo i valori delle proprietà riservate nel pacchetto. Il pacchetto può essere caricato solo da uno stesso utente in base allo stesso profilo. Gli altri utenti che aprono il pacchetto dovranno immettere le informazioni riservate. I tentativi di esecuzione del pacchetto hanno esito negativo. Per la crittografia viene utilizzato DPAPI.<br /><br /> Se usato con l'utilità **dtutil** , questo livello di protezione corrisponde al valore 1.<br /><br /> Nota: per i livelli di protezione che prevedono una chiave utente, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa gli standard DPAPI. Per altre informazioni su DPAPI, vedere MSDN Library all'indirizzo [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Usa l'archiviazione su server per la crittografia (**ServerStorage**)|Protegge l'intero pacchetto tramite ruoli del database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questa opzione è supportata quando i pacchetti vengono salvati nel database msdb di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Anche il catalogo SSISDB usa il livello di protezione **ServerStorage** .<br /><br /> Questa opzione non è supportata quando un pacchetto viene salvato nel file system da [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|  
  
## <a name="protection-level-setting-and-the-ssisdb-catalog"></a>Impostazione del livello di protezione e catalogo SSISDB  
 Il catalogo SSISDB usano il livello di protezione **ServerStorage** . Quando si distribuisce un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nel server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , il catalogo crittografa automaticamente i dati e i valori sensibili del pacchetto. Il catalogo inoltre decrittografa automaticamente i dati quando viene recuperato.  
  
 Se si esporta il progetto (file con estensione ispac) dal server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al file system, il sistema modifica automaticamente il livello di protezione in **EncryptSensitiveWithUserKey**. Se si importa il progetto usando **Importazione guidata progetto di Integration Services** in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la proprietà **ProtectionLevel** nella finestra **Proprietà** mostra il valore **EncryptSensitiveWithUserKey**.  
  
## <a name="protection-level-setting-based-on-package-life-cycle"></a>Impostazione del livello di protezione sulla base del ciclo di vita del pacchetto  
 Il livello di protezione di un pacchetto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] viene impostato quando il pacchetto viene sviluppato per la prima volta in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. È possibile aggiornarlo successivamente quando il pacchetto viene distribuito, importato o esportato da [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] a [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]oppure copiato da [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nell'archivio pacchetti [!INCLUDE[ssIS](../../includes/ssis-md.md)] o nel file system. Si supponga, ad esempio, che nei computer in uso vengano creati e salvati pacchetti in base a una delle opzioni di livello di protezione con chiave utente. Se successivamente i pacchetti vengono distribuiti ad altri utenti, sarà necessario modificarne il livello di protezione in modo che possano essere aperti dagli altri utenti.  
  
 In genere, il livello di protezione viene modificato come spiegato nei passaggi seguenti:  
  
1.  Durante lo sviluppo, lasciare il livello di protezione dei pacchetti impostato sul valore predefinito **EncryptSensitiveWithUserKey**. Con questa impostazione si avrà la certezza che solo lo sviluppatore veda i valori riservati nel pacchetto. In alternativa, è possibile provare a usare **EncryptAllWithUserKey** o **DontSaveSensitive**.  
  
2.  Al momento di distribuire i pacchetti, è necessario impostare un livello di protezione che non dipenda dalla chiave utente dello sviluppatore. Di conseguenza, in genere è necessario selezionare **EncryptSensitiveWithPassword** o **EncryptAllWithPassword**. Crittografare i pacchetti assegnando una password complessa temporanea che è nota anche dal team di gestione nell'ambiente di produzione.  
  
3.  Dopo che i pacchetti sono stati distribuiti all'ambiente di produzione, il team di gestione può crittografare nuovamente i pacchetti distribuiti assegnando una password complessa nota solo dal team stesso. In alternativa, può crittografare i pacchetti distribuiti selezionando **EncryptSensitiveWithUserKey** o **EncryptAllWithUserKey** e usando le credenziali locali dell'account per l'esecuzione dei pacchetti.  

## <a name="set-or-change-the-protection-level-of-packages"></a><a name="set_protection"></a> Impostazione o modifica del livello di protezione dei pacchetti
  Per controllare l'accesso al contenuto dei pacchetti e ai valori sensibili contenuti, ad esempio password, impostare il valore della proprietà **ProtectionLevel** . Per poter compilare il progetto, ai pacchetti contenuti in un progetto deve essere assegnato lo stesso livello di protezione del progetto. Se si modifica l'impostazione della proprietà **ProtectionLevel** nel progetto, è necessario aggiornare manualmente l'impostazione delle proprietà per i pacchetti.  
  
 Per informazioni generali sulle funzionalità di sicurezza in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Panoramica della sicurezza &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
 Le procedure presenti in questo argomento descrivono come usare [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o l'utilità della riga di comando dtutil per modificare la proprietà **ProtectionLevel** .  
  
> [!NOTE]  
>  Oltre alle procedure di questo argomento, è in genere possibile impostare o modificare la proprietà **ProtectionLevel** di un pacchetto quando si importa o esporta il pacchetto. È anche possibile modificare la proprietà **ProtectionLevel** di un pacchetto quando si usa l'Importazione/Esportazione guidata [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per salvare un pacchetto.  
  
### <a name="to-set-or-change-the-protection-level-of-a-package-in-sql-server-data-tools"></a>Per impostare o modificare il livello di protezione di un pacchetto in SQL Server Data Tools  
  
1.  Controllare i valori disponibili per la proprietà **ProtectionLevel** nella sezione [Livelli di protezione](#protection-levels) e determinare il valore appropriato per il pacchetto.  
  
2.  In [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenente il pacchetto.  
  
3.  Aprire il pacchetto nella finestra di progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
4.  Se nella finestra Proprietà non sono riportate le proprietà del pacchetto, fare clic sull'area di progettazione.  
  
5.  Selezionare il valore adatto per la proprietà **ProtectionLevel** nel gruppo **Sicurezza** della finestra Proprietà.  
  
     Se si seleziona un livello di protezione che richiede una password, immettere la password come valore della proprietà **PackagePassword** .  
  
6.  Per salvare il pacchetto modificato, scegliere **Salva elementi selezionati** dal menu **File** .  
  
### <a name="to-set-or-change-the-protection-level-of-packages-at-the-command-prompt"></a>Per impostare o modificare il livello di protezione dei pacchetti dal prompt dei comandi  
  
1.  Controllare i valori disponibili per la proprietà **ProtectionLevel** nella sezione [Livelli di protezione](#protection-levels) e determinare il valore appropriato per il pacchetto.  
  
2.  Controllare i mapping per l'opzione **Encrypt** nell'argomento [Utilità dtutil](../../integration-services/dtutil-utility.md)e determinare il valore intero appropriato da usare come valore della proprietà **ProtectionLevel** selezionata.  
  
3.  Aprire una finestra del prompt dei comandi.  
  
4.  Al prompt dei comandi, passare alla cartella contenente il pacchetto o i pacchetti per cui si vuole impostare la proprietà **ProtectionLevel** .  
  
     Negli esempi di sintassi illustrati nel passaggio seguente si presuppone che questa cartella sia la cartella corrente.  
  
5.  Impostare o modificare il livello di protezione del pacchetto o dei pacchetti utilizzando un comando simile a quello degli esempi seguenti:  
  
    -   Il comando seguente imposta la proprietà **ProtectionLevel** di un pacchetto singolo nel file system sul livello 2, "Crittografa tutti i dati sensibili con una password", con la password "strongpassword":  
  
         `dtutil.exe /file "C:\Package.dtsx" /encrypt file;"C:\Package.dtsx";2;strongpassword`  
  
    -   Il comando seguente imposta la proprietà **ProtectionLevel** di tutti i pacchetti in una particolare cartella nel file system sul livello 2, "Crittografa tutti i dati sensibili con una password", con la password "strongpassword":  
  
         `for %f in (*.dtsx) do dtutil.exe /file %f /encrypt file;%f;2;strongpassword`  
  
         Se si utilizza un comando simile in un file batch, immettere il segnaposto del file "% f" come "%% f" nel file batch.  

## <a name="package-project-protection-level-dialog-box"></a><a name="protection_dialog"></a> Finestra di dialogo Livello di protezione pacchetto e Livello di protezione del progetto
  Utilizzare la finestra di dialogo **Livello di protezione pacchetto** per aggiornare il livello di protezione di un pacchetto. Il livello di protezione determina il metodo di protezione, la password o chiave utente e l'ambito di protezione del pacchetto. La protezione può includere tutti i dati o solo i dati sensibili.  
  
 Per comprendere i requisiti e le opzioni relative alla sicurezza dei pacchetti, vedere [Panoramica della sicurezza &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
### <a name="options"></a>Opzioni  
 **Livello di protezione pacchetto**  
 Selezionare un livello di protezione dall'elenco.  
  
 **Password**  
 Se si usa il livello di protezione **Crittografa tutti i dati riservati con una password** o **Crittografa tutti i dati con una password** , digitare una password.  
  
 **Conferma password**  
 Digitare di nuovo la password.  

## <a name="package-password-dialog-box"></a><a name="password_dialog"></a> Finestra di dialogo Password pacchetto
  Utilizzare la finestra di dialogo **Password pacchetto** per specificare la password del pacchetto per un pacchetto crittografato con una password. È necessario specificare una password se il pacchetto utilizza il livello di protezione **Crittografa tutti i dati riservati con una password** oppure **Crittografa tutti i dati con una password** .  
  
### <a name="options"></a>Opzioni  
 **Password**  
 Consente di immettere la password.  
  
## <a name="see-also"></a>Vedere anche  
 [Pacchetti di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Panoramica sulla sicurezza &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
 [Utilità dtutil](../../integration-services/dtutil-utility.md)  
