---
title: finestra Variabili locali
description: Informazioni su come usare la finestra Variabili locali del debugger Transact-SQL per visualizzare e modificare le espressioni dallo stack frame di chiamate corrente.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Locals Window [Transact-SQL]
ms.assetid: 59bea640-7823-4b4d-832c-f384d83cca2f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dd6fc308bf65904d943b298546f2006b9f259abe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351610"
---
# <a name="transact-sql-debugger---locals-window"></a>Debugger Transact-SQL - Finestra Variabili locali

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Nella finestra **Variabili locali** vengono visualizzate informazioni sulle espressioni locali nell'ambito corrente del debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] . L'ambito è impostato sul frame dello stack di chiamate corrente selezionato nella finestra **Stack di chiamate** . Per visualizzare espressioni locali, è necessario utilizzare la modalità di debug.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Elenco attività

**Per accedere alla finestra Variabili locali**
  
-   Scegliere **Finestre** dal menu **Debug**, quindi fare clic su **Variabili locali**.  
  
 **Per modificare il valore di un'espressione**  
  
-   Fare clic con il pulsante destro del mouse sull'espressione e scegliere **Modifica valore**.  
  
## <a name="columns"></a>Colonne  
 **Nome**  
 Nome dell'espressione locale. Nel debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono elencati parametri, variabili e funzioni di sistema i cui nomi iniziano per @@.  
  
 **Valore**  
 Consente di visualizzare il valore è assegnato attualmente all'espressione locale. Questa colonna è vuota quando non è stato assegnato alcun valore all'espressione.  
  
 Se la lunghezza di un'espressione è maggiore della larghezza della colonna **Valore** , il valore completo verrà visualizzato in una descrizione comandi quando si sposta il puntatore sulla cella **Valore** per l'espressione.  
  
 Un'icona di lente di ingrandimento in una cella **Valore** indica che è disponibile il visualizzatore del debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] . Nell'elenco è possibile specificare **Visualizzatore testo**, **Visualizzatore XML** o **Visualizzatore HTML**. Per avviare un visualizzatore del debugger, fare clic sull'icona di lente di ingrandimento. Tramite il debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] viene visualizzata una finestra di dialogo contenente dati in un formato appropriato per il tipo di dati.  
  
 **Tipo**  
 Consente di visualizzare il tipo di dati dell'espressione.  
  
## <a name="see-also"></a>Vedere anche  
 [Debugger Transact-SQL](./transact-sql-debugger.md)   
 [Informazioni del debugger Transact-SQL](./transact-sql-debugger-information.md)   
 [finestra Espressioni di controllo](./transact-sql-debugger-watch-window.md)   
 [Finestra Stack di chiamate](./transact-sql-debugger-call-stack-window.md)   
 [Finestra di dialogo Controllo immediato](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Espressioni &#40; Transact-SQL &#41;](../../t-sql/language-elements/expressions-transact-sql.md)