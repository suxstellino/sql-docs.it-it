---
title: Migrazione di database DB2 a SQL Server (DB2ToSQL) | Microsoft Docs
description: Usare questa procedura consigliata per eseguire la migrazione di database DB2 a SQL Server o al database SQL di Azure usando SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 14d2e655-af7e-4aa5-ba28-0e3d0d025518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 912f1ffd342517add68d1ce7f628251fc86dc6aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017701"
---
# <a name="migrating-db2-databases-to-sql-server-db2tosql"></a>Migrazione di database DB2 a SQL Server (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) per DB2 è un ambiente completo che consente di migrare rapidamente i database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o al database SQL di Azure. Utilizzando SSMA per DB2, è possibile esaminare gli oggetti e i dati di database, valutare i database per la migrazione, eseguire la migrazione di oggetti di database al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure e quindi eseguire la migrazione dei dati al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database SQL di Azure. Si noti che non è possibile eseguire la migrazione di schemi SYS e DB2 di sistema.  
  
## <a name="recommended-migration-process"></a>Processo di migrazione consigliato  
Per eseguire correttamente la migrazione di oggetti e dati da database DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o database SQL di Azure, usare il processo seguente:  
  
1.  [Nuovo progetto SSMA](./new-project-db2tosql.md).  
  
    Dopo aver creato il progetto, è possibile impostare le opzioni di conversione del progetto, migrazione e mapping dei tipi. Per informazioni sulle impostazioni del progetto, vedere [Impostazioni progetto &#40;conversione&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md) e sezioni correlate. Per informazioni su come personalizzare i mapping dei tipi di dati, vedere [mapping di tipi di dati DB2 e SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
2.  [Connettersi al database DB2](./connecting-to-db2-database-db2tosql.md).  
  
3.  [Connessione a SQL Server](./connecting-to-sql-server-db2tosql.md).  
  
4.  Eseguire il [mapping di schemi DB2 a SQL Server schemi](./mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).  
  
5.  Facoltativamente, i [report di valutazione](./assessment-report-db2tosql.md) consentono di valutare gli oggetti di database per la conversione e stimare il tempo di conversione.  
  
6.  [Convertire gli schemi DB2](./converting-db2-schemas-db2tosql.md).  
  
7.  [Caricare gli oggetti di database convertiti in SQL Server](./loading-converted-database-objects-into-sql-server-db2tosql.md).  
  
    Questa operazione può essere eseguita in uno dei modi seguenti:  
  
    -   Salvare uno script ed eseguirlo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Sincronizzare gli oggetti di database.  
  
8.  [Migrazione dei dati DB2 in SQL Server](./migrating-db2-data-into-sql-server-db2tosql.md).  
  
9. Se necessario, aggiornare le applicazioni di database.  
  
## <a name="see-also"></a>Vedere anche  
[Installazione di SSMA per DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md)  
[Introduzione con SSMA per DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/getting-started-with-ssma-for-db2-db2tosql.md)  
