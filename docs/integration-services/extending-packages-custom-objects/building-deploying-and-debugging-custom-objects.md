---
description: Compilazione, distribuzione e debug di oggetti personalizzati
title: Compilazione, distribuzione e debug di oggetti personalizzati | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom objects [Integration Services]
ms.assetid: b03685bc-5398-4c3f-901a-1219c1098fbe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb634702b48de70d505851c53ce3f1f6562547a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348950"
---
# <a name="building-deploying-and-debugging-custom-objects"></a>Compilazione, distribuzione e debug di oggetti personalizzati

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dopo avere scritto il codice di un oggetto personalizzato per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], è necessario compilare l'assembly, distribuirlo e integrarlo in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] per renderlo disponibile per l'uso nei pacchetti e quindi sottoporlo a test e debug.  
  
##  <a name="steps-in-building-deploying-and-debugging-a-custom-object-for-integration-services"></a><a name="top"></a>Passaggi per la compilazione, la distribuzione e il debug di un oggetto personalizzato per Integration Services  
 La funzionalità personalizzata per l'oggetto è già stata scritta. A questo punto, è necessario testarla e renderla disponibile per gli utenti. I passaggi sono molto simili per tutti i tipi di oggetti personalizzati che è possibile creare per [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Ecco i passaggi per compilarlo, distribuirlo e testarlo.  
  
1.  [Firmare](#signing) l'assembly da generare con un nome sicuro.  
  
2.  [Compilare](#building) l'assembly.  
  
3.  [Distribuire](#deploying) l'assembly spostandolo o copiandolo nella cartella di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] appropriata.  
  
4.  [Installare](#installing) l'assembly nella Global Assembly Cache.  
  
     L'oggetto viene automaticamente aggiunto alla Casella degli strumenti.  
  
5.  Se necessario, [risolvere i problemi](#troubleshooting) della distribuzione.  
  
6.  [Eseguire il test](#testing) e il debug del codice.  
  
 È ora possibile usare Progettazione SSIS in SQL Server Data Tools (SSDT) per creare, gestire ed eseguire pacchetti destinati a versioni diverse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni sull'impatto di questo miglioramento sulle estensioni personalizzate, vedere [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/) (Supportare le estensioni SSIS personalizzate in più versioni di SSDT 2015 per SQL Server 2016).  
  
##  <a name="signing-the-assembly"></a><a name="signing"></a> Firma dell'assembly  
 Quando un assembly deve essere condiviso, è necessario installarlo nella Global Assembly Cache. Una volta aggiunto alla Global Assembly Cache, l'assembly può essere utilizzato da applicazioni come [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Un requisito della Global Assembly Cache è che l'assembly deve essere firmato con un nome sicuro che garantisca che sia globalmente univoco. Un assembly con nome sicuro dispone di un nome completo che include il nome, la lingua, la chiave pubblica e il numero di versione dell'assembly. Tali informazioni vengono utilizzate dal runtime per individuare l'assembly e distinguerlo da altri assembly aventi lo stesso nome.  
  
 Per firmare un assembly con un nome sicuro, è innanzitutto necessario avere o creare una coppia di chiavi pubblica/privata. Questa coppia di chiavi di crittografia, pubblica e privata, viene utilizzata durante la compilazione per creare un assembly con nome sicuro.  
  
 Per ulteriori informazioni sui nomi sicuri e sui passaggi che è necessario eseguire per firmare un assembly, vedere gli argomenti seguenti nella documentazione relativa a [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK:  
  
-   Assembly con nomi sicuri  
  
-   Creazione di una coppia di chiavi  
  
-   Firma di un assembly con un nome sicuro  
  
 È possibile firmare facilmente l'assembly con un nome sicuro in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] durante la compilazione. Nella finestra di dialogo **Proprietà progetto** selezionare la scheda **Firma**. Selezionare l'opzione **Firma assembly** e quindi specificare il percorso del file di chiave (con estensione snk).  
  
##  <a name="building-the-assembly"></a><a name="building"></a> Compilazione dell'assembly  
 Dopo aver firmato il progetto, è necessario compilare o ricompilare il progetto o la soluzione tramite i comandi disponibili nel menu **Compila** di [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. La soluzione può contenere un progetto distinto per un'interfaccia utente personalizzata, che deve essere firmato con un nome sicuro e può essere compilato contemporaneamente.  
  
 Il metodo più semplice per eseguire i due passaggi successivi, ovvero la distribuzione dell'assembly e l'installazione nella Global Assembly Cache, consiste nel generare script per questi passaggi come evento di post-compilazione in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Gli eventi di compilazione sono disponibili dalla pagina **Compilazione** di Proprietà progetto per un progetto [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] e dalla pagina **Eventi di compilazione** per un progetto C#. Per utilità del prompt dei comandi come **gacutil.exe** è necessario specificare il percorso completo. È necessario racchiudere tra virgolette i percorsi che contengono spazi e le macro, ad esempio $(TargetPath), che si espandono in percorsi che contengono spazi.  
  
 Di seguito è riportato un esempio di una riga di comando per eventi di post-compilazione per un provider di log personalizzato:  
  
```cmd
"C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe" -u $(TargetName)  
"C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\bin\NETFX 4.0 Tools\gacutil.exe" -i $(TargetFileName)  
copy $(TargetFileName) "C:\Program Files\Microsoft SQL Server\130\DTS\LogProviders "  
```  
  
##  <a name="deploying-the-assembly"></a><a name="deploying"></a> Distribuzione dell'assembly  
 Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] individua gli oggetti personalizzati disponibili per l'uso nei pacchetti enumerando i file trovati in una serie di cartelle create durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se si usano le impostazioni di installazione predefinite di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nel percorso **C:\Programmi\Microsoft SQL Server\130\DTS** è presente questo set di cartelle. Se invece si crea un programma di installazione per l'oggetto personalizzato, è necessario controllare il valore della chiave del Registro di sistema **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\Setup\DtsPath** per verificare il percorso della cartella.  
  
> [!NOTE]  
>  Per informazioni su come distribuire i componenti personalizzati per un uso efficiente con il supporto di più versioni in SQL Server Data Tools, vedere [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/) (Supportare le estensioni SSIS personalizzate con il supporto di più versioni di SSDT 2015 per SQL Server 2016).  
  
 È possibile inserire l'assembly nella cartella in due modi:  
  
-   Spostare o copiare l'assembly compilato nella cartella appropriata dopo averlo compilato. Per praticità, è possibile includere il comando di copia in un evento di post-compilazione.  
  
-   Compilare l'assembly direttamente nella cartella appropriata.  
  
 Per i vari tipi di oggetti personalizzati, vengono usate le cartelle di distribuzione presenti nel percorso **C:\Programmi\Microsoft SQL Server\130\DTS**:  
  
|Oggetto personalizzato|Cartella di distribuzione|  
|-------------------|-----------------------|  
|Attività|Attività|  
|Gestione connessione|Connessioni|  
|Provider di log|LogProviders|  
|Componente del flusso di dati|PipelineComponents|  
  
> [!NOTE]  
>  Gli assembly vengono copiati in queste cartelle per supportare l'enumerazione degli oggetti personalizzati disponibili, quali attività, gestioni connessioni e così via. Pertanto, non è necessario distribuire in queste cartelle gli assembly che contengono solo l'interfaccia utente personalizzata per gli oggetti personalizzati.  
  
##  <a name="installing-the-assembly-in-the-global-assembly-cache"></a><a name="installing"></a>Installazione dell'assembly nella Global Assembly Cache  
 Per installare l'assembly attività nella Global Assembly Cache, usare lo strumento da riga di comando **gacutil.exe** oppure trascinare gli assembly nella directory `%system%\assembly`. Per praticità, è anche possibile includere la chiamata a **gacutil.exe** in un evento di post-compilazione.  
  
 Il comando seguente installa un componente denominato *MyTask.dll* nella Global Assembly Cache tramite **gacutil.exe**.  
  
 `gacutil /iF MyTask.dll`  
  
 Dopo l'installazione di una nuova versione dell'oggetto personalizzato, è necessario chiudere e riaprire Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Se sono state installate versioni precedenti dell'oggetto personalizzato nella Global Assembly Cache, è necessario rimuoverle prima di installare la nuova versione. Per disinstallare un assembly, eseguire **gacutil.exe** e specificare il nome dell'assembly con l'opzione `/u`.  
  
 Per ulteriori informazioni sulla Global Assembly Cache, vedere lo strumento corrispondente (Gactutil.exe) in [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Tools.  
  
##  <a name="troubleshooting-the-deployment"></a><a name="troubleshooting"></a> Risoluzione dei problemi della distribuzione  
 Se l'oggetto personalizzato è visualizzato nella **casella degli strumenti** o nell'elenco di oggetti disponibili ma non è possibile aggiungerlo a un pacchetto, provare a effettuare le operazioni seguenti:  
  
1.  Verificare se nella Global Assembly Cache sono disponibili più versioni del componente. In caso affermativo, è possibile che la finestra di progettazione non sia in grado di caricare il componente. Eliminare tutte le istanze dell'assembly dalla Global Assembly Cache, quindi aggiungere nuovamente l'assembly.  
  
2.  Verificare che nella cartella di distribuzione esista una singola istanza dell'assembly.  
  
3.  Aggiornare la casella degli strumenti.  
  
4.  Connettere [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] a **devenv.exe** e impostare un punto di interruzione per eseguire le istruzioni del codice di inizializzazione e assicurarsi che non si verifichino eccezioni.  
  
##  <a name="testing-and-debugging-your-code"></a><a name="testing"></a> Test e debug del codice  
 L'approccio più semplice per l'esecuzione del debug dei metodi di runtime di un oggetto personalizzato consiste nell'avviare **dtexec.exe** da [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] dopo aver compilato l'oggetto e nell'eseguire quindi un pacchetto che usa il componente.  
  
 Se si vuole eseguire il debug dei metodi della fase di progettazione del componente, ad esempio il metodo **Validate**, aprire un pacchetto che usa il componente in una seconda istanza di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e connettersi al relativo processo **devenv.exe**.  
  
 Se si vuole eseguire il debug anche dei metodi di runtime del componente quando un pacchetto è aperto e in esecuzione in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)], è necessario forzare una pausa nell'esecuzione del pacchetto in modo che sia possibile connettersi anche al processo **DtsDebugHost.exe**.  
  
#### <a name="to-debug-an-objects-run-time-methods-by-attaching-to-dtexecexe"></a>Per eseguire il debug dei metodi di runtime di un oggetto tramite connessione a dtexec.exe  
  
1.  Firmare e compilare il progetto nella configurazione di debug, distribuirlo e installarlo nella Global Assembly Cache come descritto in questo argomento.  
  
2.  Nella scheda **Debug** di **Proprietà progetto** selezionare **Avvia programma esterno** come **Azione di avvio** e individuare **dtexec.exe**, installato per impostazione predefinita in C:\Programmi\Microsoft SQL Server\130\DTS\Binn.  
  
3.  Nella casella di testo **Opzioni della riga di comando** in **Opzioni di avvio** immettere gli argomenti della riga di comando richiesti per eseguire un pacchetto in cui viene usato il componente. In genere l'argomento della riga di comando sarà costituito dall'opzione /F[ILE] seguita dal percorso e dal nome del file con estensione dtsx. Per altre informazioni, vedere [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
4.  Impostare i punti di interruzione nel codice sorgente, laddove appropriato, nei metodi di runtime del componente.  
  
5.  Eseguire il progetto.  
  
#### <a name="to-debug-a-custom-objects-design-time-methods-by-attaching-to-sql-server-data-tools"></a>Per eseguire il debug dei metodi della fase di progettazione di un oggetto personalizzato tramite connessione a SQL Server Data Tools  
  
1.  Firmare e compilare il progetto nella configurazione di debug, distribuirlo e installarlo nella Global Assembly Cache come descritto in questo argomento.  
  
2.  Impostare i punti di interruzione nel codice sorgente, laddove appropriato, nei metodi della fase di progettazione dell'oggetto personalizzato.  
  
3.  Aprire una seconda istanza di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e caricare un progetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenente un pacchetto in cui è utilizzato l'oggetto personalizzato.  
  
4.  Dalla prima istanza di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], connettersi alla seconda istanza di **devenv.exe** in cui è caricato il pacchetto scegliendo **Connetti a processo** dal menu **Debug** della prima istanza.  
  
5.  Eseguire il pacchetto dalla seconda istanza di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
#### <a name="to-debug-a-custom-objects-run-time-methods-by-attaching-to-sql-server-data-tools"></a>Per eseguire il debug dei metodi di runtime di un oggetto personalizzato tramite connessione a SQL Server Data Tools  
  
1.  Dopo avere completato i passaggi descritti nella procedura precedente, forzare una pausa nell'esecuzione del pacchetto in modo che sia possibile connettersi a **DtsDebugHost.exe**. Per forzare questa pausa, è possibile aggiungere un punto di interruzione all'evento **OnPreExecute** oppure aggiungere un'attività Script al progetto e passare allo script che visualizza una finestra di messaggio modale.  
  
2.  Eseguire il pacchetto. Quando si verifica la pausa, passare all'istanza di [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] in cui è aperto il progetto di codice e scegliere **Connetti a processo** dal menu **Debug**. Assicurarsi di connettersi all'istanza di **DtsDebugHost.exe** riportata come **Gestito, x86** nella colonna **Tipo** e non all'istanza indicata solo come **x86**.  
  
3.  Tornare al pacchetto in pausa e continuare oltre il punto di interruzione oppure fare clic su **OK** per annullare la finestra di messaggio generata dall'attività Script, quindi continuare con l'esecuzione e il debug del pacchetto.  
  
## <a name="see-also"></a>Vedere anche  
 [Sviluppo di oggetti personalizzati per Integration Services](../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)   
 [Persistenza degli oggetti personalizzati](../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)   
 [Strumenti per la risoluzione dei problemi di sviluppo di pacchetti](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
  
  
