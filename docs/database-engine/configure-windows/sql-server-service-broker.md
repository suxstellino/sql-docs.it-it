---
title: SQL Server Service Broker | Microsoft Docs
description: Informazioni su Service Broker. Scoprire come offre supporto nativo per la messaggistica nel motore di database di SQL Server e in Istanza gestita di SQL di Azure.
ms.custom: ''
ms.date: 03/17/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SSBMSGTYPEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBCONTRACTPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBQUEUEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBREMSVCBINDPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBROUTEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBPRIORITYPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBSERVICEPROPERTIES.GENERAL.F1
helpviewer_keywords:
- Broker See Service Broker
- SQL Server Service Broker
- Service Broker
ms.assetid: 8b8b3b57-fd46-44de-9a4e-e3a8e3999c1e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 287e3c0abfc083607b96598da5e83cd5ab0b58dd
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611171"
---
# <a name="service-broker"></a>Broker di servizio
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSB](../../includes/sssb-md.md)] offre supporto nativo per la messaggistica e l'accodamento nel [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e in [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance-index). Gli sviluppatori possono creare facilmente applicazioni complesse che usano i componenti del [!INCLUDE[ssDE](../../includes/ssde-md.md)] per la comunicazione tra vari database, nonché compilare applicazioni distribuite e affidabili.  
  
## <a name="when-to-use-service-broker"></a>Quando usare Service Broker

 Usare i componenti di Service Broker per implementare funzionalità di elaborazione di messaggi asincroni nel database native. Gli sviluppatori di applicazioni che utilizzano [!INCLUDE[ssSB](../../includes/sssb-md.md)] possono distribuire il carico di lavoro su più database senza programmare interni di comunicazione e messaggistica complessi. Service Broker consente di ottenere una riduzione delle attività di sviluppo e test, in quanto [!INCLUDE[ssSB](../../includes/sssb-md.md)] gestisce i percorsi di comunicazione nel contesto di una conversazione, con conseguente miglioramento delle prestazioni. Ad esempio, i database front-end che supportano i siti Web possono registrare le informazioni e mettere in coda le attività con molti processi nei database back-end. [!INCLUDE[ssSB](../../includes/sssb-md.md)] si assicura che tutte le attività vengano gestite nel contesto delle transazioni per garantire affidabilità e coerenza tecnica.  
  
## <a name="overview"></a>Panoramica

  Service Broker è un framework di recapito messaggi che consente di creare applicazioni orientate ai servizi nel database native. A differenza delle classiche funzionalità di elaborazione query che leggono continuamente i dati dalle tabelle e li elaborano durante il ciclo di vita della query, nell'applicazione orientata ai servizi sono presenti servizi di database che scambiano i messaggi. Ogni servizio ha una coda in cui i messaggi vengono inseriti fino a quando non vengono elaborati.
  
![Service Broker](media/service-broker.png)
  
  I messaggi nelle code possono essere recuperati tramite il comando `RECEIVE` di Transact-SQL o dalla procedura di attivazione che viene chiamata quando il messaggio arriva nella coda.
  
### <a name="creating-services"></a>Creazione di servizi
 
  È possibile creare i servizi di database tramite l'istruzione Transact SQL [CREATE SERVICE](../../t-sql/statements/create-service-transact-sql.md). Il servizio può essere associato alla coda di messaggi creata tramite l'istruzione [CREATE QUEUE](../../t-sql/statements/create-queue-transact-sql.md):
  
```sql
CREATE QUEUE dbo.ExpenseQueue;
GO
CREATE SERVICE ExpensesService
    ON QUEUE dbo.ExpenseQueue; 
```

### <a name="sending-messages"></a>Invio di messaggi
  
  I messaggi vengono inviati nella conversazione tra i servizi usando l'istruzione Transact-SQL [SEND](../../t-sql/statements/send-transact-sql.md). Una conversazione è un canale di comunicazione che viene stabilito tra i servizi usando l'istruzione Transact-SQL `BEGIN DIALOG`. 
  
```sql
DECLARE @dialog_handle UNIQUEIDENTIFIER;

BEGIN DIALOG @dialog_handle  
FROM SERVICE ExpensesClient  
TO SERVICE 'ExpensesService';  
  
SEND ON CONVERSATION @dialog_handle (@Message) ;  
```
   Il messaggio verrà inviato a `ExpenssesService` e inserito in `dbo.ExpenseQueue`. Poiché a questa coda non è associata alcuna procedura di attivazione, il messaggio rimarrà nella coda fino a quando non verrà letto da qualcuno.

### <a name="processing-messages"></a>Elaborazione di messaggi

   I messaggi inseriti nella coda possono essere selezionati usando una query `SELECT` standard. L'istruzione `SELECT` non modifica la coda, né rimuove i messaggi. Per la lettura e il pull dei messaggi dalla coda, è possibile usare l'istruzione Transact-SQL [RECEIVE](../../t-sql/statements/receive-transact-sql.md).

```sql
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue; 
```

  Dopo aver elaborato tutti i messaggi della coda, chiudere la conversazione usando l'istruzione Transact-SQL [END CONVERSATION](../../t-sql/statements/end-conversation-transact-sql.md).

## <a name="where-is-the-documentation-for-service-broker"></a>Dove si trova la documentazione per Service Broker?  
 La documentazione di riferimento per [!INCLUDE[ssSB](../../includes/sssb-md.md)] è inclusa nella documentazione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Nella documentazione di riferimento sono incluse le sezioni seguenti:  
  
-   [Istruzioni Data Definition Language &#40;DDL&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/statements.md) per istruzioni CREATE, ALTER e DROP  
  
-   [Istruzioni di Service Broker](../../t-sql/statements/statements.md)  
  
-   [Viste del catalogo di Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)  
  
-   [Viste a gestione dinamica relative a Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
-   [Utilità ssbdiagnose &#40;Service Broker&#41;](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
 Vedere la [documentazione pubblicata in precedenza](/previous-versions/sql/sql-server-2008-r2/bb522893(v=sql.105)) per i concetti relativi a [!INCLUDE[ssSB](../../includes/sssb-md.md)] e per le attività di gestione e sviluppo. Questa documentazione non è riprodotta nella documentazione di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] a causa del numero esiguo di modifiche in [!INCLUDE[ssSB](../../includes/sssb-md.md)] in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="whats-new-in-service-broker"></a>Novità di Service Broker  

### <a name="service-broker-and-azure-sql-managed-instance"></a>Service broker e Istanza gestita di SQL di Azure

Lo scambio di messaggi tra istanze di Service Broker è supportato solo tra le istanze gestite di SQL di Azure:

- `CREATE ROUTE`: Non è possibile usare CREATE ROUTE con un indirizzo diverso dal nome locale o DNS di un altro Istanza gestita SQL. La porta specificata deve essere 4022. Vedere [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: Non è possibile usare ALTER ROUTE con ADDRESS diverso dal nome locale o DNS di un altro Istanza gestita SQL. La porta specificata deve essere 4022. Vedere l' [istruzione ALTER Route](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).

La sicurezza del trasporto è supportata. la sicurezza del dialogo non è:

- `CREATE REMOTE SERVICE BINDING` non è supportato.

Service Broker è abilitato per impostazione predefinita e non può essere disabilitato. Le seguenti opzioni di ALTER DATABASE non sono supportate:

- `ENABLE_BROKER`
- `DISABLE_BROKER`

In non sono state introdotte modifiche significative [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]sono state introdotte le modifiche riportate di seguito. 

### <a name="messages-can-be-sent-to-multiple-target-services-multicast"></a>È possibile inviare messaggi a più servizi di destinazione (multicast)  
 La sintassi dell'istruzione [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md) è stata estesa per abilitare il multicast supportando più handle di conversazione.  
  
### <a name="queues-expose-the-message-enqueued-time"></a>Le code espongono il tempo di accodamento del messaggio  
 Le code dispongono di una nuova colonna, **message_enqueue_time** in cui è indicato il tempo di accodamento di un messaggio.  
  
### <a name="poison-message-handling-can-be-disabled"></a>La gestione dei messaggi non elaborabili può essere disabilitata  
 Tramite le istruzioni [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md) e [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md) è possibile abilitare o disabilitare la gestione dei messaggi non elaborabili aggiungendo la clausola, `POISON_MESSAGE_HANDLING (STATUS = ON | OFF)`. La vista del catalogo **sys.service_queues** contiene ora la colonna **is_poison_message_handling_enabled** per indicare se il messaggio non elaborabile è abilitato o disabilitato.  
  
### <a name="always-on-support-in-service-broker"></a>Supporto Always On in Service Broker  
 Per altre informazioni, vedere [Service Broker con i gruppi di disponibilità Always On (SQL Server)](../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md).  
  
  
## <a name="next-steps"></a>Passaggi successivi

L'uso più comune di Service Broker è la [notifica degli eventi](../../relational-databases/service-broker/event-notifications.md). Informazioni su come [implementare notifiche degli eventi](../../relational-databases/service-broker/implement-event-notifications.md), [configurare la sicurezza delle finestre di dialogo](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md) o [ottenere altre informazioni](../../relational-databases/service-broker/get-information-about-event-notifications.md).
