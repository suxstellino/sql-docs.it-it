---
title: Microsoft ODBC Driver for SQL Server
description: Microsoft ODBC Driver for SQL Server abilita la connettività per SQL Server e il database SQL di Azure tramite API ODBC standard.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9f2ae91b-06af-4c9a-9d24-062df7bc4662
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b107155d9672b2a1d7b905a387e48373905dab55
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464192"
---
# <a name="microsoft-odbc-driver-for-sql-server"></a>Microsoft ODBC Driver for SQL Server

[!INCLUDE[ODBC_Current_Version](../../includes/odbc-latest-release.md)]

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

ODBC è l'API di accesso ai dati nativa principale per le applicazioni scritte in C e C++ per SQL Server. Un driver ODBC è disponibile per la maggior parte delle origini dati. Altri linguaggi che possono usare ODBC sono COBOL, Perl, PHP e Python. ODBC è ampiamente usato in scenari di integrazione dati.

Il driver ODBC include strumenti come [**sqlcmd**](../../tools/sqlcmd-utility.md) e [**bcp**](../../tools/bcp-utility.md). L'utilità **sqlcmd** consente di eseguire istruzioni Transact-SQL, procedure di sistema e script SQL. L'utilità **bcp** esegue operazioni di copia bulk di dati tra un'istanza di Microsoft SQL Server e un file di dati in un formato specificato dall'utente. L'utilità **bcp** può essere usata per importare un numero elevato di nuove righe in tabelle SQL Server oppure per esportare dati da tabelle in file di dati.  

## <a name="code-example-in-c"></a>Esempio di codice in C++

L'esempio di codice C++ illustra come usare le API ODBC per connettersi e accedere a un database:

- [Esempio di codice C++ che usa ODBC](../../odbc/reference/sample-odbc-program.md)

## <a name="download"></a>Download

- ![FrecciaGiù cerchiata](../../ssms/media/download-icon.png)[Download del driver ODBC](download-odbc-driver-for-sql-server.md)

## <a name="documentation"></a>Documentazione

### <a name="features"></a>Funzionalità

- [Provider di archivi chiavi personalizzati](../../connect/odbc/custom-keystore-providers.md)
- [Classificazione dati](../../connect/odbc/data-classification.md)
- [Parole chiave e attributi per stringhe di connessione e DSN](dsn-connection-string-attribute.md)
- [SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md) (le funzionalità disponibili sono applicabili anche a ODBC Driver for SQL Server senza OLEDB)
- [Uso di Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [Uso di Azure Active Directory](../../connect/odbc/using-azure-active-directory.md)
- [Uso della stringa di connessione TransparentNetworkIPResolution](../../connect/odbc/using-transparent-network-ip-resolution.md)
- [Uso delle transazioni XA](../../connect/odbc/use-xa-with-dtc.md)

### <a name="linux-and-macos"></a>Linux e macOS

- [Installazione del driver in Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Installazione del driver in macOS](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)
- [Connessione a SQL Server](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)
- [Connessione a **bcp**](../../connect/odbc/linux-mac/connecting-with-bcp.md)
- [Connessione a **sqlcmd**](../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)
- [Traccia di accesso ai dati](../../connect/odbc/linux-mac/data-access-tracing-with-the-odbc-driver-on-linux.md)
- [Domande frequenti](../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.yml)
- [Installazione di Gestione driver](../../connect/odbc/linux-mac/installing-the-driver-manager.md)
- [Problemi noti](../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)
- [Linee guida per la programmazione](../../connect/odbc/linux-mac/programming-guidelines.md)
- [Note sulla versione](../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)
- [Note sulla versione (mssql-tools)](../../connect/odbc/linux-mac/release-notes-tools.md)
- [Supporto per disponibilità elevata e ripristino di emergenza](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
- [Uso dell'autenticazione integrata (Kerberos)](../../connect/odbc/linux-mac/using-integrated-authentication.md)

### <a name="windows"></a>Windows

- [Esempio di esecuzione asincrona (metodo di notifica)](../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)
- [Resilienza di connessione nel driver ODBC di Windows](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
- [Pool di connessioni compatibile con il driver](../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)
- [Funzionalità e modifiche funzionali](../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)
- [Note sulla versione di ODBC per SQL Server in Windows](windows/release-notes-odbc-sql-server-windows.md)
- [Requisiti di sistema, installazione e file del driver](../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)

## <a name="community"></a>Community

- [Blog sui driver di SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [Forum di accesso ai dati di SQL Server](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
