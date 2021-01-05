---
title: Raccolte di schemi di SQL Server
description: Descrizione delle raccolte di schemi aggiuntive supportate dal provider di dati Microsoft SqlClient per SQL Server.
ms.date: 11/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3d757c09717d885dbf9bf35dc6826234ef4ea59e
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771513"
---
# <a name="sql-server-schema-collections"></a>Raccolte di schemi di SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Il provider di dati Microsoft SqlClient per SQL Server supporta raccolte di schemi aggiuntive, oltre a quelle comuni. Le raccolte di schemi variano leggermente in base alla versione di SQL Server usata. Per determinare l'elenco delle raccolte di schemi supportate, chiamare il metodo **GetSchema** senza argomenti oppure con il nome della raccolta di schemi "MetaDataCollections". In questo modo verrà restituito un oggetto <xref:System.Data.DataTable> con un elenco delle raccolte di schemi supportati, il numero delle restrizioni supportate da ciascuna raccolta e il numero di parti identificatore usate.  

## <a name="databases"></a>Database  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|database_name|string|Nome del database.|  
|dbid|Int16|ID del database.|  
|create_date|Datetime|Data di creazione del database.|  

## <a name="foreign-keys"></a>Chiavi esterne  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|CONSTRAINT_CATALOG|string|Catalogo a cui appartiene il vincolo.|  
|CONSTRAINT_SCHEMA|string|Schema contenente il vincolo.|  
|CONSTRAINT_NAME|string|Name.|  
|TABLE_CATALOG|string|Nome della tabella contenente il vincolo.|  
|TABLE_SCHEMA|string|Schema contenente la tabella.|  
|TABLE_NAME|string|Nome tabella|  
|CONSTRAINT_TYPE|string|Tipo di vincolo. È consentito solo il tipo "FOREIGN KEY".|  
|IS_DEFERRABLE|string|Specifica se il vincolo può essere rinviato. Restituisce NO.|  
|INITIALLY_DEFERRED|string|Specifica se inizialmente il vincolo può essere rinviato. Restituisce NO.|  

## <a name="indexes"></a>Indici  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|constraint_catalog|string|Catalogo a cui appartiene l'indice.|  
|constraint_schema|string|Schema contenente l'indice.|  
|constraint_name|string|Nome dell'indice.|  
|table_catalog|string|Nome della tabella a cui è associato l'indice.|  
|table_schema|string|Schema contenente la tabella a cui è associato l'indice.|  
|table_name|string|Nome della tabella.|  
|index_name|string|Nome dell'indice.|  
|type_desc|string|I valori del tipo di indice sono i seguenti:<br /><br /> -   HEAP<br />-   CLUSTERED<br />-   NONCLUSTERED<br />-   XML<br />-   SPATIAL|  

## <a name="indexcolumns"></a>IndexColumns  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|constraint_catalog|string|Catalogo a cui appartiene l'indice.|  
|constraint_schema|string|Schema contenente l'indice.|  
|constraint_name|string|Nome dell'indice.|  
|table_catalog|string|Nome della tabella a cui è associato l'indice.|  
|table_schema|string|Schema contenente la tabella a cui è associato l'indice.|  
|table_name|string|Nome della tabella.|  
|column_name|string|Nome della colonna a cui è associato l'indice.|  
|ordinal_position|Int32|Posizione ordinale della colonna.|  
|KeyType|Byte|Tipo di oggetto.|  
|index_name|string|Nome dell'indice.| 

## <a name="procedures"></a>Procedure  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|string|Nome specifico del catalogo.|  
|SPECIFIC_SCHEMA|string|Nome specifico dello schema.|  
|SPECIFIC_NAME|string|Nome specifico del catalogo.|  
|ROUTINE_CATALOG|string|Il catalogo a cui appartiene la stored procedure.|  
|ROUTINE_SCHEMA|string|Lo schema contenente la stored procedure.|  
|ROUTINE_NAME|string|Nome della stored procedure.|  
|ROUTINE_TYPE|string|Restituisce PROCEDURE per le stored procedure e FUNCTION per le funzioni.|  
|CREATED|Datetime|L'ora in cui è stata creata la routine.|  
|LAST_ALTERED|Datetime|Data e ora dell'ultima modifica della routine.| 

## <a name="procedure-parameters"></a>Parametri di procedure  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|SPECIFIC_CATALOG|string|Nome del catalogo della routine per cui viene specificato questo parametro.|  
|SPECIFIC_SCHEMA|string|Lo schema contenente la routine a cui appartiene questo parametro.|  
|SPECIFIC_NAME|string|Il nome della routine contenente questo parametro.|  
|ORDINAL_POSITION|Int32|Posizione ordinale del parametro a partire da 1. Per il valore restituito di una routine, è uguale a 0.|  
|PARAMETER_MODE|string|Restituisce IN se è un parametro di input, OUT se è un parametro di output e INOUT se è un parametro di input/output.|  
|IS_RESULT|string|Restituisce YES se indica il risultato di una routine che è una funzione. In caso contrario restituisce NO.|  
|AS_LOCATOR|string|Restituisce YES se dichiarato come indicatore di posizione. In caso contrario restituisce NO.|  
|PARAMETER_NAME|string|Nome del parametro. È NULL se corrisponde al valore restituito da una funzione.|  
|DATA_TYPE|string|Tipo di dati di sistema.|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Lunghezza massima in caratteri per tipi di dati binary o character. In caso contrario, viene restituito NULL.|  
|CHARACTER_OCTET_LENGTH|Int32|Lunghezza massima in byte per tipi di dati binary o character. In caso contrario, viene restituito NULL.|  
|COLLATION_CATALOG|string|Il nome del catalogo per le regole di confronto del parametro. Se non si tratta di uno dei tipi di dati character, restituisce NULL.|  
|COLLATION_SCHEMA|string|Viene restituito sempre NULL.|  
|COLLATION_NAME|string|Nome delle regole di confronto del parametro. Se non si tratta di uno dei tipi di dati character, restituisce NULL.|  
|CHARACTER_SET_CATALOG|string|Nome del catalogo in cui è definito il set di caratteri del parametro. Se non si tratta di uno dei tipi di dati character, restituisce NULL.|  
|CHARACTER_SET_SCHEMA|string|Viene restituito sempre NULL.|  
|CHARACTER_SET_NAME|string|Nome del set di caratteri del parametro. Se non si tratta di uno dei tipi di dati character, restituisce NULL.|  
|NUMERIC_PRECISION|Byte|Precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. In caso contrario, viene restituito NULL.|  
|NUMERIC_PRECISION_RADIX|Int16|Base di precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. In caso contrario, viene restituito NULL.|  
|NUMERIC_SCALE|Int32|Scala dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. In caso contrario, viene restituito NULL.|  
|DATETIME_PRECISION|Int16|La precisione in secondi frazionari se il tipo di parametro è datetime oppure smalldatetime. In caso contrario, viene restituito NULL.|  
|INTERVAL_TYPE|string|NULL Riservato per un utilizzo futuro da parte di SQL Server.|  
|INTERVAL_PRECISION|Int16|NULL Riservato per un utilizzo futuro da parte di SQL Server.| 

## <a name="tables"></a>Tabelle  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|string|Catalogo della tabella.|  
|TABLE_SCHEMA|string|Schema contenente la tabella.|  
|TABLE_NAME|string|Nome della tabella.|  
|TABLE_TYPE|string|Tipo di tabella. I possibili valori sono VIEW o BASE TABLE.|  

## <a name="columns"></a>Colonne  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|string|Catalogo della tabella.|  
|TABLE_SCHEMA|string|Schema contenente la tabella.|  
|TABLE_NAME|string|Nome della tabella.|  
|COLUMN_NAME|string|Nome colonna.|  
|ORDINAL_POSITION|Int32|Numero di identificazione della colonna.|  
|COLUMN_DEFAULT|string|Il valore predefinito della colonna.|  
|IS_NULLABLE|string|Impostazione relativa al supporto di valori Null nella colonna. Se la colonna supporta i valori NULL, restituisce YES. In caso contrario, restituisce NO.|  
|DATA_TYPE|string|Tipo di dati di sistema.|  
|CHARACTER_MAXIMUM_LENGTH|Int32 – Sql8, Int16 – Sql7|Lunghezza massima espressa in caratteri per i dati di tipo binario, carattere, text o image. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_OCTET_LENGTH|Int32 – SQL8, Int16 – Sql7|Lunghezza massima espressa in byte per i dati di tipo binario, carattere, text o image. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_PRECISION|Unsigned Byte|Precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_PRECISION_RADIX|Int16|Base di precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_SCALE|Int32|Scala dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|DATETIME_PRECISION|Int16|Codice di sottotipo per i tipi di dati SQL-92 datetime e interval. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_SET_CATALOG|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito master, che indica il database in cui è posizionato il set di caratteri. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_SET_SCHEMA|string|Viene restituito sempre NULL.|  
|CHARACTER_SET_NAME|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito il nome univoco del set di caratteri. Per gli altri tipi di dati viene restituito NULL.|  
|COLLATION_CATALOG|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito master, che indica il database in cui sono definite le regole di confronto. Negli altri casi la colonna è NULL.|  
|IS_FILESTREAM|string|YES in caso di colonna con attributo FILESTREAM.<br /><br /> NO se la colonna non dispone dell'attributo FILESTREAM.|  
|IS_SPARSE|string|YES in caso di colonna di tipo sparse.<br /><br /> NO se la colonna non è di tipo sparse.|  
|IS_COLUMN_SET|string|YES in caso di colonna del set di colonne.<br /><br /> NO se la colonna non fa parte del set di colonne.|  

## <a name="allcolumns"></a>AllColumns  

 La raccolta di schemi AllColumns viene usata per supportare le colonne di tipo sparse. AllColumns dispone delle stesse restrizioni e dello schema DataTable risultante della raccolta di schemi Columns, l'unica differenza è costituita dal fatto che AllColumns include colonne del set di colonne che non sono incluse nella raccolta di schemi Columns. Nella tabella seguente vengono descritte queste colonne.  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|string|Catalogo della tabella.|  
|TABLE_SCHEMA|string|Schema contenente la tabella.|  
|TABLE_NAME|string|Nome della tabella.|  
|COLUMN_NAME|string|Nome colonna.|  
|ORDINAL_POSITION|Int32|Numero di identificazione della colonna.|  
|COLUMN_DEFAULT|string|Il valore predefinito della colonna.|  
|IS_NULLABLE|string|Impostazione relativa al supporto di valori Null nella colonna. Se la colonna supporta i valori NULL, restituisce YES. In caso contrario, viene restituito NO.|  
|DATA_TYPE|string|Tipo di dati di sistema.|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Lunghezza massima espressa in caratteri per i dati di tipo binario, carattere, text o image. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_OCTET_LENGTH|Int32|Lunghezza massima espressa in byte per i dati di tipo binario, carattere, text o image. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_PRECISION|Unsigned Byte|Precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_PRECISION_RADIX|Int16|Base di precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_SCALE|Int32|Scala dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|DATETIME_PRECISION|Int16|Codice di sottotipo per i tipi di dati SQL-92 datetime e interval. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_SET_CATALOG|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito master, che indica il database in cui è posizionato il set di caratteri. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_SET_SCHEMA|string|Viene restituito sempre NULL.|  
|CHARACTER_SET_NAME|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito il nome univoco del set di caratteri. Per gli altri tipi di dati viene restituito NULL.|  
|COLLATION_CATALOG|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito master, che indica il database in cui sono definite le regole di confronto. Negli altri casi la colonna è NULL.|  
|IS_FILESTREAM|string|YES in caso di colonna con attributo FILESTREAM.<br /><br /> NO se la colonna non dispone dell'attributo FILESTREAM.|  
|IS_SPARSE|string|YES in caso di colonna di tipo sparse.<br /><br /> NO se la colonna non è di tipo sparse.|  
|IS_COLUMN_SET|string|YES in caso di colonna del set di colonne.<br /><br /> NO se la colonna non fa parte del set di colonne.|  

## <a name="columnsetcolumns"></a>ColumnSetColumns

La raccolta di schemi ColumnSetColumns viene usata per supportare le colonne di tipo sparse. La raccolta di schemi ColumnSetColumns restituisce lo schema per tutte le colonne di un set di colonne. Nella tabella seguente vengono descritte queste colonne.  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|string|Catalogo della tabella.|  
|TABLE_SCHEMA|string|Schema contenente la tabella.|  
|TABLE_NAME|string|Nome della tabella.|  
|COLUMN_NAME|string|Nome colonna.|  
|ORDINAL_POSITION|Int32|Numero di identificazione della colonna.|  
|COLUMN_DEFAULT|string|Il valore predefinito della colonna.|  
|IS_NULLABLE|string|Impostazione relativa al supporto di valori Null nella colonna. Se la colonna supporta i valori NULL, restituisce YES. In caso contrario, viene restituito NO.|  
|DATA_TYPE|string|Tipo di dati di sistema.|  
|CHARACTER_MAXIMUM_LENGTH|Int32|Lunghezza massima espressa in caratteri per i dati di tipo binario, carattere, text o image. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_OCTET_LENGTH|Int32|Lunghezza massima espressa in byte per i dati di tipo binario, carattere, text o image. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_PRECISION|Unsigned Byte|Precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_PRECISION_RADIX|Int16|Base di precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|NUMERIC_SCALE|Int32|Scala dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|DATETIME_PRECISION|Int16|Codice di sottotipo per i tipi di dati SQL-92 datetime e interval. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_SET_CATALOG|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito master, che indica il database in cui è posizionato il set di caratteri. Per gli altri tipi di dati viene restituito NULL.|  
|CHARACTER_SET_SCHEMA|string|Viene restituito sempre NULL.|  
|CHARACTER_SET_NAME|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito il nome univoco del set di caratteri. Per gli altri tipi di dati viene restituito NULL.|  
|COLLATION_CATALOG|string|Se la colonna presenta un tipo di dati carattere o testo, viene restituito master, che indica il database in cui sono definite le regole di confronto. Negli altri casi la colonna è NULL.|  
|IS_FILESTREAM|string|YES in caso di colonna con attributo FILESTREAM.<br /><br /> NO se la colonna non dispone dell'attributo FILESTREAM.|  
|IS_SPARSE|string|YES in caso di colonna di tipo sparse.<br /><br /> NO se la colonna non è di tipo sparse.|  
|IS_COLUMN_SET|string|YES in caso di colonna del set di colonne.<br /><br /> NO se la colonna non fa parte del set di colonne.|  

## <a name="users"></a>Utenti  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|uid|Int16|ID utente, univoco all'interno del database. 1 corrisponde al proprietario del database.|  
|nome_utente|string|Il nome utente o il nome del gruppo, univoco nel database.|  
|createdate|Datetime|Data in cui è stato aggiunto l'account.|  
|updatedate|Datetime|Data dell'ultima modifica dell'account.| 

## <a name="views"></a>Viste  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|TABLE_CATALOG|string|Catalogo della visualizzazione.|  
|TABLE_SCHEMA|string|Schema contenente la visualizzazione.|  
|TABLE_NAME|string|Nome della vista.|  
|CHECK_OPTION|string|Tipo di WITH CHECK OPTION. Se la visualizzazione originale è stata creata usando WITH CHECK OPTION, il tipo è CASCADE. In caso contrario restituisce NONE.|  
|IS_UPDATABLE|string|Specifica se è possibile aggiornare la vista. Restituisce sempre NO.|  

## <a name="viewcolumns"></a>ViewColumns  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|VIEW_CATALOG|string|Catalogo della visualizzazione.|  
|VIEW_SCHEMA|string|Schema contenente la visualizzazione.|  
|VIEW_NAME|string|Nome della vista.|  
|TABLE_CATALOG|string|Catalogo della tabella associata a questa visualizzazione.|  
|TABLE_SCHEMA|string|Il catalogo della tabella associata a questa visualizzazione.|  
|TABLE_NAME|string|Il nome della tabella associata alla visualizzazione. Tabella di base.|  
|COLUMN_NAME|string|Nome colonna.|  

## <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|ColumnName|DataType|Descrizione|  
|----------------|--------------|-----------------|  
|assembly_name|string|Nome del file dell'assembly.|  
|udt_name|string|Il nome della classe per l'assembly.|  
|version_major|Oggetto|Il numero di versione principale.|  
|version_minor|Oggetto|Numero di versione secondario.|  
|version_build|Oggetto|Numero di build.|  
|version_revision|Oggetto|Numero di revisione.|  
|culture_info|Oggetto|Informazioni sulle impostazioni cultura associate all'UDT.|  
|public_key|Oggetto|La chiave pubblica usata dall'assembly.|  
|is_fixed_length|Boolean|Specifica se la lunghezza del tipo è sempre uguale a max_length.|  
|max_length|Int16|La lunghezza massima del tipo in byte.|  
|Create_Date|Datetime|La data di creazione o di registrazione dell'assembly.|  
|Permission_set_desc|string|Il nome descrittivo del set di autorizzazioni o del livello di sicurezza dell'assembly.|  

## <a name="see-also"></a>Vedere anche

- [Recupero di informazioni dello schema del database](retrieving-database-schema-information.md)
- [Microsoft ADO.NET per SQL Server](microsoft-ado-net-sql-server.md)
