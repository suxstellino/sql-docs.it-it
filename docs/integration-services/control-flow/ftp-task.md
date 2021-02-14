---
description: Attività FTP
title: Attività FTP | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftptask.f1
- sql13.dts.designer.ftptask.general.f1
- sql13.dts.designer.ftptask.filetransfer.f1
helpviewer_keywords:
- FTP task [Integration Services]
ms.assetid: 41c3f2c4-ee04-460a-9822-bb9ae4036c2e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 839f9cc803dfcfe6c753851e53f0db1f2ef14daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348965"
---
# <a name="ftp-task"></a>Attività FTP

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività FTP consente di caricare e scaricare file di dati, nonché di gestire directory nei server. Un pacchetto può ad esempio scaricare file di dati da un server remoto o da un indirizzo Internet nell'ambito del flusso di lavoro di un pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . È possibile utilizzare l'attività FTP per gli scopi seguenti:  
  
-   Copia di directory e file di dati da una directory all'altra, prima o dopo lo spostamento dei dati, e applicazione di trasformazioni ai dati.  
  
-   Accesso a un percorso FTP di origine e copia di file o pacchetti in una directory di destinazione.  
  
-   Download di file da un percorso FTP e applicazione di trasformazioni ai dati delle colonne prima del caricamento dei dati in un database.  
  
 In fase di esecuzione l'attività FTP si connette a un server tramite una gestione connessione FTP configurata separatamente, a cui viene fatto riferimento dall'attività FTP. La gestione connessione FTP include le impostazioni del server, le credenziali per l'accesso al server FTP e opzioni quali il timeout e il numero dei tentativi consentiti per la connessione al server. Per altre informazioni, vedere [Gestione connessione FTP](../../integration-services/connection-manager/ftp-connection-manager.md).  
  
> [!IMPORTANT]  
>  La gestione connessione FTP supporta solo l'autenticazione anonima e l'autenticazione di base. Non supporta l'autenticazione di Windows.  
  
 Per l'accesso a un file o a una directory locale, l'attività FTP utilizza un percorso archiviato in una variabile o una gestione connessione file. Per l'accesso a un file o a una directory remota, invece, l'attività FTP utilizza un percorso archiviato in una variabile oppure specificato direttamente sul server remoto, come indicato nella gestione connessione FTP. Per altre informazioni, vedere [Gestione connessione File](../../integration-services/connection-manager/file-connection-manager.md) e [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Questo significa che l'attività FTP può ricevere più file ed eliminare più file remoti, ma se utilizza una gestione connessione può inviare ed eliminare un solo file locale alla volta, poiché una gestione connessione file può accedere a un solo file. Per accedere a più file locali, è necessario specificarne il percorso utilizzando una variabile. Una variabile contenente "C:\Test\&#42;.txt" specifica ad esempio un percorso per l'eliminazione o l'invio di tutti i file con estensione txt presenti nella directory Test.  
  
 In alternativa, per inviare più file e accedere a più file e directory locali è possibile eseguire più volte l'attività FTP includendola in un ciclo Foreach, che è in grado di eseguire un'enumerazione su tutti i file in una directory tramite l'enumeratore For Each File. Per altre informazioni, vedere [Contenitore Ciclo Foreach](../../integration-services/control-flow/foreach-loop-container.md).  
  
 L'attività FTP supporta i caratteri jolly punto interrogativo *?* e asterisco *\** nei percorsi. e questo consente di accedere a più file. I caratteri jolly possono essere tuttavia utilizzati solo nella parte del percorso che specifica il nome del file. Ad esempio, C:\Directory\\*.txt è un percorso valido, mentre C:\\\*Testo.txt non lo è.  
  
 Le operazioni FTP possono essere configurate in modo da arrestare l'attività File system se l'operazione non riesce oppure in modo da trasferire i file in modalità ASCII. Le operazioni che inviano e ricevono copie di file possono essere configurate in modo da sovrascrivere i file e le directory di destinazione.  
  
## <a name="predefined-ftp-operations"></a>Operazioni FTP predefinite  
 L'attività FTP include un set predefinito di operazioni, descritte nella tabella seguente.  
  
|Operazione|Descrizione|  
|---------------|-----------------|  
|Invia file|Invia un file dal computer locale al server FTP.|  
|Ricevi file|Salva sul computer locale un file scaricato dal server FTP.|  
|Crea directory locale|Crea una cartella sul computer locale.|  
|Crea directory remota|Crea una cartella sul server FTP.|  
|Rimuovi directory locale|Elimina una cartella dal computer locale.|  
|Rimuovi directory remota|Elimina una cartella dal server FTP.|  
|Elimina file locali|Consente di eliminare un file nel computer locale.|  
|Elimina file remoti|Consente di eliminare un file nel server FTP.|  
  
## <a name="custom-log-entries-available-on-the-ftp-task"></a>Voci di log personalizzate disponibili nell'attività FTP  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività FTP. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Indica che l'attività ha stabilito una connessione al server FTP.|  
|**FTPOperation**|Specifica l'inizio e il tipo dell'operazione FTP eseguita dall'attività.|  
  
## <a name="related-tasks"></a>Attività correlate  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per informazioni su come impostare queste proprietà nella finestra di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere [Impostare le proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
 Per altre informazioni sull'impostazione di queste proprietà a livello di programmazione, vedere <xref:Microsoft.SqlServer.Dts.Tasks.FtpTask.FtpTask>.  
  
## <a name="ftp-task-editor-general-page"></a>Editor attività FTP (pagina Generale)
  Utilizzare la pagina **Generale** della finestra di dialogo **Editor attività FTP** per specificare la gestione connessione FTP tramite cui viene stabilita la connessione al server FTP con cui comunica l'attività. È inoltre possibile specificare un nome e una descrizione per l'attività FTP.  
  
### <a name="options"></a>Opzioni  
 **FtpConnection**  
 Consente di selezionare una gestione connessione FTP esistente o di creare una gestione connessione facendo clic su \<**New connection...**>.  
  
> [!IMPORTANT]  
>  La gestione connessione FTP supporta solo l'autenticazione anonima e l'autenticazione di base. Non supporta l'autenticazione di Windows.  
  
 **Argomenti correlati**: [Gestione connessione FTP](../../integration-services/connection-manager/ftp-connection-manager.md), [Editor gestione connessione FTP](../connection-manager/ftp-connection-manager.md)  
  
 **StopOnFailure**  
 Consente di specificare il termine dell'attività FTP in caso di esito negativo di un'operazione FTP.  
  
 **Nome**  
 Consente di specificare un nome univoco per l'attività FTP. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
> [!NOTE]  
>  I nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività FTP.  
  
## <a name="ftp-task-editor-file-transfer-page"></a>Editor attività FTP (pagina Trasferimento file)
  Usare la pagina **Trasferimento file** della finestra di dialogo **Editor attività FTP** per configurare l'operazione FTP eseguita dall'attività.  
  
### <a name="options"></a>Opzioni  
 **IsRemotePathVariable**  
 Consente di specificare se il percorso remoto è archiviato in una variabile. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**True**|Il percorso di destinazione è archiviato in una variabile. Selezionando il valore viene visualizzata l'opzione dinamica **RemoteVariable**.|  
|**False**|Il percorso di destinazione è specificato in una gestione connessione file. Selezionando il valore viene visualizzata l'opzione dinamica **RemotePath**.|  
  
 **OverwriteFileAtDestination**  
 Consente di specificare se è possibile sovrascrivere un file nella destinazione.  
  
 **IsLocalPathVariable**  
 Consente di specificare se il percorso locale è archiviato in una variabile. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**True**|Il percorso di destinazione è archiviato in una variabile. Selezionando il valore viene visualizzata l'opzione dinamica **LocalVariable**.|  
|**False**|Il percorso di destinazione è specificato in una gestione connessione file. Selezionando il valore viene visualizzata l'opzione dinamica **LocalPath**.|  
  
 **Operazione**  
 Consente di selezionare l'operazione FTP da eseguire. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Invia file**|Consente di inviare i file. Selezionando questo valore vengono visualizzate le opzioni dinamiche **LocalVariable**, **LocalPathRemoteVariable** e **RemotePath**.|  
|**Ricevi file**|Consente di ricevere i file. Selezionando questo valore vengono visualizzate le opzioni dinamiche **LocalVariable**, **LocalPathRemoteVariable** e **RemotePath**.|  
|**Crea directory locale**|Consente di creare una directory locale. Selezionando questo valore vengono visualizzate le opzioni dinamiche **LocalVariable** e **LocalPath**.|  
|**Crea directory remota**|Consente di creare una directory remota. Selezionando questo valore vengono visualizzate le opzioni dinamiche **RemoteVariable** e **RemotelPath**.|  
|**Rimuovi directory locale**|Consente di rimuovere una directory locale. Selezionando questo valore vengono visualizzate le opzioni dinamiche **LocalVariable** e **LocalPath**.|  
|**Rimuovi directory remota**|Consente di rimuovere una directory remota. Selezionando questo valore vengono visualizzate le opzioni dinamiche **RemoteVariable** e **RemotePath**.|  
|**Elimina file locali**|Consente di eliminare file locali. Selezionando questo valore vengono visualizzate le opzioni dinamiche **LocalVariable** e **LocalPath**.|  
|**Elimina file remoti**|Consente di eliminare file remoti. Selezionando questo valore vengono visualizzate le opzioni dinamiche **RemoteVariable** e **RemotePath**.|  
  
 **IsTransferASCII**  
 Consente di specificare se i file ricevuti e inviati dal server FTP remoto devono essere trasferiti in modalità ASCII.  
  
### <a name="isremotepathvariable-dynamic-options"></a>Opzioni dinamiche di IsRemotePathVariable  
  
#### <a name="isremotepathvariable--true"></a>IsRemotePathVariable = True  
 **RemoteVariable**  
 Consente di selezionare una variabile esistente definita dall'utente o di creare una variabile facendo clic su \<**New variable...**>.  
  
 **Argomenti correlati:** [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Aggiungere una variabile  
  
#### <a name="isremotepathvariable--false"></a>IsRemotePathVariable = False  
 **RemotePath**  
 Consente di selezionare una gestione connessione FTP esistente o di creare una gestione connessione facendo clic su \<**New connection...**>.  
  
 **Argomenti correlati:** [Gestione connessione FTP](../../integration-services/connection-manager/ftp-connection-manager.md), [Editor gestione connessione FTP](../connection-manager/ftp-connection-manager.md)  
  
### <a name="islocalpathvariable-dynamic-options"></a>Opzioni dinamiche di IsLocalPathVariable  
  
#### <a name="islocalpathvariable--true"></a>IsLocalPathVariable = True  
 **LocalVariable**  
 Consente di selezionare una variabile esistente definita dall'utente o di creare una variabile facendo clic su \<**New variable...**>.  
  
 **Argomenti correlati:** [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Aggiungere una variabile  
  
#### <a name="islocalpathvariable--false"></a>IsLocalPathVariable = False  
 **LocalPath**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una gestione connessione.  
  
 **Argomenti correlati**: [Gestione connessione file flat](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Attività di Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flusso di controllo](../../integration-services/control-flow/control-flow.md)  
  
