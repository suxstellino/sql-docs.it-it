---
title: Funzione SQLDriverConnect | Microsoft Docs
description: La funzione SQLDriverConnect fa parte dello standard API ODBC e questa documentazione di riferimento fornisce informazioni sulla relativa sintassi.
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverConnect
helpviewer_keywords:
- SQLDriverConnect function [ODBC]
ms.assetid: e299be1d-5c74-4ede-b6a3-430eb189134f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58c9f2071825c6c7d8f873f300e8e5c3f46af9b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195640"
---
# <a name="sqldriverconnect-function"></a>SQLDriverConnect Function
**Conformità**  
 Versione introdotta: ODBC 1,0 Standard Compliance: ODBC  
  
 **Summary**  
 **SQLDriverConnect** è un'alternativa a **SQLConnect**. Supporta le origini dati che richiedono più informazioni di connessione rispetto ai tre argomenti di **SQLConnect**, le finestre di dialogo per richiedere all'utente tutte le informazioni di connessione e le origini dati che non sono definite nelle informazioni di sistema. Per ulteriori informazioni, vedere [connessione con SQLDriverConnect](../develop-app/connecting-with-sqldriverconnect.md).  

## <a name="syntax"></a>Sintassi  
  
```cpp  
  
SQLRETURN SQLDriverConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLHWND         WindowHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr,  
     SQLUSMALLINT    DriverCompletion);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ConnectionHandle*  
 [Input] Handle di connessione.  
  
 *WindowHandle*  
 Input Handle della finestra. L'applicazione può passare l'handle della finestra padre, se applicabile, o un puntatore null se l'handle della finestra non è applicabile o **SQLDriverConnect** non presenterà finestre di dialogo.  
  
 *InConnectionString*  
 Input Una stringa di connessione completa (vedere la sintassi in "comments"), una stringa di connessione parziale o una stringa vuota.  
  
 *StringLength1*  
 Input Lunghezza di **InConnectionString*, in caratteri se la stringa è Unicode, o byte se la stringa è ANSI o DBCS.  
  
 *OutConnectionString*  
 Output Puntatore a un buffer per la stringa di connessione completata. Al completamento della connessione all'origine dati di destinazione, il buffer contiene la stringa di connessione completata. Le applicazioni devono allocare almeno 1.024 caratteri per questo buffer.  
  
 Se *OutConnectionString* è null, *StringLength2Ptr* restituirà comunque il numero totale di caratteri, escluso il carattere di terminazione null per i dati di tipo carattere, disponibile per restituire nel buffer a cui punta *OutConnectionString*.  
  
 *BufferLength*  
 Input Lunghezza del buffer **OutConnectionString* , in caratteri.  
  
 *StringLength2Ptr*  
 Output Puntatore a un buffer in cui restituire il numero totale di caratteri, escluso il carattere di terminazione null, disponibile per restituire in \* *OutConnectionString*. Se il numero di caratteri disponibili per restituire è maggiore o uguale a *bufferLength*, la stringa di connessione completata in \* *OutConnectionString* viene troncata a *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
 *DriverCompletion*  
 Input Flag che indica se il driver o Gestione driver deve richiedere ulteriori informazioni di connessione:  
  
 SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE, SQL_DRIVER_COMPLETE_REQUIRED o SQL_DRIVER_NOPROMPT.  
  
 Per ulteriori informazioni, vedere "Commenti".  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLDriverConnect** restituisce SQL_ERROR o SQL_SUCCESS_WITH_INFO, è possibile ottenere un valore SQLSTATE associato chiamando **SQLGetDiagRec** con un *FHandleType* di SQL_HANDLE_DBC e un *hHandle* di *connectionHandle*. La tabella seguente elenca i valori SQLSTATE restituiti comunemente da **SQLDriverConnect** e ne illustra ognuno nel contesto di questa funzione; la notazione "(DM)" precede le descrizioni di SQLSTATE restituite da Gestione driver. Il codice restituito associato a ogni valore SQLSTATE è SQL_ERROR, a meno che non sia specificato diversamente.  
  
|SQLSTATE|Errore|Descrizione|  
|--------------|-----------|-----------------|  
|01000|Avviso generale|Messaggio informativo specifico del driver. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01004|Dati stringa, troncati a destra|L' \* *OutConnectionString* del buffer non era sufficientemente grande da restituire l'intera stringa di connessione, quindi la stringa di connessione è stata troncata. La lunghezza della stringa di connessione non troncata viene restituita in **StringLength2Ptr*. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S00|Attributo della stringa di connessione non valido|È stata specificata una parola chiave attribute non valida nella stringa di connessione (*InConnectionString*), ma il driver è stato comunque in grado di connettersi all'origine dati. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S02|Valore di opzione modificato|Il driver non supportava il valore specificato a cui punta l'argomento *ValuePtr* in **SQLSetConnectAttr** e sostituisce un valore simile. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S08|Errore durante il salvataggio del DSN del file|La stringa in *\* InConnectionString* contiene una parola chiave **FileDSN** , ma il file con estensione DSN non è stato salvato. (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|01S09|Parola chiave non valida|(DM) la stringa in *\* InConnectionString* contiene una parola chiave **SaveFile** , ma non un **driver** o una parola chiave **FileDSN** . (La funzione restituisce SQL_SUCCESS_WITH_INFO.)|  
|08001|Il client non è in grado di stabilire la connessione|Il driver non è stato in grado di stabilire una connessione con l'origine dati.|  
|08002|Nome della connessione in uso|(DM) il *connectionHandle* specificato è già stato utilizzato per stabilire una connessione con un'origine dati e la connessione è ancora aperta.|  
|08004|Il server ha rifiutato la connessione|L'origine dati ha rifiutato la creazione della connessione per motivi definiti dall'implementazione.|  
|08S01|Errore collegamento comunicazione|Il collegamento di comunicazione tra il driver e l'origine dati a cui è stato effettuato il tentativo di connessione del driver non è riuscito prima dell'elaborazione della funzione **SQLDriverConnect** .|  
|28000|Specifica di autorizzazione non valida|L'identificatore utente o la stringa di autorizzazione, o entrambi, come specificato nella stringa di connessione (*InConnectionString*), hanno violato le restrizioni definite dall'origine dati.|  
|HY000|Errore generale:|Si è verificato un errore per il quale non esiste un valore SQLSTATE specifico e per il quale non è stato definito alcun valore SQLSTATE specifico dell'implementazione. Il messaggio di errore restituito da **SQLGetDiagRec** nel buffer *\* szMessageText* descrive l'errore e la sua origine.|  
|HY000|Errore generale: DSN del file non valido|(DM) la stringa in **InConnectionString* contiene una parola chiave FILEDSN, ma il nome del file con estensione DSN non è stato trovato.|  
|HY000|Errore generale: Impossibile creare il buffer di file|(DM) la stringa in **InConnectionString* contiene una parola chiave FILEDSN, ma il file con estensione DSN non è leggibile.|  
|HY001|Errore di allocazione della memoria|Gestione driver non è in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione **SQLDriverConnect** .<br /><br /> Il driver non è stato in grado di allocare memoria necessaria per supportare l'esecuzione o il completamento della funzione.|  
|HY008|Operation canceled|L'elaborazione asincrona è stata abilitata per *connectionHandle*. La funzione è stata chiamata e prima del completamento dell'esecuzione è stata chiamata la [funzione SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) su *connectionHandle* e quindi la funzione **SQLDriverConnect** è stata chiamata nuovamente sul *connectionHandle*.<br /><br /> In alternativa, la funzione **SQLDriverConnect** è stata chiamata e prima del completamento dell'esecuzione, **SQLCancelHandle** è stato chiamato su *connectionHandle* da un thread diverso in un'applicazione multithread.|  
|HY010|Errore sequenza funzione|(DM) un'altra funzione in esecuzione asincrona (non **SQLDriverConnect**) è stata chiamata per *connectionHandle* ed è stata ancora eseguita quando è stata chiamata la funzione **SQLDriverConnect** .|  
|HY013|Errore di gestione della memoria|Impossibile elaborare la chiamata di funzione **SQLDriverConnect** perché non è possibile accedere agli oggetti di memoria sottostanti, probabilmente a causa di condizioni di memoria insufficiente.|  
|HY090|Lunghezza della stringa o del buffer non valida|(DM) il valore specificato per l'argomento *StringLength1* è minore di 0 e non è uguale a SQL_NTS.<br /><br /> (DM) il valore specificato per l'argomento *bufferLength* è minore di 0.|  
|HY092|Identificatore di attributo/opzione non valido|(DM) l'argomento *DriverCompletion* è stato SQL_DRIVER_PROMPT e l'argomento *WindowHandle* è un puntatore null.|  
|HY110|Completamento driver non valido|(DM) il valore specificato per l'argomento *DriverCompletion* non è uguale a SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE, SQL_DRIVER_COMPLETE_REQUIRED o SQL_DRIVER_NOPROMPT.<br /><br /> (DM) il pool di connessioni è stato abilitato e il valore specificato per l'argomento *DriverCompletion* non era uguale SQL_DRIVER_NOPROMPT.|  
|HYC00|Funzionalità facoltativa non implementata|Il driver non supporta la versione del comportamento ODBC richiesta dall'applicazione.|  
|HYT00|Timeout|Il periodo di timeout di accesso è scaduto prima del completamento della connessione all'origine dati. Il periodo di timeout viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Timeout connessione scaduto|Il periodo di timeout della connessione è scaduto prima che l'origine dati abbia risposto alla richiesta. Il periodo di timeout della connessione viene impostato tramite **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Il driver non supporta questa funzione|(DM) il driver corrispondente al nome dell'origine dati specificato non supporta la funzione.|  
|IM002|Origine dati non trovata e non è stato specificato alcun driver predefinito|(DM) il nome dell'origine dati specificato nella stringa di connessione (*InConnectionString*) non è stato trovato nelle informazioni di sistema e non esiste alcuna specifica del driver predefinita.<br /><br /> (DM) non è stato possibile trovare le informazioni relative all'origine dati ODBC e al driver predefinito nelle informazioni di sistema.|  
|IM003|Non è stato possibile caricare il driver specificato|(DM) il driver elencato nella specifica dell'origine dati nelle informazioni di sistema o specificato dalla parola chiave del **driver** non è stato trovato o non è stato possibile caricarlo per altri motivi.|  
|IM004|**SQLAllocHandle** del Driver su SQL_HANDLE_ENV non riuscito|(DM) durante **SQLDriverConnect**, gestione driver ha chiamato la funzione **SQLAllocHandle** del driver con un *fHandleType* di SQL_HANDLE_ENV e il driver ha restituito un errore.|  
|IM005|**SQLAllocHandle** del Driver su SQL_HANDLE_DBC non riuscito.|(DM) durante **SQLDriverConnect**, gestione driver ha chiamato la funzione **SQLAllocHandle** del driver con un *fHandleType* di SQL_HANDLE_DBC e il driver ha restituito un errore.|  
|IM006|Errore di **SQLSetConnectAttr** del driver|(DM) durante **SQLDriverConnect**, gestione driver ha chiamato la funzione **SQLSetConnectAttr** del driver e il driver ha restituito un errore.|  
|IM007|Non è stata specificata alcuna origine dati o driver; dialogo non consentito|Nella stringa di connessione non è stato specificato alcun nome o driver dell'origine dati e *DriverCompletion* è stato SQL_DRIVER_NOPROMPT.|  
|IM008|Finestra di dialogo non riuscita|Il driver ha tentato di visualizzare la finestra di dialogo di accesso e ha avuto esito negativo.<br /><br /> *WindowHandle* è un puntatore null e *DriverCompletion* non è stato SQL_DRIVER_NO_PROMPT.|  
|IM009|Impossibile caricare la DLL di traduzione|Il driver non è stato in grado di caricare la DLL di conversione specificata per l'origine dati o per la connessione.|  
|IM010|Nome origine dati troppo lungo|(DM) il valore dell'attributo per la parola chiave DSN è più lungo di SQL_MAX_DSN_LENGTH caratteri.|  
|IM011|Nome driver troppo lungo|(DM) il valore dell'attributo per la parola chiave **driver** è più lungo di 255 caratteri.|  
|IM012|Errore di sintassi della parola chiave DRIVER|(DM) la coppia parola chiave-valore per la parola chiave **driver** contiene un errore di sintassi.<br /><br /> (DM) la stringa in *\* InConnectionString* contiene una parola chiave **FileDSN** , ma il file con estensione DSN non contiene una parola chiave del **driver** o una parola chiave **DSN** .|  
|IM014|Il DSN specificato contiene una mancata corrispondenza dell'architettura tra il driver e l'applicazione|(DM) l'applicazione a 32 bit utilizza un DSN che si connette a un driver a 64 bit. o viceversa.|  
|IM015|SQLDriverConnect del driver su SQL_HANDLE_DBC_INFO_HANDLE non riuscito|Se un driver restituisce SQL_ERROR, gestione driver restituirà SQL_ERROR all'applicazione e la connessione avrà esito negativo.<br /><br /> Per ulteriori informazioni su SQL_HANDLE_DBC_INFO_TOKEN, vedere [sviluppo di Connection-Pool awareness in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).|  
|IM017|Polling disabilitato in modalità di notifica asincrona|Ogni volta che viene utilizzato il modello di notifica, il polling è disabilitato.|  
|IM018|**SQLCompleteAsync** non è stato chiamato per completare l'operazione asincrona precedente su questo handle.|Se la chiamata di funzione precedente nell'handle restituisce SQL_STILL_EXECUTING e se è abilitata la modalità di notifica, è necessario chiamare **SQLCompleteAsync** sull'handle per eseguire la post-elaborazione e completare l'operazione.|  
|S1118|Il driver non supporta la notifica asincrona|Quando il driver non supporta la notifica asincrona, non è possibile impostare SQL_ATTR_ASYNC_DBC_EVENT o SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="comments"></a>Commenti  
 Una stringa di connessione presenta la sintassi seguente:  
  
 *Connection-String* :: = *Empty-String*[;] &#124; *attributo*[;] &#124; *attributo*; *stringa di connessione*  
  
 *Empty-String* :: =*attribute* :: = Attribute *-keyword*- = *value* &#124; driver = [{]*attributo-value*[}]  
  
 *attribute-keyword* :: = DSN &#124; UID &#124; pwd &#124; *driver-defined-attribute-keyword*  
  
 *attribute-value* :: = *stringa di caratteri*  
  
 *driver-defined-attribute-keyword* :: = *Identifier*  
  
 dove la *stringa di caratteri* contiene zero o più caratteri; l' *identificatore* contiene uno o più caratteri; *attribute-keyword* non distingue tra maiuscole e minuscole. *attribute-value* può fare distinzione tra maiuscole e minuscole; il valore della parola chiave **DSN** , quindi, non è costituito solo da spazi vuoti.  
  
 A causa della sintassi della stringa di connessione e del file di inizializzazione, parole chiave e valori di attributo contenenti i caratteri **[] {} (),;? \* =! @** non racchiuso tra parentesi graffe da evitare. Il valore della parola chiave **DSN** non può essere costituito solo da spazi vuoti e non deve contenere spazi vuoti iniziali. A causa della grammatica delle informazioni di sistema, le parole chiave e i nomi delle origini dati non possono contenere il carattere barra rovesciata ( \\ ).  
  
 Le applicazioni non devono aggiungere parentesi graffe intorno al valore dell'attributo dopo la parola chiave del **driver** , a meno che l'attributo non contenga un punto e virgola (;), nel qual caso le parentesi graffe sono obbligatorie. Se il valore dell'attributo ricevuto dal driver include le parentesi graffe, il driver non deve rimuoverle, ma deve essere parte della stringa di connessione restituita.  
  
 Un DSN o un valore della stringa di connessione racchiuso tra parentesi graffe ( {} ) contenente uno dei caratteri **[] {} (),;? \* =! @** viene passato intatto al driver. Tuttavia, quando si utilizzano questi caratteri in una parola chiave, gestione driver restituisce un errore quando si utilizzano i DSN del file ma passa la stringa di connessione al driver per le normali stringhe di connessione. Evitare l'utilizzo di parentesi graffe incorporate in un valore di parola chiave.  
  
 La stringa di connessione può includere un numero qualsiasi di parole chiave definite dal driver. Poiché la parola chiave **driver** non utilizza informazioni delle informazioni di sistema, il driver deve definire parole chiave sufficienti in modo che un driver possa connettersi a un'origine dati utilizzando solo le informazioni nella stringa di connessione. Per ulteriori informazioni, vedere "linee guida per i driver" più avanti in questa sezione. Il driver definisce quali parole chiave sono necessarie per la connessione all'origine dati.  
  
 Nella tabella seguente vengono descritti i valori di attributo delle parole chiave **DSN**, **FileDSN**, **driver**, **UID**, **pwd** e **SaveFile** .  
  
|Parola chiave|Descrizione del valore dell'attributo|  
|-------------|---------------------------------|  
|**DSN**|Nome di un'origine dati restituita da **SQLDataSources** o dalla finestra di dialogo origini dati di **SQLDriverConnect**.|  
|**FILEDSN**|Nome di un file con estensione DSN dal quale verrà compilata una stringa di connessione per l'origine dati. Queste origini dati sono denominate origini dati di file.|  
|**DRIVER**|Descrizione del driver restituita dalla funzione **SQLDrivers** . Ad esempio RDB o SQL Server.|  
|**UID**|ID utente.|  
|**PWD**|Password corrispondente all'ID utente o una stringa vuota se non è presente alcuna password per l'ID utente (PWD =;).|  
|**SAVEFILE**|Il nome file di un file con estensione DSN in cui devono essere salvati i valori di attributo delle parole chiave utilizzate per rendere la connessione corretta.|  
  
 Per informazioni sul modo in cui un'applicazione sceglie un'origine dati o un driver, vedere [scelta di un'origine dati o](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)di un driver.  
  
 Se vengono ripetute parole chiave nella stringa di connessione, il driver utilizzerà il valore associato alla prima occorrenza della parola chiave. Se le parole chiave **DSN** e **driver** sono incluse nella stessa stringa di connessione, gestione driver e il driver utilizzeranno prima di tutto la parola chiave.  
  
 Le parole chiave **FileDSN** e **DSN** si escludono a vicenda: qualsiasi parola chiave visualizzata per prima viene usata e quella visualizzata come seconda viene ignorata. Le parole chiave **FileDSN** e **driver** , d'altra parte, non si escludono a vicenda. Se una parola chiave viene visualizzata in una stringa di connessione con **FileDSN**, viene utilizzato il valore dell'attributo della parola chiave nella stringa di connessione, anziché il valore dell'attributo della stessa parola chiave nel file con estensione DSN.  
  
 Se viene utilizzata la parola chiave **FileDSN** , per creare una stringa di connessione vengono utilizzate le parole chiave specificate in un file con estensione DSN. Per ulteriori informazioni, vedere "origini dati file" di seguito in questa sezione. La parola chiave **UID** è facoltativa. è possibile creare un file con estensione DSN solo con la parola chiave **driver** . La parola chiave **pwd** non è archiviata in un file con estensione DSN. La directory predefinita per il salvataggio e il caricamento di un file con estensione DSN sarà una combinazione del percorso specificato da **CommonFileDir** in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ Windows\CurrentVersion e "ODBC\DataSources". Se CommonFileDir erano "C:\Programmi file comuni", la directory predefinita sarà "C:\Programmi Comuni\odbc\data Sources".  
  
> [!NOTE]  
>  Un file con estensione DSN può essere modificato direttamente chiamando le funzioni [SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md) e [SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md) nella dll del programma di installazione.  
  
 Se viene utilizzata la parola chiave **SaveFile** , i valori di attributo delle parole chiave utilizzate per rendere la connessione riuscita verranno salvati come file con estensione DSN con il nome del valore dell'attributo della parola chiave **SaveFile** . È necessario usare la parola chiave **SaveFile** insieme alla parola chiave **driver** , alla parola chiave **FileDSN** o a entrambe le funzioni oppure la funzione restituisce SQL_SUCCESS_WITH_INFO con SQLSTATE 01S09 (parola chiave non valida). La parola chiave **SaveFile** deve trovarsi prima della parola chiave **driver** nella stringa di connessione oppure i risultati non saranno definiti.  
  
## <a name="driver-manager-guidelines"></a>Linee guida per Gestione driver  
 Gestione driver crea una stringa di connessione da passare al driver nell'argomento *InConnectionString* della funzione **SQLDriverConnect** del driver. Gestione driver non modifica l'argomento *InConnectionString* passato dall'applicazione.  
  
 L'azione di gestione driver si basa sul valore dell'argomento *DriverCompletion* :  
  
-   SQL_DRIVER_PROMPT: se la stringa di connessione non contiene la parola chiave **driver**, **DSN** o **FileDSN** , gestione driver Visualizza la finestra di dialogo origini dati. Costruisce una stringa di connessione dal nome dell'origine dati restituito dalla finestra di dialogo e da eventuali altre parole chiave passate dall'applicazione. Se il nome dell'origine dati restituito dalla finestra di dialogo è vuoto, gestione driver specifica la coppia parola chiave-valore DSN = valore predefinito. (In questa finestra di dialogo non verrà visualizzata un'origine dati con il nome "default").  
  
-   SQL_DRIVER_COMPLETE o SQL_DRIVER_COMPLETE_REQUIRED: se la stringa di connessione specificata dall'applicazione include la parola chiave **DSN** , gestione driver copia la stringa di connessione specificata dall'applicazione. In caso contrario, esegue le stesse operazioni eseguite quando *DriverCompletion* è SQL_DRIVER_PROMPT.  
  
-   SQL_DRIVER_NOPROMPT: Gestione driver copia la stringa di connessione specificata dall'applicazione.  
  
 Se la stringa di connessione specificata dall'applicazione contiene la parola chiave **driver** , gestione driver copia la stringa di connessione specificata dall'applicazione.  
  
 Utilizzando la stringa di connessione costruita, gestione driver determina quale driver utilizzare, si connette a tale driver e passa la stringa di connessione costruita al driver. Per ulteriori informazioni sull'interazione di gestione driver e del driver, vedere la sezione "Commenti" nella [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Se la stringa di connessione non contiene la parola chiave **driver** , gestione driver determina il driver da usare come indicato di seguito:  
  
1.  Se la stringa di connessione contiene la parola chiave **DSN** , gestione driver Recupera il driver associato all'origine dati dalle informazioni di sistema.  
  
2.  Se la stringa di connessione non contiene la parola chiave **DSN** o l'origine dati non viene trovata, gestione driver Recupera il driver associato all'origine dati predefinita dalle informazioni di sistema. Per ulteriori informazioni, vedere la [sottochiave default](../../../odbc/reference/install/default-subkey.md). Gestione driver modifica il valore della parola chiave **DSN** nella stringa di connessione in "default".  
  
3.  Se la parola chiave **DSN** nella stringa di connessione è impostata su "default", gestione driver Recupera il driver associato all'origine dati predefinita dalle informazioni di sistema.  
  
4.  Se l'origine dati non viene trovata e l'origine dati predefinita non viene trovata, gestione driver restituisce SQL_ERROR con SQLSTATE IM002 (origine dati non trovata e nessun driver predefinito specificato).  
  
## <a name="file-data-sources"></a>Origini dati di file  
 Se la stringa di connessione specificata dall'applicazione nella chiamata a **SQLDriverConnect** contiene la parola chiave **FileDSN** e questa parola chiave non è sostituita dalla parola chiave **DSN** o **driver** , gestione driver crea una stringa di connessione usando le informazioni contenute nel file con estensione DSN e l'argomento *InConnectionString* . Gestione driver procede come segue:  
  
1.  Verifica se il nome file del file con estensione DSN è valido. In caso contrario, restituisce SQL_ERROR con SQLSTATE IM014 (nome non valido del DSN di file). Se il nome del file è una stringa vuota ("") e SQL_DRIVER_NOPROMPT non è specificato, viene visualizzata la finestra di dialogo **Apri file** . Se il nome file contiene un percorso valido ma non un nome file o un nome file non valido e non SQL_DRIVER_NOPROMPT specificato, viene visualizzata la finestra di dialogo **Apri file** con la directory corrente impostata su quella specificata nel nome file. Se il nome del file è una stringa vuota ("") o il nome del file contiene un percorso valido ma non un nome file o un nome file non valido e viene specificato SQL_DRIVER_NOPROMPT, viene restituito SQL_ERROR con SQLSTATE IM014 (nome non valido del DSN di file).  
  
2.  Legge tutte le parole chiave nella sezione [ODBC] del file con estensione DSN. Se la parola chiave del **driver** non è presente, restituisce SQL_ERROR con SQLSTATE IM012 (errore di sintassi della parola chiave del driver), ad eccezione del caso in cui il file con estensione DSN è non condivisibile e pertanto contiene solo la parola chiave **DSN** .  
  
     Se l'origine dati del file è non condivisibile, gestione driver legge il valore della parola chiave **DSN** e si connette a seconda dell'origine dati di sistema o dell'utente a cui fa riferimento l'origine dati file non condivisibile. I passaggi da 3 a 5 non vengono eseguiti.  
  
3.  Costruisce una stringa di connessione per il driver. La stringa di connessione del driver è l'Unione delle parole chiave specificate nel file con estensione DSN e quelle specificate nella stringa di connessione dell'applicazione originale. Di seguito sono riportate le regole per la costruzione della stringa di connessione del driver in cui si sovrappongono le parole chiave  
  
    -   Se la parola chiave **driver** è presente nella stringa di connessione dell'applicazione e i driver specificati dalle parole chiave del **driver** non sono gli stessi nel file con estensione DSN e nella stringa di connessione dell'applicazione, le informazioni sul driver nel file con estensione DSN vengono ignorate e vengono usate le informazioni sul driver nella stringa di connessione dell'applicazione. Se i driver specificati dalla parola chiave **driver** sono gli stessi nel file con estensione DSN e nella stringa di connessione dell'applicazione, in cui tutte le parole chiave si sovrappongono, quelle specificate nella stringa di connessione dell'applicazione hanno la precedenza rispetto a quelle specificate nel file con estensione DSN.  
  
    -   Nella nuova stringa di connessione viene eliminata la parola chiave **FileDSN** .  
  
4.  Carica il driver cercando nella voce del registro di sistema HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI\\<nome del driver \> \Driver., dove \<Driver Name> viene specificato dalla parola chiave del **driver** .  
  
5.  Passa al driver la nuova stringa di connessione.  
  
 Per esempi di file con estensione DSN, vedere [Connecting using file Data Sources](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md).  
  
## <a name="savefile-keyword"></a>Parola chiave SAVEFILE  
 Se la stringa di connessione specificata dall'applicazione contiene la parola chiave **SaveFile** , gestione driver salva la stringa di connessione in un file con estensione DSN. Gestione driver procede come segue:  
  
1.  Verifica se il nome file del file con estensione DSN incluso come valore dell'attributo della parola chiave **SaveFile** è valido. In caso contrario, restituisce SQL_ERROR con SQLSTATE IM014 (nome non valido del DSN di file). La validità del nome file è determinata dalle regole di denominazione del sistema standard. Se il nome del file è una stringa vuota ("") e l'argomento *DriverCompletion* non è SQL_DRIVER_NOPROMPT, il nome del file è valido. Se il nome file esiste già, se *DriverCompletion* è SQL_DRIVER_NOPROMPT, il file verrà sovrascritto. Se *DriverCompletion* è SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE o SQL_DRIVER_COMPLETE_REQUIRED, viene visualizzata una finestra di dialogo in cui viene richiesto all'utente di specificare se il file deve essere sovrascritto. Se non viene immesso, viene visualizzata la finestra di dialogo **Salva file** .  
  
2.  Se il driver restituisce SQL_SUCCESS e il nome del file non è una stringa vuota, gestione driver scrive le informazioni di connessione restituite nell'argomento *OutConnectionString* nel file specificato con il formato specificato nella sezione "stringhe di connessione" riportata in precedenza in questa sezione.  
  
3.  Se il driver restituisce SQL_SUCCESS e il nome del file è una stringa vuota (""), gestione driver chiama la finestra di dialogo **File-Salva** comune con *HWND* specificato e scrive le informazioni di connessione restituite in *OutConnectionString* nel file specificato nella finestra di dialogo File-Save comune con il formato specificato nella sezione "stringhe di connessione" riportata in precedenza in questa sezione.  
  
4.  Se il driver restituisce SQL_SUCCESS, restituisce l'argomento *OutConnectionString* contenente la stringa di connessione all'applicazione.  
  
5.  Se il driver restituisce SQL_SUCCESS_WITH_INFO o SQL_ERROR, gestione driver restituisce l'identificativo SQLSTATE per l'applicazione.  
  
## <a name="driver-guidelines"></a>Linee guida per i driver  
 Il driver controlla se la stringa di connessione passata da Gestione driver contiene la parola chiave del **DSN** o del **driver** . Se la stringa di connessione contiene la parola chiave **driver** , il driver non è in grado di recuperare le informazioni sull'origine dati dalle informazioni di sistema. Se la stringa di connessione contiene la parola chiave **DSN** o non contiene il **DSN** o la parola chiave **driver** , il driver può recuperare le informazioni sull'origine dati dalle informazioni di sistema come indicato di seguito:  
  
1.  Se la stringa di connessione contiene la parola chiave **DSN** , il driver recupera le informazioni per l'origine dati specificata.  
  
2.  Se la stringa di connessione non contiene la parola chiave **DSN** , l'origine dati specificata non viene trovata o la parola chiave **DSN** è impostata su "default", il driver recupera le informazioni per l'origine dati predefinita.  
  
 Il driver utilizza le informazioni recuperate dalle informazioni di sistema per aumentare le informazioni passate nella stringa di connessione. Se le informazioni contenute nelle informazioni di sistema duplicano le informazioni nella stringa di connessione, il driver utilizzerà le informazioni contenute nella stringa di connessione.  
  
 In base al valore di *DriverCompletion*, il driver richiede all'utente le informazioni di connessione, ad esempio l'ID utente e la password, e si connette all'origine dati:  
  
-   SQL_DRIVER_PROMPT: il driver Visualizza una finestra di dialogo, usando i valori della stringa di connessione e le informazioni di sistema (se presenti) come valori iniziali. Quando l'utente esce dalla finestra di dialogo, il driver si connette all'origine dati. Costruisce anche una stringa di connessione dal valore della parola chiave del **DSN** o del **driver** in \* *InConnectionString* e dalle informazioni restituite dalla finestra di dialogo. Questa stringa di connessione viene inserita nel buffer **OutConnectionString* .  
  
-   SQL_DRIVER_COMPLETE o SQL_DRIVER_COMPLETE_REQUIRED: se la stringa di connessione contiene informazioni sufficienti e le informazioni sono corrette, il driver si connette all'origine dati e copia \* *InConnectionString* in \* *OutConnectionString*. Se le informazioni sono mancanti o errate, il driver esegue le stesse operazioni eseguite quando *DriverCompletion* è SQL_DRIVER_PROMPT, ad eccezione del fatto che se *DriverCompletion* è SQL_DRIVER_COMPLETE_REQUIRED, il driver Disabilita i controlli per tutte le informazioni non necessarie per la connessione all'origine dati.  
  
-   SQL_DRIVER_NOPROMPT: se la stringa di connessione contiene informazioni sufficienti, il driver si connette all'origine dati e copia \* *InConnectionString* in \* *OutConnectionString*. In caso contrario, il driver restituisce SQL_ERROR per **SQLDriverConnect**.  
  
 Al completamento della connessione all'origine dati, il driver imposta anche \* *StringLength2Ptr* sulla lunghezza della stringa di connessione di output disponibile per restituire in **OutConnectionString*.  
  
 Se l'utente annulla una finestra di dialogo presentata da Gestione driver o dal driver, **SQLDriverConnect** restituisce SQL_NO_DATA.  
  
 Per informazioni sul modo in cui gestione driver e il driver interagiscono durante il processo di connessione, vedere [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Se un driver supporta **SQLDriverConnect**, la sezione relativa alla parola chiave driver delle informazioni di sistema per il driver deve contenere la parola chiave **ConnectFunctions** con il secondo carattere impostato su "Y".  
  
## <a name="connecting-when-connection-pooling-is-enabled"></a>Connessione quando il pool di connessioni è abilitato  
 Il pool di connessioni consente a un'applicazione di riutilizzare una connessione già creata. Quando viene chiamato **SQLDriverConnect** , gestione driver tenta di stabilire la connessione utilizzando una connessione che fa parte di un pool di connessioni in un ambiente designato per il pool di connessioni. Per ulteriori informazioni sul pool di connessioni, vedere [funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Un'applicazione può impostare SQL_ATTR_RESET_CONNECTION prima di chiamare SQLConnect in una connessione in cui il pool è abilitato. Per ulteriori informazioni, vedere [funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
 Quando un'applicazione chiama **SQLDriverConnect** per connettersi a una connessione in pool, si applicano le restrizioni seguenti:  
  
-   Non viene eseguita alcuna elaborazione del pool di connessioni quando nella stringa di connessione viene specificata la parola chiave **SaveFile** .  
  
-   Se il pool di connessioni è abilitato, è possibile chiamare **SQLDriverConnect** solo con un argomento *DriverCompletion* di SQL_DRIVER_NOPROMPT; Se **SQLDriverConnect** viene chiamato con qualsiasi altro *DriverCompletion*, viene restituito SQLSTATE HY110 (completamento del driver non valido).  
  
## <a name="connection-attributes"></a>Attributi di connessione  
 L'attributo di connessione SQL_ATTR_LOGIN_TIMEOUT, impostato utilizzando **SQLSetConnectAttr**, definisce il numero di secondi di attesa per il completamento di una richiesta di accesso con una connessione riuscita da parte del driver prima di tornare all'applicazione. Se all'utente viene richiesto di completare la stringa di connessione, viene avviato un periodo di attesa per ogni richiesta di accesso quando il driver avvia il processo di connessione.  
  
 Per impostazione predefinita, il driver apre la connessione in modalità di accesso SQL_MODE_READ_WRITE. Per impostare la modalità di accesso su SQL_MODE_READ_ONLY, l'applicazione deve chiamare **SQLSetConnectAttr** con l'attributo SQL_ATTR_ACCESS_MODE prima di chiamare **SQLDriverConnect**.  
  
 Se nelle informazioni di sistema per l'origine dati è specificata una libreria di conversione predefinita, viene caricata dal driver. È possibile caricare una libreria di conversione diversa chiamando **SQLSetConnectAttr** con l'attributo SQL_ATTR_TRANSLATE_LIB. È possibile specificare un'opzione di conversione chiamando **SQLSetConnectAttr** con l'opzione SQL_ATTR_TRANSLATE_OPTION.  
  
 Per ulteriori informazioni, vedere [connessione con SQLDriverConnect](../../../odbc/reference/develop-app/connecting-with-sqldriverconnect.md).  
  
```cpp  
// SQLDriverConnect_ref.cpp  
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR OutConnStr[255];  
   SQLSMALLINT OutConnStrLen;  
  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            retcode = SQLDriverConnect( // SQL_NULL_HDBC  
               hdbc,   
               desktopHandle,   
               (SQLCHAR*)"driver=SQL Server",   
               _countof("driver=SQL Server"),  
               OutConnStr,  
               255,   
               &OutConnStrLen,  
               SQL_DRIVER_PROMPT );  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {                 
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
 Vedere anche il [programma ODBC di esempio](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Allocazione di un handle|[Funzione SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Individuazione ed enumerazione dei valori necessari per la connessione a un'origine dati|[Funzione SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connessione a un'origine dati|[Funzione SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Disconnessione da un'origine dati|[Funzione SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Restituzione di attributi e descrizioni dei driver|[Funzione SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|Liberare un handle|[SQLFreeHandle Function](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Impostazione di un attributo di connessione|[Pagina relativa alla funzione SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
