---
description: Campi di descrizione dei parametri con valori di tabella
title: Campi del descrittore di parametri Table-Valued | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), descriptor fields
ms.assetid: 4e009eff-c156-4d63-abcf-082ddd304de2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6c45999c04377bb88f48e7f97a85b46020673eaf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755051"
---
# <a name="table-valued-parameter-descriptor-fields"></a>Campi di descrizione dei parametri con valori di tabella
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il supporto per i parametri con valori di tabella include nuovi campi specifici di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nei descrittori di parametri delle applicazioni (APD, Application Parameter Descriptor) e nei descrittori di parametri di implementazione (IPD, Implementation Parameter Descriptor) ODBC.  
  
## <a name="remarks"></a>Commenti  
  
|Nome|Location|Tipo|Descrizione|  
|----------|--------------|----------|-----------------|  
|SQL_CA_SS_TYPE_NAME|IPD|SQLTCHAR*|Nome del tipo di server del parametro con valori di tabella.<br /><br /> Quando un nome di tipo di parametro con valori di tabella viene specificato in una chiamata a SQLBindParameter, deve sempre essere specificato come valore Unicode, anche nelle applicazioni compilate come applicazioni ANSI. Il valore utilizzato per il parametro *StrLen_or_IndPtr* deve essere SQL_NTS o la lunghezza della stringa del nome moltiplicato per sizeof (WCHAR).<br /><br /> Quando un nome di tipo di parametro con valori di tabella viene specificato tramite SQLSetDescField, può essere specificato usando un valore letterale conforme al modo in cui viene compilata l'applicazione. In Gestione driver ODBC verrà eseguita la conversione Unicode necessaria.|  
|SQL_CA_SS_TYPE_CATALOG_NAME (di sola lettura)|IPD|SQLTCHAR*|Il catalogo in cui è definito il tipo.|  
|SQL_CA_SS_TYPE_SCHEMA_NAME|IPD|SQLTCHAR*|Lo schema in cui è definito il tipo.|  
  
 Nelle applicazioni non deve essere impostato SQL_CA_SS_TYPE_CATALOG_NAME per i parametri con valori di tabella. In questo modo verrà restituito un errore SQL_ERROR, verrà registrato un record di diagnostica con SQLSTATE = HY091 e verrà visualizzato il messaggio "Identificatore del campo di descrizione non valido".  
  
 Di seguito sono indicati gli attributi di istruzione e i campi di intestazione di descrizione che si applicano ai parametri con valori di tabella quando lo stato attivo del parametro è impostato su un parametro con valori di tabella:  
  
|Nome|Location|Tipo|Descrizione|  
|----------|--------------|----------|-----------------|  
|SQL_ATTR_PARAMSET_SIZE<br /><br /> (equivale a SQL_DESC_ARRAY_SIZE in APD)|APD|SQLUINTEGER|Dimensione delle matrici di buffer per un parametro con valori di tabella. Si tratta del numero massimo di righe che può essere adattato dai buffer o della dimensione dei buffer in righe. Nel valore del parametro con valori di tabella stesso potrebbe essere presente un numero maggiore o minore di righe rispetto a quello che può essere contenuto nel buffer. Il valore predefinito è 1.<br /><br /> Nota: Se SQL_SOPT_SS_PARAM_FOCUS è impostato sul valore predefinito 0, SQL_ATTR_PARAMSET_SIZE fa riferimento all'istruzione e specifica il numero di set di parametri. Se SQL_SOPT_SS_PARAM_FOCUS è impostato sul numero ordinale di un parametro con valori di tabella, si riferisce al parametro con valori di tabella e specifica il numero di righe per set di parametri per il parametro con valori di tabella.|  
|SQL_ATTR_PARAM _BIND_TYPE|APD|SQLINTEGER|L'impostazione predefinita è SQL_PARAM_BIND_BY_COLUMN.<br /><br /> Per selezionare l'associazione per riga, questo campo è impostato sulla lunghezza della struttura o su un'istanza di un buffer che verrà associato a un set di righe del parametro con valori di tabella. Questa lunghezza deve includere lo spazio per tutte le colonne associate ed eventuale riempimento della struttura o del buffer. In questo modo si garantisce che, quando l'indirizzo di una colonna associata viene incrementato con la lunghezza specificata, il risultato punterà all'inizio della stessa colonna della riga successiva. Quando si usa l'operatore **sizeof** in ANSI C, questo comportamento è garantito.|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR|APD|SQLINTEGER*|Il valore predefinito è un puntatore null.<br /><br /> Se questo campo è non null, il driver risolve il riferimento dell'indicatore di misura, aggiunge il valore per il quale è stato risolto il riferimento a ognuno dei campi posticipati nel record del descrittore (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR e SQL_DESC_OCTET_LENGTH_PTR) e utilizza i nuovi valori dell'indicatore di misura per accedere ai valori dei dati.|  
  
 Questi campi sono validi solo per i parametri con valori di tabella e vengono ignorati per altri tipi di dati.  
  
 SQL_CA_SS_TYPE_NAME è facoltativo per le chiamate della stored procedure. Deve essere specificato per istruzioni SQL che non sono chiamate di stored procedure per consentire al server di determinare il tipo di parametro con valori di tabella.  
  
 Se il nome del tipo è obbligatorio e il tipo di tabella per il parametro con valori di tabella è definito in uno schema diverso dalla stored procedure, SQL_CA_SS_TYPE_SCHEMA_NAME deve essere specificato nel descrittore di parametri di implementazione (IPD). In caso contrario, il server non sarà in grado di determinare il tipo di parametro con valori di tabella Verrà generato un errore quando si chiama SQLExecute o SQLExecDirect. con SQLSTATE = 07006 e verrà visualizzato il messaggio "Violazione dell'attributo del tipo di dati".  
  
 Le colonne dei parametri con valori di tabella possono utilizzare l'associazione per riga o per colonna. L'impostazione predefinita è l'associazione per colonna. L'associazione per riga può essere specificata impostando SQL_ATTR_PARAM_BIND_TYPE e SQL_ATTR_ PARAM_BIND_OFFSET_PTR ed è analoga all'associazione per riga di colonne e parametri.  
  
 SQL_CA_SS_TYPE_CATALOG_NAME e SQL_CA_SS_TYPE_SCHEMA_NAME possono inoltre essere utilizzati per recuperare il catalogo e lo schema associati ai parametri del tipo CLR definito dall'utente. Si tratta di alternative agli attributi dello schema del catalogo specifico del tipo esistente per questi tipi.  
  
## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
