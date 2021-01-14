---
title: Database indipendenti | Microsoft Docs
description: Informazioni su database indipendenti, vantaggi e limitazioni dei database parzialmente indipendenti e funzionamento di SQL Server 2019 per l'isolamento dei database.
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database
- database_uncontained_usage event
- partially contained database
- contained database, understanding
ms.assetid: 36af59d7-ce96-4a02-8598-ffdd78cdc948
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bbad843d482945a29cb03fde9545b1c19fb354f9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098395"
---
# <a name="contained-databases"></a>Database indipendenti
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Un *database indipendente* è un database isolato dagli altri database e dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita il database.  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] offre all'utente quattro modalità per isolare il database dall'istanza.  
  
-   Molti dei metadati che descrivono un database vengono gestiti nel database, oltre a (o anziché) essere gestiti nel database master.  
  
-   Tutti metadati sono definiti usando le stesse regole di confronto.  
  
-   L'autenticazione degli utenti può essere eseguita dal database, riducendo la dipendenza del database dagli account di accesso dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   L'ambiente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di creare report (DMV, XEvents e così via) e agire in base alle informazioni di indipendenza.  
  
 Alcune funzionalità dei database parzialmente indipendenti, ad esempio l'archiviazione dei metadati nel database, si applicano a tutti i database di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Alcuni vantaggi dei database parzialmente indipendenti, ad esempio l'autenticazione a livello di database e le regole di confronto del catalogo, devono essere abilitati per poter essere disponibili. L'indipendenza parziale si abilita tramite le istruzioni **CREATE DATABASE** e **ALTER DATABASE** o usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Per altre informazioni sull'abilitazione dell'indipendenza parziale del database, vedere [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md).  
  
##  <a name="partially-contained-database-concepts"></a><a name="Concepts"></a> Concetti di database parzialmente indipendente  
 In un database totalmente indipendente sono incluse tutte le impostazioni del database e i metadati necessari per definire il database, mentre non sono presenti dipendenze di configurazione nell'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in cui è installato il database. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la separazione di un database dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può risultare un'operazione dispendiosa in termini di tempo e richiedere una conoscenza dettagliata della relazione tra il database e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Con i database parzialmente indipendenti è più semplice la separazione di un database dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dagli altri database.  
  
 In un database indipendente le funzionalità vengono considerate in relazione all'indipendenza. Qualsiasi entità definita dall'utente basata solo su funzioni che risiedono nel database è considerata completamente indipendente. Qualsiasi entità definita dall'utente basata su funzioni che risiedono all'esterno del database è considerata non indipendente. Per altre informazioni, vedere la sezione [Indipendenza](#containment) più avanti in questo argomento.  
  
 I termini seguenti si applicano al modello di database indipendente.  
  
 Limite del database  
 Limite tra un database e l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Limite tra un database e altri database.  
  
 Contenuto  
 Elemento completamente esistente entro il limite del database.  
  
 Non contenuto  
 Elemento che varca il limite del database.  
  
 Database non indipendente  
 Database con lo stato di indipendenza impostato su **NESSUNO**. Tutti i database delle versioni precedenti di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] sono non indipendenti. Per impostazione predefinita, per tutti i database di [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive lo stato di indipendenza è impostato su **NESSUNO**.  
  
 Database parzialmente indipendente  
 Un database parzialmente indipendente è un database indipendente in cui sono supportate funzionalità che superano il limite del database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è inclusa la possibilità di determinare quando il limite di indipendenza viene superato.  
  
 Utente indipendente  
 Per i database indipendenti sono previsti due tipi di utenti.  
  
-   **Utente del database indipendente con password**  
  
     Gli utenti del database indipendente con password vengono autenticati dal database. Per altre informazioni, vedere [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
-   **Entità di Windows**  
  
     Gli utenti di Windows autorizzati e i membri dei gruppi di Windows autorizzati possono connettersi direttamente al database e non devono disporre di account di accesso nel database **master** . Il database considera attendibile l'autenticazione di Windows.  
  
 Agli utenti con account di accesso nel database **master** può essere concesso l'accesso a un database indipendente, ma si creerebbe una dipendenza dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pertanto, per la creazione di utenti in base agli account di accesso, sono necessari database parzialmente indipendenti.
  
> [!IMPORTANT]  
>  L'abilitazione dei delegati di database parzialmente indipendenti controlla l'accesso all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per i proprietari del database. Per altre informazioni, vedere [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 Limite del database  
 Considerato che nei database parzialmente indipendenti le funzionalità del database sono separate da quelle dell'istanza, tra questi due elementi esiste una netta linea di demarcazione detta *limite del database*.  
  
 All'interno del limite del database è presente il *modello di database*, nel quale i database vengono sviluppati e gestiti. Esempi di entità che si trovano all'interno del database includono tabelle di sistema, ad esempio **sys.tables**, utenti del database indipendente con password e tabelle utente nel database corrente a cui viene fatto riferimento con un nome in due parti.  
  
 All'esterno del limite del database è presente il *modello di gestione* che si riferisce alla gestione e alle funzioni a livello di istanza. Esempi di entità che si trovano all'esterno del limite del database includono tabelle di sistema, ad esempio **sys.endpoints**, utenti su cui viene eseguito il mapping ad account di accesso e tabelle utente in un altro database a cui viene fatto riferimento con un nome in tre parti.  
  
##  <a name="containment"></a><a name="containment"></a> Indipendenza  
 Le entità utente completamente residenti all'interno del database sono considerate *indipendenti*. Qualsiasi entità residente all'esterno del database o basata sull'interazione con le funzioni esterne al database è considerata *non indipendente*.  
  
 In generale, le entità utente rientrano nelle categorie di indipendenza seguenti:  
  
-   Entità utente completamente indipendenti, ovvero quelle che non superano mai il limite del database, ad esempio sys.indexes. Qualsiasi struttura di codice in cui vengano usate queste funzionalità o qualsiasi oggetto che faccia riferimento solo a queste entità è anch'esso completamente contenuto.  
  
-   Entità utente non indipendenti, ovvero quelle che superano il limite del database, ad esempio sys.server_principals o una stessa entità server (accesso). Qualsiasi struttura di codice in cui vengano usate queste entità o qualsiasi funzione che faccia riferimento a queste entità è anch'essa non indipendente.  
  
###  <a name="partially-contained-database"></a><a name="partial"></a> Partially Contained Database  
 La caratteristica di database indipendente è attualmente disponibile solo in uno stato parzialmente indipendente. Un database parzialmente indipendente è un database indipendente in cui è supportato l'utilizzo di funzionalità non indipendenti.  
  
 Usare le viste [sys.dm_db_uncontained_entities](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) e [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) per restituire informazioni relative a oggetti o funzionalità non indipendenti. Determinando lo stato di indipendenza degli elementi del database, è possibile individuare gli oggetti o le funzionalità che è necessario sostituire o modificare ai fini dell'indipendenza.  
  
> [!IMPORTANT]  
>  Poiché l'impostazione di indipendenza predefinita di determinati oggetti è **NESSUNO**, è possibile che in questa vista vengano restituiti falsi positivi.  
  
 In termini di regole di confronto, il comportamento dei database parzialmente indipendenti è nettamente diverso da quello dei database non indipendenti. Per altre informazioni sui problemi relativi alle regole di confronto, vedere [Contained Database Collations](../../relational-databases/databases/contained-database-collations.md).  
  
##  <a name="benefits-of-using-partially-contained-databases"></a><a name="benefits"></a> Vantaggi dell'utilizzo di database parzialmente indipendenti  
 Alcuni problemi e complessità associati ai database non indipendenti possono essere risolti con un database parzialmente indipendente.  
  
### <a name="database-movement"></a>Spostamento di database  
 Uno dei problemi che si verifica quando un database viene spostato da un'istanza a un'altra è che importanti informazioni possono non essere disponibili. Ad esempio, le informazioni di accesso sono archiviate all'interno dell'istanza anziché nel database. Quando si sposta un database non indipendente da un'istanza a un'altra di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tali informazioni vengono ignorate. È necessario identificare le informazioni mancanti e spostarle insieme al database nella nuova istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questo processo può richiedere tempi lunghi e risultare difficile.  
  
 È possibile archiviare importanti informazioni nel database parzialmente indipendente in modo che dopo lo spostamento le informazioni siano ancora disponibili nel database.  
  
> [!NOTE]  
>  Un database parzialmente indipendente può fornire la documentazione che descrive le funzionalità usate da un database che non possono essere separate dall'istanza. tra cui un elenco di altri database correlati, le impostazioni di sistema richieste dal database ma non indipendenti e così via.  
  
### <a name="benefit-of-contained-database-users-with-always-on"></a>Vantaggi degli utenti di database indipendenti con AlwaysOn  
 Riducendo i valori equivalenti all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], i database parzialmente indipendenti possono essere utili durante il failover quando si usano [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 La creazione di utenti indipendenti consente all'utente di connettersi direttamente al database indipendente. Si tratta di una funzionalità molto significativa in scenari a disponibilità elevata e di ripristino di emergenza, ad esempio in una soluzione AlwaysOn. Se gli utenti sono indipendenti, in caso di failover può essere possibile connettersi al database secondario senza creare account di accesso sull'istanza che ospita il database secondario. Questo rappresenta un vantaggio immediato. Per altre informazioni, vedere [Panoramica di Gruppi di disponibilità Always On (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) e [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità Always On (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="initial-database-development"></a>Sviluppo iniziale di database  
 Poiché è possibile che uno sviluppatore non conosca l'ambiente nel quale verrà distribuita un nuovo database, limitando gli impatti ambientali distribuiti al database sarà possibile ridurre il lavoro e le problematiche per lo sviluppatore. Nel modello non indipendente lo sviluppatore deve quindi considerare i possibili impatti ambientali sul nuovo database e programmare di conseguenza. Tuttavia, usando i database parzialmente indipendenti, gli sviluppatori possono rilevare l'impatto a livello di istanza sul database e le problematiche a livello di istanza che è necessario affrontare.  
  
### <a name="database-administration"></a>Amministrazione del database  
 La gestione delle impostazioni del database nel database, anziché nel database master, consente a ogni proprietario di database di avere più controllo sul database, senza fornire al proprietario di database l'autorizzazione **sysadmin** .  
  
##  <a name="limitations"></a><a name="Limitations"></a> Limitazioni  
 I database parzialmente indipendenti non supportano le funzionalità indicate di seguito.  
  
-   Replica, Change Data Capture o rilevamento modifiche.  
  
-   Procedure numerate  
  
-   Oggetti associati a schema che dipendono da funzioni predefinite con modifiche delle regole di confronto  
  
-   Modifica di associazione a seguito di modifiche delle regole di confronto, inclusi riferimenti a oggetti, colonne, simboli o tipi.
  
> [!WARNING]  
>  Le stored procedure temporanee sono attualmente consentite. Poiché le stored procedure temporanee violano l'indipendenza, non ne è previsto il supporto nelle versioni future del database indipendente.  
  
##  <a name="identifying-database-containment"></a><a name="Identifying"></a> Identificazione dell'indipendenza del database  
 Per consentire l'identificazione dello stato di indipendenza del database sono disponibili due strumenti. [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) è una vista in cui sono riportate tutte le entità potenzialmente non indipendenti nel database. L'evento database_uncontained_usage si verifica quando viene identificata un'entità effettivamente non contenuta in fase di esecuzione.  
  
### <a name="sysdm_db_uncontained_entities"></a>sys.dm_db_uncontained_entities  
 In questa vista vengono riportate tutte entità presenti nel database che potrebbero essere non indipendenti, ad esempio quelle che superano il limite del database. Sono incluse le entità utente che potrebbero usare oggetti al di fuori del modello di database. Poiché non è tuttavia possibile determinare lo stato di indipendenza di alcune entità, ad esempio quelle che usano SQL dinamico, fino alla fase di esecuzione, nella vista potrebbero essere presenti alcune entità che non sono effettivamente non indipendenti. Per altre informazioni, vedere [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
### <a name="database_uncontained_usage-event"></a>database_uncontained_usage - evento  
 L'evento XEvent viene generato quando viene identificata un'entità non indipendente in fase di esecuzione. Sono incluse le entità che hanno origine nel codice client. Questo evento XEvent verrà generato solo per le entità effettivamente non indipendenti e solo in fase di esecuzione. Pertanto, qualsiasi entità utente non indipendente non eseguita non sarà identificata da questo XEvent.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzionalità modificate &#40;database indipendente&#41;](../../relational-databases/databases/modified-features-contained-database.md)   
 [Regole di confronto dei database indipendenti](../../relational-databases/databases/contained-database-collations.md)   
 [Procedure consigliate per la sicurezza in database indipendenti](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Utenti di database indipendente: rendere portabile un database](../../relational-databases/security/contained-database-users-making-your-database-portable.md)  
  
  
