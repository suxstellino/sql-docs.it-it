---
description: Funzione SQLDataSources
title: Funzione SQLDataSources | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSources
helpviewer_keywords:
- SQLDataSources function [ODBC]
ms.assetid: 3f63b1b4-e70e-44cd-96c6-6878d50d0117
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f22039a9e5f1443f828523d67a4cfa6c10496b23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206100"
---
# <a name="sqldatasources-function"></a>Funzione SQLDataSources
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLDataSources** restituisce informazioni su un'origine dati. Questa funzione è implementata solo da Gestione driver.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLDataSources(  
     SQLHENV          EnvironmentHandle,  
     SQLUSMALLINT     Direction,  
     SQLCHAR *        ServerName,  
     SQLSMALLINT      BufferLength1,  
     SQLSMALLINT *    NameLength1Ptr,  
     SQLCHAR *        Description,  
     SQLSMALLINT      BufferLength2,  
     SQLSMALLINT *    NameLength2Ptr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *EnvironmentHandle*  
 Input Handle di ambiente.  
  
 *Direzione*  
 Input Determina l'origine dati che Gestione driver restituisce informazioni. I possibili valori sono i seguenti:  
  
 SQL_FETCH_NEXT (per recuperare il nome dell'origine dati successiva nell'elenco), SQL_FETCH_FIRST (per recuperare dall'inizio dell'elenco), SQL_FETCH_FIRST_USER (per recuperare il primo DSN utente) o SQL_FETCH_FIRST_SYSTEM (per recuperare il primo DSN di sistema).  
  
 Quando *Direction* è impostato su SQL_FETCH_FIRST, le chiamate successive a **SQLDataSources** con la *direzione* impostata su SQL_FETCH_NEXT restituiscono sia i DSN utente che quelli di sistema. Quando *Direction* è impostato su SQL_FETCH_FIRST_USER, tutte le chiamate successive a **SQLDataSources** con la *direzione* impostata su SQL_FETCH_NEXT restituiscono solo DSN utente. Quando *Direction* è impostato su SQL_FETCH_FIRST_SYSTEM, tutte le chiamate successive a **SQLDataSources** con *direzione* impostata su SQL_FETCH_NEXT restituiscono solo DSN di sistema.  
  
 *ServerName*  
 Output Puntatore a un buffer in cui restituire il nome dell'origine dati.  
  
 Se *ServerName* è null, *NameLength1Ptr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *ServerName*.  
  
 *BufferLength1*  
 Input Lunghezza del buffer **ServerName* , in caratteri; non è necessario che sia più lungo di SQL_MAX_DSN_LENGTH più il carattere di terminazione null.  
  
 *NameLength1Ptr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri, escluso il carattere di terminazione null, disponibile per la restituzione in \* *ServerName*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *BufferLength1*, il nome dell'origine dati in \* *ServerName* viene troncato in *BufferLength1* meno la lunghezza di un carattere di terminazione null.  
  
 *Descrizione*  
 Output Puntatore a un buffer in cui restituire la descrizione del driver associato all'origine dati. Ad esempio, dBASE o SQL Server.  
  
 Se *Description* è null, *NameLength2Ptr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta la *Descrizione*.  
  
 *BufferLength2*  
 Input Lunghezza in caratteri del buffer della *Descrizione* *.  
  
 *NameLength2Ptr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri, escluso il carattere di terminazione null, disponibile per restituire la \* *Descrizione*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *BufferLength2*, la descrizione del driver nella \* *Descrizione* viene troncata a *BufferLength2* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLDataSources** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_ENV e un *handle* di *EnvironmentHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLDataSources** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|(DM) Gestione driver-messaggio informativo specifico. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|(DM) il nome \* *ServerName* del buffer non è sufficiente per restituire il nome completo dell'origine dati. Pertanto, il nome è stato troncato. La lunghezza dell'intero nome dell'origine dati viene restituita in \* *NameLength1Ptr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)<br /><br /> (DM) la \* *Descrizione* del buffer non è sufficientemente grande da restituire la descrizione completa del driver. Pertanto, la descrizione è stata troncata. La lunghezza della descrizione dell'origine dati non troncata viene restituita in **NameLength2Ptr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|(DM) si è verificato un errore per il quale non è presente alcun SQLSTATE specifico e per cui non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|(DM) Gestione driver non è in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *BufferLength1* è minore di 0.<br /><br /> (DM) il valore specificato per l'argomento *BufferLength2* è minore di 0.|  
|HY103|Codice di recupero non valido|(DM) il valore specificato per la *direzione* dell'argomento non è uguale a SQL_FETCH_FIRST, SQL_FETCH_FIRST_USER, SQL_FETCH_FIRST_SYSTEM o SQL_FETCH_NEXT.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Commenti  
 Poiché **SQLDataSources** viene implementato in Gestione driver, è supportato per tutti i driver indipendentemente dalla conformità agli standard di un driver specifico.  
  
 Un'applicazione può chiamare più volte **SQLDataSources** per recuperare tutti i nomi di origine dati. Gestione driver recupera queste informazioni dalle informazioni di sistema. Quando non sono più presenti nomi di origine dati, gestione driver restituisce SQL_NO_DATA. Se **SQLDataSources** viene chiamato con SQL_FETCH_NEXT immediatamente dopo la restituzione SQL_NO_DATA, restituirà il primo nome dell'origine dati. Per informazioni sull'utilizzo delle informazioni restituite da **SQLDataSources** da parte di un'applicazione, vedere [scelta di un'origine dati o di un driver](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Se SQL_FETCH_NEXT viene passato a **SQLDataSources** la prima volta che viene chiamato, restituirà il primo nome dell'origine dati.  
  
 Il driver determina come viene eseguito il mapping dei nomi delle origini dati alle origini dati effettive.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Individuazione ed elenco dei valori necessari per la connessione a un'origine dati|[Funzione SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connessione a un'origine dati|[Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Connessione a un'origine dati utilizzando una stringa di connessione o una finestra di dialogo|[Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Restituzione di attributi e descrizioni dei driver|[Funzione SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
