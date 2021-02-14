---
description: PRINT (Transact-SQL)
title: PRINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PRINT_TSQL
- PRINT
dev_langs:
- TSQL
helpviewer_keywords:
- PRINT statement
- user-defined messages [SQL Server]
- messages [SQL Server], PRINT statement
- displaying user-defined messages
- viewing user-defined messages
- conditionally returning messages [SQL Server]
ms.assetid: 32ba0729-c4b5-4cfb-a5aa-e8b9402be028
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5950ce0b347eb4c1547b8524374c8845f1935f97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342558"
---
# <a name="print-transact-sql"></a>PRINT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce al client un messaggio definito dall'utente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
PRINT msg_str | @local_variable | string_expr  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *msg_str*  
 Costante di stringa Unicode o stringa di caratteri. Per altre informazioni, vedere [Costanti &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md).  
  
 **@** *local_variable*  
 Variabile costituita da un qualunque tipo di dati character valido. **@** _local\_variable_ deve essere di tipo **char**, **nchar**, **varchar** o **nvarchar** oppure deve supportare la conversione implicita a questi tipi di dati.  
  
 *string_expr*  
 Espressione che restituisce una stringa. Può includere variabili, funzioni e valori letterali concatenati. Per altre informazioni, vedere [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="remarks"></a>Commenti  
 Una stringa di messaggio può contenere al massimo 8.000 caratteri se è una stringa non Unicode e 4.000 caratteri se è una stringa Unicode. Le stringhe più lunghe vengono troncate. I tipi di dati **varchar(max)** e **nvarchar(max)** vengono troncati a tipi di dati di grandezza non superiore a **varchar(8000)** e **nvarchar(4000)**.  
  
 È possibile utilizzare anche RAISERROR per restituire messaggi. RAISERROR offre questi vantaggi rispetto a PRINT:  
  
-   RAISERROR supporta la sostituzione di argomenti in una stringa di messaggio di errore utilizzando un meccanismo basato sulla funzione printf della libreria standard del linguaggio C.  
  
-   RAISERROR consente di specificare un numero di errore univoco, un livello di gravità e un codice di stato, oltre al messaggio di testo.  
  
-   È possibile utilizzare RAISERROR per restituire messaggi definiti dall'utente creati tramite la stored procedure di sistema sp_addmessage.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-conditionally-executing-print-if-exists"></a>R. Stampa eseguita in modo condizionale (IF EXISTS)  
 Nell'esempio seguente viene utilizzata l'istruzione `PRINT` per restituire un messaggio in modo condizionale.  
  
```sql  
IF @@OPTIONS & 512 <> 0  
    PRINT N'This user has SET NOCOUNT turned ON.';  
ELSE  
    PRINT N'This user has SET NOCOUNT turned OFF.';  
GO  
```  
  
### <a name="b-building-and-displaying-a-string"></a>B. Compilazione e visualizzazione di una stringa  
 Nell'esempio seguente i risultati della funzione `GETDATE` vengono convertiti nel tipo di dati `nvarchar` e concatenati con il testo letterale restituito dall'istruzione `PRINT`.  
  
```sql  
-- Build the message text by concatenating  
-- strings and expressions.  
PRINT N'This message was printed on '  
    + RTRIM(CAST(GETDATE() AS NVARCHAR(30)))  
    + N'.';  
GO  
-- This example shows building the message text  
-- in a variable and then passing it to PRINT.  
-- This was required in SQL Server 7.0 or earlier.  
DECLARE @PrintMessage NVARCHAR(50);  
SET @PrintMessage = N'This message was printed on '  
    + RTRIM(CAST(GETDATE() AS NVARCHAR(30)))  
    + N'.';  
PRINT @PrintMessage;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-conditionally-executing-print"></a>C. Stampa eseguita in modo condizionale  
 Nell'esempio seguente viene utilizzata l'istruzione `PRINT` per restituire un messaggio in modo condizionale.  
  
```sql  
IF DB_ID() = 1  
    PRINT N'The current database is master.';  
ELSE  
    PRINT N'The current database is not master.';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
  
  

