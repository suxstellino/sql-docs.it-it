---
description: Oggetto Stream (ADO)
title: Oggetto Stream (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream
helpviewer_keywords:
- Stream object [ADO]
ms.assetid: 0514531f-009d-4519-abc3-d727014a39f1
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d18917bdcc0a08c5fbddcc1ff908c5aed8ca1f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170166"
---
# <a name="stream-object-ado"></a>Oggetto Stream (ADO)
Rappresenta un flusso di dati binari o testo.  
  
 Nelle gerarchie strutturate ad albero quali un file system o un sistema di posta elettronica, è possibile che a un [record](./record-object-ado.md) sia associato un flusso binario predefinito di bit che contenga il contenuto del file o del messaggio di posta elettronica. Un oggetto **flusso** può essere usato per modificare campi o record contenenti questi flussi di dati. È possibile ottenere un oggetto **flusso** nei modi seguenti:  
  
-   Da un URL che punta a un oggetto (in genere un file) contenente dati binari o di testo. Questo oggetto può essere un documento semplice, un oggetto **record** che rappresenta un documento strutturato o una cartella.  
  
-   Aprendo l'oggetto **flusso** predefinito associato a un oggetto **record** . È possibile ottenere il flusso predefinito associato a un oggetto **record** quando il **record** viene aperto, per eliminare un round trip solo per aprire il flusso.  
  
-   Creando un'istanza di un oggetto **flusso** . Questi oggetti **flusso** possono essere usati per archiviare i dati ai fini dell'applicazione. A differenza di un **flusso** associato a un URL o al **flusso** predefinito di un **record**, per impostazione predefinita un **flusso** di cui è stata creata un'istanza non ha alcuna associazione con un'origine sottostante.  
  
 Con i metodi e le proprietà di un oggetto **flusso** , è possibile eseguire le operazioni seguenti:  
  
-   Aprire un oggetto **flusso** da un **record** o un URL con il metodo [Open](./open-method-ado-stream.md) .  
  
-   Chiudere un **flusso** con il metodo [Close](./close-method-ado.md) .  
  
-   Byte di input o testo in un **flusso** con i metodi [Write](./write-method.md) e [WRITETEXT](./writetext-method.md) .  
  
-   Leggere i byte dal **flusso** con i metodi [Read](./read-method.md) e [READTEXT](./readtext-method.md) .  
  
-   Scrivere i dati di **flusso** ancora presenti nel buffer ADO nell'oggetto sottostante con il metodo [Flush](./flush-method-ado.md) .  
  
-   Copiare il contenuto di un **flusso** in un altro **flusso** con il metodo [CopyTo](./copyto-method-ado.md) .  
  
-   Controllare il modo in cui le righe vengono lette dal file di origine con il metodo [SkipLine](./skipline-method.md)e la proprietà [LineSeparator](./lineseparator-property-ado.md) .  
  
-   Determinare la fine della posizione del flusso con la proprietà [EOS](./eos-property.md)e il metodo [SetEOS](./seteos-method.md) .  
  
-   Salvare e ripristinare i dati nei file con i metodi [SaveToFile](./savetofile-method.md)e [LoadFromFile](./loadfromfile-method-ado.md) .  
  
-   Specificare il set di caratteri utilizzato per archiviare il **flusso** con la proprietà [CharSet](./charset-property-ado.md) .  
  
-   Arrestare un'operazione di **flusso** asincrona con il metodo [Cancel](./cancel-method-ado.md) .  
  
-   Determinare il numero di byte in un **flusso** con la proprietà [size](./size-property-ado-stream.md) .  
  
-   Controllare la posizione corrente all'interno di un **flusso** con la proprietà [position](./position-property-ado.md) .  
  
-   Determinare il tipo di dati in un **flusso** con la proprietà [Type](./type-property-ado-stream.md) .  
  
-   Determinare lo stato corrente del **flusso** (chiuso, aperto o in esecuzione) con la proprietà [state](./state-property-ado.md) .  
  
-   Specificare la modalità di accesso per il **flusso** con la proprietà [mode](./mode-property-ado.md) .  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
 L'oggetto **flusso** è sicuro per lo scripting.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Stream](./stream-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Record e flussi](../../guide/data/records-and-streams.md)