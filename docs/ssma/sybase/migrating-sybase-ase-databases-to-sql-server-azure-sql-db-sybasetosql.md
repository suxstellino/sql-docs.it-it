---
title: Eseguire la migrazione dei database Sybase ASE a SQL Server-database SQL di Azure | Microsoft Docs
description: Usare questo processo consigliato per eseguire la migrazione dei database di SAP Adaptive Server Enterprise a SQL Server o al database SQL di Azure tramite SQL Server Migration Assistant (SSMA).
ms.custom: ''
ms.date: 11/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ed7952d4-8331-44d7-bccf-3440e17238b2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 222ce0d6d9a91a0aa97f9dc29dee06fe01754a2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074564"
---
# <a name="migrating-sap-ase-databases-to-sql-server---azure-sql-database-sybasetosql"></a>Migrazione dei database SAP ASE a SQL Server-database SQL di Azure (SybaseToSQL)
SQL Server Migration Assistant (SSMA) per SAP Adaptive Server Enterprise (ASE) è un ambiente completo che consente di eseguire rapidamente la migrazione dei database SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Con SSMA per SAP ASE è possibile esaminare gli oggetti e i dati di database, valutare i database per la migrazione, eseguire la migrazione di oggetti di database al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure e quindi migrare i dati in un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure o.  
  
## <a name="recommended-migration-process"></a>Processo di migrazione consigliato  
Per eseguire correttamente la migrazione di oggetti e dati dai database SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure, usare il processo seguente:  
  
1.  [Creare un nuovo progetto SSMA](working-with-ssma-projects-sybasetosql.md).  
  
    Dopo aver creato il progetto, è possibile impostare le opzioni di conversione del progetto, migrazione e mapping dei tipi. Per informazioni sulle impostazioni del progetto, vedere [impostazione delle opzioni del progetto &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md). Per informazioni sulla personalizzazione dei mapping dei tipi di dati, vedere [mapping dei tipi di dati di Sybase ASE e SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
2.  [Connettersi al server di database SAP ASE](connecting-to-sybase-ase-sybasetosql.md).  
  
3.  [Connettersi a un'istanza SQL Server](connecting-to-sql-server-sybasetosql.md) o [connettersi a un'istanza del database SQL di Azure](connecting-to-azure-sql-db-sybasetosql.md).  
  
4.  [Eseguire il mapping degli schemi di database SAP ASE a SQL Server/schemi di database di database SQL di Azure](./mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).  
  
5.  Facoltativamente, [creare rapporti di valutazione](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md) per valutare gli oggetti di database per la conversione e stimare il tempo di conversione.  
  
6.  [Convertire gli schemi di database SAP ASE in SQL Server/schemi del database SQL di Azure](./converting-sybase-ase-database-objects-sybasetosql.md).  
  
7.  [Caricare gli oggetti di database convertiti in SQL Server/database SQL di Azure](./loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
    Salvare uno script ed eseguirlo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o nel database SQL di Azure oppure sincronizzare gli oggetti di database.  
  
8.  [Eseguire la migrazione dei dati a SQL Server/database SQL di Azure](./migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md).  
  
9. Se necessario, aggiornare le applicazioni di database.  
  
## <a name="see-also"></a>Vedi anche  
[Installazione di SSMA per SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)  
[Introduzione con SSMA per SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)  
