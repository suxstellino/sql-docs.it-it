---
description: MSSQLSERVER_14421
title: MSSQLSERVER_14421 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 14421 (Database Engine error)
ms.assetid: 03e76d4a-d463-4673-8843-08e4ecaefe27
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0c492f6492ba38a00548d21052150e5f703db4c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196966"
---
# <a name="mssqlserver_14421"></a>MSSQLSERVER_14421
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|14421|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SQLErrorNum14421|  
|Testo del messaggio|Per il database secondario per il log shipping %s.%s è impostato un valore soglia per il ripristino di %d minuti e tale database non è sincronizzato. Non è stata eseguita alcuna operazione di ripristino per %d minuti. La latenza ripristinata è di %d minuti. Controllare le informazioni nel log dell'agente e del server di monitoraggio distribuzione log.|  
  
## <a name="explanation"></a>Spiegazione  
Questo messaggio indica che il log shipping non è stato sincronizzato entro la soglia per il ripristino. Per soglia si intende il numero di minuti che possono trascorrere tra operazioni di ripristino prima che venga generato un messaggio.  
  
### <a name="possible-causes"></a>Possibili cause  
Questo messaggio non indica necessariamente un problema relativo al log shipping, ma potrebbe indicare uno dei problemi seguenti:  
  
-   Il processo di ripristino non è in esecuzione.  
  
    La mancata esecuzione del processo può dipendere da diverse cause. È ad esempio possibile che il servizio SQL Server Agent nell'istanza del server secondario non sia in esecuzione, che il processo sia disabilitato oppure che la pianificazione del processo sia stata modificata.  
  
-   Si è verificato un errore relativo al processo di ripristino.  
  
    L'errore relativo al processo può dipendere da diverse cause. È ad esempio possibile che il percorso della cartella di ripristino non sia valido, che il disco sia pieno o che vi siano altri motivi che causano errori nell'istruzione RESTORE.  
  
## <a name="user-action"></a>Azione dell'utente  
Per risolvere il problema che ha causato la visualizzazione di questo messaggio:  
  
-   Verificare che il servizio SQL Server Agent sia in esecuzione per l'istanza del server secondario e che il processo di ripristino del database secondario sia abilitato e pianificato per l'esecuzione a intervalli appropriati.  
  
-   È possibile che si sia verificato un errore relativo al processo di ripristino nel server secondario. In questo caso, verificare la cronologia processo del processo di ripristino per individuarne la causa.  
  
-   È possibile che il processo di ripristino del log shipping, che viene eseguito nell'istanza del server secondario, non sia in grado di connettersi all'istanza del server di monitoraggio per aggiornare la tabella **log_shipping_monitor_secondary**. L'errore potrebbe essere causato da un problema di autenticazione tra l'istanza del server di monitoraggio e l'istanza del server secondario.  
  
-   È possibile che la soglia di avviso per il backup non sia corretta. In una situazione ideale, la soglia è impostata su un valore pari ad almeno tre volte la frequenza del processo di ripristino. Se si cambia la frequenza del processo di ripristino dopo aver configurato e reso operativo il log shipping, sarà necessario aggiornare di conseguenza la soglia di avviso per il backup.  
  
-   Quando l'istanza del server di monitoraggio passa alla modalità offline e quindi torna alla modalità online, la tabella **log_shipping_monitor_secondary** non viene aggiornata con i valori correnti prima dell'esecuzione del processo del messaggio di avviso. È quindi possibile che durante un processo di ripristino venga generato l'errore 14421 "Impossibile trovare un file di backup del log applicabile al database secondario". In tale circostanza l'ora di ripristino non viene aggiornata. In questo caso l'errore può dipendere da un problema relativo al processo di copia.  
  
    Per aggiornare le tabelle di monitoraggio con i dati più recenti del database secondario, eseguire **sp_refresh_log_shipping_monitor** nell'istanza del server secondario.  
  
-   Nell'istanza del server secondario o di monitoraggio la data o l'ora non è corretta. Questo problema può causare la generazione di messaggi di avviso. È possibile che in uno dei due server sia stata modificata la data o l'ora di sistema.  
  
    > [!NOTE]  
    > Fusi orari diversi per le due istanze del server non dovrebbero invece costituire un problema.  
  
## <a name="see-also"></a>Vedere anche  
[log_shipping_monitor_secondary &#40;Transact-SQL&#41;](~/relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)  
[Informazioni sul log shipping &#40;SQL Server&#41;](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
[sp_help_log_shipping_monitor_secondary &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)  
[sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md)  
[Informazioni sul log shipping &#40;SQL Server&#41;](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
