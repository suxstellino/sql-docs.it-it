---
description: Proprietà avviso - Nuovo avviso (pagina Generale)
title: Proprietà avviso - Nuovo avviso (pagina Generale)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.general.f1
ms.assetid: f5c11610-62e3-44df-9800-a5dc35be4a09
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 18cbd660ed24526331aed570415eccd314127c73
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472442"
---
# <a name="alert-properties---new-alert-general-page"></a>Proprietà avviso - Nuovo avviso (pagina Generale)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Usare questa pagina per visualizzare e modificare le proprietà generali degli avvisi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  

## <a name="options"></a>Opzioni  
**Nome**  
Consente di modificare il nome dell'avviso.  
  
**Attiva**  
Consente di abilitare l'avviso. Se l'avviso non è abilitato, le azioni specificate nell'avviso non verranno eseguite.  
  
**Tipo**  
Consente di selezionare il tipo di avviso:  
  
-   **Avviso per evento di SQL Server** risponde ai messaggi nel registro degli eventi di [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows.  
  
-   **Avviso relativo alle prestazioni di SQL Server** risponde a una specifica condizione in un contatore delle prestazioni.  
  
-   **Avviso per evento WMI** risponde a un evento del servizio Strumentazione gestione Windows (WMI, Windows Management Instrumentation).  
  
## <a name="sql-server-event-alert-options"></a>Opzioni di Avviso per evento di SQL Server  
**Nome database**  
Consente di specificare un database per l'evento o **tutti i database** per rispondere a un messaggio indipendentemente dal database in cui si verifica l'evento.  
  
**Numero di errore**  
Consente di specificare che l'evento risponde a un errore e di indicare il numero dell'errore.  
  
**Gravità**  
Consente di specificare che l'evento risponde a tutti messaggi con uno specifico livello di gravità e di indicare tale livello.  
  
**Genera avviso quando il messaggio contiene**  
Consente di applicare un filtro agli eventi in base a una stringa specifica. Se questa opzione è selezionata, l'avviso risponde solo agli eventi contenenti la stringa specificata.  
  
**Testo del messaggio**  
Consente di specificare la stringa da utilizzare come filtro per gli eventi.  
  
## <a name="sql-server-performance-condition-alerts"></a>Opzioni di Avviso relativo alle prestazioni di SQL Server  
**Object**  
Consente di specificare l'oggetto prestazioni da monitorare.  
  
**Contatore**  
Consente di specificare il contatore all'interno dell'oggetto prestazioni da monitorare.  
  
**Istanza**  
Consente di specificare l'istanza del contatore da monitorare.  
  
**Avvisa se il contatore**  
Consente di specificare il comportamento del contatore a cui risponde l'evento. È ad esempio possibile impostare questa opzione in modo che l'avviso risponda a una condizione in cui il valore del contatore **Spazio disponibile in tempdb (KB)** scende al di sotto di un determinata soglia o il valore di **Compilazioni SQL/sec** supera un determinato livello.  
  
**Valore**  
Consente di specificare un valore per il contatore.  
  
## <a name="wmi-event-alert-options"></a>Opzioni di Avviso per evento WMI  
**Spazio dei nomi**  
Consente di specificare lo spazio dei nomi da utilizzare per l'istruzione WQL (WMI Query Language). Sono supportati solo gli spazi dei nomi sul computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
**Query**  
Consente di specificare l'istruzione WQL che identifica l'evento al quale risponde l'avviso.  
  
## <a name="see-also"></a>Vedere anche  
[Avvisi](../../ssms/agent/alerts.md)  
[Utilizzo di WQL con il provider WMI per eventi del server](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md)  
[Creazione di un avviso utilizzando un numero di errore](../../ssms/agent/create-an-alert-using-an-error-number.md)  
[Creazione di un avviso utilizzando i livelli di gravità](../../ssms/agent/create-an-alert-using-severity-level.md)  
