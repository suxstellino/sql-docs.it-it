---
title: Connessione con l'autenticazione di Azure Active Directory
description: Informazioni su come sviluppare applicazioni Java che usano la funzionalità di autenticazione di Azure Active Directory con Microsoft JDBC Driver per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9c9d97be-de1d-412f-901d-5d9860c3df8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cbcf01d51ef4abe344b66529d72476a7d1385bbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168723"
---
# <a name="connecting-using-azure-active-directory-authentication"></a>Connessione con l'autenticazione di Azure Active Directory

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Questo articolo offre informazioni su come sviluppare applicazioni Java che usano la funzionalità di autenticazione di Azure Active Directory con Microsoft JDBC Driver per SQL Server.

È possibile usare l'autenticazione di Azure Active Directory (Azure AD) che è un meccanismo di connessione al database SQL di Azure v12 tramite identità in Azure Active Directory. Usare l'autenticazione di Azure Active Directory per gestire centralmente le identità degli utenti del database e come alternativa all'autenticazione di SQL Server. Il driver JDBC consente di specificare le credenziali di Azure Active Directory nella stringa di connessione JDBC per connettersi al database SQL di Azure. Per informazioni su come configurare l'autenticazione di Azure Active Directory, vedere [Connessione al database SQL usando l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). 

Le proprietà di connessione per supportare l'autenticazione di Azure Active Directory in Microsoft JDBC Driver per SQL Server sono le seguenti:
*   **authentication**:  usare questa proprietà per specificare il metodo di autenticazione SQL da usare per la connessione. I valori possibili sono: 
    * **ActiveDirectoryMSI**
        * Supportato a partire dalla versione **7.2** del driver, `authentication=ActiveDirectoryMSI` può essere usato per connettersi a un database SQL di Azure o a un'analisi di sinapsi dall'interno di una risorsa di Azure con il supporto "Identity" abilitato. Facoltativamente, è possibile specificare insieme a questa modalità di autenticazione anche **msiClientId** nelle proprietà Connection/DataSource che devono includere l'ID client di un'identità gestita da usare per acquisire **accessToken** per stabilire la connessione.
    * **ActiveDirectoryIntegrated**
        * Supportato a partire dalla versione **6.0** del driver, `authentication=ActiveDirectoryIntegrated` può essere usato per connettersi a un database SQL di Azure o a un'analisi di sinapsi usando l'autenticazione integrata. Per usare questa modalità di autenticazione, è necessaria la federazione di Active Directory Federation Services (ADFS) locale con Azure Active Directory nel cloud. Al termine della configurazione, è possibile connettersi aggiungendo la libreria nativa 'mssql-jdbc_auth-\<version>-\<arch>.dll' al percorso della classe dell'applicazione nel sistema operativo Windows o configurando un ticket Kerberos per il supporto dell'autenticazione multipiattaforma. È possibile accedere al database SQL di Azure o ad Azure Synapse Analytics senza che vengano richieste le credenziali quando si è connessi a un computer aggiunto al dominio.

    * **ActiveDirectoryPassword**
        * Supportato a partire dalla versione **6.0** del driver, `authentication=ActiveDirectoryPassword` può essere usato per connettersi a un database SQL di Azure o a un'analisi di sinapsi usando un Azure ad nome utente e una password.
    * **ActiveDirectoryInteractive**
        * Supportato a partire dalla versione **9.2** del driver, `authentication=ActiveDirectoryInteractive` può essere usato per connettersi a un database SQL di Azure o a un'analisi di sinapsi usando un flusso di autenticazione interattivo (autenticazione a più fattori).
    * **ActiveDirectoryServicePrincipal**
        * Supportato a partire dalla versione **9.2** del driver, `authentication=ActiveDirectoryServicePrincipal` può essere usato per connettersi a un database SQL di Azure o a un'analisi di sinapsi usando l'ID client e il segreto di un'identità dell'entità servizio.

    * **SqlPassword**
        * Usare `authentication=SqlPassword` per connettersi a SQL Server usando le proprietà userName/user e password.
    * **NotSpecified**
        * Usare `authentication=NotSpecified` o lasciare l'impostazione predefinita quando nessuno di questi metodi di autenticazione è necessario.

*   **accessToken**: usare questa proprietà di connessione per connettersi a un database SQL usando un token di accesso. accessToken può essere impostato solo usando il parametro Properties del metodo getConnection() nella classe DriverManager. Non può essere usato nell'URL di connessione.  

Per altre informazioni, vedere la proprietà di autenticazione nella pagina [Impostazione delle proprietà delle connessioni](setting-the-connection-properties.md).  


## <a name="client-setup-requirements"></a>Requisiti di installazione del client
Per l'autenticazione **ActiveDirectoryMSI**, è necessario installare nel computer client i componenti seguenti:
* Java 8 o versione successiva
* Microsoft JDBC Driver 7.2 (o versione successiva) per SQL Server
* L'ambiente client deve essere una risorsa di Azure e deve avere il supporto della funzionalità di identità abilitato.
* È necessario che nel database di destinazione sia presente un utente del database indipendente che rappresenta l'identità gestita assegnata dal sistema o l'identità gestita assegnata dall'utente della risorsa di Azure o uno dei gruppi a cui appartiene l'identità gestita e deve avere l'autorizzazione CONNECT.

Per le altre modalità di autenticazione, è necessario installare nel computer client i componenti seguenti:
* Java 7 o versione successiva
* Microsoft JDBC Driver 6.0 (o versione successiva) per SQL Server
* Se si usa la modalità di autenticazione basata su token di accesso, per eseguire gli esempi di questo articolo sono necessari [Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) e le relative dipendenze. Per ulteriori informazioni, vedere la sezione [connessione utilizzando il token di accesso](#connecting-using-access-token) .
* Se si usa la modalità di autenticazione **ActiveDirectoryPassword** , sono necessari [Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) e le relative dipendenze. Per altre informazioni, vedere la sezione [Connessione tramite la modalità di autenticazione ActiveDirectoryPassword](#connecting-using-activedirectorypassword-authentication-mode).
* Se si usa la modalità **ActiveDirectoryIntegrated** , sono necessari [Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) e le relative dipendenze. Per ulteriori informazioni, vedere la sezione [connessione mediante la modalità di autenticazione ActiveDirectoryIntegrated](#connecting-using-activedirectoryintegrated-authentication-mode) .
* Se si usa la modalità **ActiveDirectoryInteractive** , sono necessari [Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) e le relative dipendenze. Per ulteriori informazioni, vedere la sezione [connessione mediante la modalità di autenticazione ActiveDirectoryInteractive](#connecting-using-activedirectoryinteractive-authentication-mode) .
* Se si usa la modalità **ActiveDirectoryServicePrincipal** , sono necessari [Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) e le relative dipendenze. Per ulteriori informazioni, vedere la sezione [connessione mediante la modalità di autenticazione ActiveDirectoryServicePrincipal]()#connecting-using-ActiveDirectoryServicePrincipal-Authentication-Mode.


## <a name="connecting-using-activedirectorymsi-authentication-mode"></a>Connessione tramite la modalità di autenticazione ActiveDirectoryMSI
L'esempio seguente illustra come usare la modalità `authentication=ActiveDirectoryMSI`. Eseguire questo esempio dall'interno di una risorsa di Azure, ad esempio una macchina virtuale di Azure, un servizio app o un'app per le funzioni federata con Azure Active Directory.

Prima di eseguire l'esempio, sostituire il nome del server o database con il nome del proprio server o database nelle righe seguenti:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
//Optional
ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used
```

Esempio di utilizzo della modalità di autenticazione ActiveDirectoryMSI:

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AAD_MSI {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryMSI");
        // Optional
        ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

L'esecuzione di questo esempio in una macchina virtuale di Azure recupera un token di accesso dall'_identità gestita assegnata dal sistema_ o dall'_identità gestita assegnata dall'utente_ (se **msiClientId** è specificato) e stabilisce una connessione con il token di accesso recuperato. Se viene stabilita una connessione, verrà visualizzato il messaggio seguente:

```bash
You have successfully logged on as: <your Managed Identity username>
```

## <a name="connecting-using-activedirectoryintegrated-authentication-mode"></a>Connessione tramite la modalità di autenticazione ActiveDirectoryIntegrated
Con la versione 6.4, Microsoft JDBC Driver aggiunge il supporto dell'autenticazione ActiveDirectoryIntegrated usando un ticket Kerberos su più piattaforme (Windows, Linux e macOS).
Per altre informazioni, vedere [Impostare un ticket Kerberos in Windows, Linux e macOS](#set-kerberos-ticket-on-windows-linux-and-macos). In alternativa, in Windows è anche possibile usare mssql-jdbc_auth-\<version>-\<arch>.dll per l'autenticazione ActiveDirectoryIntegrated con JDBC Driver.

> [!NOTE]
>  Se si usa una versione precedente del driver, cercare in questo [collegamento](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md) le relative dipendenze necessarie per usare questa modalità di autenticazione. 

L'esempio seguente illustra come usare la modalità `authentication=ActiveDirectoryIntegrated`. Eseguire questo esempio in un computer aggiunto a un dominio federato con Azure Active Directory. È necessario che nel database sia presente un utente del database indipendente che rappresenta l'utente di Azure AD o uno dei gruppi cui si appartiene e deve avere l'autorizzazione CONNECT. 

Prima di compilare ed eseguire l'esempio, nel computer client in cui si vuole eseguire l'esempio scaricare il [file di libreria azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) e le relative dipendenze e includerli nel percorso di compilazione Java

Prima di eseguire l'esempio, sostituire il nome del server o database con il nome del proprio server o database nelle righe seguenti:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
```

Esempio di utilizzo della modalità di autenticazione ActiveDirectoryIntegrated:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADIntegrated {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryIntegrated");

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

Se si esegue questo esempio in un computer client, viene usato automaticamente il ticket Kerberos e non viene richiesta alcuna password. Se viene stabilita una connessione, verrà visualizzato il messaggio seguente:

```
You have successfully logged on as: <your domain user name>
```

### <a name="set-kerberos-ticket-on-windows-linux-and-macos"></a>Impostare un ticket Kerberos in Windows, Linux e macOS

È necessario configurare un ticket Kerberos che collega l'utente corrente a un account di dominio di Windows. Di seguito è riportato un riepilogo dei passaggi principali.

#### <a name="windows"></a>Windows
JDK include `kinit` che consente di ottenere un ticket di concessione ticket dal Centro distribuzione chiavi (KDC) in un computer aggiunto al dominio che è federato con Azure Active Directory.

##### <a name="step-1-ticket-granting-ticket-retrieval"></a>Passaggio 1: Recupero del ticket di concessione ticket
- **Eseguire in**: Windows
- **Azione**:
  - Usare il comando `kinit username@DOMAIN.COMPANY.COM` per ottenere un ticket di concessione ticket (TGT) dal Centro distribuzione chiavi (KDC). Verrà quindi richiesta la password del dominio.
  - Usare `klist` per visualizzare i ticket disponibili. Se kinit ha avuto esito positivo, verrà visualizzato un ticket proveniente da krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.

> [!NOTE]
>  Potrebbe essere necessario specificare un file `.ini` con `-Djava.security.krb5.conf` per consentire all'applicazione di individuare il Centro distribuzione chiavi (KDC).

#### <a name="linux-and-macos"></a>Linux e macOS

##### <a name="requirements"></a>Requisiti
Accesso a un computer aggiunto a un dominio di Windows per eseguire una query sul controller di dominio Kerberos.

##### <a name="step-1-find-kerberos-kdc"></a>Passaggio 1: Trovare il Centro distribuzione chiavi (KDC) Kerberos
- **Eseguire in**: Riga di comando di Windows
- **Azione**: `nltest /dsgetdc:DOMAIN.COMPANY.COM` (dove "DOMAIN.COMPANY.COM" viene mappato al nome del dominio)
- **Output di esempio**
  ```
  DC: \\co1-red-dc-33.domain.company.com
  Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
  ...
  The command completed successfully
  ```
- **Informazioni da estrarre** Il nome DC, in questo caso `co1-red-dc-33.domain.company.com`

##### <a name="step-2-configuring-kdc-in-krb5conf"></a>Passaggio 2: Configurare il Centro distribuzione chiavi (KDC) in krb5.conf
- **Eseguire in**: Linux/macOS
- **Azione**: Modificare il file /etc/krb5.conf in un editor di propria scelta. Configurare le chiavi seguenti
  ```
  [libdefaults]
    default_realm = DOMAIN.COMPANY.COM
   
  [realms]
  DOMAIN.COMPANY.COM = {
     kdc = co1-red-dc-28.domain.company.com
  }
  ```
  Salvare quindi il file krb5.conf e uscire

> [!NOTE]
>  Il dominio deve essere in lettere MAIUSCOLE.

##### <a name="step-3-testing-the-ticket-granting-ticket-retrieval"></a>Passaggio 3: Test del recupero del ticket di concessione ticket
- **Eseguire in**: Linux/macOS
- **Azione**:
  - Usare il comando `kinit username@DOMAIN.COMPANY.COM` per ottenere un ticket di concessione ticket (TGT) dal Centro distribuzione chiavi (KDC). Verrà quindi richiesta la password del dominio.
  - Usare `klist` per visualizzare i ticket disponibili. Se kinit ha avuto esito positivo, verrà visualizzato un ticket proveniente da krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.

## <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Connessione tramite la modalità di autenticazione ActiveDirectoryPassword
L'esempio seguente illustra come usare la modalità `authentication=ActiveDirectoryPassword`.

Prima di compilare ed eseguire l'esempio:
1.  Nel computer client in cui si vuole eseguire l'esempio scaricare il [file di libreria azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) e le relative dipendenze e includerli nel percorso di compilazione Java
2.  Individuare le righe di codice seguenti e sostituire il nome del server o database con il nome del proprio server o database.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Individuare le righe di codice seguenti e sostituire il nome utente con il nome dell'utente di Azure AD con cui si vuole connettersi.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ds.setPassword("password");     // replace with your password
    ```

Esempio di utilizzo della modalità di autenticazione ActiveDirectoryPassword:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADUserPassword {
    
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        ds.setPassword("password"); // Replace with your password
        ds.setAuthentication("ActiveDirectoryPassword");
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Se viene stabilita la connessione, verrà visualizzato il messaggio seguente:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> È necessario che nel database sia presente un utente del database indipendente che rappresenta l'utente di Azure AD specificato o uno dei gruppi a cui appartiene l'utente e deve avere l'autorizzazione CONNECT (ad eccezione dell'amministratore del server o del gruppo di Azure Active Directory)

## <a name="connecting-using-activedirectoryinteractive-authentication-mode"></a>Connessione tramite la modalità di autenticazione ActiveDirectoryInteractive 
L'esempio seguente illustra come usare la modalità `authentication=ActiveDirectoryInteractive`.

Prima di compilare ed eseguire l'esempio:
1.  Nel computer client in cui si vuole eseguire l'esempio scaricare il [file di libreria azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) e le relative dipendenze e includerli nel percorso di compilazione Java
2.  Individuare le righe di codice seguenti e sostituire il nome del server o database con il nome del proprio server o database.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Individuare le righe di codice seguenti e sostituire il nome utente con il nome dell'utente di Azure AD con cui si vuole connettersi.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

Esempio di utilizzo della modalità di autenticazione ActiveDirectoryInteractive:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADInteractive {
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryInteractive");
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Quando si esegue il programma, viene visualizzato un browser per autenticare l'utente. Le informazioni visualizzate dall'utente dipendono dal modo in cui è stata configurata la Azure AD. Ad esempio, è possibile che non includa richieste di autenticazione a più fattori, ad esempio un nome utente, una password, un PIN o una seconda autenticazione del dispositivo tramite telefono. Se nello stesso programma vengono eseguite più richieste di autenticazione interattiva, le richieste successive potrebbero non richiedere all'utente se la libreria di autenticazione può riutilizzare un token di autenticazione precedentemente memorizzato nella cache.

Per informazioni su come configurare Azure AD per richiedere Multi-Factor Authentication, vedere [Getting Started with Azure AD multi-factor authentication nel cloud](/azure/active-directory/authentication/howto-mfa-getstarted).

Per gli screenshot di queste finestre di dialogo, vedere [configurare l'autenticazione a più fattori per SQL Server Management Studio e Azure ad](/azure/azure-sql/database/authentication-mfa-ssms-configure).

Se l'autenticazione utente viene completata correttamente, nel browser dovrebbe essere visualizzato il messaggio seguente:
```
Authentication complete. You can close the browser and return to the application.
```
Si noti che questo indica solo che l'autenticazione utente è riuscita ma non necessariamente una connessione al server. Quando viene ripristinata l'applicazione, se viene stabilita una connessione al server, viene visualizzato il messaggio seguente come output:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Un database utente indipendente deve esistere e un utente del database indipendente che rappresenta l'utente Azure AD specificato o uno dei gruppi a cui appartiene l'utente Azure AD specificato deve esistere nel database e deve disporre dell'autorizzazione CONNECT (ad eccezione di un amministratore o di un gruppo di Azure Active Directory Server)

## <a name="connecting-using-activedirectoryserviceprincipal-authentication-mode"></a>Connessione tramite la modalità di autenticazione ActiveDirectoryServicePrincipal
L'esempio seguente illustra come usare la modalità `authentication=ActiveDirectoryServicePrincipal`.

Prima di compilare ed eseguire l'esempio:
1.  Nel computer client in cui si vuole eseguire l'esempio scaricare il [file di libreria azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) e le relative dipendenze e includerli nel percorso di compilazione Java
2.  Individuare le righe di codice seguenti e sostituire il nome del server o database con il nome del proprio server o database.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Individuare le righe di codice seguenti e sostituire il nome utente con il nome dell'utente di Azure AD con cui si vuole connettersi.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

Esempio di utilizzo della modalità di autenticazione ActiveDirectoryInteractive:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADServicePrincipal {
    public static void main(String[] args) throws Exception{
        String principalId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your AAD secure principal ID.
        String principalSecret = "..."; // Replace with your AAD principal secret.

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryServicePrincipal");
    ds.setAADSecurePrincipalId(principalId);
    ds.setAADSecurePrincipalSecret(principalSecret);
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Se viene stabilita una connessione, verrà visualizzato il messaggio seguente come output:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Un database utente indipendente deve esistere e un utente del database indipendente che rappresenta l'utente Azure AD specificato o uno dei gruppi a cui appartiene l'utente Azure AD specificato deve esistere nel database e deve disporre dell'autorizzazione CONNECT (ad eccezione di un amministratore o di un gruppo di Azure Active Directory Server)

## <a name="connecting-using-access-token"></a>Connessione tramite token di accesso
Le applicazioni e i servizi possono recuperare un token di accesso dal Azure Active Directory e usarlo per connettersi al database SQL di Azure/analisi sinapsi.

> [!NOTE] 
> **accessToken** può essere impostato solo usando il parametro Properties del metodo getConnection() nella classe DriverManager. Non può essere usato nella stringa di connessione.

L'esempio seguente contiene una semplice applicazione Java che si connette al database SQL di Azure/analisi sinapsi usando l'autenticazione basata su token di accesso. Prima di compilare ed eseguire l'esempio, seguire questa procedura:
1.  Creare un account dell'applicazione in Azure Active Directory per il servizio.
    1. Accedere al portale di Azure.
    2. Nel riquadro di spostamento a sinistra fare clic su Azure Active Directory.
    3. Fare clic su "Registrazioni app".
    4. Nel menu di spostamento fare clic su "Registrazione nuova applicazione".
    5. Immettere mytokentest come nome descrittivo per l'applicazione e selezionare "App Web/API".
    6. L'URL di accesso non è necessario. Specificare ad esempio "https://mytokentest".
    7. Fare clic su "Crea" nella parte inferiore della schermata.
    9. Sempre nel portale di Azure fare clic sulla scheda "Impostazioni" dell'applicazione e aprire la scheda "Proprietà".
    10. Trovare il valore "ID applicazione" (noto anche come ID client) e copiarlo. L'ID sarà necessario successivamente durante la configurazione dell'applicazione (ad esempio, 1846943b-ad04-4808-aa13-4702d908b5c1). Vedere lo snapshot seguente.
    11. Nella sezione "Chiavi" creare una chiave specificando un valore nel campo nome, selezionando la durata della chiave e salvando la configurazione (lasciare vuoto il campo valore). Dopo il salvataggio, il campo valore viene specificato automaticamente. Copiare il valore generato. Questo è il segreto client.
    12. Nel pannello di sinistra fare clic su Azure Active Directory. In "Registrazioni app" trovare la scheda "Endpoint". Copiare l'URL in "OATH 2.0 TOKEN ENDPOINT". Si tratta dell'URL STS.
    
    ![Endpoint di registrazione delle app nel portale di Azure - URL STS](media/jdbc_aad_token.png)  
2. Accedere al database utente del server SQL Azure come amministratore di Azure Active Directory e usando un comando T-SQL effettuare il provisioning di un utente di database indipendente per l'entità di sicurezza dell'applicazione. Per altre informazioni su come creare un amministratore di Azure Active Directory e un utente di database indipendente, vedere [Connessione al database SQL o ad Azure Synapse Analytics tramite l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

    ```
    CREATE USER [mytokentest] FROM EXTERNAL PROVIDER
    ```

3.  Nel computer client in cui si vuole eseguire l'esempio, scaricare la libreria [Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) e le relative dipendenze e includerle nel percorso di compilazione Java. Si noti che Microsoft-Authentication-Library-for-Java è necessario solo per eseguire questo esempio specifico. L'esempio usa le API di questa libreria per recuperare il token di accesso da Azure AD. Se si ha già un token di accesso, ignorare questo passaggio. Si noti che è necessario anche rimuovere dall'esempio la sezione che recupera il token di accesso.

Nell'esempio seguente sostituire l'URL STS, l'ID client, il segreto client, il nome del server e del database con i propri valori.

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

// The azure-activedirectory-library-for-java is needed to retrieve the access token from the AD.
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;

public class AADTokenBased {

    public static void main(String[] args) throws Exception {

        // Retrieve the access token from the AD.
        String spn = "https://database.windows.net/";
        String stsurl = "https://login.microsoftonline.com/..."; // Replace with your STS URL.
        String clientId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your client ID.
        String clientSecret = "..."; // Replace with your client secret.

    String scope = spn +  "/.default";
    Set<String> scopes = new HashSet<>();
        scopes.add(scope);
    
    ExecutorService executorService = Executors.newSingleThreadExecutor();
    IClientCredential credential = ClientCredentialFactory.createFromSecret(clientSecret);
    ConfidentialClientApplication clientApplication = ConfidentialClientApplication
            .builder(clientId, credential).executorService(executorService).authority(stsurl).build();
    CompletableFuture<IAuthenticationResult> future = clientApplication
            .acquireToken(ClientCredentialParameters.builder(scopes).build());
            
    IAuthenticationResult authenticationResult = future.get();
    String accessToken = authenticationResult.accessToken();

        System.out.println("Access Token: " + accessToken);

        // Connect with the access token.
        SQLServerDataSource ds = new SQLServerDataSource();

        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name.
        ds.setDatabaseName("demo"); // Replace with your database name.
        ds.setAccessToken(accessToken);

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
``` 

Se la connessione viene stabilita correttamente, verrà visualizzato il messaggio seguente:

```bash
Access Token: <your access token>
You have successfully logged on as: <your client ID>    
```
