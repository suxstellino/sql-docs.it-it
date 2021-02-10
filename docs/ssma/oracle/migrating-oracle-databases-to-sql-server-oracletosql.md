---
title: Migrazione di database Oracle a SQL Server (OracleToSQL) | Microsoft Docs
description: Usare questa procedura consigliata per eseguire la migrazione di database Oracle a SQL Server o al database SQL di Azure tramite SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 04/22/2018
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1d196dd6-4322-4c98-bb72-602c57d96134
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: df5bd55fb9c5490b8063712dbebe5328917e08b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063356"
---
# <a name="migrating-oracle-databases-to-sql-server-oracletosql"></a>Migrazione di database Oracle a SQL Server (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per Oracle è un ambiente completo che consente di eseguire rapidamente la migrazione di database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il database SQL di Azure o l'analisi delle sinapsi di Azure. Usando SSMA per Oracle, è possibile esaminare gli oggetti e i dati di database, valutare i database per la migrazione, migrare oggetti di database a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , database SQL di Azure o Azure sinapsi Analytics, quindi migrare i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , nel database SQL di Azure o in Azure sinapsi Analytics. Si noti che non è possibile eseguire la migrazione di SYS e di schemi Oracle di sistema.
  
## <a name="recommended-migration-process"></a>Processo di migrazione consigliato  
Per eseguire la migrazione di oggetti e dati da database Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , database SQL di Azure o Azure sinapsi Analytics, usare il processo seguente:
  
1.  [Creare un nuovo progetto SSMA](working-with-ssma-projects-oracletosql.md).  
  
    Dopo aver creato il progetto, è possibile impostare le opzioni di conversione del progetto, migrazione e mapping dei tipi. Per informazioni sulle impostazioni del progetto, vedere [impostazione delle opzioni del progetto &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md). Per informazioni su come personalizzare i mapping dei tipi di dati, vedere [mapping di tipi di dati Oracle e SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
2.  [Connettersi al server di database Oracle](connecting-to-oracle-database-oracletosql.md).  
  
3.  [Connettersi a un'istanza di SQL Server](connecting-to-sql-server-oracletosql.md).  
  
4.  Eseguire il [mapping di schemi di database Oracle a SQL Server schemi di database](mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).  
  
5.  Facoltativamente, [creare rapporti di valutazione](assessing-oracle-schemas-for-conversion-oracletosql.md) per valutare gli oggetti di database per la conversione e stimare il tempo di conversione.  
  
6.  [Convertire gli schemi di database Oracle in schemi SQL Server](converting-oracle-schemas-oracletosql.md).  
  
7.  [Caricare gli oggetti di database convertiti in SQL Server](loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
    Questa operazione può essere eseguita in uno dei modi seguenti:  
  
    -   Salvare uno script ed eseguirlo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Sincronizzare gli oggetti di database.  
  
8.  [Migrare i dati in SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md).  
  
9. Se necessario, aggiornare le applicazioni di database.  
  
## <a name="see-also"></a>Vedere anche  
[Installazione di SSMA per Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md)  
[Introduzione con SSMA per Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/getting-started-with-ssma-for-oracle-oracletosql.md)  
  
