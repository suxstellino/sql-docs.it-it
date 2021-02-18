---
title: Finestra Stack di chiamate
description: Informazioni su come usare la finestra Stack di chiamate del debugger Transact-SQL per visualizzare i tipi di dati dei parametri e i valori di stored procedure, funzioni e trigger.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Call Stack Window [Transact-SQL]
ms.assetid: ddb0b19c-87cd-4883-bcb8-ec09ffb30369
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d968299f4b2a2a36b6711e612a77dac6151db4a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339330"
---
# <a name="transact-sql-debugger---call-stack-window"></a>Debugger Transact-SQL - Finestra Stack di chiamate

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Nella finestra **Stack di chiamate** vengono visualizzati i moduli nello stack di chiamate e i tipi di dati e i valori di qualsiasi parametro passati ai moduli. [!INCLUDE[tsql](../../includes/tsql-md.md)] i moduli includono stored procedure, funzioni e trigger. Per visualizzare lo stack di chiamate, è necessario utilizzare la modalità di debug.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Elenco attività

**Per accedere alla finestra Stack di chiamate**

- Scegliere **Finestre** dal menu **Debug**, quindi fare clic su **Stack di chiamate**.

**Per modificare il frame dello stack di chiamate corrente**

Per impostare il frame dello stack come frame corrente, è possibile utilizzare una delle procedure seguenti:

- Fare clic con il pulsante destro del mouse sul frame dello stack, quindi scegliere **Passa al frame**.

- Fare doppio clic sul frame dello stack.  

**Per visualizzare l'origine di un frame diversa dal frame corrente**

- Fare clic con il pulsante destro del mouse sul frame dello stack, quindi scegliere **Vai a codice sorgente**.

## <a name="stack-frames"></a>Frame dello stack

Ogni riga nella finestra **Stack di chiamate** è denominata frame dello stack e rappresenta una chiamata da un file script [!INCLUDE[tsql](../../includes/tsql-md.md)] a un modulo o una chiamata da un modulo a un altro modulo. Il frame dello stack inferiore nella visualizzazione indica la riga nella finestra dell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] che ha eseguito la prima chiamata nello stack. La riga superiore indica la riga in cui il debugger ha sospeso l'esecuzione ed è identificata da una freccia gialla nel margine sinistro della finestra. Ogni riga intermedia indica il modulo e il numero di riga del codice sorgente che ha chiamato il frame dello stack superiore successivo.  

Tutte le espressioni incluse nelle finestre **Variabili locali**, **Espressione di controllo** e **Controllo immediato** vengono valutate in base al frame dello stack corrente. Nella finestra dell'editor di query viene visualizzato il codice per il frame corrente. Per impostazione predefinita, il frame dello stack corrente è il frame in cui il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] ha sospeso l'esecuzione. Quando si imposta il frame dello stack corrente su un altro frame, le espressioni incluse nelle finestre **Variabili locali**, **Espressione di controllo** e **Controllo immediato** vengono nuovamente valutate nel contesto del nuovo frame e il codice sorgente del nuovo frame viene visualizzato nella finestra dell'editor di query.  
  
## <a name="columns"></a>Colonne

 **Nome**  
 Vengono visualizzate informazioni su un modulo nello stack di chiamate.  
  
 Per la riga inferiore nello stack di chiamate, in **Nome** vengono indicati la finestra di origine dell'editor di query e il numero di riga della prima chiamata nello stack. Per le altre righe, **Nome** usa il formato **Module(Instance.Database)(ParmList) LineNumber**.  
  
 **Modulo**  
 Nome della stored procedure o della funzione che ha eseguito la chiamata al frame successivo.  
  
 **Instance.Database**  
 Istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e database che contiene il modulo.  
  
 **ParmList**  
 Indica il tipo di dati, il nome e il valore per ogni parametro passato durante la chiamata al modulo.  
  
 **LineNumber**  
 Per tutte le righe ad eccezione di quella superiore, **LineNumber** indica la riga nel modulo che ha eseguito la chiamata al frame. Per la riga superiore, **LineNumber** indica la riga in cui è attualmente attivo il debugger.  
  
 **Lingua**  
 Viene visualizzato **Transact-SQL** per [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="see-also"></a>Vedere anche

- [Debugger Transact-SQL](./transact-sql-debugger.md)
- [Informazioni del debugger Transact-SQL](./transact-sql-debugger-information.md)
- [Eseguire istruzione per istruzione il codice Transact-SQL](./step-through-transact-sql-code.md)