---
title: Crittografia delle connessioni a SQL Server in Linux
description: SQL Server in Linux usa TLS per crittografare i dati trasmessi attraverso una rete tra un'applicazione client e un'istanza di SQL Server.
ms.date: 06/29/2020
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 83d89d99bf2acebf663309926ae298a64d64732c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348144"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Crittografia delle connessioni a SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in Linux può usare TLS (Transport Layer Security) per crittografare i dati trasmessi attraverso una rete tra un'applicazione client e un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] supporta gli stessi protocolli TLS sia in Windows che in Linux: TLS 1.2, 1.1 e 1.0. I passaggi per configurare TLS sono tuttavia specifici del sistema operativo in cui [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è in esecuzione.  

## <a name="requirements-for-certificates"></a>Requisiti per i certificati 
Prima di iniziare, è necessario assicurarsi che i certificati siano conformi ai requisiti seguenti:
- L'ora di sistema corrente deve essere successiva al valore della proprietà Valido dal del certificato e antecedente al valore della proprietà Valido fino a del certificato.
- Il certificato deve essere destinato all'autenticazione del server. Per questa operazione è necessario impostare la proprietà Utilizzo chiavi avanzato del certificato su Autenticazione server (1.3.6.1.5.5.7.3.1).
- Il certificato deve essere creato tramite l'opzione KeySpec AT_KEYEXCHANGE. In genere, la proprietà del certificato relativa all'utilizzo della chiave (KEY_USAGE) include anche la crittografia della chiave (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- La proprietà Soggetto del certificato deve specificare che il nome comune (CN, Common Name) corrisponde al nome host oppure al nome di dominio completo (FQDN, Fully Qualified Domain Name) del server. Nota: sono supportati i certificati con caratteri jolly.

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>Configurazione delle librerie OpenSSL per l'utilizzo (facoltativo)
È possibile creare nella directory `/opt/mssql/lib/` collegamenti simbolici che facciano riferimento alle librerie `libcrypto.so` e `libssl.so` da usare per la crittografia. Questa opzione è utile se si vuole imporre a SQL Server l'uso di una versione specifica di OpenSSL diversa da quella predefinita fornita dal sistema. Se questi collegamenti simbolici non sono presenti, SQL Server caricherà le librerie OpenSSL predefinite configurate nel sistema.

Questi collegamenti simbolici devono essere denominati `libcrypto.so` e `libssl.so` ed essere inseriti nella directory `/opt/mssql/lib/`.

## <a name="overview"></a>Panoramica
TLS viene usato per crittografare le connessioni da un'applicazione client a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Se configurato correttamente, TLS garantisce la sia privacy che l'integrità dei dati per le comunicazioni tra il client e il server.  Le connessioni TLS possono essere avviate dal client oppure dal server. 

## <a name="client-initiated-encryption"></a>Crittografia avviata dal client 
- **Generare il certificato** (/CN deve corrispondere al nome di dominio completo dell'host di SQL Server)

> [!NOTE]
> Per questo esempio viene usato un certificato autofirmato che non deve essere usato per gli scenari di produzione. È consigliabile usare i certificati della CA.<br>
> Assicurarsi che la cartella/i salvati i certificati e le chiavi private siano accessibili da parte dell'utente o del gruppo MSSQL e che disponga delle autorizzazioni impostate su 700 (drwx-----). È possibile creare cartelle manualmente con il set di autorizzazioni a 700 (drwx------) e di proprietà dell'utente o del gruppo MSSQL oppure impostare l'autorizzazione su 755 (drwxr-xr-x) e di proprietà di un altro utente, ma ancora accessibile al gruppo di utenti MSSQL. Ad esempio, è possibile creare una cartella denominata "sslcert" nel percorso "/var/opt/MSSQL/" e quindi salvare il certificato e la chiave privata con le autorizzazioni per i file impostati su 600 come illustrato di seguito. 

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key 
# in this case we are saving the certificate to the certs folder under /etc/ssl/ which has the following permission 755(drwxr-xr-x)
sudo mv mssql.pem /etc/ssl/certs/ drwxr-xr-x 
# in this case we are saving the private key to the private folder under /etc/ssl/ with permissions set to 755(drwxr-xr-x)
sudo mv mssql.key /etc/ssl/private/ 
```

- **Configurare SQL Server**

```bash
systemctl stop mssql-server 
cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 
```

- **Registrare il certificato nel computer client (Windows, Linux o macOS)**

    -   Se si usa un certificato della CA firmato, è necessario copiare nel computer client il certificato dell'Autorità di certificazione (CA) invece del certificato dell'utente. 
    -   Se si usa il certificato autofirmato, è sufficiente copiare il file PEM nelle cartelle seguenti, a seconda della distribuzione, ed eseguire i comandi per abilitarlo 
        - **Ubuntu**: copiare il certificato in `/usr/share/ca-certificates/`, rinominare l'estensione in .crt e usare `dpkg-reconfigure ca-certificates` per abilitarlo come certificato della CA di sistema. 
        - **RHEL**: copiare il certificato in `/etc/pki/ca-trust/source/anchors/` e usare `update-ca-trust` per abilitarlo come certificato della CA di sistema.
        - **SUSE**: copiare il certificato in `/usr/share/pki/trust/anchors/` e usare `update-ca-certificates` per abilitarlo come certificato della CA di sistema.
        - **Windows**:  importare il file PEM come certificato in utente corrente-> Autorità di certificazione radice attendibili-> Certificati
        - **macOS**: 
           - Copiare il certificato in `/usr/local/etc/openssl/certs`
           - Eseguire il comando seguente per ottenere il valore hash: `/usr/local/Cellar/openssl/1.0.2l/openssl x509 -hash -in mssql.pem -noout`
           - Rinominare il certificato con il valore. Ad esempio: `mv mssql.pem dc2dd900.0`. Verificare che dc2dd900.0 sia in `/usr/local/etc/openssl/certs`
    
-   **Esempi di stringhe di connessione** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![Finestra di dialogo di connessione di SQL Server Management Studio](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "Finestra di dialogo di connessione di SQL Server Management Studio")  
  
    - **SQLCMD** 

        `sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=True; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=Yes; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=true; trustServerCertificate=false;"`

## <a name="server-initiated-encryption"></a>Crittografia avviata dal server 

- **Generare il certificato** (/CN deve corrispondere al nome di dominio completo dell'host di SQL Server)

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key   
sudo mv mssql.pem /etc/ssl/certs/ 
sudo mv mssql.key /etc/ssl/private/ 
```

- **Configurare SQL Server**

```bash
systemctl stop mssql-server 
cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1 
```

-   **Esempi di stringhe di connessione** 

    - **SQLCMD**

        `sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=False; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=no; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=false; trustServerCertificate=false;"`

> [!NOTE]
> Impostare **TrustServerCertificate** su True se il client non riesce a connettersi alla CA per convalidare l'autenticità del certificato

## <a name="common-connection-errors"></a>Errori di connessione comuni  

|Messaggio di errore |Fix |
|--- |--- |
|La catena di certificati è stata emessa da un'autorità non disponibile nell'elenco locale.  |Questo errore si verifica quando i client non riescono a verificare la firma sul certificato presentato da SQL Server durante l'handshake TLS. Verificare che il client consideri attendibile direttamente il certificato di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] oppure la CA che ha firmato il certificato di SQL Server. |
|Nome principale di destinazione scorretto.  |Verificare che il campo Nome comune nel certificato di SQL Server corrisponda al nome del server specificato nella stringa di connessione del client. |  
|Connessione in corso interrotta forzatamente dall'host remoto. |Questo errore può verificarsi quando il client non supporta la versione del protocollo TLS richiesta da SQL Server. Se ad esempio la configurazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] richiede TLS 1.2, verificare che i client supportino anche il protocollo TLS 1.2. |
| | |   
