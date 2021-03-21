---
title: ISSAsynchStatus::GetStatus (OLE DB Driver) | Microsoft Docs
description: Informazioni su come il metodo ISSAsynchStatus::GetStatus restituisce lo stato di un'operazione in esecuzione asincrona in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::GetStatus (OLE DB)
apitype: COM
helpviewer_keywords:
- GetStatus method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: eeb012036ffe08467ae5367ceb44888ef6bd48f3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752371"
---
# <a name="issasynchstatusgetstatus-ole-db"></a>ISSAsynchStatus::GetStatus (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Restituisce lo stato di un'operazione in esecuzione in modo asincrono.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT GetStatus(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation,  
        DBCOUNTITEM *pulProgress,  
        DBCOUNTITEM *pulProgressMax,  
        DBASYNCHPHASE *peAsynchPhase,  
        LPOLESTR *ppwszStatusText);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hChapter*[in]  
 Handle del capitolo. Se l'oggetto sottoposto a polling non è un oggetto set di righe o l'operazione non è applicabile a un capitolo, deve essere impostato sul valore DB_NULL_HCHAPTER, che viene ignorato dal provider.  
  
 *eOperation*[in]  
 Operazione per la quale viene richiesto lo stato asincrono. Deve essere usato il valore seguente:  
  
 DBASYNCHOP_OPEN: il consumer richiede informazioni sull'apertura o sul popolamento asincrono di un set di righe o sull'inizializzazione asincrona di un oggetto origine dati. Se il provider è un provider conforme a OLE DB 2.5 che supporta l'associazione URL diretta, il consumer richiede informazioni sull'inizializzazione o sul popolamento asincrono di un oggetto origine dati, set di righe, riga o flusso.  
  
 *pulProgress*[out]  
 Puntatore alla memoria in cui restituire informazioni sullo stato corrente dell'operazione asincrona relativa al valore massimo previsto indicato nel parametro *pulProgressMax*. Per altre informazioni sul significato di *pulProgress*, vedere la descrizione di *peAsynchPhase*.  
  
 Se *pulProgress* è un puntatore Null, non vengono restituite informazioni sullo stato.  
  
 *pulProgressMax*[out]  
 Puntatore alla memoria in cui restituire il valore massimo previsto del parametro *pulProgress*. Questo valore può variare nelle diverse chiamate al metodo. Per altre informazioni sul significato di *pulProgressMax*, vedere la descrizione di *peAsynchPhase*.  
  
 Se *pulProgressMax* è un puntatore Null, non viene restituito alcun valore massimo previsto.  
  
 *peAsynchPhase*[out]  
 Puntatore alla memoria in cui restituire informazioni aggiuntive sullo stato dell'operazione asincrona. I valori validi includono:  
  
 DBASYNCHPHASE_INITIALIZATION: l'oggetto si trova in una fase di inizializzazione. Gli argomenti *pulProgress* e *pulProgressMax* indicano un rapporto di completamento stimato. L'oggetto non è ancora completamente materializzato. Il tentativo di chiamare qualsiasi altra interfaccia potrebbe non riuscire e il set completo di interfacce potrebbe non essere disponibile per l'oggetto. Se l'operazione asincrona è il risultato della chiamata a **ICommand::Execute** per un comando che aggiorna, elimina o inserisce righe e se *cParamSets* è maggiore di 1, *pulProgress* e *pulProgressMax* possono indicare lo stato di un singolo set di parametri o di una matrice completa di set di parametri.  
  
 DBASYNCHPHASE_POPULATION: l'oggetto si trova in una fase di popolamento. Anche se il set di righe viene inizializzato completamente e l'intervallo completo di interfacce è disponibile per l'oggetto, nel set di righe potrebbero essere presenti altre righe non ancora popolate. Sebbene *pulProgress* e *pulProgressMax* possano basarsi sul numero di righe popolate, in genere si basano sulla quantità di tempo o di lavoro necessaria per popolare il set di righe. Un chiamante deve pertanto utilizzare queste informazioni come stima approssimativa del tempo che potrebbe essere necessario e non dell'eventuale conteggio delle righe. Questa fase viene restituita solo durante il popolamento di un set di righe. Non viene restituita mai durante l'inizializzazione di un oggetto origine dati o l'esecuzione di un comando che aggiorna, elimina o inserisce righe.  
  
 DBASYNCHPHASE_COMPLETE: tutta l'elaborazione asincrona dell'oggetto è stata completata. Il metodo **ISSAsynchStatus::GetStatus** restituisce un oggetto HRESULT che indica il risultato dell'operazione. In genere, si tratta dell'oggetto HRESULT che sarebbe stato restituito se l'operazione fosse stata chiamata in modo sincrono. Se l'operazione asincrona è il risultato della chiamata a **ICommand::Execute** per un comando che aggiorna, elimina o inserisce righe, *pulProgress* e *pulProgressMax* sono uguali al numero totale di righe interessate dal comando. Se *cParamSets* è maggiore di 1, indica il numero totale di righe interessate da tutti i set di parametri specificati nell'esecuzione. Se *peAsynchPhase* è un puntatore Null, non viene restituito alcun codice di stato.  
  
 DBASYNCHPHASE_CANCELED: l'elaborazione asincrona dell'oggetto è stata interrotta. Il metodo **ISSAsynchStatus::GetStatus** restituisce DB_E_CANCELED. Se l'operazione asincrona è il risultato della chiamata a **ICommand::Execute** per un comando che aggiorna, elimina o inserisce righe, *pulProgress* è uguale al numero totale di righe, per tutti i set di parametri, interessate dal comando prima dell'annullamento.  
  
 *ppwszStatusText*[in/out]  
 Puntatore alla memoria contenente informazioni aggiuntive sull'operazione. Un provider può usare questo valore per distinguere tra elementi differenti di un'operazione, ad esempio le diverse risorse a cui si accede. Questa stringa viene localizzata in base alla proprietà DBPROP_INIT_LCID dell'oggetto origine dati.  
  
 Se *ppwszStatusText* è non Null nell'input, il provider restituisce lo stato associato allo specifico elemento identificato da *ppwszStatusText*. Se *ppwszStatusText* non indica un elemento di *eOperation*, il provider restituisce S_OK con *pulProgress* e *pulProgressMax* impostati sullo stesso valore. Se il provider non distingue tra elementi basati su un identificatore testuale, imposta *ppwszStatusText* su NULL e restituisce informazioni sull'operazione nel suo complesso. In caso contrario, se *ppwszStatusText* è non Null nell'input, il provider lascia *ppwszStatusText* invariato.  
  
 Se *ppwszStatusText* è Null nell'input, il provider imposta *ppwszStatusText* su un valore che indica ulteriori informazioni sull'operazione o su Null se queste informazioni non sono disponibili o se il metodo **ISSAsynchStatus::GetStatus** restituisce un errore. Quando *ppwszStatusText* è Null nell'input, il provider alloca memoria per la stringa di stato e restituisce l'indirizzo a tale memoria. Il consumer rilascia la memoria con **IMalloc::Free** quando la stringa non è più necessaria.  
  
 Se *ppwszStatusText* è Null nell'input, non viene restituita alcuna stringa di stato e il provider restituisce informazioni sugli elementi dell'operazione o sull'operazione in generale.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è stato restituito correttamente.  
  
-   Se *peAsynchPhase* è uguale a DBASYNCHPHASE_INITIALIZATION, l'oggetto non è ancora completamente inizializzato. Il tentativo di chiamare altre interfacce potrebbe non riuscire e il set completo di interfacce potrebbe non essere disponibile sull'oggetto.  
  
-   Se *peAsynchPhase* è uguale a DBASYNCHPHASE_POPULATION, il set di righe è completamente inizializzato e l'intervallo completo di interfacce è disponibile sull'oggetto. Nel set di righe potrebbero tuttavia essere presenti altre righe non ancora popolate.  
  
-   Se *peAsynchPhase* è uguale a DBASYNCHPHASE_COMPLETE, tutta l'elaborazione asincrona dell'oggetto è stata completata. L'oggetto è stato inizializzato e popolato completamente.  
  
 DB_E_CANCELED  
 L'elaborazione asincrona è stata annullata durante il popolamento del set di righe. Il popolamento viene arrestato, ma il set di righe rimane valido per le righe già popolate.  
  
 L'elaborazione asincrona è stata annullata durante l'inizializzazione dell'oggetto origine dati. L'oggetto origine dati si trova in uno stato non inizializzato.  
  
 E_INVALIDARG  
 Il parametro *hChapter* non è corretto.  
  
 E_UNEXPECTED  
 Il metodo **ISSAsynchStatus::GetStatus** è stato chiamato su un oggetto origine dati e **IDBInitialize::Initialize** non è stato chiamato su tale oggetto.  
  
 Il metodo **ISSAsynchStatus::GetStatus** è stato chiamato su un set di righe, **ITransaction::Commit** o **ITransaction::Abort** è stato chiamato e l'oggetto si trova in uno stato non valido.  
  
 Il metodo **ISSAsynchStatus::GetStatus** è stato chiamato su un set di righe annullato in modo asincrono nella fase di inizializzazione. Il set di righe si trova in uno stato non valido.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider.  
  
## <a name="remarks"></a>Osservazioni  
 Il comportamento del metodo **ISSAsynchStatus::GetStatus** è identico a quello del metodo **IDBAsynchStatus::GetStatus** con l'eccezione che se viene interrotta l'inizializzazione di un oggetto origine dati, viene restituito E_UNEXPECTED anziché DB_E_CANCELED (anche se [ISSAsynchStatus::WaitForAsynchCompletion](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md) restituirà DB_E_CANCELED). Ciò accade perché l'oggetto origine dati non rimane nello stato non valido in seguito a un'interruzione, allo scopo di consentire altri tentativi di inizializzazione.  
  
 Se il set di righe viene inizializzato o popolato in modo asincrono, deve supportare questo metodo.  
  
 Oltre ai valori restituiti elencati, **ISSAsynchStatus::GetStatus** può restituire qualsiasi HRESULT che verrebbe restituito dal metodo che ha avviato l'operazione asincrona, per indicare l'esito positivo o negativo dell'operazione.  
  
 Alcune operazioni asincrone potrebbero non essere in grado di restituire stati diversi da "terminato" e "non terminato". Per tali operazioni è necessario impostare *pulProgressMax* sul valore 1, a indicare la granularità all-or-nothing della stima, e pertanto le relative risposte sono 0/1 o 1/1.  
  
 Un provider può modificare *pulProgressMax* nelle chiamate successive e persino restituire un rapporto inferiore a quello precedente, se riflette una stima migliore del livello di completamento dell'attività.  
  
 Il provider non è obbligato a garantire una maggiore accuratezza, anche se questa è auspicabile quando sono possibili stime accettabili del completamento. L'impegno per ottenere questo risultato migliorerà la qualità dell'interfaccia utente perché è probabile che questa funzione venga utilizzata principalmente per fornire all'utente dettagli sullo stato. La soddisfazione dell'utente aumenta proporzionalmente alla qualità dei dettagli relativi a un'attività impercettibile e con esecuzione prolungata.  
  
 Se si chiama **ISSAsynchStatus::GetStatus** su un oggetto origine dati inizializzato o su un set di righe popolato oppure si passa un valore per *eOperation* diverso da DBASYNCHOP_OPEN, viene restituito S_OK con *pulProgress* e *pulProgressMax* impostati sullo stesso valore. Se il metodo **ISSAsynchStatus::GetStatus** viene chiamato su un oggetto creato in seguito all'esecuzione di un comando che aggiorna, elimina o inserisce righe, sia *pulProgress* che *pulProgressMax* indicano il numero totale di righe interessate dal comando.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di operazioni asincrone](../../oledb/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
