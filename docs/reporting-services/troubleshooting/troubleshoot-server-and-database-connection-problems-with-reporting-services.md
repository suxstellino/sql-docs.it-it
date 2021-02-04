---
title: Risolvere i problemi di connessione al server e al database
description: In questo articolo vengono diagnosticati e risolti i problemi che si verificano durante la connessione a un server di report. Vengono inoltre fornite informazioni sui messaggi di tipo "Errore imprevisto".
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: 8bbb88df-72fd-4c27-91b7-b255afedd345
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 74bf6f0da0daa0df248243ffbce505c75c55db5e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076323"
---
# <a name="troubleshoot-server--database-connection-problems-with-reporting-services"></a>Risolvere i problemi di connessione al server e al database con Reporting Services
Utilizzare questo argomento per la risoluzione dei problemi che si verificano durante la connessione a un server di report. In questo argomento vengono inoltre fornite informazioni sui messaggi di tipo "Errore imprevisto". Per altre informazioni sulla configurazione delle origini dati e di una connessione al server di report, vedere [Specificare le informazioni sulle credenziali e le connessioni per le origini dati dei report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md) e [Configurare una connessione del database del server di report (Gestione configurazione del server di report)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="cannot-create-a-connection-to-data-source-datasourcename-rserroropeningconnection"></a>Impossibile creare una connessione all'origine dei dati "datasourcename". (rsErrorOpeningConnection)  
Si tratta di un errore generico che si verifica quando il server di report non è in grado di aprire una connessione a un'origine dei dati esterna che fornisce dati a un report. Insieme a questo messaggio di errore viene visualizzato un altro messaggio di errore in cui viene indicata la causa sottostante. Insieme a **rsErrorOpeningConnection** possono essere visualizzati i messaggi di errore seguenti.  
  
### <a name="login-failed-for-user-username"></a>Accesso non riuscito per l'utente 'UserName'  
L'utente non dispone di autorizzazioni per accedere all'origine dei dati. Se si usa un database di SQL Server, verificare che l'utente disponga di un account di accesso valido per il database. Per altre informazioni su come creare un utente del database o un account di accesso di SQL Server, vedere [Creare un utente del database](../../relational-databases/security/authentication-access/create-a-database-user.md) e [Creare un account di accesso a SQL server](../../relational-databases/security/authentication-access/create-a-login.md).  
  
### <a name="login-failed-for-user-nt-authorityanonymous-logon"></a>Accesso non riuscito per l'utente 'NT AUTHORITY\ANONYMOUS LOGON'  
Questo errore si verifica quando le credenziali vengono passate tra più connessioni. Se si utilizza un'autenticazione di Windows e il protocollo Kerberos versione 5 non è abilitato, questo errore si verifica quando le credenziali vengono passate tra più di una connessione. Per risolvere il problema, provare a utilizzare credenziali archiviate o su richiesta. Per altre informazioni su come aggirare questo problema, vedere [Specificare le informazioni sulle credenziali e le connessioni per le origini dati dei report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
### <a name="an-error-has-occurred-while-establishing-a-connection-to-the-server"></a>Si è verificato un errore durante il tentativo di stabilire una connessione al server.  
Quando ci si connette a SQL Server, è possibile che l'errore sia determinato dal fatto che le impostazioni predefinite di SQL Server non consentono le connessioni remote. (provider: Provider named pipe, errore: 40 - Non è stato possibile aprire una connessione a SQL Server). Tale errore viene restituito dall'istanza del motore di database che ospita il database del server di report. Nella maggior parte dei casi questo errore si verifica poiché il servizio SQL Server è stato arrestato. Se si usa invece SQL Server Express with Advanced Services o un'istanza denominata, l'errore si verifica anche se la stringa di connessione per il database del server di report o l'URL del server di report non è corretto. Per risolvere questi problemi, effettuare le seguenti operazioni:  
  
* Verificare che il servizio SQL Server (**MSSQLSERVER**) sia stato avviato. Nel computer che ospita l'istanza del motore di database fare clic sul pulsante Start, scegliere Strumenti di amministrazione, fare clic su Servizi e scorrere fino a SQL Server (**MSSQLSERVER**). Se non è stato avviato, fare clic con il pulsante destro del mouse sul servizio, quindi scegliere Proprietà. In Tipo di avvio selezionare Automatico, fare clic su Applica, quindi su Avvia e infine scegliere OK.   
* Verificare che l'URL del server di report e la stringa di connessione al database del server di report siano corretti. Se Reporting Services o il motore di database è stato installato come un'istanza denominata, la stringa di connessione predefinita creata durante l'installazione includerà il nome dell'istanza. Se, ad esempio, è stata installata un'istanza predefinita di SQL Server Express with Advanced Services in un server denominato DEVSRV01, l'URL del portale Web sarà DEVSRV01\Reports$SQLEXPRESS. Il nome del server di database nella stringa di connessione, inoltre, sarà simile a DEVSRV01\SQLEXPRESS. Per altre informazioni sugli URL e le stringhe di connessione all'origine dati per SQL Server Express, vedere [Reporting Services in SQL Server Express with Advanced Services](/previous-versions/sql/sql-server-2008-r2/ms365166(v=sql.105)). Per verificare la stringa di connessione al database del server di report, avviare lo strumento di configurazione di Reporting Services e visualizzare la pagina Impostazioni database.  
  
### <a name="a-connection-cannot-be-made-ensure-that-the-server-is-running"></a>Impossibile stabilire la connessione. Verificare che il server sia in esecuzione.  
Questo errore viene restituito dal provider ADOMD.NET e può verificarsi per vari motivi. Se il server è stato indicato come "localhost", provare a specificare invece il nome del server. L'errore può inoltre verificarsi se non è possibile allocare memoria alla nuova connessione. Per altre informazioni, vedere l' [articolo della Knowledge Base 912017 - FIX: Messaggio di errore quando ci si connette a un'istanza di SQL Server 2005 Analysis Services:](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/912017).  
  
Se il messaggio di errore indica anche che l'host è sconosciuto, significa che il server Analysis Services non è disponibile o rifiuta la connessione. Se il server Analysis Services è installato come istanza denominata in un computer remoto, potrebbe essere necessario eseguire il servizio SQL Server Browser per ottenere il numero di porta utilizzato da tale istanza.  
  
### <a name="report-services-soap-proxy-source"></a>(Origine proxy SOAP Reporting Services)  
Se viene visualizzato questo errore durante la generazione del modello di report e la sezione delle informazioni aggiuntive include "Server SQL inesistente o accesso negato", è possibile che si siano verificate le condizioni seguenti:  
* La stringa di connessione per l'origine dati include "localhost".  
* Il protocollo TCP/IP è disabilitato per il servizio SQL Server.  
  
Per risolvere il problema, è possibile modificare la stringa di connessione per utilizzare il nome server o abilitare TCP/IP per il servizio. Per abilitare il protocollo TCP/IP, eseguire la procedura seguente:  
  
1. Avviare Gestione configurazione SQL Server.  
2. Espandere **Configurazione di rete SQL Server**.  
3. Selezionare **Protocolli per MSSQLSERVER**.  
4. Fare clic con il pulsante destro del mouse su **TCP/IP** e selezionare **Abilita**.  
5. Selezionare **Servizi di SQL Server**.  
6. Fare clic con il pulsante destro del mouse su **SQL Server (MSSQLSERVER**) e scegliere **Riavvia**.  
  
## <a name="wmi-error-when-connecting-to-a-report-server-in-management-studio"></a>Errore WMI durante la connessione a un server di report in Management Studio  
Per impostazione predefinita, Management Studio usa il provider di Strumentazione gestione Windows (WMI) di Reporting Services per stabilire una connessione al server di report. Se il provider di WMI non è installato correttamente, nel caso in cui si tenti di connettersi al server di report si verificherà l'errore seguente:  
  
Non è possibile connettersi a \<your server name>. Il provider WMI per Reporting Services non è installato o non è configurato in modo corretto (Microsoft.SqlServer.Management.UI.RSClient).  
  
Per risolvere questo errore, è necessario reinstallare il software. Per tutti gli altri casi, per risolvere temporaneamente il problema è possibile connettersi al server di report tramite l'endpoint SOAP.  
  
* Nella finestra di dialogo **Connetti al server** di Management Studio, in **Nome server** digitare l'URL del server di report. L'impostazione predefinita è `https://<your server name>/reportserver`. Oppure, se si usa SQL Server 2008 Express with Advanced Services, è `https://<your server name>/reportserver$sqlexpress`.  
  
Per risolvere l'errore in modo che sia possibile eseguire la connessione mediante il provider WMI, è necessario eseguire il programma di installazione per ripristinare Reporting Services o reinstallare Reporting Services.  
  
## <a name="connection-error-where-login-failed-due-to-unknown-user-name-or-bad-password"></a>Errore di connessione con impossibilità di accedere a causa dell'utilizzo di un nome utente sconosciuto o di una password non valida  
È possibile che si verifichi un errore **rsReportServerDatabaseLogonFailed** quando si usa un account di dominio per la connessione dal server di report alla connessione a un database del server di report e la password per l'account di dominio è stata modificata.   
  
Il testo dell'errore completo è: "Impossibile stabilire una connessione al database del server di report. Accesso non riuscito (**rsReportServerDatabaseLogonFailed**). Errore durante l'accesso: nome utente sconosciuto o password errata."  
  
Se si reimposta la password, è necessario aggiornare la connessione. Per altre informazioni, vedere [Configurare una connessione del database del server di report (Gestione configurazione del server di report)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="the-report-server-cannot-open-a-connection-to-the-report-server-database-rsreportserverdatabaseunavailable"></a>Impossibile stabilire una connessione al database del server di report. (rsReportServerDatabaseUnavailable).  
Messaggio completo: Impossibile stabilire una connessione al database del server di report. È necessaria una connessione al database per tutte le richieste e le elaborazioni. (rsReportServerDatabaseUnavailable)  
Questo errore si verifica quando il server di report non è in grado di connettersi al database relazionale di SQL Server che viene usato per l'archiviazione interna del server. La connessione al database del server di report viene gestita mediante lo strumento di configurazione di Reporting Services. È possibile eseguire tale strumento, passare alla pagina Impostazioni database e correggere le informazioni di connessione. Questa è una procedura consigliata per aggiornare le informazioni di connessione. L'utilizzo dello strumento assicura infatti l'aggiornamento delle impostazioni dipendenti e il riavvio dei servizi. Per altre informazioni, vedere [Configurare una connessione del database del server di report](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md) e [Configurare l'account del servizio del server di report](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
L'errore può inoltre verificarsi se l'istanza del motore di database che ospita il database del server di report non è configurata per le connessioni remote. In alcune edizioni di SQL Server la connessione remota è abilitata per impostazione predefinita. Per verificare se tale connessione è abilitata nell'istanza del motore di database di SQL Server in uso, eseguire lo strumento Gestione configurazione di SQL Server. È necessario abilitare sia il protocollo TCP/IP sia le named pipe. Un server di report utilizza entrambi i protocolli. Per istruzioni su come abilitare le connessioni remote, vedere la sezione "Come configurare le connessioni remote al database del server di report" in [Configurare un server di report per l'amministrazione remota](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).  
  
Se il messaggio di errore include il testo aggiuntivo seguente, significa che la password dell'account impiegato per eseguire l'istanza del motore di database è scaduta: "Si è verificato un errore durante il tentativo di stabilire una connessione al server. Quando ci si connette a SQL Server, è possibile che l'errore sia determinato dal fatto che le impostazioni predefinite di SQL Server non consentono le connessioni remote. (**provider: Interfacce di rete di SQL Server, errore: 26 - Errore nell'individuazione del server/dell'istanza specificata)** ". Per risolvere l'errore, reimpostare la password.   
  
## <a name="rpc-server-is-not-listening"></a>"Il server RPC non è in ascolto"  
Il servizio del server di report utilizza il server RPC (Remote Procedure Call) per alcune operazioni. Se viene visualizzato l'errore "Il server RPC non è in ascolto", verificare che il servizio del server di report sia in esecuzione.  
  
## <a name="unexpected-error-general-network-error"></a>Errore imprevisto (Errore generale di rete)  
Indica un errore di connessione all'origine dei dati. Controllare la stringa di connessione e assicurarsi di disporre dell'autorizzazione per accedere all'origine dei dati. Se si utilizza un'autenticazione di Windows per l'accesso a un'origine dei dati, è necessario disporre dell'autorizzazione per accedere al computer che ospita l'origine dei dati.  
  
## <a name="unable-to-grant-database-access-in-sharepoint-central-administration"></a>Non è possibile concedere l'accesso al database in Amministrazione centrale SharePoint  
Se Reporting Services è configurato per l'integrazione con una tecnologia o un prodotto SharePoint in Windows Vista o in Windows Server 2008, è possibile che venga visualizzato il messaggio di errore seguente quando si prova a concedere l'accesso nella pagina **Concedi accesso al database** in Amministrazione centrale SharePoint: "Impossibile stabilire una connessione al computer".  
  
Questo problema si verifica perché la funzionalità Controllo dell'account utente in Windows Vista e in Windows Server 2008 richiede l'accettazione esplicita da parte di un amministratore per elevare e usare il token dell'amministratore durante l'esecuzione di attività per cui sono necessarie autorizzazioni di amministratore. In questo caso, tuttavia, il servizio Amministrazione di Windows SharePoint Services non può essere elevato per consentire all'account o agli account del servizio Reporting Services di accedere ai database di configurazione e di contenuto di SharePoint.  
  
In SQL Server 2008 Reporting Services solo l'account del servizio del server di report richiede l'accesso al database, mentre in SQL Server 2005 Reporting Services SP2 l'accesso al database è necessario sia per l'account del servizio Windows ReportServer sia per l'account del servizio Web ReportServer. Per altre informazioni sull'account del servizio del server di report in SQL Server 2008, vedere Account del servizio (configurazione di Reporting Services).  
  
Per questo problema sono disponibili due soluzioni alternative.   
1.  Una soluzione prevede la disattivazione temporanea di Controllo dell'account utente e l'utilizzo di Amministrazione centrale SharePoint per concedere l'accesso.  
> [!IMPORTANT]  
> Procedere con cautela se si sceglie di disattivare Controllo dell'account utente per risolvere questo problema e riattivare immediatamente la funzionalità dopo avere concesso l'accesso al database in Amministrazione centrale SharePoint. Se non si desidera disattivare Controllo dell'account utente, utilizzare la seconda soluzione alternativa indicata in questa sezione. Per ulteriori informazioni su Controllo dell'account utente, vedere la documentazione dei prodotti Windows.  
2. La seconda soluzione consiste nel concedere manualmente l'accesso al database all'account o agli account del servizio Reporting Services. È possibile usare la procedura seguente per concedere l'accesso aggiungendo l'account o gli account del servizio Reporting Services al gruppo di Windows e ai ruoli del database corretti. Questa procedura si applica all'account del servizio del server di report in SQL Server 2008 Reporting Services; se si usa SQL Server 2005 Reporting Services, è necessario eseguire la procedura per l'account del servizio Windows ReportServer e per l'account del servizio Web ReportServer.   
  
### <a name="to-manually-grant-database-access"></a>Per concedere manualmente l'accesso al database  
  
1. Aggiungere l'account del servizio del server di report al gruppo di Windows WSS_WPG nel computer con Reporting Services.  
2. Connettersi all'istanza del database che ospita i database di configurazione e di contenuto di SharePoint e creare un account di accesso al database SQL per l'account del servizio del server di report.  
3. Aggiungere l'account di accesso al database SQL ai ruoli del database seguenti:  
  
* Ruolo db_owner nel database WSS_Content  
* Ruolo WSS_Content_Application_Pools nel database SharePoint_Config  
  
## <a name="unable-to-connect-to-the-reports-and-reportserver-directories-when-the-report-server-databases-are-created-on-a-virtual-sql-server-that-runs-in-a-microsoft-cluster-services-mscs-cluster"></a>Non è possibile connettersi alle directory /reports e /reportserver quando i database del server di report vengono creati in un server SQL Server virtuale in esecuzione in un cluster Microsoft Cluster Services (MSCS)  
Quando si creano i database del server di report, **ReportServer** e **ReportServerTempDB**, in un server SQL Server virtuale in esecuzione in un cluster MSCS, il nome remoto in formato `<domain>\<computer_name>$` potrebbe non essere registrato in SQL Server come account di accesso. Se l'account del servizio del server di report è stato configurato come account che richiede tale nome remoto per le connessioni, gli utenti non potranno connettersi alle directory /reports e /reportserver in Reporting Services. L'account di Windows NetworkService predefinito, ad esempio, richiede il nome remoto. Per evitare questo problema, utilizzare un account di dominio esplicito o un account di accesso di SQL Server per connettersi ai database del server di report.  
    
  ## <a name="see-also"></a>Vedere anche  
[Supporto browser per Reporting Services e Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[Errori ed eventi (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
[Risolvere i problemi di recupero dei dati con i report di Reporting Services](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Risolvere i problemi di sottoscrizioni e recapito di Reporting Services](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]
