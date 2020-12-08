---
title: Oggetto SQL Statistics di SQL Server | Microsoft Docs
description: Informazioni sull'oggetto SQLServer:SQL Statistics, che fornisce contatori per il monitoraggio della compilazione e del tipo di richieste inviate a un'istanza di SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:SQL Statistics
- SQL Statistics object
ms.assetid: da7dbb4b-f632-45a0-b1ab-c35cc2695c86
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e37516249bf251a95d5c03ff5fad1bdeeab7fbf8
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505580"
---
# <a name="sql-server-sql-statistics-object"></a>Oggetto SQL Statistics di SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto **SQLServer:SQL Statistics** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fornisce contatori per il monitoraggio della compilazione e del tipo di richieste inviate a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il monitoraggio del numero di compilazioni e ricompilazioni di query e del numero di batch ricevuti da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di determinare la velocità di elaborazione delle query utente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'efficienza di Query Optmizer.  
  
 La compilazione rappresenta un aspetto fondamentale del processo di elaborazione delle query. Allo scopo di ottimizzare la compilazione, in [!INCLUDE[ssDE](../../includes/ssde-md.md)] il piano di query compilato viene salvato in una query cache. Ciò consente di limitare le operazioni di compilazione e di riutilizzare le query in un momento successivo senza doverle ricompilare. Ogni query tuttavia deve essere compilata almeno una volta. Le ricompilazioni delle query possono essere causate dai seguenti fattori:  
  
-   Modifiche di schema, incluse le modifiche allo schema di base, ad esempio l'aggiunta di colonne o indici a una tabella, e modifiche allo schema delle statistiche, ad esempio l'inserimento o l'eliminazione di un numero significativo di righe in una tabella.  
  
-   Modifiche all'ambiente (istruzione SET). Modifiche alle impostazioni di sessione, ad esempio ANSI_PADDING o ANSI_NULLS.  
  
 Per altre informazioni sulla parametrizzazione semplice e forzata, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 Di seguito sono elencati i contatori dell'oggetto **SQL Statistics** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Contatori dell'oggetto SQL Statistics|Descrizione|  
|----------------------------------------|-----------------|  
|**Tentativi parametrizzazioni automatiche/sec**|Numero di tentativi di parametrizzazione automatica al secondo. Il totale deve corrispondere alla somma delle parametrizzazioni automatiche non riuscite, sicure e non sicure. Una parametrizzazione automatica si verifica quando in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene eseguito un tentativo di parametrizzazione di una richiesta [!INCLUDE[tsql](../../includes/tsql-md.md)] tramite la sostituzione di alcuni valori letterali con parametri in modo da consentire il riutilizzo del piano di esecuzione memorizzato nella cache risultante in più richieste con struttura simile. Si noti che nelle versioni più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]le parametrizzazioni automatiche sono note anche come parametrizzazioni semplici. Sono escluse le parametrizzazioni forzate.|  
|**Richieste batch/sec**|Numero di batch di comandi [!INCLUDE[tsql](../../includes/tsql-md.md)] ricevuti al secondo. Il valore dipende da tutti i vincoli (I/O, numero di utenti, dimensioni della cache, complessità delle richieste e così via). Un valore elevato indica una velocità effettiva ottimale.|  
|**Parametrizzazioni automatiche non riuscite/sec**|Numero di tentativi di parametrizzazione automatica non riusciti al secondo. Il valore dovrebbe essere basso. Si noti che nelle versioni più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]le parametrizzazioni automatiche sono denominate anche parametrizzazioni semplici.|  
|**Parametrizzazioni forzate/sec**|Numero di parametrizzazioni forzate riuscite al secondo.|  
|**Esecuzioni piani guidate al secondo**|Numero di esecuzioni del piano al secondo in cui il piano di query è stato generato utilizzando una guida di piano.|  
|**Esecuzioni piani non guidate al secondo**|Numero di esecuzioni del piano al secondo in cui non è stato possibile applicare una guida di piano durante la generazione del piano. La guida di piano viene ignorata e viene utilizzata la compilazione normale per generare il piano eseguito.|  
|**Parametrizzazioni automatiche sicure/sec**|Numero di tentativi di parametrizzazione automatica sicura al secondo. L'aggettivo sicura indica la possibilità di condivisione di un piano di esecuzione memorizzato nella cache tra più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] con struttura simile. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegue molti tentativi di parametrizzazione automatica. Alcune parametrizzazioni risultano sicure, mentre altre hanno esito negativo. Si noti che nelle versioni più recenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]le parametrizzazioni automatiche sono denominate anche parametrizzazioni semplici. Sono escluse le parametrizzazioni forzate.|  
|**Frequenza situazioni di attenzione SQL**|Numero di situazioni di attenzione al secondo. Una situazione di attenzione corrisponde a una richiesta di interruzione della richiesta corrente inviata dal client.|  
|**Compilazioni SQL/sec**|Numero di compilazioni SQL al secondo. Indica il numero di immissioni del percorso del codice di compilazione. Include le compilazioni generate da ricompilazioni a livello di istruzione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando l'attività dell'utente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stabile, il valore rimane costante.|  
|**Ricompilazioni SQL/sec**|Numero di ricompilazioni di istruzioni al secondo. Esegue il conteggio del numero di volte in cui vengono attivate ricompilazioni di istruzioni. In generale, il numero di ricompilazioni deve essere basso.|  
|**Parametrizzazioni automatiche non sicure/sec**|Numero di tentativi di parametrizzazione automatica non sicure al secondo. Ad esempio, le parametrizzazioni automatiche eseguite quando la query presenta caratteristiche che impediscono la condivisione del piano memorizzato nella cache sono definite non sicure. Sono escluse le parametrizzazioni forzate.|  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Plan Cache di SQL Server](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)   
 [Monitorare l'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
