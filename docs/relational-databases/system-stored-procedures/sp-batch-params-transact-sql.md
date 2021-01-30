---
description: sp_batch_params (Transact-SQL)
title: sp_batch_params (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_batch_params
- sp_batch_params_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_batch_params
ms.assetid: 7b92fe9e-e755-4b7a-8a15-822c58a813d3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7ba37a2e9559cd48da23c5ed7cb7758b0405e09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206621"
---
# <a name="sp_batch_params-transact-sql"></a>sp_batch_params (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce un set di righe che contiene informazioni sui parametri inclusi in un [!INCLUDE[tsql](../../includes/tsql-md.md)] batch. **sp_batch_params** analizza solo il batch specificato e restituisce informazioni sui valori dei parametri incorporati. e non esegue il batch, né modifica l'ambiente di esecuzione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_batch_params [ [ @tsqlbatch = ] 'tsqlbatch' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tsqlbatch = ] 'tsqlbatch'` Stringa Unicode contenente un' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione o un batch per il quale si desidera ottenere informazioni sui parametri. *TSqlBatch* è di **tipo nvarchar (max)** o convertibile in modo implicito in **nvarchar (max)**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**PARAMETER_NAME**|**sysname**|Nome del parametro rilevato nel batch da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**COLUMN_TYPE**|**smallint**|Questo campo restituisce uno dei valori seguenti:<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_INPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE<br /><br /> Questa colonna è sempre 0.|  
|**DATA_TYPE**|**smallint**|Tipo di dati del parametro (codice integer per un tipo di dati ODBC). Se non è possibile effettuare il mapping di questo tipo di dati a un tipo ISO, il valore è NULL. Il nome del tipo di dati nativo viene restituito nella colonna **type_name** . Il valore è sempre NULL.|  
|**TYPE_NAME**|**sysname**|Rappresentazione in forma di stringa del tipo di dati visualizzato dal sistema DBMS sottostante. Questo valore è NULL.|  
|**PRECISION**|**int**|Numero di cifre significative. Il valore restituito per la colonna **Precision** è in base 10.|  
|**LENGTH**|**int**|Dimensioni di trasferimento dei dati. Questo valore è NULL.|  
|**SCALA**|**smallint**|Numero di cifre a destra del separatore decimale. Questo valore è NULL.|  
|**RADIX**|**smallint**|Base per i tipi di dati numerici. Questo valore è NULL.|  
|**NULLABLE**|**smallint**|Specifica se i valori Null sono supportati o meno:<br /><br /> 1 = Per il parametro è possibile creare il tipo di dati con supporto per valori Null.<br /><br /> 0 = I valori Null non sono supportati.<br /><br /> Questo valore è NULL.|  
|**SQL_DATA_TYPE**|**smallint**|Valore del tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] visualizzato nel campo TYPE del descrittore. Questa colonna corrisponde alla colonna **DATA_TYPE**, tranne che per i tipi di dati **datetime** e ISO **interval**. In questa colonna viene sempre restituito un valore. Questo valore è NULL.|  
|**SQL_DATETIME_SUB**|**smallint**|Sottocodice **DateTime** o **intervallo** ISO se il valore di **SQL_DATA_TYPE** è SQL_DATETIME o SQL_INTERVAL. Per i tipi di dati diversi da **datetime** e **ISO interval**, questa colonna è NULL. Questo valore è NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Lunghezza massima, in byte, di un parametro di tipo **carattere** o **binario** . Per gli tutti gli altri tipi di dati, il valore di questa colonna è NULL. Il valore è sempre NULL.|  
|**ORDINAL_POSITION**|**int**|Posizione ordinale del parametro nel batch. Se il nome del parametro viene ripetuto più volte, questa colonna include il numero ordinale della prima occorrenza. Il primo parametro è associato al numero ordinale 1. In questa colonna viene sempre restituito un valore.|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per l'esecuzione di **sp_batch_params** viene concessa a **public**.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente una query viene passata a `sp_batch_params`. Il set di risultati enumera l'elenco dei valori dei parametri incorporati.  
  
```  
DECLARE @SQLString nvarchar(500);  
/* Build the SQL string */  
SET @SQLString =  
     N'SELECT * FROM AdventureWorks2012.HumanResources.Employee   
     WHERE BusinessEntityID = @BusinessEntityID';  
EXECUTE sp_batch_params @SQLString;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di stored procedure](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Procedure per l'esecuzione di stored procedure &#40;ODBC&#41;](../native-client-odbc-how-to/running-stored-procedures-call-stored-procedures.md)   
 [Esecuzione di stored procedure &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/stored-procedures-running.md)  
  
