---
description: SQLColAttribute
title: SQLColAttribute | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColAttribute function
ms.assetid: a5387d9e-a243-4cfe-b786-7fad5842b1d6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4d62e13e7bed632ffed99ef0eab8ddf5cbb02a59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755091"
---
# <a name="sqlcolattribute"></a>SQLColAttribute
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  È possibile utilizzare **SQLColAttribute** per recuperare un attributo di una colonna del set di risultati per istruzioni ODBC preparate o eseguite. La chiamata di **SQLColAttribute** sulle istruzioni preparate causa un round trip a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client riceve i dati della colonna del set di risultati come parte dell'esecuzione dell'istruzione, quindi la chiamata di **SQLColAttribute** dopo il completamento di **SQLExecute** o **SQLExecDirect** non comporta un round trip del server.  
  
> [!NOTE]  
>  Gli attributi di identificazione di colonna ODBC non sono disponibili in tutti i set di risultati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Identificatore del campo|Descrizione|  
|----------------------|-----------------|  
|SQL_COLUMN_TABLE_NAME|Disponibile nei set di risultati recuperati da istruzioni che generano cursori server o in istruzioni SELECT eseguite contenenti una clausola FOR BROWSE.|  
|SQL_DESC_BASE_COLUMN_NAME|Disponibile nei set di risultati recuperati da istruzioni che generano cursori server o in istruzioni SELECT eseguite contenenti una clausola FOR BROWSE.|  
|SQL_DESC_BASE_TABLE_NAME|Disponibile nei set di risultati recuperati da istruzioni che generano cursori server o in istruzioni SELECT eseguite contenenti una clausola FOR BROWSE.|  
|SQL_DESC_CATALOG_NAME|nome del database. Disponibile nei set di risultati recuperati da istruzioni che generano cursori server o in istruzioni SELECT eseguite contenenti una clausola FOR BROWSE.|  
|SQL_DESC_LABEL|Disponibile in tutti i set di risultati. Il valore è identico al valore del campo SQL_DESC_NAME.<br /><br /> Il campo è di lunghezza zero solo se una colonna è il risultato di un'espressione e l'espressione non contiene un'assegnazione di etichetta.|  
|SQL_DESC_NAME|Disponibile in tutti i set di risultati. Il valore è identico al valore del campo SQL_DESC_LABEL.<br /><br /> Il campo è di lunghezza zero solo se una colonna è il risultato di un'espressione e l'espressione non contiene un'assegnazione di etichetta.|  
|SQL_DESC_SCHEMA_NAME|Nome del proprietario. Disponibile nei set di risultati recuperati da istruzioni che generano cursori server o in istruzioni SELECT eseguite contenenti una clausola FOR BROWSE.<br /><br /> Disponibile solo se viene specificato il nome del proprietario per la colonna nell'istruzione SELECT.|  
|SQL_DESC_TABLE_NAME|Disponibile nei set di risultati recuperati da istruzioni che generano cursori server o in istruzioni SELECT eseguite contenenti una clausola FOR BROWSE.|  
|SQL_DESC_UNNAMED|SQL_NAMED per tutte le colonne in un set di risultati, a meno che una colonna non sia il risultato di un'espressione che non contiene un'assegnazione di etichetta come parte dell'espressione. Quando SQL_DESC_UNNAMED restituisce SQL_UNNAMED, tutti gli attributi di identificazione di colonna ODBC contengono stringhe di lunghezza zero per la colonna.|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Il driver ODBC di Native client utilizza l'istruzione SET FMTONLY per ridurre l'overhead del server quando **SQLColAttribute** viene chiamato per istruzioni preparate ma non eseguite.  
  
 Per i tipi di valore di grandi dimensioni, **SQLColAttribute** restituirà i valori seguenti:  
  
|Identificatore del campo|Descrizione della modifica|  
|----------------------|---------------------------|  
|SQL_DESC_DISPLAY_SIZE|Si tratta del numero massimo di caratteri necessari per visualizzare dati dalla colonna. Per le colonne con tipo di dati per valori di grandi dimensioni, il valore restituito è SQL_SS_LENGTH_UNLIMITED.|  
|SQL_DESC_LENGTH|Restituisce la lunghezza effettiva della colonna nel set di risultati. Per le colonne con tipo di dati per valori di grandi dimensioni, il valore restituito è SQL_SS_LENGTH_UNLIMITED.|  
|SQL_DESC_OCTET_LENGTH|Restituisce la lunghezza massima di una colonna con tipo di dati per valori di grandi dimensioni. SQL_SS_LENGTH_UNLIMITED viene utilizzato per indicare dimensioni illimitate.|  
|SQL_DESC_PRECISION|Restituisce il valore SQL_SS_LENGTH_UNLIMITED per colonne con tipo di dati per valori di grandi dimensioni.|  
|SQL_DESC_TYPE|Restituisce SQL_VARCHAR, SQL_WVARCHAR e SQL_VARBINARY per tipi di dati per valori di grandi dimensioni.|  
|SQL_DESC_TYPE_NAME|Restituisce "varchar", "varbinary" e "nvarchar" per tipi di dati per valori di grandi dimensioni.|  
  
 Per tutte le versioni, gli attributi di colonna vengono indicati solo per il primo set di risultati quando più set di risultati vengono generati da un batch preparato di istruzioni SQL.  
  
 Gli attributi di colonna seguenti sono estensioni esposte dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client restituisce tutti i valori nel parametro *NumericAttrPtr* . I valori vengono restituiti come SDWORD (signed long) ad eccezione di SQL_CA_SS_COMPUTE_BYLIST, che è un puntatore a una matrice di WORD.  
  
|Identificatore del campo|Valore restituito|  
|----------------------|--------------------|  
|SQL_CA_SS_COLUMN_HIDDEN*|TRUE se la colonna a cui si fa riferimento fa parte di una chiave primaria nascosta creata per supportare un'istruzione Transact-SQL SELECT che contiene la clausola FOR BROWSE.|  
|SQL_CA_SS_COLUMN_ID|Posizione ordinale di una colonna di risultati della clausola COMPUTE all'interno dell'istruzione Transact-SQL SELECT corrente.|  
|SQL_CA_SS_COLUMN_KEY*|TRUE se la colonna a cui si fa riferimento è parte di una chiave primaria per la riga e se l'istruzione Transact-SQL SELECT contiene la clausola FOR BROWSE.|  
|SQL_CA_SS_COLUMN_OP|Numero intero che specifica l'operatore di aggregazione responsabile del valore in una colonna della clausola COMPUTE. Le definizioni dei valori integer sono incluse in sqlncli.h.|  
|SQL_CA_SS_COLUMN_ORDER|Posizione ordinale della colonna all'interno di una clausola ORDER BY di un'istruzione ODBC o Transact-SQL SELECT.|  
|SQL_CA_SS_COLUMN_SIZE|Lunghezza massima, in byte, necessaria per associare un valore di dati recuperato dalla colonna a una variabile SQL_C_BINARY.|  
|SQL_CA_SS_COLUMN_SSTYPE|Tipo di dati nativo dei dati archiviati nella colonna di SQL Server. Le definizioni dei valori dei tipi sono incluse in sqlncli.h.|  
|SQL_CA_SS_COLUMN_UTYPE|Tipo di dati di base del tipo di dati definito dall'utente della colonna di SQL Server. Le definizioni dei valori dei tipi sono incluse in sqlncli.h.|  
|SQL_CA_SS_COLUMN_VARYLEN|TRUE se i dati della colonna possono variare in lunghezza; in caso contrario, FALSE.|  
|SQL_CA_SS_COMPUTE_BYLIST|Puntatore a una matrice di WORD (unsigned short) che specifica le colonne utilizzate nella frase BY di una clausola COMPUTE. Se la clausola COMPUTE non specifica una frase BY, viene restituito un puntatore NULL.<br /><br /> Il primo elemento della matrice contiene il conteggio di colonne dell'elenco BY. I numeri ordinali di colonna sono elementi aggiuntivi.|  
|SQL_CA_SS_COMPUTE_ID|*ComputeID* di una riga che è il risultato di una clausola COMPUTE nell'istruzione Transact-SQL SELECT corrente.|  
|SQL_CA_SS_NUM_COMPUTES|Numero di clausole COMPUTE specificate nell'istruzione Transact-SQL SELECT corrente.|  
|SQL_CA_SS_NUM_ORDERS|Numero di colonne specificate in una clausola ORDER BY di un'istruzione ODBC o Transact-SQL SELECT.|  
  
 \*   Disponibile se l'attributo Statement SQL_SOPT_SS_HIDDEN_COLUMNS è impostato su SQL_HC_ON.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sono stati introdotti campi di descrizione specifici del driver per fornire informazioni aggiuntive per indicare rispettivamente il nome della raccolta XML Schema, il nome dello schema e il nome del catalogo. Tali proprietà non richiedono virgolette o un carattere di escape se contengono caratteri non alfanumerici. Nella tabella seguente sono inclusi i nuovi campi di descrizione:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_CATALOG_NAME|CharacterAttributePtr|Nome del catalogo in cui viene definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome del catalogo, questa variabile contiene una stringa vuota.<br /><br /> Queste informazioni vengono restituite dal campo del record SQL_DESC_SS_XML_SCHEMACOLLECTION_CATALOG_NAME del descrittore delle righe di implementazione, che è un campo di lettura/scrittura.|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|CharacterAttributePtr|Nome dello schema in cui viene definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome dello schema, questa variabile contiene una stringa vuota.<br /><br /> Queste informazioni vengono restituite dal campo del record SQL_DESC_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME del descrittore delle righe di implementazione, che è un campo di lettura/scrittura.|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_NAME|CharacterAttributePtr|Nome di una raccolta di XML Schema. Se non è possibile trovare il nome, questa variabile contiene una stringa vuota.<br /><br /> Queste informazioni vengono restituite dal campo del record SQL_DESC_SS_XML_SCHEMACOLLECTION_NAME del descrittore delle righe di implementazione, che è un campo di lettura/scrittura.|  
  
 In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sono stati inoltre introdotti nuovi campi di descrizione specifici del driver per fornire informazioni aggiuntive per una colonna con tipo definito dall'utente di un set di risultati o un parametro con tipo definito dall'utente di una stored procedure o di una query con parametri. Tali proprietà non richiedono virgolette o un carattere di escape se contengono caratteri non alfanumerici. Nella tabella seguente sono inclusi i nuovi campi di descrizione:  
  
|Nome colonna|Type|Descrizione|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_UDT_CATALOG_NAME|CharacterAttributePtr|Nome del catalogo contenente il tipo definito dall'utente.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|CharacterAttributePtr|Nome dello schema contenente il tipo definito dall'utente.|  
|SQL_CA_SS_UDT_TYPE_NAME|CharacterAttributePtr|Nome del tipo definito dall'utente.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|CharacterAttributePtr|Nome completo dell'assembly del tipo definito dall'utente.|  
  
 L'identificatore SQL_DESC_TYPE_NAME del campo di descrizione esistente viene utilizzato per indicare il nome del tipo definito dall'utente. Il campo SQL_DESC_TYPE per una colonna con tipo definito dall'utente è SQL_SS_UDT.  
  
## <a name="sqlcolattribute-support-for-enhanced-date-and-time-features"></a>Supporto di SQLColAttribute per le caratteristiche avanzate di data e ora  
 Per i valori restituiti per i tipi data/ora, vedere la sezione "informazioni restituite nei campi IRD" nei [metadati dei parametri e dei risultati](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md).  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlcolattribute-support-for-large-clr-udts"></a>Supporto di SQLColAttribute per tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLColAttribute** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlcolattribute-support-for-sparse-columns"></a>Supporto di SQLColAttribute per colonne di tipo sparse  
 SQLColAttribute esegue una query sul campo IRD (nuovo descrittore delle righe di implementazione) SQL_CA_SS_IS_COLUMN_SET per determinare se una colonna è una colonna **column_set** .  
  
 Per ulteriori informazioni, vedere [supporto di colonne di tipo sparse &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLColAttribute (funzione)](../../odbc/reference/syntax/sqlcolattribute-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  
