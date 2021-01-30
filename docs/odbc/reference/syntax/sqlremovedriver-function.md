---
description: Funzione SQLRemoveDriver
title: Funzione SQLRemoveDriver | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDriver
helpviewer_keywords:
- SQLRemoveDriver function [ODBC]
ms.assetid: 9a3b4f8b-982b-44b9-ade6-754ff026dc90
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ee0165f815b874844f072c36325fb7afb8046c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192505"
---
# <a name="sqlremovedriver-function"></a>Funzione SQLRemoveDriver
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLRemoveDriver** modifica o rimuove le informazioni relative al driver dalla voce Odbcinst.ini nelle informazioni di sistema.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLRemoveDriver(  
     LPCSTR   lpszDriver,  
     BOOL     fRemoveDSN,  
     LPDWORD  lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszDriver*  
 Input Nome del driver registrato nella chiave Odbcinst.ini delle informazioni sul sistema.  
  
 *fRemoveDSN*  
 Input I valori validi sono:  
  
 TRUE: rimuove i DSN associati al driver specificato in *lpszDriver*. FALSE: non rimuovere i DSN associati al driver specificato in *lpszDriver*.  
  
 *lpdwUsageCount*  
 Output Conteggio di utilizzo del driver dopo la chiamata a questa funzione.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo. Se nelle informazioni di sistema non è presente alcuna voce quando viene chiamata questa funzione, la funzione restituisce FALSE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLRemoveDriver** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Componente non trovato nel registro di sistema|Il programma di installazione non è riuscito a rimuovere le informazioni sul driver perché non esisteva nel registro di sistema o non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszDriver* non è valido.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Non è stato possibile incrementare o decrementare il numero di utilizzi dei componenti|Il programma di installazione non è riuscito a decrementare il conteggio di utilizzo del driver.|  
|ODBC_ERROR_REQUEST_FAILED|Richiesta non riuscita|L'argomento *fRemoveDSN* è true. Tuttavia, non è stato possibile rimuovere uno o più DSN. La chiamata a **SQLConfigDriver** con la richiesta ODBC_REMOVE_DRIVER non è riuscita.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLRemoveDriver** completa la funzione [SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md) e aggiorna il conteggio di utilizzo dei componenti nelle informazioni di sistema. Questa funzione deve essere chiamata solo da un'applicazione di installazione.  
  
 **SQLRemoveDriver** decrementerà di 1 il valore del numero di utilizzo dei componenti. Se il conteggio utilizzo componenti passa a 0, si verificherà quanto segue:  
  
1.  Verrà chiamata la funzione **SQLConfigDriver** con l'opzione ODBC_REMOVE_DRIVER. Se l'opzione *fRemoveDSN* è impostata su true, la funzione **ConfigDSN** chiama **SQLRemoveDSNFromIni** per rimuovere tutte le origini dati associate al driver specificato in *lpszDriver.* Se l'opzione *fRemoveDSN* è impostata su false, le origini dati non verranno eliminate.  
  
2.  La voce relativa al driver nelle informazioni sul sistema verrà rimossa. La voce del driver si trova nel seguente percorso di informazioni di sistema, sotto il nome del driver:  
  
     `HKEY_LOCAL_MACHINE`  
  
     `SOFTWARE`  
  
     `ODBC`  
  
     `Odbcinst.ini`  
  
 **SQLRemoveDriver** non rimuove effettivamente alcun file. Il programma chiamante è responsabile dell'eliminazione dei file e della gestione del numero di utilizzo del file. Solo dopo che il conteggio di utilizzo del componente e il conteggio di utilizzo file sono stati raggiunti zero è un file eliminato fisicamente. È possibile eliminare alcuni file in un componente e non eliminarli, a seconda che i file vengano utilizzati da altre applicazioni che hanno incrementato il numero di utilizzo del file.  
  
 **SQLRemoveDriver** viene chiamato anche come parte di un processo di aggiornamento. Se un'applicazione rileva che è necessario eseguire un aggiornamento e in precedenza ha installato il driver, il driver deve essere rimosso e quindi reinstallato. **SQLRemoveDriver** deve essere chiamato prima per decrementare il numero di utilizzo del componente, quindi è necessario chiamare **SQLInstallDriverEx** per incrementare il conteggio di utilizzo dei componenti. Il programma di installazione dell'applicazione deve sostituire i file obsoleti con i nuovi file. Il conteggio di utilizzo file rimarrà invariato e altre applicazioni che utilizzano i file della versione precedente utilizzeranno ora la versione più recente.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un driver|[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md) (nella DLL di installazione)|  
|Aggiunta, modifica o rimozione di un driver|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Installazione di un driver|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|
