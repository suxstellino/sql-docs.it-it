---
title: Supporto dei tipi di parametri con valori di tabella OLE DB (proprietà) | Microsoft Docs
description: Informazioni sulle proprietà e gli insiemi di proprietà di OLE DB associati agli oggetti set di righe di parametri con valori di tabella in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (OLE DB), API support (properties)
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7dc83e9b9cadf8d0569e1a26e852c7b4525e56c6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750831"
---
# <a name="ole-db-table-valued-parameter-type-support-properties"></a>Supporto dei tipi di parametri con valori di tabella OLE DB (proprietà)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In questo argomento sono incluse informazioni sulle proprietà e i set di proprietà OLE DB associati a oggetti set di righe di parametri con valori di tabella.  
  
## <a name="properties"></a>Proprietà  
 Di seguito viene specificato l'elenco delle proprietà esposte tramite il metodo IRowsetInfo::GetProperties sugli oggetti set di righe di parametri con valori di tabella. Si noti che tutte le proprietà dei set di righe di parametri con valori di tabella sono di sola lettura. Pertanto, il tentativo di impostare una delle proprietà tramite i metodi IOpenRowset:: OpenRowset o ITableDefinitionWithConstraints:: CreateTableWithConstraints sui rispettivi valori non predefiniti comporterà un errore e non verrà creato alcun oggetto.  
  
 Le proprietà non implementate nell'oggetto set di righe di parametri con valori di tabella non sono incluse nell'elenco. Per un elenco completo delle proprietà, vedere la documentazione di OLE DB in Windows Data Access Components.  
  
|ID proprietà|valore|  
|-----------------|-----------|  
|DBPROP_ABORTPRESERVE|VARIANT_TRUE|  
|DBPROP_ACCESSORDER|DBPROPVAL_AO_RANDOM|  
|DBPROP_BLOCKINGSTORAGEOBJECTS|VARIANT_TRUE|  
|DBPROP_BOOKMARKS<br /><br /> DBPROP_LITERALBOOKMARKS|L/S: Sola lettura<br /><br /> Impostazione predefinita: VARIANT_FALSE<br /><br /> Descrizione: i segnalibri non sono consentiti negli oggetti set di righe di parametri con valori di tabella.|  
|DBPROP_BOOKMARKSKIPPED|VARIANT_FALSE|  
|DBPROP_BOOKMARKTYPE|DBPROPVAL_BMK_NUMERIC|  
|DBPROP_CANHOLDROWS|VARIANT_FALSE|  
|DBPROP_CHANGEINSERTEDROWS|VARIANT_TRUE|  
|DBPROP_COLUMNRESTRICT|VARIANT_FALSE|  
|DBPROP_COMMANDTIMEOUT|0|  
|DBPROP_COMMITPRESERVE|VARIANT_TRUE|  
|DBPROP_DEFERRED|VARIANT_FALSE|  
|DBPROP_DELAYSTORAGEOBJECTS|VARIANT_FALSE|  
|DBPROP_IAccessor<br /><br /> DBPROP_IColumnsInfo<br /><br /> DBPROP_IConvertType<br /><br /> DBPROP_IRowset<br /><br /> DBPROP_IRowsetInfo,<br /><br /> DBPROP_IColumnsRowset|VARIANT_TRUE|  
|DBPROP_IConnectionPointContainer<br /><br /> DBPROP_IMultipleResults<br /><br /> DBPROP_IRowsetUpdate<br /><br /> DBPROP_IRowsetIdentity<br /><br /> DBPROP_IRowsetLocate<br /><br /> DBPROP_IRowsetScroll<br /><br /> DBPROP_IRowsetResynch|VARIANT_FALSE|  
|DBPROP_IRowsetChange|VARIANT_TRUE<br /><br /> Nota: l'oggetto set di righe di parametri con valori di tabella supporta le interfacce IRowsetChange.<br /><br /> Un set di righe creato tramite DBPROP_IRowsetChange uguale a VARIANT_TRUE indica comportamenti basati sulla modalità di aggiornamento immediato.<br /><br /> Se, tuttavia, le colonne BLOB vengono associate come oggetti ISequentialStream, il consumer le manterrà prevedibilmente per la durata dell'oggetto set di righe di parametri con valori di tabella.|  
|DBPROP_ISupportErrorInfo|VARIANT_TRUE|  
|DBPROP_ISequentialStream|VARIANT_TRUE|  
|DBPROP_IMMOBILEROWS|VARIANT_TRUE|  
|DBPROP_LITERALIDENTITY|VARIANT_TRUE|  
|DBPROP_LOCKMODE|DBPROPVAL_LM_NONE|  
|DBPROP_MAXOPENROWS|0|  
|DBPROP_MAXPENDINGROWS|0|  
|DBPROP_MAXROWS|0|  
|DBPROP_NOTIFICATIONPHASES|0|  
|DBPROP_NOTIFYCOLUMNSET DBPROP_NOTIFYROWDELETE DBPROP_NOTIFYROWFIRSTCHANGE DBPROP_NOTIFYROWINSERT DBPROP_NOTIFYROWRESYNCH DBPROP_NOTIFYROWSETRELEASE DBPROP_NOTIFYROWSETFETCH-POSITIONCHANGE DBPROP_NOTIFYROWUNDOCHANGE DBPROP_NOTIFYROWUNDODELETE DBPROP_NOTIFYROWUNDOINSERT DBPROP_NOTIFYROWUPDATE|0|  
|DBPROP_OTHERINSERT DBPROP_OTHERUPDATEDELETE|VARIANT_FALSE|  
|DBPROP_OWNINSERT<br /><br /> DBPROP_OWNUPDATEDELETE|VARIANT_TRUE|  
|DBPROP_QUICKRESTART|VARIANT_TRUE|  
|DBPROP_REENTRANTEVENTS|VARIANT_FALSE|  
|DBPROP_REMOVEDELETED|VARIANT_TRUE|  
|DBPROP_RETURNPENDINGINSERTS|VARIANT_TRUE|  
|DBPROP_ROWRESTRICT|VARIANT_FALSE|  
|DBPROP_ROWTHREADMODEL|DBPROPVAL_RT_FREETHREAD|  
|DBPROP_SERVERCURSOR|VARIANT_FALSE|  
|DBPROP_SERVERDATAONINSERT|VARIANT_FALSE|  
|DBPROP_STRONGIDENTITY|VARIANT_TRUE|  
|DBPROP_TRANSACTEDOBJECT|VARIANT_FALSE|  
|DBPROP_UNIQUEROWS|VARIANT_FALSE|  
|DBPROP_UPDATABILITY|DBPROPVAL_UP_CHANGE &#124; DBPROPVAL_UP_DELETE &#124; DBPROPVAL_UP_INSERT|  
  
## <a name="property-sets"></a>Set di proprietà  
 I set di proprietà seguenti supportano i parametri con valori di tabella.  
  
### <a name="dbpropset_sqlservercolumn"></a>DBPROPSET_SQLSERVERCOLUMN  
 Questa proprietà viene usata dal consumer nel processo di creazione di un oggetto set di righe di parametri con valori di tabella tramite ITableDefinitionWithConstraints::CreateTableWithConstraints per ogni colonna della struttura DBCOLUMNDESC, se necessario.  
  
|ID proprietà|Valore proprietà|  
|-----------------|--------------------|  
|SSPROP_COL_COMPUTED|L/S: Lettura/Scrittura<br /><br /> Impostazione predefinita: VARIANT_FALSE<br /><br /> Tipo: VT_BOOL<br /><br /> Descrizione: se impostata su VARIANT_TRUE, indica che la colonna è una colonna calcolata. VARIANT_FALSE indica che non si tratta di una colonna calcolata.|  
  
### <a name="dbpropset_sqlserverparameter"></a>DBPROPSET_SQLSERVERPARAMETER  
 Queste proprietà vengono lette dal consumer durante l'individuazione delle informazioni sul tipo di parametro con valori di tabella nelle chiamate a ISSCommandWithParameters::GetParameterProperties e vengono impostate dal consumer durante l'impostazione di proprietà specifiche relative al parametro con valori di tabella tramite ISSCommandWithParameters::SetParameterProperties.  
  
 Tali proprietà vengono descritte in modo dettagliato nella tabella seguente.  
  
|ID proprietà|Valore proprietà|  
|-----------------|--------------------|  
|SSPROP_PARAM_TYPE_TYPENAME|L/S: Lettura/Scrittura<br /><br /> Impostazione predefinita: VT_EMPTY<br /><br /> Tipo: VT_BSTR<br /><br /> Descrizione: i consumer utilizzano questa proprietà per ottenere o impostare il nome del tipo di parametro con valori di tabella.<br /><br /> Questa proprietà può essere utilizzata anche con i tipi CLR definiti dall'utente.<br /><br /> Questa proprietà può essere specificata facoltativamente per fornire un nome del tipo di tabella per un parametro con valori di tabella (in caso di comando della sintassi di ODBC). Questa proprietà è obbligatoria per le query SQL ad hoc con parametri.|  
|SSPROP_PARAM_TYPE_SCHEMANAME|L/S: Lettura/Scrittura<br /><br /> Impostazione predefinita: VT_EMPTY<br /><br /> Tipo: VT_BSTR<br /><br /> Descrizione: i consumer utilizzano questa proprietà per ottenere o impostare il nome dello schema del tipo di parametro con valori di tabella.<br /><br /> Questa proprietà può essere utilizzata anche con i tipi CLR definiti dall'utente.|  
|SSPROP_PARAM_TYPE_CATALOGNAME|L/S: Sola lettura<br /><br /> Impostazione predefinita: VT_EMPTY<br /><br /> Tipo: VT_BSTR<br /><br /> Descrizione: i consumer utilizzano questa proprietà per ottenere il nome del catalogo del tipo di parametro con valori di tabella.<br /><br /> Questa proprietà può essere utilizzata anche con i tipi CLR definiti dall'utente. Non è corretto impostare questa proprietà, in quanto i tipi definiti dall'utente devono essere inclusi nello stesso database in cui si trovano i parametri con valori di tabella che li utilizzano.|  
|SSPROP_PARAM_TABLE_DEFAULT_COLUMNS|L/S: Lettura/Scrittura<br /><br /> Impostazione predefinita: VT_EMPTY<br /><br /> Tipo: VT_UI2 &#124; VT_ARRAY<br /><br /> Descrizione: i consumer utilizzano questa proprietà per specificare quale set di colonne nel set di righe deve essere considerato quello predefinito. Per tali colonne non verrà inviato alcun valore. Durante il recupero di dati dall'oggetto set di righe del consumer, il provider non richiede un'associazione per tali colonne.<br /><br /> Ogni elemento della matrice deve essere un numero ordinale di una colonna nell'oggetto set di righe. Numeri ordinali non validi producono errori in fase di esecuzione del comando.|  
|SSPROP_PARAM_TABLE_COLUMN_ORDER|L/S: Lettura/Scrittura<br /><br /> Impostazione predefinita: VT_EMPTY<br /><br /> Tipo: VT_UI2 &#124; VT_ARRAY<br /><br /> Descrizione: questa proprietà viene utilizzata dal consumer per fornire un hint al server per indicare il tipo di ordinamento dei dati della colonna. Il provider non esegue alcuna convalida e presuppone che il consumer sia conforme alla specifica fornita. Il server utilizza questa proprietà per eseguire ottimizzazioni.<br /><br /> Le informazioni sull'ordine delle colonne per ogni colonna vengono rappresentate da una coppia di elementi nella matrice. Il primo elemento nella coppia è il numero della colonna. Il secondo elemento nella coppia sarà 1 per l'ordine crescente o 2 per l'ordine decrescente.|  
  
## <a name="see-also"></a>Vedere anche  
 [Supporto dei tipi di parametri con valori di tabella OLE DB](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support.md)   
 [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
