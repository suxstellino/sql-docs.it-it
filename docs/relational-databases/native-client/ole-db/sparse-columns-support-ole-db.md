---
description: Supporto delle colonne di tipo sparse nei SQL Server Native Client (OLE DB)
title: Supporto di colonne di tipo sparse (OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 918574b3-c62e-4937-9e5f-37310dedc8f9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f336c36293224b16528367c820ded5a6b0c496b0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744851"
---
# <a name="sparse-columns-support-in-sql-server-native-client-ole-db"></a>Supporto delle colonne di tipo sparse nei SQL Server Native Client (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono fornite informazioni sul supporto OLE DB di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client per le colonne di tipo sparse. Per ulteriori informazioni sulle colonne di tipo sparse, vedere [supporto di colonne di tipo sparse in SQL Server Native Client](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md). Per un esempio, vedere [Visualizzare i metadati della colonna e del catalogo per le colonne di tipo sparse &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-how-to/display-column-and-catalog-metadata-for-sparse-columns-ole-db.md).  
  
## <a name="ole-db-statement-metadata"></a>Metadati di istruzione OLE DB  
 A partire da [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], è disponibile un nuovo valore per il flag DBCOLUMNFLAGS, ovvero DBCOLUMNFLAGS_SS_ISCOLUMNSET. Questo valore deve essere impostato per colonne che sono valori **column_set**. Il flag DBCOLUMNFLAGS può essere recuperato tramite il parametro *dwFlags* di IColumnsInfo:: GetColumnsInfo e la colonna DBCOLUMN_FLAGS del set di righe restituito da IColumnsRowset::GetColumnsRowset.  
  
## <a name="ole-db-catalog-metadata"></a>Metadati del catalogo OLE DB  
 Due colonne aggiuntive specifiche di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sono state aggiunte a DBSCHEMA_COLUMNS.  
  
|Nome colonna|Tipo di dati|Valore/commenti|  
|-----------------|---------------|---------------------|  
|SS_IS_SPARSE|DBTYPE_BOOL|Se la colonna è una colonna di tipo sparse, il valore di questa colonna è VARIANT_TRUE; in caso contrario, è VARIANT_FALSE.|  
|SS_IS_COLUMN_SET|DBTYPE_BOOL|Se la colonna è la colonna **column_set** di tipo sparse, il relativo valore è VARIANT_TRUE; in caso contrario, è VARIANT_FALSE.|  
  
 Sono inoltre stati aggiunti altri due set di righe dello schema. Tali set di righe hanno la stessa struttura di DBSCHEMA_COLUMNS ma restituiscono contenuto diverso. DBSCHEMA_COLUMNS_EXTENDED restituisce tutte le colonne indipendentemente dall'appartenenza a **column_set**. DBSCHEMA_SPARSE_COLUMN_SET restituisce solo le colonne che sono colonne **column_set** di tipo sparse.  
  
## <a name="ole-db-datatypecompatibility-behavior"></a>Comportamento OLE DB di DataTypeCompatibility  
 Il comportamento con **DataTypeCompatibility=80** (nella stringa di connessione) è coerente con un client [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)], come indicato di seguito:  
  
-   I nuovi set di righe dello schema non sono visibili e i set di righe dello schema non includono righe per essi.  
  
-   Le nuove colonne nel set di righe COLUMNS non sono visibili.  
  
-   DBCOLUMNFLAGS_SS_ISCOLUMNSET non è impostato per le colonne **column_set**.  
  
-   DBCOMPUTEMODE_NOTCOMPUTED è impostato per le colonne **column_set**.  
  
## <a name="ole-db-support-for-sparse-columns"></a>Supporto OLE DB per colonne di tipo sparse  
 Le interfacce OLE DB seguenti sono state modificate in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client per supportare le colonne di tipo sparse:  
  
|Tipo o funzione membro|Descrizione|  
|-----------------------------|-----------------|  
|IColumnsInfo::GetColumnsInfo|Un nuovo valore del flag DBCOLUMNFLAGS, ovvero DBCOLUMNFLAGS_SS_ISCOLUMNSET, è impostato per le colonne **column_set** in *dwFlags*.<br /><br /> DBCOLUMNFLAGS_WRITE è impostato per le colonne **column_set**.|  
|IColumsRowset::GetColumnsRowset|Un nuovo valore del flag DBCOLUMNFLAGS, ovvero DBCOLUMNFLAGS_SS_ISCOLUMNSET, è impostato per le colonne **column_set** in DBCOLUMN_FLAGS.<br /><br /> DBCOLUMN_COMPUTEMODE è impostato su DBCOMPUTEMODE_DYNAMIC per le colonne **column_set**.|  
|IDBSchemaRowset::GetSchemaRowset|DBSCHEMA_COLUMNS restituisce due nuove colonne: SS_IS_COLUMN_SET e SS_IS_SPARSE.<br /><br /> DBSCHEMA_COLUMNS restituisce solo colonne che non sono membri di **column_set**.<br /><br /> Sono stati aggiunti due nuovi set di righe dello schema: DBSCHEMA_COLUMNS_EXTENDED restituirà tutte le colonne indipendentemente dal fatto che siano o meno di tipo sparse o dall'appartenenza a **column_set**. DBSCHEMA_SPARSE_COLUMN_SET restituisce solo le colonne che sono membri di **column_set**. Questi nuovi set di righe includono le stesse colonne e comportano le stesse restrizioni di DBSCHEMA_COLUMNS.|  
|IDBSchemaRowset::GetSchemas|IDBSchemaRowset::GetSchemas include i GUID per i nuovi set di righe DBSCHEMA_COLUMNS_EXTENDED e DBSCHEMA_SPARSE_COLUMN_SET nell'elenco dei set di righe dello schema disponibili.|  
|ICommand::Execute|Se si utilizza **seleziona \* dalla** *tabella*, vengono restituite tutte le colonne che non sono membri del **column_set** di tipo sparse, nonché una colonna XML che contiene i valori di tutte le colonne non null membri del **column_set** di tipo sparse, se presente.|  
|IOpenRowset::OpenRowset|IOpenRowset::OpenRowset restituisce un rowset ewith thcon le stesse colonne di ICommand::Execute, con una query **selezionare \*** nella stessa tabella.|  
|ITableDefinition|Non è stata apportata alcuna modifica a questa interfaccia per le colonne di tipo sparse o per le colonne **column_set**. Le applicazioni che devono apportare modifiche allo schema devono eseguire direttamente l'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] appropriata.|  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
