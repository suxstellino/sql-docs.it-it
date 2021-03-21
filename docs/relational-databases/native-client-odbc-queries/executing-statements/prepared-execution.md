---
description: Esecuzione preparata
title: Esecuzione preparata | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- deferred statement preparation
- prepared execution [ODBC]
- SQLPrepare function
- ODBC applications, statements
- SQLExecute function
- statements [ODBC], prepared execution
ms.assetid: f3a9d32b-6cd7-4f0c-b38d-c8ccc4ee40c3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8da07014b33b710a2036f5e411b3310180eaae02
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752891"
---
# <a name="prepared-execution"></a>Esecuzione preparata
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  L'API ODBC definisce l'esecuzione preparata per ridurre l'overhead dell'analisi e della compilazione associato all'esecuzione ripetuta di un'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Nell'applicazione viene compilata una stringa di caratteri contenente un'istruzione SQL che viene eseguita in due fasi. Chiama la [funzione SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md) una volta per analizzare e compilare l'istruzione in un piano di esecuzione da [!INCLUDE[ssDE](../../../includes/ssde-md.md)] . Viene quindi chiamato **SQLExecute** per ogni esecuzione del piano di esecuzione preparato. con conseguente risparmio dell'overhead correlato all'analisi e alla compilazione in ogni esecuzione. L'esecuzione preparata viene generalmente utilizzata dalle applicazioni per eseguire ripetutamente la stessa istruzione SQL con parametri.  
  
 Per la maggior parte dei database, l'esecuzione preparata è più veloce dell'esecuzione diretta per le istruzioni eseguite più di tre o quattro volte, sopratutto perché l'istruzione viene compilata una sola volta, mentre le istruzioni eseguite direttamente vengono compilate ogni volta che vengono eseguite. L'esecuzione preparata può inoltre offrire una riduzione del traffico di rete perché il driver può inviare all'origine dati un identificatore del piano di esecuzione e i valori dei parametri, anziché un'intera istruzione SQL, ogni volta che viene eseguita l'istruzione.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] riduce la differenza di prestazioni tra l'esecuzione diretta e preparata tramite algoritmi ottimizzati per il rilevamento e il riutilizzo dei piani di esecuzione da **SQLExecDirect**. offrendo alle istruzioni eseguite direttamente alcuni dei vantaggi di prestazioni associati all'esecuzione preparata. Per ulteriori informazioni, vedere [esecuzione diretta](../../../relational-databases/native-client-odbc-queries/executing-statements/direct-execution.md).  
  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è inoltre disponibile il supporto nativo per l'esecuzione preparata. Un piano di esecuzione viene compilato in **SQLPrepare** e successivamente eseguito quando viene chiamato **SQLExecute** . Poiché [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non è necessario per compilare stored procedure temporanee in **SQLPrepare**, non si verifica alcun overhead aggiuntivo nelle tabelle di sistema in **tempdb**.  
  
 Per motivi di prestazioni, la preparazione dell'istruzione viene posticipata fino alla chiamata a **SQLExecute** o all'esecuzione di un'operazione di metaproprietà, ad esempio [SQLDescribeCol](../../../relational-databases/native-client-odbc-api/sqldescribecol.md) o [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) in ODBC. Comportamento predefinito. Eventuali errori nell'istruzione da preparare saranno noti solo dopo l'esecuzione dell'istruzione o dell'operazione di metaproprietà. È possibile disattivare questo comportamento predefinito impostando l'attributo SQL_SOPT_SS_DEFER_PREPARE dell'istruzione specifica del driver ODBC di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client su SQL_DP_OFF.  
  
 In caso di preparazione posticipata, la chiamata a **SQLDescribeCol** o **SQLDescribeParam** prima di chiamare **SQLExecute** genera un round trip aggiuntivo al server. In **SQLDescribeCol** il driver rimuove la clausola WHERE dalla query e la invia al server con SET FMTONLY ON per ottenere la descrizione delle colonne del primo set di risultati restituito dalla query. In **SQLDescribeParam** il driver chiama il server per ottenere una descrizione delle espressioni o delle colonne a cui fanno riferimento i marcatori dei parametri nella query. Questo metodo presenta inoltre alcune restrizioni, ad esempio non è in grado di risolvere i parametri nelle sottoquery.  
  
 Un utilizzo eccessivo di **SQLPrepare** con il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client comporta un peggioramento delle prestazioni, soprattutto quando si è connessi a versioni precedenti di SQL Server. L'esecuzione preparata non deve essere utilizzata per le istruzioni eseguite una sola volta. L'esecuzione preparata è più lenta dell'esecuzione diretta per una singola esecuzione di un'istruzione perché richiede un round trip in rete aggiuntivo dal client al server. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene inoltre generata una stored procedure temporanea.  
  
 Le istruzioni preparate non possono essere utilizzate per creare oggetti temporanei in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Alcune applicazioni ODBC iniziali utilizzano **SQLPrepare** ogni volta che si utilizza [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md) . **SQLBindParameter** non richiede l'uso di **SQLPrepare**, può essere usato con **SQLExecDirect**. Ad esempio, utilizzare **SQLExecDirect** con **SQLBindParameter** per recuperare il codice restituito o i parametri di output da un stored procedure eseguito una sola volta. Non usare **SQLPrepare** con **SQLBindParameter** a meno che la stessa istruzione non venga eseguita più volte.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di istruzioni &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
