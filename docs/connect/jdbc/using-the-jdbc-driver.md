---
title: Uso del driver JDBC | Microsoft Docs
description: In questa sezione viene fornita una guida introduttiva alla creazione di una connessione semplice a un database SQL Server usando Microsoft JDBC Driver per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6faaf05b-8b70-4ed2-9b44-eee5897f1cd0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b3550536a3512315d511efc831adc27b35ec974
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195255"
---
# <a name="using-the-jdbc-driver"></a>Uso del driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Questa sezione include istruzioni introduttive per creare una connessione semplice a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Prima di connettersi a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è innanzitutto necessario installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer locale o in un server e quindi installare il driver JDBC nel computer locale.  
  
## <a name="choosing-the-right-jar-file"></a>Scelta del file JAR corretto

Microsoft JDBC Driver fornisce diversi file JAR da usare in base alle impostazioni JRE (Java Runtime Environment) preferite, come illustrato di seguito:

Microsoft JDBC driver 9,2 per SQL Server fornisce i file di libreria di classi **MSSQL-JDBC-9.2.0. jre8. jar**, **MSSQL-JDBC-9.2.0. jre11. jar** e **MSSQL-JDBC-9.2.0. JRE15. jar** .

Microsoft JDBC Driver 8.4 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** e **mssql-jdbc-8.4.1.jre14.jar**.

Microsoft JDBC Driver 8.2 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** e **mssql-jdbc-8.2.2.jre13.jar**.

Microsoft JDBC Driver 7.4 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** e **mssql-jdbc-7.4.1.jre12.jar**.

Microsoft JDBC Driver 7.2 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-7.2.2.jre8.jar** e **mssql-jdbc-7.2.2.jre11.jar**.

Microsoft JDBC Driver 7.0 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-7.0.0.jre8.jar** e **mssql-jdbc-7.0.0.jre10.jar**.

Microsoft JDBC Driver 6.4 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** e **mssql-jdbc-6.4.0.jre9.jar**.

Microsoft JDBC Driver 6.2 per SQL Server fornisce i file della libreria di classi **mssql-jdbc-6.2.2.jre7.jar** e **mssql-jdbc-6.2.2.jre8.jar**.
  
Microsoft JDBC Drivers 6.0 e 4.2 per SQL Server forniscono i file della libreria di classi **sqljdbc41.jar** e **sqljdbc42.jar**.
  
Microsoft JDBC Driver 4.1 per SQL Server fornisce i file della libreria di classi **sqljdbc41.jar**.

La scelta determinerà anche le funzionalità disponibili. Per altre informazioni su quale file JAR scegliere, vedere [Requisiti di sistema per il driver JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="setting-the-classpath"></a>Impostazione del classpath

I file JAR di Microsoft JDBC Driver non fanno parte di Java SDK e devono essere inclusi nel classpath dell'applicazione dell'utente.

Se si usa il driver JDBC 4.1 o 4.2, impostare il classpath per l'inclusione del file **sqljdbc41.jar** o **sqljdbc42.jar** dal download del driver corrispondente.

Se si usa il driver JDBC 6.2, impostare il classpath per l'inclusione del file **mssql-jdbc-6.2.2.jre7.jar** o **mssql-jdbc-6.2.2.jre8.jar**.

Se si usa il driver JDBC 6.4, impostare il classpath per l'inclusione del file **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** o **mssql-jdbc-6.4.0.jre9.jar**.

Se si usa il driver JDBC 7.0, impostare il classpath per l'inclusione del file **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar**.

Se si usa il driver JDBC 7.2, impostare il classpath per l'inclusione del file **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar**.

Se si usa il driver JDBC 7.4, impostare il classpath per l'inclusione del file **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** o **mssql-jdbc-7.4.1.jre12.jar**.

Se si usa il driver JDBC 8.2, impostare il classpath per l'inclusione del file **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar**.

Se si usa il driver JDBC 8.4, impostare il classpath per l'inclusione del file **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** o **mssql-jdbc-8.4.1.jre14.jar**.

Se si usa il driver JDBC 9,2, impostare il classpath in modo da includere **MSSQL-JDBC-9.2.0. jre8. jar**, **MSSQL-JDBC-9.2.0. jre11. jar** o **MSSQL-JDBC-9.2.0. JRE15. jar**.

Se nel classpath manca una voce per il file JAR necessario, un'applicazione genera l'eccezione comune `Class not found`.  

### <a name="for-microsoft-jdbc-driver-92"></a>Per Microsoft JDBC driver 9,2

I file **MSSQL-JDBC-9.2.0. jre8. jar**, **MSSQL-JDBC-9.2.0. jre11. jar** o **MSSQL-JDBC-9.2.0. JRE15. jar** vengono installati nei percorsi seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.0.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.0.jre15.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 9.2 for SQL Server\sqljdbc_9.2\enu\mssql-jdbc-9.2.0.jre11.jar`

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_9.2/enu/mssql-jdbc-9.2.0.jre11.jar`

Verificare che l'istruzione CLASSPATH contenga solo una [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] , ad esempio **MSSQL-JDBC-9.2.0. jre8. jar**, **MSSQL-JDBC-9.2.0. jre11. jar** o **MSSQL-JDBC-9.2.0. JRE15. jar**.


### <a name="for-microsoft-jdbc-driver-84"></a>Per Microsoft JDBC Driver 8.4

I file **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** e **mssql-jdbc-8.4.1.jre14.jar** vengono installati nelle posizioni seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre14.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.4 for SQL Server\sqljdbc_8.4\enu\mssql-jdbc-8.4.1.jre11.jar`

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.4/enu/mssql-jdbc-8.4.1.jre11.jar`

Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** o **mssql-jdbc-8.4.1.jre14.jar**.


### <a name="for-microsoft-jdbc-driver-82"></a>Per Microsoft JDBC Driver 8.2

Il file **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar** viene installato nelle posizioni seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre13.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.2 for SQL Server\sqljdbc_8.2\enu\mssql-jdbc-8.2.2.jre11.jar`

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.2/enu/mssql-jdbc-8.2.2.jre11.jar`

Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** o **mssql-jdbc-8.2.2.jre13.jar**.

### <a name="for-microsoft-jdbc-driver-74"></a>Per Microsoft JDBC Driver 7.4

Il file **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar**, o **mssql-jdbc-7.4.1.jre12.jar** viene installato nelle posizioni seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre12.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.4 for SQL Server\sqljdbc_7.4\enu\mssql-jdbc-7.4.1.jre11.jar`

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.4/enu/mssql-jdbc-7.4.1.jre11.jar`

Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar**, o **mssql-jdbc-7.4.1.jre12.jar**.

### <a name="for-microsoft-jdbc-driver-72"></a>Per Microsoft JDBC Driver 7.2

Il file **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar** viene installato nelle posizioni seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre11.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.2 for SQL Server\sqljdbc_7.2\enu\mssql-jdbc-7.2.2.jre11.jar`

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.2/enu/mssql-jdbc-7.2.2.jre11.jar`

Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio **mssql-jdbc-7.2.2.jre8.jar** o **mssql-jdbc-7.2.2.jre11.jar**.
  
### <a name="for-microsoft-jdbc-driver-70"></a>Per Microsoft JDBC Driver 7.0

Il file **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar** viene installato nelle posizioni seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre10.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:  

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.0 for SQL Server\sqljdbc_7.0\enu\mssql-jdbc-7.0.0.jre10.jar`  
  
Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre10.jar`  
  
Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio **mssql-jdbc-7.0.0.jre8.jar** o **mssql-jdbc-7.0.0.jre10.jar**.

### <a name="for-microsoft-jdbc-driver-64"></a>Per Microsoft JDBC Driver 6.4

Il file **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar, o **mssql-jdbc-6.4.0.jre9.jar** viene installato nelle posizioni seguenti:  

```bash  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre9.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.4 for SQL Server\sqljdbc_6.4\enu\mssql-jdbc-6.4.0.jre9.jar`  
  
Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.4/enu/mssql-jdbc-6.4.0.jre9.jar`  
  
Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar o **mssql-jdbc-6.4.0.jre9.jar**.

### <a name="for-microsoft-jdbc-driver-62"></a>Per Microsoft JDBC Driver 6.2

Il file **mssql-jdbc-6.2.2.jre7.jar** o **mssql-jdbc-6.2.2.jre8.jar** viene installato nelle posizioni seguenti:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre8.jar
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.2 for SQL Server\sqljdbc_6.2\enu\mssql-jdbc-6.2.2.jre8.jar`  
  
Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.2/enu/mssql-jdbc-6.2.2.jre8.jar`  
  
Assicurarsi che l'istruzione CLASSPATH contenga un solo [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio mssql-jdbc-6.2.2.jre7.jar o mssql-jdbc-6.2.2.jre8.jar.  

### <a name="for-microsoft-jdbc-driver-41-42-and-60"></a>Per Microsoft JDBC Driver 4.1, 4.2 e 6.0

I file sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar e sqljdbc42.jar vengono installati nei percorsi seguenti:  

```bash
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc4.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc41.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc42.jar  
```

Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.0 for SQL Server\sqljdbc_4.2\enu\sqljdbc42.jar`  
  
Il frammento di codice seguente è un esempio di istruzione CLASSPATH usata per un'applicazione Unix/Linux:  

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_4.2/enu/sqljdbc42.jar`  
  
Verificare che l'istruzione CLASSPATH contenga solo un [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ad esempio sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar.  
  
> [!NOTE]  
> Nei sistemi Windows i nomi di directory con lunghezza superiore a quella della convenzione 8.3 o i nomi delle cartelle con spazi possono causare problemi con i classpath. In questo caso si consiglia di spostare provvisoriamente il file sqljdbc.jar file, sqljdbc4.jar file o sqljdbc41.jar in un nome di directory semplice, ad esempio `C:\Temp`, modificare il classpath e verificare se in questo modo il problema è stato risolto.  
  
### <a name="applications-that-are-run-directly-at-the-command-prompt"></a>Applicazioni eseguite direttamente dal prompt dei comandi

Il classpath è configurato nel sistema operativo. Accodare sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar al classpath del sistema. In alternativa, specificare il classpath nella riga di comando Java che esegue l'applicazione usando l'opzione `java -classpath`.  
  
### <a name="applications-that-run-in-an-ide"></a>Applicazioni eseguite in un IDE  

Ogni fornitore IDE offre un metodo diverso per impostare il classpath nel proprio IDE. La semplice impostazione del classpath nel sistema operativo non funzionerà. È necessario aggiungere sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar al classpath IDE.  
  
### <a name="servlets-and-jsps"></a>Servlet e JSP  

Servlet e JSP vengono eseguiti in un motore servlet/JSP, ad esempio Tomcat. Il classpath deve essere impostato in base alla documentazione del motore del servlet/JSP. La semplice impostazione del classpath nel sistema operativo non funzionerà. Alcuni motori del servlet/JSP offrono schermate di installazione utilizzabili per impostare il classpath del motore. In questo caso è necessario accodare il file JDBC Driver JAR corretto al classpath del motore esistente e riavviare il motore. In altri casi è possibile distribuire il driver copiando il file sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar in una directory specifica, ad esempio lib, durante l'installazione del motore. Il classpath del driver del motore può anche essere indicato in un file di configurazione specifico.  
  
### <a name="enterprise-java-beans"></a>Enterprise Java Beans  

Le piattaforme Enterprise Java Beans (EJB) vengono eseguite in un contenitore EJB. I contenitori EJB provengono da vari fornitori. Le applet Java vengono eseguite in un browser ma scaricate da un server Web. Copiare il file sqljdbc.jar, sqljdbc4.jar o sqljdbc41.jar nella radice del server Web e specificare il nome del file JAR nella scheda di archiviazione HTML dell'applet, ad esempio `<applet ... archive=mssql-jdbc-**_.jar>`.  
  
## <a name="making-a-simple-connection-to-a-database"></a>Creazione di una connessione semplice a un database

Se si utilizza la libreria di classi sqljdbc.jar, le applicazioni devono innanzitutto registrare il driver, come indicato di seguito:  
  
`Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");`  

Quando viene caricato il driver, è possibile stabilire una connessione tramite un URL di connessione e il metodo getConnection della classe DriverManager:

```java
String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
   "databaseName=AdventureWorks;user=MyUserName;password=_****;";  
Connection con = DriverManager.getConnection(connectionUrl);  
```

A partire dall'API di JDBC 4.0 il metodo `DriverManager.getConnection()` è stato ottimizzato in modo da consentire il caricamento automatico dei driver JDBC. Le applicazioni non devono pertanto chiamare il metodo `Class.forName` per registrare o caricare il driver quando si usano librerie jar del driver.  
  
Quando si chiama il metodo getConnection della classe DriverManager, viene individuato un driver appropriato nel set di driver JDBC registrati. Il file sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar include il file "META-INF/services/java.sql.Driver", che contiene **com.microsoft.sqlserver.jdbc.SQLServerDriver** come driver registrato. Le applicazioni esistenti, che attualmente caricano i driver usando il metodo Class.forName, continueranno a funzionare senza alcuna modifica.  
  
> [!NOTE]  
> la libreria di classi sqljdbc4.jar, sqljdbc41.jar o sqljdbc42.jar non può essere utilizzata con le versioni precedenti di Java Runtime Environment (JRE). Visualizzare [Requisiti di sistema per il driver JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md) per l'elenco delle versioni JRE supportate da [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  

Per altre informazioni su come connettersi con le origini dati e usare un URL di connessione, vedere [Costruzione dell'URL della connessione](../../connect/jdbc/building-the-connection-url.md) e [Impostazione delle proprietà delle connessioni](../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Vedere anche  

[Panoramica del driver JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
