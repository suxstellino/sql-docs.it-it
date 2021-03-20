---
description: SQLBindParameter
title: SQLBindParameter | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBindParameter function
ms.assetid: c302c87a-e7f4-4d2b-a0a7-de42210174ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a2b6beb7697957cebcffb1f58c36a2902872ac35
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751411"
---
# <a name="sqlbindparameter"></a>SQLBindParameter
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLBindParameter** può eliminare il carico di conversione dei dati quando viene utilizzato per fornire i dati per il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client, ottenendo un miglioramento significativo delle prestazioni per i componenti client e server delle applicazioni. Un altro vantaggio è costituito da un'inferiore perdita di precisione durante l'inserimento o l'aggiornamento di tipi di dati numerici approssimativi.  
  
> [!NOTE]  
>  Quando si inseriscono dati di tipo **char** e **WCHAR** in una colonna image, vengono usate le dimensioni dei dati passati, invece della dimensione dei dati dopo la conversione in un formato binario.  
  
 Se il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client rileva un errore in un singolo elemento di matrice di una matrice di parametri, continua a eseguire l'istruzione per gli elementi di matrice rimanenti. Se l'applicazione ha associato una matrice di elementi di stato dei parametri per l'istruzione, le righe di parametri che generano errori possono essere determinate dalla matrice.  
  
 Quando si utilizza il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, specificare SQL_PARAM_INPUT quando si associano parametri di input. Specificare solo SQL_PARAM_OUTPUT o SQL_PARAM_INPUT_OUTPUT quando si associano parametri di stored procedure definiti con la parola chiave OUTPUT.  
  
 [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md) non è affidabile con il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client se un elemento di matrice di una matrice di parametri associati causa un errore nell'esecuzione dell'istruzione. L'attributo SQL_ATTR_PARAMS_PROCESSED_PTR dell'istruzione ODBC indica il numero di righe elaborate prima del verificarsi dell'errore. L'applicazione può quindi attraversare la relativa matrice degli stati dei parametri per individuare il numero di istruzioni eseguite correttamente, se necessario.  
  
## <a name="binding-parameters-for-sql-character-types"></a>Associazione di parametri per i tipi di caratteri SQL  
 Se il tipo di dati SQL passato è un tipo di carattere, *ColumnSize* è la dimensione in caratteri (non byte). Se la lunghezza della stringa di dati in byte è maggiore di 8000, *ColumnSize* deve essere impostato su **SQL_SS_LENGTH_UNLIMITED**, a indicare che non esiste alcun limite per le dimensioni del tipo SQL.  
  
 Se, ad esempio, il tipo di dati SQL è **SQL_WVARCHAR**, *ColumnSize* non deve essere maggiore di 4000. Se la lunghezza effettiva dei dati è maggiore di 4000, *ColumnSize* deve essere impostato su **SQL_SS_LENGTH_UNLIMITED** in modo che il driver possa usare **nvarchar (max)** .  
  
## <a name="sqlbindparameter-and-table-valued-parameters"></a>SQLBindParameter e parametri con valori di tabella  
 Analogamente ad altri tipi di parametri, i parametri con valori di tabella sono associati da SQLBindParameter.  
  
 Una volta associato un parametro con valori di tabella, vengono associate anche le colonne del parametro. Per associare le colonne, chiamare [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) per impostare SQL_SOPT_SS_PARAM_FOCUS sull'ordinale del parametro con valori di tabella. Chiamare quindi SQLBindParameter per ogni colonna nel parametro con valori di tabella. Per tornare alle associazioni di parametro di livello principale, impostare SQL_SOPT_SS_PARAM_FOCUS su 0.  
  
 Per informazioni sul mapping dei parametri ai campi di descrizione per i parametri con valori di tabella, vedere [Binding e trasferimento dati di Table-Valued parametri e valori di colonna](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlbindparameter-support-for-enhanced-date-and-time-features"></a>Supporto di SQLBindParameter per le caratteristiche avanzate di data e ora  
 I valori dei parametri di tipo data/ora vengono convertiti come descritto in [conversioni da C a SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md). Si noti che i parametri di tipo **Time** e **DateTimeOffset** devono avere un *ValueType* specificato come **SQL_C_DEFAULT** o **SQL_C_BINARY** se vengono usate le strutture corrispondenti (**SQL_SS_TIME2_STRUCT** e **SQL_SS_TIMESTAMPOFFSET_STRUCT**).  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlbindparameter-support-for-large-clr-udts"></a>Supporto di SQLBindParameter per i tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLBindParameter** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [Pagina relativa alla funzione SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)  
  
