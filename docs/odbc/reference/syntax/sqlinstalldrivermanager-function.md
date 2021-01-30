---
description: Funzione SQLInstallDriverManager
title: Funzione SQLInstallDriverManager | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallDriverManager
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallDriverManager
helpviewer_keywords:
- SQLInstallDriverManager function [ODBC]
ms.assetid: aebc439b-fffd-4d98-907a-0163f79aee8d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b85af0323de46972ec6d6c4bf88359209b0015d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189701"
---
# <a name="sqlinstalldrivermanager-function"></a>Funzione SQLInstallDriverManager
**Conformità**  
 Versione introdotta: ODBC 1,0: deprecato in Windows XP Service Pack 2, Windows Server 2003 Service Pack 1 e sistemi operativi successivi  
  
 **Summary**  
 **SQLInstallDriverManager** restituisce il percorso della directory di destinazione per l'installazione dei componenti di base di ODBC. Il programma chiamante deve effettivamente copiare i file di gestione driver nella directory di destinazione.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLInstallDriverManager(  
     LPSTR    lpszPath,  
     WORD     cbPathMax,  
     WORD *   pcbPathOut);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszPath*  
 Output Percorso della directory di destinazione dell'installazione.  
  
 *cbPathMax*  
 Input Lunghezza di *lpszPath*. Deve essere almeno _MAX_PATH byte.  
  
 *pcbPathOut*  
 Output Numero totale di byte (escluso il byte di terminazione null) restituito in *lpszPath*. Se il numero di byte disponibili per restituire è maggiore o uguale a *cbPathMax*, il percorso in *lpszPath* viene troncato a *cbPathMax* meno il carattere di terminazione null. L'argomento *pcbPathOut* può essere un puntatore null.  
  
## <a name="returns"></a>Restituisce  
 La funzione restituisce TRUE se ha esito positivo, FALSE in caso di esito negativo.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLInstallDriverManager** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_INVALID_BUFF_LEN|Lunghezza del buffer non valida|L'argomento *lpszPath* non è sufficientemente grande da contenere il percorso di output. Il buffer contiene il percorso troncato.<br /><br /> L'argomento *cbPathMax* è minore di _MAX_PATH.|  
|ODBC_ERROR_USAGE_UPDATE_FAILED|Non è stato possibile incrementare o decrementare il numero di utilizzi dei componenti|Il programma di installazione non è riuscito a incrementare il conteggio di utilizzo del componente principale ODBC.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLInstallDriverManager** viene chiamato per restituire il percorso per i componenti di base di ODBC e incrementare il numero di utilizzo dei componenti nelle informazioni di sistema. Se una versione di gestione driver esiste già, ma il numero di utilizzo del componente per il driver non esiste, il nuovo valore di conteggio utilizzo componenti viene impostato su 2.  
  
 Il programma di installazione dell'applicazione è responsabile della copia fisica dei file del componente di base e della gestione dei conteggi di utilizzo del file. Se un file componente di base non è stato installato in precedenza, il programma di installazione dell'applicazione deve copiare il file e creare il conteggio di utilizzo dei file. Se il file è stato precedentemente installato, il programma di installazione incrementerà semplicemente il numero di utilizzo del file.  
  
 Se una versione precedente di gestione driver è stata installata in precedenza dal programma di installazione dell'applicazione, i componenti principali devono essere disinstallati e reinstallati, in modo che il conteggio di utilizzo dei componenti di base sia valido. **SQLRemoveDriverManager** deve essere chiamato prima per decrementare il numero di utilizzo dei componenti. **SQLInstallDriverManager** deve quindi essere chiamato per incrementare il conteggio di utilizzo dei componenti. Il programma di installazione dell'applicazione deve sostituire i vecchi file dei componenti di base con i nuovi file. I conteggi di utilizzo dei file rimarranno invariati e altre applicazioni che utilizzano i file dei componenti di base della versione precedente utilizzeranno ora i file di versione più recenti.  
  
 In una nuova installazione dei componenti di base di ODBC, driver e convertitori, il programma di installazione dell'applicazione deve chiamare le funzioni seguenti in sequenza: **SQLInstallDriverManager**, **SQLInstallDriverEx**, **SQLConfigDriver** (con un *fRequest* di ODBC_INSTALL_DRIVER), quindi **SQLInstallTranslatorEx**. In una disinstallazione dei componenti di base, dei driver e dei convertitori, il programma di installazione dell'applicazione deve chiamare le seguenti funzioni in sequenza: **SQLRemoveTranslator**, **SQLRemoveDriver** e quindi **SQLRemoveDriverManager**. Queste funzioni devono essere chiamate in questa sequenza. In un aggiornamento di tutti i componenti, tutte le funzioni di disinstallazione devono essere chiamate in sequenza, quindi tutte le funzioni di installazione devono essere chiamate in sequenza.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Aggiunta, modifica o rimozione di un driver|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|  
|Installazione di un driver|[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|  
|Installazione di un convertitore|[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|  
|Rimozione di un driver|[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|  
|Rimozione di gestione driver|[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|  
|Rimozione di un convertitore|[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|
