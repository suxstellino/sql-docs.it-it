---
description: Oggetto DataSpace (Servizi Desktop remoto)
title: Oggetto DataSpace (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataSpace object [RDS]
ms.assetid: 9194bffa-5bdf-4dff-af86-f7158c23bfa7
author: rothja
ms.author: jroth
ms.openlocfilehash: cf507d982ede5af1ced904618e7ffc17be083207
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053182"
---
# <a name="dataspace-object-rds"></a>Oggetto DataSpace (Servizi Desktop remoto)
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
 Crea proxy sul lato client per oggetti business personalizzati che si trovano nel livello intermedio.  
  
 Remote Data Service richiede proxy di oggetti business in modo che i componenti lato client possano comunicare con gli oggetti business presenti nel livello intermedio. I proxy semplificano la creazione di pacchetti, la disassemblaggio e il trasporto (marshalling) dei dati del [Recordset](../ado-api/recordset-object-ado.md) dell'applicazione attraverso i limiti del processo o del computer.  
  
 Remote Data Service USA **RDS.** Metodo [CreateObject](./createobject-method-rds.md) dell'oggetto DataSpace per la creazione di proxy di oggetti business. Il proxy dell'oggetto business viene creato dinamicamente ogni volta che viene creata un'istanza della relativa controparte dell'oggetto business di livello intermedio. Remote Data Service supporta i protocolli seguenti: HTTP, HTTPS (HTTP Secure Sockets), DCOM e in-process (i componenti client e l'oggetto business si trovano nello stesso computer).  
  
> [!NOTE]
>  RDS si comporta in modo "senza stato" quando il Servizi Desktop remoto **. L'oggetto DataSpace** usa i protocolli http o HTTPS. Ovvero, qualsiasi informazione interna relativa a una richiesta client viene eliminata dopo che il server restituisce una risposta.  
  
> [!NOTE]
>  Anche se l'oggetto business sembra esistere per la durata del proxy dell'oggetto business, l'oggetto business esiste effettivamente solo fino a quando non viene inviata una risposta a una richiesta. Quando viene eseguita una richiesta (ovvero, viene richiamato un metodo sull'oggetto business), il proxy apre una nuova connessione al server e il server crea una nuova istanza dell'oggetto business. Dopo che l'oggetto business risponde alla richiesta, il server Elimina l'oggetto business e chiude la connessione.  
  
> [!NOTE]
>  Questo comportamento significa che non è possibile passare dati da una richiesta a un'altra utilizzando una proprietà o una variabile dell'oggetto business. Per salvare in modo permanente i dati di stato, è necessario utilizzare un altro meccanismo, ad esempio un file o un argomento del metodo.  
  
 ID di classe per **RDS. L'oggetto DataSpace** è BD96C556-65A3-11D0-983A-00C04FC29E36.  
  
 L'oggetto **DataSpace** è sicuro per lo scripting.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto DataSpace (Servizi Desktop remoto)](./dataspace-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio dell'oggetto DataSpace e del metodo CreateObject (VBScript)](./dataspace-object-and-createobject-method-example-vbscript.md)