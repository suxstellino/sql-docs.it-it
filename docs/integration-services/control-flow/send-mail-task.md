---
description: Invia messaggi - attività
title: Attività Invia messaggi| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sendmailtask.f1
- sql13.dts.designer.sendmailtask.general.f1
- sql13.dts.designer.sendmailtask.mail.f1
helpviewer_keywords:
- mail [Integration Services]
- Send Mail task
- e-mail [Integration Services]
- messages [Integration Services]
- sending messages
ms.assetid: fe0b7cbc-fe8e-4fe2-95b4-2953efff5869
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 362da93d1048564812826fceb3c0f2e76a47977a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348283"
---
# <a name="send-mail-task"></a>Invia messaggi - attività

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'attività Invia messaggi consente di inviare un messaggio di posta elettronica. Tramite l'attività Invia messaggi un pacchetto può inviare messaggi quando le attività nel flusso di lavoro del pacchetto vengono completate o non riescono oppure in risposta a un evento generato dal pacchetto in fase di esecuzione. È ad esempio possibile utilizzare questa attività per notificare all'amministratore di un database l'esito positivo o negativo dell'attività Backup database.  
  
 Per configurare l'attività Invia messaggi, procedere nel modo seguente:  
  
-   Specificare il testo del messaggio di posta elettronica.  
  
-   Specificare l'oggetto del messaggio di posta elettronica.  
  
-   Impostare il livello di priorità del messaggio. L'attività supporta tre livelli di priorità: normale, medio e alto.  
  
-   Specificare i destinatari nelle righe A, Cc e Ccn. Per specificare più destinatari, separarli con punti e virgola.  
  
    > [!NOTE]  
    >  Le righe A, Cc e Ccn presentano il limite di 256 caratteri ognuna, in conformità agli standard Internet.  
  
-   Includere eventuali allegati. Per specificare più allegati, separarli con una barra verticale (|).  
  
    > [!NOTE]  
    >  Se un file allegato non esiste al momento dell'esecuzione del pacchetto, verrà generato un errore.  
  
-   Specificare la gestione connessione SMTP da utilizzare.  
  
    > [!IMPORTANT]  
    >  La gestione connessione SMTP supporta solo l'autenticazione anonima e l'autenticazione di Windows. Non supporta l'autenticazione di base.  
  
 Il testo del messaggio può essere una stringa specificata, una connessione a un file che contiene il testo o il nome di una variabile che contiene il testo. Per connettersi a un file l'attività utilizza una gestione connessione file. Per ulteriori informazioni, vedere [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 Per connettersi a un server di posta elettronica l'attività utilizza una gestione connessione SMTP. Per altre informazioni, vedere [Gestione connessione SMTP](../../integration-services/connection-manager/smtp-connection-manager.md).  
  
## <a name="custom-logging-messages-available-on-the-send-mail-task"></a>Messaggi di registrazione personalizzati disponibili nell'attività Invia messaggi  
 Nella tabella seguente sono elencate le voci di log personalizzate disponibili per l'attività Invia messaggi. Per altre informazioni, vedere [registrazione di Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Voce di log|Descrizione|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Indica che l'attività ha iniziato a inviare un messaggio di posta elettronica.|  
|**SendMailTaskEnd**|Indica che l'attività ha terminato l'invio di un messaggio di posta elettronica.|  
|**SendMailTaskInfo**|Offre informazioni descrittive sull'attività.|  
  
## <a name="configuring-the-send-mail-task"></a>Configurazione dell'attività Invia messaggi  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic sull'argomento seguente:  
  
-   [Pagina Espressioni](../../integration-services/expressions/expressions-page.md)  
  
 Per informazioni sull'impostazione di queste proprietà a livello di codice, fare clic sull'argomento seguente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.SendMailTask.SendMailTask>  
  
## <a name="related-tasks"></a>Attività correlate  
 Per informazioni su come impostare queste proprietà nella finestra di Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , fare clic su [Impostazione delle proprietà di un'attività o di un contenitore](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="related-content"></a>Contenuto correlato  
  
-   Articolo tecnico relativo all' [invio della posta elettronica con notifica di recapito in C#](https://go.microsoft.com/fwlink/?LinkId=237625)su shareourideas.com  
  
## <a name="send-mail-task-editor-general-page"></a>Editor attività Invia messaggi (pagina Generale)
  Utilizzare la pagina **Generale** della finestra di dialogo **Editor attività Invia messaggi** per assegnare un nome all'attività Invia messaggi e descriverla.  
  
### <a name="options"></a>Opzioni  
 **Nome**  
 Consente di specificare un nome univoco per l'attività Invia messaggi. Tale nome viene utilizzato come etichetta nell'icona dell'attività.  
  
 **Nota** i nomi delle attività devono essere univoci all'interno di un pacchetto.  
  
 **Descrizione**  
 Consente di digitare una descrizione dell'attività Invia messaggi.  
  
## <a name="send-mail-task-editor-mail-page"></a>Editor attività Invia messaggi (pagina Messaggio)
  Usare la pagina **Messaggio** della finestra di dialogo **Editor attività Invia messaggi** per specificare i destinatari, il tipo di messaggio e la priorità relativi a un messaggio. È inoltre possibile allegare file al messaggio. Il testo del messaggio può essere una stringa specificata, una connessione a un file che contiene il testo o il nome di una variabile che contiene il testo.  
  
### <a name="options"></a>Opzioni  
 **SMTPConnection**  
 Selezionare una gestione connessione SMTP nell'elenco oppure fare clic su **\<New connection...>** per creare una nuova gestione connessione.  
  
> [!IMPORTANT]  
>  La gestione connessione SMTP supporta solo l'autenticazione anonima e l'autenticazione di Windows. Non supporta l'autenticazione di base.  
  
 **Argomenti correlati:** [Gestione connessione SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)  
  
 **From**  
 Consente di specificare l'indirizzo di posta elettronica del mittente.  
  
 **To**  
 Consente di specificare gli indirizzi di posta elettronica dei destinatari, separati da punti e virgola.  
  
 **Cc**  
 Consente di specificare gli indirizzi di posta elettronica, separati da punti e virgola, di altri destinatari che riceveranno copie del messaggio.  
  
 **Bcc**  
 Consente di specificare gli indirizzi di posta elettronica, separati da punti e virgola, di destinatari in copia nascosta che riceveranno copie del messaggio.  
  
 **Oggetto**  
 Consente di specificare l'oggetto del messaggio di posta elettronica.  
  
 **MessageSourceType**  
 Consente di selezionare il tipo di origine del messaggio. Per questa proprietà sono disponibili le opzioni elencate nella tabella seguente.  
  
|valore|Descrizione|  
|-----------|-----------------|  
|**Input diretto**|Consente di impostare l'origine sul testo del messaggio. Selezionando questo valore, verrà visualizzata l'opzione dinamica **MessageSource**.|  
|**Connessione file**|Consente di impostare l'origine sul file contenente il testo del messaggio. Selezionando questo valore, verrà visualizzata l'opzione dinamica **MessageSource**.|  
|**Variabile**|Consente di impostare l'origine su una variabile contenente il testo del messaggio. Selezionando questo valore, verrà visualizzata l'opzione dinamica **MessageSource**.|  
  
 **Priorità**  
 Consente di impostare il livello di priorità del messaggio.  
  
 **Allegati**  
 Consente di specificare i nomi file degli allegati del messaggio di posta elettronica, separati dal carattere di barra verticale (|).  
  
> [!NOTE]  
>  Le righe A, Cc e Ccn sono limitate a 256 caratteri, in conformità con gli standard Internet.  
  
### <a name="messagesourcetype-dynamic-options"></a>Opzioni dinamiche MessageSourceType  
  
#### <a name="messagesourcetype--direct-input"></a>MessageSourceType = Input diretto  
 **MessageSource**  
 Digitare il testo del messaggio oppure fare clic sul pulsante Sfoglia (...), quindi digitare il messaggio nella finestra di dialogo **Origine messaggio**.  
  
#### <a name="messagesourcetype--file-connection"></a>MessageSourceType = Connessione file  
 **MessageSource**  
 Selezionare una gestione connessione file nell'elenco oppure fare clic su \<**New connection...**> per creare una nuova gestione connessione.  
  
 **Argomenti correlati:** [Gestione connessione file](../../integration-services/connection-manager/file-connection-manager.md), [Editor gestione connessione file](../connection-manager/file-connection-manager.md)  
  
#### <a name="messagesourcetype--variable"></a>MessageSourceType = Variabile  
 **MessageSource**  
 Selezionare una variabile nell'elenco oppure fare clic su \<**New variable...**> per crearne una nuova.  
  
 **Argomenti correlati:** [Variabili di Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Aggiungere una variabile](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Attività di Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flusso di controllo](../../integration-services/control-flow/control-flow.md)  
  
