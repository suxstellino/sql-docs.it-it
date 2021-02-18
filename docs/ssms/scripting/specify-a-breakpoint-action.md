---
title: Impostazione di un'azione del punto di interruzione
description: Informazioni su come specificare l'azione Quando raggiunto, ovvero un'attività personalizzata per il debugger Transact-SQL che viene eseguita quando viene raggiunto un punto di interruzione e vengono soddisfatte determinate altre condizioni.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint action
- Transact-SQL debugger, breakpoint when hit action
ms.assetid: f97f0097-6f51-40c1-b2e0-294a93ce1e1b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42902b55150c4efe5d9c81eb5698b3cff59b7aab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344138"
---
# <a name="specify-a-breakpoint-action"></a>Impostazione di un'azione del punto di interruzione

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Un'azione **Quando raggiunto** per un punto di interruzione specifica un'attività personalizzata eseguita dal debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] per un punto di interruzione. Se viene raggiunto il numero di passaggi specificato e viene soddisfatta qualsiasi condizione per il punto di interruzione, il debugger esegue l'azione specificata per il punto di interruzione.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
##  <a name="action-considerations"></a><a name="BKMK_ActionConsiderations"></a> Considerazioni sulle azioni

L'azione predefinita per un punto di interruzione consiste nell'interrompere l'esecuzione una volta raggiunto il numero di passaggi e soddisfatta la condizione per il punto di interruzione. L'utilizzo principale di un'azione **Quando raggiunto** nel debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] consiste nello stampare informazioni nella finestra **Output** del debugger specificando un messaggio di stampa.  
  
È possibile specificare un messaggio di stampa nell'opzione **Stampa un messaggio** . Il messaggio di stampa viene specificato come stringa di testo che include espressioni contenenti informazioni ottenute dal debug di [!INCLUDE[tsql](../../includes/tsql-md.md)] . Le espressioni includono gli elementi seguenti:  
  
-   Un'espressione [!INCLUDE[tsql](../../includes/tsql-md.md)] racchiusa tra parentesi graffe ({}). Le espressioni possono includere variabili, parametri e funzioni predefinite [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ad esempio, {@MyVariable}, {@NameParameter}, {@@SPID} o {SERVERPROPERTY('ProcessID')}.  
  
-   Una delle parole chiave seguenti:  
  
    1.  $ADDRESS restituisce il nome della stored procedure o della funzione definita dall'utente in cui è impostato il punto di interruzione. Se il punto di interruzione è impostato nella finestra dell'editor, $ADDRESS restituisce il nome del file di script modificato. $ADDRESS e $FUNCTION restituiscono le stesse informazioni nel debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
    2.  $CALLER restituisce il nome dell'unità di codice [!INCLUDE[tsql](../../includes/tsql-md.md)] che ha chiamato una stored procedure o una funzione. Se il punto di interruzione è nella finestra dell'editor, $CALLER restituisce \<No caller available>. Se il punto di interruzione è in una stored procedure o in una funzione definita dell'utente chiamata dal codice nella finestra dell'editor, $CALLER restituisce il nome del file modificato. Se il punto di interruzione è in una stored procedure o in una funzione definita dell'utente chiamata da un'altra stored procedure o funzione, $CALLER restituisce il nome della procedura o della funzione chiamante.  
  
    3.  $CALLSTACK restituisce lo stack di chiamate delle funzioni nella catena che hanno chiamato la stored procedure o la funzione definita dall'utente corrente. Se il punto di interruzione è nella finestra dell'editor, $CALLSTACK restituisce il nome del file di script modificato.  
  
    4.  $FUNCTION restituisce il nome della stored procedure o della funzione definita dall'utente in cui è impostato il punto di interruzione. Se il punto di interruzione è impostato nella finestra dell'editor, $FUNCTION restituisce il nome del file di script modificato.  
  
    5.  $PID e $PNAME restituiscono l'ID e il nome del processo del sistema operativo che esegue l'istanza del Motore di database in cui viene eseguito [!INCLUDE[tsql](../../includes/tsql-md.md)] . $PID restituisce lo stesso ID di SERVERPROPERTY('ProcessID'), con la differenza che $PID è un valore esadecimale mentre SERVERPROPERTY('ProcessID') è un valore decimale.  
  
    6.  $TID e $TNAME restituiscono l'ID e il nome del thread del sistema operativo che esegue il batch [!INCLUDE[tsql](../../includes/tsql-md.md)] . Il thread è un thread associato al processo che esegue l'istanza del Motore di database. $TID restituisce lo stesso valore di SELECT kpid FROM sys.sysprocesses WHERE spid = @@SPID, con la differenza che $TID è un valore esadecimale mentre kpid è un valore decimale.  
  
-   È anche possibile usare il carattere barra rovesciata (\\) come un carattere di escape per consentire la presenza di parentesi graffe e barre rovesciate nel messaggio: \\{, \\} e \\\\.  
  
#### <a name="to-specify-a-when-hit-action"></a>Per specificare un'azione Quando raggiunto  
  
1.  Nella finestra dell'editor fare clic con il pulsante destro del mouse sul glifo del punto di interruzione, quindi scegliere **Quando raggiunto** dal menu di scelta rapida.  
  
     -oppure-  
  
     Nella finestra **Punti di interruzione** fare clic con il pulsante destro del mouse sul glifo del punto di interruzione, quindi scegliere **Quando raggiunto** dal menu di scelta rapida.  
  
2.  Nella finestra di dialogo **Quando il punto di interruzione viene raggiunto** selezionare il comportamento desiderato:  
  
    1.  Selezionare **Stampa un messaggio** per stampare un messaggio nella finestra Output del debugger quando il punto di interruzione viene raggiunto.  
  
    2.  L'opzione **Esegui una macro** non è disponibile dal debugger [!INCLUDE[tsql](../../includes/tsql-md.md)] ed è disattivata.  
  
    3.  Selezionare **Continua esecuzione** se non si desidera che l'esecuzione venga sospesa dal punto di interruzione. Questa opzione è attiva solo se è stata selezionata l'opzione **Stampa un messaggio** .  
  
3.  Fare clic su **OK** per implementare le modifiche o su **Annulla** per uscire senza applicare le modifiche.  
  
## <a name="see-also"></a>Vedere anche  
 [Impostare una condizione del punto di interruzione](./specify-a-breakpoint-condition.md)   
 [Specificare un numero di passaggi](./specify-a-hit-count.md)