---
description: Accedere agli schemi di inventario (AccessToSQL)
title: Accedere agli schemi di inventario (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- columns table
- databases table
- exported metadata
- exporting, schemas of exported metadata
- foreign keys table
- forms table
- indexes, table
- inventory schemas
- macros table
- modules table
- queries table
- reference, inventory schemas
- reports table
- schemas of exported metadata
- schemas, exported metadata
- SSMA_Access_InventoryColumns
- SSMA_Access_InventoryDatabases
- SSMA_Access_InventoryForeignKeys
- SSMA_Access_InventoryForms
- SSMA_Access_InventoryIndexes
- SSMA_Access_InventoryMacros
- SSMA_Access_InventoryModules
- SSMA_Access_InventoryQueries
- SSMA_Access_InventoryReports
- SSMA_Access_InventoryTables
- tables, inventory
ms.assetid: fdd3cff2-4d62-4395-8acf-71ea8f17f524
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bddb9d2329c20203aca602e7aec37f235c4b6850
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077672"
---
# <a name="access-inventory-schemas-accesstosql"></a>Accedere agli schemi di inventario (AccessToSQL)
Nelle sezioni seguenti vengono descritte le tabelle create da SSMA quando si esportano gli schemi di accesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="databases"></a>Database  
I metadati del database vengono esportati nella tabella **SSMA_Access_InventoryDatabases** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|GUID che identifica in modo univoco ogni database. Questa colonna è anche la chiave primaria per la tabella.|  
|**DatabaseName**|**nvarchar(4000)**|Nome del database di Access.|  
|**ExportTime**|**datetime**|Data e ora di creazione dei metadati da parte di SSMA.|  
|**FilePath**|**nvarchar(4000)**|Percorso completo e nome file del database di Access.|  
|**FileSize**|**bigint**|Dimensioni del database di Access in KB.|  
|**Fileowner**|**nvarchar(4000)**|Account di Windows specificato come proprietario del database di Access.|  
|**DateCreated**|**datetime**|Data e ora di creazione del database di Access.|  
|**DateModified**|**datetime**|Data e ora dell'Ultima modifica del database di Access.|  
|**TablesCount**|**int**|Il numero di tabelle nel database di Access.|  
|**QueriesCount**|**int**|Numero di query nel database di Access.|  
|**FormsCount**|**int**|Numero di moduli nel database di Access.|  
|**ModulesCount**|**int**|Il numero di moduli nel database di Access.|  
|**ReportsCount**|**int**|Numero di report nel database di Access.|  
|**MacrosCount**|**int**|Numero di macro nel database di Access.|  
|**AccessVersion**|**nvarchar(4000)**|Versione di accesso del database.|  
|**Regole di confronto**|**nvarchar(4000)**|Regole di confronto del database di Access. Le regole di confronto determinano il modo in cui un database ordina e confronta le stringhe.|  
|**JetVersion**|**nvarchar(4000)**|Versione del motore di database Jet. I database di Access utilizzano il motore di database Jet sottostante.|  
|**Aggiornabile**|**bit**|Indica se il database può essere aggiornato. Se il valore è 1, il database è aggiornabile. Se il valore è 0, il database è di sola lettura.|  
|**QueryTimeout**|**int**|Valore di timeout della query ODBC configurato per il database, in secondi. Il valore predefinito è 60 secondi.|  
  
## <a name="tables"></a>Tabelle  
I metadati della tabella vengono esportati nella tabella **SSMA_Access_InventoryTables** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente la tabella.|  
|**TableId**|**uniqueidentifier**|GUID che identifica in modo univoco la tabella. Questa colonna è anche la chiave primaria per la tabella.|  
|**TableName**|**nvarchar(4000)**|Nome della tabella.|  
|**RowsCount**|**int**|Numero di righe nella tabella.|  
|**ValidationRule**|**nvarchar(4000)**|Regola che definisce un input valido per la tabella. Se non esiste alcuna regola di convalida, il campo conterrà una stringa vuota.|  
|**LinkedTable**|**nvarchar(4000)**|Un'altra tabella, se presente, collegata alla tabella. Il collegamento di tabelle consente l'aggiunta, l'eliminazione e l'aggiornamento dell'altra tabella tramite questa tabella.|  
|**ExternalSource**|**nvarchar(4000)**|Origine dati, se presente, associata alla tabella. Se una tabella è collegata, in questo campo è stata specificata un'origine dati esterna.|  
  
## <a name="columns"></a>Colonne  
I metadati della colonna vengono esportati nella tabella **SSMA_Access_InventoryColumns** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente la colonna.|  
|**TableId**|**uniqueidentifier**|Identifica la tabella che contiene la colonna.|  
|**ColumnId**|**int**|Intero incrementale che identifica la colonna. **ColumnID** è la chiave primaria per la tabella.|  
|**ColumnName**|**nvarchar(4000)**|Nome della colonna.|  
|**IsNullable**|**bit**|Specifica se la colonna può contenere valori null. Se il valore è 1, la colonna può contenere valori null. Se il valore è 0, la colonna non può contenere valori null. Si noti che la regola di convalida può essere usata anche per impedire i valori null.|  
|**DataType**|**nvarchar(4000)**|Tipo di dati di accesso della colonna, ad esempio **testo** o **Long**.|  
|**IsAutoIncrement**|**bit**|Specifica se la colonna incrementa automaticamente i valori integer. Se il valore è 1, i numeri interi vengono incrementati automaticamente.|  
|**Ordinale**|**smallint**|Ordine della colonna nella tabella, a partire da zero.|  
|**DefaultValue**|**nvarchar(4000)**|Il valore predefinito per la colonna.|  
|**ValidationRule**|**nvarchar(4000)**|Regola utilizzata per convalidare i dati aggiunti o aggiornati nella colonna.|  
  
## <a name="indexes"></a>Indici  
I metadati dell'indice vengono esportati nella tabella **SSMA_Access_InventoryIndexes** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente l'indice.|  
|**TableId**|**uniqueidentifier**|Identifica la tabella che contiene l'indice.|  
|**IndexId**|**int**|Intero incrementale che identifica l'indice. Questa colonna è la chiave primaria per la tabella.|  
|**IndexName**|**nvarchar(4000)**|Nome dell'indice.|  
|**ColumnsIncluded**|**nvarchar(4000)**|Elenca le colonne incluse nell'indice. I nomi delle colonne sono separati da un punto e virgola.|  
|**IsUnique**|**bit**|Specifica se ogni elemento nell'indice deve essere univoco. In un indice a più colonne la combinazione di valori deve essere univoca. Se il valore è 1, l'indice impone valori univoci.|  
|**IsPK**|**bit**|Specifica se l'indice è stato creato automaticamente come parte della definizione della chiave primaria.|  
|**IsClustered**|**bit**|Specifica se l'indice è di tipo cluster. Un indice cluster riordina l'archiviazione fisica dei dati. Una tabella può avere un solo indice cluster.|  
  
## <a name="foreign-keys"></a>Chiavi esterne  
I metadati di chiave esterna vengono esportati nella tabella **SSMA_Access_InventoryForeignKeys** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente la chiave esterna.|  
|**TableId**|**uniqueidentifier**|Identifica la tabella che contiene la chiave esterna.|  
|**ForeignKeyId**|**int**|Intero incrementale che identifica la chiave esterna. Questa colonna è la chiave primaria per la tabella.|  
|**ForeignKeyName**|**nvarchar(4000)**|Nome dell'indice.|  
|**ReferencedTableId**|**uniqueidentifier**|Identifica la tabella contenente le colonne di origine.|  
|**SourceColumns**|**nvarchar(4000)**|Elenca la colonna o le colonne chiave esterna.|  
|**ReferencedColumns**|**nvarchar(4000)**|Elenca la colonna o le colonne chiave primaria a cui fa riferimento la chiave esterna.|  
|**IsCascadeForUpdate**|**bit**|Specifica che, se il valore della chiave primaria viene aggiornato, vengono aggiornate anche tutte le righe che fanno riferimento a tale valore di chiave.|  
|**IsCascadeForDelete**|**bit**|Specifica che se il valore della chiave primaria viene eliminato, vengono eliminate anche tutte le righe che fanno riferimento a tale valore di chiave.|  
|**Applicato**|**bit**|Specifica che viene applicato il vincolo di chiave esterna.|  
  
## <a name="queries"></a>Query  
I metadati della query vengono esportati nella tabella **SSMA_Access_InventoryQueries** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente la query.|  
|**QueryId**|**int**|Intero incrementale che identifica la query. Questa colonna è la chiave primaria per la tabella.|  
|**Nomequery**|**nvarchar(4000)**|Nome della query.|  
|**QueryText**|**nvarchar(4000)**|Codice della query SQL, ad esempio un'istruzione SELECT.|  
|**IsUpdateable**|**bit**|Specifica se la query è aggiornabile o di sola lettura.|  
|**QueryType**|**nvarchar(4000)**|Specifica il tipo di query, ad esempio **Select** o **seoperation**.|  
|**ExternalSource**|**nvarchar(4000)**|Se la query fa riferimento a un'origine dati esterna, si tratta della stringa di connessione utilizzata dalla query.|  
  
## <a name="forms"></a>Moduli  
I metadati del modulo vengono esportati nella tabella **SSMA_Access_InventoryForms** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente il form.|  
|**FormId**|**int**|Intero incrementale che identifica il form. Questa colonna è la chiave primaria per la tabella.|  
|**Nomemodulo**|**nvarchar(4000)**|Nome del form.|  
  
## <a name="macros"></a>Macro  
I metadati della macro vengono esportati nella tabella **SSMA_Access_InventoryMacros** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente la macro.|  
|**MacroId**|**int**|Intero incrementale che identifica la macro. Questa colonna è la chiave primaria per la tabella.|  
|**Nomemacro**|**nvarchar(4000)**|Nome della macro.|  
  
## <a name="reports"></a>Report  
I metadati del report vengono esportati nella tabella **SSMA_Access_InventoryReports** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente il report.|  
|**ID report**|**int**|Intero incrementale che identifica il rapporto. Questa colonna è la chiave primaria per la tabella.|  
|**ReportName**|**nvarchar(4000)**|Nome del report.|  
  
## <a name="modules"></a>Moduli  
I metadati del modulo vengono esportati nella tabella **SSMA_Access_InventoryModules** . Questa tabella contiene le colonne seguenti:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|---------------|-------------|---------------|  
|**DatabaseId**|**uniqueidentifier**|Identifica il database contenente il modulo.|  
|**ModuleId**|**int**|Intero incrementale che identifica il modulo. Questa colonna è la chiave primaria per la tabella.|  
|**ModuleName**|**nvarchar(4000)**|Nome del modulo.|  
  
## <a name="see-also"></a>Vedere anche  
[Esportazione di un inventario di Access](exporting-an-access-inventory-accesstosql.md)  
  
