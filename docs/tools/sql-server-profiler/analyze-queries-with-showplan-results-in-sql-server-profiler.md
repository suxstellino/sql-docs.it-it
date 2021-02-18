---
title: Analizzare query con risultati SHOWPLAN
titleSuffix: SQL Server Profiler
description: Informazioni su come estrarre gli eventi showplan da una traccia in modo che SQL Server Profiler visualizzi le informazioni sul piano di query. Visualizzare una tabella di eventi di traccia showplan.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6a2f7727-141c-4f59-8613-2e452bc78467
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 31771ddc584d72dee2b3c859b349dd5752e4d4d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349453"
---
# <a name="analyze-queries-with-showplan-results-in-sql-server-profiler"></a>Analizzare query con risultati SHOWPLAN in SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  È possibile aggiungere a una definizione di traccia classi di eventi Showplan che determinano la raccolta e la visualizzazione delle informazioni sul piano della query nella traccia in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . È inoltre possibile estrarre eventi Showplan dagli altri eventi raccolti nella traccia e salvarli in un file XML distinto.  
  
 L'estrazione di eventi Showplan dalla traccia può essere eseguita in uno dei modi seguenti:  
  
-   Durante la configurazione della traccia, con la scheda **Impostazioni estrazione eventi** . Questa scheda non viene visualizzata a meno che non si selezioni uno degli eventi Showplan presenti nella scheda **Selezione eventi** .  
  
-   Con il comando **Estrai eventi di SQL Server** del menu **File** .  
  
-   Facendo clic con il pulsante destro del mouse su un evento specifico e scegliendo **Estrai dati eventi** per estrarre e salvare singoli eventi.  
  
## <a name="showplan-events"></a>Eventi Showplan  
 Nella tabella seguente sono elencati e descritti gli eventi di traccia Showplan.  
  
|Nome evento|Descrizione|  
|----------------|-----------------|  
|**Performance statistics**|Indica la prima volta che uno Showplan compilato viene inserito nella cache, quando viene ricompilato e quando viene eliminato dalla cache del piano. La colonna **TextData** contiene Showplan in formato XML. Per altre informazioni, vedere [Classe di evento Performance Statistics](../../relational-databases/event-classes/performance-statistics-event-class.md).|  
|**Showplan All**|Visualizza il piano della query dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguita con dettagli completi sulla compilazione, ad esempio i costi stimati e gli elenchi di colonne. Per altre informazioni, vedere [Classe di evento Showplan All](../../relational-databases/event-classes/showplan-all-event-class.md).|  
|**Showplan All For Query Compile**|Si verifica quando una query viene compilata o ricompilata su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È la controparte per il tempo di compilazione dell'evento **Showplan All** . **Showplan All** si verifica quando viene eseguita una query. **Showplan All For Query Compile** si verifica quando viene compilata una query. Per altre informazioni, vedere [Classe di evento Showplan All for Query Compile](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md).|  
|**Showplan Statistics Profile**|Visualizza il piano della query dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in esecuzione con dettagli completi sulla fase di run-time, inclusi il numero effettivo di righe passate tramite ogni operazione. Per altre informazioni, vedere [Classe di evento Showplan Statistics Profile](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md).|  
|**Showplan Text**|Visualizza l'albero del piano della query per l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in esecuzione come dati binari. Per altre informazioni, vedere [Classe di evento Showplan Text](../../relational-databases/event-classes/showplan-text-event-class.md).|  
|**Showplan Text (Unencoded)**|Visualizza l'albero del piano della query per l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in esecuzione come testo. Questa classe di evento visualizza le stesse informazioni di Showplan Text, con la sola differenza che le informazioni sono visualizzate come testo anziché come dati binari. Per altre informazioni, vedere [Classe di evento Showplan Text &#40;Unencoded&#41;](../../relational-databases/event-classes/showplan-text-unencoded-event-class.md).|  
|**Showplan XML**|Visualizza il piano della query con i dati completi raccolti durante l'ottimizzazione della query. Questo evento viene generato unicamente quando un piano di query è ottimizzato. Per altre informazioni, vedere [Classe di evento Showplan XML](../../relational-databases/event-classes/showplan-xml-event-class.md).|  
|**Showplan XML For Query Compile**|Visualizza il piano della query quando la query viene compilata. Per altre informazioni, vedere [Classe di evento Showplan XML for Query Compile](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md).|  
|**Showplan XML Statistics Profile**|Visualizza il piano della query con dettagli completi sulla fase di esecuzione in formato XML. Ad esempio, la classe di evento acquisisce il numero di righe passate tramite ogni operatore dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] in esecuzione. Per altre informazioni, vedere [Classe di evento Showplan XML Statistics Profile](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md).|  
  
## <a name="see-also"></a>Vedere anche  
 [Categoria di eventi Prestazioni](../../relational-databases/event-classes/performance-event-category.md)  
  
  
