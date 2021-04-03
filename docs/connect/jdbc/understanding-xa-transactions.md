---
title: Informazioni sulle transazioni XA
description: Il driver JDBC di Microsoft SQL Server offre il supporto per le transazioni distribuite opzionali Java Platform, Enterprise Edition/JDBC 2.0.
ms.custom: ''
ms.date: 09/29/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 574e326f-0520-4003-bdf1-62d92c3db457
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7fa4598fc332a2f84a02b428e786ed381e388a1e
ms.sourcegitcommit: a7af7bead92044595556b8687e640a0eab0bc455
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2021
ms.locfileid: "106179881"
---
# <a name="understanding-xa-transactions"></a>Informazioni sulle transazioni XA

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] supporta transazioni distribuite facoltative Java Platform, Enterprise Edition/JDBC 2.0. Le connessioni JDBC ottenute dalla classe [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md) possono partecipare ad ambienti standard di elaborazione delle transazioni distribuite quali i server applicazioni Java Platform, Enterprise Edition (Java EE).  

In questo articolo, XA sta per l'architettura estesa.

> [!WARNING]  
> Microsoft JDBC Driver 4.2 (e versioni successive) per SQL include nuove opzioni di timeout per la funzionalità di rollback automatico delle transazioni non preparate esistente. Per ulteriori informazioni vedere [Configurazione delle impostazioni di timeout sul lato server per il rollback automatico delle transazioni non preparate](../../connect/jdbc/understanding-xa-transactions.md#BKMK_ServerSide) più avanti in questo argomento.  

## <a name="remarks"></a>Osservazioni

Le classi per l'implementazione delle transazioni distribuite sono le seguenti:  
  
| Classe                                              | Implementa                      | Descrizione                                       |
| -------------------------------------------------- | ------------------------------- | ------------------------------------------------- |
| com.microsoft.sqlserver.jdbc.SQLServerXADataSource | javax.sql.XADataSource          | Class factory per le connessioni distribuite.    |
| com.microsoft.sqlserver.jdbc.SQLServerXAResource   | javax.transaction.xa.XAResource | Adattatore di risorse per il responsabile delle transazioni. |
  
> [!NOTE]  
> Le connessioni delle transazioni distribuite XA sono impostate sul livello di isolamento predefinito Read Committed.  
  
## <a name="guidelines-and-limitations-when-using-xa-transactions"></a>Linee guida e limitazioni relative all'uso di transazioni XA  

Le seguenti linee guida aggiuntive si applicano alle transazioni a "regime di controllo stretto ("tightly-coupled")":  

- Quando si usano le transazioni XA con Microsoft Distributed Transaction Coordinator (MS DTC), è possibile che la versione corrente di MS DTC non supporti il comportamento dei rami XA a regime di controllo stretto ("tightly-coupled"). In MS DTC è ad esempio presente un mapping uno-a-uno tra un ID di transazione dei rami XA (XID) e un ID di transazione MS DTC e il lavoro eseguito da ognuno dei rami XA a regime di controllo libero ("loosely-coupled") viene isolato da quello degli altri rami.  
  
- MS DTC fornisce anche il supporto per i rami XA strettamente associati in caso di mapping tra più rami XA con lo stesso ID di transazione globale (GTRID) e un singolo ID di transazione MS DTC. Questo supporto consente a più rami XA strettamente associati di vedere le modifiche degli altri rami nello strumento di gestione delle risorse, ad esempio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
- Un flag [SSTRANSTIGHTLYCPLD](../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) consente alle applicazioni di usare le transazioni XA strettamente associate con ID di transazione dei rami XA (BQUAL) diversi ma con lo stesso ID di transazione globale (GTRID) e ID di formato (FormatID). Per utilizzare questa funzionalità è necessario impostare [SSTRANSTIGHTLYCPLD](../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) nel parametro flag del metodo XAResource.start:
  
    ```java
    xaRes.start(xid, SQLServerXAResource.SSTRANSTIGHTLYCPLD);  
    ```  
  
## <a name="configuration-instructions"></a>Istruzioni di configurazione

Per utilizzare le origini dei dati XA con Microsoft Distributed Transaction Coordinator (MS DTC) per la gestione delle transazioni distribuite, è necessario utilizzare la procedura seguente.  

> [!NOTE]  
> I componenti della transazione distribuita JDBC sono inclusi nella directory xa dell'installazione del driver JDBC. Tali componenti includono i file xa_install.sql e sqljdbc_xa.dll. Se sono presenti versioni diverse di JDBC Driver in client diversi, è consigliabile usare il file sqljdbc_xa.dll più recente nel server.  

> [!NOTE]  
> I componenti di transazione distribuiti JDBC XA sono inclusi nel motore di SQL Server in SQL Server 2017 a partire dall'aggiornamento cumulativo 16 e in SQL Server 2019 e possono essere abilitati o disabilitati con una stored procedure di sistema. Il sqjdbc_xa.dll dal driver non è obbligatorio ed è consigliabile abilitare i componenti server per queste versioni del server.
> Per abilitare i componenti necessari per l'esecuzione di transazioni distribuite XA tramite il driver JDBC, eseguire la stored procedure seguente.
>
> EXEC sp_sqljdbc_xa_install
>
> Per disabilitare i componenti installati in precedenza, eseguire la stored procedure seguente.
>
> EXEC sp_sqljdbc_xa_uninstall

### <a name="running-the-ms-dtc-service"></a>Esecuzione del servizio MS DTC

Il servizio MS DTC deve essere contrassegnato come **Automatico** in Service Manager per assicurarsi che sia in esecuzione all'avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per abilitare MS DTC per le transazioni XA, procedere come segue:  
  
In Windows Vista e versioni successive:  
  
1. Fare clic sul pulsante **Start**, digitare **dcomcnfg** nella casella **Cerca** del menu Start e quindi premere INVIO per aprire **Servizi componenti**. Per aprire **Servizi componenti** è anche possibile digitare %windir%\system32\comexp.msc nella casella **Cerca**.  
  
2. Espandere Servizi componenti, Computer, Computer locale, quindi Distributed Transaction Coordinator.  
  
3. Fare clic con il pulsante destro del mouse su **DTC locale** e quindi scegliere **Proprietà**.  
  
4. Fare clic sulla scheda **Sicurezza** nella finestra di dialogo **Proprietà - DTC locale**.  
  
5. Selezionare la casella di controllo **Abilita transazioni XA** e quindi fare clic su **OK**. Questa azione causerà il riavvio di un servizio MS DTC.
  
6. Fare di nuovo clic su **OK** per chiudere la finestra di dialogo **Proprietà** e quindi chiudere **Servizi componenti**.  
  
7. Arrestare e riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per assicurarsi che venga eseguita la sincronizzazione con le modifiche di MS DTC.  

### <a name="configuring-the-jdbc-distributed-transaction-components"></a>Configurazione dei componenti delle transazioni distribuite JDBC  

Per configurare i componenti delle transazioni distribuite del driver JDBC, procedere come segue:  
  
1. Copiare il nuovo file sqljdbc_xa.dll dalla directory di installazione del driver JDBC alla directory Binn di ogni computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che parteciperà alle transazioni distribuite.  
  
    > [!NOTE]  
    > Se si stanno usando transazioni XA con una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a 32 bit, usare il file sqljdbc_xa.dll nella cartella x86, anche se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è installato in un computer con processore x64. Se si usano transazioni XA con una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a 64 bit e un processore x64, usare il file sqljdbc_xa.dll nella cartella x64.  
  
2. Eseguire lo script di database xa_install.sql in ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che parteciperà alle transazioni distribuite. Lo script installa le stored procedure estese chiamate da sqljdbc_xa.dll. Tali stored procedure estese implementano il supporto per XA e per le transazioni distribuite per [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Sarà necessario eseguire lo script come amministratore dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3. Per concedere a un utente specifico l'autorizzazione necessaria per partecipare alle transazioni distribuite con il driver JDBC, aggiungere l'utente al ruolo SqlJDBCXAUser.  
  
È possibile configurare solo una versione alla volta dell'assembly sqljdbc_xa.dll in ogni istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile che le applicazioni debbano usare versioni diverse del driver JDBC per connettersi alla stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite la stessa connessione XA. In tal caso è necessario installare nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il file sqljdbc_xa.dll, fornito con la versione più recente del driver JDBC.  
  
È possibile verificare la versione del file sqljdbc_xa.dll attualmente installata nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in tre modi:
  
1. Aprire la directory LOG del computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che parteciperà alle transazioni distribuite. Selezionare e aprire il file "ERRORLOG" di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eseguire la ricerca della frase "Using 'SQLJDBC_XA.dll' version ..." nel file "ERRORLOG".  
  
2. Aprire la directory Binn del computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che parteciperà alle transazioni distribuite. Selezionare l'assembly sqljdbc_xa.dll.

    - In Windows Vista o versioni successive: fare clic con il pulsante destro del mouse su sqljdbc_xa.dll, quindi scegliere Proprietà. Fare clic sulla scheda **Dettagli**. Nel campo **Versione file** è indicata la versione del file sqljdbc_xa.dll attualmente installata nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3. Impostare la funzionalità di registrazione come illustrato nell'esempio di codice della sezione successiva. Eseguire la ricerca della frase "Server XA DLL version:..." nel file del log di output.  

### <a name="configuring-server-side-timeout-settings-for-automatic-rollback-of-unprepared-transactions"></a><a name="BKMK_ServerSide"></a> Configurazione delle impostazioni di timeout sul lato server per il rollback automatico delle transazioni non preparate  

> [!WARNING]  
> Questa opzione sul lato server è stata introdotta con Microsoft JDBC Driver 4.2 (e versioni successive) per SQL Server. Per ottenere il comportamento aggiornato, verificare che il file sqljdbc_xa.dll nel server sia aggiornato. Per altre informazioni sull'impostazione dei timeout lato client, vedere [XAResource.setTransactionTimeout()](https://docs.oracle.com/javase/8/docs/api/javax/transaction/xa/XAResource.html).  

Sono disponibili due impostazioni del Registro di sistema (valori DWORD) per controllare il comportamento di timeout delle transazioni distribuite:  
  
- **XADefaultTimeout** (in secondi): il valore di timeout predefinito da utilizzare quando l'utente non specifica alcun timeout. Il valore predefinito è 0.  
  
- **XAMaxTimeout** (in secondi): il valore massimo di timeout che un utente può impostare. Il valore predefinito è 0.  
  
Queste impostazioni sono specifiche dell'istanza di SQL Server e devono essere create nella seguente chiave del Registro di sistema:  

```bash
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL<version>.<instance_name>\XATimeout  
```

> [!NOTE]  
> Per SQL Server a 32 bit in esecuzione in computer a 64 bit, le impostazioni del Registro di sistema devono essere create nella seguente chiave: `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Microsoft SQL Server\MSSQL<version>.<instance_name>\XATimeout`
  
Un valore di timeout viene impostato per ogni transazione all'avvio e SQL Server esegue il rollback della transazione se il timeout scade. Il timeout viene determinato in base a queste impostazioni del Registro di sistema e a seconda di ciò che ha specificato l'utente tramite XAResource.setTransactionTimeout(). Di seguito sono riportati alcuni esempi su come vengono interpretati questi valori di timeout:  
  
- `XADefaultTimeout = 0`, `XAMaxTimeout = 0`
  
     Indica che non verrà utilizzato alcun timeout predefinito e non verrà applicato alcun timeout massimo per i client. In questo caso, le transazioni avranno un timeout solo se il client imposta un timeout utilizzando XAResource.setTransactionTimeout.  
  
- `XADefaultTimeout = 60`, `XAMaxTimeout = 0`
  
     Indica che tutte le transazioni avranno un timeout di 60 secondi se il client non specifica alcun timeout. Se il client specifica un timeout, verrà utilizzato tale valore di timeout. Non viene applicato alcun valore massimo per il timeout.  
  
- `XADefaultTimeout = 30`, `XAMaxTimeout = 60`
  
     Indica che tutte le transazioni avranno un timeout di 30 secondi se il client non specifica alcun timeout. Se il client specifica un timeout, viene utilizzato il timeout del client, a condizione che sia minore di 60 secondi (il valore massimo).  
  
- `XADefaultTimeout = 0`, `XAMaxTimeout = 30`
  
     Indica che tutte le transazioni avranno un timeout di 30 secondi (il valore massimo) se il client non specifica alcun timeout. Se il client specifica un timeout, viene utilizzato il timeout del client, a condizione che sia minore di 30 secondi (il valore massimo).  
  
### <a name="upgrading-sqljdbc_xadll"></a>Aggiornamento di sqljdbc_xa.dll

Quando si installa una nuova versione del driver JDBC, è consigliabile utilizzare il file sqljdbc_xa.dll della nuova versione per aggiornare il file sqljdbc_xa.dll nel server.  
  
> [!IMPORTANT]  
> Aggiornare il file sqljdbc_xa.dll durante il periodo di manutenzione o quando non è in corso nessuna transazione MS DTC.
  
1. Scaricare il file sqljdbc_xa.dll mediante il comando [!INCLUDE[tsql](../../includes/tsql-md.md)] **DBCC sqljdbc_xa (FREE)** .  
  
2. Copiare il nuovo file sqljdbc_xa.dll dalla directory di installazione del driver JDBC alla directory Binn di ogni computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che parteciperà alle transazioni distribuite.  
  
    La nuova DLL verrà caricata quando viene chiamata una procedura estesa in sqljdbc_xa.dll. Non è necessario riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per caricare le nuove definizioni.  
  
### <a name="configuring-the-user-defined-roles"></a>Configurazione dei ruoli definiti dall'utente

Per concedere a un utente specifico l'autorizzazione necessaria per partecipare alle transazioni distribuite con il driver JDBC, aggiungere l'utente al ruolo SqlJDBCXAUser. Ad esempio, usare il codice [!INCLUDE[tsql](../../includes/tsql-md.md)] seguente per aggiungere un utente denominato 'shelly' (utente di accesso standard SQL denominato 'shelly') al ruolo SqlJDBCXAUser:  

```sql
USE master  
GO  
EXEC sp_grantdbaccess 'shelly', 'shelly'  
GO  
EXEC sp_addrolemember [SqlJDBCXAUser], 'shelly'  
```

I ruoli SQL definiti dall'utente vengono definiti per database. Per creare il proprio ruolo al fine di ottenere maggiore sicurezza, sarà necessario definire il ruolo in ciascun database e aggiungere gli utenti per database. Il ruolo SqlJDBCXAUser è rigorosamente definito nel database master poiché viene usato per concedere l'accesso alle stored procedure estese SQL JDBC che risiedono nel master. Sarà necessario concedere prima l'accesso al master ai singoli utenti, quindi consentire l'accesso al ruolo SqlJDBCXAUser una volta connessi al database master.  

## <a name="example"></a>Esempio  

```java
import java.net.Inet4Address;
import java.sql.*;
import java.util.Random;
import javax.sql.XAConnection;
import javax.transaction.xa.*;
import com.microsoft.sqlserver.jdbc.*;


public class testXA {

    public static void main(String[] args) throws Exception {

        // Create variables for the connection string.
        String prefix = "jdbc:sqlserver://";
        String serverName = "localhost";
        int portNumber = 1433;
        String databaseName = "AdventureWorks";
        String user = "UserName";
        String password = "*****";

        String connectionUrl = prefix + serverName + ":" + portNumber + ";databaseName=" + databaseName + ";user="
                + user + ";password=" + password;

        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");

        try (Connection con = DriverManager.getConnection(connectionUrl); Statement stmt = con.createStatement()) {
            stmt.executeUpdate("CREATE TABLE XAMin (f1 int, f2 varchar(max))");

        }
        // Create the XA data source and XA ready connection.
        SQLServerXADataSource ds = new SQLServerXADataSource();
        ds.setUser(user);
        ds.setPassword(password);
        ds.setServerName(serverName);
        ds.setPortNumber(portNumber);
        ds.setDatabaseName(databaseName);

        XAConnection xaCon = ds.getXAConnection();
        try (Connection con = xaCon.getConnection()) {

            // Get a unique Xid object for testing.
            XAResource xaRes = null;
            Xid xid = null;
            xid = XidImpl.getUniqueXid(1);

            // Get the XAResource object and set the timeout value.
            xaRes = xaCon.getXAResource();
            xaRes.setTransactionTimeout(0);

            // Perform the XA transaction.
            System.out.println("Write -> xid = " + xid.toString());
            xaRes.start(xid, XAResource.TMNOFLAGS);
            PreparedStatement pstmt = con.prepareStatement("INSERT INTO XAMin (f1,f2) VALUES (?, ?)");
            pstmt.setInt(1, 1);
            pstmt.setString(2, xid.toString());
            pstmt.executeUpdate();

            // Commit the transaction.
            xaRes.end(xid, XAResource.TMSUCCESS);
            xaRes.commit(xid, true);
        }
        xaCon.close();

        // Open a new connection and read back the record to verify that it worked.
        try (Connection con = DriverManager.getConnection(connectionUrl); Statement stmt = con.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT * FROM XAMin")) {
            rs.next();
            System.out.println("Read -> xid = " + rs.getString(2));
            stmt.executeUpdate("DROP TABLE XAMin");
        }
    }
}


class XidImpl implements Xid {

    public int formatId;
    public byte[] gtrid;
    public byte[] bqual;

    public byte[] getGlobalTransactionId() {
        return gtrid;
    }

    public byte[] getBranchQualifier() {
        return bqual;
    }

    public int getFormatId() {
        return formatId;
    }

    XidImpl(int formatId, byte[] gtrid, byte[] bqual) {
        this.formatId = formatId;
        this.gtrid = gtrid;
        this.bqual = bqual;
    }

    public String toString() {
        int hexVal;
        StringBuffer sb = new StringBuffer(512);
        sb.append("formatId=" + formatId);
        sb.append(" gtrid(" + gtrid.length + ")={0x");
        for (int i = 0; i < gtrid.length; i++) {
            hexVal = gtrid[i] & 0xFF;
            if (hexVal < 0x10)
                sb.append("0" + Integer.toHexString(gtrid[i] & 0xFF));
            else
                sb.append(Integer.toHexString(gtrid[i] & 0xFF));
        }
        sb.append("} bqual(" + bqual.length + ")={0x");
        for (int i = 0; i < bqual.length; i++) {
            hexVal = bqual[i] & 0xFF;
            if (hexVal < 0x10)
                sb.append("0" + Integer.toHexString(bqual[i] & 0xFF));
            else
                sb.append(Integer.toHexString(bqual[i] & 0xFF));
        }
        sb.append("}");
        return sb.toString();
    }

    // Returns a globally unique transaction id.
    static byte[] localIP = null;
    static int txnUniqueID = 0;

    static Xid getUniqueXid(int tid) {

        Random rnd = new Random(System.currentTimeMillis());
        txnUniqueID++;
        int txnUID = txnUniqueID;
        int tidID = tid;
        int randID = rnd.nextInt();
        byte[] gtrid = new byte[64];
        byte[] bqual = new byte[64];
        if (null == localIP) {
            try {
                localIP = Inet4Address.getLocalHost().getAddress();
            } catch (Exception ex) {
                localIP = new byte[] {0x01, 0x02, 0x03, 0x04};
            }
        }
        System.arraycopy(localIP, 0, gtrid, 0, 4);
        System.arraycopy(localIP, 0, bqual, 0, 4);

        // Bytes 4 -> 7 - unique transaction id.
        // Bytes 8 ->11 - thread id.
        // Bytes 12->15 - random number generated by using seed from current time in milliseconds.
        for (int i = 0; i <= 3; i++) {
            gtrid[i + 4] = (byte) (txnUID % 0x100);
            bqual[i + 4] = (byte) (txnUID % 0x100);
            txnUID >>= 8;
            gtrid[i + 8] = (byte) (tidID % 0x100);
            bqual[i + 8] = (byte) (tidID % 0x100);
            tidID >>= 8;
            gtrid[i + 12] = (byte) (randID % 0x100);
            bqual[i + 12] = (byte) (randID % 0x100);
            randID >>= 8;
        }
        return new XidImpl(0x1234, gtrid, bqual);
    }
}

```

## <a name="see-also"></a>Vedere anche  

[Esecuzione di transazioni con il driver JDBC](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
