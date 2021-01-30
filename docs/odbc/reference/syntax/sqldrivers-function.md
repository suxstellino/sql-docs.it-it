---
description: Funzione SQLDrivers
title: Funzione SQLDrivers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDrivers
helpviewer_keywords:
- SQLDrivers function [ODBC]
ms.assetid: 6b5b7514-e9cb-4cfd-8b7a-ab51dfab9efa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 905dfe1bc24872468868abef9f1c8cc2323d110a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195614"
---
# <a name="sqldrivers-function"></a>Funzione SQLDrivers
**Conformità**  
 Versione introdotta: ODBC 2,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLDrivers** elenca le descrizioni dei driver e le parole chiave degli attributi del driver. Questa funzione è implementata solo da Gestione driver.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLDrivers(  
     SQLHENV         EnvironmentHandle,  
     SQLUSMALLINT    Direction,  
     SQLCHAR *       DriverDescription,  
     SQLSMALLINT     BufferLength1,  
     SQLSMALLINT *   DescriptionLengthPtr,  
     SQLCHAR *       DriverAttributes,  
     SQLSMALLINT     BufferLength2,  
     SQLSMALLINT *   AttributesLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *EnvironmentHandle*  
 Input Handle di ambiente.  
  
 *Direzione*  
 Input Determina se Gestione driver recupera la descrizione del driver successiva nell'elenco (SQL_FETCH_NEXT) o se la ricerca inizia dall'inizio dell'elenco (SQL_FETCH_FIRST).  
  
 *DriverDescription*  
 Output Puntatore a un buffer in cui restituire la descrizione del driver.  
  
 Se *DriverDescription* è null, *DescriptionLengthPtr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *DriverDescription*.  
  
 *BufferLength1*  
 Input Lunghezza del buffer **DriverDescription* , in caratteri.  
  
 *DescriptionLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri, escluso il carattere di terminazione null, disponibile per restituire in \* *DriverDescription*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *BufferLength1*, la descrizione del driver in \* *DriverDescription* viene troncata a *BufferLength1* meno la lunghezza di un carattere di terminazione null.  
  
 *DriverAttributes*  
 Output Puntatore a un buffer in cui restituire l'elenco di coppie di valori di attributo driver (vedere "Commenti").  
  
 Se *DriverAttributes* è null, *AttributesLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *DriverAttributes*.  
  
 *BufferLength2*  
 Input Lunghezza del buffer \* *DriverAttributes* , in caratteri. Se il valore *\* DriverDescription* è una stringa Unicode (quando si chiama **SQLDriversW**), l'argomento *bufferLength* deve essere un numero pari.  
  
 *AttributesLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di byte, escluso il byte di terminazione null, disponibile per restituire in \* *DriverAttributes*. Se il numero di byte disponibili per restituire è maggiore o uguale a *BufferLength2*, l'elenco di coppie di valori di attributo in \* *DriverAttributes* viene troncato a *BufferLength2* meno la lunghezza del carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLDrivers** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *HandleType* di SQL_HANDLE_ENV e un *handle* di *EnvironmentHandle*. Nella tabella seguente sono elencati i valori SQLSTATE generalmente restituiti da **SQLDrivers** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|(DM) Gestione driver-messaggio informativo specifico. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|(DM) il buffer \* *DriverDescription* non era sufficientemente grande da restituire la descrizione completa del driver. Pertanto, la descrizione è stata troncata. La lunghezza della descrizione del driver completa viene restituita in \* *DescriptionLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)<br /><br /> (DM) il buffer \* *DriverAttributes* non era sufficientemente grande da restituire l'elenco completo di coppie di valori di attributo. Pertanto, l'elenco è stato troncato. La lunghezza dell'elenco non troncato di coppie di valori di attributo viene restituita in **AttributesLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|(DM) Gestione driver non è in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** è stato chiamato per *statementHandle* e restituito SQL_PARAM_DATA_AVAILABLE. Questa funzione è stata chiamata prima del recupero dei dati per tutti i parametri trasmessi.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *BufferLength1* è minore di 0.<br /><br /> (DM) il valore specificato per l'argomento *BufferLength2* è minore di 0 o uguale a 1.|  
|HY103|Codice di recupero non valido|(DM) il valore specificato per la *direzione* dell'argomento non è uguale a SQL_FETCH_FIRST o SQL_FETCH_NEXT.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Commenti  
 **SQLDrivers** restituisce la descrizione del driver nel \* buffer *DriverDescription* . Restituisce informazioni aggiuntive sul driver nel \* buffer *DriverAttributes* come un elenco di coppie parola chiave/valore. Tutte le parole chiave elencate nelle informazioni di sistema per i driver verranno restituite per tutti i driver, ad eccezione di **CreateDSN**, che viene usato per richiedere la creazione di origini dati ed è pertanto facoltativo. Ogni coppia viene terminata con un byte null e l'elenco completo termina con un byte null (ovvero due byte null contrassegnano la fine dell'elenco). Ad esempio, un driver basato su file che usa la sintassi C può restituire l'elenco di attributi seguente ("\ 0" rappresenta un carattere null):  
  
```  
FileUsage=1\0FileExtns=*.dbf\0\0  
```  
  
 Se \* *DriverAttributes* non è sufficientemente grande da mantenere l'intero elenco, l'elenco viene troncato, **SQLDrivers** restituisce SQLSTATE 01004 (dati troncati) e la lunghezza dell'elenco (escluso il byte finale con terminazione null) viene restituita in **AttributesLengthPtr*.  
  
 Quando il driver è installato, vengono aggiunte parole chiave degli attributi del driver dalle informazioni di sistema. Per ulteriori informazioni, vedere [Installing ODBC Components](../../../odbc/reference/install/installing-odbc-components.md).  
  
 Un'applicazione può chiamare **SQLDrivers** più volte per recuperare tutte le descrizioni dei driver. Gestione driver recupera queste informazioni dalle informazioni di sistema. Quando non sono più presenti descrizioni di driver, **SQLDrivers** restituisce SQL_NO_DATA. Se **SQLDrivers** viene chiamato con SQL_FETCH_NEXT immediatamente dopo la restituzione SQL_NO_DATA, restituisce la prima descrizione del driver. Per informazioni sul modo in cui un'applicazione utilizza le informazioni restituite da **SQLDrivers**, vedere [scelta di un'origine dati o di un driver](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Se SQL_FETCH_NEXT viene passato a **SQLDrivers** la prima volta che viene chiamato, **SQLDrivers** restituisce il primo nome dell'origine dati.  
  
 Poiché **SQLDrivers** è implementato in Gestione driver, è supportato per tutti i driver indipendentemente dalla conformità agli standard di un driver specifico.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Individuazione ed elenco dei valori necessari per la connessione a un'origine dati|[Funzione SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connessione a un'origine dati|[Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Restituzione di nomi di origini dati|[Funzione SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)|  
|Connessione a un'origine dati utilizzando una stringa di connessione o una finestra di dialogo|[Funzione SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
