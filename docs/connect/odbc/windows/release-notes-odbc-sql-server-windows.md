---
title: Note sulla versione per ODBC Driver for SQL Server in Windows
description: Questo articolo di note sulla versione descrive le modifiche in ogni versione del driver Microsoft ODBC per SQL Server in Windows.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: b8459ed8-625e-4d8b-891c-e7e78c9977cc
ms.reviewer: v-daenge
author: v-chojas
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: 23af1879610f55cfccb7c6e8a9f8071752a2a35c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837298"
---
# <a name="release-notes-for-microsoft-odbc-driver-for-sql-server-on-windows"></a>Note sulla versione per Microsoft ODBC Driver for SQL Server in Windows

Questo articolo relativo alle note sulla versione descrive le novità del driver Microsoft ODBC per SQL Server in Windows.

<!--
PLEASE USE THE STANDARD 2-COLUMN TABLE FORMAT!

For all our Release Notes articles (What's New too?), we are standardizing on the 2-column format that you see here for version "## 17.3".

Going forward, all new additions to this article must use the 2-column format.

Also, use the shorter ## H2 title format, which eliminates all the redundant constants, and appends the date-added.
One beneift of shortness is the avoidance of the annoying wrapping of unnecessarily long H2 titles in the rightNav.
- OLD H2:  ## What's New in the [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] on Windows
- NEW H2:  ## 17.3, February 2019

By the way, in GitHub, the file name is changing today 2019/03/30:
- FROM:  docs/connect/odbc/windows/release-notes.md
- TO  :  docs/connect/odbc/windows/release-notes-odbc-sql-server-windows.md

Thank you.
GeneMi (and CraigG).  2019/03/30.
-->

## <a name="177"></a>17.7

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2153471)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2153469)  

Numero di versione: 17.7.1.1  
Rilasciata: 29 gennaio 2021

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

| Funzionalità aggiunta | Dettagli |
| :------- | :------ |
| Autenticazione interattiva di Azure Key Vault | Vedere [Uso di Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Autenticazione di un'entità servizio | Vedere [Parole chiave e attributi per stringhe di connessione e DSN](../dsn-connection-string-attribute.md). |
| Possibilità di inserire in colonne Money e smallmoney crittografate | Vedere [Uso di Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Correzioni di bug. | [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="previous-releases"></a>Versioni precedenti

## <a name="176"></a>17.6

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2137027)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2137028)  

Numero di versione: 17.6.1.1  
Resa disponibile: 31 luglio 2020

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40a)

| Funzionalità aggiunta | Dettagli |
| :------- | :------ |
| Memorizzazione nella cache dei metadati per le istruzioni preparate | Vedere [Uso di Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Attributo di connessione SQL_COPT_SS_AUTOBEGINTXN per controllare l'esecuzione automatica di BEGIN TRANSACTION dopo ROLLBACK o COMMIT | Vedere [Parole chiave e attributi per stringhe di connessione e DSN](../dsn-connection-string-attribute.md). |
| Correzioni di bug. | [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1752"></a>17.5.2

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120137)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120140)  

Numero di versione: 17.5.2.1  
Resa disponibile: 6 marzo 2020

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40a)  

### <a name="features-added-in-1752"></a>Funzionalità aggiunte nella versione 17.5.2

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per l'autenticazione con l'identità gestita per Azure Key Vault | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Supporto per endpoint Azure Key Vault aggiuntivi | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

Scaricare le versioni precedenti di ODBC Driver facendo clic sui collegamenti di download nelle sezioni seguenti:

## <a name="175"></a>17.5

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120248)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120353)  

Numero di versione: 17.5.1.1  
Resa disponibile: 31 gennaio 2020

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40a)  

### <a name="features-added-in-175"></a>Funzionalità aggiunte nella versione 17.5

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Attributo di connessione SQL_COPT_SS_SPID per recuperare SPID senza round trip al server | Vedere [Parole chiave e attributi per stringhe di connessione e DSN](../dsn-connection-string-attribute.md). |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1742"></a>17.4.2

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120354)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120249)  

Numero di versione: 17.4.2.1  
Resa disponibile: Ottobre 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40a)  

### <a name="features-added-in-1742"></a>Funzionalità aggiunte nella versione 17.4.2

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per endpoint Azure Key Vault aggiuntivi | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Supporto per l'impostazione della versione di classificazione dei dati | Vedere [Classificazione dei dati](../data-classification.md#bkmk-version). |
| Includere Azure Active Directory Authentication Library (adal.dll) nel programma di installazione | Ora incluso nell'installazione dei driver di base, il programma di installazione di ODBC aggiornerà le installazioni esistenti di Microsoft Active Directory Authentication Library per SQL Server, rimuovendo l'applicazione dall'elenco delle applicazioni installate in Windows. |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="174"></a>17.4

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120149)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120150)  

Numero di versione: 17.4.1.1  
Resa disponibile: Luglio 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40a)  

### <a name="features-added-in-174"></a>Funzionalità aggiunte nella versione 17.4

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Always Encrypted con enclave sicuri. | Vedere [Uso di Always Encrypted con il driver ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Impostazioni keep-alive TCP configurabili. | Vedere [Connessione a SQL Server](../linux-mac/connection-string-keywords-and-data-source-names-dsns.md). |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="173"></a>17.3

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120355)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120356)  

Numero di versione: 17.3.1.1  
Resa disponibile: Febbraio 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40a)  

### <a name="features-added-in-173"></a>Funzionalità aggiunte nella versione 17.3

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Modalità di autenticazione dell'identità del servizio gestita di Azure Active Directory (assegnata dal sistema e dall'utente). | Vedere [Uso di Azure Active Directory con il driver ODBC](../using-azure-active-directory.md). |
| Capacità di trasmettere i parametri di input alle colonne Always Encrypted. | Vedere [Limitazioni del driver ODBC quando si usa Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted). |
| Transazioni distribuite XA. | [Uso delle transazioni XA](../use-xa-with-dtc.md). |
| Visual C++ Redistributable aggiornato | La dipendenza di runtime è stata aggiornata a Visual C++ 2017 Redistributable ([download x64](https://aka.ms/vs/15/release/vc_redist.x64.exe), [download x86](https://aka.ms/vs/15/release/vc_redist.x86.exe)) |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="172"></a>17.2

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120250)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120357)  

Numero di versione: 17.2.0.1  
Resa disponibile: Luglio 2018

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40a)  

### <a name="features-added-in-172"></a>Funzionalità aggiunte nella versione 17.2

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Classificazione dei dati per il database SQL di Microsoft Azure e SQL Server. | Vedere [Classificazione dei dati](../data-classification.md). |
| Supporto per la codifica server UTF-8. | &nbsp; |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="171"></a>17.1

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120151)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120443)  

Numero di versione: 17.1.0.1  
Resa disponibile: Marzo 2018

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40a)  

### <a name="features-added-in-171"></a>Funzionalità aggiunte nella versione 17.1

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto degli attributi di connessione `SQL_COPT_SS_CEKCACHETTL` e `SQL_COPT_SS_TRUSTEDCMKPATHS`. | &bull; &nbsp; `SQL_COPT_SS_CEKCACHETTL`<br/>Consente di controllare l'ora in cui è presente la cache locale delle chiavi di crittografia di colonna e di scaricarla.<br/><br/>&bull; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS`<br/>Consente all'applicazione di limitare le operazioni Always Encrypted in modo che usi solo l'elenco specificato di chiavi master della colonna.<br/><br/> Per altre informazioni, vedere [Uso di Always Encrypted con ODBC Driver for SQL Server](../using-always-encrypted-with-the-odbc-driver.md). |
| Supporto dell'autenticazione interattiva di Azure Active Directory | &nbsp; |
| Correzioni di bug. | Vedere [Correzioni di bug](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="170"></a>17.0

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2120444)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120152)  

Numero di versione: 17.0.1.1  
Resa disponibile: Febbraio 2018

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40a)  

### <a name="features-added-in-170"></a>Funzionalità aggiunte nella versione 17.0

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto di Always Encrypted per l'API BCP. | &nbsp; |
| Nuovo attributo della stringa di connessione `UseFMTOnly`. | Il driver userà i metadati legacy nei casi particolari in cui sono richieste tabelle temporanee. |
| Supporto di Istanza gestita di database SQL di Azure. | Vedere l'elenco seguente in [Differenze nell'uso di Istanza gestita (ODBC versione 17)](#diffs-managed-instance-17). |
| &nbsp; | &nbsp; |

| Dipendenza modificata | Dettagli |
| :------------ | :------ |
| Rimosso il servizio Assistente per l'accesso a Microsoft Online Services | La dipendenza è stata rimossa. |
| &nbsp; | &nbsp; |

### <a name="differences-when-using-managed-instance-odbc-version-17"></a><a name="diffs-managed-instance-17"></a> Differenze nell'uso di Istanza gestita (ODBC versione 17)

Questa versione di ODBC contiene il supporto per Istanza gestita del database SQL di Azure. Vedere l'elenco con le note seguente relativo alle differenze nell'uso di Istanza gestita.

> [!NOTE]
> Esistono molte differenze nell'uso di Istanza gestita:
>
> - FILESTREAM non è supportato.
> - L'accesso al file system locale non è supportato, ma è obbligatorio per elementi come i file di traccia.
> - La creazione di tipi definiti dall'utente dal percorso locale non è supportata.
> - L'autenticazione integrata di Windows non è supportata.
> - DTC non è supportato.
> - L'account `sa` non è presente. L'account predefinito è `cloudSA`.
> - L'errore del token TDS (0xAA) restituisce un nome server non corretto.
> - Non sono supportati i caratteri speciali nel nome del database.
> - ALTER DATABASE [dbname1] MODIFY NAME = [dbname2] non è supportato.
> - I messaggi di errore vengono sempre visualizzati in inglese, indipendentemente dalle impostazioni della lingua (come in Azure).

## <a name="131"></a>13.1

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2121020)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120923)  

Numero di versione: 13.1  

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40a)  

[Download di Microsoft Command Line Utilities 13.1 for SQL Server](https://www.microsoft.com/download/details.aspx?id=53591)

### <a name="features-added-in-131"></a>Funzionalità aggiunte nella versione 13.1

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aggiunge supporto per [Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) e [Azure Active Directory](../using-azure-active-directory.md). | Questi supporti aggiuntivi sono disponibili quando ci si connette a Microsoft SQL Server 2016 o a una versione successiva. |
| Esistono parole chiave e attributi del pool di connessioni che si riferiscono ai supporti per Always Encrypted e Azure Active Directory. | Le parole chiave e gli attributi sono descritti in [Pool di connessioni compatibile con il driver in ODBC Driver for SQL Server](driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="13"></a>13

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2121118)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2120924)  

Numero di versione: 13  

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40a)  

[Download di Microsoft Command Line Utilities 13 for SQL Server](https://www.microsoft.com/download/details.aspx?id=52680)

### <a name="features-added-in-13"></a>Funzionalità aggiunte nella versione 13

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Aggiunge supporto per Microsoft SQL Server 2016. | Mantiene la funzionalità del driver ODBC versione 11. |
| &nbsp; | &nbsp; |

## <a name="11"></a>11

![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2121206)  
![download](../../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2121021)  

Numero di versione: 11  

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40a)  

[Download di Microsoft Command Line Utilities 11 for SQL Server](https://www.microsoft.com/download/details.aspx?id=36433)  

### <a name="features-added-in-11"></a>Funzionalità aggiunte nella versione 11

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Contiene nuove funzionalità. | Vedere [Funzionalità di Microsoft ODBC Driver for SQL Server in Windows](features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md). |
| Contiene tutte le funzionalità disponibili con ODBC in SQL Server 2012 Native Client. | &nbsp; |
| &nbsp; | &nbsp; |
