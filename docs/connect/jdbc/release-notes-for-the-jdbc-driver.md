---
title: Note sulla versione per il driver JDBC
description: Questo articolo elenca le versioni di Microsoft JDBC Driver per SQL Server. Per ogni versione sono elencate e descritte le modifiche.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 074f211e-984a-4b76-bb15-ee36f5946f12
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ffc7d8e4090673f3fc8feb96aa3650d35361281
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234857"
---
# <a name="release-notes-for-the-microsoft-jdbc-driver-for-sql-server"></a>Note sulla versione per Microsoft JDBC Driver per SQL Server

Questo articolo elenca le versioni del _driver Microsoft JDBC per SQL Server_. Per ogni versione sono elencate e descritte le modifiche.

## <a name="92"></a><a id="92"></a> 9,2

**[![Download ](../../ssms/media/download-icon.png) scaricare Microsoft JDBC Driver 9,2 for SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2153622)**  
**[![Download ](../../ssms/media/download-icon.png) scaricare Microsoft JDBC Driver 9,2 per SQL Server (tar. gz)](https://go.microsoft.com/fwlink/?linkid=2153521)**  

Numero di versione: 9.2.0 rilasciata: 29 gennaio 2021

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file ZIP: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40a)  

### <a name="compliance"></a>Conformità

| Modifica di conformità | Dettagli |
| :---------------- | :------ |
| Scaricare gli aggiornamenti più recenti per il driver JDBC 9,2. | &bull;&nbsp; [GitHub, 9.2.0](https://github.com/Microsoft/mssql-jdbc/releases/tag/v9.2.0)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Completamente conforme alla specifica API JDBC 4.2. | I file jar nel pacchetto 9,2 vengono denominati in base alla compatibilità delle versioni Java.<br/><br/>Ad esempio, il file MSSQL-JDBC-9.2.0. JRE15. jar del pacchetto 9,2 deve essere usato con Java 15. |
| Compatibile con Java Development Kit (JDK) versione 15,0, 11,0 e 1,8. | Microsoft JDBC driver 9,2 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 15,0, oltre a JDK 11,0 e 1,8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Versioni

Numero di versione: 9.2.0 rilasciata: 29 gennaio 2021

### <a name="support-for-jdk-15"></a>Supporto per JDK 15

Microsoft JDBC driver 9,2 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 15,0, oltre a JDK 11,0 e 1,8.

### <a name="added-support-for-azure-active-directory-interactive-authentication"></a>Aggiunta del supporto per Azure Active Directory autenticazione interattiva
| Aggiunta del tipo di autenticazione | Dettagli |
| :---------- | :------ |
| Microsoft JDBC driver 9,2 per SQL Server supporta ora l'autenticazione per Azure Key Vault tramite l'autenticazione interattiva. | Vedere [connessione mediante l'autenticazione Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-active-directory-service-principal-authentication"></a>Aggiunta del supporto per l'autenticazione Azure Active Directory entità servizio
| Aggiunta del tipo di autenticazione | Dettagli |
| :---------- | :------ |
| Microsoft JDBC driver 9,2 per SQL Server supporta ora l'autenticazione per Azure Key Vault usando l'ID client e il segreto di un'identità dell'entità servizio. | Vedere [connessione mediante l'autenticazione Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="updated-azure-key-vault-library-to-use-modern-azure-key-vault-library"></a>Aggiornamento della libreria Azure Key Vault per l'uso della libreria di Azure Key Vault moderna
| Modifiche della libreria per Azure Key Vault | Dettagli |
| :------------------- | :------ |
| Microsoft JDBC driver 9,2 ha eseguito la migrazione dalla libreria Azure Key Vault di generazione precedente e dalle librerie ADAL ai Azure Key Vault più moderni e agli equivalenti di Azure Identity. | **modifica delle dipendenze** Assicurarsi di aggiornare le dipendenze dell'applicazione se si sfruttano i vantaggi dell'autenticazione Azure Active Directory o Azure Key Vault. Vedere i requisiti di dipendenza quando si [utilizza il provider di Azure Key Vault](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-the-azure-key-vault-provider) o l' [autenticazione Azure Active Directory](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-azure-active-directory-authentication) |
| &nbsp; | &nbsp; |

### <a name="enabled-usebulkcopyforbatchinsert-for-non-azure-synapse-analytics-servers"></a>Abilitazione di useBulkCopyForBatchInsert per server di analisi sinapsi non di Azure
| useBulkCopyForBatchInsert modifiche per i server non Azure sinapsi Analytics | Dettagli |
| :------------------- | :------ |
| Microsoft JDBC driver 9,2 consente ora l'API per la copia bulk per l'operazione di inserimento batch sui server di analisi sinapsi non di Azure. | Vedere [uso dell'API per la copia bulk per l'operazione di inserimento batch](../../connect/jdbc/use-bulk-copy-api-batch-insert-operation.md). |
| &nbsp; | &nbsp; |


## <a name="previous-releases"></a>Versioni precedenti

## <a name="84"></a><a id="84"></a> 8.4

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 8.4 per SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2137600)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 8.4 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2137502)**  

Numero di versione: 8.4.1  
Resa disponibile: 27 agosto 2020

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file ZIP: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40a)  

### <a name="compliance"></a>Conformità

| Modifica di conformità | Dettagli |
| :---------------- | :------ |
| Scaricare gli aggiornamenti più recenti per il driver JDBC 8.4. | &bull; &nbsp; [GitHub, 8.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Completamente conforme alla specifica API JDBC 4.2. | I file JAR nel pacchetto 8.4 sono denominati in base alla compatibilità delle versioni di Java.<br/><br/>Ad esempio, il file mssql-jdbc-8.4.1.jre14.jar dal pacchetto 8.4 deve essere usato con Java 14. |
| Compatibile con Java Development Kit (JDK) versione 14.0, 11.0 e JDK 1.8. | Microsoft JDBC Driver 8.4 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 14.0, oltre a JDK 11.0 e 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Rilasci

Numero di versione: 8.4.1  
Resa disponibile: 27 agosto 2020  
Problemi risolti:  

- Correzione di un problema relativo a `SQLServerConnectionPoolProxy` non compatibile con `delayLoadingLobs`
- Correzione di un potenziale problema di `NullPointerException` con `delayLoadingLobs`
- Correzione di un problema relativo alla decrittografia delle chiavi di crittografia della colonna quando si usa l'archivio certificati di Windows

Numero di versione: 8.4.0  
Resa disponibile: 31 luglio 2020  

### <a name="support-for-jdk-14"></a>Supporto per JDK 14

Microsoft JDBC Driver 8.4 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 14.0, oltre a JDK 11.0 e 1.8.

### <a name="added-support-for-authentication-to-azure-key-vault-using-managed-identity"></a>Aggiunto il supporto per l'autenticazione in Azure Key Vault usando l'identità gestita

| Aggiunta del tipo di autenticazione | Dettagli |
| :---------- | :------ |
| Microsoft JDBC Driver 8.4 per SQL Server ora supporta l'autenticazione per Azure Key Vault usando le identità gestite. | Vedere [Uso di Always Encrypted con il driver JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="extended-support-for-bulk-copy-for-azure-data-warehouse"></a>Supporto "Extended" per la copia bulk per Azure Data Warehouse

| Modifiche della copia bulk per Azure Data Warehouse | Dettagli |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 aggiunge una nuova proprietà di connessione, `sendTemporalDataTypesAsStringForBulkCopy`. Questa proprietà booleana è TRUE per impostazione predefinita. | Vedere [Uso della copia bulk con il driver JDBC](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-sql-dns-caching"></a>Aggiunto il supporto per la memorizzazione nella cache DNS di Azure SQL

| Memorizzazione nella cache DNS | Dettagli |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 per SQL Server supporta ora la memorizzazione nella cache DNS per i server SQL di Azure. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="added-backwards-compatibility-for-streaming-lob-objects"></a>Aggiunta della compatibilità con le versioni precedenti per lo streaming di oggetti LOB

| Streaming LOB | Dettagli |
| :------------------- | :------ |
| In Microsoft JDBC Driver 8.4 per SQL Server è stata aggiunta una nuova proprietà di connessione, `delayLoadingLobs`. | Se si imposta `delayLoadingLobs` su FALSE, tutti gli oggetti LOB recuperati da ResultSet non verranno trasmessi in streaming. Ciò significa che il driver caricherà l'intero oggetto LOB in memoria in una sola volta, in modo analogo al funzionamento del driver prima della versione 6.4. |
| &nbsp; | &nbsp; |

### <a name="added-support-for-client-certificate-authentication-for-loopback-scenarios"></a>Aggiunto il supporto per l'autenticazione del certificato client per scenari di loopback

| Autenticazione del certificato client | Dettagli |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 per SQL Server ha aggiunto un nuovo metodo di autenticazione detto autenticazione del certificato client per gli scenari di loopback. | Vedere [Autenticazione del certificato client per scenari di loopback](../../connect/jdbc/client-certification-authentication-for-loopback-scenarios.md). |

## <a name="82"></a><a id="82"></a> 8.2

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 8.2 per SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2122433)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 8.2 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122536)**  

Numero di versione: 8.2.2 Data di rilascio: 24 marzo 2020

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file ZIP: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40a)  

### <a name="compliance"></a>Conformità

| Modifica di conformità | Dettagli |
| :---------------- | :------ |
| Scaricare gli aggiornamenti più recenti per il driver JDBC 8.2. | &bull; &nbsp; [GitHub, 8.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Completamente conforme alla specifica API JDBC 4.2. | I file JAR nel pacchetto 8.2 sono denominati in base alla compatibilità delle versioni di Java.<br/><br/>Ad esempio, il file mssql-jdbc-8.2.2.jre11.jar dal pacchetto 8.2 deve essere usato con Java 11. |
| Compatibile con Java Development Kit (JDK) versione 13.0, 11.0 e JDK 1.8. | Il driver Microsoft JDBC 8.2 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 13.0, oltre a JDK 11.0 e 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Rilasci

Numero di versione: 8.2.2  
Resa disponibile: 24 marzo 2020  
Problemi risolti:  

- Aggiunta un'opzione che consente di configurare l'elenco degli endpoint Azure Key Vault attendibili

Numero di versione: 8.2.1  
Resa disponibile: 26 febbraio 2020  
Problemi risolti:  

- Correzione di un potenziale problema di `NullPointerException` durante il recupero dei dati come tipo `java.time.LocalTime` o `java.time.LocalDate` con `SQLServerResultSet.getObject()`

Numero di versione: 8.2.0  
Resa disponibile: 31 gennaio 2020  

### <a name="support-for-jdk-13"></a>Supporto per JDK 13

Il driver Microsoft JDBC 8.2 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 13.0, oltre a JDK 11.0 e 1.8.

### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted con enclave sicuri

| Modifica per Always Encrypted | Dettagli |
| :--------- | :------ |
| Microsoft JDBC Driver 8.2 per SQL Server ora supporta Always Encrypted con enclave sicure. Qui sono disponibili informazioni dettagliate: Always Encrypted con enclave sicuri. |
| Ulteriori dettagli e codice di esempio. | Vedere [Always Encrypted con enclave sicuri](../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="performance-improvement-when-retrieving-temporal-datatypes-from-sql-server-sup1sup"></a>Miglioramento delle prestazioni durante il recupero di tipi di dati temporali da SQL Server <sup>1</sup>

| Modifica per i tipi di dati temporali | Dettagli |
| :---------- | :------ |
| Microsoft JDBC Driver 8.2 per SQL Server ha migliorato le prestazioni durante il recupero dei tipi di dati temporali da SQL Server. | Questa modifica elimina le conversioni dei tipo di dati temporali superflue eliminando l'uso di java.util.Calendar laddove possibile. |
| Di seguito è riportato un elenco dei tipi di dati temporali interessati da questo miglioramento delle prestazioni, con il tipo di dati in formato SQL Server seguito dal tipo Java corrispondente. | date (java.sql.Date), datetime (java.sql.Timestamp), datetime2 (java.sql.Timestamp), smalldatetime (java.sql.Timestamp) e time (java.sql.Time). |
| &nbsp; | &nbsp; |

<sup>1</sup> A causa delle differenze nel modo in cui vengono gestiti i fusi orari tra l'API java.util.Calendar e java.time.LocalDateTime, i tipi di dati temporali con associato un oggetto java.util.Calendar fornito dall'utente o i tipi di dati microsoft.sql.DateTimeOffset non traggono vantaggio da questo miglioramento.

### <a name="deployment-of-mssql-jdbc_auth-version-archdll-previously-sqljdbc_authdll-to-maven-repository"></a>Distribuzione di mssql-jdbc_auth-\<version>-\<arch>.dll (in precedenza sqljdbc_auth.dll) nel repository Maven

| Modifica di sqljdbc_auth.dll | Dettagli |
| :------------------- | :------ |
| A partire da Microsoft JDBC Driver 8.2 per SQL Server, il driver si basa su mssql-jdbc_auth-\<version>-\<arch>.dll anziché su sqljdbc_auth.dll per usare la funzionalità di autenticazione di Azure Active Directory. | &nbsp; |
| La DLL è stata inoltre caricata nel repository Maven per semplificare l'accesso. | Vedere [questa pagina](https://search.maven.org/artifact/com.microsoft.sqlserver/mssql-jdbc_auth). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

| Problemi noti | Dettagli |
| :----------- | :------ |
| Quando si usa Always Encrypted con enclave sicure con Java 8. | Gli utenti devono includere il provider BouncyCastle come dipendenza o eseguire il mapping o il caricamento di un provider di sicurezza che supporta l'algoritmo di firma RSASSA-PSS. |
| &nbsp; | &nbsp; |

## <a name="a-id74-741"></a><a id="74"> 7.4.1

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 7.4.1 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122712)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 7.4.1 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122613)**  

Numero di versione: 7.4.1  
Resa disponibile: 2 agosto 2019

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40a)  

### <a name="compliance"></a>Conformità

| Modifica di conformità | Dettagli |
| :---------------- | :------ |
| Scaricare gli aggiornamenti più recenti per il driver JDBC 7.4. | &bull; &nbsp; [GitHub, 7.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Completamente conforme alla specifica API JDBC 4.2. | I file JAR nel pacchetto 7.4 sono denominati in base alla compatibilità delle versioni di Java.<br/><br/>Ad esempio, il file mssql-jdbc-7.4.1.jre11.jar dal pacchetto 7.4 deve essere usato con Java 11. |
| Compatibile con Java Development Kit (JDK) versione 12.0, 11.0 e JDK 1.8. | Il driver Microsoft JDBC 7.4 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 12.0, oltre a JDK 11.0 e 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Rilasci

Numero di versione: 7.4.1  
Resa disponibile: 2 agosto 2019  
Problemi risolti:  

- Ripristinate le nuove implementazioni API `hashCode()` e `equals()` da `SQLServerDataTable` e `SQLServerDataColumn` perché la modifica dell'API ha interrotto la compatibilità con le versioni precedenti

Numero di versione: 7.4.0  
Resa disponibile: 31 luglio 2019  

### <a name="support-for-jdk-12"></a>Supporto per JDK 12

Il driver Microsoft JDBC 7.4 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 12.0, oltre a JDK 11.0 e 1.8.

### <a name="introduces-ntlm-authentication"></a>Introduzione dell'autenticazione NTLM

| Modifica di NTLM | Dettagli |
| :--------- | :------ |
| Supporto della modalità di autenticazione NTLM. | Questa modalità di autenticazione consente ai client Windows e non Windows di autenticarsi in SQL Server usando gli utenti del dominio Windows. |
| Altri dettagli e un'applicazione di esempio per usare questa modalità di autenticazione. | Vedere [Connessione mediante l'autenticazione NTLM](using-ntlm-authentication-to-connect-to-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-querying-parametermetadata-via-_usefmtonly_"></a>Introduzione dell'esecuzione di query su ParameterMetaData tramite _useFmtOnly_

| Modifica di useFmtOnly | Dettagli |
| :---------- | :------ |
| Aggiunta della proprietà di connessione **useFmtOnly**. | Questa funzionalità consente agli utenti di eseguire una query facoltativa su ParameterMetaData tramite l'API legacy `SET FMTONLY ON`. Questa operazione è utile per gli scenari in cui `sp_describe_undeclared_parameters` non offre le prestazioni previste. |
| Altri dettagli e limitazioni. | Vedere [Uso di useFMTOnly](using-usefmtonly.md) |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-121"></a>Aggiornamento di _Microsoft Azure Key Vault SDK for Java_, versione 1.2.1

| Modifica di Key Vault SDK | Dettagli |
| :------------------- | :------ |
| Aggiornamento della dipendenza di Maven da _Microsoft Azure Key Vault SDK for Java_ alla versione 1.2.1. | &nbsp; |
| Rimozione di _Microsoft Azure SDK per Key Vault WebKey_ come dipendenza Maven. | &nbsp; |
| Dettagli aggiuntivi. | Vedere [Dipendenze delle funzionalità di Microsoft JDBC Driver per SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

| Problemi noti | Dettagli |
| :----------- | :------ |
| Quando si usa l'autenticazione NTLM. | Non è attualmente supportato abilitare la protezione estesa e al tempo stesso le connessioni crittografate. |
| Quando si usa useFmtOnly. | Esistono alcuni problemi con la funzionalità causati da malfunzionamenti nella logica di analisi di SQL. Vedere [Uso di useFmtOnly](using-usefmtonly.md) per altri dettagli e suggerimenti per la soluzione. |
| &nbsp; | &nbsp; |

## <a name="a-id72-722"></a><a id="72"> 7.2.2

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 7.2.2 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122435)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 7.2.2 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122434)**  

Numero di versione: 7.2.2  
Resa disponibile: 16 aprile 2019

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40a)  

### <a name="compliance"></a>Conformità

| Modifica di conformità | Dettagli |
| :---------------- | :------ |
| Scaricare gli aggiornamenti più recenti per il driver JDBC 7.2. | &bull; &nbsp; [GitHub, 7.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Completamente conforme alla specifica API JDBC 4.2. | I file JAR nel pacchetto 7.2 sono denominati in base alla compatibilità delle versioni di Java.<br/><br/>Ad esempio, il file mssql-jdbc-7.2.2.jre11.jar dal pacchetto 7.2 deve essere usato con Java 11. |
| Compatibile con Java Development Kit (JDK) versione 11.0 oltre a JDK 1.8. | Il driver Microsoft JDBC 7.2 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 11.0, oltre a JDK 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Rilasci

Numero di versione: 7.2.2  
Resa disponibile: 16 aprile 2019  
Problemi risolti:  

- Risolti problemi relativi alla pulizia non corretta degli ActivityID

Numero di versione: 7.2.1  
Resa disponibile: 11 febbraio 2019  
Problemi risolti:  

- Risolti problemi di analisi relativi ad alcune query con parametri

Numero di versione: 7.2.0  
Resa disponibile: 31 gennaio 2019  

### <a name="active-directory-_managed-identity_-msi-authentication"></a>Autenticazione tramite _identità gestite_ di Azure Active Directory

| Modifica dell'identità del servizio gestita | Dettagli |
| :--------- | :------ |
| Supporta la modalità di autenticazione tramite identità gestite di Active Directory. | Questa modalità di autenticazione è applicabile alle risorse di Azure con il supporto per la funzionalità "Identità" abilitato.<br/><br/>Il driver supporta entrambi i tipi di identità gestita per l'acquisizione di **accessToken** per stabilire una connessione sicura. |
| Altri dettagli e un'applicazione di esempio per usare questa modalità di autenticazione. | Vedere [Connessione con l'autenticazione di Azure Active Directory](connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-_open-service-gateway-initiative_-osgi-support"></a>Introduce il supporto di _Open Service Gateway Initiative_ (OSGi)

| Modifica per OSGi | Dettagli |
| :---------- | :------ |
| Aggiunta l'implementazione di **DataSourceFactory**. | &bull; &nbsp; `org.osgi.service.jdbc.DataSourceFactory`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.SQLServerDataSourceFactory` |
| Aggiunta l'implementazione di **Activator**. | &bull; &nbsp; `org.osgi.framework.BundleActivator`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.Activator` |
| &nbsp; | &nbsp; |

### <a name="introduces-_sqlservererror_-apis"></a>Introduce le API _SQLServerError_

| Modifica per le API di errore | Dettagli |
| :--------------- | :------ |
| Introduzione dell'API SQLServerError. | API getter per recuperare dettagli aggiuntivi sull'errore generato dal server.<br/><br/>&bull; &nbsp; `SQLServerException.getSQLServerError()`<br/>&bull; &nbsp; `SQLServerError` |
| Dettagli aggiuntivi. | Vedere [Gestione degli errori](handling-errors.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-active-directory-authentication-library-adal4j-for-java_-version-163"></a>Aggiornamento di _Microsoft Azure Active Directory Authentication Library (ADAL4J) per Java_, versione 1.6.3

| Modifica per ADAL4J | Dettagli |
| :------------ | :------ |
| Aggiornamento della dipendenza di Maven da ADAL4J alla versione 1.6.3. | &nbsp; |
| Introduce _Java Client Runtime for AutoRest_ come dipendenza Maven, versione 1.6.5. | &nbsp; |
| Dettagli aggiuntivi. | Vedere [Dipendenze delle funzionalità di Microsoft JDBC Driver per SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-120"></a>Aggiornamento di _Microsoft Azure Key Vault SDK for Java_, versione 1.2.0

| Modifica di Key Vault SDK | Dettagli |
| :------------------- | :------ |
| Aggiornamento della dipendenza di Maven da _Microsoft Azure Key Vault SDK for Java_ alla versione 1.2.0. | &nbsp; |
| Introduce _Microsoft Azure SDK per Key Vault WebKey_ come dipendenza Maven, versione 1.2.0. | &nbsp; |
| Dettagli aggiuntivi. | Vedere [Dipendenze delle funzionalità di Microsoft JDBC Driver per SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

| Problemi noti | Dettagli |
| :----------- | :------ |
| Query con parametri, in alcuni casi. | È stato rilasciato un aggiornamento della versione 7.2.0 (7.2.1) a febbraio 2019 per risolvere questo problema. |
| Pulizia di ActivityId. | È stato rilasciato un aggiornamento della versione 7.2.1 (7.2.2) ad aprile 2019 per risolvere questo problema. |
| &nbsp; | &nbsp; |

## <a name="70"></a>7.0

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 7.0 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122713)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 7.0 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122614)**  

Numero di versione: 7.0.0  
Resa disponibile: 31 luglio 2018

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40a)  

Il driver Microsoft JDBC 7.0 per SQL Server è completamente conforme alla specifica API JDBC 4.2. I file JAR nel pacchetto 7.0 sono denominati in base alla compatibilità delle versioni di Java. Ad esempio, il file mssql-jdbc-7.0.0.jre10.jar dal pacchetto 7.0 deve essere usato con Java 10.

### <a name="support-for-jdk-10"></a>Supporto per JDK 10

Il driver Microsoft JDBC 7.0 per SQL Server è ora compatibile con Java Development Kit (JDK) versione 10.0, oltre a JDK 1.8. Questo aggiornamento espone anche `Automatic-Module-Name` del driver come `com.microsoft.sqlserver.jdbc` attraverso file manifesto corrispondente.

### <a name="support-for-spatial-datatypes"></a>Supporto per tipi di dati spaziali

Il driver Microsoft JDBC 7.0 per SQL Server offre ora il supporto dei tipi di dati spaziali Geometry e Geography di SQL Server. Per altre informazioni sulle API dei tipi di dati spaziali e su come usarle, vedere [Uso dei tipi di dati spaziali](use-spatial-datatypes.md).

### <a name="implementation-for-jdbc-43-introduced-javasqlconnection-apis-beginrequest-and-endrequest"></a>Implementazione per JDBC 4.3 che ha introdotto le API java.sql.Connection beginRequest() ed endRequest()

Il driver Microsoft JDBC 7.0 per SQL Server implementa ora le API `beginRequest()` e `endRequest()` dalla classe `java.sql.Connection`. Queste API sono state introdotte con la specifica JDBC 4.3 e JDK 9. Per altre informazioni sull'implementazione del driver di queste API, vedere [Conformità a JDBC 4.3 per il driver JDBC](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="support-for-sql-data-discovery-and-classification"></a>Supporto di individuazione dati e classificazione SQL

Il driver Microsoft JDBC 7.0 per SQL Server fornisce supporto per l'individuazione di dati SQL e la classificazione con qualsiasi database di destinazione che supporta questa funzionalità. Il driver ora espone le API `SQLServerResultSet.getSensitivityClassification()` per estrarre queste informazioni dal `ResultSet` recuperato.

Per altre informazioni su come usare questa funzionalità con il driver JDBC, vedere l'esempio in [Individuazione dati e classificazione SQL](data-discovery-classification-sample.md).

### <a name="added-connection-property-usebulkcopyforbatchinsert"></a>Aggiunta della proprietà di connessione: useBulkCopyForBatchInsert

Il driver Microsoft JDBC 7.0 per SQL Server introduce una nuova proprietà di connessione, `useBulkCopyForBatchInsert`. Questa proprietà è supportata solo per Azure Synapse Analytics.

Questa proprietà è disabilitata per impostazione predefinita. È possibile abilitarla per migliorare le prestazioni delle applicazioni utente quando si esegue il push di grandi quantità di dati in Azure Synapse Analytics. L'abilitazione di questa proprietà modifica il comportamento delle operazioni di inserimento batch per passare a operazioni di copia bulk con dati forniti dall'utente. Per altre informazioni su questa proprietà e le relative limitazioni, vedere [Uso dell'API di copia bulk per un'operazione di inserimento batch](use-bulk-copy-api-batch-insert-operation.md).

### <a name="added-connection-property-cancelquerytimeout"></a>Aggiunta della proprietà di connessione: cancelQueryTimeout

Il driver Microsoft JDBC 7.0 per SQL Server introduce una nuova proprietà di connessione, `cancelQueryTimeout` per annullare `queryTimeout` su oggetti `java.sql.Connection` e `java.sql.Statement`.

### <a name="added-azure-key-vault-provider-constructors"></a>Aggiunta di costruttori per il provider Azure Key Vault

Il driver Microsoft JDBC 7.0 per SQL Server reintroduce un costruttore rimosso in precedenza, per `SQLServerColumnEncryptionAzureKeyVaultProvider`. È consentita l'autenticazione tramite un metodo personalizzato implementato su `SQLServerKeyVaultAuthenticationCallback` per recuperare un token di accesso.

I nuovi costruttori hanno la definizione seguente:

```java
/* This constructor is added to provide backward compatibility with 6.0
* version of the driver. It is marked deprecated for removal in the next
* stable release.
*/
@Deprecated
public SQLServerColumnEncryptionAzureKeyVaultProvider(
        SQLServerKeyVaultAuthenticationCallback authenticationCallback,
        ExecutorService executorService) throws SQLServerException;

/*New constructor to replace the above constructor*/
public SQLServerColumnEncryptionAzureKeyVaultProvider(
            SQLServerKeyVaultAuthenticationCallback authenticationCallback) throws SQLServerException;
```

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-160"></a>Aggiornamento di "Microsoft Azure Active Directory Authentication Library (ADAL4J) per Java" versione: 1.6.0

Il driver Microsoft JDBC 7.0 per SQL Server ha aggiornato la dipendenza di Maven da "Microsoft Azure Active Directory Authentication Library (ADAL4J) fo Java" alla versione 1.6.0. Per altre informazioni sulle dipendenze, vedere [Dipendenze delle funzionalità di Microsoft JDBC Driver per SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="64"></a>6.4

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 6.4 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122436)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 6.4 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122537)**  

Numero di versione: 6.4.0  
Resa disponibile: 27 febbraio 2018

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40a)  

Il driver Microsoft JDBC 6.4 per SQL Server è completamente conforme alla specifica JDBC 4.1 e 4.2. I file JAR nel pacchetto 6.4 sono denominati in base alla compatibilità delle versioni di Java. Ad esempio, il file mssql-jdbc-6.4.0.jre8.jar dal pacchetto 6.4 deve essere usato con Java 8.

### <a name="support-for-jdk-9"></a>Supporto per JDK 9

Il driver supporta JDK versione 9.0, oltre a JDK 8.0 e 7.0.

### <a name="jdbc-43-compliance"></a>Conformità a JDBC 4.3

Il driver supporta la specifica Java Database Connectivity API 4.3, oltre a 4.1 e 4.2. I metodi dell'API di JDBC 4.3 sono stati aggiunti ma non ancora implementati. Per informazioni dettagliate, vedere [Conformità a JDBC 4.3 per il driver JDBC](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="added-connection-property-sslprotocol"></a>Aggiunta della proprietà di connessione: sslProtocol

Una nuova proprietà di connessione consente agli utenti di specificare la parola chiave per il protocollo TLS. I valori possibili sono: "TLS", "TLSv1", "TLSv1.1" e "TLSv1.2". Per informazioni dettagliate, vedere [SSLProtocol](https://github.com/Microsoft/mssql-jdbc/wiki/SSLProtocol).

### <a name="deprecated-connection-property-fipsprovider"></a>Proprietà di connessione deprecata: fipsProvider

La proprietà di connessione `fipsProvider` è stata rimossa dall'elenco delle proprietà di connessione accettate. Per informazioni dettagliate, vedere la [richiesta pull di GitHub](https://github.com/Microsoft/mssql-jdbc/pull/460) correlata.

### <a name="added-connection-properties-for-specifying-a-custom-trustmanager"></a>Aggiunta di proprietà di connessione per specificare un TrustManager personalizzato

Il driver supporta ora la specifica di un TrustManager personalizzato con le proprietà di connessione aggiunte `trustManagerClass` e `trustManagerConstructorArg`. È possibile specificare in modo dinamico un set di certificati considerati attendibili per ogni connessione, senza modificare le impostazioni globali per l'ambiente Java Virtual Machine (JVM).

### <a name="added-support-for-datetimesmalldatetime-in-table-valued-parameters"></a>Aggiunta del supporto per datetime/smallDatetime nei parametri con valori di tabella

Il driver supporta ora i tipi di dati `datetime` e `smallDatetime` quando si usano parametri con valori di tabella (TVP).

### <a name="added-support-for-the-sql_variant-datatype"></a>Aggiunta del supporto per il tipo di dati sql_variant

Il driver JDBC supporta ora i tipi di dati `sql_variant` da usare con SQL Server. Il tipo di dati `sql_variant` è supportato anche con funzionalità quali parametri con valori di tabella e copia bulk con le limitazioni seguenti:

* **Per i valori di data**:

  Quando si usano parametri con valori di tabella per popolare una tabella che contiene valori `datetime`, `smalldatetime` o `date` archiviati in una colonna `sql_variant`, la chiamata del metodo `getDateTime()`, `getSmallDateTime()` o `getDate()` sul set di risultati non funziona e genera l'eccezione seguente:

  `java java.lang.String cannot be cast to java.sql.Timestamp`

  In alternativa, usare il metodo `getString()` o `getObject()`.

* **Uso di TVP con sql_variant per valori null**:
  
  Se si usano parametri con valori di tabella per popolare una tabella e inviare un valore NULL al tipo di colonna `sql_variant`, verrà generata un'eccezione. L'inserimento di un valore NULL con il tipo di colonna `sql_variant` in un parametro con valori di tabella non è attualmente supportato.

### <a name="implemented-prepared-statement-metadata-caching"></a>Implementazione della memorizzazione nella cache dei metadati delle istruzioni preparate

Il driver JDBC ha implementato la memorizzazione nella cache dei metadati per le istruzioni preparate per migliorare le prestazioni. Il driver supporta ora la memorizzazione nella cache dei metadati per le istruzioni preparate nel driver con le proprietà di connessione `disableStatementPooling` e `statementPoolingCacheSize`. Questa funzionalità è disabilitata per impostazione predefinita. Per altre informazioni, vedere [Memorizzazione nella cache dei metadati delle istruzioni preparate per il driver JDBC](prepared-statement-metadata-caching-for-the-jdbc-driver.md).

### <a name="added-support-for-azure-ad-integrated-authentication-on-linuxmacos"></a>Aggiunta del supporto per l'autenticazione integrata di Azure AD in Linux/macOS

Il driver JDBC ora supporta l'autenticazione integrata di Azure Active Directory in tutti i sistemi operativi supportati (Windows, Linux e macOS) con Kerberos. In alternativa, nei sistemi operativi Windows gli utenti possono eseguire l'autenticazione con mssql-jdbc_auth-\<version>-\<arch>.dll.

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-140"></a>Aggiornamento di "Microsoft Azure Active Directory Authentication Library (ADAL4J) per Java" versione: 1.4.0

Il driver JDBC ha aggiornato la dipendenza di Maven da "Microsoft Azure Active Directory Authentication Library (ADAL4J) for Java" alla versione 1.4.0. Per altre informazioni sulle dipendenze, vedere [Dipendenze delle funzionalità di Microsoft JDBC Driver per SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="62"></a>6.2

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 6.2 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122616)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 6.2 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122615)**  

Numero di versione: 6.2.2  
Resa disponibile: 29 settembre 2017

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40a)  

Il driver Microsoft JDBC 6.2 per SQL Server è completamente conforme alla specifica JDBC 4.1 e 4.2. I file JAR nel pacchetto 6.2 sono denominati in base alla compatibilità delle versioni di Java. Ad esempio, è consigliato l'uso del file mssql-jdbc-6.2.2.jre8.jar dal pacchetto 6.2 con Java 8.

### <a name="releases"></a>Rilasci

Numero di versione: 6.2.2  
Resa disponibile: 3 ottobre 2017  
Problemi risolti:  

- Aggiornata la dipendenza ADAL4J alla versione 1.2.0 e la dipendenza Azure Key Vault alla versione 1.0.0

Numero di versione: 6.2.1  
Resa disponibile: 14 luglio 2017  
Problemi risolti:  

- Correzione di un problema durante l'esecuzione di query senza parametri usando `preparedStatement`

Numero di versione: 6.2.0  
Resa disponibile: 30 giugno 2017  

> [!NOTE]  
> È stato rilevato un problema con il miglioramento della memorizzazione nella cache dei metadati nella versione JDBC 6.2 RTW rilasciata il 29 giugno 2017. È stato eseguito il rollback del miglioramento e sono stati rilasciati nuovi file JAR (versione 6.2.1) in data 17 luglio 2017.
>
> Un altro miglioramento ha aggiornato la versione della libreria dipendente da Azure Key Vault alla versione 1.0.0 e sono stati rilasciati nuovi file JAR (versione 6.2.2) in data 19 ottobre 2017.
>
> Scaricare gli aggiornamenti più recenti per il driver JDBC 6.2 tramite i collegamenti precedenti, [GitHub](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2) o [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver). Aggiornare i progetti per l'uso dei file JAR della versione 6.2.2. Per altre informazioni, vedere le note sulla versione per le versioni [6.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.1) e [6.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2).

### <a name="azure-ad-support-for-linux"></a>Supporto di Azure AD per Linux

Connettere le applicazioni Linux al database SQL di Azure con l'autenticazione di Azure AD tramite i metodi con nome utente/password e token di accesso.

### <a name="fips-enabled-jvms"></a>JVM abilitate per FIPS

È ora possibile usare il driver JDBC su JVM eseguite in modalità conformità 140 FIPS (Federal informazioni Processing Standard) per soddisfare gli standard federali sulla conformità.

### <a name="kerberos-authentication-improvements"></a>Miglioramenti per l'autenticazione Kerberos

Il driver JDBC include ora il supporto per:

* Metodo basato su entità di sicurezza/password per le applicazioni in cui la configurazione di Kerberos non può essere modificata o non è possibile recuperare un nuovo token o keytab. Questo metodo può essere usato per l'autenticazione in un'istanza di SQL Server che consente solo l'autenticazione Kerberos.
* Autenticazione tra aree di autenticazione che usa l'autenticazione integrata Kerberos senza impostare in modo esplicito il nome dell'entità servizio (SPN) del server. Il driver calcola ora automaticamente l'area di autenticazione anche quando non viene fornita.
* Delega vincolata Kerberos con l'accettazione di credenziali utente rappresentate come oggetto credenziali GSS tramite l'origine dati. Queste credenziali rappresentate vengono poi usate per stabilire una connessione Kerberos.

### <a name="added-timeouts"></a>Aggiunta dei timeout

Il driver JDBC supporta ora i timeout configurabili seguenti. È possibile modificarli in base alle esigenze dell'applicazione.

* Timeout delle query per controllare il numero di secondi di attesa prima che si verifichi un timeout durante l'esecuzione di una query.
* Timeout del socket per specificare il numero di millisecondi di attesa prima che si verifichi un timeout per un'operazione di lettura o accettazione del socket.

## <a name="61"></a>6.1

Numero di versione: 6.1.0  
Resa disponibile: 17 novembre 2016  

Il driver Microsoft JDBC 6.1 per SQL Server è completamente conforme alla specifica JDBC 4.1 e 4.2. Questa è la versione open source iniziale del driver JDBC. Il codice sorgente è disponibile nel [tag GitHub v6.1.0](https://github.com/microsoft/mssql-jdbc/releases/tag/v6.1.0). Compila i file mssql-jdbc-6.1.0.jre8.jar e mssql-jdbc-6.1.0.jre7.jar, che corrispondono alla compatibilità delle versioni di Java.

## <a name="60"></a>6.0

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 6.0 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122617)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 6.0 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122714)**  

Numero di versione: 6.0.8112  
Resa disponibile: 17 gennaio 2017

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40a)  

Il driver Microsoft JDBC 6.0 per SQL Server è completamente conforme alla specifica JDBC 4.1 e 4.2. I file JAR nel pacchetto 6.0 sono denominati in base alla relativa conformità con la versione dell'API JDBC. Ad esempio, il file sqljdbc42.jar dal pacchetto 6.0 è conforme all'API JDBC 4.2. Analogamente, il file sqljdbc41.jar è conforme all'API JDBC 4.1.

Per assicurarsi di avere il file corretto sqljdbc41.jar o sqljdbc42.jar, eseguire le righe di codice seguenti. Se l'output è "Versione driver: 6.0.7507.100", è disponibile il pacchetto JDBC Driver 6.0.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="always-encrypted"></a>Always Encrypted

Il driver supporta la funzionalità Always Encrypted in SQL Server 2016. Questa funzionalità garantisce che i dati sensibili non vengano mai visualizzati in testo non crittografato in un'istanza di SQL Server. Always Encrypted crittografa in modo trasparente i dati nell'applicazione, in modo che SQL Server debba gestire solo i dati crittografati e non i valori di testo non crittografato. In caso di compromissione dell'istanza di SQL Server o del computer host, un utente malintenzionato ottiene solo il testo crittografato dei dati sensibili. Per informazioni dettagliate, vedere [Uso di Always Encrypted con il JDBC Driver](using-always-encrypted-with-the-jdbc-driver.md).

### <a name="internationalized-domain-names"></a>Nomi IDN (Internationalized Domain Name)

Il driver supporta i nomi IDN (Internationalized Domain Name) per i nomi dei server. Per informazioni dettagliate, vedere "Utilizzo di International Domain Names (IDN)" nell'articolo [Caratteristiche internazionali del driver JDBC](international-features-of-the-jdbc-driver.md).

### <a name="parameterized-queries"></a>Query con parametri

Il driver supporta ora il recupero dei metadati dei parametri con istruzioni preparate per le query complesse, ad esempio le sottoquery e/o i join. Si noti che questo miglioramento è disponibile solo quando si usa SQL Server 2012 e versioni successive.

### <a name="azure-active-directory"></a>Azure Active Directory

L'autenticazione di Azure AD è un meccanismo di connessione al database SQL di Azure v12 tramite identità in Azure AD. Usare l'autenticazione di Azure AD per gestire centralmente le identità degli utenti del database e come alternativa all'autenticazione di SQL Server.

È possibile usare il driver JDBC 6.0 per specificare le credenziali di Azure AD nella stringa di connessione JDBC per connettersi al database SQL di Azure. Per informazioni dettagliate, vedere la proprietà di autenticazione nell'articolo [Impostazione delle proprietà delle connessioni](setting-the-connection-properties.md).

### <a name="table-valued-parameters"></a>Parametri con valori di tabella

I parametri con valori di tabella offrono un modo semplice per effettuare il marshalling di più righe di dati da un'applicazione client di SQL Server senza richiedere più round trip o una logica speciale sul lato server per l'elaborazione dei dati. I parametri con valori di tabella possono essere usati per incapsulare le righe di dati in un'applicazione client e inviare i dati al server in un singolo comando con parametri. Le righe di dati in ingresso vengono archiviate in una variabile di tabella su cui è possibile operare tramite Transact-SQL. Per informazioni dettagliate, vedere [Uso di parametri con valori di tabella](using-table-valued-parameters.md).

### <a name="always-on-availability-groups"></a>Gruppi di disponibilità AlwaysOn

Il driver supporta ora le connessioni trasparenti ai gruppi di disponibilità Always On. Il driver individua rapidamente la topologia AlwaysOn corrente dell'infrastruttura server e si connette in modo trasparente al server attivo corrente.

## <a name="42"></a>4.2

**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 4.2 per SQL Server (EXE autoestraente)](https://go.microsoft.com/fwlink/?linkid=2122538)**  
**[![Download](../../ssms/media/download-icon.png) Scaricare Microsoft JDBC Driver 4.2 per SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122437)**  

Numero di versione: 4.2.8112  
Resa disponibile: 24 agosto 2015

Se è necessario scaricare il driver in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver in un file EXE autoestraente: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40a)  
Per il driver in un file tar.gz: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40a)  

Il driver Microsoft JDBC 4.2 per SQL Server è completamente conforme alla specifica JDBC 4.1 e 4.2. I file JAR nel pacchetto 4.2 sono denominati in base alla relativa conformità con la versione dell'API JDBC. Ad esempio, il file sqljdbc42.jar dal pacchetto 4.2 è conforme all'API JDBC 4.2. Analogamente, il file sqljdbc41.jar è conforme all'API JDBC 4.1.

Per assicurarsi di avere il file corretto sqljdbc41.jar o sqljdbc42.jar, eseguire le righe di codice seguenti. Se l'output è "Versione driver: 4.2.6420.100", è disponibile il pacchetto JDBC Driver 4.2.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="support-for-jdk-8"></a>Supporto per JDK 8

Il driver supporta JDK versione 8.0, oltre a JDK 7.0, 6.0 e 5.0.

### <a name="jdbc-41-and-42-compliance"></a>Conformità con JDBC 4.1 e 4.2

Il driver supporta le specifiche Java Database Connectivity API 4.1 e 4.2, oltre a 4.0. Per informazioni dettagliate, vedere [Conformità con JDBC 4.1 per il driver JDBC](jdbc-4-1-compliance-for-the-jdbc-driver.md) e [Conformità con JDBC 4.2 per il driver JDBC](jdbc-4-2-compliance-for-the-jdbc-driver.md).

### <a name="bulk-copy"></a>Copia bulk

La funzionalità di copia bulk viene usata per copiare rapidamente grandi quantità di dati in tabelle o viste nei database di SQL Server. Per informazioni dettagliate, vedere [Uso della copia bulk con il driver JDBC](using-bulk-copy-with-the-jdbc-driver.md).

### <a name="xa-transaction-rollback-option"></a>Opzione di rollback di transazione XA

Il driver include nuove opzioni di timeout per il rollback automatico esistente di transazioni non preparate. Per informazioni dettagliate, vedere [Informazioni sulle transazioni XA](understanding-xa-transactions.md).

### <a name="new-kerberos-principal-connection-property"></a>Nuova proprietà di connessione principale Kerberos

Il driver usa una nuova proprietà di connessione per aumentare la flessibilità con le connessioni Kerberos. Per informazioni dettagliate, vedere [Uso dell'autenticazione integrata Kerberos per la connessione a SQL Server](using-kerberos-integrated-authentication-to-connect-to-sql-server.md).

## <a name="41"></a>4.1

Numero di versione: 4.1.8112  
Resa disponibile: 12 dicembre 2014

### <a name="support-for-jdk-7"></a>Supporto per JDK 7

Il driver supporta JDK versione 7.0, oltre a JDK 6.0 e 5.0.

## <a name="itanium-not-supported-for-jdbc-driver-applications"></a>Itanium non supportato per le applicazioni JDBC Driver

Le applicazioni Microsoft JDBC Driver per SQL Server non supportano i computer Itanium.

## <a name="see-also"></a>Vedere anche

[Panoramica del driver JDBC](overview-of-the-jdbc-driver.md)
