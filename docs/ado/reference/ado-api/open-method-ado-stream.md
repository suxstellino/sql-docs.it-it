---
description: Metodo Open (Stream - ADO)
title: Metodo Open (flusso ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_Open
- _Stream::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: d26f48fb-904e-4932-a245-3b4332ca1600
author: rothja
ms.author: jroth
ms.openlocfilehash: ba1b13e92b4c26be0c117693b807371e66e651ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041361"
---
# <a name="open-method-ado-stream"></a>Metodo Open (Stream - ADO)
Apre un oggetto [Stream](./stream-object-ado.md) per modificare i flussi di dati binari o di testo.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
Stream.Open Source, Mode , OpenOptions, UserName, Password  
```  
  
#### <a name="parameters"></a>Parametri  
 *Origine*  
 facoltativo. Valore **Variant** che specifica l'origine dei dati per il **flusso**. L' *origine* può contenere una stringa URL assoluta che punta a un nodo esistente in una struttura ad albero nota, ad esempio un messaggio di posta elettronica o un file System. È necessario specificare un URL tramite la parola chiave URL ("URL =*schema*://*Server* / *Folder*"). In alternativa, l' *origine* può contenere un riferimento a un oggetto [record](./record-object-ado.md) già aperto, che consente di aprire il flusso predefinito associato al **record**. Se l' *origine* non è specificata, per impostazione predefinita viene creata un'istanza e viene aperto un **flusso** , associato a nessuna origine sottostante. Per ulteriori informazioni sugli schemi URL e sui relativi provider associati, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
 *Modalità*  
 facoltativo. Valore [ConnectModeEnum](./connectmodeenum.md) che specifica la modalità di accesso per il **flusso** risultante (ad esempio, di lettura/scrittura o di sola lettura). Il valore predefinito è **adModeUnknown**. Per ulteriori informazioni sulle modalità di accesso, vedere la proprietà [mode](./mode-property-ado.md) . Se non si specifica *mode* , viene ereditato dall'oggetto di origine. Se, ad esempio, il **record** di origine viene aperto in modalità di sola lettura, per impostazione predefinita il **flusso** verrà aperto anche in modalità di sola lettura.  
  
 *OpenOptions*  
 facoltativo. Valore [StreamOpenOptionsEnum](./streamopenoptionsenum.md) . Il valore predefinito è **adOpenStreamUnspecified**.  
  
 *UserName*  
 facoltativo. Valore **stringa** che contiene l'identificazione dell'utente che, se necessario, accede all'oggetto **flusso** .  
  
 *Password*  
 facoltativo. Valore **stringa** che contiene la password che, se necessaria, accede all'oggetto **flusso** .  
  
## <a name="remarks"></a>Commenti  
 Quando un oggetto **record** viene passato come parametro di origine, i parametri *userid* e *password* non vengono utilizzati perché l'accesso all'oggetto **record** è già disponibile. Analogamente, la [modalità](./mode-property-ado.md) dell'oggetto **record** viene trasferita all'oggetto **flusso** . Quando l' *origine* non è specificata, il **flusso** aperto non contiene dati e ha una [dimensione](./size-property-ado-stream.md) pari a zero (0). Per evitare di perdere i dati scritti in questo **flusso** quando il **flusso** viene chiuso, salvare il **flusso** con i metodi [CopyTo](./copyto-method-ado.md) o [SaveToFile](./savetofile-method.md) oppure salvarlo in un'altra posizione di memoria.  
  
 Un valore *OpenOptions* di **adOpenStreamFromRecord** identifica il contenuto del parametro di *origine* come oggetto **record** già aperto. Il comportamento predefinito consiste nel considerare *source* come un URL che punta direttamente a un nodo in una struttura ad albero, ad esempio un file. Viene aperto il flusso predefinito associato al nodo.  
  
 Mentre il **flusso** non è aperto, è possibile leggere tutte le proprietà di sola lettura del **flusso**. Se un **flusso** viene aperto in modo asincrono, tutte le operazioni successive (ad eccezione del controllo [dello stato](./state-property-ado.md) e di altre proprietà di sola lettura) vengono bloccate fino al completamento dell'operazione di **apertura** .  
  
 Oltre alle opzioni descritte in precedenza, non specificando l' *origine*, è possibile creare un'istanza di un oggetto **flusso** in memoria senza associarla a un'origine sottostante. È possibile aggiungere dati al flusso in modo dinamico scrivendo dati binari o di testo nel **flusso** con [Write](./write-method.md) o [WRITETEXT](./writetext-method.md)o caricando i dati da un file con [LoadFromFile](./loadfromfile-method-ado.md).  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Metodo Open (connessione ADO)](./open-method-ado-connection.md)   
 [Metodo Open (record ADO)](./open-method-ado-record.md)   
 [Metodo Open (recordset ADO)](./open-method-ado-recordset.md)   
 [Metodo OpenSchema](./openschema-method.md)   
 [Metodo SaveToFile](./savetofile-method.md)