---
description: Requisiti di sistema per il driver JDBC
title: Requisiti di sistema per il driver JDBC | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 447792bb-f39b-49b4-9fd0-1ef4154c74ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 56ce0af2be925244e4ca2013b40d2b98060bc974
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464809"
---
# <a name="system-requirements-for-the-jdbc-driver"></a>Requisiti di sistema per il driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Per accedere a dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] tramite [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], è necessario che nel computer siano installati i componenti seguenti:

- [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ([download](download-microsoft-jdbc-driver-for-sql-server.md))
- Java Runtime Environment

## <a name="java-runtime-environment-requirements"></a>Requisiti di Java Runtime Environment  

 A partire da Microsoft JDBC driver 9,2 per SQL Server, sono supportati Java Development Kit (JDK) 15,0 e Java Runtime Environment (JRE) 15,0.

 A partire da Microsoft JDBC Driver 8.4 per SQL Server sono supportati Java Development Kit (JDK) 14.0 e Java Runtime Environment (JRE) 14.0.

 A partire da Microsoft JDBC Driver 8.2 per SQL Server sono supportati Java Development Kit (JDK) 13.0 e Java Runtime Environment (JRE) 13.0.

 A partire da Microsoft JDBC Driver 7.4 per SQL Server sono supportati Java Development Kit (JDK) 12.0 e Java Runtime Environment (JRE) 12.0.

 A partire da Microsoft JDBC Driver 7.2 per SQL Server sono supportati Java Development Kit (JDK) 11.0 e Java Runtime Environment (JRE) 11.0.

 A partire da Microsoft JDBC Driver 7.0 per SQL Server sono supportati Java Development Kit (JDK) 10.0 e Java Runtime Environment (JRE) 10.0.

 A partire da Microsoft JDBC Driver 6.4 per SQL Server sono supportati Java Development Kit (JDK) 9.0 e Java Runtime Environment (JRE) 9.0.

 A partire da Microsoft JDBC Driver 4.2 per SQL Server sono supportati Java Development Kit (JDK) 8.0 e Java Runtime Environment (JRE) 8.0. Il supporto per l'API specifiche JDBC è stato esteso in modo da includere l'API JDBC 4,1 e 4,2.
  
 A partire da Microsoft JDBC Driver 4.1 per SQL Server sono supportati Java Development Kit (JDK) 7.0 e Java Runtime Environment (JRE) 7.0.
  
 A partire da [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)] , il supporto del driver JDBC per l'API della specifica JDBC è stato esteso in modo da includere l'API jdbc 4,0. L'API JDBC 4.0 è stata introdotta come parte di Java Development Kit (JDK) 6.0 e Java Runtime Environment (JRE) 6.0. JDBC 4.0 è un superset dell'API di JDBC 3.0.
  
 Quando si distribuisce [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] nei sistemi operativi Windows e UNIX, è necessario usare i pacchetti di installazione, rispettivamente *sqljdbc_\<version>_enu.exe* e *sqljdbc_\<version>_enu.tar.gz*. Per altre informazioni su come distribuire il driver JDBC, vedere l'argomento [Distribuzione del driver JDBC](../../connect/jdbc/deploying-the-jdbc-driver.md).  

**Microsoft JDBC driver 9,2 per SQL Server:**  

  Il driver JDBC 9,2 include tre librerie di classi JAR in ogni pacchetto di installazione: **MSSQL-JDBC-9.2.1. jre8. jar**, **MSSQL-JDBC-9.2.1. jre11. jar** e **MSSQL-JDBC-9.2.1. JRE15. jar**.

  Il driver JDBC 9,2 è progettato per funzionare con e essere supportato da tutte le principali macchine virtuali Java, ma viene testato solo in OpenJDK 1,8, OpenJDK 11,0, OpenJDK 15,0, Azul Zulu JRE 1,8, Azul Zulu JRE 11,0 e Azul Zulu JRE 15,0.
  
  Nel grafico seguente viene riepilogato il supporto fornito dai due file JAR inclusi con Microsoft JDBC driver 9,2 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|MSSQL-JDBC-9.2.1. jre8. jar|4.2|8|Richiede Java Runtime Environment (JRE) versione 1.8. L'uso di JRE 1.7 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità disponibili in 9,2 includono il supporto per JDK 15, il supporto per l'autenticazione Azure Active Directory interattiva, il supporto per l'autenticazione Azure Active Directory entità servizio e il supporto per useBulkCopyForBatchInsert per i server di analisi sinapsi non di Azure. |
|MSSQL-JDBC-9.2.1. jre11. jar|4.3|11|Richiede Java Runtime Environment (JRE) 11.0. L'utilizzo di JRE 10.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità disponibili in 9,2 includono il supporto per JDK 15, il supporto per l'autenticazione Azure Active Directory interattiva, il supporto per l'autenticazione Azure Active Directory entità servizio e il supporto per useBulkCopyForBatchInsert per i server di analisi sinapsi non di Azure. |
|MSSQL-JDBC-9.2.1. JRE15. jar|4.3|15|Richiede un Java Runtime Environment (JRE) 15,0. Se si usa JRE 14,0 o Lower, viene generata un'eccezione.<br /><br /> Le nuove funzionalità disponibili in 9,2 includono il supporto per JDK 15, il supporto per l'autenticazione Azure Active Directory interattiva, il supporto per l'autenticazione Azure Active Directory entità servizio e il supporto per useBulkCopyForBatchInsert per i server di analisi sinapsi non di Azure. |

  Il driver JDBC 9,2 è disponibile anche nel repository centrale Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel POM.XML:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 8.4 per SQL Server:**  

  JDBC Driver 8.4 include tre librerie di classe JAR in ogni pacchetto di installazione: **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** e **mssql-jdbc-8.4.1.jre14.jar**.

  JDBC Driver 8.4 è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su OpenJDK 1.8, OpenJDK 11.0, OpenJDK 14.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 e Azul Zulu JRE 14.0.
  
  Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 8.4 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-8.4.1.jre8.jar|4.2|8|Richiede Java Runtime Environment (JRE) versione 1.8. L'uso di JRE 1.7 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 8.4 includono: supporto di JDK 14, supporto dell'autenticazione in Azure Key Vault tramite identità gestita, supporto esteso della copia bulk per Azure Data Warehouse, memorizzazione nella cache DNS di Azure SQL, supporto della compatibilità con le versioni precedenti per lo streaming di oggetti LOB e autenticazione del certificato client per gli scenari di loopback. |
|mssql-jdbc-8.4.1.jre11.jar|4.3|11|Richiede Java Runtime Environment (JRE) 11.0. L'utilizzo di JRE 10.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 8.4 includono: supporto di JDK 14, supporto dell'autenticazione in Azure Key Vault tramite identità gestita, supporto esteso della copia bulk per Azure Data Warehouse, memorizzazione nella cache DNS di Azure SQL, supporto della compatibilità con le versioni precedenti per lo streaming di oggetti LOB e autenticazione del certificato client per gli scenari di loopback. |
|mssql-jdbc-8.4.1.jre13.jar|4.3|14|Richiede Java Runtime Environment (JRE) versione 14.0. L'uso di JRE 13.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 8.4 includono: supporto di JDK 14, supporto dell'autenticazione in Azure Key Vault tramite identità gestita, supporto esteso della copia bulk per Azure Data Warehouse, memorizzazione nella cache DNS di Azure SQL, supporto della compatibilità con le versioni precedenti per lo streaming di oggetti LOB e autenticazione del certificato client per gli scenari di loopback. |

  Il driver JDBC 8.4 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>8.4.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 8.2 per SQL Server:**  

  JDBC Driver 8.2 include tre librerie di classe JAR in ogni pacchetto di installazione: **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** e **mssql-jdbc-8.2.2.jre13.jar**.

  JDBC Driver 8.2 è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su OpenJDK 1.8, OpenJDK 11.0, OpenJDK 13.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 e Azul Zulu JRE 13.0.
  
  Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 8.2 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-8.2.2.jre8.jar|4.2|8|Richiede Java Runtime Environment (JRE) versione 1.8. L'uso di JRE 1.7 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 8,2 includono il supporto per JDK 13, Always Encrypted con enclavi sicure e miglioramenti delle prestazioni dei dati temporali. |
|mssql-jdbc-8.2.2.jre11.jar|4.3|11|Richiede Java Runtime Environment (JRE) 11.0. L'utilizzo di JRE 10.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 8,2 includono il supporto per JDK 13, Always Encrypted con enclavi sicure e miglioramenti delle prestazioni dei dati temporali. |
|mssql-jdbc-8.2.2.jre13.jar|4.3|13|Richiede Java Runtime Environment (JRE) versione 13.0. L'uso di JRE 11.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 8,2 includono il supporto per JDK 13, Always Encrypted con enclavi sicure e miglioramenti delle prestazioni dei dati temporali. |

  Il driver JDBC 8.2 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>8.2.2.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.4 per SQL Server:**  

  JDBC Driver 7.4 include tre librerie di classe JAR in ogni pacchetto di installazione: **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** e **mssql-jdbc-7.4.1.jre12.jar**.

  JDBC Driver 7.4 è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su OpenJDK 1.8, OpenJDK 11.0, OpenJDK 12.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 e Azul Zulu JRE 12.0.
  
  Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 7.4 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.4.1.jre8.jar|4.2|8|Richiede Java Runtime Environment (JRE) versione 1.8. L'uso di JRE 1.7 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.4 includono: supporto per JDK 12, autenticazione NTLM e useFmtOnly. |  
|mssql-jdbc-7.4.1.jre11.jar|4.3|11|Richiede Java Runtime Environment (JRE) 11.0. L'utilizzo di JRE 10.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.4 includono: supporto per JDK 12, autenticazione NTLM e useFmtOnly. |  
|mssql-jdbc-7.4.1.jre12.jar|4.3|12|Richiede Java Runtime Environment (JRE) versione 12.0. L'uso di JRE 11.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.4 includono: supporto per JDK 12, autenticazione NTLM e useFmtOnly. |  

  Il driver JDBC 7.4 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.4.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.2 per SQL Server:**  

  Il driver JDBC 7.2 include due librerie di classi JAR in ogni pacchetto di installazione: **mssql-jdbc-7.2.2.jre8.jar** e **mssql-jdbc-7.2.2.jre11.jar**.

  Il driver JDBC 7,2 è progettato per funzionare con e essere supportato da tutte le principali macchine virtuali Java, ma viene testato solo in OpenJDK 8,0, OpenJDK 11,0, Azul Zulu JRE 8,0 e Azul Zulu JRE 11,0.
  
  Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 7.2 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.2.2.jre8.jar|4.2|8|Richiede un Java Runtime Environment (JRE) versione 8.0. L'utilizzo di JRE 7.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.2 includono: supporto per JDK 11, autenticazione dell'identità gestita (MSI) di Active Directory, supporto OSGi, API SQLServerError. |  
|mssql-jdbc-7.2.2.jre11.jar|4.3|10|Richiede Java Runtime Environment (JRE) 11.0. L'utilizzo di JRE 10.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.2 includono: supporto per JDK 11, autenticazione dell'identità gestita (MSI) di Active Directory, supporto OSGi, API SQLServerError. |  

  Il driver JDBC 7.2 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.2.2.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.0 per SQL Server**  

  JDBC Driver 7.0 include due librerie di classe JAR in ogni pacchetto di installazione: **mssql-jdbc-7.0.0.jre8.jar** e **mssql-jdbc-7.0.0.jre10.jar**.

  JDBC Driver 7.0 è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su OpenJDK 8.0 e 10.0.
  
  Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 7.0 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.0.0.jre8.jar|4.2|8|Richiede un Java Runtime Environment (JRE) versione 8.0. L'utilizzo di JRE 7.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.0 includono: supporto JDK 10, livello di conformità predefinito aggiornato in base alle specifiche JDBC 4.2, supporto dei tipi di dati spaziali, proprietà di connessione cancelQueryTimeout, metodi di limitazione delle richieste, proprietà di connessione useBulkCopyForBatchInsert, informazioni di individuazione e classificazione dei dati, estensione della funzionalità UTF-8 e supporto CityHash. |  
|mssql-jdbc-7.0.0.jre10.jar|4.3|10|Richiede Java Runtime Environment (JRE) 10.0. L'utilizzo di JRE 9.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità della versione 7.0 includono: supporto JDK 10, livello di conformità predefinito aggiornato in base alle specifiche JDBC 4.2, supporto dei tipi di dati spaziali, proprietà di connessione cancelQueryTimeout, metodi di limitazione delle richieste, proprietà di connessione useBulkCopyForBatchInsert, informazioni di individuazione e classificazione dei dati, estensione della funzionalità UTF-8 e supporto CityHash. |  

  Il driver JDBC 7.0 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.0.0.jre10</version>
</dependency>
```
  
**Microsoft JDBC Driver 6.4 per SQL Server**  

  JDBC Driver 6.4 include tre librerie di classe JAR in ogni pacchetto di installazione: **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** e **mssql-jdbc-6.4.0.jre9.jar**.

  JDBC Driver 6.4 è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su OpenJDK 7.0, 8.0 e 9.0.
  
  Il diagramma seguente riepiloga il supporto offerto dai tre file con estensione jar inclusi in Microsoft JDBC Driver 6.4 per SQL Server:  
  
  |JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-6.4.0.jre7.jar|4.1|7|Richiede un Java Runtime Environment (JRE) versione 7.0. L'utilizzo di JRE 6.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 6,4 includono: Azure AD autenticazione per Linux, metodo principale/password per Kerberos, rilevamento automatico dell'area di autenticazione in SPN per l'autenticazione tra domini, delega vincolata Kerberos, timeout query, timeout socket e riutilizzo dell'handle di istruzione preparata. |  
|mssql-jdbc-6.4.0.jre8.jar|4.2|8|Richiede un Java Runtime Environment (JRE) versione 8.0. L'utilizzo di JRE 7.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 6,4 includono: Azure AD autenticazione per Linux, metodo principale/password per Kerberos, rilevamento automatico dell'area di autenticazione in SPN per l'autenticazione tra domini, delega vincolata Kerberos, timeout query, timeout socket e riutilizzo dell'handle di istruzione preparata. |  
|mssql-jdbc-6.4.0.jre9.jar|4.3|9|Richiede Java Runtime Environment (JRE) 9.0. L'utilizzo di JRE 8.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 6,4 includono: Azure AD autenticazione per Linux, metodo principale/password per Kerberos, rilevamento automatico dell'area di autenticazione in SPN per l'autenticazione tra domini, delega vincolata Kerberos, timeout query, timeout socket e riutilizzo dell'handle di istruzione preparata. |

Il driver JDBC 6.4 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML

 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.4.0.jre9</version>
</dependency>
```

**Microsoft JDBC Driver 6.2 per SQL Server:**  
  
  JDBC Driver 6.2 include due librerie di classe JAR in ogni pacchetto di installazione: **mssql-jdbc-6.2.2.jre7.jar** e **mssql-jdbc-6.2.2.jre8.jar**.
  
 JDBC Driver 6.2 è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su Sun JRE 5.0, 6.0, 7.0 e 8.0.
  
 Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 6.0 e 4.2 per SQL Server:  
  
|JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|
|mssql-jdbc-6.2.2.jre7.jar|4.1|7|Richiede un Java Runtime Environment (JRE) versione 7.0. L'utilizzo di JRE 6.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 6,2 includono: Azure AD autenticazione per Linux, metodo principale/password per Kerberos, rilevamento automatico dell'area di autenticazione in SPN per l'autenticazione tra domini, delega vincolata Kerberos, timeout query, timeout socket e riutilizzo dell'handle di istruzione preparata. |  
|mssql-jdbc-6.2.3.jre8.jar|4.2|8|Richiede un Java Runtime Environment (JRE) versione 8.0. L'utilizzo di JRE 7.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità di 6,2 includono l'autenticazione Azure AD per Linux, il metodo principale/password per Kerberos, il rilevamento automatico dell'area di autenticazione in SPN per l'autenticazione tra domini, la delega vincolata Kerberos, il timeout delle query, il timeout del socket e il riutilizzo dell'handle di istruzione preparato|  

  Il driver JDBC 6.2 è disponibile anche nell'archivio centrale di Maven e può essere aggiunto a un progetto Maven aggiungendo il codice seguente nel modello POM.XML
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.2.jre8</version>
</dependency>
```

 **Microsoft JDBC Driver 6.0 e 4.2 per SQL Server:**  
  
  I driver JDBC 6.0 e 4.2 includono due librerie di classi JAR in ogni pacchetto di installazione: **sqljdbc41.jar** e **sqljdbc42.jar**.
  
 JDBC Driver 6.0 e 4.2 sono progettati per funzionare con ed essere supportati da tutte le principali macchine virtuali Java, ma sono testati solo su Sun JRE 5.0, 6.0, 7.0 e 8.0.
  
 Il diagramma seguente riepiloga il supporto offerto dai due file con estensione jar inclusi in Microsoft JDBC Driver 6.0 e 4.2 per SQL Server:  
  
|JAR|Conformità versione JDBC|Versione Java consigliata|Descrizione|  
|---------|-----------------------------|----------------------|-----------------|  
|sqljdbc41.jar|4.1|7|Richiede un Java Runtime Environment (JRE) versione 7.0. L'utilizzo di JRE 6.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità nei pacchetti 6.0 e 4.2 includono: conformità a JDBC 4.1 e copia bulk<br /><br /> Le nuove funzionalità del solo pacchetto 6.0 includono inoltre: Always Encrypted, parametri con valori di tabella, autenticazione di Azure Active Directory, connessioni trasparenti ai gruppi di disponibilità AlwaysOn, miglioramento nel recupero dei metadati dei parametri per le query preparate e IDN (Internationalized Domain Name)|  
|sqljdbc42.jar|4.2|8|Richiede un Java Runtime Environment (JRE) versione 8.0. L'utilizzo di JRE 7.0 o versioni precedenti genera un'eccezione.<br /><br /> Le nuove funzionalità nei pacchetti 6.0 e 4.2 includono: conformità a JDBC 4.1, conformità a JDBC 4.2 e copia bulk<br /><br /> Le nuove funzionalità del solo pacchetto 6.0 includono inoltre: Always Encrypted, parametri con valori di tabella, autenticazione di Azure Active Directory, connessioni trasparenti ai gruppi di disponibilità AlwaysOn, miglioramento nel recupero dei metadati dei parametri per le query preparate e IDN (Internationalized Domain Name)|  
  
 **Microsoft JDBC Driver 4.1 per SQL Server:**  
  
 Il driver JDBC 4.1 include una libreria di classi JAR in ogni pacchetto di installazione: **sqljdbc41.jar**.  

|JAR|Descrizione|  
|---------|-----------------|  
|sqljdbc41.jar|La libreria di classi **sqljdbc41.jar** fornisce il supporto per l'API JDBC 4.0 Sono incluse tutte le funzionalità del driver JDBC 4,0 e dei metodi dell'API JDBC 4,0. JDBC 4.1 non è supportato (viene generata un'eccezione "SQLFeatureNotSupportedException").<br /><br /> La libreria di classi **sqljdbc41.jar** richiede Java Runtime Environment (JRE) 7.0. L'utilizzo di **sqljdbc41.jar** in JRE 6.0 e 5.0 genera un'eccezione.<br /><br />
  
 JDBC Driver è progettato per funzionare con ed essere supportato da tutte le principali macchine virtuali Java, ma è testato solo su Sun JRE 5.0, 6.0 e 7.0.
  
 Il diagramma seguente riepiloga il supporto offerto dal file con estensione jar incluso con Microsoft JDBC Driver 4.1 per SQL Server.  
  
|JAR|Versione JDBC|JRE (esecuzione)|JDK (compilazione)|  
|---------|------------------|---------------------|-------------------------|  
|sqljdbc41.jar|4|7|7 6 5|  
  
## <a name="sql-server-requirements"></a>Requisiti di SQL Server  

 JDBC Driver supporta le connessioni al database SQL di Azure e a SQL Server. Per Microsoft JDBC Driver 4.2 e 4.1 per SQL Server, il supporto inizia con SQL Server 2008.
  
## <a name="operating-system-requirements"></a>Requisiti del sistema operativo  

 Il driver JDBC è stato sviluppato per essere utilizzato su qualsiasi sistema operativo che supporti l'utilizzo di Java Virtual Machine (JVM). Tuttavia, sono stati ufficialmente testati solo i sistemi operativi Sun Solaris, SUSE Linux, Ubuntu Linux, CentOS Linux, macOS e Windows.  
  
## <a name="supported-languages"></a>Lingue supportate  

 JDBC Driver supporta tutte le regole di confronto delle colonne di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni sulle regole di confronto supportate dal driver JDBC, vedere [Caratteristiche internazionali del driver JDBC](../../connect/jdbc/international-features-of-the-jdbc-driver.md).  
  
 Per altre informazioni sulle regole di confronto, vedere "Uso delle regole di confronto" nella documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  

 [Panoramica del driver JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
