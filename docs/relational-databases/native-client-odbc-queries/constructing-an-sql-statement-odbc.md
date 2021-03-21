---
title: Creazione di un'istruzione SQL (ODBC) | Microsoft Docs
description: Informazioni sul modo in cui il driver ODBC del client SQL Server gestisce le istruzioni SQL, analizzando alcune istruzioni Transact-SQL e passando le altre al database senza modifiche.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, statements
- statements [ODBC], constructing
- ODBC applications, statements
ms.assetid: 0acc71e2-8004-4dd8-8592-05c022bdd692
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a729c1e9bc89cf1c7d6d911260742811b7eac8b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750991"
---
# <a name="constructing-an-sql-statement-odbc"></a>Costruzione di un'istruzione SQL (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le applicazioni ODBC eseguono l'accesso al database quasi sempre mediante l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il formato di tali istruzioni dipende dai requisiti dell'applicazione. Le istruzioni SQL possono essere costruite nei modi seguenti:  
  
-   Hard-coded  
  
     Istruzioni statiche eseguite da un'applicazione come attività fissa.  
  
-   In fase di esecuzione  
  
     Istruzioni SQL costruite in fase di esecuzione che consentono all'utente di personalizzare l'istruzione servendosi di clausole comuni, ad esempio SELECT, WHERE e ORDER BY. Sono incluse query ad hoc immesse dagli utenti.  
  
 Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC del client analizza le istruzioni SQL solo per la sintassi ODBC e ISO non supportata direttamente da [!INCLUDE[ssDE](../../includes/ssde-md.md)] , in cui il driver viene trasformato [!INCLUDE[tsql](../../includes/tsql-md.md)] . Tutte le altre sintassi SQL vengono passate all'oggetto [!INCLUDE[ssDE](../../includes/ssde-md.md)] invariato, in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] determinerà se è valido [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Questo approccio comporta due vantaggi:  
  
-   Riduzione dell'overhead  
  
     L'elaborazione dell'overhead per il driver viene ridotto al minimo, in quanto l'analisi deve essere eseguita solo per un piccolo set di clausole ODBC e ISO.  
  
-   Flessibilità  
  
     I programmatori possono personalizzare la portabilità delle applicazioni. Per migliorare la portabilità rispetto a più database, utilizzare principalmente la sintassi ODBC e ISO. Per utilizzare miglioramenti specifici di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilizzare la sintassi di [!INCLUDE[tsql](../../includes/tsql-md.md)] appropriata. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client supporta la [!INCLUDE[tsql](../../includes/tsql-md.md)] sintassi completa, pertanto le applicazioni basate su ODBC possono sfruttare tutte le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 L'elenco di colonne di un'istruzione SELECT deve contenere solo le colonne richieste per eseguire l'attività corrente. In questo modo non solo si riduce la quantità di dati inviati attraverso la rete, ma anche l'effetto delle modifiche del database sull'applicazione. Se in un'applicazione non si fa riferimento a una colonna di una tabella, l'applicazione non viene interessata dalle modifiche apportate alla colonna.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di query &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
