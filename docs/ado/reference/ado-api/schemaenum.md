---
description: SchemaEnum
title: SchemaEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SchemaEnum
helpviewer_keywords:
- SchemaEnum enumeration [ADO]
ms.assetid: 21c97651-297f-469f-b5b5-c48af72b62a8
author: rothja
ms.author: jroth
ms.openlocfilehash: 55a55d39117f2367b1b13c1a25b3785fe2d9cacc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170308"
---
# <a name="schemaenum"></a>SchemaEnum
Specifica il tipo di **Recordset** dello schema recuperato dal metodo [OpenSchema](./openschema-method.md) .  
  
## <a name="remarks"></a>Commenti  
 Ulteriori informazioni sulla funzione e sulle colonne restituite per ogni costante ADO sono disponibili negli argomenti dell' [Appendice B: set di righe dello schema](/previous-versions/windows/desktop/ms712921(v=vs.85)) del riferimento per programmatori OLE DB. Il nome di ogni argomento è elencato tra parentesi nella sezione Descrizione della tabella seguente.  
  
 Per informazioni aggiuntive sulla funzione e sulle colonne restituite per ogni costante ADO MD, vedere gli argomenti [OLE DB per gli oggetti OLAP e i set di righe dello schema](/previous-versions/windows/desktop/ms723056(v=vs.85)) nella OLE DB per la documentazione relativa all'elaborazione analitica online (OLAP). Il nome di ogni argomento è elencato tra parentesi nella colonna Descrizione della tabella seguente.  
  
 È possibile convertire i tipi di dati delle colonne nella documentazione di OLE DB in tipi di dati ADO facendo riferimento alla colonna Descrizione dell'argomento ADO [DataTypeEnum](./datatypeenum.md) . Ad esempio, un tipo di dati OLE DB di **DBTYPE_WSTR** è equivalente a un tipo di dati ADO di **adWChar**.  
  
 ADO genera risultati simili allo schema per le costanti, **adSchemaDBInfoKeywords** e **adSchemaDBInfoLiterals**. ADO crea un **Recordset**, quindi riempie ogni riga con i valori restituiti rispettivamente dai metodi **IDBInfo:: GetKeywords** e **IDBInfo:: GetLiteralInfo** . Altre informazioni su questi metodi sono disponibili nella sezione [IDBInfo](/previous-versions/windows/desktop/ms713663(v=vs.85)) del riferimento per programmatori OLE DB.  
  
|Costante|Valore|Descrizione|Colonne vincolo|  
|--------------|-----------|-----------------|------------------------|  
|**adSchemaAsserts**|0|Restituisce le asserzioni definite nel catalogo di proprietà di un determinato utente.<br /><br /> (Set di righe ASSERZIONi)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCatalogs**|1|Restituisce gli attributi fisici associati ai cataloghi accessibili dal sistema DBMS.<br /><br /> (Set di righe CATALOGs)|CATALOG_NAME|  
|**adSchemaCharacterSets**|2|Restituisce i set di caratteri definiti nel catalogo accessibili a un determinato utente.<br /><br /> (Set di righe CHARACTER_SETS)|CHARACTER_SET_CATALOG CHARACTER_SET_SCHEMA CHARACTER_SET_NAME|  
|**adSchemaCheckConstraints**|5|Restituisce i vincoli CHECK definiti nel catalogo di proprietà di un determinato utente.<br /><br /> (CHECK_CONSTRAINTS) Righe|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCollations**|3|Restituisce le regole di confronto dei caratteri definite nel catalogo accessibili a un determinato utente.<br /><br /> (Set di righe delle regole di confronto)|COLLATION_CATALOG COLLATION_SCHEMA COLLATION_NAME|  
|**adSchemaColumnPrivileges**|13|Restituisce i privilegi per le colonne di tabelle definite nel catalogo disponibili o concesse da un determinato utente.<br /><br /> (Set di righe COLUMN_PRIVILEGES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|**adSchemaColumns**|4|Restituisce le colonne delle tabelle, comprese le visualizzazioni, definite nel catalogo accessibili a un determinato utente.<br /><br /> (Set di righe COLUMNS)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaColumnsDomainUsage**|11|Restituisce le colonne definite nel catalogo e che dipendono dal dominio definito nel catalogo e il cui proprietario è un determinato utente.<br /><br /> (Set di righe COLUMN_DOMAIN_USAGE)|DOMAIN_CATALOG DOMAIN_SCHEMA DOMAIN_NAME COLUMN_NAME|  
|**adSchemaConstraintColumnUsage**|6|Restituisce le colonne utilizzate da vincoli referenziali, UNIQUE e CHECK, e le asserzioni definite nel catalogo e appartenenti a un determinato utente.<br /><br /> (Set di righe CONSTRAINT_COLUMN_USAGE)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaConstraintTableUsage**|7|Restituisce le tabelle utilizzate da vincoli referenziali, UNIQUE e CHECK, e le asserzioni definite nel catalogo appartenenti a un determinato utente.<br /><br /> (Set di righe CONSTRAINT_TABLE_USAGE)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaCubes**|32|Restituisce informazioni sui cubi disponibili in uno schema o sul catalogo, se il provider non supporta gli schemi.<br /><br /> (Set di righe cubi *)|CATALOG_NAME SCHEMA_NAME CUBE_NAME|  
|**adSchemaDBInfoKeywords**|30|Restituisce l'elenco delle parole chiave specifiche del provider.<br /><br /> (IDBInfo:: GetKeywords)|\<None>|  
|**adSchemaDBInfoLiterals**|31|Restituisce un elenco di valori letterali specifici del provider utilizzati nei comandi di testo.<br /><br /> (IDBInfo:: GetLiteralInfo)|\<None>|  
|**adSchemaDimensions**|33|Restituisce informazioni sulle dimensioni in un cubo specificato. Include una riga per ogni dimensione.<br /><br /> (Set di righe dimensioni)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_NAME DIMENSION_UNIQUE_NAME|  
|**adSchemaForeignKeys**|27|Restituisce le colonne di chiave esterna definite nel catalogo da un determinato utente.<br /><br /> (Set di righe FOREIGN_KEYS)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|**adSchemaHierarchies**|34|Restituisce informazioni sulle gerarchie disponibili in una dimensione.<br /><br /> (Set di righe GERARCHIe)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_NAME HIERARCHY_UNIQUE_NAME|  
|**adSchemaIndexes**|12|Restituisce gli indici definiti nel catalogo di proprietà di un determinato utente.<br /><br /> (Set di righe indici)|TABLE_CATALOG TABLE_SCHEMA TIPO DI INDEX_NAME TABLE_NAME|  
|**adSchemaKeyColumnUsage**|8|Restituisce le colonne definite nel catalogo che sono vincolate come chiavi da un determinato utente.<br /><br /> (Set di righe KEY_COLUMN_USAGE)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaLevels**|35|Restituisce informazioni sui livelli disponibili in una dimensione.<br /><br /> (Set di righe livelli)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_NAME LEVEL_UNIQUE_NAME|  
|**adSchemaMeasures**|36|Restituisce informazioni sulle misure disponibili.<br /><br /> (Set di righe misure)|CATALOG_NAME SCHEMA_NAME CUBE_NAME MEASURE_NAME MEASURE_UNIQUE_NAME|  
|**adSchemaMembers**|38|Restituisce informazioni sui membri disponibili.<br /><br /> (Set di righe MEMBERs)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME LEVEL_NUMBER MEMBER_NAME MEMBER_UNIQUE_NAME MEMBER_CAPTION MEMBER_TYPE. Per ulteriori informazioni, vedere OLE DB per l'elaborazione analitica in linea (OLAP).|  
|**adSchemaPrimaryKeys**|28|Restituisce le colonne di chiave primaria definite nel catalogo da un determinato utente.<br /><br /> (Set di righe PRIMARY_KEYS)|PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME|  
|**adSchemaProcedureColumns**|29|Restituisce informazioni relative alle colonne del rowset restituite da routine.<br /><br /> (Set di righe PROCEDURE_COLUMNS)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME COLUMN_NAME|  
|**adSchemaProcedureParameters**|26|Restituisce informazioni relative ai parametri e ai codici restituiti delle routine.<br /><br /> (Set di righe PROCEDURE_PARAMETERS)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|**adSchemaProcedures**|16|Restituisce le procedure definite nel catalogo di proprietà di un determinato utente.<br /><br /> (Set di righe PROCEDURES)|PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PROCEDURE_TYPE|  
|**adSchemaProperties**|37|Restituisce informazioni sulle proprietà disponibili per ogni livello della dimensione.<br /><br /> (Set di righe PROPERTIES)|CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME MEMBER_UNIQUE_NAME PROPERTY_TYPE PROPERTY_NAME|  
|**adSchemaProviderSpecific**|-1|Utilizzato se il provider definisce query dello schema non standard.|\<Provider specific>|  
|**adSchemaProviderTypes**|22|Restituisce i tipi di dati (base) supportati dal provider di dati.<br /><br /> (Set di righe PROVIDER_TYPES)|DATA_TYPE BEST_MATCH|  
|**AdSchemaReferentialConstraints**|9|Restituisce i vincoli referenziali definiti nel catalogo di proprietà di un determinato utente.<br /><br /> (Set di righe REFERENTIAL_CONSTRAINTS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaSchemata**|17|Restituisce gli schemi (oggetti di database) di proprietà di un determinato utente.<br /><br /> (Set di righe schemi)|CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|**adSchemaSQLLanguages**|18|Restituisce i livelli di conformità, le opzioni e le lingue supportate dall'implementazione SQL con cui sono elaborati i dati definiti nel catalogo.<br /><br /> (Set di righe SQL_LANGUAGES)|\<None>|  
|**adSchemaStatistics**|19|Restituisce le statistiche definite nel catalogo di proprietà di un determinato utente.<br /><br /> (Set di righe delle statistiche)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaTableConstraints**|10|Restituisce i vincoli di tabella definiti nel catalogo di proprietà di un determinato utente.<br /><br /> (Set di righe TABLE_CONSTRAINTS)|CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|**adSchemaTablePrivileges**|14|Restituisce i privilegi sulle tabelle definiti nel catalogo, disponibili o concessi da un determinato utente.<br /><br /> (Set di righe TABLE_PRIVILEGES)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|**adSchemaTables**|20|Restituisce le tabelle, comprese le visualizzazioni, definite nel catalogo e accessibili a un determinato utente.<br /><br /> (Set di righe Tables)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|**adSchemaTranslations**|21|Restituisce le conversioni di caratteri definite nel catalogo accessibili a un determinato utente.<br /><br /> (Set di righe TRANSLATIONs)|TRANSLATION_CATALOG TRANSLATION_SCHEMA TRANSLATION_NAME|  
|**adSchemaTrustees**|39|Riservato per utilizzi futuri.||  
|**adSchemaUsagePrivileges**|15|Restituisce i privilegi di utilizzo sugli oggetti definiti nel catalogo, disponibili o concessi da un determinato utente.<br /><br /> (Set di righe USAGE_PRIVILEGES)|OBJECT_CATALOG OBJECT_SCHEMA OBJECT_NAME UTENTE AUTORIZZATO AUTORIZZATO OBJECT_TYPE|  
|**adSchemaViewColumnUsage**|24|Restituisce le colonne in cui dipendono le tabelle visualizzate, definite nel catalogo e di proprietà di un determinato utente.<br /><br /> (Set di righe VIEW_COLUMN_USAGE)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
|**adSchemaViews**|23|Restituisce le visualizzazioni definite nel catalogo accessibili a un determinato utente.<br /><br /> (Set di righe VIEWS)|TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaViewTableUsage**|25|Restituisce le tabelle sulle quali si basano le tabelle visualizzate, definite nel catalogo e appartenenti a un determinato utente.<br /><br /> (Set di righe VIEW_TABLE_USAGE)|VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. Schema. ASSERTs|  
|AdoEnums. Schema. CATALOGs|  
|AdoEnums. Schema. CHARACTERSETS|  
|AdoEnums. Schema. CHECKCONSTRAINTS|  
|AdoEnums. Schema. COLLAtions|  
|AdoEnums. Schema. COLUMNPRIVILEGES|  
|AdoEnums. Schema. COLUMNS|  
|AdoEnums. Schema. COLUMNSDOMAINUSAGE|  
|AdoEnums. Schema. CONSTRAINTCOLUMNUSAGE|  
|AdoEnums. Schema. CONSTRAINTTABLEUSAGE|  
|AdoEnums. Schema. CUBEs|  
|AdoEnums. Schema. DBINFOKEYWORDS|  
|AdoEnums. Schema. DBINFOLITERALS|  
|AdoEnums. Schema. DIMENSIONs|  
|AdoEnums. Schema. FOREIGNKEYS|  
|AdoEnums. Schema. GERARCHIe|  
|AdoEnums. Schema. INDEXes|  
|AdoEnums. Schema. KEYCOLUMNUSAGE|  
|AdoEnums. Schema. LEVELs|  
|AdoEnums. Schema. MEASUREs|  
|AdoEnums. Schema. MEMBERs|  
|AdoEnums. Schema. PRIMARYKEYS|  
|AdoEnums. Schema. PROCEDURECOLUMNS|  
|AdoEnums. Schema. PROCEDUREPARAMETERS|  
|AdoEnums. Schema. PROCEDURES|  
|AdoEnums. Schema. PROPERTIES|  
|AdoEnums. Schema. PROVIDERSPECIFIC|  
|AdoEnums. Schema. PROVIDERTYPES|  
|AdoEnums. Schema. REFERENTIALCONTRAINTS|  
|AdoEnums. Schema. schemi|  
|AdoEnums. Schema. sqllanguages|  
|AdoEnums. Schema. STATISTICs|  
|AdoEnums. Schema. TABLECONSTRAINTS|  
|AdoEnums. Schema. TABLEPRIVILEGES|  
|AdoEnums. Schema. Tables|  
|AdoEnums. Schema. TRANSLATIONs|  
|AdoEnums. Schema. TRUSTee|  
|AdoEnums. Schema. USAGEPRIVILEGES|  
|AdoEnums. Schema. VIEWCOLUMNUSAGE|  
|AdoEnums. Schema. VIEWS|  
|AdoEnums. Schema. VIEWTABLEUSAGE|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo OpenSchema](./openschema-method.md)