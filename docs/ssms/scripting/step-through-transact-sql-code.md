---
title: Esecuzione istruzione per istruzione del codice Transact-SQL
description: Informazioni su come usare il debugger Transact-SQL per controllare quali istruzioni Transact-SQL vengono eseguite in una finestra dell'editor di query del motore di database.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, debugging code
- Transact-SQL debugger, step over
- Transact-SQL debugger, step out
- Transact-SQL debugger, step into
ms.assetid: e09079b8-c4c9-42b4-821b-4ce81a98a086
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a48a07ddec4cb6c1f0670a5e6b137690b6f2185
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340687"
---
# <a name="step-through-transact-sql-code"></a>Esecuzione istruzione per istruzione del codice Transact-SQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] consente di controllare quali istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono eseguite in una finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . È possibile sospendere l'esecuzione del debugger in corrispondenza di singole istruzioni e successivamente visualizzare lo stato degli elementi di codice in quei punti.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>Punti di interruzione

Un punto di interruzione indica al debugger di sospendere temporaneamente l'esecuzione in corrispondenza di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] specifica. Per altre informazioni sui punti di interruzione, vedere [Punti di interruzione Transact-SQL](./transact-sql-breakpoints.md).  
  
## <a name="controlling-statement-execution"></a>Controllo dell'esecuzione di istruzioni

Nel debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] è possibile specificare le opzioni seguenti per eseguire il codice [!INCLUDE[tsql](../../includes/tsql-md.md)] dall'istruzione corrente:

- Eseguire il codice fino al successivo punto di interruzione.

- Eseguire la successiva istruzione.  

    Se l'istruzione successiva richiama una stored procedure, una funzione o un trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] , verrà visualizzata dal debugger una nuova finestra dell'editor di query contenente il codice del modulo. La finestra sarà in modalità di debug e l'esecuzione verrà sospesa in corrispondenza della prima istruzione nel modulo. È possibile spostarsi quindi nel codice del modulo, ad esempio, impostando dei punti di interruzione o avanzando istruzione per istruzione nel codice.

- Passare all'istruzione successiva.

    Viene eseguita l'istruzione successiva. Tuttavia, se l'istruzione richiama una stored procedure, una funzione o un trigger, il codice del modulo viene eseguito fino alla fine e i risultati vengono restituiti al codice che ha effettuato la chiamata. Se si è sicuri che in una stored procedure non vi siano errori, è possibile superarla. L'esecuzione viene sospesa in corrispondenza dell'istruzione che segue la chiamata alla stored procedure, la funzione o il trigger.

- Uscire da una stored procedure, una funzione o un trigger.  

    L'esecuzione viene sospesa in corrispondenza dell'istruzione che segue la chiamata alla stored procedure, la funzione o il trigger.  

- Eseguire il codice dalla posizione corrente alla posizione corrente del puntatore e ignorare tutti i punti di interruzione.  

 Nella tabella seguente sono elencate le varie procedure che consentono di controllare il modo in cui le istruzioni sono eseguite nel debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
|Azione|Azione da eseguire:|  
|------------|---------------------|  
|Eseguire tutte le istruzioni dall'istruzione corrente al successivo punto di interruzione|Fare clic su **Continua** nel menu **Debug** .<br /><br /> Fare clic sul pulsante **Continua** sulla barra degli strumenti **Debug** .|  
|Eseguire la successiva istruzione o il successivo modulo.|Fare clic su **Esegui istruzione** nel menu **Debug** .<br /><br /> Fare clic sul pulsante **Esegui istruzione** sulla barra degli strumenti **Debug** .<br /><br /> Premere F11.|  
|Passare alla successiva istruzione o al successivo modulo.|Fare clic su **Esegui istruzione/routine** nel menu **Debug** .<br /><br /> Fare clic sul pulsante **Esegui istruzione/routine** sulla barra degli strumenti **Debug** .<br /><br /> Premere F10.|  
|Uscire da un modulo|Fare clic su **Esci da istruzione/routine** nel menu **Debug** .<br /><br /> Fare clic sul pulsante **Esci da istruzione/routine** sulla barra degli strumenti **Debug** .<br /><br /> Premere MAIUSC+F11.|  
|Eseguire il codice fino alla posizione corrente del cursore|Fare clic con il pulsante destro del mouse nella finestra dell'editor di query, quindi fare clic su **Esegui fino al cursore**.<br /><br /> Premere CTRL+F10.|  
  
## <a name="see-also"></a>Vedere anche

- [Informazioni del debugger Transact-SQL](./transact-sql-debugger-information.md)