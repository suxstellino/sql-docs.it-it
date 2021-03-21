---
title: Formati di dati per l'importazione o l'esportazione bulk
description: SQL Server accetta dati in formato carattere o binario nativo. Usare il formato carattere tra SQL Server e altre app e il formato nativo tra le istanze di SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], choosing
- bulk importing [SQL Server], data formats
ms.assetid: 73fe6741-9437-4b26-b030-28b863e74399
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 9a71933adc9e99fadad04fb2c94d9b114a9005bc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749681"
---
# <a name="data-formats-for-bulk-import-or-bulk-export-sql-server"></a>Formati di dati per l'importazione o l'esportazione bulk (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può accettare dati nel formato dati di tipo carattere o nel formato dati binario nativo. Utilizzare il formato carattere se vengono spostati dati tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e un'altra applicazione (ad esempio [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel) o un altro server di database (ad esempio Oracle o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). È possibile utilizzare il formato nativo unicamente se si trasferiscono dati tra due istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Contenuto dell'argomento**  
  
-   [Formati di dati per l'importazione o l'esportazione bulk](#ComponentsAndConcepts)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="data-formats-for-bulk-import-or-export"></a><a name="ComponentsAndConcepts"></a> Formati di dati per l'importazione o l'esportazione bulk  
 Nella tabella seguente viene indicato quale formato di dati è generalmente appropriato utilizzare in base alla modalità di rappresentazione dei dati e all'origine o destinazione dell'operazione.  
  
|Operazione|Nativo|nativi Unicode|Carattere|carattere Unicode|  
|---------------|------------|--------------------|---------------|-----------------------|  
|Trasferimenti bulk di dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite un file di dati che non contiene caratteri estesi o DBCS (Double-Byte Character Set). A meno che non venga utilizzato un file di formato, la definizione delle tabelle deve essere identica.|Sì*|-|-|-|  
|Per le colonne **sql_variant** è consigliabile usare il formato di dati nativo perché, a differenza dei formati carattere o Unicode, mantiene i metadati per ogni valore **sql_variant** .|Sì|-|-|-|  
|Trasferimenti bulk di dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite un file di dati contenente caratteri estesi o DBCS.|-|Sì|-|-|  
|Importazione bulk di dati da un file di testo creato da un altro programma.|-|-|Sì|-|  
|Esportazione bulk di dati in un file di testo che verrà utilizzato in un altro programma.|-|-|Sì|-|  
|Trasferimenti bulk di dati tra più istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite un file di dati contenente dati Unicode e che non contiene caratteri estesi o DBCS.|-|-|-|Sì|  
  
 \* Metodo più veloce per l'esportazione in blocco di dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se si usa **bcp**.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utilizzo del formato carattere per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Utilizzo del formato carattere Unicode per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Specificare i formati di dati per la compatibilità con bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)  
  
  
