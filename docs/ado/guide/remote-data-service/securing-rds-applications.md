---
description: Sicurezza delle applicazioni RDS
title: Protezione delle applicazioni RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS security [ADO]
ms.assetid: 82fb1330-d6c6-4c17-ad3e-d417ff822b25
author: rothja
ms.author: jroth
ms.openlocfilehash: d91b2ef0d4efba7453e3acedb12c8f55d528b1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031803"
---
# <a name="securing-rds-applications"></a>Sicurezza delle applicazioni RDS
In questo argomento vengono fornite informazioni sulla sicurezza per Servizi Desktop remoto.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="microsoft-internet-explorer-security-issues"></a>Problemi di sicurezza di Microsoft Internet Explorer  
 Con i nuovi miglioramenti della sicurezza aggiunti a Microsoft Internet Explorer, alcuni oggetti ADO e RDS sono limitati all'esecuzione solo in un ambiente in modalità "Safe". A questo scopo, è necessario essere a conoscenza di questi problemi, tra cui zone diverse, livelli di sicurezza, comportamento restrittivo, operazioni non sicure e impostazioni di sicurezza personalizzate.  
  
## <a name="security-and-your-web-server"></a>Sicurezza e server Web  
 Se si usa l'oggetto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) sul server Web Internet, tenere presente che questa operazione crea un potenziale rischio per la sicurezza. Gli utenti esterni che ottengono informazioni valide sul nome dell'origine dati (DSN), sull'ID utente e sulla password possono scrivere pagine per inviare qualsiasi query a tale origine dati. Se si desidera un accesso più limitato a un'origine dati, un'opzione consiste nell'annullare la registrazione ed eliminare l'oggetto **RDSServer. DataFactory** (msadcf.dll) e utilizzare invece oggetti business personalizzati con query hardcoded.  
  
 Per ulteriori informazioni sulle implicazioni di sicurezza relative all'utilizzo dell'oggetto RDSServer. DataFactory, vedere Microsoft Security Bulletin MS99-025 sul sito Web Microsoft Security.  
  
## <a name="client-impersonation-and-security"></a>Rappresentazione e sicurezza del client  
 Se la proprietà di **autenticazione della password** per il server Web IIS è impostata su autenticazione di richiesta/risposta di Windows NT (per windows NT 4,0) o sull'autenticazione integrata di Windows (per Windows 2000), gli oggetti business vengono richiamati nel contesto di sicurezza del client. Si tratta di una nuova funzionalità di RDS 1,5 che consente la rappresentazione client su HTTP. Quando si utilizza questa modalità, l'accesso al server Web (IIS) non è anonimo, ma utilizza l'ID utente e la password con cui è in esecuzione il computer client. Se i DSN ODBC sono configurati per l'utilizzo di una connessione trusted, l'accesso ai database, ad esempio SQL Server, avviene anche nel contesto di sicurezza del client. Ma funziona solo se il database si trova nello stesso computer di IIS. le credenziali del client non possono essere trasferite a un altro computer.  
  
 Ad esempio, un client, John Doe, con userid = "johnd" e password = "Secret" è connesso a un computer client. Esegue un'applicazione basata su browser che deve accedere all'oggetto **RDSServer. DataFactory** per creare un [Recordset](../../reference/ado-api/recordset-object-ado.md) eseguendo una query SQL sul computer "MyServer" che esegue IIS. MyServer, un sistema che esegue Windows NT Server 4,0, è configurato per l'utilizzo dell'autenticazione di richiesta/risposta di Windows NT, il DSN ODBC è selezionato "utilizza connessione trusted" e il server contiene anche l'origine dati SQL Server. Quando una richiesta viene ricevuta sul server Web, chiede al client l'ID utente e la password. Quindi, la richiesta viene registrata in MyServer come proveniente da "johnd"/"Secret" invece di IUSER_MyServer (che è l'impostazione predefinita quando l'autenticazione con password anonima è on). Analogamente, quando si accede a SQL Server, viene usato "johnd"/"Secret".  
  
 Di conseguenza, la modalità di autenticazione Challenge/Response di IIS Windows NT consente di creare pagine HTML senza che all'utente vengano richieste esplicitamente le informazioni relative all'ID utente e alla password necessarie per accedere al database. Se è stata utilizzata l'autenticazione di base di IIS, è necessario eseguire anche questa operazione.  
  
## <a name="password-authentication"></a>Autenticazione della password  
 Servizi Desktop remoto è in grado di comunicare con un server Web IIS in esecuzione in una delle tre modalità di autenticazione della password, ovvero l'autenticazione di risposta/richiesta anonima, di base o NT (denominata autenticazione integrata di Windows in Windows 2000). Queste impostazioni definiscono il modo in cui un server Web controlla l'accesso, ad esempio richiedendo che un computer client disponga di privilegi di accesso espliciti sul server Web NT.