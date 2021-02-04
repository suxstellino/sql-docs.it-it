---
title: Domande frequenti sul driver JDBC
description: Questa pagina offre le risposte alle domande frequenti su Microsoft JDBC Driver per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cbc0e397-ecf2-4494-87b2-a492609bceae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 634bbd66e2ad00ccda3b60f6f2fccfea976d09b0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163620"
---
# <a name="frequently-asked-questions-faq-for-jdbc-driver"></a>Domande frequenti sul driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Questa pagina offre le risposte alle domande frequenti su Microsoft JDBC Driver per SQL Server.

## <a name="frequently-asked-questions"></a>Domande frequenti

**Come è possibile contribuire al miglioramento del driver JDBC?**  
Il driver JDBC è open source e il codice sorgente è disponibile in [GitHub](https://github.com/microsoft/mssql-jdbc). È possibile contribuire al miglioramento del driver registrando i problemi e contribuendo alla base di codice.

**Quali versioni di SQL Server e Java sono supportate dal driver?**  
Vedere la pagina [Matrice di supporto di Microsoft JDBC Driver per SQL Server](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md) per altri dettagli.

**Qual è la differenza tra i pacchetti del driver JDBC disponibili nella [pagina di download Microsoft](download-microsoft-jdbc-driver-for-sql-server.md) e il driver JDBC disponibile in GitHub?**  
I file del driver JDBC disponibili nel repository GitHub per Microsoft JDBC Driver sono la parte fondamentale del driver JDBC e sono coperti dalla licenza open source elencata nel repository. I pacchetti del driver nella [pagina di download Microsoft](download-microsoft-jdbc-driver-for-sql-server.md) includono librerie aggiuntive per l'autenticazione integrata di Windows e per l'abilitazione delle transazioni XA con il driver JDBC. Queste librerie aggiuntive sono coperte dalla licenza inclusa nel pacchetto scaricabile.

**Quali informazioni sono necessarie prima di aggiornare il driver?**  
Microsoft JDBC driver 9,2 supporta le specifiche JDBC 4,2 e 4,3 (parzialmente) e include tre librerie di classi JAR nel pacchetto di installazione, come indicato di seguito:

| JAR                        | Specifica JDBC            | Versione JDK |
| -------------------------- | ----------------------------- | ----------- |
| MSSQL-JDBC-9.2.0. JRE15. jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 15,0    |
| MSSQL-JDBC-9.2.0. jre11. jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 11.0    |
| MSSQL-JDBC-9.2.0. jre8. jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 8.4 supporta le specifiche JDBC 4.2 e 4.3 (parzialmente) e include tre librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                        | Specifica JDBC            | Versione JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.4.1.jre14.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 14.0    |
| mssql-jdbc-8.4.1.jre11.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 11.0    |
| mssql-jdbc-8.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 8.2 supporta le specifiche JDBC 4.2 e 4.3 (parzialmente) e include tre librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                        | Specifica JDBC            | Versione JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.2.2.jre13.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 13.0    |
| mssql-jdbc-8.2.2.jre11.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 11.0    |
| mssql-jdbc-8.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.4 supporta le specifiche JDBC 4.2 e 4.3 (parzialmente) e include tre librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                        | Specifica JDBC            | Versione JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.4.1.jre12.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 12.0    |
| mssql-jdbc-7.4.1.jre11.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 11.0    |
| mssql-jdbc-7.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.2 supporta le specifiche JDBC 4.2 e 4.3 (parzialmente) e include due librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                        | Specifica JDBC            | Versione JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.2.2.jre11.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 11.0    |
| mssql-jdbc-7.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.0 supporta le specifiche JDBC 4.2 e 4.3 (parzialmente) e include due librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                        | Specifica JDBC            | Versione JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.0.0.jre10.jar | JDBC 4.3 (parzialmente) e 4.2 | JDK 10.0    |
| mssql-jdbc-7.0.0.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 6.4 supporta le specifiche JDBC 4.1, 4.2 e 4.3 (parzialmente) e include tre librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                       | Specifica JDBC                 | Versione JDK |
| ------------------------- | ---------------------------------- | ----------- |
| mssql-jdbc-6.4.0.jre9.jar | JDBC 4.3 (parzialmente), 4.2 e 4.1 | JDK 9.0     |
| mssql-jdbc-6.4.0.jre8.jar | JDBC 4.2 e 4.1                  | JDK 8.0     |
| mssql-jdbc-6.4.0.jre7.jar | JDBC 4.1                           | JDK 7.0     |
| &nbsp;                    | &nbsp;                             | &nbsp;      |

Microsoft JDBC Driver 6.2 supporta le specifiche JDBC 4.0, 4.1 e 4.2 e include due librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR                       | Specifica JDBC     | Versione JDK |
| ------------------------- | ---------------------- | ----------- |
| mssql-jdbc-6.2.2.jre8.jar | JDBC 4.2, 4.1 e 4.0 | JDK 8.0     |
| mssql-jdbc-6.2.2.jre7.jar | JDBC 4.1 e 4.0       | JDK 7.0     |
| &nbsp;                    | &nbsp;                 | &nbsp;      |

Microsoft JDBC Driver 6.0 e 4.2 per SQL Server supportano le specifiche JDBC 4.0, 4.1 e 4.2 e includono due librerie di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR           | Specifica JDBC     | Versione JDK |
| ------------- | ---------------------- | ----------- |
| sqljdbc42.jar | JDBC 4.2, 4.1 e 4.0 | JDK 8.0     |
| sqljdbc41.jar | JDBC 4.1 e 4.0       | JDK 7.0     |
| &nbsp;        | &nbsp;                 | &nbsp;      |

Microsoft JDBC Driver 4.1 per SQL Server supporta la specifica JDBC 4.0 e include una libreria di classi JAR nel pacchetto di installazione, come descritto di seguito:

| JAR           | Specifica JDBC | Versione JDK     |
| ------------- | ------------------ | --------------- |
| sqljdbc41.jar | JDBC 4.0           | JDK 7.0 e 6.0 |
| &nbsp;        | &nbsp;             | &nbsp;      |

**È necessario modificare in qualche modo il codice dell'applicazione per usare il driver più recente con la versione di SQL Server esistente?**  
In generale, il driver è progettato per essere compatibile con le versioni precedenti in modo che non sia necessario modificare le applicazioni esistenti quando si aggiorna il driver. Nel caso in cui una nuova versione del driver introduca una modifica significativa, la sezione [Note sulla versione per il driver JDBC](../../connect/jdbc/release-notes-for-the-jdbc-driver.md) specifica tutti i dettagli necessari su tale modifica e gli effetti sulle applicazioni esistenti. Inoltre, è possibile esaminare le note sulla versione incluse con il driver per un elenco dei bug corretti in tale versione e per i problemi noti.

**Quanto costa il driver?**  
Microsoft JDBC Driver per SQL Server è disponibile gratuitamente.

**È possibile ridistribuire il driver?**  
I driver JDBC 6.0, 6.2, 6.4 e 7.0 sono ridistribuibili. Esaminare la clausola "Codice distribuibile" nei contratti di licenza.

**È possibile usare il driver per accedere a Microsoft SQL Server da un computer Linux?**  
Sì. È possibile usare il driver per accedere a SQL Server da Unix, Linux e altre piattaforme non Windows. Per altre informazioni, vedere [Matrice di supporto di Microsoft JDBC Driver per SQL Server](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md).

**Il driver supporta la crittografia TLS?**  
A partire dalla versione 1.2, il driver supporta la crittografia Transport Layer Security (TLS), precedentemente nota come crittografia Secure Sockets Layer (SSL). Per altre informazioni, vedere [Uso della crittografia](../../connect/jdbc/using-ssl-encryption.md).

**Quali tipi di autenticazione supporta Microsoft JDBC Driver per SQL Server?**  
La tabella seguente elenca le opzioni di autenticazione disponibili. L'autenticazione Kerberos Java pura è disponibile a partire dalla versione 4.0 del driver.

| Piattaforma    | Authentication                        |
| ----------- | ------------------------------------- |
| Non Windows | Kerberos Java pura                    |
| Non Windows | SQL Server                            |
| Non Windows | Autenticazione di Azure Active Directory |
| Non Windows | NTLM                                  |
| Windows     | Kerberos Java pura                    |
| Windows     | SQL Server                            |
| Windows     | Kerberos con backup NTLM             |
| Windows     | NTLM                                  |
| Windows     | Autenticazione di Azure Active Directory |
| &nbsp;      | &nbsp;                                |

**Il driver supporta gli indirizzi del protocollo IPv6?**  
Sì. Il driver supporta l'uso degli indirizzi IPv6. Usare la raccolta di proprietà di connessione e la proprietà della stringa di connessione serverName. Per altre informazioni, vedere [Costruzione dell'URL di connessione](../../connect/jdbc/building-the-connection-url.md).

**Che cos'è il buffering adattivo?**  
Il buffer adattivo è stato introdotto a partire da Microsoft SQL Server 2005 JDBC Driver versione 1.2. È progettato per recuperare qualsiasi tipo di dati con valori di grandi dimensioni senza l'overhead dei cursori server. La funzionalità di buffering adattivo di Microsoft SQL Server JDBC Driver fornisce una proprietà della stringa di connessione responseBuffering che può essere impostata su "adattiva" o "completa". Nella versione 1.2 la modalità di buffering predefinita è "full" e l'applicazione deve impostare la modalità di buffering adattivo in modo esplicito. A partire da JDBC Driver versione 2.0 il comportamento predefinito è "adattivo". Per ottenere il comportamento di buffering adattivo, l'applicazione non deve quindi richiederlo in modo esplicito. Per altre informazioni, vedere [Uso del buffer adattivo](../../connect/jdbc/using-adaptive-buffering.md) e il blog [What is adaptive response buffering and why should I use it?](/archive/blogs/jdbcteam/) (Che cos'è il buffering delle risposte adattivo e perché usarlo).

**Il driver supporta il pool di connessioni?**  
Il driver fornisce il supporto per il pool di connessioni Java Platform, Enterprise Edition 5 (Java EE 5). Il driver implementa le interfacce JDBC 3.0 necessarie per consentire al driver di partecipare a qualsiasi implementazione di pool di connessioni offerta dai fornitori di server applicazioni middleware. Il driver partecipa alle connessioni in pool in questi ambienti. Per altre informazioni, vedere [Uso del pool di connessioni](../../connect/jdbc/using-connection-pooling.md). Il driver non fornisce una propria implementazione di pool, ma si basa su server applicazioni Java di terze parti.

**È disponibile il supporto per il driver?**  
Sono disponibili diverse opzioni di supporto. È possibile pubblicare eventuali domande o problemi nel [repository GitHub](https://github.com/microsoft/mssql-jdbc), che è monitorato da Microsoft. I [forum](https://go.microsoft.com/fwlink/?LinkID=246673) sono monitorati da Microsoft, dagli MVP e dalla community. È anche possibile contattare il Supporto tecnico Microsoft. Il team di sviluppo potrebbe richiedere di riprodurre il problema all'esterno di eventuali server applicazioni di terze parti. Se il problema non può essere riprodotto all'esterno dell'ambiente contenitore Java che lo ospita, sarà necessario coinvolgere le terze parti interessate per consentire al team di continuare a offrire assistenza. Per una risoluzione più efficace del problema, il team potrebbe anche richiedere di riprodurlo in un sistema operativo come Windows.

**Il driver è certificato per l'uso con server applicazioni di terze parti?**  
Il driver è stato testato nei principali server applicazioni, inclusi IBM WebSphere e SAP NetWeaver.

**Come si abilita la traccia?**  
Il driver supporta l'utilizzo della traccia (o registrazione) per semplificare la risoluzione dei problemi relativi all'utilizzo del driver JDBC nell'applicazione. Per abilitare l'utilizzo della traccia JAR sul lato client, il driver JDBC usa le API di registrazione in java.util.logging. Per altre informazioni, vedere [Creazione di tracce](../../connect/jdbc/tracing-driver-operation.md). Per informazioni sulla traccia XA sul lato server, vedere la pagina relativa alla [traccia di accesso ai dati in SQL Server](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)).

**Da dove è possibile scaricare le versioni precedenti del driver, ad esempio SQL Server 2000 JDBC Driver, 2005 Driver oppure le versioni del driver 1.0, 1.1 o 1.2?**  
Queste versioni del driver non sono disponibili per il download perché non sono più supportate. Il supporto per la connettività Java è in continuo miglioramento, pertanto è consigliabile usare sempre la versione più recente del driver Microsoft JDBC.

**Si sta usando JRE 1.4. Quali sono i driver compatibili?**  
I clienti che usano prodotti SAP e richiedono il supporto di JRE 1.4 possono contattare [SAP Service Marketplace](https://service.sap.com/) per ottenere Microsoft JDBC Driver 1.2.

**Il driver può comunicare usando gli algoritmi FIPS convalidati?**  
Microsoft JDBC Driver non contiene algoritmi di crittografia. Se un cliente usa algoritmi del sistema operativo, dell'applicazione e JVM considerati accettabili da FIPS (Federal Information Processing Standard) e configura il driver per l'uso di tali algoritmi, il driver usa solo gli algoritmi designati per la comunicazione.

## <a name="see-also"></a>Vedere anche

[Panoramica del driver JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)