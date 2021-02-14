---
title: "Esercitazione: Usare l'autenticazione di Active Directory per SQL Server in Linux"
titleSuffix: SQL Server
description: Questa esercitazione illustra la procedura di configurazione dell'autenticazione di Active Directory (AD) per SQL Server in Linux.
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.date: 12/18/2019
ms.topic: tutorial
ms.prod: sql
ms.custom: seodec18
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: c6487f5c22a61cf1a4979af18d5cc3fdae5b0eb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273152"
---
# <a name="tutorial-use-active-directory-authentication-with-sql-server-on-linux"></a>Esercitazione: Usare l'autenticazione di Azure Active Directory con SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questa esercitazione illustra come configurare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux per supportare l'autenticazione di Active Directory (AD), detta anche autenticazione integrata. Per una panoramica, vedere [Autenticazione di Active Directory per SQL Server in Linux](sql-server-linux-active-directory-auth-overview.md).

Questa esercitazione è costituita dalle attività seguenti:

> [!div class="checklist"]
> * Aggiungere l'host di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] al dominio di Active Directory
> * Creare un utente di Active Directory per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e impostare il nome dell'entità servizio (SPN)
> * Configurare il keytab del servizio [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
> * Proteggere il file keytab
> * Configurare SQL Server per l'uso del file keytab per l'autenticazione Kerberos
> * Creare account di accesso basati su AD in Transact-SQL
> * Connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando l'autenticazione di Active Directory

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare l'autenticazione di Active Directory, è necessario:

* Configurare un controller di dominio di Active Directory (Windows) nella rete  
* Installare [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
  * [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)
  * [SUSE Linux Enterprise Server (SLES)](quickstart-install-connect-suse.md)
  * [Ubuntu](quickstart-install-connect-ubuntu.md)

## <a name="join-ssnoversion-host-to-ad-domain"></a><a id="join"></a> Aggiungere l'host di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] al dominio di Active Directory

Aggiungere l'host di SQL Server in Linux a un controller di dominio Active Directory. Aggiungere un host di SQL Server in Linux a un dominio di Active DirectoryPer informazioni sull'aggiunta a un dominio Active Directory, vedere [Aggiungere un host di SQL Server in Linux a un dominio di Active Directory](sql-server-linux-active-directory-join-domain.md).

## <a name="create-ad-user-or-msa-for-ssnoversion-and-set-spn"></a><a id="createuser"></a> Creare un utente di Active Directory (o un account del servizio gestito) per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e impostare il nome dell'entità servizio (SPN)

> [!NOTE]
> Nella procedura seguente viene usato il [nome di dominio completo](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Se si è in **Azure**, è necessario **[crearne uno](/azure/virtual-machines/linux/portal-create-fqdn)** prima di procedere.

1. Nel controller di dominio eseguire il comando di PowerShell [New-ADUser](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617253(v=technet.10)) per creare un nuovo utente di Active Directory con una password che non scade mai. L'esempio seguente assegna all'account il nome `mssql`, ma si può scegliere un nome qualsiasi. Viene chiesto di immettere una nuova password per l'account.

   ```PowerShell
   Import-Module ActiveDirectory

   New-ADUser mssql -AccountPassword (Read-Host -AsSecureString "Enter Password") -PasswordNeverExpires $true -Enabled $true
   ```

   > [!NOTE]
   > Come misura di sicurezza, è consigliabile avere un account AD dedicato per SQL Server, in modo che le credenziali di SQL Server non siano condivise con altri servizi che usano lo stesso account. Tuttavia, se si vuole è possibile riutilizzare un account AD esistente se se ne conosce la password (necessaria per generare un file keytab nel passaggio successivo). L'account, inoltre, deve essere abilitato per supportare la crittografia AES Kerberos a 128 e 256 bit (attributo **msDS-SupportedEncryptionTypes**) nell'account utente. Per verificare che l'account sia abilitato per la crittografia AES, individuare l'account nell'utilità **Utenti e computer di Active Directory** e selezionare **Proprietà**. Individuare la scheda **Account** nella finestra **Proprietà** e verificare che le due caselle di controllo seguenti siano selezionate. 
   >
   > 1. **Questo account supporta la crittografia AES 128 bit Kerberos**
   >
   > 2. **Questo account supporta la crittografia AES 256 bit Kerberos**

2. Impostare il nome dell'entità servizio (SPN) per l'account usando lo strumento **setspn.exe**. Il nome SPN deve essere formattato esattamente come specificato nell'esempio seguente. È possibile trovare il nome di dominio completo del computer host di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] eseguendo `hostname --all-fqdns` nell'host di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La porta TCP deve essere la 1433, a meno che [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] non sia stato configurato per l'uso di un numero di porta diverso.

   ```PowerShell
   setspn -A MSSQLSvc/<fully qualified domain name of host machine>:<tcp port> mssql
   setspn -A MSSQLSvc/<netbios name of the host machine>:<tcp port> mssql
   ```

   > [!NOTE]
   > Se viene visualizzato un messaggio di errore, `Insufficient access rights`, verificare con l'amministratore del dominio di avere autorizzazioni sufficienti per impostare un SPN per l'account. L'account usato per registrare un nome dell'entità servizio richiederà le autorizzazioni **servicePrincipalName di scrittura**. Per altre informazioni, vedere [Registrare un nome dell'entità servizio per le connessioni Kerberos](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).
   >
   > Se si cambia la porta TCP in futuro, è necessario eseguire di nuovo il comando **setspn** con il nuovo numero di porta. È anche necessario aggiungere il nuovo SPN al keytab del servizio SQL Server seguendo la procedura descritta nella sezione successiva.

Per altre informazioni, vedere [Registrare un nome dell'entità servizio per le connessioni Kerberos](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).

## <a name="configure-ssnoversion-service-keytab"></a><a id="configurekeytab"></a> Configurare il keytab del servizio [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

La configurazione dell'autenticazione di Active Directory per SQL Server in Linux richiede un account AD (account del servizio gestito o account utente di Active Directory) e il nome dell'entità servizio creata nella sezione precedente.

> [!IMPORTANT]
> Se si cambia la password dell'account di Active Directory o la password dell'account a cui sono assegnati i nomi dell'entità servizio, è necessario aggiornare il keytab con la nuova password e il numero di versione della chiave. Alcuni servizi potrebbero anche cambiare automaticamente le password a rotazione. Esaminare i criteri di rotazione delle password per gli account in questione e allinearli alle attività di manutenzione pianificata per evitare tempi di inattività imprevisti.

### <a name="spn-keytab-entries"></a><a id="spn"></a> Voci del keytab SPN

1. Verificare il numero di versione della chiave per l'account di Active Directory creato nel passaggio precedente. In genere è 2, ma potrebbe essere un altro numero intero se la password dell'account è stata cambiata più volte. Nel computer host SQL Server eseguire i comandi seguenti:

    - Gli esempi seguenti presuppongono che `user` si trovi nel dominio `@CONTOSO.COM`. Modificare l'utente e il nome di dominio per l'utente e il nome di dominio.

   ```bash
   kinit user@CONTOSO.COM
   kvno user@CONTOSO.COM
   kvno MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM
   ```

   > [!NOTE]
   > La propagazione dei nomi SPN nel dominio può richiedere diversi minuti, soprattutto se il dominio è di grandi dimensioni. Se viene visualizzato il messaggio di errore `kvno: Server not found in Kerberos database while getting credentials for MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM`, attendere qualche minuto e riprovare.</br></br> I comandi precedenti funzioneranno solo se il server è stato aggiunto a un dominio di Active Directory, come descritto nella sezione precedente.

1. Tramite [**ktpass**](/windows-server/administration/windows-commands/ktpass), aggiungere voci keytab per ogni nome dell'entità servizio usando i comandi seguenti in un prompt dei comandi del computer Windows:

    - `<DomainName>\<UserName>`: può essere un account del servizio gestito o un account utente di Active Directory
    - `@CONTOSO.COM`: usare il nome di dominio
    - `<#>`: sostituire `/kvno <#>` con il numero di versione della chiave ottenuto in un passaggio precedente
    - `<StrongPassword>`: usare una password complessa

   ```bash
   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>
   ```

   > [!NOTE]
   > I comandi precedenti ammettono sia gli algoritmi di crittografia AES sia RC4 per l'autenticazione AD. RC4 è un algoritmo di crittografia più obsoleto. Se è necessario un livello di sicurezza più elevato, è possibile scegliere di creare le voci keytab usando soltanto l'algoritmo di crittografia AES.
   > Le ultime due voci `UserName` devono essere in lettere minuscole oppure l'autenticazione delle autorizzazioni potrebbe non riuscire.

1. Dopo aver eseguito il comando precedente, è necessario disporre di un file keytab denominato mssql.keytab. Copiare il file nel computer SQL Server nella cartella `/var/opt/mssql/secrets`.

1. Proteggere il file keytab.

    Chiunque abbia accesso a questo file keytab può rappresentare SQL Server nel dominio, quindi assicurarsi di limitare l'accesso al file in modo che solo l'account mssql disponga dell'accesso in lettura:

    ```bash
    sudo chown mssql:mssql /var/opt/mssql/secrets/mssql.keytab
    sudo chmod 400 /var/opt/mssql/secrets/mssql.keytab
    ```

1. È necessario impostare l'opzione di configurazione seguente usando lo strumento **mssql-conf** per specificare l'account da usare durante l'accesso al file keytab.

   ```bash
   sudo mssql-conf set network.privilegedadaccount <username>
   ```

   > [!NOTE]
   > Includere solo il nome utente e non nomedominio\nomeutente o username@domain. SQL Server aggiunge internamente il nome di dominio necessario insieme al nome utente, se usato.

1. Attenersi alla procedura seguente per configurare SQL Server in modo che inizi a usare il file keytab per l'autenticazione Kerberos.

    ```bash
    sudo mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
    sudo systemctl restart mssql-server
    ```

    > [!TIP]
    > Facoltativamente disabilitare le connessioni UDP al controller di dominio per migliorare le prestazioni. In molti casi, le connessioni UDP hanno sempre esito negativo quando ci si connette a un controller di dominio, quindi è possibile impostare le opzioni di configurazione in **/etc/krb5.conf** in modo da ignorare le chiamate UDP. Modificare **/etc/krb5.conf** e impostare le opzioni seguenti:
    > ```bash
    > /etc/krb5.conf
    > [libdefaults]
    > udp_preference_limit=0
    > ```

A questo punto si è pronti per usare gli account di accesso basati su Active Directory in SQL Server.

## <a name="create-ad-based-logins-in-transact-sql"></a><a id="createsqllogins"></a> Creare account di accesso basati su AD in Transact-SQL

1. Connettersi a SQL Server e creare un nuovo account di accesso basato su Active Directory:

   ```sql
   CREATE LOGIN [CONTOSO\user] FROM WINDOWS;
   ```

1. Verificare che l'account di accesso sia ora elencato nella vista del catalogo di sistema [sys.server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md):

   ```sql
   SELECT name FROM sys.server_principals;
   ```

## <a name="connect-to-sql-server-using-ad-authentication"></a><a id="connect"></a> Connettersi a SQL Server usando l'autenticazione di Active Directory

Accedere a un computer client usando le credenziali del proprio dominio. A questo punto è possibile connettersi a SQL Server senza immettere nuovamente la password usando l'autenticazione di Active Directory. Se si crea un account di accesso per un gruppo di Active Directory, qualsiasi utente di Active Directory che sia membro di tale gruppo potrà connettersi nello stesso modo.

Lo specifico parametro della stringa di connessione che indica ai client di usare l'autenticazione di Active Directory dipende dal driver in uso. Considerare gli esempi nelle sezioni riportate di seguito.

### <a name="sqlcmd-on-a-domain-joined-linux-client"></a>sqlcmd in un client Linux aggiunto a un dominio

Accedere a un client Linux aggiunto al dominio usando **ssh** e le credenziali del dominio:

```bash
ssh -l user@contoso.com client.contoso.com
```

Assicurarsi di aver installato il pacchetto [mssql-tools](sql-server-linux-setup-tools.md), quindi connettersi usando **sqlcmd** senza specificare le credenziali:

```bash
sqlcmd -S mssql-host.contoso.com
```
A differenza di SQL Windows, l'autenticazione Kerberos funziona per la connessione locale in SQL Linux. È comunque necessario specificare il nome di dominio completo dell'host SQL Linux. L'autenticazione di Active Directory non funzionerà se si tenta di connettersi a '.' ,'localhost','127.0.0.1', e così via.

### <a name="ssms-on-a-domain-joined-windows-client"></a>SSMS in un client Windows aggiunto a un dominio

Accedere a un client Windows aggiunto al dominio usando le credenziali del dominio. Assicurarsi che SQL Server Management Studio sia installato, quindi connettersi all'istanza di SQL Server, ad esempio `mssql-host.contoso.com`, specificando **Autenticazione di Windows** nella finestra di dialogo **Connetti al server**.

### <a name="ad-authentication-using-other-client-drivers"></a>Autenticazione di Active Directory con altri driver client

La tabella seguente contiene alcuni consigli per altri driver client:

| Driver client | Recommendation |
|---|---|
| **JDBC** | Usare l'autenticazione integrata Kerberos per la connessione a SQL Server. |
| **ODBC** | Usare l'autenticazione integrata SQL. |
| **ADO.NET** | Sintassi della stringa di connessione. |

## <a name="additional-configuration-options"></a><a id="additionalconfig"></a> Opzioni di configurazione aggiuntive

Se si usano utilità di terze parti, come [PBIS](https://www.beyondtrust.com/), [VAS](https://www.oneidentity.com/products/authentication-services/) o [Centrify](https://www.centrify.com/), per aggiungere l'host Linux al dominio di Active Directory e si vuole forzare SQL Server a usare direttamente la libreria openldap, è possibile configurare l'opzione **disablesssd** con **mssql-conf** come indicato di seguito:

```bash
sudo mssql-conf set network.disablesssd true
systemctl restart mssql-server
```

> [!NOTE]
> Alcune utilità, come **realmd**, installano SSSD, mentre altri strumenti, come PBIS, VAS e Centrify, non lo fanno. Se l'utilità usata per l'aggiunta al dominio di Active Directory non prevede l'installazione di SSSD, è consigliabile impostare l'opzione **disablesssd** su `true`. Anche se non è necessario, poiché SQL Server tenterà di usare SSSD per Active Directory prima di eseguire il fallback al meccanismo openldap, sarebbe più efficiente configurarlo in modo che SQL Server esegua le chiamate a openldap direttamente ignorando il meccanismo SSSD.

Se il controller di dominio supporta LDAPS, è possibile forzare l'esecuzione di tutte le connessioni da SQL Server ai controller di dominio tramite LDAPS. Per verificare che il client sia in grado di contattare il controller di dominio tramite LDAPS, eseguire il comando bash `ldapsearch -H ldaps://contoso.com:3269`. Per impostare SQL Server in modo da usare solo LDAPS, eseguire il comando seguente:

```bash
sudo mssql-conf set network.forcesecureldap true
systemctl restart mssql-server
```

Verrà usato LDAPS su SSSD se l'aggiunta al dominio AD dell'host è stata eseguita tramite il pacchetto SSSD e **disablesssd** non è impostato su true. Se **disablesssd** è impostato su true e lo è anche **forcesecureldap**, il protocollo LDAPS verrà usato sulle chiamate alla libreria openldap eseguite da SQL Server.

### <a name="post-sql-server-2017-cu14"></a>Dopo SQL Server 2017 CU14

A partire da SQL Server 2017 CU14, se SQL Server è stato aggiunto a un controller di dominio di Active Directory usando provider di terze parti ed è configurato per l'uso di chiamate openldap per la ricerca tramite AD generale impostando **disablesssd** su true, è possibile usare anche l'opzione **enablekdcfromkrb5** per forzare SQL Server a usare la libreria krb5 per la ricerca KDC anziché la ricerca DNS inversa per il server KDC.

Questo può essere utile se si vogliono configurare manualmente i controller di dominio con cui SQL Server tenta di comunicare e si usa il meccanismo della libreria openldap tramite l'elenco KDC in **krb5.conf**.

Prima di tutto impostare **disablessd** e **enablekdcfromkrb5conf** su True, quindi riavviare SQL Server:

```bash
sudo mssql-conf set network.disablesssd true
sudo mssql-conf set network.enablekdcfromkrb5conf true
systemctl restart mssql-server
```

Configurare poi l'elenco KDC in **/etc/krb5.conf** nel modo seguente:

```/etc/krb5.conf
[realms]
CONTOSO.COM = {
  kdc = dcWithGC1.contoso.com
  kdc = dcWithGC2.contoso.com
}
```

> [!NOTE]
> Sebbene non sia consigliato, è possibile usare utilità, come **realmd**, che installano SSSD durante l'aggiunta dell'host Linux al dominio, impostando **disablesssd** su true in modo che SQL Server usi openldap anziché SSSD per le chiamate correlate ad Active Directory.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione sono state illustrate le procedure per configurare l'autenticazione di Active Directory con SQL Server in Linux. Si è appreso come:
> [!div class="checklist"]
> * Aggiungere l'host di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] al dominio di Active Directory
> * Creare un utente di Active Directory per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e impostare il nome dell'entità servizio (SPN)
> * Configurare il keytab del servizio [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
> * Creare account di accesso basati su AD in Transact-SQL
> * Connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usando l'autenticazione di Active Directory

Esaminare quindi gli altri scenari di sicurezza per SQL Server in Linux.

> [!div class="nextstepaction"]
> [Crittografia delle connessioni a SQL Server in Linux](sql-server-linux-encrypted-connections.md)