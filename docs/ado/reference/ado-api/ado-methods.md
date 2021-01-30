---
description: Metodi ADO
title: Metodi ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO, methods
- methods [ADO]
ms.assetid: a38c5670-ba28-44f3-bd5b-fcb46880e904
author: rothja
ms.author: jroth
ms.openlocfilehash: dd98478cad7c13b2f4e6950586c1317c1d5d37dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164905"
---
# <a name="ado-methods"></a>Metodi ADO

|Metodo|Descrizione|  
|-|-|  
|[AddNew](./addnew-method-ado.md)|Crea un nuovo record per un oggetto **Recordset** aggiornabile.|  
|[Accoda](./append-method-ado.md)|Accoda un oggetto a una raccolta. Se la raccolta è **Fields**, è possibile creare un nuovo oggetto **campo** prima che venga aggiunto alla raccolta.|  
|[AppendChunk](./appendchunk-method-ado.md)|Accoda i dati a un **campo** di dati di testo o binario di grandi dimensioni o a un oggetto **Parameter** .|  
|[BeginTrans, CommitTrans e RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md)|Gestisce l'elaborazione delle transazioni all'interno di un oggetto **connessione** come indicato di seguito:<br /><br /> **BeginTrans** : inizia una nuova transazione.<br /><br /> **CommitTrans** -Salva le modifiche e termina la transazione corrente. Potrebbe inoltre avviare una nuova transazione.<br /><br /> **RollbackTrans** : Annulla tutte le modifiche e termina la transazione corrente. Potrebbe inoltre avviare una nuova transazione.|  
|[Annulla](./cancel-method-ado.md)|Annulla l'esecuzione di una chiamata al metodo asincrona in sospeso.|  
|[CancelBatch](./cancelbatch-method-ado.md)|Annulla un aggiornamento batch in sospeso.|  
|[CancelUpdate](./cancelupdate-method-ado.md)|Annulla tutte le modifiche apportate alla riga corrente o nuova di un oggetto **Recordset** oppure alla raccolta **Fields** di un oggetto **record** , prima di chiamare il metodo **Update** .|  
|[Cancella](./clear-method-ado.md)|Rimuove tutti gli oggetti **Error** dalla raccolta **Errors** .|  
|[Clone](./clone-method-ado.md)|Crea un oggetto **Recordset** duplicato da un oggetto **Recordset** esistente. Facoltativamente, specifica che il clone deve essere di sola lettura.|  
|[Close](./close-method-ado.md)|Chiude un oggetto aperto e tutti gli oggetti dipendenti.|  
|[CompareBookmarks](./comparebookmarks-method-ado.md)|Confronta due segnalibri e restituisce un'indicazione dei valori relativi.|  
|[CopyRecord](./copyrecord-method-ado.md)|Copia un file o una directory e il relativo contenuto in un'altra posizione.|  
|[CopyTo](./copyto-method-ado.md)|Copia il numero specificato di caratteri o byte (a seconda del **tipo**) nel **flusso** in un altro oggetto **flusso** .|  
|[CreateParameter](./createparameter-method-ado.md)|Crea un nuovo oggetto **Parameter** con le proprietà specificate.|  
|[Delete (raccolta di parametri ADO)](./delete-method-ado-parameters-collection.md)|Elimina un oggetto dalla raccolta di **parametri** .|  
|[Delete (raccolta di campi ADO)](./delete-method-ado-fields-collection.md)|Elimina un oggetto dalla raccolta di **campi** .|  
|[Delete (recordset ADO)](./delete-method-ado-recordset.md)|Elimina il record corrente o un gruppo di record.|  
|[DeleteRecord](./deleterecord-method-ado.md)|Elimina un file o una directory e tutte le relative sottodirectory.|  
|[Execute (comando ADO)](./execute-method-ado-command.md)|Esegue la query, l'istruzione SQL o stored procedure specificata nella proprietà **CommandText** .|  
|[Execute (connessione ADO)](./execute-method-ado-connection.md)|Esegue la query, l'istruzione SQL, il stored procedure o il testo specifico del provider specificato.|  
|[Find](./find-method-ado.md)|Esegue la ricerca di un **Recordset** per la riga che soddisfa i criteri specificati.|  
|[Svuotamento](./flush-method-ado.md)|Impone il contenuto del **flusso** rimanente nel buffer ADO all'oggetto sottostante a cui è associato il **flusso** .|  
|[Metodo get_OLEDBCommand](./get-oledbcommand-method.md)|Restituisce il comando OLEDB sottostante, propagando prima di tutto le informazioni sui parametri impostate nel comando ADO al comando OLEDB.|  
|[GetChildren](./getchildren-method-ado.md)|Restituisce un **Recordset** le cui righe rappresentano i file e le sottodirectory della directory rappresentata da questo **record**.|  
|[GetChunk](./getchunk-method-ado.md)|Restituisce tutto o una parte di, il contenuto di un oggetto **campo** di dati di testo o binario di grandi dimensioni.|  
|[Metodo GetDataProviderDSO](./getdataproviderdso-method.md)|Recupera l'oggetto origine dati OLEDB sottostante dal provider di forme.|  
|[GetRows](./getrows-method-ado.md)|Recupera più record di un oggetto **Recordset** in una matrice.|  
|[GetString](./getstring-method-ado.md)|Restituisce il **Recordset** sotto forma di stringa.|  
|[LoadFromFile](./loadfromfile-method-ado.md)|Carica il contenuto di un file esistente in un **flusso**.|  
|[Sposta](./move-method-ado.md)|Sposta la posizione del record corrente in un oggetto **Recordset** .|  
|[MoveFirst, MoveLast, MoveNext e MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Passa al record primo, ultimo, successivo o precedente in un oggetto **Recordset** specificato e fa in modo che registri il record corrente.|  
|[MoveRecord](./moverecord-method-ado.md)|Sposta un file o una directory e il relativo contenuto in un'altra posizione.|  
|[NextRecordset](./nextrecordset-method-ado.md)|Cancella l'oggetto **Recordset** corrente e restituisce il **Recordset** successivo avanzando attraverso una serie di comandi.|  
|[Apri (connessione ADO)](./open-method-ado-connection.md)|Apre una connessione a un'origine dati.|  
|[Apri (record ADO)](./open-method-ado-record.md)|Apre un oggetto **record** esistente o crea un nuovo file o directory.|  
|[Open (recordset ADO)](./open-method-ado-recordset.md)|Apre un cursore.|  
|[Apri (flusso ADO)](./open-method-ado-stream.md)|Apre un oggetto **Stream** per modificare i flussi di dati binari o di testo.|  
|[OpenSchema](./openschema-method.md)|Ottiene le informazioni sullo schema del database dal provider.|  
|[Metodo put_OLEDBCommand](./put-oledbcommand-method.md)|Questo metodo non esegue alcuna operazione. restituisce sempre S_OK.|  
|[Lettura](./read-method.md)|Legge un numero specificato di byte da un oggetto **flusso** .|  
|[ReadText](./readtext-method.md)|Legge un numero specificato di caratteri da un oggetto **flusso** di testo.|  
|[Aggiorna](./refresh-method-ado.md)|Aggiorna gli oggetti di una raccolta in modo da riflettere gli oggetti disponibili e specifici del provider.|  
|[Requery](./requery-method.md)|Aggiorna i dati in un oggetto **Recordset** eseguendo nuovamente la query su cui è basato l'oggetto.|  
|[Risincronizza](./resync-method.md)|Aggiorna i dati nell'oggetto **Recordset** corrente o nella raccolta di **campi** di un oggetto **record** dal database sottostante.|  
|[Salva](./save-method.md)|Salva il **Recordset** in un oggetto file o **flusso** .|  
|[SaveToFile](./savetofile-method.md)|Salva il contenuto binario di un **flusso** in un file.|  
|[Seek](./seek-method.md)|Esegue una ricerca nell'indice di un **Recordset** per individuare rapidamente la riga che corrisponde ai valori specificati e imposta la posizione della riga corrente su quella riga.|  
|[SetEOS](./seteos-method.md)|Imposta la posizione che rappresenta la fine del flusso.|  
|[SkipLine](./skipline-method.md)|Ignora un'intera riga durante la lettura di un flusso di testo.|  
|[Stat](./stat-method.md)|Ottiene informazioni statistiche su un flusso aperto.|  
|[Supporti](./supports-method.md)|Determina se un oggetto **Recordset** specificato supporta un particolare tipo di funzionalità.|  
|[Aggiornamento](./update-method.md)|Salva le modifiche apportate alla riga corrente di un oggetto **Recordset** o alla raccolta di **campi** di un oggetto **record** .|  
|[UpdateBatch](./updatebatch-method.md)|Scrive tutti gli aggiornamenti batch in sospeso sul disco.|  
|[Scrittura](./write-method.md)|Scrive dati binari in un oggetto **flusso** .|  
|[WriteText](./writetext-method.md)|Scrive una stringa di testo specificata in un oggetto **flusso** .|  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento all'API ADO](./ado-api-reference.md)   
 [Raccolte ADO](./ado-collections.md)   
 [Proprietà dinamiche ADO](./ado-dynamic-properties.md)   
 [Costanti enumerate ADO](./ado-enumerated-constants.md)   
 [Appendice B: errori ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Eventi ADO](./ado-events.md)   
 [Modello a oggetti ADO](./ado-object-model.md)   
 [Oggetti e interfacce ADO](./ado-objects-and-interfaces.md)   
 [Proprietà ADO](./ado-properties.md)