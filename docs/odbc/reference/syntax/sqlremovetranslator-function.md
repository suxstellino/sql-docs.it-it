---
description: Funzione SQLRemoveTranslator
title: Funzione SQLRemoveTranslator | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveTranslator
helpviewer_keywords:
- SQLRemoveTranslator function [ODBC]
ms.assetid: c6feda49-0359-4224-8de9-77125cf2397b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cc8d1c7340d548276f462512bf2b52a3deb37b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192476"
---
# <a name="sqlremovetranslator-function"></a>Funzione SQLRemoveTranslator
**Conformità**  
 Versione introdotta: ODBC 3,0  
  
 **Summary**  
 **SQLRemoveTranslator** rimuove le informazioni su un convertitore dalla sezione Odbcinst.ini delle informazioni sul sistema e decrementa di 1 il numero di utilizzo dei componenti del traduttore.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLRemoveTranslator(  
     LPCSTR    lpszTranslator,  
     LPDWORD   lpdwUsageCount);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszTranslator*  
 Input Nome del convertitore registrato nella chiave Odbcinst.ini delle informazioni sul sistema.  
  
 *lpdwUsageCount*  
 Output Conteggio di utilizzo del traduttore dopo la chiamata a questa funzione.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo. Se nelle informazioni di sistema non è presente alcuna voce quando viene chiamata questa funzione, la funzione restituisce FALSE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLRemoveTranslator** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|Componente non trovato nel registro di sistema|Il programma di installazione non è riuscito a rimuovere le informazioni del traduttore perché non esisteva nel registro di sistema o non è stato trovato nel registro di sistema.|  
|ODBC_ERROR_INVALID_NAME|Nome del driver o del traduttore non valido|L'argomento *lpszTranslator* non è valido.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Non è stato possibile incrementare o decrementare il numero di utilizzi dei componenti|Il programma di installazione non è riuscito a decrementare il conteggio di utilizzo del driver.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLRemoveTranslator** completa la funzione [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md) e aggiorna il conteggio di utilizzo dei componenti nelle informazioni di sistema. Questa funzione deve essere chiamata solo da un'applicazione di installazione.  
  
 **SQLRemoveTranslator** decrementerà il numero di utilizzo del componente di 1. Se il conteggio utilizzo componenti passa a 0, la voce di traduzione nelle informazioni sul sistema verrà rimossa. La voce Translator si trova nel percorso seguente nelle informazioni di sistema, sotto il nome del traduttore:  
  
 `HKEY_LOCAL_MACHINE`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbcinst.ini`  
  
 **SQLRemoveTranslator** non rimuove effettivamente alcun file. Il programma chiamante è responsabile dell'eliminazione dei file e della gestione del numero di utilizzo del file. Solo dopo che il conteggio di utilizzo del componente e il conteggio di utilizzo file sono stati raggiunti zero è un file eliminato fisicamente. È possibile eliminare alcuni file in un componente e non eliminarli, a seconda che i file vengano utilizzati da altre applicazioni che hanno incrementato il numero di utilizzo del file.  
  
 **SQLRemoveTranslator** viene chiamato anche come parte di un processo di aggiornamento. Se un'applicazione rileva che è necessario eseguire un aggiornamento e in precedenza ha installato il driver, il driver deve essere rimosso e quindi reinstallato. **SQLRemoveTranslator** deve essere chiamato prima per decrementare il numero di utilizzo del componente, quindi è necessario chiamare **SQLInstallTranslatorEx** per incrementare il conteggio di utilizzo dei componenti. Il programma di installazione dell'applicazione deve sostituire fisicamente i file obsoleti con i nuovi file. Il conteggio di utilizzo file rimarrà invariato e altre applicazioni che utilizzano i file della versione precedente utilizzeranno ora la versione più recente.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Installazione di un convertitore|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|
