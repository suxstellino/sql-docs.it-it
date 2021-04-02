---
title: Note sulla versione dei driver Microsoft per PHP
description: Questa pagina elenca le variazioni in ogni versione dei driver Microsoft per PHP per SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- what's new in version 1.1
ms.assetid: 91cca3d2-ba99-4a6d-b0de-beb9699cb3f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a225c2fae4b20ad75666415e4315ec12c7f7aab9
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2021
ms.locfileid: "106176958"
---
# <a name="release-notes-for-the-microsoft-drivers-for-php-for-sql-server"></a>Note sulla versione dei driver Microsoft per PHP per SQL Server

Questa pagina illustra gli elementi aggiunti in ogni versione dei [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

<!--
Hello, We are standardizing the format of content inside our Release Notes (or What's New) articles.
Instead of bullets (or paragraphs), we have shifted to the 2-column format you see for H2 **What's New in Version 5.6**.
It is not necessary to reformat all the older H2 sections in this Release Notes file, but.....

Going forward, please be sure to use the 2-column format.

Also, all Release Notes .md file names now must begin with 'release-notes-*.md'.  And no filler words.
The 5.6 edition of this file is being renamed.....
FROM:  'release-notes-for-the-php-sql-driver.md'
TO  :  'release-notes-php-sql-driver.md'

For any questions, ask GeneMi or CraigG.
Thanks a lot.  2019-03-28  (DevO= 1467988)
-->

## <a name="59"></a>5.9

![download scaricare il ](../../ssms/media/download-icon.png) tag di versione GitHub [pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=?????) 
 [(i pacchetti Linux e MacOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.9.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.9.0<sup>1</sup>
- Rilasciata: 29 gennaio 2021

### <a name="whats-new-in-59"></a>Novità di 5,9

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Aggiunta del supporto per PHP 8,0. | &nbsp; |
| Il supporto per PHP 7,2 è stato eliminato. | &nbsp; |
| Aggiunta del supporto per Microsoft ODBC driver 17,7 su tutte le piattaforme. | &nbsp; |
| Aggiunta del supporto per macOS Big Sur, Ubuntu 20,04, Ubuntu 20,10 ed alpine 3,12. | Alcuni richiedono il driver ODBC 17,5 o versione successiva. |
| È stato rilasciato il supporto per macOS High Sierra, Debian 8 e Ubuntu 19,10. | &nbsp; |
| Supporto per le impostazioni locali di GB18030. | &nbsp; |
| DOP esteso `errorinfo` per includere messaggi ODBC aggiuntivi, se disponibili. | &nbsp; |
| Supporto per la classificazione dei dati con informazioni sul rango. | Richiede SQL Server 2019 e il driver ODBC 17.4.2 o versione successiva. |
| Aggiunta Azure Active Directory supporto per l'autenticazione dell'entità servizio. | Richiede il driver ODBC 17,7 o versione successiva. |
| Miglioramento delle prestazioni quando si gestiscono numeri decimali come input o output e si rimuovono le conversioni non necessarie per i valori numerici. | &nbsp; |
| Miglioramento delle prestazioni durante il recupero di numeri con buffer client. | &nbsp; |
| Impostare timeout query senza usare TIMEOUT blocco, che consente di salvare un ulteriore viaggio nel server. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> questa versione richiede il driver ODBC 17.4.2 o versione successiva. In caso contrario, verrà visualizzato un avviso relativo alla mancata impostazione di un attributo. Questo avviso può essere eliminato quando si utilizza un driver ODBC meno recente. Se si usa SQLSRV, vedere [procedura: configurare la gestione degli errori e degli avvisi con il driver sqlsrv](./how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver.md). Se si utilizza PDO_SQLSRV, gli avvisi vengono eliminati per impostazione predefinita, ma possono essere registrati. Per informazioni dettagliate, controllare l' [attività di registrazione](./logging-activity.md) .

## <a name="previous-releases"></a>Versioni precedenti

## <a name="581"></a>5.8.1

Questa versione si applica solo a Linux e macOS.

[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.1)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.8.1
- Resa disponibile: 15 aprile 2020

### <a name="whats-new-in-581"></a>Novità della versione 5.8.1

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Correzione di bug | Correzione di problemi con le impostazioni locali predefinite in Alpine Linux. |
| Correzione di bug | Rimossa la struttura dei dati non necessaria per il supporto della funzionalità cursori lato client in Alpine Linux. |
| Correzione di bug | Risolti problemi di registrazione quando sono abilitati entrambi i driver in Alpine Linux. |
| &nbsp; | &nbsp; |

## <a name="58"></a>5.8

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120362)  
[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.8.0
- Resa disponibile: 31 gennaio 2020

### <a name="whats-new-in-58"></a>Novità della versione 5.8

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Aggiunta del supporto per PHP 7.4. | &nbsp; |
| Eliminazione del supporto per PHP 7.1. | &nbsp; |
| Aggiunta del supporto per il driver Microsoft ODBC 17.5 per tutte le piattaforme. | &nbsp; |
| Aggiunta del supporto per Debian 10 e Red Hat 8. | Entrambi richiedono il driver ODBC 17.4 o versione successiva. |
| Aggiunta del supporto per macOS Catalina, Alpine Linux 3.11<sup>1</sup> e Ubuntu 19.10. | Tutti richiedono il driver ODBC 17.5 o versione successiva. |
| Eliminazione del supporto per SQL Server 2008 R2, macOS Sierra, Ubuntu 18.10 e Ubuntu 19.04. | &nbsp; |
| Supporto per l'opzione Lingua al momento della connessione a SQL Server. | &nbsp; |
| Supporto per i tipi di stringa estesi PHP introdotti in PHP 7.2. | &nbsp; |
| Supporto per il recupero dei metadati di riservatezza della classificazione dati. | Richiede SQL Server 2019 e il driver ODBC 17.4.2 o versione successiva. |
| Supporto di Always Encrypted con enclave sicuri. | Richiede il driver ODBC 17.4 o versione successiva. |
| Supporto delle opzioni configurabili per le impostazioni locali in Linux e macOS. |
| Miglioramento delle prestazioni grazie alla memorizzazione dei metadati nella cache durante le operazioni di recupero e all'omissione di chiamate ridondanti. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> Il supporto Alpine Linux è sperimentale per la versione 5.8.

## <a name="561"></a>5.6.1

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120446)  
[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.1)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.6.1
- Resa disponibile: 19 marzo 2019

### <a name="whats-new-in-561"></a>Novità della versione 5.6.1

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Correzione di bug | Correzione dei presupposti eseguiti durante il calcolo dei metadati del campo o della colonna che possono avere causato la chiusura dell'applicazione. |
| Correzione di bug | Modifica del file di configurazione sqlsrv in modo che possa essere compilato indipendentemente da pdo_sqlsrv. |
| Correzione di bug | Correzione di PDOStatement::getColumnMeta() perché sia restituito False quando si verifica un errore. |
| &nbsp; | &nbsp; |

## <a name="56"></a>5.6

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120450)  
[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.6.0
- Resa disponibile: 21 febbraio 2019

### <a name="whats-new-in-56"></a>Novità della versione 5.6

| Nuovo elemento | Dettagli |
| :------- | :------ |
| Supporto per PHP 7.3. | &nbsp; |
| Eliminazione del supporto per PHP 7.0. | &nbsp; |
| Supporto per il driver Microsoft ODBC 17.3 per tutte le piattaforme. | &nbsp; |
| Supporto per macOS Mojave. | Richiede il driver ODBC 17.3 o versione successiva. |
| Supporto per Ubuntu 18.10 e SUSE Linux 15. | Entrambi richiedono il driver ODBC 17.3 o versione successiva. |
| Eliminazione del supporto per Linux Ubuntu 17.10 e macOS El Capitan. | &nbsp; |
| Supporto per il token di accesso di Azure AD. | In Linux e macOS richiede il driver ODBC 17.2 o versione successiva e unixODBC 2.3.6 o versione successiva. |
| Supporto per l'autenticazione con Azure AD tramite l'identità gestita per le risorse di Azure. | Richiede il driver ODBC 17.3 o versione successiva. |
| Nuove funzionalità di recupero | &bull; &nbsp; Nuovo flag PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE per la restituzione di datetime come oggetti da parte di pdo_sqlsrv.<br/><br/>&bull; &nbsp; Aggiunta dell'opzione ReturnDatesAsStrings a livello di istruzione per sqlsrv.<br/><br/>&bull; &nbsp; Nuove opzioni a livello di connessione e istruzione per entrambi i driver per la formattazione dei valori decimali nei risultati recuperati. |
| Supporto per la compilazione statica di driver se gli utenti scelgono di eseguire la compilazione dall'origine. | &nbsp; |
| Miglioramento delle prestazioni tramite la memorizzazione dei metadati nella cache durante le operazioni di recupero e accelerazione delle conversioni di stringhe Unicode. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="53"></a>5.3

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120447)  
[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.3.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.3.0
- Resa disponibile: 20 luglio 2018

### <a name="whats-new-in-53"></a>Novità della versione 5.3

- Supporto per il driver Microsoft ODBC 17.2 per tutte le piattaforme
- Supporto per macOS High Sierra (richiede il driver ODBC 17 e versioni successive)
- Supporto per Azure Key Vault per Always Encrypted per le funzionalità CRUD di base. Rende disponibile la funzionalità Always Encrypted per tutte le piattaforme Windows, Linux o macOS supportate [tramite l'uso di Always Encrypted con i driver PHP per SQL Server](using-always-encrypted-php-drivers.md)
- Supporto per Ubuntu 18.04 LTS (richiede il driver ODBC 17.2)
- Supporto per Resilienza connessione anche in Linux o macOS (richiede il driver ODBC 17.2)

## <a name="52"></a>5,2

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120451)  
[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v5.2.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 5.2.0
- Resa disponibile: 23 marzo 2018

### <a name="whats-new-in-52"></a>Novità della versione 5.2

- Supporto per PHP 7.2.1 e versioni successive in Windows e per PHP 7.2.0 e versioni successive in altre piattaforme
- Supporto per il driver Microsoft ODBC 17
  - La versione 17 è ora l'impostazione predefinita in tutte le piattaforme
- Supporto per Ubuntu 17.10, Debian 9 e SUSE Enterprise Linux 12
- Eliminazione del supporto per Ubuntu 15.10
- Supporto per Always Encrypted con funzionalità CRUD in Windows. Per altre informazioni, vedere [Uso di Always Encrypted con i driver PHP per SQL Server](using-always-encrypted-php-drivers.md).
  - Supporto per l'archivio certificati di Windows
  - Always Encrypted è supportato solo con il driver Microsoft ODBC 17 e versioni successive
- Supporto per le impostazioni locali non UTF-8 in Linux e macOS
  - Le impostazioni locali non UTF-8 in Linux e macOS sono supportate solo con il driver Microsoft ODBC 17 e versioni successive
- Supporto per Azure Synapse Analytics
- Supporto per Istanza gestita di database SQL di Azure

## <a name="43"></a>4.3

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120616)  
[Tag di versione di GitHub (i pacchetti Linux e macOS sono disponibili qui)](https://github.com/Microsoft/msphpsql/releases/tag/v4.3.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 4.3.0
- Resa disponibile: 6 luglio 2017

### <a name="whats-new-in-43"></a>Novità della versione 4.3

- Supporto per PHP 7.1
- Supporto per macOS Sierra e macOS El Capitan
- Supporto per Ubuntu 15.10 e Debian 8
- Eliminazione del supporto per Ubuntu 15.04
- Supporto per i gruppi di disponibilità Always On tramite risoluzione IP di rete trasparente. Per altre informazioni, vedere [Connection Options](connection-options.md).
- Aggiunta del supporto per il tipo di dati sql_variant con limitazioni.
- Supporto per Resilienza connessione inattiva in Windows. Per altre informazioni, vedere [Connection Options](connection-options.md).
- Supporto dei pool di connessioni per Linux e macOS. Per altre informazioni, vedere [Pool di connessioni](connection-pooling-microsoft-drivers-for-php-for-sql-server.md).
- Supporto per l'autenticazione di Azure Active Directory con ActiveDirectoryPassword e SqlPassword. Per altre informazioni, vedere [Connection Options](connection-options.md).

## <a name="40"></a>4.0

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120448)  
[Tag di versione GitHub](https://github.com/microsoft/msphpsql/releases/tag/v4.0-RTW)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 4.0
- Resa disponibile: 1 luglio 2016

### <a name="whats-new-in-40"></a>Novità della versione 4.0

- Supporto per PHP 7.0  
- Supporto a 64 bit completo
- Supporto per Ubuntu 15,04, Ubuntu 16,04 e Red Hat 7

## <a name="32"></a>3.2

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2120449)  
[Tag di versione GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.2.0.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 3.2
- Resa disponibile: 9 marzo 2015

### <a name="whats-new-in-32"></a>Novità della versione 3.2

- Supporto per PHP 5.6  
- Include gli aggiornamenti più recenti per le versioni precedenti di PHP, la 5.5 e la 5.4  
- Richiede Microsoft ODBC Driver 11 per SQL Server  

## <a name="31"></a>3.1

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2143027)  
[Tag di versione GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.1.0.0)

### <a name="version-information"></a>Informazioni sulla versione

- Numero di versione: 3.1
- Resa disponibile: 12 dicembre 2014

### <a name="whats-new-in-31"></a>Novità della versione 3.1

- Supporto per PHP 5.5  
- Richiede Microsoft ODBC Driver 11 per SQL Server. Le versioni precedenti richiedono SQL Native Client.  

## <a name="30"></a>3,0

![scaricato](../../ssms/media/download-icon.png) [Download del pacchetto Windows](https://go.microsoft.com/fwlink/?linkid=2143026)  

### <a name="whats-new-in-30"></a>Novità della versione 3.0  

- Supporto per PHP 5.4  PHP 5.2 non è supportato nella versione 3 di [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
- È stata aggiunta l'opzione di connessione AttachDBFileName. Per altre informazioni, vedere [Connection Options](connection-options.md).  
- Supporto per Local DB, aggiunto in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Per altre informazioni, vedere [Supporto per LocalDB](php-driver-for-sql-server-support-for-localdb.md).
- È stata aggiunta l'opzione di connessione AttachDBFileName. Per altre informazioni, vedere [Connection Options](connection-options.md).  
- Supporto per le funzionalità di ripristino di emergenza a disponibilità elevata. Per altre informazioni, vedere [Supporto per disponibilità elevata e ripristino di emergenza](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).
- Supporto per i cursori sul lato client (memorizzazione nella cache di un set di risultati in memoria). Per altre informazioni, vedere [Tipi di cursore &#40;driverSQLSRV&#41;](cursor-types-sqlsrv-driver.md) e [Tipi di cursore &#40;driver PDO_SQLSRV&#41;](cursor-types-pdo-sqlsrv-driver.md).
- È stato aggiunto l'attributo PDO::ATTR_EMULATE_PREPARES. Per altre informazioni, vedere [PDO::prepare](pdo-prepare.md).  

## <a name="20"></a>2.0

### <a name="whats-new-in-20"></a>Novità della versione 2.0

Nella versione 2.0 è stato aggiunto il supporto per il driver PDO_SQLSRV. Per altre informazioni, vedere [Guida di riferimento del driver PDO_SQLSRV](pdo-sqlsrv-driver-reference.md).  

## <a name="see-also"></a>Vedere anche

[Panoramica dei driver Microsoft per PHP per SQL Server](overview-of-the-php-sql-driver.md)
