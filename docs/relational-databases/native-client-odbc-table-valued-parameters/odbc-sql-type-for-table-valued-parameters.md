---
description: Tipo SQL ODBC per parametri con valori di tabella
title: Tipo SQL ODBC per parametri di Table-Valued | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL_SS_TABLE
ms.assetid: 6725bfb9-5f10-4115-be09-fd9c9f5779ea
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 723177b3e61af5b5734f3c93dce7d6de72b8bb12
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748105"
---
# <a name="odbc-sql-type-for-table-valued-parameters"></a>Tipo SQL ODBC per parametri con valori di tabella
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il supporto per i parametri con valori di tabella viene fornito da un nuovo tipo ODBC SQL, SQL_SS_TABLE.  
  
## <a name="remarks"></a>Commenti  
 SQL_SS_TABLE non può essere convertito in un altro tipo di dati ODBC o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se SQL_SS_TABLE viene usato come tipo di dati C nel parametro *ValueType* di SQLBindParameter o viene effettuato un tentativo di impostare SQL_DESC_TYPE in un record del descrittore del parametro dell'applicazione (APD) per SQL_SS_TABLE, SQL_ERROR viene restituito e viene generato un record di diagnostica con SQLSTATE = HY003, "tipo di buffer dell'applicazione non valido".  
  
 Se SQL_DESC_TYPE è impostato su SQL_SS_TABLE in un record IPD e il record del descrittore del parametro dell'applicazione corrispondente non è SQL_C_DEFAULT, viene restituito SQL_ERROR e viene generato un record di diagnostica con SQLSTATE=HY003, "Tipo di buffer dall'applicazione non valido". Questa situazione può verificarsi con *ParameterType* di un SQLSetDescField, SQLSetDescRec o SQLBindParameter.  
  
 Se il parametro *targetType* è SQL_SS_TABLE quando si chiama SQLGetData, viene restituito SQL_ERROR e viene generato un record di diagnostica con SQLSTATE = HY003, "tipo di buffer dell'applicazione non valido".  
  
 Non è possibile associare una colonna di parametri con valori di tabella come tipo SQL_SS_TABLE. Se **SQLBindParameter** viene chiamato con *ParameterType* impostato su SQL_SS_TABLE, viene restituito SQL_ERROR e viene generato un record di diagnostica con SQLSTATE = HY004, "tipo di dati SQL non valido". Questa situazione può verificarsi anche con SQLSetDescField e SQLSetDescRec.  
  
 I valori della colonna di parametri con valori di tabella presentano le stesse opzioni di conversione dei dati dei parametri e delle colonne dei risultati.  
  
 Un parametro con valori di tabella può essere solo un parametro di input in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versioni successive. Se viene effettuato un tentativo di impostare SQL_DESC_PARAMETER_TYPE su un valore diverso da SQL_PARAM_INPUT tramite SQLBindParameter o SQLSetDescField, viene restituito SQL_ERROR e viene aggiunto un record di diagnostica all'istruzione con SQLSTATE = HY105 e il messaggio "tipo di parametro non valido".  
  
 Le colonne di parametri con valori di tabella non possono utilizzare SQL_DEFAULT_PARAM in *StrLen_or_IndPtr* perché i valori predefiniti per riga non sono supportati con i parametri con valori di tabella. È invece possibile impostare l'attributo della colonna SQL_CA_SS_COL_HAS_DEFAULT_VALUE su 1. Ciò significa che per tutte le righe della colonna saranno disponibili valori predefiniti. Se *StrLen_or_IndPtr* è impostato su SQL_DEFAULT_PARAM, SQLExecute o SQLExecDirect restituirà SQL_ERROR e verrà aggiunto un record di diagnostica all'istruzione con SQLSTATE = HY090 e il messaggio "lunghezza di stringa o di buffer non valida".  
  
## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
