---
title: Registrare un nome dell'entità servizio (SPN) per un server di report | Microsoft Docs
description: Informazioni su come creare un nome dell'entità servizio (SPN) per il servizio del server di report, se questo viene eseguito come utente di dominio e se la rete usa Kerberos per l'autenticazione.
ms.date: 09/24/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: dda91d4f-77cc-4898-ad03-810ece5f8e74
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 38985454ae83a73fd27dac886fd0f4ee10e5ad55
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125587"
---
# <a name="register-a-service-principal-name-spn-for-a-report-server"></a>Registrare un nome dell'entità servizio (SPN) per un server di report
  Se si distribuisce [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in una rete che utilizza il protocollo Kerberos per l'autenticazione reciproca, è necessario creare un nome dell'entità servizio (SPN) per il server di report se questo è configurato per l'esecuzione come account utente di dominio.  
  
## <a name="about-spns"></a>Informazioni sugli SPN  
 Un SPN è un identificatore univoco per un servizio in una rete che utilizza l'autenticazione Kerberos. È costituito da una classe di servizio, un nome host e talvolta una porta. I nomi dell'entità servizio HTTP non richiedono una porta. In una rete che utilizza l'autenticazione Kerberos un SPN per il server deve essere registrato con un account computer predefinito, ad esempio NetworkService o LocalSystem, o un account utente. Gli SPN vengono registrati automaticamente per gli account predefiniti. Quando, tuttavia, si esegue un servizio utilizzando un account utente di dominio, è necessario registrare manualmente l'SPN per l'account che si desidera utilizzare.  
  
 Per creare un SPN, è possibile usare l'utilità della riga di comando **SetSPN** . Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Setspn](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) (https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)).  
  
-   [Service Principal Names (SPNs) SetSPN Syntax (Setspn.exe)](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx) (Sintassi SetSPN dei nomi dell'entità servizio) (https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).  
  
 Per eseguire l'utilità nel controller di dominio, è necessario essere un amministratore di dominio.  
  
## <a name="syntax"></a>Sintassi  

Quando si modificano i nomi SPN con SetSPN, il nome SPN deve essere immesso nel formato corretto. Il formato di un nome SPN HTTP è `http/host`. La sintassi del comando per l'utilizzo dell'utilità SetSPN per la creazione di un SPN per il server di report è analoga alla seguente:  
  
```  
Setspn -s http/<computer-name>.<domain-name> <domain-user-account>  
```  
  
 **SetSPN** è disponibile in Windows Server. L'argomento **-s** aggiunge un nome SPN dopo avere verificato che non sono presenti duplicati. **NOTA:** -s è disponibile in Windows Server a partire da Windows Server 2008.  
  
 **HTTP** è la classe del servizio. Il servizio Web ReportServer viene eseguito in HTTP.SYS. Una conseguenza della creazione di un SPN per HTTP è che a tutte le applicazioni Web presenti nello stesso computer ed eseguite in HTTP.SYS, incluse le applicazioni ospitate in IIS, verranno concessi ticket basati sull'account utente di dominio. Se tali servizi vengono eseguiti con un account diverso, le richieste di autenticazione avranno esito negativo. Per evitare questo problema, assicurarsi di configurare tutte le applicazioni HTTP per l'esecuzione con lo stesso account oppure creare intestazioni host per ogni applicazione e quindi SPN distinti per ciascuna intestazione host. Quando si configurano le intestazioni host, è necessario apportare le modifiche DNS indipendentemente dalla configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 I valori specificati per \<*computername*> e \<*domainname*> identificano l'indirizzo di rete univoco del computer che ospita il server di report. Può trattarsi di un nome host locale o di un nome di dominio completo (FQDN). Se è presente un solo dominio, è possibile omettere \<*domainname*> dalla riga di comando. \<*domain-user-account*> è l'account utente usato per l'esecuzione del servizio del server di report e per la registrazione dell'SPN.  
  
## <a name="register-an-spn-for-domain-user-account"></a>Registrare un SPN per l'account utente di dominio  
  
### <a name="to-register-an-spn-for-a-report-server-service-running-as-a-domain-user"></a>Per registrare un SPN per un servizio del server di report eseguito come utente di dominio  
  
1.  Installare [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e configurare il servizio del server di report per l'esecuzione come account utente di dominio. Si noti che gli utenti non saranno in grado di connettersi al server di report fino a quando non vengono completati i passaggi seguenti.  
  
2.  Accedere al controller di dominio come amministratore di dominio.  
  
3.  Aprire una finestra del prompt dei comandi.  
  
4.  Copiare il comando seguente, sostituendo i valori segnaposto con quelli effettivi, validi per la rete in uso:  
  
    ```  
    Setspn -s http/<computer-name>.<domain-name> <domain-user-account>  
    ```  
  
    Ad esempio: `Setspn -s http/MyReportServer.MyDomain.com MyDomainUser`  
  
5.  Eseguire il comando.  
  
6.  Aprire il file **RsReportServer.config** e individuare la sezione `<AuthenticationTypes>`.  
  
7.  Aggiungere `<RSWindowsNegotiate />` come prima voce in questa sezione per abilitare Kerberos.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare un account del servizio &#40;Gestione configurazione del server di report&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Configurare l'account del servizio del server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Gestire un server di report in modalità nativa di Reporting Services](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)  
  
