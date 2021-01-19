---
title: Usare Query Store dopo l'aggiornamento
description: Questo articolo illustra l'uso di Query Store per stabilire una baseline e modificare il livello di compatibilità del database in un aggiornamento di SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server], migrating
- upgrading SQL Server, migrating query plans
- plan guides [SQL Server], migrating query plans
ms.assetid: 7e02a137-6867-4f6a-a45a-2b02674f7e65
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7d4394c5fe4d790668bcc6733ce0aba0f643f7f3
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170893"
---
# <a name="change-the-database-compatibility-level-and-use-the-query-store"></a>Modificare il livello di compatibilità del database e usare Query Store

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

In [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive, alcune modifiche vengono abilitate solo dopo la modifica del [livello di compatibilità del database](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Questa operazione viene eseguita per diversi motivi:  
  
- Poiché l'aggiornamento è un'operazione unidirezionale, in quanto non è possibile effettuare il downgrade del formato del file, è utile impostare l'abilitazione delle nuove funzionalità come un'operazione separata all'interno del database. È possibile ripristinare un'impostazione a un livello di compatibilità del database precedente.  Il nuovo modello riduce il numero di operazioni che devono essere eseguite durante un intervallo di interruzione.  
  
- Le modifiche apportate a Query Processor possono avere effetti complessi. Anche se una modifica "positiva" al sistema è ideale per la maggior parte dei carichi di lavoro, potrebbe causare una regressione inaccettabile altrove in una query importante. Separare la logica dal processo di aggiornamento consente a funzionalità quali Query Store di ridurre rapidamente le regressioni della scelta del piano o persino di evitarle completamente nei server di produzione.  
  
> [!IMPORTANT]  
> Sono previsti i comportamenti seguenti per [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] quando un database viene collegato o ripristinato e dopo un aggiornamento sul posto:
> - Se il livello di compatibilità di un database utente era 100 o superiore prima dell'aggiornamento, rimane invariato dopo l'aggiornamento.    
> - Se il livello di compatibilità di un database utente è 90 prima dell'aggiornamento, nel database aggiornato viene impostato su 100, ovvero sul livello di compatibilità supportato più basso in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].    
> - I livelli di compatibilità dei database tempdb, model, msdb e Resource vengono impostati sul livello di compatibilità corrente dopo l'aggiornamento.   
> - Per il database di sistema master viene mantenuto il livello di compatibilità precedente l'aggiornamento.    
  
Il processo di aggiornamento per abilitare nuove funzionalità di Query Processor è correlato al modello di manutenzione post-rilascio del prodotto.  Alcune di queste correzioni vengono rilasciate con il [flag di traccia 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).  I clienti che necessitano di correzioni possono acconsentire esplicitamente a esse senza causare regressioni impreviste per altri clienti. Il modello di manutenzione post-rilascio per gli aggiornamenti rapidi di Query Processor è documentato [qui](https://support.microsoft.com/kb/974006). A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] il passaggio a un nuovo livello di compatibilità implica che il flag di traccia 4199 non è più necessario, perché tali correzioni sono ora abilitate per impostazione predefinita nel livello di compatibilità più recente. Come parte del processo di aggiornamento, è pertanto importante verificare che al termine del processo di aggiornamento il flag di traccia 4199 non sia abilitato.  

> [!NOTE]
> Il flag di traccia 4199 tuttavia è ancora necessario per abilitare eventuali nuove correzioni di Query Processor rilasciate dopo la versione RTM, se applicabile.
  
Il flusso di lavoro consigliato per aggiornare Query Processor alla versione più recente del codice è documentato nella [sezione Mantenere la stabilità delle prestazioni durante l'aggiornamento alla nuova versione di SQL Server in Scenari di utilizzo di Query Store](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade), come illustrato di seguito.  
  
![Diagramma che mostra il flusso di lavoro consigliato per l'aggiornamento di Query Processor alla versione più recente del codice.](../../relational-databases/performance/media/query-store-usage-5.png "utilizzo archivio query 5") 

A partire da [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18, gli utenti possono essere guidati nel flusso di lavoro consigliato con l'Assistente ottimizzazione query. Per altre informazioni, vedere [Aggiornamento di database mediante l'Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md).
 
## <a name="see-also"></a>Vedere anche  
[Visualizzare o modificare il livello di compatibilità di un database](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)     
[Scenari di utilizzo dell'Archivio query](../../relational-databases/performance/query-store-usage-scenarios.md)     
[Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)     
[Aggiornamento di database mediante l'Assistente ottimizzazione query](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)        
  
