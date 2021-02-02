---
title: Risolvere i problemi di connessione al motore di database di SQL Server
description: Informazioni su come risolvere i problemi di connessione. Consente di visualizzare i passaggi da eseguire quando non è possibile usare TCP/IP per connettersi a un motore di database di SQL Server in un singolo server.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: troubleshooting
helpviewer_keywords:
- troubleshooting, connecting to Database Engine
- connecting to Database Engine, troubleshooting
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: sqlfreshmay19
ms.date: 11/25/2019
ms.openlocfilehash: cd4dae49fb19532e9aee082e4046c4d79dbf0a82
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237999"
---
# <a name="troubleshoot-connecting-to-the-sql-server-database-engine"></a>Risolvere i problemi di connessione al motore di database di SQL Server

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo articolo elenca le tecniche di risoluzione dei problemi da usare quando non è possibile connettersi a un'istanza del motore di database di SQL Server in un server singolo.

>[!NOTE]
>Per altri scenari, vedere:
>- [Listener del gruppo di disponibilità](../availability-groups/windows/listeners-client-connectivity-application-failover.md)
>- [Istanze del cluster di failover](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

I passaggi non seguono l'ordine dei problemi più frequenti dei quali probabilmente sono già stati tentati i passaggi. I passaggi seguono invece un ordine basato sulla complessità, dai problemi di base ai problemi più complessi. Nei passaggi si presuppone che la connessione all'istanza di SQL Server venga stabilita da un altro computer usando il protocollo TCP/IP, che è la situazione più comune.

Le istruzioni specificate risultano particolarmente utili per la risoluzione dell'errore di **connessione al server**, ad esempio `Error Number: 11001 (or 53), Severity: 20, State: 0`. Di seguito è riportato un esempio di messaggio di errore:

> `A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.`
>
> `(provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server) (Microsoft SQL Server, Error: 53)`
>
> `(provider: TCP Provider, error: 0 - No such host is known.) (Microsoft SQL Server, Error: 11001)`

Questo errore indica in genere che il client non è in grado di trovare l'istanza di SQL Server. Questa situazione si verifica in genere quando è presente almeno uno dei problemi seguenti:

- Il nome del computer che ospita l'istanza di SQL Server
- non risolve l'IP corretto
- Il numero di porta TCP non è specificato correttamente

> [!TIP]
> Una pagina di risoluzione interattiva è disponibile dal Supporto Tecnico Microsoft [!INCLUDE[msCoName_md](../../includes/msconame-md.md)] in [Risoluzionedei problemi di connettività a SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server).

## <a name="not-included"></a>Errori non inclusi

- Questo argomento non include informazioni sugli errori SSPI. Per gli errori SSPI, vedere [Risoluzione del messaggio di errore "Impossibile generare il contesto SSPI"](https://support.microsoft.com/kb/811889).
- Questo argomento non include informazioni sugli errori Kerberos. Per altre informazioni, vedere [Microsoft Kerberos Configuration Manager per SQL Server](https://www.microsoft.com/download/details.aspx?id=39046).
- Questo argomento non include informazioni sulla connettività al database SQL di Azure. Per altre informazioni, vedere [Risoluzione dei problemi di connettività al database SQL di Microsoft Azure](/azure/sql-database/troubleshoot-connectivity-issues-microsoft-azure-sql-database).

## <a name="get-instance-name-from-configuration-manger"></a>Recuperare il nome dell'istanza da Gestione configurazione

Nel server che ospita l'istanza di SQL Server verificare il nome dell'istanza. Usare [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

La funzionalità Gestione configurazione viene installata automaticamente nel computer durante l'installazione di SQL Server. Le istruzioni per l'avvio di Gestione configurazione variano leggermente a seconda della versione di SQL Server e Windows. Per i dettagli specifici della versione, vedere [Gestione configurazione SQL Server](../../relational-databases/sql-server-configuration-manager.md).

1. Accedere al computer che ospita l'istanza di SQL Server.
2. Avviare Gestione configurazione SQL Server.
3. Nel riquadro sinistro selezionare **Servizi di SQL Server**.
4. Nel riquadro destro verificare il nome dell'istanza del motore di database.

   - `SQL SERVER (MSSQLSERVER)` indica un'istanza predefinita di SQL Server. Il nome dell'istanza predefinita è `<computer name>`.
   - `SQL SERVER (<instance name>)` indica un'istanza denominata di SQL Server. Il nome dell'istanza denominata è `<computer name>\<instance name>`.

## <a name="verify---the-instance-is-running"></a>Verificare che l'istanza sia in esecuzione.

Per verificare che l'istanza sia in esecuzione, in Gestione configurazione osservare il simbolo accanto all'istanza di SQL Server.

- Una freccia verde indica che un'istanza è in esecuzione.
- Un quadrato rosso indica che un'istanza è arrestata.

Se l'istanza è arrestata, fare clic con il pulsante destro del mouse sull'istanza e quindi scegliere **Avvia**. L'istanza del server viene avviata e l'indicatore mostra una freccia verde.

## <a name="verify---sql-server-browser-service-is-running"></a><a name = "startbrowser"></a> Verificare che il servizio SQL Server Browser sia in esecuzione

Per connettersi a un'istanza denominata, il servizio SQL Server Browser deve essere in esecuzione. In Gestione configurazione individuare il servizio **SQL Server Browser** e verificare che sia in esecuzione. In caso contrario, avviarlo. Il servizio SQL Server Browser non è necessario per le istanze predefinite.

Un'istanza predefinita di SQL Server non richiede il servizio SQL Server Browser.

## <a name="testing-a-local-connection"></a>Test di una connessione locale

Prima di risolvere un problema di connessione da un altro computer, verificare che sia possibile connettersi da un'applicazione client installata in locale nel computer che esegue SQL Server. La connessione locale consente di evitare problemi con reti e firewall. 

In questa procedura viene usato SQL Server Management Studio. Se Management Studio non è installato, vedere [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md). Se non è possibile installare Management Studio, provare a testare la connessione usando l'utilità `sqlcmd.exe`. L'utilità `sqlcmd.exe` viene installata con il motore di database. Per altre informazioni su `sqlcmd.exe`, vedere [Utilità sqlcmd](../../tools/sqlcmd-utility.md).

1. Accedere al computer in cui è installato SQL Server usando un account con autorizzazioni di accesso a SQL Server. Durante l'installazione, SQL Server richiede almeno un accesso come amministratore di SQL Server. Se non si conosce un amministratore, vedere [Connettersi a SQL Server se gli amministratori di sistema sono bloccati](connect-to-sql-server-when-system-administrators-are-locked-out.md).
2. Nella pagina iniziale digitare **SQL Server Management Studio** oppure nelle versioni precedenti di Windows nel menu Start scegliere **Tutti i programmi**, quindi scegliere **Microsoft SQL Server** e fare clic su **SQL Server Management Studio**.
3. Nella finestra di dialogo **Connetti al server** selezionare **Motore di database** nella casella **Tipo di server**. Nella casella **Autenticazione** selezionare **Autenticazione di Windows**. Nella casella **Nome server** digitare uno dei tipi di connessione seguenti:

   |Connessione a|Type|Esempio|
   |:-----------------|:---------------|:-----------------|
   |Istanza predefinita|`<computer name>`|`ACCNT27`|
   |Istanza denominata|`<computer name\instance name>`|`ACCNT27\PAYROLL`|

   > [!NOTE]
   > Per la connessione a SQL Server da un'applicazione client nello stesso computer viene usato il protocollo Shared Memory. Poiché Shared Memory è un tipo di named pipe locale, a volte si verificano errori relativi alle pipe.

   Se si verifica un errore in questa fase, è necessario risolverlo prima di continuare. Possono verificarsi diversi tipi di problemi. È possibile che l'account di accesso non sia autorizzato a connettersi. Il database predefinito potrebbe non essere disponibile.

   > [!NOTE]
   > Alcuni messaggi di errore passati al client non forniscono intenzionalmente informazioni sufficienti per risolvere il problema. Si tratta di una misura di sicurezza per evitare di fornire informazioni su SQL Server a un utente malintenzionato. Per visualizzare informazioni complete sull'errore, esaminare il log degli errori di SQL Server. Il log contiene informazioni dettagliate. 

4. Se viene restituito l'errore `18456 Login failed for user`, vedere l'argomento [MSSQLSERVER_18456](../../relational-databases/errors-events/mssqlserver-18456-database-engine-error.md) della documentazione online che include informazioni aggiuntive sui codici errore. Un blog di Aaron Bertrand include un elenco completo dei codici di errore in [Troubleshooting Error 18456](https://sqlblog.org/2011/01/14/troubleshooting-error-18456). Se si è in grado di stabilire la connessione, è possibile visualizzare il log degli errori con SSMS nella sezione Gestione di Esplora oggetti. In caso contrario, è possibile visualizzare il log degli errori con il Blocco note di Windows. Il percorso predefinito varia in base alla versione e può essere modificato durante l'installazione. Il percorso predefinito per [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] è `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Log\ERRORLOG`. 

5. Se è possibile connettersi tramite Shared Memory, testare la connessione usando TCP. È possibile forzare una connessione TCP specificando `tcp:` prima del nome. Ad esempio:

   |Connessione a:|Digitare:|Esempio:|
   |-----------------|---------------|-----------------|
   |Istanza predefinita|`tcp:<computer name>`|`tcp:ACCNT27`|
   |Istanza denominata|`tcp:<computer name/instance name>`|`tcp:ACCNT27\PAYROLL`|

6. Se è possibile connettersi con Shared Memory e non con TCP, è necessario correggere il problema TCP. La causa più probabile è che TCP non sia abilitato. Per abilitare TCP, vedere i passaggi descritti sopra in [Abilitare i protocolli](#enableprotocols).

7. Quando è possibile connettersi come amministratore, se si vuole stabilire la connessione con un account diverso dall'account di amministratore, provare a connettersi usando l'account di accesso con autenticazione di Windows o l'account di accesso con autenticazione di SQL Server dell'applicazione client.

## <a name="get-the-ip-address-of-the-server"></a>Ottenere l'indirizzo IP del server

Ottenere l'indirizzo IP del computer che ospita l'istanza di SQL Server.

1. Nel menu Start fare clic su **Esegui**. Nella finestra **Esegui** digitare **cmd** e quindi fare clic su **OK**.
2. Nella finestra del prompt dei comandi digitare `ipconfig` e quindi premere INVIO. Prendere nota dell'indirizzo **IPv4** e dell'indirizzo **IPv6** .

  >SQL Server può stabilire la connessione usando la versione 4 o la versione 6 del protocollo IP. È possibile che la rete supporti uno dei due protocolli o entrambi. Nella maggior parte dei casi la risoluzione dei problemi viene iniziata con l'indirizzo **IPv4** . È più breve e più facile da digitare.

## <a name="get-the-sql-server-instance-tcp-port"></a><a name = "getTCP"></a>Ottenere la porta TCP dell'istanza di SQL Server

Nella maggior parte dei casi si stabilisce la connessione al motore di database da un altro computer usando il protocollo TCP.

1. Usando SQL Server Management Studio nel computer che esegue SQL Server, connettersi all'istanza di SQL Server. In Esplora oggetti espandere **Gestione**, espandere **Log di SQL Server** e fare doppio clic sul log corrente.
2. Nel Visualizzatore log fare clic sul pulsante **Filtro** sulla barra degli strumenti. Nella casella **Testo contenuto nel messaggio** digitare `server is listening on`, fare clic su **Applica filtro** e quindi su **OK**.
3. Verrà visualizzato un messaggio simile al seguente: `Server is listening on [ 'any' <ipv4> 1433]`. 

Questo messaggio indica che l'istanza di SQL Server è in attesa su tutti gli indirizzi IP nel computer in uso (per la versione IP 4) ed è in attesa sulla porta TCP 1433. La porta TCP 1433 è in genere la porta usata dal motore di database o da un'istanza predefinita di SQL Server. Poiché una porta può essere usata da una sola istanza di SQL Server, in presenza di più istanze di SQL Server installate è necessario che alcune istanze usino altri numeri di porta. Prendere nota del numero di porta usato dall'istanza di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] a cui si sta tentando di connettersi.

  > [!NOTE]
  > Verrà probabilmente indicato `IP address 127.0.0.1`. Si tratta dell'indirizzo dell'adapter di loopback e solo i processi nello stesso computer possono usarlo per connettersi. Può essere utile per la risoluzione dei problemi, ma non può essere usato per la connessione da un altro computer.

## <a name="enable-protocols"></a><a name = "enableprotocols"></a>Abilitare i protocolli

In alcune installazioni di SQL Server la connessione al motore di database da un altro computer non è abilitata, a meno che non venga abilitata da un amministratore usando Gestione configurazione. Per abilitare le connessioni da un altro computer:

1. Aprire Gestione configurazione SQL Server come descritto sopra.
2. In Gestione configurazione nel riquadro sinistro espandere **Configurazione di rete SQL Server** e quindi selezionare l'istanza di SQL Server a cui si vuole connettersi. Nel riquadro destro sono elencati i protocolli di connessione disponibili. Il protocollo Shared Memory è in genere abilitato. Poiché può essere usato solo dallo stesso computer, il protocollo Shared Memory è abilitato nella maggior parte delle installazioni. La connessione a SQL Server da un altro computer avviene in genere tramite TCP/IP. Se TCP/IP non è abilitato, fare clic con il pulsante destro del mouse su **TCP/IP** e quindi fare clic su **Abilita**.
3. Se si modifica l'impostazione di abilitazione per un protocollo, è necessario riavviare il motore di database. Nel riquadro sinistro selezionare **Servizi di SQL Server**. Nel riquadro destro fare clic con il pulsante destro del mouse sull'istanza del motore di database e quindi fare clic su **Riavvia**.

## <a name="testing-tcpip-connectivity"></a><a name="testTCPIP"></a>Test della connettività TCP/IP

Per connettersi a SQL Server tramite TCP/IP è necessario che Windows sia in grado di stabilire la connessione. Usare lo strumento `ping` per testare la connettività TCP.

1. Nel menu Start fare clic su **Esegui**. Nella finestra **Esegui** digitare **cmd** e quindi fare clic su **OK**. 
2. Nella finestra del prompt dei comandi digitare `ping <ip address>` e quindi l'indirizzo IP del computer che esegue SQL Server. Ad esempio:

    - IPv4: `ping 192.168.1.101`
    - IPv6: `ping fe80::d51d:5ab5:6f09:8f48%11`

3. Se la rete è configurata correttamente, `ping` restituisce `Reply from <IP address>` seguito da alcune informazioni aggiuntive. Se `ping` restituisce `Destination host unreachable` o `Request timed out`, significa che la connettività TCP/IP non è configurata correttamente. Gli errori di questo tipo potrebbero indicare un problema del computer client, del computer server o di un componente della rete, ad esempio un router. Per risolvere i problemi di rete, vedere [Risoluzione avanzata dei problemi relativi alla connettività TCP/IP](/windows/client-management/troubleshoot-tcpip).
4. Successivamente, se il test di ping ha esito positivo usando l'indirizzo IP, verificare che il nome del computer possa essere risolto nell'indirizzo TCP/IP. Nella finestra del prompt dei comandi nel computer client digitare `ping` seguito dal nome del computer che esegue SQL Server. Ad esempio: `ping newofficepc`.
5. Se il comando `ping` per l'indirizzo IP ha esito positivo, ma il `ping` al computer restituisce `Destination host unreachable` o `Request timed out`, è possibile che nella cache del computer client siano memorizzate informazioni di risoluzione dei nomi obsolete (non aggiornate). Digitare `ipconfig /flushdns` per cancellare la cache DNS (Dynamic Name Resolution). Quindi eseguire nuovamente il ping del computer per nome. Con la cache DNS vuota, il computer client cercherà le informazioni più recenti sull'indirizzo IP del computer server.
6. Se la rete è configurata correttamente, `ping` restituisce `Reply from <IP address>` seguito da alcune informazioni aggiuntive. Se il ping al computer server con l'indirizzo IP ha esito positivo ma viene restituito un errore, ad esempio `Destination host unreachable.` o `Request timed out.`, quando si esegue il ping con il nome del computer, significa che la risoluzione dei nomi non è configurata correttamente. Per altre informazioni, vedere l'articolo del 2006 indicato in precedenza [How to Troubleshoot Basic TCP/IP Problems](https://support.microsoft.com/kb/169790) (Come risolvere i problemi di base TCP/IP). Poiché la risoluzione dei nomi non è necessaria per connettersi a SQL Server, se il nome del computer non può essere risolto in un indirizzo IP, è necessario stabilire le connessioni specificando l'indirizzo IP. La risoluzione dei nomi può essere corretta in seguito.

## <a name="open-a-port-in-the-firewall"></a>Aprire una porta nel firewall

Per impostazione predefinita, Windows Firewall è attivato e bloccherà le connessioni da altri computer. Per connettersi tramite TCP/IP da un altro computer, è necessario configurare il firewall nel computer SQL Server per consentire le connessioni alla porta TCP usata dal motore di database. Per impostazione predefinita, l'istanza predefinita è in ascolto sulla porta TCP 1433. Se sono presenti istanze denominate o se la porta dell'istanza predefinita è stata modificata, la porta TCP di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] potrebbe essere in ascolto su un'altra porta. Vedere [Ottenere la porta TCP dell'istanza di SQL Server](#getTCP).

Se si stabilisce la connessione a un'istanza denominata o a una porta diversa dalla porta TCP 1433, è necessario aprire anche la porta UDP 1434 per il servizio SQL Server Browser. Per istruzioni dettagliate sull'apertura di una porta in Windows Firewall, vedere [Configurazione di Windows Firewall per l'accesso al Motore di database](configure-a-windows-firewall-for-database-engine-access.md).

## <a name="test-the-connection"></a>Testare la connessione

Quando è possibile connettersi tramite TCP nello stesso computer, provare a connettersi dal computer client. Sebbene sia in teoria possibile usare qualsiasi applicazione client, per evitare complessità è consigliabile installare gli strumenti di gestione di SQL Server nel client ed eseguire il tentativo usando SQL Server Management Studio.

1. Nel computer client usare SQL Server Management Studio per tentare di connettersi con l'indirizzo IP e il numero di porta TCP nel formato indirizzo IP virgola numero di porta. Ad esempio: `192.168.1.101,1433`. Se la connessione non riesce, è probabile che si sia verificato uno dei problemi seguenti:

   - Il `ping` dell'indirizzo IP non funziona e indica un problema di configurazione TCP generale. Tornare alla sezione [Test della connettività TCP/IP](#testTCPIP).
   - SQL Server non è in attesa sul protocollo TCP. Tornare alla sezione [Abilitare i protocolli](#enableprotocols).
   - SQL Server è in attesa su una porta diversa da quella specificata. Tornare alla sezione [Ottenere il numero di porta TCP](#getTCP).
   - La porta TCP di SQL Server è bloccata dal firewall. Tornare alla sezione [Aprire una porta nel firewall](#open-a-port-in-the-firewall).

2. Quando è possibile connettersi usando l'indirizzo IP e il numero di porta, tentare di connettersi usando l'indirizzo IP senza un numero di porta. Per un'istanza predefinita, è sufficiente usare l'indirizzo IP. Per un'istanza denominata, usare l'indirizzo IP e il nome dell'istanza nel formato "indirizzo IP barra rovesciata nome dell'istanza", ad esempio `192.168.1.101\<instance name>`. Se non funziona, è probabile che si sia verificato uno dei problemi seguenti:

   - Se ci si connette all'istanza predefinita, è possibile che l'istanza sia in attesa su una porta diversa dalla porta TCP 1433 e che il client non stia tentando di connettersi al numero di porta corretto.
   - Se ci si connette a un'istanza denominata, il numero di porta non viene restituito al client.

   Entrambi questi problemi sono correlati al servizio SQL Server Browser che indica il numero di porta al client. Le soluzioni sono:

   - Avviare il servizio SQL Server Browser. Vedere le istruzioni per [avviare il browser in Gestione configurazione SQL Server](#startbrowser).
   - Il servizio SQL Server Browser è bloccato dal firewall. Aprire la porta UDP 1434 nel firewall. Tornare alla sezione [Aprire una porta nel firewall](#open-a-port-in-the-firewall). Assicurarsi di aprire una porta UDP e non TCP.
   - Le informazioni sulla porta UDP 1434 sono bloccate da un router. La comunicazione UDP (User Datagram Protocol) non è progettata per passare tramite router. Questo consente di evitare che la rete venga occupata da traffico con priorità bassa. È possibile configurare il router per l'inoltro del traffico UDP oppure si può decidere di comunicare sempre il numero di porta quando si stabilisce la connessione.
   - Se il computer client usa Windows 7, Windows Server 2008 o un sistema operativo più recente, è possibile che il traffico UDP venga eliminato dal sistema operativo client poiché la risposta del server viene restituita da un indirizzo IP diverso da quello richiesto. Si tratta di una misura di sicurezza che blocca il "loose source mapping". Per altre informazioni, vedere la sezione **Più indirizzi IP del server** dell'argomento della documentazione online [Risoluzione dei problemi: Timeout scaduto](/previous-versions/sql/sql-server-2008-r2/ms190181(v=sql.105)). Sebbene si tratti di un articolo relativo a SQL Server 2008 R2, le nozioni fornite sono ancora valide. È possibile configurare il client in modo che venga usato l'indirizzo IP corretto oppure si può decidere di comunicare sempre il numero di porta quando si stabilisce la connessione.

3. Quando è possibile connettersi usando l'indirizzo IP o l'indirizzo IP e il nome dell'istanza per un'istanza denominata, provare a connettersi usando il nome del computer o il nome del computer e il nome dell'istanza per un'istanza denominata. Inserire `tcp:` prima del nome del computer per forzare una connessione TCP/IP. Ad esempio, per l'istanza predefinita in un computer denominato `ACCNT27`usare `tcp:ACCNT27` . Per un'istanza denominata `PAYROLL`nello stesso computer usare `tcp:ACCNT27\PAYROLL` . Se è possibile connettersi usando l'indirizzo IP ma non è possibile farlo usando il nome del computer, significa che c'è un problema di risoluzione dei nomi. Tornare alla parte 4 della sezione **Test della connettività TCP/IP**.

4. Quando è possibile connettersi usando il nome del computer forzando TCP, tentare la connessione usando il nome del computer senza forzare TCP. Ad esempio, per un'istanza predefinita usare solo il nome del computer, ad esempio `CCNT27` Per un'istanza denominata usare il nome del computer e il nome di istanza, ad esempio `ACCNT27\PAYROLL` Se è stato possibile connettersi forzando TCP ma non è stato possibile senza forzare TCP, è probabile che il client stia usando un altro protocollo, ad esempio named pipe.

    1. In Gestione configurazione SQL Server nel computer client espandere **Configurazione** *SQL Native Client* **versione** nel riquadro sinistro e quindi selezionare **Protocolli client**.
    2. Nel riquadro destro assicurarsi che TCP/IP sia abilitato. Se TCP/IP è disabilitato, fare clic con il pulsante destro del mouse su **TCP/IP** e quindi fare clic su **Abilita**.
    3. Assicurarsi che l'ordine dei protocolli per TCP/IP sia rappresentato da un numero più piccolo rispetto a quello di named pipe o protocolli VIA (nelle versioni precedenti). In genere è consigliabile lasciare Shared Memory con ordine 1 e TCP/IP con ordine 2. Shared Memory viene usato solo quando il client e SQL Server sono in esecuzione nello stesso computer. Viene tentato l'uso di tutti i protocolli abilitati nell'ordine fino a quando non viene stabilita una connessione, ad eccezione di Shared Memory che viene ignorato quando non viene stabilita la connessione allo stesso computer.