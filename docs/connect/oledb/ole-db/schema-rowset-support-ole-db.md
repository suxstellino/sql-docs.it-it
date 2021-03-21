---
title: Supporto del set di righe dello schema (OLE DB) | Microsoft Docs
description: OLE DB driver per SQL Server supporta anche la restituzione di informazioni sullo schema da un server collegato durante l'elaborazione di query distribuite Transact-SQL.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- OLE DB, schema rowsets
- OLE DB rowsets, schema
- OLE DB Driver for SQL Server, schema rowsets
- rowsets [OLE DB], schema
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 700d5b770176f9aefdab1ac7dfd43eee21f78d91
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754161"
---
# <a name="schema-rowset-support-ole-db"></a>Supporto dei set di righe dello schema (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Il driver OLE DB per SQL Server supporta anche la restituzione di informazioni sullo schema da un server collegato durante l'elaborazione di query distribuite [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  Anche se [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] supporta i sinonimi, non vengono restituiti metadati per i sinonimi da OLE DB Driver per SQL Server.  
  
 Nelle tabelle seguenti sono elencati i set di righe dello schema e le colonne di restrizione supportati dal driver OLE DB per SQL Server.  
  
|Set di righe dello schema|Colonne di restrizione|  
|-------------------|-------------------------|  
|DBSCHEMA_CATALOGS|CATALOG_NAME|  
|DBSCHEMA_COLUMN_PRIVILEGES|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|DBSCHEMA_COLUMNS|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME<br /><br /> Le colonne aggiuntive seguenti sono specifiche di  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:<br /><br /> COLUMN_LCID, che rappresenta l'ID delle impostazioni locali delle regole di confronto. COLUMN_LCID coincide con il valore dell'LCID di Windows.<br /><br /> COLUMN_COMPFLAGS definisce i confronti supportati per le regole di confronto. Il formato di dati corrisponde a DBPROB_FINDCOMPAREOPS.<br /><br /> COLUMN_SORTID, che rappresenta lo stile di ordinamento di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per le regole di confronto.<br /><br /> COLUMN_TDSCOLLATION, che rappresenta le regole di confronto di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per la colonna.<br /><br /> IS_COMPUTED, che è VARIANT_TRUE se la colonna è una colonna calcolata e VARIANT_FALSE in caso contrario.|  
|DBSCHEMA_FOREIGN_KEYS|Sono supportate tutte le restrizioni.<br /><br /> PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|DBSCHEMA_INDEXES|Sono supportate le restrizioni 1, 2, 3 e 5.<br /><br /> TABLE_CATALOG TABLE_SCHEMA INDEX_NAME TABLE_NAME|  
|DBSCHEMA_PRIMARY_KEYS|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|DBSCHEMA_PROCEDURE_PARAMETERS|Sono supportate tutte le restrizioni.<br /><br /> PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|DBSCHEMA_PROCEDURES|Sono supportate le restrizioni 1, 2 e 3.<br /><br /> PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME<br /><br /> DBSCHEMA_PROCEDURES restituisce solo le procedure che possono essere eseguite dall'utente corrente o per cui l'utente corrente dispone dell'autorizzazione VIEW DEFINITION.|  
|DBSCHEMA_PROVIDER_TYPES|Sono supportate tutte le restrizioni.<br /><br /> DATA_TYPE BEST_MATCH|  
|DBSCHEMA_SCHEMATA|Sono supportate tutte le restrizioni.<br /><br /> CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|DBSCHEMA_STATISTICS|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|DBSCHEMA_TABLE_CONSTRAINTS|Sono supportate tutte le restrizioni.<br /><br /> CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|DBSCHEMA_TABLE_PRIVILEGES|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|DBSCHEMA_TABLES|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|DBSCHEMA_TABLES_INFO|Sono supportate tutte le restrizioni.<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Supporto di query distribuite nei set di righe dello schema](../../oledb/ole-db/schema-rowsets-distributed-query-support.md)  
  
 [Set di righe LINKEDSERVERS &#40;OLE DB&#41;](../../oledb/ole-db/schema-rowsets-linkedservers-rowset.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Driver OLE DB per programmazione con SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [Uso dei tipi definiti dall'utente](../../oledb/features/using-user-defined-types.md)  
  
  
