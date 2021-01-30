---
description: SQLManageDataSources
title: SQLManageDataSources | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLManageDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLManageDataSources
helpviewer_keywords:
- SQLManageDataSources [ODBC]
ms.assetid: ac6d186f-b394-406c-94c4-c6331d1ca468
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fbb34d2daafc4a135cbf51c2b0382d3965f9dfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160804"
---
# <a name="sqlmanagedatasources"></a>SQLManageDataSources
**Conformità**  
 Versione introdotta: ODBC 2,0  
  
 **Summary**  
 **SQLManageDataSources** Visualizza una finestra di dialogo con cui gli utenti possono impostare, aggiungere ed eliminare origini dati nelle informazioni di sistema.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp  
  
BOOL SQLManageDataSources(  
     HWND     hwnd);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HWND*  
 Input Handle della finestra padre.  
  
## <a name="returns"></a>Restituisce  
 **SQLManageDataSources** restituisce false se *HWND* non è un handle di finestra valido. In caso contrario, restituisce TRUE.  
  
## <a name="diagnostics"></a>Diagnostica  
 Quando **SQLManageDataSources** restituisce false, è possibile ottenere un valore *\* pfErrorCode* associato chiamando **SQLInstallerError**. La tabella seguente elenca i valori *\* pfErrorCode* che possono essere restituiti da **SQLInstallerError** e ne illustra ognuno nel contesto di questa funzione.  
  
|*\*pfErrorCode*|Errore|Descrizione|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|Errore generale del programma di installazione|Si è verificato un errore per il quale non è stato specificato alcun errore di programma di installazione.|  
|ODBC_ERROR_REQUEST_FAILED|*Richiesta* non riuscita|La chiamata a **ConfigDSN** non è riuscita.|  
|ODBC_ERROR_INVALID__HWND|Handle di finestra non valido|Argomento *HWND* non valido o null.|  
|ODBC_ERROR_OUT_OF_MEM|Memoria insufficiente|Il programma di installazione non è riuscito a eseguire la funzione a causa di memoria insufficiente.|  
  
## <a name="managing-data-sources"></a>Gestione delle origini dati  
 **SQLManageDataSources** Visualizza inizialmente la finestra di dialogo **Amministrazione origine dati ODBC** , come illustrato nella figura seguente.  
  
 ![Finestra di dialogo Amministratore origine dati ODBC](../../../odbc/reference/syntax/media/ch23e.gif "CH23E")  
  
 Nella finestra di dialogo vengono visualizzate le origini dati elencate nelle informazioni di sistema in tre schede: **DSN utente**, **DSN di sistema** e **DSN su file**. Se l'utente fa doppio clic su un'origine dati o seleziona un'origine dati e fa clic su **Configura**, **SQLManageDataSources** chiama **ConfigDSN** nella DLL di installazione con l'opzione ODBC_CONFIG_DSN.  
  
 Se l'utente fa clic su **Aggiungi**, **SQLManageDataSources** Visualizza la finestra di dialogo **Crea nuova origine dati** , mostrata nella figura seguente.  
  
 ![Finestra di dialogo Crea nuova origine dati](../../../odbc/reference/syntax/media/ch23f.gif "CH23F")  
  
 Nella finestra di dialogo viene visualizzato un elenco dei driver installati. Se l'utente fa doppio clic su un driver o seleziona un driver e fa clic su **OK**, **SQLManageDataSources** chiama **ConfigDSN** nella DLL di installazione e passa l'opzione ODBC_ADD_DSN.  
  
 Se l'utente seleziona un'origine dati e fa clic su **Rimuovi**, **SQLManageDataSources** chiede se l'utente vuole eliminare l'origine dati. Se l'utente fa clic su **Sì**, **SQLManageDataSources** chiama **ConfigDSN** nella DLL di installazione con l'opzione ODBC_REMOVE_DSN.  
  
 La finestra di dialogo **Crea nuova origine dati** viene utilizzata per aggiungere o eliminare un'origine dati utente, un'origine dati di sistema o un'origine dati file.  
  
## <a name="user-dsns"></a>DSN utente  
 I DSN creati per i singoli utenti verranno chiamati DSN utente per distinguerli dai DSN di sistema. I DSN utente vengono registrati come indicato di seguito nelle informazioni di sistema:  
  
 `HKEY_CURRENT_USERS`  
  
 `SOFTWARE`  
  
 `ODBC`  
  
 `Odbc.ini`  
  
## <a name="system-dsns"></a>DSN di sistema  
 La finestra di dialogo **Crea nuova origine dati** consente di aggiungere un'origine dati di sistema al computer locale o di eliminarne uno oppure di impostare la configurazione per un'origine dati di sistema.  
  
 Un'origine dati configurata con un nome dell'origine dati di sistema (DSN) può essere utilizzata da più di un utente nello stesso computer. Può anche essere usato da un servizio a livello, che può quindi ottenere l'accesso all'origine dati anche se nessun utente è connesso al computer.  
  
 Un DSN di sistema viene registrato nella voce HKEY_LOCAL_MACHINE delle informazioni di sistema anziché nella voce HKEY_CURRENT_USER. Non è associato a un utente che accede con il nome utente e la password specifici ma può essere usato da qualsiasi utente del computer o da un servizio a livello automatico. Il DSN di sistema, tuttavia, è associato a un solo computer. Non supporta la funzionalità di utilizzo di DSN remoti tra computer. I DSN di sistema vengono registrati come indicato di seguito nelle informazioni di sistema:  
  
 Odbc.ini ODBC SOFTWARE HKEY_LOCAL_MACHINE  
  
## <a name="file-dsns"></a>DSN file  
 Un'origine dati file non dispone di un nome di origine dati, così come un'origine dati del computer, e non è registrata per un utente o un computer. Le informazioni di connessione per l'origine dati sono contenute in un file con estensione DSN che può essere copiato in qualsiasi computer. Un'origine dati di file può essere condivisibile, nel qual caso il file con estensione DSN si trova in una rete e può essere utilizzato contemporaneamente da più utenti nella rete, purché l'utente disponga del driver appropriato. Un'origine dati file può inoltre essere non condivisibile, nel qual caso può essere utilizzata solo in un singolo computer.  
  
 Per ulteriori informazioni sulle origini dati dei file, vedere [connessione tramite origini dati di file](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md)o vedere [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
## <a name="managing-drivers"></a>Gestione dei driver  
 Se l'utente fa clic sulla scheda **driver** nella finestra di dialogo **Amministrazione origine dati ODBC** , **SQLManageDataSources** Visualizza un elenco di driver ODBC installati nel sistema, nonché informazioni sui driver. La data visualizzata corrisponde alla data di creazione del driver, come illustrato nella figura seguente.  
  
 ![Scheda Driver della finestra di dialogo Amministratore origine dati ODBC](../../../odbc/reference/syntax/media/ch23g.gif "ch23g")  
  
## <a name="tracing-options"></a>Opzioni di traccia  
 Se l'utente fa clic sulla scheda **traccia** nella finestra di dialogo **Amministrazione origine dati ODBC** , **SQLManageDataSources** Visualizza le opzioni di traccia, come illustrato nella figura seguente.  
  
 ![Scheda Analisi della finestra di dialogo Amministratore origine dati ODBC](../../../odbc/reference/syntax/media/ch23h.gif "Ch23h")  
  
 Se l'utente fa clic su **Avvia traccia ora** e quindi fa clic su **OK**, **SQLManageDataSources** Abilita la traccia manualmente per tutte le applicazioni attualmente in esecuzione nel computer.  
  
 Se l'utente specifica il nome di un file di traccia nella casella di testo **percorso file di log** e quindi fa clic su **OK**, **SQLManageDataSources** imposta la parola chiave **TraceFile** nella sezione [ODBC] delle informazioni di sistema sul nome specificato.  
  
> [!IMPORTANT]  
>  Il supporto per Visual Studio Analyzer è stato rimosso a partire da Windows 8 (Visual Studio Analyzer era incluso solo nelle versioni precedenti di Visual Studio). Per un meccanismo alternativo di risoluzione dei problemi, usare la traccia delle offerte.  
  
 Se l'utente fa clic su **Start Visual Studio Analyzer** e quindi fa clic su **OK**, Visual Studio Analyzer è abilitato. Rimane abilitata fino a quando non si fa clic su **arresta Visual Studio Analyzer** .  
  
 Per ulteriori informazioni sulla traccia, vedere la pagina relativa alla [traccia](../../../odbc/reference/develop-app/tracing.md). Per ulteriori informazioni sulle parole chiave **Trace** e **TraceFile** , vedere la [sottochiave ODBC](../../../odbc/reference/install/odbc-subkey.md).  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Creazione di origini dati|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|
