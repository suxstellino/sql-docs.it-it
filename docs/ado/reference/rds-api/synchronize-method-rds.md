---
description: Metodo Synchronize (Servizi Desktop remoto)
title: Metodo Synchronize (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Synchronize method [ADO]
ms.assetid: 7af42866-7db2-4174-8251-388a2cf741f2
author: rothja
ms.author: jroth
ms.openlocfilehash: c569505febe68a588799a0eb5c3d3142b3633137
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052852"
---
# <a name="synchronize-method-rds"></a>Metodo Synchronize (Servizi Desktop remoto)
Sincronizzare il recordset specificato con il database specificato dalla stringa di connessione per l'utilizzo in ADO 2,5 e versioni successive.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.Synchronize(ConnectionString As String, HandlerString As String, lSynchronizeOptions As Long, ppRecordset As Object, pStatusArray, [lcid As Long], [pInformation)  
```  
  
#### <a name="parameters"></a>Parametri  
 *ConnectionString*  
 Stringa utilizzata per la connessione al provider OLE DB in cui verrà inviata la richiesta. Se viene utilizzato un gestore, il gestore può modificare o sostituire la stringa di connessione.  
  
 *HandlerString*  
 La stringa identifica il gestore da usare con questa esecuzione. La stringa contiene due parti. La prima parte contiene il nome (ProgID) del gestore da usare. La seconda parte della stringa contiene gli argomenti da passare al gestore. Il modo in cui la stringa arguments viene interpretata è un gestore specifico. Le due parti sono separate dalla prima istanza di una virgola nella stringa (sebbene la stringa degli argomenti possa contenere altre virgole). Gli argomenti sono facoltativi.  
  
 *lSynchronizeOptions*  
 Maschera di bit delle opzioni di sincronizzazione.  
  
 1 =*UpdateTransact* aggiornamenti al database sono racchiusi in una transazione. Se uno degli aggiornamenti ha esito negativo, la transazione verrà interrotta.  
  
 2 =*RefreshWithUpdate* causa la restituzione degli Stati delle righe quando non viene impostato né *Refresh* né *RefreshConflicts* .  
  
 4 =*Aggiorna* il recordset viene aggiornato con i dati correnti del database. Gli aggiornamenti in sospeso non vengono inseriti nel database. Se questo bit non è impostato, il recordset non viene aggiornato e tutti gli aggiornamenti in sospeso vengono inseriti nel database.  
  
 8 =*RefreshConflicts* le righe con modifiche in sospeso non vengono aggiornate. Le righe che non è stato possibile aggiornare vengono aggiornate con i dati correnti del database.  
  
 *ppRecordset*  
 Puntatore al recordset da sincronizzare.  
  
 *pStatusArray*  
 Variante utilizzata per restituire una matrice sicura di Stati di riga per le righe interessate dalla sincronizzazione. Non impostato se nessuna delle seguenti opzioni di sincronizzazione è impostata: *RefreshWithUpdate*, *Refresh* e *RefreshConflicts*.  
  
 *lcid*  
 Identificatore LCID utilizzato per compilare eventuali errori restituiti in *pInformation*.  
  
 *pInformation*  
 Puntatore a un errore di informazioni restituito da **Execute**. Se è NULL, non vengono restituite informazioni sull'errore.  
  
## <a name="remarks"></a>Commenti  
 Il parametro *HandlerString* può essere null. Ciò che accade in questo caso dipende dalla modalità di configurazione del server RDS. Una stringa del gestore "MSDFMAP. handler" indica che deve essere utilizzato il gestore fornito da Microsoft (Msdfmap.dll). Una stringa del gestore "MASDFMAP. Handler, sample.ini" indica che è necessario utilizzare il gestore di Msdfmap.dll e che l'argomento "sample.ini" deve essere passato al gestore. Msdfmap.dll interpreterà quindi l'argomento come direzione per usare il sample.ini per verificare le stringhe di connessione e di query.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)