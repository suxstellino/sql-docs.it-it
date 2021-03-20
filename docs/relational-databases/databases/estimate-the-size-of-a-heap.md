---
title: Stimare le dimensioni di un heap | Microsoft Docs
description: Usare questa procedura per valutare la quantità di spazio necessaria per l'archiviazione dei dati in un heap in SQL Server.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- disk space [SQL Server], indexes
- estimating heap size
- size [SQL Server], heap
- space [SQL Server], indexes
- heaps
ms.assetid: 81fd5ec9-ce0f-4c2c-8ba0-6c483cea6c75
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aa3910c7ff3b66474835f2d3335e0cdac44430bf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748051"
---
# <a name="estimate-the-size-of-a-heap"></a>Stima delle dimensioni di un heap
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  I seguenti passaggi sono utilizzabili per valutare la quantità di spazio necessaria per l'archiviazione dei dati in un heap:  
  
1.  Specificare il numero di righe che verranno incluse nella tabella:  
  
     **_Num_Rows_**  = numero di righe della tabella  
  
2.  Specificare il numero di colonne di lunghezza fissa e e variabile e calcolare lo spazio necessario per la loro archiviazione:  
  
     Calcolare lo spazio occupato da ognuno di questi gruppi di colonne all'interno della riga di dati. Le dimensioni di una colonna dipendono dal tipo di dati e dalla lunghezza specificata.  
  
     **_Num_Cols_**  = numero totale di colonne (a lunghezza fissa e a lunghezza variabile)  
  
     **_Fixed_Data_Size_**  = dimensioni totali in byte di tutte le colonne a lunghezza fissa  
  
     **_Num_Variable_Cols_**  = numero di colonne a lunghezza variabile  
  
     **_Max_Var_Size_**  = dimensioni massime totali in byte di tutte le colonne a lunghezza variabile  
  
3.  Parte della riga, nota come mappa di bit null, è riservata alla gestione del supporto dei valori Null in una colonna. Calcolarne le dimensioni:  
  
     **_Null_Bitmap_**  = 2 + ((**_Num_Cols_** + 7) / 8)  
  
     Solo l'integrale di questa espressione dovrebbe essere utilizzato. Eliminare le parti restanti.  
  
4.  Calcolare le dimensioni dei dati di lunghezza variabile:  
  
     Se la tabella include colonne di lunghezza variabile, determinare la quantità di spazio utilizzata per l'archiviazione delle colonne nella riga:  
  
     **_Variable_Data_Size_**  = 2 + (**_Num_Variable_Cols_** x 2) + **_Max_Var_Size_**  
  
     I byte aggiunti a **_Max_Var_Size_** servono a tenere traccia di ogni colonna a lunghezza variabile. Questa formula si basa sul presupposto che tutte le colonne a lunghezza variabile siano piene al 100%. Se si prevede che verrà usata una percentuale inferiore dello spazio di archiviazione delle colonne a lunghezza variabile, è possibile modificare il valore di **_Max_Var_Size_** in base a tale percentuale per ottenere una stima più accurata delle dimensioni complessive della tabella.  
  
    > [!NOTE]  
    >  È possibile combinare colonne **varchar**, **nvarchar**, **varbinary** o **sql_variant** che fanno eccedere gli 8.060 byte per la larghezza totale definita della tabella. La lunghezza di ognuna di queste colonne deve comunque essere compresa nel limite di 8.000 byte per una colonna **varchar**, **nvarchar, varbinary** o **sql_variant**. Le larghezze combinate di tali colonne possono tuttavia superare il limite di 8.060 byte in una tabella.  
  
     Se non sono presenti colonne di lunghezza variabile, impostare **_Variable_Data_Size_** su 0.  
  
5.  Calcolare le dimensioni totali della riga:  
  
     **_Row_Size_**  = **_Fixed_Data_Size_** + **_Variable_Data_Size_** + **_Null_Bitmap_** + 4  
  
     Il valore 4 nel formula è l'overhead dell'intestazione di riga della riga di dati.  
  
6.  Calcolare il numero di righe per pagina (8096 byte liberi per pagina):  
  
     **_Rows_Per_Page_**  = 8096 / (**_Row_Size_** + 2)  
  
     Poiché le righe non si estendono su più pagine, il numero di righe per pagina deve essere arrotondato alla riga completa più vicina. Il valore 2 nella formula è per la voce di riga nella matrice di slot della pagina.  
  
7.  Calcolare il numero di pagine necessario per archiviare tutte le righe:  
  
     **_Num_Pages_**  = **_Num_Rows_** / **_Rows_Per_Page_**  
  
     Il numero di pagine stimato deve essere arrotondato alla pagina intera più vicina.  
  
8.  Infine, calcolare la quantità di spazio necessaria per archiviare i dati nell'heap (8192 byte totali per pagina):  

     Dimensioni dell'heap (byte) = 8192 x **_Num_Pages_**  
  
 Il calcolo non prende in considerazione i fattori seguenti:  
  
-   Partizionamento  
  
     L'overhead dello spazio derivante dal partizionamento è minimo, ma difficile da calcolare. Non è fondamentale includerlo.  
  
-   Pagine di allocazione  
  
     Esiste almeno una pagina IAM utilizzata per tenere traccia delle pagine allocate su un heap, ma l'overhead dello spazio è minimo e non è presente alcun algoritmo per calcolare in modo deterministico l'esatto numero di pagine IAM che verranno utilizzate.  
  
-   Valori LOB  
  
     L'algoritmo per determinare con esattezza la quantità di spazio usata per archiviare i tipi di dati LOB **varchar(max)** , **varbinary(max)** , **nvarchar(max)** , **text**, **ntextxml** e **image** è complesso. È sufficiente aggiungere soltanto le dimensioni medie dei valori LOB previsti e aggiungerle alle dimensioni totali dell'heap.  
  
-   Compressione  
  
     Non è possibile pre-calcolare la dimensione di un heap compresso.  
  
-   Colonne di tipo sparse  
  
     Per informazioni sui requisiti di spazio delle colonne di tipo sparse, vedere [Utilizzo di colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Heap &#40;tabelle senza indici cluster&#41;](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md)   
 [Descrizione di indici cluster e non cluster.](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [Creare indici cluster](../../relational-databases/indexes/create-clustered-indexes.md)   
 [Creare indici non cluster](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [Stima delle dimensioni di una tabella](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [Stima delle dimensioni di un indice cluster](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [Stima delle dimensioni di un indice non cluster](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [Stima delle dimensioni di un database](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
