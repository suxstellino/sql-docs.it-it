---
description: Esecuzione diretta
title: Esecuzione diretta | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, statements
- direct execution [ODBC]
- SQLExecDirect function
- statements [ODBC], direct execution
ms.assetid: fa36e1af-ed98-4abc-97c1-c4cc5d227b29
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a043fd35876566c724b946daeca0e95b0b021cd3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750721"
---
# <a name="direct-execution"></a>Esecuzione diretta
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  L'esecuzione diretta rappresenta la modalità più semplice di esecuzione di un'istruzione. Un'applicazione compila una stringa di caratteri contenente un' [!INCLUDE[tsql](../../../includes/tsql-md.md)] istruzione e la invia per l'esecuzione mediante la funzione **SQLExecDirect** . Dopo avere raggiunto il server, l'istruzione viene compilata da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un piano di esecuzione, che viene quindi immediatamente eseguito.  
  
 L'esecuzione diretta viene in genere utilizzata dalle applicazioni che compilano ed eseguono istruzioni in fase di esecuzione e rappresenta il metodo più efficiente per le istruzioni eseguite una sola volta. Lo svantaggio con molti database consiste nel fatto che l'istruzione SQL deve essere analizzata e compilata ogni volta che viene eseguita determinando un aumento dell'overhead se l'istruzione viene eseguita più volte.  
  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono notevolmente migliorate le prestazioni dell'esecuzione diretta delle istruzioni utilizzate più di frequente negli ambienti multiutente e l'utilizzo di SQLExecDirect con gi marcatori di parametro per le istruzioni SQL utilizzate più di frequente si avvicina all'efficienza dell'esecuzione preparata.  
  
 Quando si è connessi a un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client utilizza [sp_executesql](../../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) per trasmettere l'istruzione SQL o il batch specificato in **SQLExecDirect**. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dispone della logica per determinare rapidamente se un'istruzione SQL o un batch eseguito con **sp_executesql** corrisponde all'istruzione o al batch che ha generato un piano di esecuzione già esistente in memoria. Se esiste una corrispondenza, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] riutilizza semplicemente il piano esistente anziché compilarne uno nuovo. Ciò significa che le istruzioni SQL comunemente eseguite eseguite con **SQLExecDirect** in un sistema con molti utenti trarranno vantaggio da molti dei vantaggi del riutilizzo del piano disponibili solo per le stored procedure nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Il vantaggio di riutilizzare i piani di esecuzione è valido solo quando più utenti eseguono lo stesso batch o la stessa istruzione SQL. Rispettare le seguenti convenzioni di scrittura del codice per aumentare la probabilità che le istruzioni SQL eseguite da client diversi siano simili abbastanza per potere riutilizzare i piani di esecuzione:  
  
-   Non includere costanti di dati nelle istruzioni SQL; utilizzare invece i marcatori di parametro associati alle variabili di programma. Per ulteriori informazioni, vedere [utilizzo dei parametri dell'istruzione](../../../relational-databases/native-client-odbc-queries/using-statement-parameters.md).  
  
-   Utilizzare nomi di oggetto completi. I piani di esecuzione non vengono riutilizzati se i nomi di oggetto non sono completi.  
  
-   Se possibile, utilizzare per le connessioni dell'applicazione un set comune di opzioni dell'istruzione e di connessione. I piani di esecuzione generati per una connessione con un set di opzioni, ad esempio ANSI_NULLS, non vengono riutilizzati per una connessione con un altro set di opzioni. Il driver ODBC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client e il provider ODBC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client hanno le stesse impostazioni predefinite per queste opzioni.  
  
 Se tutte le istruzioni eseguite con **SQLExecDirect** vengono codificate utilizzando queste convenzioni, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] può riutilizzare i piani di esecuzione quando si verifica l'opportunità.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di istruzioni &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
  
