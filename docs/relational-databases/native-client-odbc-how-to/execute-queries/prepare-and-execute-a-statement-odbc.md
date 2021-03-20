---
description: Preparare ed eseguire un'istruzione (ODBC)
title: Preparare ed eseguire un'istruzione (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
- statement preparation
ms.assetid: 0adecc63-4da5-486c-bc48-09a004a2fae6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 94270caf9aae58a48ec9905062b7f8f317cdfeb3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753721"
---
# <a name="prepare-and-execute-a-statement-odbc"></a>Preparare ed eseguire un'istruzione (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-prepare-a-statement-once-and-then-execute-it-multiple-times"></a>Per preparare un'istruzione da eseguire più volte  
  
1.  Chiamare la [funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md) per preparare l'istruzione.  
  
2.  Facoltativamente, chiamare [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) per determinare il numero di parametri nell'istruzione preparata.  
  
3.  Per ogni parametro dell'istruzione preparata è possibile:  
  
    -   Chiamare [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) per ottenere informazioni sui parametri.  
  
    -   Associare ogni parametro a una variabile di programma usando [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md). Configurare i parametri data-at-execution.  
  
4.  Per ogni esecuzione di un'istruzione preparata:  
  
    -   Se nell'istruzione sono inclusi marcatori di parametro, inserire i valori dei dati nel buffer di parametri associato.  
  
    -   Chiamare [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) per eseguire l'istruzione preparata.  
  
    -   Se vengono utilizzati parametri di input data-at-execution, [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) restituisce SQL_NEED_DATA. Inviare i dati in blocchi usando [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) e [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md).  
  
### <a name="to-prepare-a-statement-with-column-wise-parameter-binding"></a>Per preparare un'istruzione con associazione di parametri a livello di colonna  
  
1.  Chiamare [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) per impostare gli attributi seguenti:  
  
    -   Impostare SQL_ATTR_PARAMSET_SIZE sul numero di set (S) di parametri.  
  
    -   Impostare SQL_ATTR_PARAM_BIND_TYPE su SQL_PARAMETER_BIND_BY_COLUMN.  
  
    -   Impostare l'attributo SQL_ATTR_PARAMS_PROCESSED_PTR in modo che punti a una variabile SQLUINTEGER che contenga il numero di parametri elaborati.  
  
    -   Impostare SQL_ATTR_PARAMS_STATUS_PTR in modo che punti a una matrice [S] di variabili SQLUSSMALLINT che contenga gli indicatori di stato dei parametri.  
  
2.  Chiamare SQLPrepare per preparare l'istruzione.  
  
3.  Facoltativamente, chiamare [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) per determinare il numero di parametri nell'istruzione preparata.  
  
4.  Facoltativamente, per ogni parametro nell'istruzione preparata, chiamare SQLDescribeParam per ottenere informazioni sui parametri.  
  
5.  Per ogni marcatore di parametro:  
  
    -   Allocare una matrice di buffer di S parametri per archiviare i valori dei dati.  
  
    -   Allocare una matrice di buffer di S parametri per archiviare le lunghezze dei dati.  
  
    -   Chiamare SQLBindParameter per associare il valore dei dati del parametro e le matrici di lunghezza dei dati al parametro dell'istruzione.  
  
    -   Se il parametro è un testo data-at-execution o un parametro di immagine, configurarlo.  
  
    -   Se vengono utilizzati parametri data-at-execution, configurarli.  
  
6.  Per ogni esecuzione di un'istruzione preparata:  
  
    -   Inserire le S lunghezze e gli S valori di dati nelle matrici di parametri associate.  
  
    -   Chiamare SQLExecute per eseguire l'istruzione preparata.  
  
    -   Se vengono utilizzati parametri di input data-at-execution, SQLExecute restituisce SQL_NEED_DATA. Inviare i dati in blocchi usando SQLParamData e SQLPutData.  
  
### <a name="to-prepare-a-statement-with-row-wise-bound-parameters"></a>Per preparare un'istruzione con associazione di parametri a livello di riga  
  
1.  Allocare una matrice [S] di strutture, dove S è il numero di set di parametri. Nella struttura è presente un elemento per ogni parametro e ogni elemento è costituito da due parti:  
  
    -   La prima parte è una variabile del tipo di dati appropriato che contiene i dati dei parametri.  
  
    -   La seconda parte è una variabile SQLINTEGER che deve contenere l'indicatore di stato.  
  
2.  Chiamare [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) per impostare gli attributi seguenti:  
  
    -   Impostare SQL_ATTR_PARAMSET_SIZE sul numero di set (S) di parametri.  
  
    -   Impostare SQL_ATTR_PARAM_BIND_TYPE sulla dimensione della struttura allocata nel Passaggio 1.  
  
    -   Impostare l'attributo SQL_ATTR_PARAMS_PROCESSED_PTR in modo che punti a una variabile SQLUINTEGER che contenga il numero di parametri elaborati.  
  
    -   Impostare SQL_ATTR_PARAMS_STATUS_PTR in modo che punti a una matrice [S] di variabili SQLUSSMALLINT che contenga gli indicatori di stato dei parametri.  
  
3.  Chiamare SQLPrepare per preparare l'istruzione.  
  
4.  Per ogni marcatore di parametro, chiamare SQLBindParameter per puntare il valore dei dati del parametro e il puntatore alla lunghezza dei dati alle variabili nel primo elemento della matrice di strutture allocate nel passaggio 1. Se il parametro è di tipo data-at-execution, configurarlo.  
  
5.  Per ogni esecuzione di un'istruzione preparata:  
  
    -   Inserire i valori dei dati nella matrice di buffer dei parametri associati.  
  
    -   Chiamare SQLExecute per eseguire l'istruzione preparata. Il driver esegue in modo efficace l'istruzione SQL S volte, una volta per ogni set di parametri.  
  
    -   Se vengono utilizzati parametri di input data-at-execution, SQLExecute restituisce SQL_NEED_DATA. Inviare i dati in blocchi usando SQLParamData e SQLPutData.  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure relative all'esecuzione di query &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
