---
description: 'Creazione di istanze di eventi ADO: ADO e WFC'
title: "Creazione di istanze dell'evento ADO: ADO e WFC | Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 9ee4be21-657b-407a-afa4-0b27a6b096ce
author: rothja
ms.author: jroth
ms.openlocfilehash: b7ed585adcfc9542e3d396f2b0fd1a82d65cdcf4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028353"
---
# <a name="ado-event-instantiation-ado-and-wfc"></a>Creazione di istanze di eventi ADO: ADO e WFC
ADO per Windows Foundation Classes (ADO/WFC) si basa sul modello di eventi ADO e presenta una Application Programming Interface semplificata. In generale, ADO/WFC intercetta gli eventi ADO, consolida i parametri dell'evento in una singola classe di evento e quindi chiama il gestore eventi.  
  
### <a name="to-use-ado-events-in-adowfc"></a>Per utilizzare gli eventi ADO in ADO/WFC  
  
1.  Definire un gestore eventi personalizzato per elaborare un evento. Se ad esempio si vuole elaborare l'evento **ConnectComplete** nella famiglia **ConnectionEvent** , è possibile usare il codice seguente:  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    {  
        System.out.println("onConnectComplete:" + e);  
    }  
    ```  
  
2.  Definire un oggetto gestore per rappresentare il gestore eventi. Il tipo di dati dell'oggetto gestore deve essere **ConnectEventHandler** per un evento di tipo **ConnectionEvent** o **RecordsetEventHandler** per un evento di tipo **RecordsetEvent**. Ad esempio, scrivere il codice seguente per il gestore dell'evento **ConnectComplete** :  
  
    ```  
    ConnectionEventHandler handler =   
        new ConnectionEventHandler(this, "onConnectComplete");  
    ```  
  
     Il primo argomento del costruttore **ConnectionEventHandler** è un riferimento alla classe che contiene il gestore eventi denominato nel secondo argomento.  
  
3.  Aggiungere il gestore eventi a un elenco di gestori designati per elaborare un particolare tipo di evento. Usare il metodo con un nome, ad esempio **addon**_EventName_(*gestore*).  
  
4.  ADO/WFC implementa internamente tutti i gestori eventi ADO. Un evento causato da una **connessione** o da un'operazione **Recordset** viene pertanto intercettato da un gestore eventi ADO/WFC.  
  
     Il gestore dell'evento ADO/WFC passa i parametri ADO **ConnectionEvent** in un'istanza della classe **CONNECTIONEVENT** ADO/WFC o nei parametri ADO **RecordsetEvent** in un'istanza della classe ADO/WFC **RecordsetEvent** . Queste classi ADO/WFC consolidano i parametri dell'evento ADO; ovvero ogni classe ADO/WFC contiene un membro dati per ogni parametro univoco in tutti i metodi ADO **ConnectionEvent** o **RecordsetEvent** .  
  
5.  ADO/WFC chiama quindi il gestore eventi con l'oggetto evento ADO/WFC. Ad esempio, il gestore **onConnectComplete** ha una firma simile alla seguente:  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    ```  
  
     Il primo argomento è il tipo di oggetto che ha inviato l'evento ([connessione](../../reference/ado-api/connection-object-ado.md) o [Recordset](../../reference/ado-api/recordset-object-ado.md)) e il secondo argomento è l'oggetto evento ADO/WFC (**ConnectionEvent** o **RecordsetEvent**).  
  
     La firma del gestore eventi è più semplice rispetto a un evento ADO. Tuttavia, è comunque necessario comprendere il modello di eventi ADO per conoscere i parametri applicabili a un evento e la modalità di risposta.  
  
6.  Tornare dal gestore eventi al gestore ADO/WFC per l'evento ADO. ADO/WFC copia i membri dei dati di evento ADO/WFC pertinenti nei parametri dell'evento ADO, quindi il gestore dell'evento ADO restituisce.  
  
7.  Al termine dell'elaborazione, rimuovere il gestore dall'elenco dei gestori eventi ADO/WFC. Usare il metodo con un nome, ad esempio **privo RemoveOn**_EventName_(*gestore*).  
  
## <a name="see-also"></a>Vedere anche  
 [Riepilogo del gestore eventi ADO](./ado-event-handler-summary.md)   
 [Indice della sintassi ADO-WFC](../../reference/ado-api/ado-wfc-syntax-index.md)   
 [Parametri evento](./event-parameters.md)   
 [Interazione tra i gestori eventi](./how-event-handlers-work-together.md)   
 [Tipi di eventi](./types-of-events.md)