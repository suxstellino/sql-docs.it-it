---
description: Tipi CLR definiti dall'utente di grandi dimensioni (ODBC)
title: Tipi di User-Defined CLR di grandi dimensioni (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, large user-defined types
- large user-defined types [ODBC]
ms.assetid: ddce337e-bb6e-4a30-b7cc-4969bb1520a9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f1e55ddcd51072688d4544a41b415f42bb79fd58
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748348"
---
# <a name="large-clr-user-defined-types-odbc"></a>Tipi CLR definiti dall'utente di grandi dimensioni (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono illustrate le modifiche apportate a ODBC in SQL Server Native Client per supportare i tipi CLR (Common Language Runtime) definiti dall'utente (UDT) di grandi dimensioni.  
  
 Per un esempio che illustra il supporto ODBC per i tipi definiti dall'utente CLR di grandi dimensioni, vedere [supporto per tipi definiti dall'utente](../../../relational-databases/native-client-odbc-how-to/support-for-large-udts.md)  
  
 Per ulteriori informazioni sul supporto di tipi definiti dall'utente CLR di grandi dimensioni in SQL Server Native Client, vedere [tipi di User-Defined CLR di grandi dimensioni](../../../relational-databases/native-client/features/large-clr-user-defined-types.md).  
  
## <a name="data-format"></a>Formato dati  
 SQL Server Native Client utilizza SQL_SS_LENGTH_UNLIMITED per indicare che le dimensioni di una colonna sono maggiori di 8.000 byte per i tipi LOB (Large Object). A partire da SQL Server 2008, lo stesso valore viene utilizzato per i tipi CLR definiti dall'utente quando le dimensioni sono maggiori di 8.000 byte.  
  
 I valori dei tipi definiti dall'utente vengono rappresentati come matrici di byte. Le conversioni da e verso le stringhe esadecimali sono supportate. I valori letterali vengono rappresentati come stringhe esadecimali con il prefisso "0x".  
  
 Nella tabella seguente viene illustrato il mapping dei tipi di dati nei parametri e nei set di risultati:  
  
|Tipo di dati di SQL Server|Tipo di dati SQL|valore|  
|--------------------------|-------------------|-----------|  
|tipo CLR definito dall'utente|SQL_SS_UDT|-151 (sqlncli.h)|  
  
 Nella tabella seguente vengono illustrati il tipo ODBC C e la struttura corrispondente. CLR UDT è essenzialmente un tipo **varbinary** con metadati aggiuntivi.  
  
|Tipo di dati SQL|Layout in memoria|Tipo di dati C|Valore (sqlext.h)|  
|-------------------|-------------------|-----------------|------------------------|  
|SQL_SS_UDT|SQLCHAR * (unsigned char \* )|SQL_C_BINARY|SQL_BINARY (-2)|  
  
## <a name="descriptor-fields-for-parameters"></a>Campi di descrizione per i parametri  
 Di seguito sono riportate le informazioni restituite nei campi IPD:  
  
|Campo di descrizione|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|  
|----------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_LOCAL_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_SCALE|0|0|  
|SQL_DESC_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|  
|SQL_CA_SS_UDT_CATALOG_NAME|Nome del catalogo che contiene il tipo definito dall'utente.|Nome del catalogo che contiene il tipo definito dall'utente.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|Nome dello schema che contiene il tipo definito dall'utente.|Nome dello schema che contiene il tipo definito dall'utente.|  
|SQL_CA_SS_UDT_TYPE_NAME|Nome del tipo definito dall'utente.|Nome del tipo definito dall'utente.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|Nome completo del tipo definito dall'utente.|Nome completo del tipo definito dall'utente.|  
  
 Per i parametri UDT, è necessario impostare sempre SQL_CA_SS_UDT_TYPE_NAME tramite **SQLSetDescField**. SQL_CA_SS_UDT_CATALOG_NAME e SQL_CA_SS_UDT_SCHEMA_NAME sono facoltativi.  
  
 Se il tipo definito dall'utente viene definito nello stesso database con uno schema diverso rispetto alla tabella, è necessario impostare SQL_CA_SS_UDT_SCHEMA_NAME.  
  
 Se il tipo definito dall'utente viene definito in un database diverso rispetto alla tabella, è necessario impostare SQL_CA_SS_UDT_CATALOG_NAME e SQL_CA_SS_UDT_SCHEMA_NAME.  
  
 Se sono presenti errori o omissioni nelle impostazioni per SQL_CA_SS_UDT_TYPE_NAME, SQL_CA_SS_UDT_CATALOG_NAME o SQL_CA_SS_UDT_SCHEMA_NAME, viene generato un record di diagnostica con l'identificativo SQLSTATE HY000 e il testo del messaggio specifico del server.  
  
## <a name="descriptor-fields-for-results"></a>Campi di descrizione per i risultati  
 Di seguito sono riportate le informazioni restituite nei campi IRD:  
  
|Campo di descrizione|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|  
|----------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_DISPLAY_SIZE|2 *n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_LITERAL_PREFIX|"0x"|"0x"|  
|SQL_DESC_LITERAL_SUFFIX|""|""|  
|SQL_DESC_LOCAL_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_SCALE|0|0|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|SQL_PRED_NONE|  
|SQL_DESC_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|  
|SQL_CA_SS_UDT_CATALOG_NAME|Nome del catalogo che contiene il tipo definito dall'utente.|Nome del catalogo che contiene il tipo definito dall'utente.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|Nome dello schema che contiene il tipo definito dall'utente.|Nome dello schema che contiene il tipo definito dall'utente.|  
|SQL_CA_SS_UDT_TYPE_NAME|Nome del tipo definito dall'utente.|Nome del tipo definito dall'utente.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|Nome completo del tipo definito dall'utente.|Nome completo del tipo definito dall'utente.|  
  
## <a name="column-metadata-returned-by-sqlcolumns-and-sqlprocedurecolumns-catalog-metadata"></a>Metadati della colonna restituiti da SQLColumns e SQLProcedureColumns (metadati del catalogo)  
 Per i tipi di dati definiti dall'utente vengono restituiti i valori di colonna seguenti:  
  
|Nome colonna|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|  
|-----------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|DATA_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|TYPE_NAME|Nome del tipo definito dall'utente.|Nome del tipo definito dall'utente.|  
|COLUMN_SIZE|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|BUFFER_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|DECIMAL_DIGITS|NULL|NULL|  
|SQL_DATA_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DATETIME_SUB|NULL|NULL|  
|CHAR_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SS_UDT_CATALOG_NAME|Nome del catalogo che contiene il tipo definito dall'utente.|Nome del catalogo che contiene il tipo definito dall'utente.|  
|SS_UDT_SCHEMA_NAME|Nome dello schema che contiene il tipo definito dall'utente.|Nome dello schema che contiene il tipo definito dall'utente.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Nome completo del tipo definito dall'utente.|Nome completo del tipo definito dall'utente.|  
  
 Le ultime tre colonne sono specifiche del driver. Vengono aggiunti dopo qualsiasi colonna definita da ODBC, ma prima di tutte le colonne specifiche del driver esistenti del set di risultati di SQLColumns o SQLProcedureColumns.  
  
 Non viene restituita alcuna riga da SQLGetTypeInfo, per i singoli tipi definiti dall'utente o per il tipo generico "UDT".  
  
## <a name="bindings-and-conversions"></a>Associazioni e conversioni  
 Di seguito sono riportate le conversioni supportate dai tipi di dati SQL ai tipi di dati C:  
  
|Conversione da e verso:|SQL_SS_UDT|  
|-----------------------------|------------------|  
|SQL_C_WCHAR|Supportato|  
|SQL_C_BINARY|Supportato|  
|SQL_C_CHAR|Supportato|  
  
 \* I dati binari vengono convertiti in una stringa esadecimale.  
  
 Di seguito sono riportate le conversioni supportate dai tipi di dati C ai tipi di dati SQL:  
  
|Conversione da e verso:|SQL_SS_UDT|  
|-----------------------------|------------------|  
|SQL_C_WCHAR|Supportato|  
|SQL_C_BINARY|Supportato|  
|SQL_C_CHAR|Supportato|  
  
 \* Viene eseguita la conversione da stringa esadecimale a dati binari.  
  
## <a name="sql_variant-support-for-udts"></a>Supporto di SQL_VARIANT per i tipi definiti dall'utente  
 I tipi definiti dall'utente non sono supportati nelle colonne SQL_VARIANT.  
  
## <a name="bcp-support-for-udts"></a>Supporto di BCP per i tipi definiti dall'utente  
 I valori dei tipi definiti dall'utente possono essere importati ed esportati solo come caratteri o valori binari.  
  
## <a name="downlevel-client-behavior-for-udts"></a>Comportamento dei client legacy per i tipi definiti dall'utente  
 I tipi definiti dall'utente sono soggetti al mapping dei tipi con i client legacy nel modo seguente:  
  
|Versione del server|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|  
|--------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL Server 2005|**UDT**|**varbinary(max)**|  
|SQL Server 2008 e versioni successive|**UDT**|**UDT**|  
  
## <a name="odbc-functions-supporting-large-clr-udts"></a>Funzioni ODBC che supportano i tipi CLR definiti dall'utente di grandi dimensioni  
 In questa sezione vengono illustrate le modifiche apportate alle funzioni ODBC di SQL Server Native Client per supportare i tipi CLR definiti dall'utente di grandi dimensioni.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 I valori della colonna risultato definito dall'utente vengono convertiti dai tipi di oggetto SQL ai tipi di databases, come descritto nella sezione "associazioni e conversioni", più indietro in questo argomento.  
  
### <a name="sqlbindparameter"></a>SQLBindParameter  
 I valori necessari per i tipi definiti dall'utente sono i seguenti:  
  
|Tipo di dati SQL|*ParameterType*|*ColumnSizePtr*|*DecimalDigitsPtr*|  
|-------------------|---------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 I valori restituiti per i tipi definiti dall'utente sono uguali a quelli descritti nella sezione "Campi di descrizione per i risultati" riportata in precedenza in questo argomento.  
  
### <a name="sqlcolumns"></a>SQLColumns  
 I valori restituiti per i tipi definiti dall'utente sono uguali a quelli descritti nella sezione "Metadati della colonna restituiti da SQLColumns e SQLProcedureColumns (metadati del catalogo)" riportata in precedenza in questo argomento.  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 I valori restituiti per i tipi definiti dall'utente sono i seguenti:  
  
|Tipo di dati SQL|*DataTypePtr*|*ColumnSizePtr*|*DecimalDigitsPtr*|  
|-------------------|-------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqldescribeparam"></a>SQLDescribeParam  
 I valori restituiti per i tipi definiti dall'utente sono i seguenti:  
  
|Tipo di dati SQL|*DataTypePtr*|*ColumnSizePtr*|*DecimalDigitsPtr*|  
|-------------------|-------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlfetch"></a>SQLFetch  
 I valori della colonna risultato definito dall'utente vengono convertiti dai tipi di oggetto SQL ai tipi di databases, come descritto nella sezione "associazioni e conversioni", più indietro in questo argomento.  
  
### <a name="sqlfetchscroll"></a>SQLFetchScroll  
 I valori della colonna risultato definito dall'utente vengono convertiti dai tipi di oggetto SQL ai tipi di databases, come descritto nella sezione "associazioni e conversioni", più indietro in questo argomento.  
  
### <a name="sqlgetdata"></a>SQLGetData  
 I valori della colonna risultato definito dall'utente vengono convertiti dai tipi di oggetto SQL ai tipi di databases, come descritto nella sezione "associazioni e conversioni", più indietro in questo argomento.  
  
### <a name="sqlgetdescfield"></a>SQLGetDescField  
 I campi di descrizione disponibili con i nuovi tipi sono descritti nelle sezioni "Campi di descrizione per i parametri" e "Campi di descrizione per i risultati" riportate in precedenza in questo argomento.  
  
### <a name="sqlgetdescrec"></a>SQLGetDescRec  
 I valori restituiti per i tipi definiti dall'utente sono i seguenti:  
  
|Tipo di dati SQL|Tipo|Sottotipo|Length|Precision|Scalabilità|  
|-------------------|----------|-------------|------------|---------------|-----------|  
|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT|0|*n*|n|0|  
|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|SQL_SS_UDT|0|SQL_SS_LENGTH_UNLIMITED (0)|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlgettypeinfo"></a>SQLGetTypeInfo  
 I valori restituiti per i tipi definiti dall'utente sono uguali a quelli descritti nella sezione "Metadati della colonna restituiti da SQLColumns e SQLProcedureColumns (metadati del catalogo)" riportata in precedenza in questo argomento.  
  
### <a name="sqlprocedurecolumns"></a>SQLProcedureColumns  
 I valori restituiti per i tipi definiti dall'utente sono uguali a quelli descritti nella sezione "Metadati della colonna restituiti da SQLColumns e SQLProcedureColumns (metadati del catalogo)" riportata in precedenza in questo argomento.  
  
### <a name="sqlputdata"></a>SQLPutData  
 I valori dei parametri UDT vengono convertiti da C ai tipi di oggetto SQL, come descritto nella sezione "associazioni e conversioni", più indietro in questo argomento.  
  
### <a name="sqlsetdescfield"></a>SQLSetDescField  
 I campi di descrizione disponibili con i nuovi tipi sono descritti nelle sezioni "Campi di descrizione per i parametri" e "Campi di descrizione per i risultati" riportate in precedenza in questo argomento.  
  
### <a name="sqlsetdescrec"></a>SQLSetDescRec  
 I valori consentiti per i tipi definiti dall'utente sono i seguenti:  
  
|Tipo di dati SQL|Tipo|Sottotipo|Length|Precision|Scalabilità|  
|-------------------|----------|-------------|------------|---------------|-----------|  
|SQL_SS_UDT<br /><br /> (lunghezza minore o uguale a 8.000 byte)|SQL_SS_UDT|0|*n*|*n*|0|  
|SQL_SS_UDT<br /><br /> (lunghezza maggiore di 8.000 byte)|SQL_SS_UDT|0|SQL_SS_LENGTH_UNLIMITED (0)|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlspecialcolumns"></a>SQLSpecialColumns  
 I valori restituiti per i tipi di dati definiti dall'utente delle colonne DATA_TYPE, TYPE_NAME, COLUMN_SIZE, BUFFER_LENGTH e DECIMAL_DIGTS sono uguali a quelli descritti nella sezione "Metadati della colonna restituiti da SQLColumns e SQLProcedureColumns (metadati del catalogo)" riportata in precedenza in questo argomento.  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi CLR definiti dall'utente di grandi dimensioni](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)  
  
  
