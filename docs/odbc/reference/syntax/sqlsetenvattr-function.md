---
description: SQLSetEnvAttr Function
title: Funzione SQLSetEnvAttr | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetEnvAttr
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC]
ms.assetid: 0343241c-4b15-4d4b-aa2b-2e8ab5215cd2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed611f343275c2d777d4f390154b160aa6346094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200959"
---
# <a name="sqlsetenvattr-function"></a>SQLSetEnvAttr Function
**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLSetEnvAttr** imposta gli attributi che governano gli aspetti degli ambienti.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLSetEnvAttr(  
     SQLHENV      EnvironmentHandle,  
     SQLINTEGER   Attribute,  
     SQLPOINTER   ValuePtr,  
     SQLINTEGER   StringLength);  
```  
  
## <a name="arguments"></a>Argomenti  
 *EnvironmentHandle*  
 Input Handle di ambiente.  
  
 *Attributo*  
 Input Attributo da impostare, elencato in "Commenti".  
  
 *ValuePtr*  
 Input Puntatore al valore da associare all' *attributo*. A seconda del valore dell' *attributo*, *ValuePtr* sarà un valore integer a 32 bit oppure punterà a una stringa di caratteri con terminazione null.  
  
 *StringLength*  
 Input Se *ValuePtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di **ValuePtr*. Per i dati di tipo stringa di caratteri, questo argomento deve contenere il numero di byte nella stringa.  
  
 Se *ValuePtr* è un numero intero, *StringLength* viene ignorato.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLSetEnvAttr** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con *HandleType* di SQL_HANDLE_ENV e un *handle* di *EnvironmentHandle*. Nella tabella seguente sono elencati i valori SQLSTATE restituiti in genere da **SQLSetEnvAttr** e ne viene illustrato ciascuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente. Se un driver non supporta un attributo di ambiente, l'errore può essere restituito solo durante la fase di connessione.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Il driver non supporta il valore specificato in *ValuePtr* e sostituisce un valore simile. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* MessageText* descrive l'errore e la sua origine.|  
|HY001|Errore di allocazione della memoria|Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY009|Uso non valido del puntatore null|L'argomento attribute ha identificato un attributo Environment che richiede un valore stringa e l'argomento *ValuePtr* è un puntatore null.|  
|HY010|Errore sequenza funzione|(DM) è stato allocato un handle di connessione in *EnvironmentHandle*.<br /><br /> (DM) **SQL_ATTR_ODBC_VERSION** non è stato impostato con **SQLSetEnvAttr** e l' *attributo* non è uguale a **SQL_ATTR_ODBC_VERSION**. Non è necessario impostare **SQL_ATTR_ODBC_VERSION** in modo esplicito se si utilizza **SQLAllocHandleStd**.|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione perché non è possibile accedere agli oggetti memoria sottostante, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY024|Valore di attributo non valido|Dato il valore dell' *attributo* specificato, in *ValuePtr* è stato specificato un valore non valido.|  
|HY090|Lunghezza della stringa o del buffer non valida|L'argomento *StringLength* è minore di 0 ma non è stato SQL_NTS.|  
|HY092|Identificatore di attributo/opzione non valido|(DM) il valore specificato per l' *attributo* argument non era valido per la versione di ODBC supportata dal driver.|  
|HY117|Connessione sospesa a causa di uno stato di transazione sconosciuto. Sono consentite solo le funzioni di disconnessione e di sola lettura.|(DM) per ulteriori informazioni sullo stato Suspended, vedere [funzione SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Funzionalità facoltativa non implementata|Il valore specificato per l' *attributo* argument è un attributo dell'ambiente ODBC valido per la versione di ODBC supportata dal driver, ma non è supportato dal driver.<br /><br /> (DM) l'argomento dell' *attributo* è stato SQL_ATTR_OUTPUT_NTS e *ValuePtr* è stato SQL_FALSE.|  
  
## <a name="comments"></a>Commenti  
 Un'applicazione può chiamare **SQLSetEnvAttr** solo se nell'ambiente non è allocato alcun handle di connessione. Tutti gli attributi dell'ambiente impostati correttamente dall'applicazione per l'ambiente vengono mantenuti fino a quando non viene chiamato **SQLFreeHandle** sull'ambiente. È possibile allocare più di un handle di ambiente simultaneamente in ODBC *3. x*.  
  
 Il formato delle informazioni impostate tramite *ValuePtr* dipende dall' *attributo* specificato. **SQLSetEnvAttr** accetterà informazioni sugli attributi in uno dei due formati diversi, ovvero una stringa di caratteri con terminazione null o un valore integer a 32 bit. Il formato di ogni è indicato nella descrizione dell'attributo.  
  
 Non sono presenti attributi di ambiente specifici del driver.  
  
 Impossibile impostare gli attributi di connessione tramite una chiamata a **SQLSetEnvAttr**. Il tentativo di eseguire questa operazione restituirà SQLSTATE HY092 (identificatore di attributo/opzione non valido).  
  
|*Attributo*|Contenuto di *ValuePtr*|  
|-----------------|-------------------------|  
|SQL_ATTR_CONNECTION_POOLING (ODBC 3,8)|Valore SQLUINTEGER a 32 bit che Abilita o Disabilita il pool di connessioni a livello di ambiente. Vengono usati i valori seguenti:<br /><br /> SQL_CP_OFF = il pool di connessioni è disattivato. Questo è il valore predefinito.<br /><br /> SQL_CP_ONE_PER_DRIVER = è supportato un singolo pool di connessioni per ogni driver. Ogni connessione in un pool è associata a un driver.<br /><br /> SQL_CP_ONE_PER_HENV = è supportato un singolo pool di connessioni per ogni ambiente. Ogni connessione in un pool è associata a un ambiente.<br /><br /> SQL_CP_DRIVER_AWARE = utilizzare la funzionalità di riconoscimento del pool di connessioni del driver, se disponibile. Se il driver non supporta il riconoscimento del pool di connessioni, SQL_CP_DRIVER_AWARE viene ignorato e viene utilizzato SQL_CP_ONE_PER_HENV. Per ulteriori informazioni, vedere [pool di connessioni compatibile](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)con i driver. In un ambiente in cui alcuni driver supportano e alcuni driver non supportano la funzionalità di riconoscimento del pool di connessioni, SQL_CP_DRIVER_AWARE possibile abilitare la funzionalità di riconoscimento del pool di connessioni su questi driver di supporto, ma equivale a impostare su SQL_CP_ONE_PER_HENV sui driver che non supportano la funzionalità di riconoscimento del pool di connessioni.<br /><br /> Il pool di connessioni viene abilitato chiamando **SQLSetEnvAttr** per impostare l'attributo SQL_ATTR_CONNECTION_POOLING su SQL_CP_ONE_PER_DRIVER o SQL_CP_ONE_PER_HENV. Questa chiamata deve essere eseguita prima che l'applicazione allochi l'ambiente condiviso per il quale deve essere abilitato il pool di connessioni. L'handle di ambiente nella chiamata a **SQLSetEnvAttr** è impostato su null, che rende SQL_ATTR_CONNECTION_POOLING un attributo a livello di processo. Quando il pool di connessioni è abilitato, l'applicazione alloca un ambiente condiviso implicito chiamando **SQLAllocHandle** con l'argomento *inputhandle puntare* impostato su SQL_HANDLE_ENV.<br /><br /> Dopo che il pool di connessioni è stato abilitato ed è stato selezionato un ambiente condiviso per un'applicazione, non è possibile reimpostare SQL_ATTR_CONNECTION_POOLING per tale ambiente, perché **SQLSetEnvAttr** viene chiamato con un handle di ambiente null quando si imposta questo attributo. Se questo attributo è impostato quando il pool di connessioni è già abilitato in un ambiente condiviso, l'attributo influirà solo sugli ambienti condivisi che vengono allocati successivamente.<br /><br /> È anche possibile abilitare il pool di connessioni in un ambiente. Tenere presente quanto segue per il pool di connessioni di ambiente:<br /><br /> -L'abilitazione del pool di connessioni su un handle NULL è un attributo a livello di processo. Gli ambienti allocati successivamente saranno un ambiente condiviso e erediteranno l'impostazione del pool di connessioni a livello di processo.<br />-Dopo l'allocazione di un ambiente, un'applicazione può comunque modificare le impostazioni del pool di connessioni.<br />-Se il pool di connessioni dell'ambiente è abilitato e il driver della connessione usa il pooling dei driver, il pool di ambienti è preferenziale.<br /><br /> SQL_ATTR_CONNECTION_POOLING viene implementato all'interno di gestione driver. Non è necessario che un driver implementi SQL_ATTR_CONNECTION_POOLING. Le applicazioni ODBC 2,0 e 3,0 possono impostare questo attributo Environment.<br /><br /> Per altre informazioni, vedere la pagina relativa ai [pool di connessioni ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_CP_MATCH (ODBC 3,0)|Valore SQLUINTEGER a 32 bit che determina il modo in cui viene scelta una connessione da un pool di connessioni. Quando viene chiamato **SQLConnect** o **SQLDriverConnect** , gestione driver determina quale connessione viene riutilizzata dal pool. Gestione driver tenta di far corrispondere le opzioni di connessione nella chiamata e gli attributi di connessione impostati dall'applicazione alle parole chiave e agli attributi di connessione delle connessioni nel pool. Il valore di questo attributo determina il livello di precisione dei criteri di corrispondenza.<br /><br /> Per impostare il valore di questo attributo vengono usati i valori seguenti:<br /><br /> SQL_CP_STRICT_MATCH = solo le connessioni che corrispondono esattamente alle opzioni di connessione nella chiamata e gli attributi di connessione impostati dall'applicazione vengono riutilizzati. Questo è il valore predefinito.<br /><br /> SQL_CP_RELAXED_MATCH = è possibile utilizzare le connessioni con parole chiave corrispondenti della stringa di connessione. Le parole chiave devono corrispondere, ma non tutti gli attributi di connessione devono corrispondere.<br /><br /> Per ulteriori informazioni su come Gestione driver esegue la corrispondenza nella connessione a una connessione in pool, vedere [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Per ulteriori informazioni sul pool di connessioni, vedere la pagina relativa al [pool di connessioni ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_ODBC_VERSION (ODBC 3,0)|Intero A 32 bit che determina se determinate funzionalità mostrano il comportamento di ODBC *2. x* o il comportamento di ODBC *3. x* . Per impostare il valore di questo attributo vengono usati i valori seguenti:<br /><br /> SQL_OV_ODBC3_80 = Gestione driver e driver presentano il seguente comportamento di ODBC 3,8:<br /><br /> -Il driver restituisce e prevede codici ODBC *3. x* per data, ora e timestamp.<br />-Il driver restituisce i codici SQLSTATE ODBC *3. x* quando viene chiamato **SQLError**, **SQLGetDiagField** o **SQLGetDiagRec** .<br />-L'argomento *CatalogName* in una chiamata a **SQLTables** accetta un criterio di ricerca.<br />-Gestione driver supporta l'estensibilità del tipo di dati C. Per ulteriori informazioni sull'estensibilità del tipo di dati C, vedere [tipi di dati c in ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).<br /><br /> Per ulteriori informazioni, vedere Novità [di ODBC 3,8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).<br /><br /> SQL_OV_ODBC3 = Gestione driver e driver presentano il seguente comportamento ODBC *3. x* :<br /><br /> -Il driver restituisce e prevede codici ODBC *3. x* per data, ora e timestamp.<br />-Il driver restituisce i codici SQLSTATE ODBC *3. x* quando viene chiamato **SQLError**, **SQLGetDiagField** o **SQLGetDiagRec** .<br />-L'argomento *CatalogName* in una chiamata a **SQLTables** accetta un criterio di ricerca.<br />-Gestione driver non supporta l'estensibilità del tipo di dati C.<br /><br /> SQL_OV_ODBC2 = Gestione driver e driver presentano il seguente comportamento di ODBC *2. x* . Questa operazione è particolarmente utile per un'applicazione ODBC *2. x* che utilizza un driver ODBC *3. x* .<br /><br /> -Il driver restituisce e prevede codici ODBC *2. x* per data, ora e timestamp.<br />-Il driver restituisce i codici SQLSTATE ODBC *2. x* quando viene chiamato **SQLError**, **SQLGetDiagField** o **SQLGetDiagRec** .<br />-L'argomento *CatalogName* in una chiamata a **SQLTables** non accetta un criterio di ricerca.<br />-Gestione driver non supporta l'estensibilità del tipo di dati C.<br /><br /> Un'applicazione deve impostare questo attributo dell'ambiente prima di chiamare qualsiasi funzione con un argomento SQLHENV oppure la chiamata restituirà SQLSTATE HY010 (errore della sequenza di funzioni). Indica se esiste un comportamento aggiuntivo per questi flag ambientali.<br /><br /> -Per altre informazioni, vedere [dichiarazione delle modifiche della versione e del comportamento di ODBC dell'applicazione](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md) . [](../../../odbc/reference/develop-app/behavioral-changes.md)|  
|SQL_ATTR_OUTPUT_NTS (ODBC 3,0)|Intero A 32 bit che determina il modo in cui il driver restituisce i dati stringa. Se SQL_TRUE, il driver restituisce i dati stringa con terminazione null. Se SQL_FALSE, il driver non restituisce i dati stringa con terminazione null.<br /><br /> Per impostazione predefinita, questo attributo viene SQL_TRUE. Una chiamata a **SQLSetEnvAttr** per impostarla su SQL_TRUE restituisce SQL_SUCCESS. Una chiamata a **SQLSetEnvAttr** per impostarla su SQL_FALSE restituisce SQL_ERROR e SQLSTATE HYC00 (funzionalità facoltativa non implementata).|  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Restituzione dell'impostazione di un attributo Environment|[Funzione SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Novità di ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md)
