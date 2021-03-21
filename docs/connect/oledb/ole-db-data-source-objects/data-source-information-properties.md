---
title: Proprietà delle informazioni sulle origini dati (OLE DB Driver) | Microsoft Docs
description: Informazioni sulle proprietà delle informazioni sulle origini dati per il set di proprietà specifico del provider DBPROPSET_SQLSERVERDATASOURCEINFO in OLE DB Driver per SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, data source properties
- properties [OLE DB]
- data source properties [OLE DB]
- information properties [OLE DB]
- OLE DB data source properties [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e2e4e68528bf8ad98c31099565ccf146b9e431e8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735317"
---
# <a name="data-source-information-properties"></a>Proprietà delle informazioni sulle origini dati
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Nel set di proprietà DBPROPSET_SQLSERVERDATASOURCEINFO specifico del provider il driver OLE DB per SQL Server definisce le seguenti proprietà delle informazioni sulle origini dati.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|SSPROP_COLUMNLEVELCOLLATION|Tipo: VT_BOOL<br /><br /> L/S: Lettura<br /><br /> Impostazione predefinita: VARIANT_TRUE<br /><br /> Descrizione: utilizzato per determinare se le regole di confronto a livello di colonna sono supportate.<br /><br /> VARIANT_TRUE: le regole di confronto a livello di colonna sono supportate.<br /><br /> VARIANT_FALSE: le regole di confronto a livello di colonna non sono supportate.|  
|SSPROP_UNICODELCID|Tipo: VT_I4 L/S: Lettura<br /><br /> Descrizione: ID delle impostazioni locali Unicode.<br /><br /> Si tratta delle impostazioni locali utilizzate per l'ordinamento dei dati Unicode.|  
|SSPROP_UNICODECOMPARISONSTYLE|Tipo: VT_I4 L/S: Lettura<br /><br /> Descrizione: stile di confronto Unicode.<br /><br /> Opzioni di ordinamento utilizzate per l'ordinamento dei dati Unicode.|  
  
 Nel set di proprietà DBPROPSET_SQLSERVERSTREAM specifico del provider il driver OLE DB per SQL Server definisce le proprietà aggiuntive indicate di seguito.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|SSPROP_STREAM_XMLROOT|Tipo: VT_BSTR L/S: Lettura/Scrittura<br /><br /> Descrizione: il risultato di una query FOR XML potrebbe non essere un documento corretto. Quando questa proprietà viene specificata, viene eseguito il wrapping del risultato di una query 'select ... for XML' nel tag radice fornito dalla proprietà per restituire un documento XML corretto. Se la query viene eseguita nel browser, è possibile che nel browser stesso vengano visualizzati errori del parser durante il caricamento del risultato. Per evitare l'errore, SQL ISAPI supporta la parola chiave ROOT che viene mappata alla proprietà SSPROP_STREAM_XMLROOT.|  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetti di origine dati &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
