---
description: Funzionamento del driver
title: Cosa fa il driver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: 75dcdea6-ff6b-4ac8-aa11-a1f9edbeb8e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 37c6c84b74e6b89dc9b31a342e9c47c8a63eb030
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158174"
---
# <a name="what-the-driver-does"></a>Funzionamento del driver
Nella tabella seguente sono riepilogate le funzioni e gli attributi di istruzione che un driver ODBC *3. x* deve implementare per i cursori Block e scorrevoli.  
  
|Funzione or<br /><br /> attributo di istruzione|Commenti|  
|-----------------------------------------|--------------|  
|SQL_ATTR_ROW_STATUS_PTR|Imposta l'indirizzo della matrice di stato della riga compilata da **SQLFetch** e **SQLFetchScroll**. Questa matrice viene inoltre compilata da **SQLSetPos** se **SQLSetPos** viene chiamato nello stato dell'istruzione S6. Se **SQLSetPos** viene chiamato nello stato S7, questa matrice non viene compilata, ma viene riempita la matrice a cui fa riferimento l'argomento *RowStatusArray* di **SQLExtendedFetch** . Per ulteriori informazioni, vedere [transizioni di istruzioni](../../../odbc/reference/appendixes/statement-transitions.md) nell'Appendice B: tabelle di transizione dello stato ODBC.|  
|SQL_ATTR_ROWS_FETCHED_PTR|Imposta l'indirizzo del buffer in cui **SQLFetch** e **SQLFetchScroll** restituiscono il numero di righe recuperate. Se viene chiamato **SQLExtendedFetch** , questo buffer non viene compilato ma l'argomento *RowCountPtr* punta al numero di righe recuperate.|  
|SQL_ATTR_ROW_ARRAY_SIZE|Imposta le dimensioni del set di righe utilizzate da **SQLFetch** e **SQLFetchScroll**.|  
|SQL_ROWSET_SIZE|Imposta la dimensione del set di righe utilizzata da **SQLExtendedFetch**. I driver ODBC *3. x* implementano questa operazione se desiderano utilizzare le applicazioni ODBC *2. x* che chiamano **SQLExtendedFetch** o **SQLSetPos**.|  
|**SQLBulkOperations**|Se un driver ODBC *3. x* deve funzionare con le applicazioni ODBC *2. x* che **usano SQLSetPos** con un' *operazione* di SQL_ADD, il driver deve supportare **SQLSetPos** con un' *operazione* di SQL_ADD oltre a **SQLBulkOperations** con un' *operazione* di SQL_ADD.|  
|**SQLExtendedFetch**|Restituisce il set di righe specificato. I driver ODBC *3. x* implementano questa operazione se desiderano utilizzare le applicazioni ODBC *2. x* che chiamano **SQLExtendedFetch** o **SQLSetPos**. Di seguito sono riportati i dettagli di implementazione:<br /><br /> -Il driver recupera le dimensioni del set di righe dal valore dell'attributo dell'istruzione SQL_ROWSET_SIZE.<br />-Il driver recupera l'indirizzo della matrice di stato della riga dall'argomento *RowStatusArray* , non l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR. L'argomento *RowStatusArray* in una chiamata a **SQLExtendedFetch** non deve essere un puntatore null. Si noti che in ODBC *3. x*, l'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR può essere un puntatore null.<br />-Il driver recupera l'indirizzo del buffer recuperato dalle righe dall'argomento *RowCountPtr* , non l'attributo SQL_ATTR_ROWS_FETCHED_PTR Statement.<br />-Il driver restituisce SQLSTATE 01S01 (Error in Row) per indicare che si è verificato un errore durante il recupero delle righe da parte di una chiamata a **SQLExtendedFetch**. Un driver ODBC *3. x* deve restituire SQLSTATE 01S01 (Error in Row) solo quando viene chiamato **SQLExtendedFetch** , non quando viene chiamato **SQLFetch** o **SQLFetchScroll** . Per mantenere la compatibilità con le versioni precedenti, quando 01S01 (errore nella riga) viene restituito da **SQLExtendedFetch**, gestione driver non Ordina i record di stato nella coda degli errori in base alle regole indicate nella sezione "sequenza di record di stato" di [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).|  
|**SQLFetch**|Restituisce il set di righe successivo. Di seguito sono riportati i dettagli di implementazione:<br /><br /> -Il driver recupera le dimensioni del set di righe dal valore dell'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE.<br />-Il driver recupera l'indirizzo della matrice di stato della riga dall'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.<br />-Il driver recupera l'indirizzo del buffer recuperato dalle righe dall'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR.<br />-L'applicazione può combinare le chiamate tra **SQLFetchScroll** e **SQLFetch**.<br />-   **SQLFetch** restituisce segnalibri se è associata la colonna 0.<br />-   **SQLFetch** può essere chiamato per restituire più di una riga.<br />-Il driver non restituisce SQLSTATE 01S01 (Error in Row) per indicare che si è verificato un errore durante il recupero delle righe da parte di una chiamata a **SQLFetch**.|  
|**SQLFetchScroll**|Restituisce il set di righe specificato. Di seguito sono riportati i dettagli di implementazione:<br /><br /> -Il driver recupera le dimensioni del set di righe dall'attributo dell'istruzione SQL_ATTR_ROW_ARRAY_SIZE.<br />-Il driver recupera l'indirizzo della matrice di stato della riga dall'attributo dell'istruzione SQL_ATTR_ROW_STATUS_PTR.<br />-Il driver recupera l'indirizzo del buffer recuperato dalle righe dall'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR.<br />-L'applicazione può combinare le chiamate tra **SQLFetchScroll** e **SQLFetch**.<br />-Il driver non restituisce SQLSTATE 01S01 (Error in Row) per indicare che si è verificato un errore durante il recupero delle righe da parte di una chiamata a **SQLFetchScroll**.|  
|**SQLSetPos**|Esegue diverse operazioni posizionate. Di seguito sono riportati i dettagli di implementazione:<br /><br /> -Può essere chiamato negli Stati di istruzione S6 o S7. Per ulteriori informazioni, vedere [transizioni di istruzioni](../../../odbc/reference/appendixes/statement-transitions.md) nell'Appendice B: tabelle di transizione dello stato ODBC.<br />-Se questa operazione viene chiamata nello stato dell'istruzione S5 o S6, il driver recupera le dimensioni del set di righe dall'attributo dell'istruzione SQL_ATTR_ROWS_FETCHED_PTR e l'indirizzo della matrice di stato della riga dall'attributo SQL_ATTR_ROW_STATUS_PTR istruzione.<br />-Se viene chiamato nello stato dell'istruzione S7, il driver recupera le dimensioni del set di righe dall'attributo dell'istruzione SQL_ROWSET_SIZE e l'indirizzo della matrice di stato della riga dall'argomento *RowStatusArray* di **SQLExtendedFetch**.<br />-Il driver restituisce SQLSTATE 01S01 (Error in Row) solo per indicare che si è verificato un errore durante il recupero delle righe da parte di una chiamata a **SQLSetPos** per eseguire un'operazione bulk quando la funzione viene chiamata nello stato S7. Per mantenere la compatibilità con le versioni precedenti, se SQLSetPos SQLSTATE 01S01 (Error in Row) viene restituito da , gestione driver non Ordina i record di stato nella coda degli errori in base alle regole indicate nella sezione "sequenza di record di stato" di [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).<br />-Se il driver deve funzionare con le applicazioni ODBC *2. x* che chiamano **SQLSetPos** con un argomento *Operation* di SQL_ADD, il driver deve supportare **SQLSetPos** con un argomento *Operation* di SQL_ADD.|
