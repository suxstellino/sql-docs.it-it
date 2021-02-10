---
description: Costanti enumerate ADO
title: Costanti enumerate ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- enumerated constants [ADO]
ms.assetid: c97ed131-1a93-463c-9e61-22f029b0c474
author: rothja
ms.author: jroth
ms.openlocfilehash: a0295fd9f1feec8db54734a5dfc02eba483e1615
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031283"
---
# <a name="ado-enumerated-constants"></a>Costanti enumerate ADO
Per semplificare il debug, le enumerazioni ADO elencano un valore per ogni costante. Tuttavia, questo valore è puramente consultivo e può variare da una versione di ADO a un'altra. Il codice deve dipendere solo dal nome, non dal valore effettivo, di ogni costante enumerata.  
  
|Costante|Descrizione|  
|-|-|  
|[ADCPROP_ASYNCTHREADPRIORITY_ENUM](./adcprop-asyncthreadpriority-enum.md)|Per un oggetto **Recordset** RDS, specifica la priorità di esecuzione del thread asincrono che recupera i dati.|  
|[ADCPROP_AUTORECALC_ENUM](./adcprop-autorecalc-enum.md)|Specifica quando il provider **MSDataShape** ricalcola le colonne aggregate e calcolate in un **Recordset** gerarchico.|  
|[ADCPROP_UPDATECRITERIA_ENUM](./adcprop-updatecriteria-enum.md)|Specifica i campi che è possibile utilizzare per rilevare i conflitti durante un aggiornamento ottimistico di una riga dell'origine dati con un oggetto **Recordset** .|  
|[ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md)|Specifica se il metodo **UpdateBatch** è seguito da un'operazione implicita di **Risincronizzazione** del metodo e, in tal caso, dall'ambito di tale operazione.|  
|[AffectEnum](./affectenum.md)|Specifica quali record sono interessati da un'operazione.|  
|[BookmarkEnum](./bookmarkenum.md)|Specifica un segnalibro che indica dove deve iniziare l'operazione.|  
|[CommandTypeEnum](./commandtypeenum.md)|Specifica la modalità di interpretazione di un argomento di comando.|  
|[CompareEnum](./compareenum.md)|Specifica la posizione relativa di due record rappresentati dai relativi segnalibri.|  
|[ConnectModeEnum](./connectmodeenum.md)|Specifica le autorizzazioni disponibili per la modifica dei dati in una **connessione**, l'apertura di un **record** o la specifica di valori per la proprietà **mode** degli oggetti **record** e **flusso** .|  
|[ConnectOptionEnum](./connectoptionenum.md)|Specifica se il metodo **Open** di un oggetto **Connection** deve essere restituito dopo (in modalità sincrona) o prima (in modo asincrono) che la connessione viene stabilita.|  
|[ConnectPromptEnum](./connectpromptenum.md)|Specifica se deve essere visualizzata una finestra di dialogo per richiedere parametri mancanti durante l'apertura di una connessione a un'origine dati ODBC.|  
|[CopyRecordOptionsEnum](./copyrecordoptionsenum.md)|Specifica il comportamento del metodo **CopyRecord** .|  
|[CursorLocationEnum](./cursorlocationenum.md)|Specifica la posizione del motore del cursore.|  
|[CursorOptionEnum](./cursoroptionenum.md)|Specifica la funzionalità che il metodo **supporta** deve testare.|  
|[CursorTypeEnum](./cursortypeenum.md)|Specifica il tipo di cursore utilizzato in un oggetto **Recordset** .|  
|[DataTypeEnum](./datatypeenum.md)|Specifica il tipo di dati di un **campo**, un **parametro** o una **proprietà**.|  
|[EditModeEnum](./editmodeenum.md)|Specifica lo stato di modifica di un record.|  
|[ErrorValueEnum](./errorvalueenum.md)|Specifica il tipo di errore in fase di esecuzione ADO.|  
|[EventReasonEnum](./eventreasonenum.md)|Specifica il motivo per cui è stato generato un evento.|  
|[EventStatusEnum](./eventstatusenum.md)|Specifica lo stato corrente dell'esecuzione di un evento.|  
|[ExecuteOptionEnum](./executeoptionenum.md)|Specifica la modalità di esecuzione di un comando da un provider.|  
|[FieldEnum](./fieldenum.md)|Specifica i campi speciali a cui viene fatto riferimento nella raccolta **Fields** di un oggetto **record** .|  
|[FieldAttributeEnum](./fieldattributeenum.md)|Specifica uno o più attributi di un oggetto **campo** .|  
|[FieldStatusEnum](./fieldstatusenum.md)|Specifica lo stato di un oggetto **campo** .|  
|[FilterGroupEnum](./filtergroupenum.md)|Specifica il gruppo di record da filtrare da un **Recordset**.|  
|[GetRowsOptionEnum](./getrowsoptionenum.md)|Specifica il numero di record da recuperare da un **Recordset**.|  
|[IsolationLevelEnum](./isolationlevelenum.md)|Specifica il livello di isolamento della transazione per un oggetto **Connection** .|  
|[LineSeparatorsEnum](./lineseparatorsenum.md)|Specifica il carattere utilizzato come separatore di riga negli oggetti del **flusso** di testo.|  
|[LockTypeEnum](./locktypeenum.md)|Specifica il tipo di blocco inserito nei record durante la modifica.|  
|[MarshalOptionsEnum](./marshaloptionsenum.md)|Specifica i record che devono essere restituiti al server.|  
|[MoveRecordOptionsEnum](./moverecordoptionsenum.md)|Specifica il comportamento del metodo **MoveRecord** dell'oggetto **record** .|  
|[ObjectStateEnum](./objectstateenum.md)|Specifica se un oggetto è aperto o chiuso, connettendosi a un'origine dati, eseguendo un comando o recuperando i dati.|  
|[ParameterAttributesEnum](./parameterattributesenum.md)|Specifica gli attributi di un oggetto **Parameter** .|  
|[ParameterDirectionEnum](./parameterdirectionenum.md)|Specifica se il **parametro** rappresenta un parametro di input, un parametro di output o entrambi o se il parametro è il valore restituito da un stored procedure.|  
|[PersistFormatEnum](./persistformatenum.md)|Specifica il formato in cui salvare un **Recordset**.|  
|[PositionEnum](./positionenum.md)|Specifica la posizione corrente del puntatore del record all'interno di un **Recordset**.|  
|[PropertyAttributesEnum](./propertyattributesenum.md)|Specifica gli attributi di un oggetto **Proprietà** .|  
|[RecordCreateOptionsEnum](./recordcreateoptionsenum.md)|Specifica per il metodo di **apertura** dell'oggetto **record** se deve essere aperto un **record** esistente o se deve essere creato un nuovo **record** .|  
|[RecordOpenOptionsEnum](./recordopenoptionsenum.md)|Specifica le opzioni per l'apertura di un **record**. Questi valori possono essere combinati usando un operatore OR.|  
|[RecordStatusEnum](./recordstatusenum.md)|Specifica lo stato di un record per quanto riguarda gli aggiornamenti in batch e altre operazioni bulk.|  
|[RecordTypeEnum](./recordtypeenum.md)|Specifica il tipo di oggetto **record** .|  
|[ResyncEnum](./resyncenum.md)|Specifica se i valori sottostanti vengono sovrascritti da una chiamata alla **Risincronizzazione**.|  
|[SaveOptionsEnum](./saveoptionsenum.md)|Specifica se un file deve essere creato o sovrascritto durante il salvataggio da un oggetto **flusso** . I valori possono essere combinati con un operatore AND.|  
|[SchemaEnum](./schemaenum.md)|Specifica il tipo di **Recordset** dello schema recuperato dal metodo **OpenSchema** . Specifica la direzione di una ricerca di record in un **Recordset**.|  
|[SearchDirectionEnum](./searchdirectionenum.md)|Specifica la direzione di una ricerca di record in un **Recordset**. Specifica il tipo di **ricerca** da eseguire.|  
|[SeekEnum](./seekenum.md)|Specifica il tipo di **ricerca** da eseguire. Specifica le opzioni per l'apertura di un oggetto **flusso** . I valori possono essere combinati con un operatore AND.|  
|[StreamOpenOptionsEnum](./streamopenoptionsenum.md)|Specifica le opzioni per l'apertura di un oggetto **flusso** . I valori possono essere combinati con un operatore AND. Specifica se l'intero flusso o la riga successiva devono essere letti da un oggetto **flusso** .|  
|[StreamReadEnum](./streamreadenum.md)|Specifica se l'intero flusso o la riga successiva devono essere letti da un oggetto **flusso** . Specifica il tipo di dati archiviati in un oggetto **flusso** .|  
|[StreamTypeEnum](./streamtypeenum.md)|Specifica il tipo di dati archiviati in un oggetto **flusso** . Specifica se un separatore di riga viene aggiunto alla stringa scritta in un oggetto **flusso** .|  
|[StreamWriteEnum](./streamwriteenum.md)|Specifica se un separatore di riga viene aggiunto alla stringa scritta in un oggetto **flusso** . Specifica il formato durante il recupero di un **Recordset** come stringa.|  
|[StringFormatEnum](./stringformatenum.md)|Specifica il formato durante il recupero di un **Recordset** come stringa. Specifica gli attributi di transazione di un oggetto **Connection** .|  
|[XactAttributeEnum](./xactattributeenum.md)|Specifica gli attributi di transazione di un oggetto **Connection** .|  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento all'API ADO](./ado-api-reference.md)   
 [Raccolte ADO](./ado-collections.md)   
 [Proprietà dinamiche ADO](./ado-dynamic-properties.md)   
 [Appendice B: errori ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventi ADO](./ado-events.md)   
 [Metodi ADO](./ado-methods.md)   
 [Modello a oggetti ADO](./ado-object-model.md)   
 [Oggetti e interfacce ADO](./ado-objects-and-interfaces.md)   
 [Proprietà ADO](./ado-properties.md)