---
description: Metodo Synchronize21 (Servizi Desktop remoto)
title: Metodo Synchronize21 (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Synchronize21 method [ADO]
ms.assetid: 6b35f136-9d9a-4bdd-8144-67decfd3c4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: fb782b8c61f3ae80c3e37d5ff28b040651eb5669
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166091"
---
# <a name="synchronize21-method-rds"></a>Metodo Synchronize21 (Servizi Desktop remoto)
Sincronizzare il recordset specificato con il database specificato dalla stringa di connessione per l'utilizzo con ADO 2,1.  
  
> [!IMPORTANT]
>  A partire da Windows 8 e Windows Server 2012, i componenti server Servizi Desktop remoto non sono più inclusi nel sistema operativo Windows. per altri dettagli, vedere le informazioni di riferimento sulla compatibilità di Windows 8 e [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) . I componenti client Servizi Desktop remoto verranno rimossi in una versione futura di Windows. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Le applicazioni che utilizzano Servizi Desktop remoto devono eseguire la migrazione a [WCF Data Services](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
object.Synchronize21(ConnectionString As String, HandlerString As String, lSynchronizeOptions As Long, ppRecordset As Object, pStatusArray)  
```  
  
#### <a name="parameters"></a>Parametri  
 *ConnectionString*  
 Stringa utilizzata per la connessione al provider OLE DB in cui verrà inviata la richiesta. Se viene utilizzato un gestore, il gestore può modificare o sostituire la stringa di connessione.  
  
 *HandlerString*  
 La stringa identifica il gestore da usare con questa esecuzione. La stringa contiene due parti. La prima parte contiene il nome (ProgID) del gestore da usare. La seconda parte della stringa contiene gli argomenti da passare al gestore. Il modo in cui la stringa arguments viene interpretata è un gestore specifico. Le due parti sono separate dalla prima istanza di una virgola nella stringa. La stringa arguments può contenere altre virgole. Gli argomenti sono facoltativi.  
  
 *lSynchronizeOptions*  
 Maschera di bit delle opzioni di sincronizzazione.  
  
 1 =*UpdateTransact* aggiornamenti al database sono racchiusi in una transazione. Se uno degli aggiornamenti ha esito negativo, la transazione verrà interrotta.  
  
 2 =*RefreshWithUpdate* causa la restituzione degli Stati delle righe quando non viene impostato né *Refresh* né *RefreshConflicts* .  
  
 4 =*Aggiorna* il recordset viene aggiornato con i dati correnti del database. Gli aggiornamenti in sospeso non vengono inseriti nel database. Se questo bit non è impostato, il recordset non viene aggiornato e viene effettuato il push di tutti gli aggiornamenti in sospeso nel database.  
  
 8 =*RefreshConflicts* le righe con modifiche in sospeso non vengono aggiornate. Le righe che non è stato possibile aggiornare vengono aggiornate con i dati correnti del database.  
  
 *ppRecordset*  
 Puntatore a un puntatore al recordset da sincronizzare.  
  
 *pStatusArray*  
 Variante utilizzata per restituire una matrice sicura di Stati di riga per le righe interessate dalla sincronizzazione. Non impostato se nessuna delle seguenti opzioni di sincronizzazione è impostata: *RefreshWithUpdate*, *Refresh* e *RefreshConflicts*.  
  
## <a name="remarks"></a>Commenti  
 Il parametro *HandlerString* può essere null. Ciò che accade in questo caso dipende dalla modalità di configurazione del server RDS. Una stringa del gestore "MSDFMAP. handler" indica che deve essere utilizzato il gestore fornito da Microsoft (Msdfmap.dll). Una stringa del gestore "MASDFMAP. Handler, sample.ini" indica che è necessario utilizzare il gestore di Msdfmap.dll e che l'argomento "sample.ini" deve essere passato al gestore. Msdfmap.dll interpreterà quindi l'argomento come direzione per usare il sample.ini per verificare le stringhe di connessione e di query.  
  
> [!NOTE]
>  Il metodo **Synchronize21** è semplicemente una versione del [metodo Synchronize (RDS)](./synchronize-method-rds.md). Quando è necessario usare il metodo **Synchronize** per comunicare con ADO 2,1, è possibile chiamare il metodo **Synchronize21** . Le funzionalità del metodo **Synchronize** in ADO 2,5 e versioni successive sono un superset delle funzionalità fornite per lo stesso metodo in ADO 2,1.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto DataFactory (RDSServer)](./datafactory-object-rdsserver.md)