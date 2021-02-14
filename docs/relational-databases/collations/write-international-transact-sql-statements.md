---
description: Scrittura di istruzioni Transact-SQL internazionali
title: Scrittura di istruzioni Transact-SQL internazionali | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- writing international statements
- Transact-SQL international considerations
- international considerations [SQL Server], Transact-SQL
- Database Engine international considerations [SQL Server], Transact-SQL
- statements [SQL Server], international
- database international considerations [SQL Server], Transact-SQL
- dates [SQL Server], international considerations
ms.assetid: f0b10fee-27f7-45fe-aece-ccc3f63bdcdb
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8872cf6e593279a282c34868a2f019ac82e5bf01
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100339495"
---
# <a name="write-international-transact-sql-statements"></a>Scrittura di istruzioni Transact-SQL internazionali
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Le linee guida seguenti consentono di aumentare il grado di portabilità tra lingue diverse, nonché il supporto di più lingue, per i database e le applicazioni di database che utilizzano istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] .  

-   A partire da [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] usare:
    -   I tipi di dati **char**, **varchar** e **varchar(max)** con le regole di confronto abilitate per [UTF-8](../../relational-databases/collations/collation-and-unicode-support.md#utf8) e i dati codificati con UTF-8.
    -   I tipi di dati **nchar**, **nvarchar** e **nvarchar(max)** con le regole di confronto abilitate per [caratteri supplementari (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) e i dati codificati con UTF-16. L'uso di regole di confronto non SC comporta la codifica dei dati con UCS-2.      

    In questo modo si evitano problemi di conversione della tabella codici. Per altre considerazioni, vedere [Differenze nell'archiviazione tra UTF-8 e UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).  

-   Fino a [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] sostituire tutte le occorrenze dei tipi di dati **char**, **varchar** e **varchar(max)** con **nchar**, **nvarchar** e **nvarchar(max)**. Se si usano regole di confronto abilitate per [caratteri supplementari (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters), i dati vengono codificati con UTF-16. L'uso di regole di confronto non SC comporta la codifica dei dati con UCS-2. In questo modo si evitano problemi di conversione della tabella codici. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). 

    > [!IMPORTANT]
    > Il tipo di dati **text** è deprecato e non deve essere usato in nuovi progetti di sviluppo. Pianificare la conversione dei dati **text** al tipo di dati **varchar(max)**.
  
-   Quando si eseguono confronti e operazioni con mesi o giorni della settimana, usare le parti numeriche della data invece delle stringhe di nomi. I nomi dei mesi e dei giorni della settimana restituiti variano a seconda dell'impostazione della lingua. Ad esempio, `DATENAME(MONTH,GETDATE())` restituisce `May` quando la lingua è impostata su inglese (Stati Uniti), `Mai` se è impostato il tedesco e `mai` se è impostato il francese. Specificare quindi una funzione quale [DATEPART](../../t-sql/functions/datepart-transact-sql.md) che usa il numero invece del nome del mese. Utilizzare i nomi DATEPART quando si compilano i set di risultati da visualizzare all'utente, in quanto le stringhe di nomi sono più significative delle parti numeriche. Non creare tuttavia il codice di logica che dipende dai nomi visualizzati di una lingua specifica.  
  
-   Quando si specificano le date nei confronti o nell'input di istruzioni INSERT e UPDATE, utilizzare costanti che vengono interpretate nello stesso modo indipendentemente dall'impostazione della lingua:  
  
    -   Le applicazioni ADO, OLE DB e ODBC devono utilizzare le clausole di escape seguenti relative a timestamp, data e ora:  
  
         **{ ts'** _yyyy_ **-** _mm_ **-** _dd_ _hh_ **:** _mm_ **:** _ss_ [ **.** _fff_] **'}** such as: **{ ts'1998-09-24 10:02:20'}**  
  
         **{ d'** _yyyy_ **-** _mm_ **-** _dd_ **'}** such as: **{ d'1998-09-24'}**
  
         **{ t'** _hh_ **:** _mm_ **:** _ss_ **'}** such as: **{ t'10:02:20'}**  
  
    -   Nelle applicazioni che utilizzano altre API, oppure script, stored procedure e trigger di [!INCLUDE[tsql](../../includes/tsql-md.md)] , è necessario utilizzare le stringhe numeriche non separate, Ad esempio, *aaaammgg* come 19980924.  
  
    -   Nelle applicazioni che usano altre API oppure script, stored procedure e trigger di [!INCLUDE[tsql](../../includes/tsql-md.md)] è anche possibile usare l'istruzione [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) con un parametro di stile esplicito per tutte le conversioni tra i tipi di dati **time**, **date**, **smalldate**, **datetime**, **datetime2** e **datetimeoffset** e i tipi di dati stringa di caratteri. L'istruzione seguente viene interpretata nello stesso modo indipendentemente dalle impostazioni di connessione della lingua o del formato della data:  
  
        ```sql  
        SELECT *  
        FROM AdventureWorks2012.Sales.SalesOrderHeader  
        WHERE OrderDate = CONVERT(DATETIME, '20060719', 101)  
        ```  
  
## <a name="see-also"></a>Vedere anche
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)     
[DATEPART &#40;Transact-SQL&#41;](../../t-sql/functions/datepart-transact-sql.md)        
[Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)      
