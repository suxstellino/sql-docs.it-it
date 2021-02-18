---
description: Supplemento alla privacy di SQL Server
title: Supplemento alla privacy di SQL Server | Microsoft Docs
ms.date: 11/11/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: wopeter
ms.custom: ''
ms.topic: conceptual
f1_keywords: ''
helpviewer_keywords: ''
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1821ebdd7c52d306a34be98f6e506de24c31539e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341669"
---
# <a name="sql-server-privacy-supplement"></a>Supplemento alla privacy di SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Questo articolo riepiloga le funzionalità abilitate per Internet che consentono di raccogliere e inviare a Microsoft dati anonimi di diagnostica e di utilizzo delle funzionalità. SQL Server può raccogliere informazioni standard sul computer e dati relativi all'uso e alle prestazioni che possono venire trasmessi a Microsoft e analizzati al fine di migliorare la qualità, la sicurezza e l'affidabilità del prodotto. Se si installa SQL Server in una macchina virtuale nel servizio Microsoft Azure, potrebbero essere inviate a Microsoft informazioni sull'ambiente in modo che Microsoft possa installare l'estensione Agente IaaS di SQL Server nella macchina virtuale e registrare la risorsa macchina virtuale SQL con il provider di risorse macchina virtuale SQL, come descritto [qui](/azure/azure-sql/virtual-machines/windows/sql-vm-resource-provider-register).

Questo articolo funge da supplemento all'[Informativa sulla privacy di Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839). La classificazione dei dati in questo articolo si applica solo alle versioni del prodotto SQL Server locale. Non si applica agli elementi seguenti:

- Database SQL di Azure
- [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-telemetry-ssms.md)
- SQL Server Data Tools (SSDT)
- Azure Data Studio
- Database Migration Assistant
- SQL Server Migration Assistant
- Estensione MS-SQL

Definizione di *scenari di utilizzo consentiti*. Per il contesto di questo articolo, Microsoft definisce "scenari di utilizzo consentiti" le azioni o le attività avviate da Microsoft.

## <a name="access-control"></a>Controllo di accesso

Informazioni correlate alle credenziali usate per proteggere gli account di accesso, gli utenti o gli account all'interno di un'installazione di SQL Server.

### <a name="examples-of-access-control"></a>Esempi di controllo di accesso

- Password
- Certificati

### <a name="permitted-usage-scenarios"></a>Scenari di utilizzo consentiti

|Scenario |Restrizioni di accesso |Requisiti di mantenimento |
|---------|---------|---------|
|Queste credenziali non lasciano mai il computer dell'utente come dati di Utilizzo e diagnostica. |- |- |
|I dump di arresto anomalo del sistema possono contenere dati di controllo di accesso. |- |Dump di arresto anomalo del sistema: massimo 30 giorni. |
|Queste credenziali non lasciano mai il computer dell'utente mediante il feedback utente a meno che il cliente non le inserisca manualmente |Limite all'uso interno Microsoft senza accesso per terze parti. |Feedback utente: massimo 1 anno|
|&nbsp;|&nbsp;|&nbsp;|

## <a name="customer-data"></a>Dati del cliente

Si definiscono dati del cliente i dati archiviati nelle tabelle utente, in modo diretto o indiretto. I dati includono valori letterali o statistiche dell'utente all'interno di testi di query che possono essere archiviati nelle tabelle utente.

### <a name="examples-of-customer-data"></a>Esempi di dati del cliente

- Valori di dati archiviati nelle righe di una tabella utente.
- Oggetti di statistiche contenenti le copie di valori nelle righe di una tabella utente.
- Testi di query contenenti valori letterali.

### <a name="permitted-usage-scenarios"></a>Scenari di utilizzo consentiti

|Scenario  |Restrizioni di accesso  |Requisiti di mantenimento |
|---------|---------|---------|
|Questi dati non lasciano il computer dell'utente come dati di Utilizzo e diagnostica. |- |- |
|I dump di arresto anomalo del sistema possono includere dati del cliente ed essere inviati a Microsoft. |- |Dump di arresto anomalo: massimo 30 giorni. |
|I clienti possono dare il proprio consenso all'invio a Microsoft di feedback utente in cui sono inclusi i loro dati. |Limite all'uso interno Microsoft senza accesso per terze parti. Microsoft può rendere disponibili i dati al cliente originale. |Feedback utente: massimo 1 anno |

## <a name="personal-data"></a>Dati personali

Dati ricevuti da un utente o generati dall'uso del prodotto.
- Collegabili a un singolo utente.
- Non sono inclusi dati del cliente.

### <a name="examples-of-personal-data"></a>Esempi di dati personali

- Identificazione dell'interfaccia. Indirizzo IP completo
- Nome del computer
- Credenziali di accesso/Nomi utente
- Parte personalizzata dell'indirizzo di posta elettronica (joe@contoso.com)
- Informazioni sulla posizione
- Identificazione del cliente

### <a name="permitted-usage-scenarios"></a>Scenari di utilizzo consentiti

|Scenario  |Restrizioni di accesso  |Requisiti di mantenimento|
|---------|---------|---------|
|Questi dati non lasciano il computer dell'utente come dati di Utilizzo e diagnostica. |- |- |
|I dump di arresto anomalo del sistema possono includere dati personali ed essere inviati a Microsoft. |- |Dump di arresto anomalo: massimo 30 giorni |
|L'ID di identificazione del cliente può essere inviato a Microsoft per offrire nuove funzionalità ibride e cloud che gli utenti hanno sottoscritto. |- |Attualmente non esiste alcuna funzionalità ibrida o cloud di questo tipo.|
|I clienti possono dare il proprio consenso all'invio a Microsoft di feedback utente in cui sono inclusi i loro dati.|Limite all'uso interno Microsoft senza accesso per terze parti. Microsoft può rendere disponibili i dati al cliente originale. |Feedback utente: massimo 1 anno |

## <a name="internet-based-services-data"></a>Dati dei servizi basati su Internet

Dati necessari per offrire servizi basati su Internet, in base al contratto di licenza con l'utente finale di SQL Server.

### <a name="examples-of-internet-based-services-data"></a>Esempi di dati dei servizi basati su Internet

- Informazioni sulle specifiche del computer
- Nome/versione del browser
- Versione di SQL Server
- Codice lingua
- Un indirizzo IP con determinati ottetti rimossi
- Dati di mapping

### <a name="permitted-usage-scenarios"></a>Scenari di utilizzo consentiti

|Scenario  |Restrizioni di accesso  |Requisiti di mantenimento|
|---------|---------|---------| 
|Possono essere usati da Microsoft per migliorare le funzionalità e/o eliminare bug nelle funzionalità correnti. |Limite all'uso interno Microsoft senza accesso per terze parti. Microsoft può rendere disponibili i dati al cliente originale.  Ad esempio, i dashboard |Minimo 90 giorni - Massimo 3 anni |
|I clienti possono dare il proprio consenso all'invio a Microsoft di feedback utente in cui sono inclusi i loro dati. |Limite all'uso interno Microsoft senza accesso per terze parti. |I clienti possono dare il proprio consenso all'invio a Microsoft di feedback utente in cui sono inclusi i loro dati. |
|Gli elementi mappa di Power View e di SQL Server Reporting Services possono inviare dati per l'uso delle mappe di Bing. |Limite ai dati della sessione |- |

## <a name="non-personal-data"></a>Dati non personali

1. Dati ricevuti da un'organizzazione o generati dall'uso del prodotto all'interno dell'organizzazione. Sono collegabili a un'organizzazione e non contengono dati del cliente.

   - Esempio
     - Nome dell'organizzazione (ad esempio: Microsoft Corp.)

   - Scenari di utilizzo consentiti

     |Scenario  |Restrizioni di accesso  |Requisiti di mantenimento|
     |---------|---------|---------|
     | Microsoft può raccogliere dati di utilizzo generici delle istanze di SQL Server in esecuzione in Macchine virtuali di Azure allo scopo di offrire ai clienti vantaggi facoltativi in Azure per l'uso di SQL Server in Macchine virtuali di Azure. | Microsoft può esporre i dati al cliente, ad esempio tramite il portale di Azure, per consentire ai clienti che eseguono SQL Server in Macchine virtuali di Azure di accedere ai vantaggi specifici dell'esecuzione di SQL Server in Azure. </br></br>Microsoft non userà questi dati per i controlli delle licenze senza il consenso anticipato del cliente. | Minimo 90 giorni - Massimo 3 anni |

2. Dati che descrivono o vengono usati per configurare server, database, tabelle e altre risorse create o fornite dai clienti. Includono i nomi delle tabelle e delle colonne del database ma non i contenuti delle righe del database o altri dati del cliente. I clienti non devono inserire dati personali in questi campi né creare applicazioni progettate per archiviarvi dati personali. Per gli scenari di utilizzo consentiti riportati di seguito, viene usato solo il formato hash per determinare i modelli di utilizzo per migliorare il prodotto.

   - Esempio
      - Nomi di database di SQL Server
      - Nomi di tabelle e colonne
      - Nomi di statistiche

    - Scenari di utilizzo consentiti

      > [!NOTE]
      > Prima della raccolta viene eseguito l'hashing di tutti i valori dei metadati.
      >

      |Scenario  |Restrizioni di accesso  |Requisiti di mantenimento|
      |---------|---------|---------|
      |Possono essere usati da Microsoft per migliorare le funzionalità e/o eliminare bug nelle funzionalità correnti. |Limite all'uso interno Microsoft senza accesso per terze parti. |Minimo 90 giorni - Massimo 3 anni|

3. Dati generati nel corso dell'esecuzione del server. Non contengono dati del cliente, dati non personali (come indicato nelle sezioni 1. e 2. precedenti), dati di controllo di accesso del cliente o dati personali.

   - Esempio
     - GUID del database
     - Hash del nome del computer
     - Hash del nome dell'istanza
     - Nome applicazione
     - Dati sul comportamento o l'utilizzo
     - Dati relativi al programma Analisi utilizzo software di SQL
     - Dati di configurazione del server, ad esempio le impostazioni di sp_configure
     - Dati di configurazione delle funzionalità
     - Nomi degli eventi e codici di errore
     - Impostazioni hardware e identificazione, ad esempio il produttore OEM

   Microsoft esamina i valori dei nomi di applicazione impostati da altri programmi che usano SQL Server (ad esempio SharePoint o programmi di terze parti includono queste informazioni nei campi di metadati inviati a Microsoft quando sono abilitati i dati di utilizzo). I clienti non devono inserire dati personali in questi campi di metadati né creare applicazioni progettate per archiviarvi dati personali.

   - Scenari di utilizzo consentiti

     |Scenario  |Restrizioni di accesso  |Requisiti di mantenimento|
     |---------|---------|---------|
     |Possono essere usati da Microsoft per migliorare le funzionalità e/o eliminare bug nelle funzionalità correnti.|Limite all'uso interno Microsoft senza accesso per terze parti. |Minimo 90 giorni - Massimo 3 anni |
     |Possono essere usati per offrire suggerimenti al cliente.  Ad esempio, si può consigliare, in base all'utilizzo del prodotto, di valutare l'uso della funzionalità *X* poiché consentirebbe prestazioni superiori. |Microsoft può rendere disponibili i dati al cliente originale, ad esempio mediante i dashboard. |Registri di protezione dei dati del cliente: minimo 3 anni - massimo 6 anni |
     |Possono essere usati da Microsoft per la pianificazione futura dei prodotti. |Microsoft può condividere queste informazioni con altri fornitori di hardware e software per migliorare il funzionamento dei loro prodotti con il software Microsoft. |Minimo 90 giorni - Massimo 3 anni|
     |Possono essere usati da Microsoft per offrire servizi basati su cloud in base ai dati di Utilizzo e diagnostica generati. Ad esempio, un dashboard cliente che illustra l'utilizzo di funzionalità in tutte le installazioni di SQL Server all'interno di un'organizzazione. |Microsoft può rendere disponibili i dati al cliente originale, ad esempio mediante i dashboard. |Minimo 90 giorni - Massimo 3 anni |
     |I clienti possono dare il proprio consenso all'invio a Microsoft di feedback utente in cui sono inclusi i loro dati. |Limite all'uso interno Microsoft senza accesso per terze parti. Microsoft può rendere disponibili i dati al cliente originale. |Feedback utente: massimo 1 anno |
     |Il nome del database e il nome dell'applicazione possono essere usati per classificare i database e le applicazioni in categorie note, ad esempio quelli che potrebbero eseguire software fornito da Microsoft o da altre società.|Limite all'uso interno Microsoft senza accesso per terze parti.|Minimo 90 giorni - Massimo 3 anni |

## <a name="system-generated-logs-controls"></a>Controlli dei log generati dal sistema

Per informazioni sul modo in cui è possibile attivare o disattivare i log generati dal sistema nel prodotto, vedere [Configurare la raccolta di dati di diagnostica e utilizzo per SQL Server (Analisi utilizzo software)](usage-and-diagnostic-data-configuration-for-sql-server.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
