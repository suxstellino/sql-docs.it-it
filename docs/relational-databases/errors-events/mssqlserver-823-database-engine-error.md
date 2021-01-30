---
title: Errore 823 di MSSQLSERVER | mssqlserver_823
description: Una descrizione e alcune soluzioni comuni per l'errore 823 di Microsoft SQL Server (mssqlserver_823), restituito per segnalare una grave condizione di errore a livello di sistema che minaccia l'integrità del database e deve essere risolta immediatamente.
ms.custom: ''
ms.date: 01/27/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 823 (Database Engine error)
ms.assetid: 0d9fce3c-3772-46ce-a7a3-4f4988dc6cae
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7ed748bdf748aa51813d11348859bc769525ed35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197893"
---
# <a name="mssqlserver-error-823"></a>Errore 823 di MSSQLSERVER
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|823|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|B_HARDERR|  
|Testo del messaggio|Il sistema operativo ha restituito l'errore %ls a SQL Server durante un'operazione %S_MSG, all'offset %#016I64x nel file '%ls'. Per informazioni più dettagliate, vedere i messaggi aggiuntivi nel log degli errori di SQL Server e nel registro eventi di sistema. Si tratta di una condizione di errore grave a livello di sistema, che può compromettere l'integrità del database e deve essere corretta immediatamente. Eseguire un controllo di consistenza completo del database (DBCC CHECKDB). L'errore può essere causato da molti fattori. Per ulteriori informazioni, vedere la documentazione online di SQL Server.|  
  
## <a name="explanation"></a>Spiegazione  
SQL Server usa le API di Windows, ad esempio [ReadFile](/windows/win32/api/fileapi/nf-fileapi-readfile), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [ReadFileScatter](/windows/win32/api/fileapi/nf-fileapi-readfilescatter), [WriteFileGather](/windows/win32/api/fileapi/nf-fileapi-writefilegather), per eseguire operazioni di I/O su file. Dopo l'esecuzione di queste operazioni di I/O, SQL Server controlla se a queste chiamate API sono associate condizioni di errore. Se le chiamate API hanno esito negativo con un errore del sistema operativo, SQL Server restituisce l'errore 823.

 Il messaggio di errore 823 contiene le informazioni seguenti:
 - Il file di database su cui è stata eseguita l'operazione di I/O
 - L'offset all'interno del file in cui è stata tentata l'operazione di I/O. Si tratta dell'offset di byte fisico dall'inizio del file. Dividendo questo numero per 8192 si otterrà il numero di pagina logica interessato dall'errore.
 - Se l'operazione di I/O è una richiesta di lettura o scrittura
 - Codice di errore del sistema operativo e descrizione dell'errore tra parentesi
 

**Errore del sistema operativo:** una chiamata API di Windows in lettura o scrittura non riesce e SQL Server rileva un errore del sistema operativo correlato alla chiamata API di Windows. Di seguito è riportato un esempio di messaggio dell'errore 823:

```
Error: 823, Severity: 24, State: 2.
2010-03-06 22:41:19.55 spid58 The operating system returned error 1117 (The request could not be performed because of an I/O device error.) to SQL Server during a read at offset 0x0000002d460000 in file 'e:\program files\Microsoft SQL Server\mssql\data\mydb.MDF'. Additional messages in the SQL Server error log and system event log may provide more detail. This is a severe, system-level error condition that threatens database integrity and must be corrected immediately. It is recommended to complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```

È possibile che non vengano restituiti errori dall'istruzione DBCC CHECKDB sul database associato al file nel messaggio di errore. È possibile eseguire l'istruzione DBCC CHECKDB quando viene visualizzato un errore 823. Se l'istruzione DBCC CHECKDB non segnala alcun errore, è probabile che si sia verificato un problema di sistema intermittente o un problema del disco.

Quando si usa il flag di traccia 818, è possibile che nel file di log degli errori di SQL Server vengano scritte informazioni di diagnostica aggiuntive per gli errori 823.
Per altre informazioni, vedere l'articolo [KB 826433: Diagnostica di SQL Server aggiunta per rilevare problemi di I/O non segnalati](https://support.microsoft.com/help/826433/sql-server-diagnostics-added-to-detect-unreported-i-o-problems-due-to)


## <a name="cause"></a>Causa
Il messaggio di errore 823 indica in genere che si è verificato un problema con il sistema di archiviazione sottostante o con l'hardware oppure con un driver che si trova nel percorso della richiesta di I/O. Questo errore può verificarsi se sono presenti incoerenze nel file system o se il file di database è danneggiato. Nel caso di una lettura di file, SQL Server ripete la richiesta di lettura quattro volte prima di restituire 823. Se l'operazione di ripetizione dei tentativi ha esito positivo, la query verrà eseguita, ma il messaggio [825](mssqlserver-825-database-engine-error.md) verrà scritto in ERRORLOG e nel registro eventi.

## <a name="user-action"></a>Azione dell'utente  
 - Esaminare la tabella [suspect_pages](../system-tables/suspect-pages-transact-sql.md) in MSDB per altre pagine in cui si è verificato il problema (nello stesso database o in database diversi).
 - Verificare la coerenza dei database presenti nello stesso volume (come quello riportato nel messaggio 823) usando il comando DBCC CHECKDB. Se vengono rilevate incoerenze dal comando DBCC CHECKDB, usare le linee guida disponibili in [Risoluzione degli errori di coerenza del database segnalati da DBCC CHECKDB](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check). 
 - Esaminare i registri eventi di Windows per individuare eventuali errori o messaggi segnalati dal sistema operativo oppure da un dispositivo di archiviazione o un driver di dispositivo. Se in qualche modo sono correlati a questo errore, risolvere prima tali errori. Ad esempio, oltre al messaggio 823, è possibile notare un evento come "Il driver ha rilevato un errore del controller in \Device\Harddisk4\DR4" segnalato dall'origine disco nel registro eventi. In tal caso, è necessario valutare se il file è presente nel dispositivo e quindi correggere gli errori del disco rilevati.
 - Usare l'utilità [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) per verificare se questi errori 823 possono essere riprodotti al di fuori delle normali richieste di I/O di SQL Server. L'utilità SQLIOSim viene fornita con SQL Server 2008 e versioni successive, pertanto non è necessario un download separato. In genere è possibile trovarla nella cartella `C:\Program Files\Microsoft SQL Server\MSSQLxx.MSSQLSERVER\MSSQL\Binn`.
 - Collaborare con il fornitore dell'hardware o il produttore del dispositivo per verificare quanto segue:
   - I dispositivi hardware e la configurazione sono conformi ai requisiti di I/O di SQL Server
   - I driver di dispositivo e altri componenti software di supporto di tutti i dispositivi nel percorso di I/O sono aggiornati
 - Se il fornitore dell'hardware o il produttore del dispositivo ha fornito utilità di diagnostica, usarle per valutare l'integrità del sistema di I/O
 - Valutare se sono presenti [driver di filtro](https://support.microsoft.com/help/2454053/use-of-system-filter-drivers-can-lead-to-sql-server-database-engine-pe) nel percorso di queste richieste di I/O in cui si verificano problemi.
   - Controllare se sono presenti aggiornamenti per questi driver di filtro
   - I driver di filtro possono essere rimossi o disabilitati per verificare se il problema che causa l'errore 823 è stato risolto  
