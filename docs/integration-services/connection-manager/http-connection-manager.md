---
description: gestione connessione HTTP
title: Gestione connessione HTTP | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.httpconnection.server.f1
- sql13.dts.designer.httpconnection.proxy.f1
helpviewer_keywords:
- HTTP connection manager
- Web site connections [Integration Services]
- connection managers [Integration Services], HTTP
- Web server connections [Integration Services]
- connections [Integration Services], HTTP
ms.assetid: 26b2b3e1-d02c-46ca-8d31-7aef2bbc3c53
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cd88a7f40787ecc33d99fe0c749c477f23b526a3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "88496139"
---
# <a name="http-connection-manager"></a>gestione connessione HTTP

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Una connessione HTTP consente a un pacchetto di accedere al server Web utilizzando il protocollo HTTP per l'invio o la ricezione di file. Questa gestione connessione viene usata dall'attività Servizio Web inclusa in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Quando si aggiunge una gestione connessione HTTP a un pacchetto, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea una gestione connessione che in fase di esecuzione verrà risolta in una connessione HTTP, imposta le proprietà della gestione connessione e quindi la aggiunge alla raccolta **Connections** del pacchetto.  
  
 La proprietà **ConnectionManagerType** della gestione connessione viene impostata su **HTTP**.  
  
 Per configurare la gestione connessione HTTP, procedere nel modo seguente:  
  
-   Utilizzo di credenziali. Se la gestione connessione utilizza credenziali, le sue proprietà includeranno nome utente, password e dominio.  
  
    > [!IMPORTANT]  
    >  La gestione connessione HTTP supporta solo l'autenticazione anonima e l'autenticazione di base. Non supporta l'autenticazione di Windows.  
  
-   Utilizzo di un certificato client. Se la gestione connessione utilizza un certificato client, le sue proprietà includeranno il nome del certificato.  
  
-   Specificare il timeout per la connessione al server e le dimensioni del blocco per la scrittura dei dati.  
  
-   Utilizzo di un server proxy. È inoltre possibile configurare il server proxy in modo da utilizzare credenziali e da ignorare il server proxy e utilizzare indirizzi locali.  
  
## <a name="configuration-of-the-http-connection-manager"></a>Configurazione della gestione connessione HTTP  
 È possibile impostare le proprietà tramite Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
 Per informazioni sulla configurazione di una gestione connessione a livello di codice, vedere <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>.  
  
## <a name="http-connection-manager-editor-server-page"></a>Editor gestione connessione HTTP (pagina Server)
  La scheda **Server** della finestra di dialogo **Editor gestione connessione HTTP** consente di configurare la gestione connessione HTTP specificando proprietà quali l'URL e le credenziali di sicurezza. Una connessione HTTP consente a un pacchetto di accedere al server Web utilizzando il protocollo HTTP per l'invio o la ricezione di file. Dopo aver configurato la gestione connessione HTTP sarà inoltre possibile verificare la connessione.  
  
> [!IMPORTANT]  
>  La gestione connessione HTTP supporta solo l'autenticazione anonima e l'autenticazione di base. Non supporta l'autenticazione di Windows.  
  
 Per ulteriori informazioni sulla gestione connessione HTTP, vedere [HTTP Connection Manager](../../integration-services/connection-manager/http-connection-manager.md). Per ulteriori informazioni su uno scenario di utilizzo comune della gestione connessione HTTP, vedere [Web Service Task](../../integration-services/control-flow/web-service-task.md).  
  
### <a name="options"></a>Opzioni  
 **URL del server**  
 Digitare l'URL per il server.  
  
 Se si intende utilizzare il pulsante **Scarica WSDL** nella pagina **Generale** di **Editor attività Servizio Web** per scaricare un file WSDL, digitare l'URL del file WSDL. L'URL termina con "?wsdl".  
  
 **Usa credenziali**  
 Consente di specificare se si desidera che per la gestione connessione HTTP vengano utilizzate le credenziali di sicurezza dell'utente per l'autenticazione.  
  
 **Nome utente**  
 Se per la gestione connessione HTTP è stato impostato l'utilizzo di credenziali, è necessario specificare nome utente, password e dominio.  
  
 **Password**  
 Se per la gestione connessione HTTP è stato impostato l'utilizzo di credenziali, è necessario specificare nome utente, password e dominio.  
  
 **Dominio**  
 Se per la gestione connessione HTTP è stato impostato l'utilizzo di credenziali, è necessario specificare nome utente, password e dominio.  
  
 **Usa certificato client**  
 Consente di specificare se si desidera che per la gestione connessione HTTP venga utilizzato un certificato client per l'autenticazione.  
  
 **Certificate**  
 Consente di selezionare un certificato nell'elenco usando la finestra di dialogo **Seleziona certificato** . Nella casella di testo viene visualizzato il nome associato al certificato.  
  
 **Timeout (in secondi)**  
 Consente di fornire un valore di timeout per la connessione al server Web. Il valore predefinito di questa proprietà è 30 secondi.  
  
 **Dimensioni blocco (in KB)**  
 Consente di specificare le dimensioni del blocco per la scrittura dei dati.  
  
 **Test connessione**  
 Dopo aver configurato la gestione connessione HTTP, fare clic su **Test connessione** per assicurarsi che la connessione sia operativa.  
  
## <a name="http-connection-manager-editor-proxy-page"></a>Editor gestione connessione HTTP (pagina Proxy)
  La scheda **Proxy** della finestra di dialogo **Editor gestione connessione HTTP** consente di configurare Gestione connessione HTTP in modo che utilizzi un server proxy. Una connessione HTTP consente a un pacchetto di accedere al server Web utilizzando il protocollo HTTP per l'invio o la ricezione di file.  
  
 Per ulteriori informazioni sulla gestione connessione HTTP, vedere [HTTP Connection Manager](../../integration-services/connection-manager/http-connection-manager.md). Per ulteriori informazioni su uno scenario di utilizzo comune della gestione connessione HTTP, vedere [Web Service Task](../../integration-services/control-flow/web-service-task.md).  
  
### <a name="options"></a>Opzioni  
 **Usa proxy**  
 Consente di specificare se per la gestione connessione HTTP è necessario connettersi tramite un server proxy.  
  
 **URL proxy**  
 Consente di digitare l'URL del server proxy.  
  
 **Ignora proxy in locale**  
 Consente di specificare se per la gestione connessione HTTP è necessario ignorare il server proxy per gli indirizzi locali.  
  
 **Usa credenziali**  
 Consente di specificare se per la gestione connessione HTTP è necessario utilizzare le credenziali di sicurezza per il server proxy.  
  
 **Nome utente**  
 Se per la gestione connessione HTTP è stato impostato l'utilizzo di credenziali, è necessario specificare nome utente, password e dominio.  
  
 **Password**  
 Se per la gestione connessione HTTP è stato impostato l'utilizzo di credenziali, è necessario specificare nome utente, password e dominio.  
  
 **Dominio**  
 Se per la gestione connessione HTTP è stato impostato l'utilizzo di credenziali, è necessario specificare nome utente, password e dominio.  
  
 **Elenco proxy da ignorare**  
 Elenco degli indirizzi per i quali si desidera ignorare il server proxy.  
  
 **Aggiungere**  
 Consente di digitare un indirizzo per il quale si desidera ignorare il server proxy.  
  
 **Rimuovi**  
 Consente di selezionare un indirizzo e quindi di rimuoverlo facendo clic su **Rimuovi**.  
  
## <a name="see-also"></a>Vedere anche  
 [Attività Servizio Web](../../integration-services/control-flow/web-service-task.md)   
 [Connessioni in Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
