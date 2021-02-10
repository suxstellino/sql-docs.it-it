---
title: Eseguire la migrazione di database MySQL a SQL Server-database SQL di Azure | Microsoft Docs
description: Usare questa procedura consigliata per eseguire la migrazione di database MySQL a SQL Server o al database SQL di Azure usando SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8006f9a0-394d-4238-8dc5-44255134628b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f4599389fd313447a926cfbc898735fc60f4823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014651"
---
# <a name="migrating-mysql-databases-to-sql-server---azure-sql-database-mysqltosql"></a>Migrazione di database MySQL a SQL Server-database SQL di Azure (MySQLToSql)
SQL Server Migration Assistant (SSMA) per MySQL è un ambiente completo che consente di migrare rapidamente i database MySQL a SQL Server o SQL Azure. Usando SSMA per MySQL, è possibile esaminare gli oggetti e i dati di database, valutare i database per la migrazione, migrare oggetti di database a SQL Server o SQL Azure, quindi migrare i dati in SQL Server o SQL Azure.  
  
## <a name="recommended-migration-process"></a>Processo di migrazione consigliato  
Per eseguire correttamente la migrazione di oggetti e dati da database MySQL a SQL Server o SQL Azure, usare il processo seguente:  
  
1.  [Utilizzo di progetti SSMA &#40;&#41;MySQLToSQL ](../../ssma/mysql/working-with-ssma-projects-mysqltosql.md).  
  
    Dopo aver creato il progetto, è possibile impostare le opzioni di conversione del progetto, migrazione e mapping dei tipi. Per altre informazioni sulle impostazioni del progetto, vedere [impostazione delle opzioni del progetto &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md). Per informazioni su come personalizzare i mapping dei tipi di dati, vedere [mapping di tipi di dati MySQL e SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
2.  [Connessione a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
3.  [Connessione a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
4.  [Mapping di database MySQL a schemi SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
5.  [Connessione al database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
  
6.  Facoltativamente, è possibile [valutare i database MySQL per la conversione &#40;&#41;MySQLToSQL ](../../ssma/mysql/assessing-mysql-databases-for-conversion-mysqltosql.md) per valutare gli oggetti di database per la conversione e stimare il tempo di conversione.  
  
7.  [Conversione dei database MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
8.  [Sincronizzazione](loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
9. Questa operazione può essere eseguita in uno dei modi seguenti:  
  
    -   Salvare uno script ed eseguirlo in SQL Server o SQL Azure.  
  
    -   Sincronizzare gli oggetti di database.  
  
10. [Migrazione dei dati MySQL in SQL Server-database SQL di Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
11. Se necessario, aggiornare le applicazioni di database.  
  
> [!NOTE]  
> Non è possibile eseguire la migrazione di schemi Information_schema e MySQL.  
  
## <a name="see-also"></a>Vedere anche  
[Installazione di SSMA per MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
[Introduzione con SSMA per MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/getting-started-with-ssma-for-mysql-mysqltosql.md)  
  
