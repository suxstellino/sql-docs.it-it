---
description: Funzione SQLGetEnvAttr
title: Funzione SQLGetEnvAttr | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetEnvAttr
helpviewer_keywords:
- SQLGetEnvAttr function [ODBC]
ms.assetid: 01f4590f-427a-4280-a1c3-18de9f7d86c1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 35df579ce679b8375574a16eea16a2f5d84c3043
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199852"
---
# <a name="sqlgetenvattr-function"></a>Funzione SQLGetEnvAttr
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetEnvAttr** restituisce l'impostazione corrente di un attributo Environment.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLGetEnvAttr(  
     SQLHENV        EnvironmentHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *EnvironmentHandle*  
 Input Handle di ambiente.  
  
 *Attributo*  
 Input Attributo da recuperare.  
  
 *ValuePtr*  
 Output Puntatore a un buffer in cui restituire il valore corrente dell'attributo specificato dall' *attributo*.  
  
 Se *ValuePtr* è null, *StringLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *ValuePtr*.  
  
 *BufferLength*  
 Input Se *ValuePtr* punta a una stringa di caratteri, questo argomento deve essere la lunghezza di \* *ValuePtr*. Se \* *ValuePtr* è un numero intero, *bufferLength* viene ignorato. Se *\* ValuePtr* è una stringa Unicode (quando si chiama **SQLGetEnvAttrW**), l'argomento *bufferLength* deve essere un numero pari. Se il valore dell'attributo non è una stringa di caratteri, *bufferLength* è inutilizzato.  
  
 *StringLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di byte, escluso il carattere di terminazione null, disponibile per restituire in *\* ValuePtr*. Se il valore dell'attributo è una stringa di caratteri e il numero di byte disponibili per restituire è maggiore o uguale a *bufferLength*, i dati in \* *ValuePtr* vengono troncati a *bufferLength* meno la lunghezza di un carattere di terminazione null e con terminazione null dal driver.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLGetEnvAttr** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_ENV e un *handle* di *EnvironmentHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLGetEnvAttr** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|I dati restituiti in \* *ValuePtr* sono stati troncati in modo da essere *bufferLength* meno il carattere di terminazione null. La lunghezza del valore stringa untruncated viene restituita in **StringLengthPtr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY010|Errore sequenza funzione|(DM) **SQL_ATTR_ODBC_VERSION** non è stato ancora impostato tramite **SQLSetEnvAttr**. Non è necessario impostare **SQL_ATTR_ODBC_VERSION** in modo esplicito se si utilizza **SQLAllocHandleStd**.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY092|Identificatore di attributo/opzione non valido|Il valore specificato per l' *attributo* argument non è valido per la versione di ODBC supportata dal driver.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il valore specificato per l' *attributo* argument è un attributo dell'ambiente ODBC valido per la versione di ODBC supportata dal driver ma non supportato dal driver.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver corrispondente a *EnvironmentHandle* non supporta la funzione.|  
  
## <a name="comments"></a>Commenti  
 Per un elenco di attributi, vedere [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md). Non sono presenti attributi di ambiente specifici del driver. Se *attribute* specifica un attributo che restituisce una stringa, *ValuePtr* deve essere un puntatore a un buffer in cui restituire la stringa. La lunghezza massima della stringa, incluso il byte di terminazione null, sarà *bufferLength* byte.  
  
 **SQLGetEnvAttr** può essere chiamato in qualsiasi momento tra l'allocazione e la liberazione di un handle di ambiente. Tutti gli attributi dell'ambiente impostati correttamente dall'applicazione per l'ambiente vengono mantenuti fino a quando non viene chiamato **SQLFreeHandle** su *EnvironmentHandle* con *HandleType* di SQL_HANDLE_ENV. È possibile allocare più di un handle di ambiente simultaneamente in ODBC 3 *. x*. Un attributo di ambiente in un ambiente non è influenzato quando è stato allocato un altro ambiente.  
  
> [!NOTE]
>  L'attributo di ambiente SQL_ATTR_OUTPUT_NTS è supportato dalle applicazioni conformi agli standard. Quando viene chiamato **SQLGetEnvAttr** , gestione driver ODBC 3 *. x* restituisce sempre SQL_TRUE per questo attributo. SQL_ATTR_OUTPUT_NTS può essere impostato su SQL_TRUE solo da una chiamata a **SQLSetEnvAttr**.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Restituzione dell'impostazione di un attributo di connessione|[Funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Restituzione dell'impostazione di un attributo di istruzione|[Funzione SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Impostazione di un attributo di connessione|[Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Impostazione di un attributo Environment|[SQLSetEnvAttr Function](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Impostazione di un attributo di istruzione|[Funzione SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
