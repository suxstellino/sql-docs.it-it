---
description: Informazioni dettagliate sul modello di programmazione RDS
title: Modello di programmazione RDS in dettaglio | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO], details
ms.assetid: 3e57af8d-519b-4467-a0bd-af468534cefd
author: rothja
ms.author: jroth
ms.openlocfilehash: 8421340798a308204a0961340e06216f3b0cb24a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031893"
---
# <a name="rds-programming-model-in-detail"></a>Informazioni dettagliate sul modello di programmazione RDS
Di seguito sono riportati gli elementi chiave del modello di programmazione RDS:  
  
-   RDS. DataSpace  
  
-   RDSServer. DataFactory  
  
-   RDS. DataControl  
  
-   Evento  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="rdsdataspace"></a>RDS. DataSpace  
 L'applicazione client deve specificare il server e il programma server da richiamare. In restituzione, l'applicazione riceve un riferimento al programma server e può considerare il riferimento come se fosse il programma server.  
  
 Il modello a oggetti di RDS incarna questa funzionalità con [RDS. Oggetto DataSpace](../../reference/rds-api/dataspace-object-rds.md) .  
  
 Il programma server è specificato con un identificatore del programma o un *ProgID*. Il server utilizza il *ProgID* e il registro di sistema del computer server per individuare informazioni sul programma effettivo da avviare.  
  
 Servizi Desktop remoto distingue internamente a seconda che il programma server si trovi in un server remoto in Internet o in una rete Intranet. un server in una rete locale; in alternativa, in un server, ma in una libreria di collegamento dinamico (DLL) locale. Questa distinzione determina la modalità di scambio delle informazioni tra il client e il server e rende una differenza tangibile nel tipo di riferimento restituito all'applicazione client. Tuttavia, dal punto di vista dell'utente, questa distinzione non ha alcun significato speciale. Ciò che conta è la ricezione di un riferimento a un programma utilizzabile.  
  
## <a name="rdsserverdatafactory"></a>RDSServer. DataFactory  
 Servizi Desktop remoto fornisce un programma server predefinito che può eseguire una query SQL sull'origine dati e restituire un oggetto [Recordset](../../reference/ado-api/recordset-object-ado.md) oppure utilizzare un oggetto **Recordset** e aggiornare l'origine dati.  
  
 Il modello a oggetti di RDS incarna questa funzionalità con l'oggetto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) .  
  
 Inoltre, questo oggetto dispone di un metodo per la creazione di un oggetto **Recordset** vuoto che è possibile compilare a livello di codice ([CreateRecordset](../../reference/rds-api/createrecordset-method-rds.md)) e un altro metodo per la conversione di un oggetto **Recordset** in una stringa di testo per compilare una pagina Web ([ConvertToString](../../reference/rds-api/converttostring-method-rds.md)).  
  
 Con ADO, è possibile eseguire l'override di alcuni dei comportamenti standard di connessione e comando di **RDSServer. DataFactory** con un gestore **DataFactory** e un file di personalizzazione che contiene i parametri di connessione, comando e sicurezza.  
  
 Il programma server viene talvolta definito *oggetto business*. È possibile scrivere un oggetto business personalizzato in grado di eseguire l'accesso ai dati complesso, i controlli di validità e così via. Anche quando si scrive un oggetto business personalizzato, è possibile creare un'istanza di un oggetto **RDSServer. DataFactory** e usare alcuni dei relativi metodi per eseguire le proprie attività.  
  
## <a name="rdsdatacontrol"></a>RDS. DataControl  
 RDS fornisce un mezzo per combinare le funzionalità di Servizi Desktop remoto **. DataSpace** e **RDSServer. DataFactory**, oltre a consentire ai controlli visivi di usare facilmente l'oggetto **Recordset** restituito da una query da un'origine dati. I tentativi di Servizi Desktop remoto, per la maggior parte dei casi, eseguono il maggior numero possibile di operazioni per ottenere automaticamente l'accesso alle informazioni su un server e visualizzarle in un controllo visivo.  
  
 Il modello a oggetti di RDS incarna questa funzionalità con [RDS. Oggetto DataControl](../../reference/rds-api/datacontrol-object-rds.md) .  
  
 **RDS. DataControl** presenta due aspetti. Un aspetto riguarda l'origine dati. Se si impostano le informazioni di comando e connessione usando le proprietà **Connect** e **SQL** di **RDS. DataControl**, utilizzerà automaticamente **RDS. DataSpace** per creare un riferimento all'oggetto **RDSServer. DataFactory** predefinito. **RDSServer. DataFactory** utilizzerà quindi il valore della proprietà **Connect** per connettersi all'origine dati, utilizzerà il valore della proprietà **SQL** per ottenere un **Recordset** dall'origine dati e restituirà l'oggetto **Recordset** a **RDS. DataControl**.  
  
 Il secondo aspetto riguarda la visualizzazione delle informazioni del **Recordset** restituite in un controllo visivo. È possibile associare un controllo visivo a **RDS. DataControl** (in un processo denominato binding) e ottenere l'accesso alle informazioni nell'oggetto **Recordset** associato, visualizzando i risultati della query in una pagina Web in Microsoft® Internet Explorer. Ogni Servizi Desktop remoto **. L'oggetto DataControl** associa un oggetto **Recordset** , che rappresenta i risultati di una singola query, a uno o più controlli visivi, ad esempio una casella di testo, una casella combinata, un controllo griglia e così via. Potrebbero essere presenti più servizi desktop remoto **. Oggetto DataControl** in ogni pagina. Ogni Servizi Desktop remoto **. L'oggetto DataControl** può essere connesso a un'origine dati diversa e contenere i risultati di una query separata.  
  
 **RDS. L'oggetto DataControl** dispone inoltre di metodi propri per lo spostamento, l'ordinamento e il filtro delle righe dell'oggetto **Recordset** associato. Questi metodi sono simili, ma non uguali ai metodi dell'oggetto **Recordset** ADO.  
  
## <a name="events"></a>Eventi  
 RDS supporta due dei propri eventi, indipendenti dal modello di eventi ADO. L'evento [onReadyStateChange](../../reference/rds-api/onreadystatechange-event-rds.md) viene chiamato ogni volta che **RDS. DataControl** [ReadyState](../../reference/rds-api/readystate-property-rds.md) proprietà modificata, in modo che venga inviata una notifica quando un'operazione asincrona è stata completata, terminata o si è verificato un errore. L'evento [OnError](../../reference/rds-api/onerror-event-rds.md) viene chiamato ogni volta che si verifica un errore, anche se l'errore si verifica durante un'operazione asincrona.  
  
> [!NOTE]
>  Microsoft Internet Explorer fornisce due eventi aggiuntivi per Servizi Desktop remoto: **onDataSetChanged**, che indica che il **Recordset** è funzionante ma sta ancora recuperando righe e **onDataSetComplete**, che indica che il **Recordset** ha completato il recupero delle righe.  
  
## <a name="see-also"></a>Vedere anche  
 [Modello di programmazione RDS con oggetti](./rds-programming-model-with-objects.md)   
 [Oggetto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Oggetto DataFactory (RDSServer)](../../reference/rds-api/datafactory-object-rdsserver.md)   
 [Oggetto DataSpace (RDS)](../../reference/rds-api/dataspace-object-rds.md)   
 [Scenario RDS](./rds-scenario.md)   
 [Esercitazione su RDS](./rds-tutorial.md)   
 [Utilizzo e sicurezza per RDS](./rds-usage-and-security.md)