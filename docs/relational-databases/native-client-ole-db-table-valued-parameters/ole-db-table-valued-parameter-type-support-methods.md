---
description: OLE DB Table-Valued supporto del tipo di parametro in SQL Server Native Client (metodi)
title: Tipo di parametro OLE DB Table-Valued (metodi)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (OLE DB), API support (methods)
ms.assetid: e3c2a450-8fd4-44cb-93d8-affe1b65c68e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fa16f6742f23c689a017e8fdaf4165e3ba9185aa
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753491"
---
# <a name="ole-db-table-valued-parameter-type-support-in-sql-server-native-client-methods"></a>OLE DB Table-Valued supporto del tipo di parametro in SQL Server Native Client (metodi)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  I parametri con valori di tabella sono supportati dai seguenti metodi OLE DB standard:  
  
|Metodo|Supporto dei parametri con valori di tabella|  
|------------|-------------------------------------|  
|ITableDefinitionWithConstraints::CreateTableWithConstraints|Utilizzato quando si conoscono le informazioni sul tipo di parametro con valori di tabella e si desidera creare un'istanza dell'oggetto set di righe di parametri con valori di tabella in base alle informazioni sul tipo.<br /><br /> Per altre informazioni, vedere "Scenario statico" in [Creazione di un set di righe di parametri con valori di tabella](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md).|  
|IOpenRowset::OpenRowset|Utilizzato quando non si conoscono le informazioni sul tipo di un parametro con valori di tabella e si desidera creare un'istanza dell'oggetto set di righe di parametri con valori di tabella in base alle informazioni sui metadati recuperate dal server.<br /><br /> Per altre informazioni, vedere "Scenario dinamico" in [Creazione di un set di righe di parametri con valori di tabella](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md).|  
|ISSCommandWithParameters::SetParameterInfo|Per specificare un parametro del comando con parametri con valori di tabella, il consumer specifica il tipo del parametro come "table" o "DBTYPE_TABLE" nel membro *pwszName* della struttura DBPARAMBINDINFO. Il campo *ulParamSize* è impostato su ~0. Per altre informazioni, vedere "Specifica del parametro con valori di tabella" in [Esecuzione di comandi contenenti parametri con valori di tabella](../../relational-databases/native-client-ole-db-table-valued-parameters/executing-commands-containing-table-valued-parameters.md).|  
|ISSCommandWithParameters::SetParameterProperties|Imposta le proprietà specifiche per i parametri con valori di tabella, ad esempio nome dello schema, nome del tipo, ordine delle colonne e colonne predefinite.<br /><br /> Il consumer specifica il numero ordinale del parametro nel membro *iOrdinal* della struttura SSPARAMPROPS. Il set di proprietà richiesto è DBPROPSET_SQLSERVERPARAMETER.|  
|ISSCommandWithParameters::GetParameterInfo|Ottiene i tipi di tutti i parametri per un comando specificato.<br /><br /> Per i parametri con valori di tabella, il tipo del campo *wType* nella struttura DBPARAMINFO sarà DBTYPE_TABLE. Il campo *ulParamSize* verrà impostato su ~0 per indicare che la lunghezza non è nota.|  
|ISSCommandWithParameters::GetParameterProperties|Ottiene informazioni sul tipo aggiuntive per i parametri del tipo DBTYPE_TABLE.<br /><br /> Il consumer specifica il numero ordinale del parametro nel membro *iOrdinal* della struttura SSPARAMPROPS. Il consumer può richiedere qualsiasi proprietà presente nel set di proprietà DBPROPSET_SQLSERVERPARAMETER riportata in ISSCommandWithParameters::SetParameterProperties.<br /><br /> Poiché il consumer non conosce il tipo di parametro con valori di tabella, il provider deve impostare SSPROP_PARAM_TYPE_TYPENAME, SSPROP_PARAM_TYPE_SCHEMANAME e SSPROP_PARAM_TYPE_CATALOGNAME sui valori corretti. Le proprietà restanti, SSPROP_PARAM_TABLE_DEFAULT_COLUMNS e SSPROP_PARAM_TABLE_COLUMN_SORT_ORDER, utilizzeranno i valori predefiniti. Dopo che il consumer ha individuato il nome del tipo di parametro con valori di tabella, usa IOpenRowset::OpenRowset per creare un'istanza di questo parametro con valori di tabella, specificando il nome del tipo di parametro con valori di tabella. Per altre informazioni, vedere [Individuazione dei tipi di parametri con valori di tabella](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameter-type-discovery.md).|  
|IRowsetInfo::GetProperties|Ottiene le proprietà del set di righe di parametri con valori di tabella. Il consumer può utilizzare queste proprietà per configurare le associazioni in modo ottimale.|  
|IColumnsRowset::GetColumnsRowset|Recupera le informazioni sui metadati relative a una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per i parametri con valori di tabella, questa stessa interfaccia fornisce informazioni dettagliate sui metadati relative a ogni colonna, ad esempio:<br /><br /> DBCOLUMN_FLAGS indica se i valori sono supportati tramite il bit DBCOLUMNFLAGS_ISNULLABLE.<br /><br /> DBCOLUMN_ISUNIQUE indica se la nuova colonna è una colonna Identity.<br /><br /> DBCOLUMN_COMPUTEMODE indica se la colonna viene calcolata.|  
|IAccessor::CreateAccessor|Per associare un oggetto set di righe di parametri con valori di tabella a un parametro del comando, creare una funzione di accesso con il membro *wType* impostato su DBTYPE_TABLE. La struttura DBOBJECT conterrà l'interfaccia IID_IRowset o un'altra interfaccia valida dell'oggetto set di righe nel membro *iid*. Il resto dei campi viene gestito in modo analogo a DBTYPE_IUNKNOWN.|  
|||

## <a name="see-also"></a>Vedere anche  
 [Supporto dei tipi di parametri con valori di tabella OLE DB](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support.md)   
 [Creazione di un set di righe di parametri con valori di tabella](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md)   
 [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
