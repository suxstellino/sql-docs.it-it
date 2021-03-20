---
title: Risolvere i problemi di connettività di PolyBase Kerberos | Microsoft Docs
description: Per risolvere i problemi di autenticazione per PolyBase con un cluster Hadoop protetto con Kerberos, è possibile usare la diagnostica interattiva integrata in PolyBase.
author: alazad-msft
ms.author: alazad
ms.reviewer: mikeray
ms.technology: polybase
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/02/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 21b5f1b82a1ae15b6eaecf47c9a2480844d193cb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753761"
---
# <a name="troubleshoot-polybase-kerberos-connectivity"></a>Risolvere i problemi di connettività di PolyBase Kerberos

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

È possibile usare la diagnostica interattiva integrata in PolyBase per risolvere i problemi di autenticazione quando si usa PolyBase per un cluster Hadoop protetto con Kerberos. 

Questo articolo può essere usato come guida per eseguire il debug dei problemi tramite la diagnostica integrata.

> [!TIP]
> Invece di seguire i passaggi descritti in questa guida, è possibile scegliere di eseguire il [tester HDFS Kerberos](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/hdfs-kerberos-tester) per risolvere i problemi relativi alle connessioni HDFS Kerberos per PolyBase, quando si verifica un errore di HDFS Kerberos durante la creazione di una tabella esterna in un cluster HDFS protetto con Kerberos.
> Questo strumento consentirà di escludere i problemi non correlati a SQL Server per concentrarsi sulla risoluzione dei problemi di configurazione di HDFS Kerberos, identificando in particolare i problemi di errata configurazione di nome utente/password e di errata configurazione dei cluster Kerberos.      
> Questo strumento è completamente indipendente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È disponibile come Jupyter Notebook e richiede Azure Data Studio.

## <a name="prerequisites"></a>Prerequisiti

1. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM CU6 / [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU3 / [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] o versione successiva con PolyBase installato
1. Un cluster Hadoop (Cloudera o Hortonworks) protetto con Kerberos (Active Directory o MIT)

## <a name="introduction"></a>Introduzione
È utile per capire il protocollo Kerberos a livello generale. Sono tre gli attori coinvolti:

1. Client Kerberos (SQL Server)
1. Risorsa protetta (HDFS, MR2, YARN, cronologia processo e così via)
1. Centro distribuzione chiavi (definito controller di dominio in Active Directory)

Ogni risorsa protetta di Hadoop è registrata nel **Centro distribuzione chiavi (KDC)** con un **nome dell'entità servizio (SPN)** univoco quando Kerberos viene configurato nel cluster Hadoop. L'obiettivo è consentire al client di ottenere un ticket utente temporaneo, detto **Ticket Granting Ticket (TGT)** , in modo da richiedere un altro ticket temporaneo, detto **ticket di servizio (ST)** , dal KDC per il nome dell'entità servizio specifico a cui si vuole accedere.  

In PolyBase, quando è richiesta l'autenticazione per qualsiasi risorsa protetta con Kerberos, viene eseguito il seguente handshake a quattro vie:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si connette al KDC e ottiene un ticket TGT per l'utente. Il ticket TGT viene crittografato usando la chiave privata del KDC.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiama la risorsa protetta di Hadoop, HDFS, e determina per quale SPN è necessario un ticket di servizio.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] torna al KDC, passa di nuovo il ticket TGT e richiede un ticket di servizio per accedere a quella particolare risorsa protetta. Il ticket ST viene crittografato usando la chiave privata del servizio protetto.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inoltra il ticket di servizio in Hadoop e viene autenticato per la creazione di una sessione per quel servizio.

![PolyBase SQL Server](./media/polybase-sqlserver.png)

I problemi di autenticazione rientrano in uno o più dei quattro passaggi precedenti. Per rendere il debug più veloce, PolyBase ha introdotto uno strumento di diagnostica integrato per l'identificazione del punto di errore.

## <a name="troubleshooting"></a>Risoluzione dei problemi

PolyBase ha i seguenti file XML di configurazione che contengono le proprietà del cluster Hadoop:

- core-site.xml
- hdfs-site.xml
- hive-site.xml
- jaas.conf
- mapred-site.xml
- yarn-site.xml

Questi file si trovano in:

`\[System Drive\]:{install path}\{MSSQL##.INSTANCENAME}\MSSQL\Binn\PolyBase\Hadoop\conf`

Ad esempio, il valore predefinito per [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] è `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf`.

Aggiornare **core-site.xml** aggiungendo le tre proprietà riportate di seguito. Impostare i valori in base all'ambiente:

```xml
<property>
    <name>polybase.kerberos.realm</name>
    <value>CONTOSO.COM</value>
</property>
<property>
    <name>polybase.kerberos.kdchost</name>
    <value>kerberos.contoso.com</value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```
> [!NOTE]
> Il valore per la proprietà `polybase.kerberos.realm` deve essere in lettere maiuscole.

Gli altri XML in un secondo tempo dovranno essere a loro volta aggiornati se si vogliono eseguire operazioni di distribuzione, ma con solo questo file configurato, deve essere almeno possibile accedere al file system HDFS.

Lo strumento viene eseguito in modo indipendente da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quindi non è necessario che sia in esecuzione, né che venga riavviato se vengono aggiornati gli XML di configurazione. Per eseguire lo strumento, usare i comandi seguenti nell'host in cui è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

```cmd
> cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase  
> java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge {Name Node Address} {Name Node Port} {Service Principal} {Filepath containing Service Principal's Password} {Remote HDFS file path (optional)}
```

## <a name="arguments"></a>Argomenti

| Argomento | Descrizione|
| --- | --- |
| *Indirizzo del nodo del nome* | IP o nome di dominio completo del nodo del nome. Si riferisce all'argomento "LOCATION" in CREATE EXTERNAL DATA SOURCE T-SQL. Nota: Per la versione SQL Server 2019 dello strumento è necessario che *hdfs:\/\/* preceda l'IP o il nome di dominio completo.|
| *Porta del nodo del nome* | La porta del nodo del nome. Si riferisce all'argomento "LOCATION" in CREATE EXTERNAL DATA SOURCE T-SQL. Ad esempio, 8020. |
| *Entità servizio* | L'entità servizio di amministratore per il KDC. Corrisponde all'argomento "IDENTITY" in `CREATE DATABASE SCOPED CREDENTIAL` T-SQL.|
| *Password del servizio* | Anziché digitare la password nella console, archiviarla in un file e passare il percorso del file qui. Il contenuto del file deve corrispondere all'oggetto usato come argomento "SECRET" in `CREATE DATABASE SCOPED CREDENTIAL` T-SQL. |
| *Percorso del file HDFS remoto (facoltativo)* | Percorso di un file esistente a cui accedere. Se non è specificato, verrà usata la radice "/". |

## <a name="example"></a>Esempio

```cmd
java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
```

L'output è dettagliato per il debug avanzato, ma esistono solo quattro checkpoint principali da cercare indipendentemente dal fatto che si usi MIT o Active Directory. Corrispondono ai quattro passaggi descritti in precedenza. 

I seguenti estratti provengono da un centro distribuzione chiavi (KDC) MIT. I riferimenti agli output di esempio completi per MIT e AD sono riportati alla fine di questo articolo.

## <a name="checkpoint-1"></a>Checkpoint 1
Deve essere presente un dump esadecimale di un ticket con `Server Principal = krbtgt/MYREALM.COM@MYREALM.COM`. Indica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha eseguito correttamente l'autenticazione nel KDC e ha ricevuto un ticket TGT. In caso contrario, il problema riguarda esclusivamente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il KDC e non Hadoop.

PolyBase **non** supporta le relazioni di trust tra AD e MIT e deve essere configurato per lo stesso KDC come è stato configurato nel cluster Hadoop. In tali ambienti la creazione manuale di un account di servizio per il KDC e l'uso dell'account per eseguire l'autenticazione funzionano.

```cmd
|>>> KrbAsReq creating message 
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=143 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=143 
 >>> KrbKdcReq send: #bytes read=646 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbAsRep cons in KrbAsReq.getReply myuser 
 [2017-04-25 21:34:33,548] INFO 687[main] - com.microsoft.polybase.client.KerberosSecureLogin.secureLogin(KerberosSecureLogin.java:97) - Subject: 
 Principal: admin_user@CONTOSO.COM 
 Private Credential: Ticket (hex) = 
 0000: 61 82 01 48 30 82 01 44 A0 03 02 01 05 A1 0E 1B a..H0..D........ 
 0010: 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 21 30 .CONTOSO.COM.!0 
 0020: 1F A0 03 02 01 02 A1 18 30 16 1B 06 6B 72 62 74 ........0...krbt 
 0030: 67 74 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D gt..CONTOSO.COM 
 0040: A3 82 01 08 30 82 01 04 A0 03 02 01 10 A1 03 02 ....0........... 
 *[...Condensed...]* 
 0140: 67 6D F6 41 6C EB E0 C3 3A B2 BD B1 gm.Al...:... 
 Client Principal = admin_user@CONTOSO.COM 
 Server Principal = krbtgt/CONTOSO.COM@CONTOSO.COM 
 *[...Condensed...]* 
 [2017-04-25 21:34:34,500] INFO 1639[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1579) - Successfully authenticated against KDC server. 
```

## <a name="checkpoint-2"></a>Checkpoint 2
PolyBase tenterà di accedere all'HDFS con esito negativo perché la richiesta non contiene il ticket di servizio necessario.

```cmd
 [2017-04-25 21:34:34,501] INFO 1640[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1584) - Attempting to access external filesystem at URI: hdfs://10.193.27.232:8020 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Entered Krb5Context.initSecContext with state=STATE_NEW 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Service ticket not found in the subject 
```

## <a name="checkpoint-3"></a>Checkpoint 3
Un secondo dump esadecimale indica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha usato correttamente il TGT e ha acquisito il ticket di servizio applicabile per il nome dell'entità servizio del nodo del nome dal KDC.

```cmd
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=664 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=664 
 >>> KrbKdcReq send: #bytes read=669 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbApReq: APOptions are 00100000 00000000 00000000 00000000 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 Krb5Context setting mySeqNumber to: 1033039363 
 Created InitSecContextToken: 
 0000: 01 00 6E 82 02 4B 30 82 02 47 A0 03 02 01 05 A1 ..n..K0..G...... 
 0010: 03 02 01 0E A2 07 03 05 00 20 00 00 00 A3 82 01 ......... ...... 
 0020: 63 61 82 01 5F 30 82 01 5B A0 03 02 01 05 A1 0E ca.._0..[....... 
 0030: 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 26 ..CONTOSO.COM.& 
 0040: 30 24 A0 03 02 01 00 A1 1D 30 1B 1B 02 6E 6E 1B 0$.......0...nn. 
 0050: 15 73 68 61 73 74 61 2D 68 64 70 32 35 2D 30 30 .hadoop-hdp25-00 
 0060: 2E 6C 6F 63 61 6C A3 82 01 1A 30 82 01 16 A0 03 .local....0..... 
 0070: 02 01 10 A1 03 02 01 01 A2 82 01 08 04 82 01 04 ................ 
 *[...Condensed...]* 
 0240: 03 E3 68 72 C4 D2 8D C2 8A 63 52 1F AE 26 B6 88 ..hr.....cR..&.. 
 0250: C4 . 
```

## <a name="checkpoint-4"></a>Checkpoint 4
Infine, le proprietà del file del percorso di destinazione devono essere stampate insieme a un messaggio di conferma. Le proprietà del file confermano che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato autenticato da Hadoop usando il ticket di servizio e che a una sessione è stato concesso di accedere alla risorsa protetta.

Quando si raggiunge questo punto viene confermato che: (i) i tre attori sono in grado di comunicare correttamente, (ii) core-site.xml e jaas.conf sono corretti e (iii) il KDC ha riconosciuto le credenziali.

```cmd
 [2017-04-25 21:34:35,096] INFO 2235[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1586) - File properties for "/": FileStatus{path=hdfs://10.193.27.232:8020/; isDirectory=true; modification_time=1492028259862; access_time=0; owner=hdfs; group=hdfs; permission=rwxr-xr-x; isSymlink=false} 
 [2017-04-25 21:34:35,098] INFO 2237[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1587) - Successfully accessed the external file system. 
```

## <a name="common-errors"></a>Errori comuni
Se è stato eseguito lo strumento e le proprietà del file del percorso di destinazione *non* sono state stampate (Checkpoint 4), deve essere stata generata un'eccezione in un punto intermedio. Esaminarla e considerare il contesto in cui si è verificato nel flusso in quattro passaggi. Prendere in considerazione i seguenti problemi comuni che si possono essere verificati, nell'ordine:

| Eccezione e messaggi | Causa | 
| --- | --- |
| org.apache.hadoop.security.AccessControlException<br>SIMPLE l'autenticazione non è abilitata. Available:[TOKEN, KERBEROS] | Per core-site.xml la proprietà hadoop.security.authentication non è impostata su "KERBEROS".|
|javax.security.auth.login.LoginException<br>Client non trovato nel database Kerberos (6) - CLIENT_NOT_FOUND |    L'entità servizio dell'amministratore specificata non esiste nell'area di autenticazione specificata in core-site.xml.|
| javax.security.auth.login.LoginException<br> Checksum non è riuscita |L'entità servizio dell'amministratore esiste, ma la password non è valida. |
| Native config name: C:\Windows\krb5.ini<br>Caricamento dalla configurazione nativa | Questa messaggio indica che krb5LoginModule di Java ha rilevato configurazioni client personalizzate nel computer. Controllare le impostazioni client personalizzate perché possono essere la causa del problema. |
| javax.security.auth.login.LoginException<br>java.lang.IllegalArgumentException<br>Illegal principal name admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule: No rules applied to admin_user@CONTOSO.COM | Aggiungere la proprietà "hadoop.security.auth_to_local" a core-site.xml con le regole appropriate in base al cluster Hadoop. |
| java.net.ConnectException<br>Tentativo di accesso esterno del file System nell'URI: hdfs://10.193.27.230:8020<br>Call From IAAS16981207/10.107.0.245 to 10.193.27.230:8020 failed on connection exception |    L'autenticazione per il KDC è riuscita, ma non è stato possibile accedere al nodo del nome Hadoop. Controllare l'IP e la porta del nodo del nome. Verificare che il firewall sia disabilitato in Hadoop. |
| java.io.FileNotFoundException<br>File inesistente: /test/data.csv |    L'autenticazione è riuscita, ma il percorso specificato non esiste. Controllare il percorso o eseguire prima un test con la radice "/" . |

## <a name="debugging-tips"></a>Suggerimenti per il debug

### <a name="mit-kdc"></a>KDC MIT  
Tutti i nomi dell'entità servizio registrati con il KDC, tra cui gli amministratori, possono essere visualizzati eseguendo **kadmin.local** > (account di accesso dell'amministratore) > **listprincs** sull'host KDC o qualsiasi client KDC configurato. Se Kerberos è stato correttamente configurato nel cluster Hadoop, deve essere presente un SPN per ognuno dei servizi disponibili nel cluster, ad esempio `nn`, `dn`, `rm`, `yarn`, `spnego` e così via. Per impostazione predefinita, i file keytab corrispondenti (sostituti delle password) possono essere visualizzati in **/etc/security/keytabs**. Vengono crittografati usando la chiave privata del KDC.  

Può anche essere opportuno usare [`kinit`](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html)per verificare le credenziali di amministratore nel KDC a livello locale. Un esempio di utilizzo è: `kinit identity@MYREALM.COM`. La richiesta di una password indica che l'identità esiste.  
Per impostazione predefinita, i log del KDC sono disponibili in **/var/log/krb5kdc.log**, che include tutte le richieste di ticket, con l'indirizzo IP del client che ha effettuato la richiesta. Devono essere presenti due richieste provenienti dall'indirizzo IP del computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è stato eseguito lo strumento: la prima per il ticket TGT del server di autenticazione specificata come **AS\_REQ** e la seconda, **TGS\_REQ**, per il ticket di servizio proveniente dal server di concessione dei ticket.

```bash
 [root@MY-KDC log]# tail -2 /var/log/krb5kdc.log 
 May 09 09:48:26 MY-KDC.local krb5kdc[2547](info): **AS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **krbtgt/CONTOSO.COM@CONTOSO.COM** 
 May 09 09:48:29 MY-KDC.local krb5kdc[2547](info): **TGS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **nn/hadoop-hdp25-00.local@CONTOSO.COM** 
```

### <a name="active-directory"></a>Active Directory 
In Active Directory è possibile visualizzare i nomi dell'entità servizio passando a Pannello di controllo > Utenti e computer di Active Directory > *AreaAutenticazione* > *UnitàOrganizzativa*. Se Kerberos è configurato correttamente nel cluster Hadoop, è presente un SPN per ognuno dei servizi disponibili, ad esempio `nn`, `dn`, `rm`, `yarn`, `spnego` e così via.

### <a name="general-debugging-tips"></a>Suggerimenti generali per il debug
È utile avere esperienza Java per esaminare i log ed eseguire il debug dei problemi di Kerberos, che sono indipendenti dalla funzionalità PolyBase di SQL server.

Se si riscontrano ancora problemi ad accedere a Kerberos, attenersi alla procedura seguente per eseguire il debug:

1. Verificare di poter accedere ai dati di HDFS Kerberos dall'esterno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile: 

    - Scrivere un programma Java proprio oppure
    - Usare la classe `HdfsBridge` dalla cartella di installazione di PolyBase. Ad esempio:

      ```java
      -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
      ```

     Nell'esempio precedente, `admin_user` include solo il nome utente e nessuna parte di dominio.

2. Se non è possibile accedere ai dati di Hadoop Distributed File System Kerberos dall'esterno di PolyBase:
    - Esistono due tipi di autenticazione Kerberos: L'autenticazione Active Directory Kerberos e l'autenticazione MIT Kerberos.
    - Verificare che l'utente sia presente nell'account di dominio e usare lo stesso account utente per accedere al sistema HDFS.

3. Per Active Directory Kerberos, assicurarsi che sia possibile vedere il ticket memorizzato nella cache usando il comando `klist` in Windows.
    - Accedere al computer PolyBase ed eseguire `klist` e `klist tgt` nel prompt dei comandi per verificare se KDC, il nome utente e i tipi di crittografia sono corretti.

4. Se KDC può supportare solo AES256, assicurarsi che i [file dei criteri JCE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) siano installati.

## <a name="see-also"></a>Vedere anche
[Blog sull'integrazione di PolyBase con Cloudera usando l'autenticazione di Active Directory](/archive/blogs/microsoftrservertigerteam/integrating-polybase-with-cloudera-using-active-directory-authentication)  
[Guida di Cloudera per la configurazione di Kerberos per CDH](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_sg_principal_keytab.html)  
[Guida di Hortonworks per la configurazione di Kerberos per HDP](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/ch_configuring_amb_hdp_for_kerberos.html)  
[Risoluzione dei problemi di base](polybase-troubleshooting.md)   
[Errori di polibase e possibili soluzioni](polybase-errors-and-possible-solutions.md)   
