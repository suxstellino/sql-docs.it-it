---
description: binary e varbinary (Transact-SQL)
title: binary e varbinary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- binary_TSQL
- varbinary_TSQL
- binary
- varbinary
dev_langs:
- TSQL
helpviewer_keywords:
- varbinary data type
- binary [SQL Server], about binary data type
ms.assetid: bcce65f9-10db-4b3e-bfaf-dfc06c6f820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49c20ae59ec25668725b3fa763eb4f81304d0962
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737111"
---
# <a name="binary-and-varbinary-transact-sql"></a>binary e varbinary (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipi di dati binary a lunghezza fissa o variabile.
  
## <a name="arguments"></a>Argomenti

**binary** [ ( _n_ ) ] Dati binari a lunghezza fissa con lunghezza di _n_ byte, dove _n_ rappresenta un valore compreso tra 1 e 8.000. Le dimensioni di archiviazione corrispondono a _n_ byte.
  
**varbinary** [ ( _n_ | **max**) ] Dati binari a lunghezza variabile. _n_ può essere un valore compreso tra 1 e 8.000. **max** indica che la capacità di memorizzazione massima è di 2^31-1 byte. Le dimensioni dello spazio di archiviazione corrispondono alla lunghezza effettiva dei dati immessi + 2 byte. È possibile che la lunghezza dei dati immessi sia pari a 0 byte. L'equivalente di ANSI SQL per **varbinary** è **binary varying**.
  
## <a name="remarks"></a>Osservazioni  
Se non si specifica _n_ in un'istruzione di definizione dei dati o di dichiarazione di variabili, la lunghezza predefinita è 1. Se non si specifica _n_ con la funzione CAST, la lunghezza predefinita è 30.

| Tipo di dati | Usare se... |
| --- | --- |
| **binary** | le dimensioni delle voci di dati delle colonne sono consistenti.|
| **varbinary** | le dimensioni delle voci di dati delle colonne presentano notevoli differenze.|
| **varbinary(max)** | le voci di dati delle colonne superano gli 8.000 byte.|


## <a name="converting-binary-and-varbinary-data"></a>Conversione dei dati di tipo binary e varbinary
In caso di conversione da un tipo di dati stringa a un tipo di dati **binary** o **varbinary** di lunghezza diversa, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue il riempimento o il troncamento dei dati a destra. Tali tipi di dati stringa sono:

* **char** 
* **varchar**
* **nchar**
* **nvarchar**
* **binary**
* **varbinary**
* **text**
* **ntext**
* **image**

Nella conversione di altri tipi di dati nel tipo **binary** o **varbinary**, viene eseguito il riempimento o il troncamento dei dati a sinistra. Il riempimento viene eseguito utilizzando zero esadecimali.
  
La conversione nei tipi di dati **binary** e **varbinary** è utile quando i dati **binary** risultano i dati più semplici da spostare. A un certo punto si potrebbe convertire un tipo valore in un valore binario di dimensioni sufficienti e quindi riconvertirlo. Questa conversione genera sempre lo stesso valore se entrambe le conversioni vengono eseguite nella stessa versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La rappresentazione binaria di un valore può variare da una versione all'altra di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
È possibile convertire **int**, **smallint** e **tinyint** in **binary** o **varbinary**. Se si riconverte il valore **binary** in un valore intero ed è stato eseguito il troncamento, il valore ottenuto sarà diverso dal valore intero originale. L'istruzione SELECT seguente, ad esempio, mostra che il valore intero `123456` viene in genere archiviato come `0x0001e240` binario:
  
```sql
SELECT CAST( 123456 AS BINARY(4) );  
```  
  
L'istruzione `SELECT` seguente illustra il troncamento automatico delle cifre iniziali se il valore di destinazione **binary** è troppo piccolo per l'archiviazione dell'intero valore, in modo che lo stesso numero possa essere archiviato sotto forma di `0xe240`:
  
```sql
SELECT CAST( 123456 AS BINARY(2) );  
```  
  
Il batch seguente illustra come il troncamento automatico possa influire sulle operazioni aritmetiche senza tuttavia generare errori:
  
```sql
DECLARE @BinaryVariable2 BINARY(2);  
  
SET @BinaryVariable2 = 123456;  
SET @BinaryVariable2 = @BinaryVariable2 + 1;  
  
SELECT CAST( @BinaryVariable2 AS INT);  
GO  
```  
  
Il risultato finale è `57921`, non `123457`.
  
> [!NOTE]  
>  È possibile che le conversioni da un tipo di dati ai tipi di dati **binary** e viceversa eseguite in versioni diverse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] producano risultati diversi.  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversione del tipo di dati &#40;motore di database&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
