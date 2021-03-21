---
description: SQLBindCol
title: SQLBindCol | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBindCol function
ms.assetid: fbd7ba20-d917-4ca9-b018-018ac6af9f98
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3f3ca406011ca16d207b58d5cffc35942ba2fb8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751441"
---
# <a name="sqlbindcol"></a>SQLBindCol
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Come regola generale, prendere in considerazione le implicazioni dell'utilizzo di **SQLBindCol** per la conversione dei dati. Le conversioni per associazione sono processi client. Se ad esempio viene recuperato un valore a virgola mobile associato a una colonna di tipo character, nel driver viene eseguita in locale la conversione da float a character quando viene recuperata una riga. La funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] CONVERT può essere utilizzata per riportare il costo della conversione dei dati nel server.  
  
 Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può restituire più set di righe di risultati in una singola esecuzione dell'istruzione. Ogni set di risultati deve essere associato separatamente. Per ulteriori informazioni sull'associazione per più set di risultati, vedere [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md).  
  
 Lo sviluppatore può associare colonne a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di dati C specifici usando il  valore TargetType **SQL_C_BINARY**. Le colonne associate a tipi specifici di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non sono portabili. I tipi di dati ODBC C specifici di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiti corrispondono alle definizioni del tipo di DB-Library e gli sviluppatori di DB-Library che si occupano della portabilità delle applicazioni potrebbero sfruttare questa caratteristica.  
  
 Il troncamento dei dati di Reporting è un processo dispendioso per il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client. È possibile evitare il troncamento assicurandosi che la larghezza di tutti i buffer di dati associati sia sufficiente per restituire i dati. Per i dati di tipo character, la larghezza deve includere lo spazio per un carattere di terminazione della stringa quando viene utilizzato il comportamento predefinito del driver per la terminazione della stringa. Ad esempio, l'associazione di una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonna **char (5)** a una matrice di cinque caratteri comporta un troncamento per ogni valore recuperato. L'associazione della stessa colonna a una matrice di sei caratteri evita il troncamento fornendo un elemento character in cui archiviare il terminatore null. [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) può essere utilizzato per recuperare in modo efficiente dati binari e di tipo carattere lungo senza troncamenti.  
  
 Per i tipi di dati con valori di grandi dimensioni, se il buffer fornito dall'utente non è sufficientemente grande da mantenere l'intero valore della colonna, viene restituito **SQL_SUCCESS_WITH_INFO** e la stringa "data; troncamento a destra. viene generato un avviso. L'argomento **StrLen_or_IndPtr** conterrà il numero di caratteri/byte archiviati nel buffer.  
  
## <a name="sqlbindcol-support-for-enhanced-date-and-time-features"></a>Supporto di SQLBindCol per le caratteristiche avanzate di data e ora  
 I valori della colonna dei risultati dei tipi data/ora vengono convertiti come descritto in [conversioni da SQL a C](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-sql-to-c.md). Si noti che per recuperare le colonne time e DateTimeOffset come strutture corrispondenti (**SQL_SS_TIME2_STRUCT** e **SQL_SS_TIMESTAMPOFFSET_STRUCT**), è necessario specificare *targetType* come **SQL_C_DEFAULT** o **SQL_C_BINARY**.  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlbindcol-support-for-large-clr-udts"></a>Supporto di SQLBindCol per tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLBindCol** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLBindCol (funzione)](../../odbc/reference/syntax/sqlbindcol-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
