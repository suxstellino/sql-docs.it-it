---
description: Funzione SQLGetPrivateProfileString
title: Funzione SQLGetPrivateProfileString | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetPrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetPrivateProfileString
helpviewer_keywords:
- SQLGetPrivateProfileString function [ODBC]
ms.assetid: b72ca065-4d67-48df-baac-e18379a8320a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6ec5dfa03f04b004f704ad707fb7916d40e69716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186190"
---
# <a name="sqlgetprivateprofilestring-function"></a>Funzione SQLGetPrivateProfileString
**Conformità**  
 Versione introdotta: ODBC 2,0  
  
 **Summary**  
 **SQLGetPrivateProfileString** ottiene un elenco di nomi di valori o dati corrispondenti a un valore delle informazioni di sistema.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
int SQLGetPrivateProfileString(  
     LPCSTR   lpszSection,  
     LPCSTR   lpszEntry,  
     LPCSTR   lpszDefault,  
     LPCSTR   RetBuffer,  
     INT      cbRetBuffer,  
     LPCSTR   lpszFilename);  
```  
  
## <a name="arguments"></a>Argomenti  
 *lpszSection*  
 Input Punta a una stringa con terminazione null che specifica la sezione contenente il nome della chiave. Se questo argomento è NULL, la funzione copia tutti i nomi di sezione del file nel buffer fornito.  
  
 *lpszEntry*  
 Input Punta alla stringa con terminazione null contenente il nome della chiave di cui è necessario recuperare la stringa associata. Se questo argomento è NULL, tutti i nomi di chiave nella sezione specificata dall'argomento *lpszSection* vengono copiati nel buffer specificato dall'argomento *RetBuffer* .  
  
 *lpszDefault*  
 Input Punta a una stringa con terminazione null che specifica il valore predefinito per la chiave specificata, se la chiave non è stata trovata nel file di inizializzazione. Questo argomento non può essere null.  
  
 *RetBuffer*  
 Output Punta al buffer che riceve la stringa recuperata.  
  
 *cbRetBuffer*  
 Input Specifica la dimensione, in caratteri, del buffer a cui punta l'argomento *RetBuffer* .  
  
 *lpszFilename*  
 Input Punta a una stringa con terminazione null che denomina il file di inizializzazione. Se questo argomento non contiene un percorso completo del file, viene eseguita una ricerca nella directory predefinita.  
  
## <a name="returns"></a>Restituisce  
 **SQLGetPrivateProfileString** restituisce un valore integer che indica il numero di caratteri letti.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando una chiamata a **SQLGetPrivateProfileString** ha esito negativo, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="comments"></a>Commenti  
 **SQLGetPrivateProfileString** viene fornito come metodo semplice per trasferire i driver e le DLL di installazione dei driver da Microsoft® Windows® a Microsoft windows NT®/Windows 2000. Le chiamate a **GetPrivateProfileString** che recuperano una stringa del profilo dal file di Odbc.ini devono essere sostituite con le chiamate a **SQLGetPrivateProfileString**. **SQLGetPrivateProfileString** chiama funzioni nell'API Win32® per recuperare i nomi richiesti di valori o dati corrispondenti a un valore della sottochiave Odbc.ini delle informazioni di sistema.  
  
 La modalità di configurazione (impostata da **SQLSetConfigMode**) indica il punto in cui la voce Odbc.ini elencando i valori DSN è presente nelle informazioni di sistema. Se il DSN è un DSN utente (la modalità di configurazione è USERDSN_ONLY), la funzione legge dalla voce Odbc.ini in HKEY_CURRENT_USER. Se il DSN è un DSN di sistema (SYSTEMDSN_ONLY), la funzione legge dalla voce Odbc.ini in HKEY_LOCAL_MACHINE. Se la modalità di configurazione è BOTHDSN, viene tentata HKEY_CURRENT_USER e, in caso di errore, viene utilizzato HKEY_LOCAL_MACHINE.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Scrittura di un valore nelle informazioni sul sistema|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
