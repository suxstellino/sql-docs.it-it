---
description: Set di righe dello schema modificati per OLE DB parametri di Table-Valued in SQL Server Native Client
title: Set di righe dello schema, OLE DB parametri di Table-Valued
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- table-valued parameters (OLE DB), schema rowsets changed for (OLE DB)
ms.assetid: 581e3ead-53db-44da-8718-f3fc4b5108f1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c2419ae25a8a77185aa52da02445ed02e9d3ae34
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751791"
---
# <a name="schema-rowsets-changed-for-ole-db-table-valued-parameters-in-sql-server-native-client"></a>Set di righe dello schema modificati per OLE DB parametri di Table-Valued in SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Di seguito sono elencati i set di righe dello schema che sono stati modificati o aggiunti per il supporto dei parametri con valori di tabella.  
  
|Set di righe dello schema|Descrizione|  
|-------------------|-----------------|  
|DBSCHEMA_PROCEDURE_PARAMETERS|Alla fine del set di righe sono state aggiunte due nuove colonne denominate SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMANAME. Tali colonne potrebbero essere riutilizzate per i tipi futuri. Le colonne TYPE_NAME e LOCAL_TYPE_NAME conterranno il nome del tipo TABLE dei parametri con valori di tabella. La colonna DATA_TYPE avrà il valore DBTYPE_TABLE = 143 per i parametri con valori di tabella.|  
|DBSCHEMA_TABLE_TYPES|Questo set di righe è stato aggiunto per supportare i parametri con valori di tabella. È identico a DBSCHEMA_TABLES, con l'eccezione che restituisce metadati solo per i tipi di tabella, anziché per tabelle, viste o sinonimi. La colonna TABLE_TYPE avrà il valore TABLE TYPE.|  
|DBSCHEMA_TABLE_TYPE_PRIMARY_KEYS|Questo set di righe è stato aggiunto per supportare i parametri con valori di tabella. È identico a DBSCHEMA_PRIMARY_KEYS, con l'eccezione che restituisce chiavi primarie solo per i tipi di tabella, anziché per tabelle, viste o sinonimi.|  
|DBSCHEMA_TABLE_TYPE_COLUMNS|Questo set di righe è stato aggiunto per supportare i parametri con valori di tabella. È identico a DBSCHEMA_COLUMNS, con l'eccezione che restituisce metadati di colonna solo per i tipi di tabella, anziché per tabelle, viste o sinonimi.|  
|||

## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Usare parametri con valori di tabella &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
