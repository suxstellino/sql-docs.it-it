---
title: Note sulla versione per OLE DB Driver
description: Questo articolo sulle note sulla versione descrive le modifiche in ogni versione di Microsoft OLE DB Driver per SQL Server.
ms.date: 12/01/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: genemi
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dfc66e7c1f1aa787ccc366c7dc50613741724a1f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837454"
---
# <a name="release-notes-for-the-microsoft-ole-db-driver-for-sql-server"></a>Note sulla versione per il driver Microsoft OLE DB per SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa pagina elenca le nuove funzionalità aggiunte o modificate in ogni versione di Microsoft OLE DB Driver for SQL Server.

<!--
USE THE TABLE FORMAT!
Hello, from now on, please use the table-based format standard for all new Release Notes content.
See section "## 18.2.1" for a live example in this article.
Thank you. For questions, contact GeneMi. (2019/03/16)
-->

## <a name="1850"></a>18.5.0
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2135577)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2135722)  

Resa disponibile: 1 dicembre 2020

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
    Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40a)  
    Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40a)  

### <a name="features-added"></a>Funzionalità aggiunte

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per [individuazione e classificazione dei dati SQL](../../relational-databases/security/sql-data-discovery-and-classification.md) | [Uso della classificazione dei dati](features/using-data-classification.md) |
| Supporto per l'autenticazione con entità servizio di Azure Active Directory (`ActiveDirectoryServicePrincipal`) | [Uso di Azure Active Directory](features/using-azure-active-directory.md) |

### <a name="bugs-fixed"></a>Bug risolti

| Bug risolto | Dettagli |
| :-------- | :------ |
| Risoluzione di un problema con caratteri null incorporati. | Correzione di un bug per cui il driver restituisce una lunghezza non corretta delle stringhe con caratteri null incorporati. |
| Correzione di una perdita di memoria nell'interfaccia [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md). | Correzione di una perdita di memoria nell'interfaccia [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md) che interessa le operazioni di copia bulk del tipo di dati `sql_variant`. |
| Correzione dei bug che causano la restituzione di valori non corretti per le proprietà `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD` e `SSPROP_MUTUALLYAUTHENTICATED`. | Le versioni precedenti del driver restituiscono valori troncati della proprietà `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD`. Inoltre, nel caso dell'autenticazione `ActiveDirectoryIntegrated` il valore restituito della proprietà `SSPROP_MUTUALLYAUTHENTICATED` è `VARIANT_FALSE` anche quando entrambi i lati vengono reciprocamente autenticati.|
| Correzione di un bug di inserimento della tabella remota del server collegato. | Correzione di un bug che causa l'esito negativo dell'inserimento della tabella remota del server collegato se l'[opzione di configurazione del server NOCOUNT](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md) è stata abilitata. |

## <a name="previous-releases"></a>Versioni precedenti

## <a name="1840"></a>18.4.0
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2129954)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2131003)  

Resa disponibile: Maggio 2020

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40a)  

### <a name="features-added"></a>Funzionalità aggiunte

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per la risoluzione dell'IP di rete trasparente |[Risoluzione dell'IP di rete trasparente](features/using-transparent-network-ip-resolution.md)|
| Supporto per la codifica client UTF-8 | [Supporto UTF-8 in OLE DB Driver for SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |

### <a name="bugs-fixed"></a>Bug risolti

| Bug risolto | Dettagli |
| :-------- | :------ |
| Correzione di diversi bug nell'interfaccia [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) | Alcuni bug che interessano le tabelle codici multibyte fanno sì che l'interfaccia indichi prematuramente la fine del flusso durante l'operazione di lettura.|
| Correzione di una perdita di memoria nell'interfaccia [IOpenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) | Correzione di una perdita di memoria nell'interfaccia [IOpenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) quando è abilitata la proprietà `SSPROP_IRowsetFastLoad`. |
| Correzione di un bug in scenari che includono un tipo di dati `sql_variant` e stringhe non ASCII. | L'esecuzione di determinati scenari che includono un tipo di dati `sql_variant` e stringhe non ASCII può causare il danneggiamento dei dati. Per informazioni dettagliate, vedere: [Problemi noti](ole-db-data-types/ssvariant-structure.md#known-issues). |
| Correzione di problemi relativi al pulsante *Test connessione* nella [finestra di dialogo di configurazione di UDL](help-topics/data-link-pages.md). | Il pulsante *Test connessione* nella [finestra di dialogo di configurazione di UDL](help-topics/data-link-pages.md) applica ora le proprietà di inizializzazione impostate nella scheda *Tutte*. |
| Correzione della gestione del valore predefinito della proprietà `SSPROP_INIT_PACKETSIZE`. | Correzione di un errore imprevisto quando la proprietà `SSPROP_INIT_PACKETSIZE` è impostata sul valore predefinito `0`. Per informazioni su questa proprietà, vedere [Proprietà di inizializzazione e autorizzazione](ole-db-data-source-objects/initialization-and-authorization-properties.md). |
| Correzione dei problemi di overflow del buffer in [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md). | Correzione dei problemi di overflow del buffer quando si usano file di dati in formato non valido. |
| Correzione dei problemi di accessibilità. | Correzione dei problemi di accessibilità nell'interfaccia utente del programma di installazione e nella [finestra di dialogo di accesso a SQL Server](help-topics/sql-server-login-dialog.md) (lettura di contenuto, tabulazioni). |

## <a name="1830"></a>18.3.0

![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2117515)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2117517)  

Resa disponibile: Ottobre 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40a)  

### <a name="features-added"></a>Funzionalità aggiunte

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto dell'autenticazione di Azure Active Directory (`ActiveDirectoryInteractive`, `ActiveDirectoryMSI`) | [Uso di Azure Active Directory](features/using-azure-active-directory.md) |
| Includere Azure Active Directory Authentication Library (adal.dll) nel programma di installazione | Ora incluso nell'installazione dei driver di base, il programma di installazione di OLE DB aggiornerà le installazioni esistenti di Microsoft Active Directory Authentication Library per SQL Server, rimuovendo l'applicazione dall'elenco delle applicazioni installate in Windows. |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed"></a>Bug risolti

| Bug risolto | Dettagli |
| :-------- | :------ |
| Correzione della logica di eliminazione degli indici in [IIndexDefinition::DropIndex](/previous-versions/windows/desktop/ms722733(v=vs.85)). | Le versioni precedenti del driver OLE DB non possono eliminare un indice di chiave primaria se l'ID schema e l'ID utente del proprietario dell'indice non sono uguali. |
| &nbsp; | &nbsp; |

Scaricare le versioni precedenti del driver OLE DB facendo clic sui collegamenti di download nelle sezioni seguenti:

## <a name="1823"></a>18.2.3

![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2119554)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2119738)  

Resa disponibile: Giugno 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40a)  

### <a name="features-added-in-1823"></a>Funzionalità aggiunte nella versione 18.2.3

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per gli aggiornamenti del driver dai supporti rimovibili di SQL Server | Questo miglioramento consente gli aggiornamenti dei driver direttamente dai supporti rimovibili di SQL Server. |
| &nbsp; | &nbsp; |

## <a name="1822"></a>18.2.2

![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2118512)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2118415)  

Resa disponibile: Maggio 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40a)  

### <a name="bugs-fixed-in-1822"></a>Bug risolti nella versione 18.2.2

| Bug risolto | Dettagli |
| :-------- | :------ |
| Correzione dell'autenticazione non interattiva di Azure Active Directory nell'apartment a thread multipli (MTA). | OLE DB Driver 18.2.1 prova erroneamente a modificare il modello di concorrenza COM in un apartment che è stato inizializzato in precedenza con thread multipli (MTA). Di conseguenza, in un'applicazione che effettua più di una chiamata successiva a [CoInitialize](/windows/win32/api/objbase/nf-objbase-coinitialize) o a [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) prima di chiamare l'interfaccia [IDBInitialize::Initialize](/previous-versions/windows/desktop/ms718026(v=vs.85)), il driver non riesce a connettersi quando si usa una qualsiasi modalità di autenticazione di Azure Active Directory. |
| &nbsp; | &nbsp; |

## <a name="1821"></a>18.2.1

![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2118511)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2118278)  

Resa disponibile: Febbraio 2019

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40a)  

### <a name="features-added-in-1821"></a>Funzionalità aggiunte nella versione 18.2.1

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per la codifica server UTF-8 | [Supporto UTF-8 in OLE DB Driver for SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |
| Supporto dell'autenticazione di Azure Active Directory | [Uso di Azure Active Directory](features/using-azure-active-directory.md) |
| &nbsp; | &nbsp; |

## <a name="1810"></a>18.1.0

![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2118506)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2118509)  

Resa disponibile: Luglio 2018

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118509&2118509=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40a)  

### <a name="features-added-in-1810"></a>Funzionalità aggiunte nella versione 18.1.0

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per la parola chiave della stringa di connessione `UseFMTONLY` e per la proprietà di inizializzazione `SSPROP_INIT_USEFMTONLY` | `UseFMTONLY` controlla la modalità di recupero dei metadati durante la connessione a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br/><br/>Per altre informazioni, vedere: [Uso delle parole chiave delle stringhe di connessione con OLE DB Driver for SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed-in-1810"></a>Bug risolti nella versione 18.1.0

| Bug risolto | Dettagli |
| :-------- | :------ |
| Correzione della versione errata del file di formato BCP. | OLE DB Driver 18.0 imposta in modo errato la versione del file di formato BCP su 18.0, anziché su 11.0.<br/>OLE DB Driver 18.1 non è in grado di leggere file di formato generati da OLE DB Driver 18.0.<br/>Se è necessario usare con il nuovo driver file di formato generati con la versione precedente, è possibile modificare manualmente i file in modo da impostare la versione su 11.0. |
| &nbsp; | &nbsp; |

## <a name="1802"></a>18.0.2

![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x64](https://go.microsoft.com/fwlink/?linkid=2118504)  
![download](../../ssms/media/download-icon.png) [Scaricare il programma di installazione x86](https://go.microsoft.com/fwlink/?linkid=2118277)  

Resa disponibile: Marzo 2018

Se è necessario scaricare il programma di installazione in una lingua diversa da quella rilevata, è possibile usare questi collegamenti diretti.  
Per il driver x64: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40a)  
Per il driver x86: [Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40a)  

### <a name="features-added-in-1802"></a>Funzionalità aggiunte nella versione 18.0.2

| Funzionalità aggiunta | Dettagli |
| :------------ | :------ |
| Supporto per la parola chiave della stringa di connessione `MultiSubnetFailover` e la proprietà di inizializzazione `SSPROP_INIT_MULTISUBNETFAILOVER`. | Per altre informazioni, vedere:<br/>&bull; &nbsp; [Supporto di OLE DB Driver per SQL Server per il ripristino di emergenza a disponibilità elevata](features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).<br/>&bull; &nbsp; [Uso delle parole chiave delle stringhe di connessione con OLE DB Driver for SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Vedere anche

[Driver Microsoft OLE DB per SQL Server](oledb-driver-for-sql-server.md)
