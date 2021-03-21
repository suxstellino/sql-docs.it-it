---
title: Uso di tipi di dati XML | Microsoft Docs
description: Informazioni sui tipi di dati XML con OLE DB Driver per SQL Server, che consentono di archiviare documenti e frammenti XML in un database SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- IRowsetChange interface
- IRowsetUpdate interface
- data access [OLE DB Driver for SQL Server], xml data type
- OLE DB Driver for SQL Server schema rowsets
- PROVIDER_TYPES rowset
- IColumnsInfo interface
- IRowsetFind interface
- IColumnsRowset interface
- PROCEDURE_PARAMETERS rowset
- MSOLEDBSQL, XML
- xml data type [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, XML
- IRowset interface
- ISequentialStream interface
- ISSCommandWithParameters interface
- SS_XMLSCHEMA rowset
- OLE DB Driver for SQL Server OLE DB interfaces
- XML [SQL Server], OLE DB Driver for SQL Server
- COLUMNS rowset
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 087429db4c42793799eb1abb7a08ab4267b1afa0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742431"
---
# <a name="using-xml-data-types"></a>Utilizzo di tipi di dati XML
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] è stato introdotto il tipo di dati **xml** che consente di archiviare documenti e frammenti XML in un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il tipo di dati **xml**,un tipo di dati predefinito di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], è simile per alcuni aspetti ad altri tipi predefiniti, quali **int** e **varchar**. Come per gli altri tipi predefiniti, è possibile usare il tipo di dati **xml** come tipo di colonna quando si crea una tabella, come tipo di variabile, tipo di parametro o tipo restituito dalla funzione oppure in funzioni CAST e CONVERT.  
  
## <a name="programming-considerations"></a>Considerazioni sulla programmazione  
 XML può essere autodescrittivo in quanto può eventualmente includere un'intestazione XML che specifica la codifica del documento, ad esempio:  
  
 `<?xml version="1.0" encoding="windows-1252"?><doc/>`  
  
 Lo standard XML descrive il modo in cui un processore XML può rilevare la codifica utilizzata per un documento esaminando i primi byte del documento. È possibile che la codifica specificata nell'applicazione crei conflitti con quella specificata nel documento. Poiché per i documenti passati come parametri associati i dati XML vengono considerati dati binari in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], non viene eseguita alcuna conversione e il parser XML può utilizzare senza problemi la codifica specificata nel documento. Per i dati XML associati come WSTR, tuttavia, l'applicazione deve quindi garantire che il documento venga codificato come Unicode. Questo scenario può comportare il caricamento del documento in un modello DOM, impostando la codifica su Unicode e serializzando il documento. Se questo passaggio non viene eseguito, possono verificarsi conversioni dei dati che producono dati XML non validi o danneggiati.  
  
 Vi è inoltre il rischio di conflitti quando XML viene specificato in valori letterali. I dati seguenti, ad esempio, non sono validi:  
  
 `INSERT INTO xmltable(xmlcol) VALUES('<?xml version="1.0" encoding="UTF-16"?><doc/>')`  
  
 `INSERT INTO xmltable(xmlcol) VALUES(N'<?xml version="1.0" encoding="UTF-8"?><doc/>')`  
  
## <a name="ole-db-driver-for-sql-server"></a>Driver OLE DB per SQL Server 
 DBTYPE_XML è un nuovo tipo di dati specifico di XML in OLE DB Driver per SQL Server. È inoltre possibile accedere ai dati XML tramite i tipi OLE DB esistenti DBTYPE_BYTES, DBTYPE_WSTR, DBTYPE_BSTR, DBTYPE_XML, DBTYPE_STR, DBTYPE_VARIANT e DBTYPE_IUNKNOWN. I dati archiviati in colonne di tipo XML possono essere recuperati da una colonna in un set di righe del driver OLE DB per SQL Server nei formati seguenti:  
  
-   Stringa di testo  
  
-   **ISequentialStream**  
  
> [!NOTE]  
>  Il driver OLE DB per SQL Server non include un lettore SAX, ma l'interfaccia **ISequentialStream** può essere passata in modo semplice a oggetti SAX e DOM in MSXML.  
  
 Per il recupero di documenti XML di dimensioni elevate, è necessario usare **ISequentialStream**. A XML si applicano le stesse tecniche utilizzate per altri tipi di valore di grandi dimensioni. Per altre informazioni, vedere la sezione [Uso di tipi valore di grandi dimensioni](../../oledb/features/using-large-value-types.md).  
  
 I dati in colonne di tipo XML in un set di righe possono inoltre essere recuperati, inseriti o aggiornati da un'applicazione tramite le normali interfacce, ad esempio **IRow::GetColumns**, **IRowChange::SetColumns** e **ICommand::Execute**. Analogamente al caso del recupero, un'applicazione può passare una stringa di testo o un'interfaccia **ISequentialStream** al driver OLE DB per SQL Server.  
  
> [!NOTE]  
>  Per inviare dati XML in formato stringa tramite l'interfaccia **ISequentialStream**, è necessario ottenere **ISequentialStream** specificando DBTYPE_IUNKNOWN e impostare il relativo argomento *pObject* su Null nell'associazione.  
  
 Quando i dati XML recuperati risultano troncati a causa di un buffer del consumer di dimensioni troppo ridotte, è possibile che la lunghezza venga restituita come 0xffffffff, a indicare che non è nota. Questo comportamento è coerente con l'implementazione come tipo di dati trasmesso al client senza inviare informazioni sulla lunghezza dei dati effettivi. In alcuni casi è possibile che venga restituita la lunghezza effettiva quando il provider ha memorizzato nel buffer il valore intero, ad esempio **IRowset::GetData**, e laddove venga eseguita la conversione dei dati.  
  
 I dati XML inviati a SQL Server vengono considerati dati binari dal server. Questo comportamento impedisce il verificarsi di qualsiasi conversione e consente al parser XML di rilevare automaticamente la codifica XML. Di conseguenza, una più ampia gamma di documenti XML, ad esempio quelli codificati in UTF-8, viene accettata come input in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Se i dati XML di input vengono associati come DBTYPE_WSTR, l'applicazione deve verificare che i dati siano già codificati come Unicode per evitare qualsiasi rischio di danni dovuti a conversioni di dati indesiderate.  
  
### <a name="data-bindings-and-coercions"></a>Associazione dati e coercizioni  
 Nella tabella seguente vengono descritte l'associazione e la coercizione che si verificano quando si usano i tipi di dati elencati con il tipo di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]xml **di**.  
  
|Tipo di dati|Al server<br /><br /> **XML**|Al server<br /><br /> **Non XML**|Dal server<br /><br /> **XML**|Dal server<br /><br /> **Non XML**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_XML|Pass-through<sup>6,7</sup>|Errore<sup>1</sup>|OK<sup>11,6</sup>|Errore<sup>8</sup>|  
|DBTYPE_BYTES|Pass-through<sup>6,7</sup>|N/D<sup>2</sup>|OK <sup>11, 6</sup>|N/D <sup>2</sup>|  
|DBTYPE_WSTR|Pass-through<sup>6,10</sup>|N/D <sup>2</sup>|OK<sup>4, 6, 12</sup>|N/D <sup>2</sup>|  
|DBTYPE_BSTR|Pass-through<sup>6,10</sup>|N/D <sup>2</sup>|OK <sup>3</sup>|N/D <sup>2</sup>|  
|DBTYPE_STR|OK<sup>6, 9, 10</sup>|N/D <sup>2</sup>|OK<sup>5, 6, 12</sup>|N/D <sup>2</sup>|  
|DBTYPE_IUNKNOWN|Flusso di byte tramite **ISequentialStream**<sup>7</sup>|N/D <sup>2</sup>|Flusso di byte tramite **ISequentialStream**<sup>11</sup>|N/D <sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Pass-through<sup>6,7</sup>|N/D <sup>2</sup>|N/D|N/D <sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|Pass-through<sup>6,10</sup>|N/D <sup>2</sup>|OK<sup>3</sup>|N/D <sup>2</sup>|  
  
 <sup>1</sup>Se un tipo di server diverso da DBTYPE_XML è specificato con **ICommandWithParameters::SetParameterInfo** e il tipo di funzione di accesso è DBTYPE_XML, si verifica un errore quando viene eseguita l'istruzione (DB_E_ERRORSOCCURRED, lo stato del parametro è DBSTATUS_E_BADACCESSOR). In caso contrario, i dati vengono inviati al server, ma il server restituisce un errore indicante che non è disponibile alcuna conversione implicita da XML al tipo di dati del parametro.  
  
 <sup>2</sup>Esula dall'ambito di questo argomento.  
  
 <sup>3</sup>Il formato è UTF-16, nessun byte order mark (BOM), nessuna specifica di codifica, nessuna terminazione Null.  
  
 <sup>4</sup>Il formato è UTF-16, nessun BOM, nessuna specifica di codifica, terminazione Null.  
  
 <sup>5</sup>Il formato è caratteri multibyte codificati nella tabella codici del client con terminatore Null. Poiché la conversione dal formato Unicode fornito dal server può provocare danni ai dati, questa associazione è sconsigliata.  
  
 <sup>6</sup>Può essere usato BY_REF.  
  
 <sup>7</sup>I dati UTF-16 devono iniziare con un indicatore BOM. In caso contrario, la codifica potrebbe non essere riconosciuta correttamente dal server.  
  
 <sup>8</sup>La convalida può verificarsi in fase di creazione della funzione di accesso o in fase di recupero. L'errore è DB_E_ERRORSOCCURRED, lo stato dell'associazione è impostato su DBBINDSTATUS_UNSUPPORTEDCONVERSION.  
  
 <sup>9</sup>I dati vengono convertiti in Unicode usando la tabella codici del client prima di inviarli al server. Se la codifica del documento non corrisponde alla tabella codici del client, i dati possono risultare danneggiati e, pertanto, questa associazione è fortemente sconsigliata.  
  
 <sup>10</sup>Ai dati inviati al server viene sempre aggiunto un indicatore BOM (Byte Order Mark). Se i dati iniziano già con un indicatore BOM (Byte Order Mark), saranno presenti due indicatori BOM all'inizio del buffer. Il server utilizza il primo indicatore dell'ordine dei byte per riconoscere la codifica come UTF-16, quindi lo ignora. Il secondo indicatore dell'ordine dei byte viene interpretato come spazio unificatore di larghezza zero.  
  
 <sup>11</sup>Il formato è UTF-16, nessuna specifica di codifica, BOM aggiunto ai dati ricevuti dal server. Se dal server viene restituita una stringa vuota, all'applicazione viene comunque restituito un indicatore dell'ordine dei byte. Se la lunghezza di buffer è un numero dispari di byte, i dati vengono troncati correttamente. Se il valore intero viene restituito in blocchi, questi possono essere concatenati per ricostituire il valore corretto.  
  
 <sup>12</sup>Se la lunghezza di buffer è minore di due caratteri, ovvero non vi è spazio sufficiente per la terminazione Null, viene segnalato un errore di overflow.  
  
> [!NOTE]  
>  Per i valori XML NULL non viene restituito alcun dato.  
  
 Lo standard XML richiede che i dati XML con codifica UTF-16 inizino con un indicatore dell'ordine dei byte, codice di carattere UTF-16 0xFEFF. Quando si usano associazioni WSTR e BSTR, nel driver OLE DB per SQL Server non è necessario né viene aggiunto un BOM in quanto la codifica viene definita in modo implicito dall'associazione. Quando si utilizzano associazioni BYTES, XML o IUNKNOWN, lo scopo è semplicità di gestione di altri processori XML e sistemi di archiviazione. In questo caso, con i dati XML con codifica UTF-16 deve essere presente un BOM e l'applicazione non deve considerare l'effettiva codifica, in quando la maggior parte dei processori XML, incluso SQL Server, deduce la codifica controllando i primi byte del valore. I dati XML ricevuti dal driver OLE DB per SQL Server usando associazioni BYTES, XML o IUNKNOWN sono sempre codificati in UTF-16 con un BOM e senza una dichiarazione di codifica incorporata.  
  
 Le conversioni dei dati fornite dai servizi OLE DB (**IDataConvert**) principali non sono applicabili a DBTYPE_XML.  
  
 La convalida viene eseguita durante l'invio dei dati al server. La convalida lato client e le modifiche di codifica devono essere gestite dall'applicazione. È consigliabile non elaborare direttamente i dati XML, ma di usare invece un lettore DOM o SAX per elaborarli.  
  
 DBTYPE_NULL e DBTYPE_EMPTY possono essere associati per i parametri di input ma non per i parametri di output o per i risultati. Se vengono associati per i parametri di input, lo stato deve essere impostato su DBSTATUS_S_ISNULL o DBSTATUS_S_DEFAULT.  
  
 DBTYPE_XML può essere convertito in DBTYPE_EMPTY e DBTYPE_NULL, DBTYPE_EMPTY può essere convertito in DBTYPE_XML, ma DBTYPE_NULL non può essere convertito in DBTYPE_XML. Questo comportamento è coerente con DBTYPE_WSTR.  
  
 Come illustrato nella tabella precedente, DBTYPE_IUNKNOWN è un'associazione supportata, ma non è disponibile alcuna conversione tra DBTYPE_XML e DBTYPE_IUNKNOWN. DBTYPE_IUNKNOWN non può essere utilizzato con DBTYPE_BYREF.  
  
### <a name="ole-db-rowset-additions-and-changes"></a>Aggiunte e modifiche ai set di righe OLE DB  
 In OLE DB Driver per SQL Server sono stati aggiunti nuovi valori o modifiche a molti set di righe dello schema OLE DB principale.  
  
#### <a name="the-columns-and-procedure_parameters-schema-rowsets"></a>Set di righe dello schema COLUMNS e PROCEDURE_PARAMETERS  
 Tra le aggiunte ai set di righe dello schema COLUMNS e PROCEDURE_PARAMETERS sono incluse le colonne seguenti:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Nome di un catalogo in cui viene definita una raccolta di XML Schema. NULL per una colonna non XML o una colonna XML non tipizzata.|  
|SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Nome di uno schema in cui viene definita una raccolta di XML Schema. NULL per una colonna non XML o una colonna XML non tipizzata.|  
|SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nome della raccolta di XML Schema. NULL per una colonna non XML o una colonna XML non tipizzata.|  
  
#### <a name="the-provider_types-schema-rowset"></a>Set di righe dello schema PROVIDER_TYPES  
 Nel set di righe dello schema PROVIDER_TYPES il valore di COLUMN_SIZE è 0 per il tipo di dati **xml** e DATA_TYPE è DBTYPE_XML.  
  
#### <a name="the-ss_xmlschema-schema-rowset"></a>Set di righe dello schema SS_XMLSCHEMA  
 È stato introdotto un nuovo set di righe dello schema per i client, denominato SS_XMLSCHEMA, che consente il recupero di informazioni su XML Schema. Il set di righe SS_XMLSCHEMA contiene le colonne seguenti:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Catalogo cui appartiene una raccolta XML.|  
|SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Schema cui appartiene una raccolta XML.|  
|SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nome di una raccolta di XML Schema per colonne XML tipizzate. In caso contrario, NULL.|  
|TARGETNAMESPACEURI|DBTYPE_WSTR|Spazio dei nomi di destinazione di un elemento XML Schema.|  
|SCHEMACONTENT|DBTYPE_WSTR|Contenuto dell'elemento XML Schema.|  
  
 L'ambito di ogni XML Schema è costituito da nome del catalogo, nome dello schema, nome della raccolta di schemi e URI dello spazio dei nomi di destinazione. Viene inoltre definito un nuovo GUID con il nome DBSCHEMA_XML_COLLECTIONS. Il numero di restrizioni e di colonne limitate per il set di righe dello schema SS_XMLSCHEMA viene definito come segue.  
  
|GUID|Numero di restrizioni|Colonne limitate|  
|----------|----------------------------|------------------------|  
|DBSCHEMA_XML_COLLECTIONS|4|SCHEMACOLLECTION_CATALOGNAME<br /><br /> SCHEMACOLLECTION_SCHEMANAME<br /><br /> SCHEMACOLLECTIONNAME<br /><br /> TARGETNAMESPACEURI|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>Aggiunte e modifiche ai set di proprietà OLE DB  
 In OLE DB Driver per SQL Server sono stati aggiunti nuovi valori o modifiche a molti set di proprietà dello schema OLE DB principale.  
  
#### <a name="the-dbpropset_sqlserverparameter-property-set"></a>Set di proprietà DBPROPSET_SQLSERVERPARAMETER  
 Ai fini del supporto del tipo di dati **xml** tramite OLE DB, nel driver OLE DB per SQL Server è stato implementato il nuovo set di proprietà DBPROPSET_SQLSERVERPARAMETER, che contiene i valori seguenti.  
  
|Nome|Tipo|Descrizione|  
|----------|----------|-----------------|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Nome di un catalogo (database) in cui viene definita una raccolta di XML Schema. Una delle tre parti di cui è composto l'identificatore del nome SQL.|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Nome di un elemento XML Schema all'interno della raccolta di schemi. Una delle tre parti di cui è composto l'identificatore del nome SQL.|  
|SSPROP_PARAM_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nome della raccolta di XML Schema all'interno del catalogo. Una delle tre parti di cui è composto l'identificatore del nome SQL.|  
  
#### <a name="the-dbpropset_sqlservercolumn-property-set"></a>Set di proprietà DBPROPSET_SQLSERVERCOLUMN  
 Ai fini del supporto della creazione di tabelle nell'interfaccia **ITableDefinition**, nel driver OLE DB per SQL Server sono state aggiunte tre nuove colonne al set di proprietà DBPROPSET_SQLSERVERCOLUMN.  
  
|Nome|Tipo|Descrizione|  
|----------|----------|-----------------|  
|SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME|VT_BSTR|Per le colonne XML tipizzate, questa proprietà è una stringa che specifica il nome del catalogo in cui viene archiviato l'elemento XML Schema. Per gli altri tipi di colonna questa proprietà restituisce una stringa vuota.|  
|SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME|VT_BSTR|Per le colonne XML tipizzate, questa proprietà è una stringa che specifica il nome dell'elemento XML Schema che definisce la colonna.|  
|SSPROP_COL_XML_SCHEMACOLLECTIONNAME|VT_BSTR|Per le colonne XML tipizzate, questa proprietà è una stringa che specifica il nome della raccolta di XML Schema che definisce il valore.|  
  
 Analogamente ai valori di SSPROP_PARAM, tutte queste proprietà sono facoltative e non sono specificate per impostazione predefinita. SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME e SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME possono essere specificate solo se si specifica SSPROP_COL_XML_SCHEMACOLLECTIONNAME. Se quando si passano dati XML al server questi valori sono inclusi, ne viene verificata l'esistenza (validità) rispetto al database corrente e i dati dell'istanza vengono controllati rispetto allo schema. In tutti i casi, per essere validi tali valori devono essere tutti vuoti o specificati.  
  
### <a name="ole-db-interface-additions-and-changes"></a>Aggiunte e modifiche alle interfacce OLE DB  
 In OLE DB Driver per SQL Server sono stati aggiunti nuovi valori o modifiche a molte interfacce dello schema OLE DB principale.  
  
#### <a name="the-isscommandwithparameters-interface"></a>Interfaccia ISSCommandWithParameters  
 Ai fini del supporto del tipo di dati **xml** tramite OLE DB, nel driver OLE DB per SQL Server sono state implementate alcune modifiche, ad esempio l'aggiunta dell'interfaccia [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md). Questa nuova interfaccia eredita dall'interfaccia OLE DB principale **ICommandWithParameters**. Oltre ai tre metodi ereditati da **ICommandWithParameters**; **GetParameterInfo**, **MapParameterNames** e **SetParameterInfo**; **ISSCommandWithParameters** fornisce i metodi [GetParameterProperties](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md) e [SetParameterProperties](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) usati per gestire i tipi di dati specifici del server.  
  
> [!NOTE]  
>  L'interfaccia **ISSCommandWithParameters** usa anche la nuova struttura SSPARAMPROPS.  
  
#### <a name="the-icolumnsrowset-interface"></a>Interfaccia IColumnsRowset  
 Nel driver OLE DB per SQL Server sono state aggiunte le colonne specifiche di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seguenti al set di righe restituito dal metodo **IColumnRowset::GetColumnsRowset**. Tali colonne contengono il nome in tre parti di una raccolta di XML Schema. Per le colonne non XML o per le colonne XML non tipizzate, le tre colonne assumono tutte il valore predefinito NULL.  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Catalogo cui appartiene una raccolta di XML Schema.<br /><br /> In caso contrario, NULL.|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Schema cui appartiene una raccolta di XML Schema. In caso contrario, NULL.|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Nome della raccolta di XML Schema per le colonne XML tipizzate. In caso contrario, NULL.|  
  
#### <a name="the-irowset-interface"></a>Interfaccia IRowset  
 Un'istanza XML in una colonna XML viene recuperata tramite il metodo **IRowset::GetData**. A seconda dell'associazione specificata dal client, un'istanza XML può essere recuperata come DBTYPE_BSTR, DBTYPE_WSTR DBTYPE_VARIANT, DBTYPE_XML, DBTYPE_STR, DBTYPE_BYTES o come interfaccia tramite DBTYPE_IUNKNOWN. Se il consumer specifica DBTYPE_BSTR, DBTYPE_WSTR o DBTYPE_VARIANT, il provider converte l'istanza XML nel tipo richiesto dall'utente e la archivia nella posizione specificata nell'associazione corrispondente.  
  
 Se il consumer specifica DBTYPE_IUNKNOWN e imposta l'argomento *pObject* su NULL oppure imposta l'argomento *pObject* su IID_ISequentialStream, il provider restituisce un'interfaccia **ISequentialStream** all'utente in modo che questi possa eseguire il flusso dei dati XML all'esterno della colonna. **ISequentialStream** restituisce quindi i dati XML come flusso di caratteri Unicode.  
  
 Quando viene restituito un valore XML associato a DBTYPE_IUNKNOWN, il provider segnala un valore di dimensioni `sizeof (IUnknown *)`. Questo comportamento è coerente con l'approccio adottato quando una colonna è associata come DBTYPE_IUnknown o DBTYPE_IDISPATCH e tramite DBTYPE_IUNKNOWN/ISequentialStream quando non è possibile determinare con esattezza le dimensioni della colonna.  
  
#### <a name="the-irowsetchange-interface"></a>Interfaccia IRowsetChange  
 Il consumer può aggiornare un'istanza XML in una colonna utilizzando due modalità diverse. La prima consiste nell'usare l'oggetto **ISequentialStream** creato dal provider. Il consumer può chiamare il metodo **ISequentialStream::Write** per aggiornare direttamente l'istanza XML restituita dal provider.  
  
 Il secondo approccio consiste nell'usare il metodo **IRowsetChange::SetData** o **IRowsetChange::InsertRow**. In questo approccio è possibile specificare un'istanza XML nel buffer del consumer in un'associazione di tipo DBTYPE_BSTR, DBTYPE_WSTR, DBTYPE_VARIANT, DBTYPE_XML o DBTYPE_IUNKNOWN.  
  
 Se viene specificato il tipo DBTYPE_BSTR, DBTYPE_WSTR o DBTYPE_VARIANT, il provider archivia l'istanza XML che si trova nel buffer del consumer nella colonna appropriata.  
  
 Se viene specificato DBTYPE_IUNKNOWN/ISequentialStream e il consumer non specifica alcun oggetto di archiviazione, il consumer deve creare un oggetto **ISequentialStream** in anticipo, associare il documento XML all'oggetto, quindi passare l'oggetto al provider tramite il metodo **IRowsetChange::SetData**. Il consumer può creare inoltre un oggetto di archiviazione, impostare l'argomento pObject su IID_ISequentialStream, creare un oggetto **ISequentialStream**, quindi passare l'oggetto **ISequentialStream** al metodo **IRowsetChange::SetData**. In entrambi casi, il provider può recuperare l'oggetto XML tramite l'oggetto **ISequentialStream** e può inserirlo in una colonna appropriata.  
  
#### <a name="the-irowsetupdate-interface"></a>Interfaccia IRowsetUpdate  
 L'interfaccia **IRowsetUpdate** fornisce una funzionalità per aggiornamenti ritardati. I dati resi disponibili per i set di righe non vengono resi disponibili per le altre transazioni fino a quando il consumer non chiama il metodo **IRowsetUpdate::Update**.  
  
#### <a name="the-irowsetfind-interface"></a>Interfaccia IRowsetFind  
 Non è possibile usare il metodo **IRowsetFind::FindNextRow** con il tipo di dati **xml**. Quando **IRowsetFind::FindNextRow** viene chiamato e l'argomento *hAccessor* specifica una colonna di tipo DBTYPE_XML, viene restituito DB_E_BADBINDINFO. Ciò si verifica indipendentemente dal tipo di colonna di cui si esegue la ricerca. Per qualsiasi altro tipo di associazione, **FindNextRow** ha esito negativo con DB_E_BADCOMPAREOP se la colonna da cercare è del tipo di dati **xml**.  
 
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per funzionalità di SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
