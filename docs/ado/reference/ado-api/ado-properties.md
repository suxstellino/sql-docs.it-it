---
description: Proprietà ADO
title: Proprietà ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- properties [ADO]
- ADO properties
ms.assetid: 0ac0d1a7-6c7a-4f4c-b115-428935e0f98b
author: rothja
ms.author: jroth
ms.openlocfilehash: f29b56b54c2800a77ac76134f0c946f09e34a621
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035721"
---
# <a name="ado-properties"></a>Proprietà ADO

|Proprietà|Descrizione|  
|-|-|  
|[AbsolutePage](./absolutepage-property-ado.md)|Indica in quale pagina si trova il record corrente.|  
|[AbsolutePosition](./absoluteposition-property-ado.md)|Indica la posizione ordinale del record corrente di un oggetto **Recordset** .|  
|[ActiveCommand](./activecommand-property-ado.md)|Indica l'oggetto **comando** che ha creato l'oggetto **Recordset** associato.|  
|[ActiveConnection](./activeconnection-property-ado.md)|Indica a quale oggetto **connessione** appartiene attualmente il **comando**, il **Recordset** o l'oggetto **record** specificato.|  
|[ActualSize](./actualsize-property-ado.md)|Indica la lunghezza effettiva del valore di un campo.|  
|[Attributes (Attributi)](./attributes-property-ado.md)|Indica una o più caratteristiche di un oggetto.|  
|[BOF e EOF](./bof-eof-properties-ado.md)|**BOF** indica che la posizione del record corrente precede il primo record in un oggetto recordset.<br /><br /> **EOF** indica che la posizione corrente del record è successiva all'ultimo record in un oggetto recordset.|  
|[Segnalibro](./bookmark-property-ado.md)|Indica un segnalibro che identifica in modo univoco il record corrente in un oggetto **Recordset** o imposta il record corrente in un oggetto **Recordset** sul record identificato da un segnalibro valido.|  
|[CacheSize](./cachesize-property-ado.md)|Indica il numero di record di un oggetto **Recordset** memorizzati nella cache localmente in memoria.|  
|[Capitolo](./chapter-property-ado.md)|Ottiene o imposta un oggetto OLE DB **capitolo** da/su un oggetto **ADORecordsetConstruction** .|  
|[CharSet](./charset-property-ado.md)|Indica il set di caratteri in cui deve essere convertito il contenuto di un **flusso** di testo.|  
|[CommandStream](./commandstream-property-ado.md)|Indica il flusso utilizzato come input per un oggetto **comando** .|  
|[CommandText](./commandtext-property-ado.md)|Indica il testo di un comando da emettere a fronte di un provider.|  
|[CommandTimeout](./commandtimeout-property-ado.md)|Indica il tempo di attesa durante l'esecuzione di un comando prima di terminare il tentativo e generare un errore.|  
|[CommandType](./commandtype-property-ado.md)|Indica il tipo di un oggetto **comando** .|  
|[Proprietà ConnectionString](./connectionstring-property-ado.md)|Indica le informazioni utilizzate per stabilire una connessione a un'origine dati.|  
|[ConnectionTimeout](./connectiontimeout-property-ado.md)|Indica il tempo di attesa durante il tentativo di stabilire una connessione prima di terminare il tentativo e generare un errore.|  
|[Count](./count-property-ado.md)|Indica il numero di oggetti in una raccolta.|  
|[CursorLocation](./cursorlocation-property-ado.md)|Indica la posizione del servizio del cursore.|  
|[CursorType](./cursortype-property-ado.md)|Indica il tipo di cursore utilizzato in un oggetto **Recordset** .|  
|[DataMember](./datamember-property.md)|Indica il nome del membro dati che verrà recuperato dall'oggetto a cui fa riferimento la proprietà **DataSource** .|  
|[DataSource](./datasource-property-ado.md)|Indica un oggetto che contiene i dati che devono essere rappresentati come oggetto **Recordset** .|  
|[DefaultDatabase](./defaultdatabase-property.md)|Indica il database predefinito per un oggetto **Connection** .|  
|[DefinedSize](./definedsize-property.md)|Indica la capacità dei dati di un oggetto **campo** .|  
|[Descrizione](./description-property.md)|Descrive un oggetto **Error** .|  
|[Sottolinguaggio](./dialect-property.md)|Indica la sintassi e le regole generali che il provider utilizzerà per analizzare le proprietà **CommandText** o **CommandStream** .|  
|[Direzione](./direction-property.md)|Indica se il **parametro** rappresenta un parametro di input, un parametro di output o entrambi o se il parametro è il valore restituito da un stored procedure.|  
|[EditMode](./editmode-property.md)|Indica lo stato di modifica del record corrente.|  
|[EOS](./eos-property.md)|Indica se la posizione corrente è alla fine del flusso.|  
|[Filter](./filter-property.md)|Indica un filtro per i dati in un **Recordset**.|  
|[HelpContext e fileguida](./helpcontext-helpfile-properties.md)|Indica il file della guida e l'argomento associato a un oggetto **Error** .<br /><br /> **HelpContextID** restituisce un ID di contesto, come valore **Long** , per un argomento in un file della guida.<br /><br /> FilePath restituisce un valore **stringa** **che restituisce un** percorso completamente risolto di un file della guida.|  
|[Index](./index-property.md)|Indica il nome dell'indice attualmente attivo per un oggetto **Recordset** .|  
|[IsolationLevel](./isolationlevel-property.md)|Indica il livello di isolamento per un oggetto **Connection** .|  
|[Item](./item-property-ado.md)|Indica un membro specifico di una raccolta, in base al nome o al numero ordinale.|  
|[LineSeparator](./lineseparator-property-ado.md)|Indica il carattere binario da utilizzare come separatore di riga negli oggetti del **flusso** di testo.|  
|[LockType](./locktype-property-ado.md)|Indica il tipo di blocchi inseriti nei record durante la modifica.|  
|[MarshalOptions](./marshaloptions-property-ado.md)|Indica i record di cui deve essere eseguito il marshalling sul server.|  
|[MaxRecords](./maxrecords-property-ado.md)|Indica il numero massimo di record da restituire a un **Recordset** da una query.|  
|[Modalità](./mode-property-ado.md)|Indica le autorizzazioni disponibili per la modifica dei dati in una **connessione**, un **record** o un oggetto **flusso** .|  
|[Nome](./name-property-ado.md)|Indica il nome di un oggetto.|  
|[NativeError](./nativeerror-property-ado.md)|Indica il codice di errore specifico del provider per un determinato oggetto **Error** .|  
|[Number](./number-property-ado.md)|Indica il numero che identifica in modo univoco un oggetto **Error** .|  
|[NumericScale](./numericscale-property-ado.md)|Indica la scala dei valori numerici in un **parametro** o in un oggetto **campo** .|  
|[OriginalValue](./originalvalue-property-ado.md)|Indica il valore di un **campo** esistente nel record prima che siano state apportate modifiche.|  
|[PageCount](./pagecount-property-ado.md)|Indica il numero di pagine di dati contenute nell'oggetto **Recordset** .|  
|[PageSize](./pagesize-property-ado.md)|Indica il numero di record che rappresentano una pagina nel **Recordset**.|  
|[ParentRow](./parentrow-property-ado.md)|Imposta il contenitore di un oggetto OLE DB **riga** su un oggetto **ADORecordConstruction** , in modo che l'elemento padre della riga venga trasformato in un oggetto **record** ADO.|  
|[ParentURL](./parenturl-property-ado.md)|Indica una stringa URL assoluta che punta al **record** padre dell'oggetto **record** corrente.|  
|[Position](./position-property-ado.md)|Indica la posizione corrente all'interno di un oggetto **flusso** .|  
|[Precisione](./precision-property-ado.md)|Indica il grado di precisione per i valori numerici in un oggetto **Parameter** o per gli oggetti **campo** numerico.|  
|[Prepared](./prepared-property-ado.md)|Indica se salvare una versione compilata di un comando prima dell'esecuzione.|  
|[Provider](./provider-property-ado.md)|Indica il nome del provider per un oggetto **Connection** .|  
|[RecordCount](./recordcount-property-ado.md)|Indica il numero di record in un oggetto **Recordset** .|  
|[RecordType](./recordtype-property-ado.md)|Indica il tipo di oggetto **record** .|  
|[Riga](./row-property-ado.md)|Ottiene o imposta un oggetto OLE DB **riga** da/in un oggetto **ADORecordConstruction** .|  
|[RowPosition](./rowposition-property-ado.md)|Ottiene o imposta un OLE DB oggetto **RowPosition** da/in un oggetto **ADORecordsetConstruction** .|  
|[Set di righe](./rowset-property-ado.md)|Ottiene o imposta un oggetto OLE DB **set di righe** da/in un oggetto **ADORecordsetConstruction** .|  
|[Origine (errore ADO)](./source-property-ado-error.md)|Indica il nome dell'oggetto o dell'applicazione che ha generato originariamente un errore.|  
|[Origine (record ADO)](./source-property-ado-record.md)|Indica l'entità rappresentata dall'oggetto **record** .|  
|[Origine (recordset ADO)](./source-property-ado-recordset.md)|Indica l'origine dei dati in un oggetto **Recordset** .|  
|[SQLState](./sqlstate-property.md)|Indica lo stato SQL per un oggetto **errore** specifico.|  
|[State](./state-property-ado.md)|Indica per tutti gli oggetti applicabili se lo stato dell'oggetto è aperto o chiuso. Indica per tutti gli oggetti applicabili che eseguono un metodo asincrono, se lo stato corrente dell'oggetto è la connessione, l'esecuzione o il recupero.|  
|[Stato (campo ADO)](./status-property-ado-field.md)|Indica lo stato di un oggetto **campo** .|  
|[Stato (recordset ADO)](./status-property-ado-recordset.md)|Indica lo stato del record corrente relativo agli aggiornamenti batch o ad altre operazioni bulk.|  
|[StayInSync](./stayinsync-property.md)|Indica, in un oggetto **Recordset** gerarchico, se il riferimento ai record figlio sottostanti (ovvero il *capitolo*) cambia quando la posizione della riga padre cambia.|  
|[Proprietà Stream](./stream-property.md)|Ottiene o imposta un oggetto OLE DB **flusso** da/in un oggetto **ADOStreamConstruction** .|  
|[Tipo](./type-property-ado.md)|Indica il tipo operativo o il tipo di dati di un **parametro**, un **campo** o un oggetto **proprietà** .|  
|[Tipo (flusso ADO)](./type-property-ado-stream.md)|Indica il tipo di dati contenuti nel **flusso** (binario o testo).|  
|[UnderlyingValue](./underlyingvalue-property.md)|Indica il valore corrente nel database per un oggetto **campo** .|  
|[Valore](./value-property-ado.md)|Indica il valore assegnato a un **campo**, un **parametro** o un oggetto **proprietà** .|  
|[Versione](./version-property-ado.md)|Indica il numero di versione ADO.|  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento all'API ADO](./ado-api-reference.md)   
 [Raccolte ADO](./ado-collections.md)   
 [Proprietà dinamiche ADO](./ado-dynamic-properties.md)   
 [Costanti enumerate ADO](./ado-enumerated-constants.md)   
 [Appendice B: errori ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventi ADO](./ado-events.md)   
 [Metodi ADO](./ado-methods.md)   
 [Modello a oggetti ADO](./ado-object-model.md)   
 [Interfacce e oggetti ADO](./ado-objects-and-interfaces.md)