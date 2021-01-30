---
description: 'Appendice B: Tabelle della transizione di stato ODBC'
title: 'Appendice B: tabelle di transizione dello stato ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC]
- transitioning states [ODBC], about state transitions
- state transitions [ODBC], about state transitions
ms.assetid: 15088dbe-896f-4296-b397-02bb3d0ac0fb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 67c14205590ccdf9d20a30f44c13aa2da5abbe8d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212567"
---
# <a name="appendix-b-odbc-state-transition-tables"></a>Appendice B: Tabelle della transizione di stato ODBC
Nelle tabelle di questa appendice viene illustrato il modo in cui le funzioni ODBC determinano transizioni degli Stati dell'ambiente, della connessione, dell'istruzione e del descrittore. Lo stato dell'ambiente, della connessione, dell'istruzione o del descrittore determina in genere quando è possibile chiamare le funzioni che utilizzano il tipo di handle corrispondente (ambiente, connessione, istruzione o descrittore). Gli Stati dell'ambiente, della connessione, dell'istruzione e del descrittore si sovrappongono approssimativamente come illustrato nelle illustrazioni seguenti. Ad esempio, l'esatta sovrapposizione degli Stati di connessione C5 e C6 e gli Stati di istruzione da S1 a S12 sono dipendenti dall'origine dati, perché le transazioni iniziano in momenti diversi in origini dati diverse e lo stato del descrittore D1i (descrittore allocato in modo implicito) dipende dallo stato di qualsiasi istruzione. Per una descrizione di ogni stato, vedere [transizioni di ambiente](../../../odbc/reference/appendixes/environment-transitions.md), [transizioni di connessione](../../../odbc/reference/appendixes/connection-transitions.md), [transizioni di istruzioni](../../../odbc/reference/appendixes/statement-transitions.md)e [transizioni di descrittori](../../../odbc/reference/appendixes/descriptor-transitions.md), più avanti in questa appendice.  
  
 Gli Stati dell'ambiente e della connessione si sovrappongono come segue:  
  
 ![Sovrapposizione degli stati della connessione e dell'ambiente](../../../odbc/reference/appendixes/media/app01.gif "app01")  
  
 Gli Stati di connessione e di istruzione si sovrappongono come segue:  
  
 ![Sovrapposizione degli stati della connessione e dell'istruzione](../../../odbc/reference/appendixes/media/app02.gif "app02,")  
  
 Gli Stati dell'istruzione e del descrittore si sovrappongono come segue:  
  
 ![Sovrapposizione degli stati del descrittore e dell'istruzione](../../../odbc/reference/appendixes/media/app03.gif "app03")  
  
 Gli Stati di connessione e descrittore si sovrappongono come segue:  
  
 ![Sovrapposizione degli stati del descrittore e della connessione](../../../odbc/reference/appendixes/media/app04.gif "app04")  
  
 Ogni voce in una tabella di transizione può essere uno dei valori seguenti:  
  
-   **--** -Lo stato è invariato dopo l'esecuzione della funzione.  
  
-   **E**  

     **_n_** , **C_n_**, **S_n_** o **D_n_** : lo stato dell'ambiente, della connessione, dell'istruzione o del descrittore si sposta nello stato specificato.  
 
-   **(IH)** : un handle non valido è stato passato alla funzione. Se l'handle è un handle null o è un handle valido del tipo errato. ad esempio, è stato passato un handle di connessione quando è richiesto un handle di istruzione. la funzione restituisce SQL_INVALID_HANDLE; in caso contrario, il comportamento non è definito e probabilmente è irreversibile. Questo errore viene visualizzato solo quando è l'unico risultato possibile della chiamata della funzione nello stato specificato. Questo errore non modifica lo stato e viene sempre rilevato da Gestione driver, come indicato dalle parentesi.  
  
-   **NS** : stato successivo. La transizione dell'istruzione è identica a quella di se l'istruzione non è passata attraverso gli Stati asincroni. Si supponga, ad esempio, che un'istruzione che crea un set di risultati entri nello stato S11 dallo stato S1 perché **SQLExecDirect** ha restituito SQL_STILL_EXECUTING. La notazione NS nello stato S11 indica che le transizioni per l'istruzione sono le stesse di quelle per un'istruzione nello stato S1 che consente di creare un set di risultati. Se **SQLExecDirect** restituisce un errore, l'istruzione rimane nello stato S1; Se ha esito positivo, l'istruzione passa allo stato S5; Se sono necessari dati, l'istruzione passa allo stato S8; Se è ancora in esecuzione, rimane nello stato S11.  

-   **_Xxxxx_**  o **(*xxxxx*)** : valore SQLSTATE correlato alla tabella di transizione. Gli stati rilevati da Gestione driver sono racchiusi tra parentesi. La funzione ha restituito SQL_ERROR e il SQLSTATE specificato, ma lo stato non cambia. Se, ad esempio, viene chiamato **SQLExecute** prima di **SQLPrepare**, viene restituito SQLSTATE HY010 (errore della sequenza di funzioni).  

> [!NOTE]  
>  Nelle tabelle non vengono visualizzati errori non correlati alle tabelle di transizione che non modificano lo stato. Ad esempio, quando **SQLAllocHandle** viene chiamato nello stato dell'ambiente E1 e restituisce SQLSTATE HY001 (errore di allocazione della memoria), l'ambiente rimane nello stato E1; Questa operazione non viene visualizzata nella tabella di transizione dell'ambiente per **SQLAllocHandle**.  
  
 Se l'ambiente, la connessione, l'istruzione o il descrittore può passare a più di uno stato, viene visualizzato ogni possibile stato e una o più note a piè di pagina spiegano le condizioni in base alle quali viene effettuata ogni transizione. Le note a piè di pagina seguenti possono essere visualizzate in qualsiasi tabella.  
  
|Nota|Significato|  
|--------------|-------------|  
|b|Prima o dopo. Il cursore è stato posizionato prima dell'inizio del set di risultati o dopo la fine del set di risultati.|  
|c|Funzione corrente. La funzione corrente è stata eseguita in modo asincrono.|  
|d|Sono necessari i dati. La funzione ha restituito SQL_NEED_DATA.|  
|h|Errore. La funzione ha restituito SQL_ERROR.|  
|i|Riga non valida. Il cursore è stato posizionato in corrispondenza di una riga nel set di risultati e la riga è stata eliminata o si è verificato un errore in un'operazione sulla riga. Se la matrice di stato della riga esiste, il valore nella matrice di stato della riga per la riga è stato SQL_ROW_DELETED o SQL_ROW_ERROR. La matrice di stato della riga fa riferimento all'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.|  
|nf|Non trovato. La funzione ha restituito SQL_NO_DATA. Questa operazione non si applica quando **SQLExecDirect**, **SQLExecute** o **SQLParamData** restituisce SQL_NO_DATA dopo l'esecuzione di un'istruzione di aggiornamento o eliminazione ricercata.|  
|np|Non preparato. L'istruzione non è stata preparata.|  
|nr|Nessun risultato. L'istruzione non creerà o non creerà un set di risultati.|  
|o|Altra funzione. Un'altra funzione era in esecuzione in modo asincrono.|  
|p|Preparato. L'istruzione è stata preparata.|  
|r|Risultati. L'istruzione creerà o creerà un set di risultati (possibilmente vuoto).|  
|s|Esito positivo. La funzione ha restituito SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.|  
|v|Riga valida. Il cursore è stato posizionato in corrispondenza di una riga nel set di risultati e la riga è stata inserita correttamente, aggiornata correttamente oppure un'altra operazione sulla riga è stata completata correttamente. Se la matrice di stato della riga esiste, il valore nella matrice di stato della riga per la riga è SQL_ROW_ADDED, SQL_ROW_SUCCESS o SQL_ROW_UPDATED. La matrice di stato della riga fa riferimento all'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.|  
|x|In esecuzione. La funzione ha restituito SQL_STILL_EXECUTING.|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 In questo esempio, la riga nella tabella di transizione dello stato dell'ambiente per **SQLFreeHandle** quando *HandleType* è SQL_HANDLE_ENV è la seguente.  
  
|E0<br /><br /> Non allocato|E1<br /><br /> Allocato|E2<br /><br /> Connessioni|  
|------------------------|----------------------|-----------------------|  
|IH|E0|HY010|  
  
 Se **SQLFreeHandle** viene chiamato nello stato dell'ambiente E0 con *HandleType* impostato su SQL_HANDLE_ENV, gestione driver restituisce SQL_INVALID_HANDLE. Se viene chiamato nello stato E1 con *HandleType* impostato su SQL_HANDLE_ENV, l'ambiente passa allo stato E0 se la funzione ha esito positivo e rimane nello stato E1 se la funzione ha esito negativo. Se viene chiamato in stato E2 con *HandleType* impostato su SQL_HANDLE_ENV, gestione driver restituisce sempre SQL_ERROR e SQLSTATE HY010 (errore della sequenza di funzioni) e l'ambiente rimane nello stato E2.  
  
 Per comprendere le tabelle di transizione dello stato, è necessario conoscere l'elemento (ambiente, connessione, istruzione o descrittore) a cui fanno riferimento. Si supponga che una funzione accetti l'handle di un elemento di tipo X. La tabella di transizione dello stato X per la funzione descrive il modo in cui la chiamata della funzione, con l'handle di un elemento di tipo X, interessa tale elemento. **Disconnect** , ad esempio, accetta un handle di connessione. La tabella della transizione dello stato della connessione per **Disconnect** descrive il modo in cui **Disconnect** influiscono sullo stato della connessione per cui viene chiamata.  
  
 Si supponga che una funzione accetti l'handle di un elemento di tipo Y, dove Y non è uguale a X. La tabella di transizione dello stato X per la funzione descrive il modo in cui la chiamata della funzione, con un handle di tipo X associato all'elemento di tipo Y, influiscono sull'elemento di tipo Y. Ad esempio, la tabella di transizione dello stato dell'istruzione per **Disconnect** descrive il modo in cui **Disconnect** influiscono sullo stato di un'istruzione quando viene chiamato con l'handle della connessione a cui è associata l'istruzione.  
  
 Questa appendice contiene gli argomenti seguenti.  
  
-   [Transizioni di ambiente](../../../odbc/reference/appendixes/environment-transitions.md)  
  
-   [Transizioni di connessione](../../../odbc/reference/appendixes/connection-transitions.md)  
  
-   [Transizioni di istruzione](../../../odbc/reference/appendixes/statement-transitions.md)  
  
-   [Transizioni dei descrittori](../../../odbc/reference/appendixes/descriptor-transitions.md)
