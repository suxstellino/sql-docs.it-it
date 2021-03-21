---
title: Proprietà sessione - Driver OLE DB per SQL Server | Microsoft Docs
description: Informazioni sul modo in cui OLE DB Driver per SQL Server interpreta le proprietà della sessione OLE DB, incluso un set di proprietà specifico del provider.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- OLE DB Driver for SQL Server, sessions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 326b0d09fbcf071d981db56440956393eda93e73
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755631"
---
# <a name="session-properties---ole-db-driver-for-sql-server"></a>Proprietà sessione - Driver OLE DB per SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver per SQL Server interpreta le proprietà di sessione di OLE DB nel modo descritto di seguito.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|Il driver OLE DB per SQL Server supporta tutti i livelli di isolamento delle transazioni autocommit, ad eccezione del livello Chaos DBPROPVAL_TI_CHAOS.|  
  
 Nel set di proprietà DBPROPSET_SQLSERVERSESSION specifico del provider il driver OLE DB per SQL Server definisce le proprietà di sessione aggiuntive indicate di seguito.  
  
|ID proprietà|Descrizione|  
|-----------------|-----------------|  
|SSPROP_QUOTEDCATALOGNAMES|Tipo: VT_BOOL<br /><br /> R/W (L/S): Lettura/Scrittura<br /><br /> Impostazione predefinita: VARIANT_FALSE<br /><br /> Descrizione: gli identificatori tra virgolette consentiti nella restrizione CATALOG.<br /><br /> VARIANT_TRUE: gli identificatori tra virgolette sono riconosciuti per una restrizione per catalogo per i set di righe dello schema che forniscono il supporto query distribuito.<br /><br /> VARIANT_FALSE: gli identificatori tra virgolette non sono riconosciuti per una restrizione per catalogo per i set di righe dello schema che forniscono il supporto di query distribuite.<br /><br /> Per altre informazioni sui set di righe dello schema che offrono il supporto di query distribuite, vedere [Supporto di query distribuite nei set di righe dello schema](../../oledb/ole-db/schema-rowsets-distributed-query-support.md).|  
|SSPROP_ALLOWNATIVEVARIANT|Digitare: VT_BOOL<br /><br /> R/W (L/S): Lettura/Scrittura<br /><br /> Predefinito: VARIANT_FALSE<br /><br /> Descrizione: determina se i dati recuperati appartengono alla categoria DBTYPE_VARIANT o DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE: il tipo di colonna viene restituito come DBTYPE_SQLVARIANT e in tal caso il buffer conserva la struttura SSVARIANT.<br /><br /> VARIANT_FALSE: il tipo di colonna viene restituito come DBTYPE_VARIANT e il buffer presenta la struttura VARIANT.|  
|SSPROP_ASYNCH_BULKCOPY|Per utilizzare la modalità asincrona, impostare la proprietà di sessione SSPROP_ASYNCH_BULKCOPY specifica del provider su VARIANT_TRUE prima di chiamare il metodo BCPExec. Questa proprietà è disponibile nel set di proprietà DBPROPSET_SQLSERVERSESSION.|  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetti di origine dati &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
