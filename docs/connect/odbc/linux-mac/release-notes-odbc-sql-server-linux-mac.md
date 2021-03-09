---
title: Note sulla versione per ODBC Driver for SQL Server in Linux e macOS
description: Informazioni sulle novità e le modifiche per le versioni rilasciate di Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: v-chojas
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: 83f8dbb834af379e02635130e23c0f5fd7299f3b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464286"
---
# <a name="release-notes-for-the-microsoft-odbc-driver-for-sql-server-on-linux-and-macos"></a>Note sulla versione per Microsoft ODBC Driver for SQL Server in Linux e macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Questo articolo elenca e descrive le nuove funzionalità dei rilasci con versioni di [!INCLUDE[msCoName](../../../includes/msconame_md.md)]ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux e macOS.

<!--
Going forward, please use the new 2-column markdown table for each new H2 version section.
And we are keeping the H2 titles much shorter, partly by removing redundant or obvious info.
We want to track the Month YYYY each new version H2 section is added, at the end of the H2 title.
This is the new standard format for Release Notes article content.

OLD     FILE NAME:    linux-mac/release-notes.md
NOW NEW FILE NAME:    linux-mac/release-notes-odbc-sql-server-linux-mac.md

Thank you.
GeneMi.  2019/04/03.
-->

## <a name="177-january-2021"></a>17,7, gennaio 2021

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Nuove distribuzioni supportate. | Ubuntu 20,10, macOS Big Sur (11,0), Oracle Linux 7 |
| Autenticazione di un'entità servizio | Vedere [Parole chiave e attributi per stringhe di connessione e DSN](../dsn-connection-string-attribute.md). |
| Possibilità di inserire in colonne Money e smallmoney crittografate | Vedere [Uso di Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Correzioni di bug. | [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="176-july-2020"></a>17.6, luglio 2020

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Nuove distribuzioni supportate. | Ubuntu 20.04 |
| Supporto dell'autenticazione federata | Vedere [Uso di Azure Active Directory](../using-azure-active-directory.md). |
| Memorizzazione nella cache dei metadati per le istruzioni preparate | Vedere [Uso di Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Attributo di connessione SQL_COPT_SS_AUTOBEGINTXN per controllare l'esecuzione automatica di BEGIN TRANSACTION dopo ROLLBACK o COMMIT | Vedere [Parole chiave e attributi per stringhe di connessione e DSN](../dsn-connection-string-attribute.md). |
| Correzioni di bug. | [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="17522-april-2020-alpine-linux-only"></a>17.5.2.2, aprile 2020 (solo Alpine Linux)

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Bug risolto. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1752-march-2020"></a>17.5.2, marzo 2020

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per l'autenticazione con l'identità gestita per Azure Key Vault | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Supporto per endpoint Azure Key Vault aggiuntivi | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="175-january-2020"></a>17.5, gennaio 2020

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Attributo di connessione SQL_COPT_SS_SPID per recuperare SPID senza round trip al server | Vedere [Parole chiave e attributi per stringhe di connessione e DSN](../dsn-connection-string-attribute.md). |
| Supporto per l'indicazione dell'accettazione del contratto di licenza tramite `debconf` in Debian e Ubuntu | Vedere[Installazione del driver](./installing-the-microsoft-odbc-driver-for-sql-server.md). |
| Nuove distribuzioni supportate. | &bull; &nbsp; &nbsp; Alpine Linux (3.10, 3.11)<br/>&bull; &nbsp; &nbsp; Oracle Linux 8<br/>&bull; &nbsp; &nbsp; Ubuntu 19.10<br/>&bull; &nbsp; &nbsp; macOS 10.15 |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1742-october-2019"></a>17.4.2, ottobre 2019

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per endpoint Azure Key Vault aggiuntivi | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Supporto per l'impostazione della versione di classificazione dei dati | Vedere [Classificazione dei dati](../data-classification.md#bkmk-version). |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

**Problema noto:**

Quando si usa Always Encrypted con enclave sicuri e Azure Key Vault, i percorsi delle chiavi con lunghezza dispari possono causare errori di verifica della firma CMK. Se si verifica questo problema, provare a modificare di un carattere la lunghezza del percorso della chiave rinominando la chiave Azure Key Vault.

## <a name="174-august-2019"></a>Versione 17.4, agosto 2019

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Always Encrypted con enclave sicuri. | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Caricamento dinamico di OpenSSL | Vedere [Linee guida per la programmazione](programming-guidelines.md#bkmk-openssl). |
| Impostazioni keep-alive TCP configurabili. | Vedere [Connessione a SQL Server](connection-string-keywords-and-data-source-names-dsns.md). |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="173-february-2019"></a>17.3, febbraio 2019

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Nuove distribuzioni supportate. | &bull; &nbsp; &nbsp; SUSE 15<br/>&bull; &nbsp; &nbsp; Ubuntu 18.10<br/>&bull; &nbsp; &nbsp; macOS 10.14 |
| Modalità di autenticazione dell'identità del servizio gestita di Azure Active Directory (assegnata dal sistema e dall'utente). | Vedere [Uso di Azure Active Directory con il driver ODBC](../using-azure-active-directory.md). |
| Capacità di trasmettere i parametri di input alle colonne Always Encrypted. | Per altre informazioni, vedere [Limitazioni del driver ODBC quando si usa Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted). |
| Transazioni distribuite XA. | Vedere [Uso delle transazioni XA](../use-xa-with-dtc.md).<br/><br/>XA è l'acronimo di _eXtended Architecture_, uno standard per l'esecuzione di una transazione globale che accede a più di un sistema di archiviazione dei dati lato server. |
| &nbsp; | &nbsp; |

## <a name="172-july-2018"></a>17.2, luglio 2018

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Nuove distribuzioni supportate. | &bull; &nbsp; &nbsp; Ubuntu 18.04 |
| Classificazione dei dati per il database SQL di Microsoft Azure e SQL Server. | Vedere [Classificazione dei dati](../data-classification.md). |
| Supporto della codifica server UTF-8. | &nbsp; |
| `SQLBrowseConnect` | &nbsp; |
| Dipendenza dinamica da `libcurl`. | A partire da questa versione, il pacchetto `libcurl` non è una dipendenza esplicita.<br/>Il pacchetto `libcurl` per OpenSSL o NSS è necessario quando si usa Azure Key Vault o l'autenticazione di Azure Active Directory.<br/>Se si verifica un errore relativo a `libcurl`, verificare che sia installato. |
| Resilienza delle connessioni inattive con parole chiave ConnectRetryCount e ConnectRetryInterval nella stringa di connessione. | &bull; &nbsp; &nbsp; Usare `SQL_COPT_SS_CONNECT_RETRY_COUNT`(sola lettura) per recuperare il numero di nuovi tentativi di connessione.<br/><br/>&bull; &nbsp; &nbsp; Usare `SQL_COPT_SS_CONNECT_RETRY_INTERVAL`(sola lettura) per recuperare la durata dell'intervallo dei tentativi di connessione.<br/><br/>Vedere [Resilienza di connessione nel driver ODBC di Windows](../windows/connection-resiliency-in-the-windows-odbc-driver.md). |
| Correzioni di bug. | [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="171-march-2018"></a>17.1, marzo 2018

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Supporto degli attributi di connessione `SQL_COPT_SS_CEKCACHETTL` e `SQL_COPT_SS_TRUSTEDCMKPATHS`. | &bull; &nbsp; &nbsp; `SQL_COPT_SS_CEKCACHETTL` consente di controllare l'ora in cui è presente la cache locale delle chiavi di crittografia della colonna e di scaricarla.<br/><br/>&bull; &nbsp; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS` consente all'applicazione di limitare le operazioni Always Encrypted in modo che usino solo l'elenco specificato di chiavi master della colonna.<br/><br/>Vedere [Uso di Always Encrypted con ODBC Driver for SQL Server](../using-always-encrypted-with-the-odbc-driver.md). |
| Supporto per il caricamento di `.rll` dalla posizione predefinita. | Vedere la [sezione relativa al caricamento di file nel documento di installazione](installing-the-microsoft-odbc-driver-for-sql-server.md#resource-file-loading). |
| Correzioni di bug. | [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="17"></a>17

**Nuove distribuzioni supportate**: macOS High Sierra e Ubuntu 17.10

**Miglioramenti delle prestazioni**: prestazioni migliorate di oltre 10 volte quando il driver esegue la conversione da e verso UTF-8/16.

**Funzionalità aggiunte**:

Supporto per Always Encrypted per l'API BCP

Il nuovo attributo della stringa di connessione UseFMTOnly fa in modo che i driver usino metadati legacy in casi particolari che richiedono tabelle temporanee.

Supporto di Istanza gestita di database SQL di Azure.

> [!NOTE]
> Esistono molte differenze nell'uso di Istanza gestita:
>
> - FILESTREAM non è supportato
> - L'accesso al file system locale non è supportato, ma è obbligatorio per elementi come i file di traccia
> - La creazione di tipi definiti dall'utente dal percorso locale non è supportata
> - L'autenticazione integrata di Windows non è supportata
> - DTC non è supportato
> - L'account 'sa' non è presente (l'account predefinito si chiama 'cloudSA')
> - L'errore del token TDS (0xAA) restituisce il nome server errato
> - Non sono supportati i caratteri speciali nel nome del database
> - Non è supportato ALTER DATABASE [dbname1] MODIFY NAME = [dbname2]
> - I messaggi di errore vengono sempre visualizzati in inglese, indipendentemente dalle impostazioni della lingua (come in Azure)

## <a name="131-for-ssnoversion-on-linux-and-macos-may-2017"></a>13.1, per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux e macOS, maggio 2017

ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aggiunge il supporto per Always Encrypted e Azure Active Directory quando viene usato in combinazione con Microsoft SQL Server 2016.

**Nuove distribuzioni supportate**: sono supportati OS X 10.11 e macOS 10.12 nella prima versione del driver ODBC in macOS. È ora supportato anche Ubuntu 16.10 insieme a Red Hat 6, 7 e SUSE 12. Ogni piattaforma ha un pacchetto piattaforma pertinente (RPM o DEB) per semplificare l'installazione e la configurazione. Per altre informazioni, vedere le istruzioni per l'installazione del driver ODBC per [Linux](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) e [macOS](../../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md).

**Modifiche al supporto di Gestione driver unixODBC 2.3.1**: il driver ODBC non dipende più dai pacchetti personalizzati per Gestione driver unixODBC (tranne in RedHat 6) e si basa invece sulla gestione dei pacchetti di distribuzione per risolvere la dipendenza UnixODBC dai repository di distribuzione.

**Supporto per l'API BCP**: il driver ODBC per Linux e macOS supporta ora l'uso delle [funzioni dell'API BCP (**bcp_init** e così via)](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md).

## <a name="130-for-ssnoversion-on-linux"></a>13.0, per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux

Con Microsoft ODBC Driver 13.0 for SQL Server, sono ora supportati anche SQL Server 2014 e SQL Server 2016.  

**Nuove distribuzioni supportate**:

Ubuntu è ora supportato, insieme a Red Hat e SUSE. Ogni piattaforma ha un pacchetto piattaforma pertinente (RPM o DEB) per semplificare l'installazione e la configurazione.  Per le istruzioni di installazione, vedere l'articolo relativo all'[installazione del driver](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

**Supporto di Gestione driver unixODBC 2.3.1**: oltre a una versione più recente di Gestione driver, è disponibile un pacchetto per l'installazione di questa dipendenza che semplifica l'installazione e la configurazione.  

**Risoluzione dell'IP di rete trasparente**: Risoluzione dell'IP di rete trasparente è una revisione della funzionalità di failover su più subnet esistente che interessa la sequenza di connessione del driver nel caso in cui il primo indirizzo IP risolto del nome host non risponda e siano presenti più indirizzi IP associati al nome host.

**Supporto di TLS 1.2**: Microsoft ODBC Driver 13.0 for SQL Server in Linux supporta ora TLS 1.2 quando vengono usate le comunicazioni protette con SQL Server.

## <a name="11-for-ssnoversion-on-linux"></a>11, per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Linux

Il driver ODBC in SUSE Linux (anteprima) supporta SUSE Linux Enterprise 11 Service Pack 2 a 64 bit. Per altre informazioni, vedere [System Requirements](../../../connect/odbc/linux-mac/system-requirements.md).  

Il driver ODBC in Linux supporta [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]. Per altre informazioni, vedere [ODBC Driver on Linux Support for High Availability, Disaster Recovery](../../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md) (Supporto del driver ODBC in Linux per disponibilità elevata e ripristino di emergenza).  

Il driver ODBC in Linux supporta le connessioni al database SQL di Azure.

L'opzione `-l` (timeout di accesso) è stata aggiunta a `bcp`. Per altre informazioni, vedere [Connessione a **bcp**](../../../connect/odbc/linux-mac/connecting-with-bcp.md).
