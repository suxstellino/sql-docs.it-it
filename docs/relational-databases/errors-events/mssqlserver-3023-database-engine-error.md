---
description: MSSQLSERVER_3023
title: MSSQLSERVER_3023
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3023 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 213e761e37355f9e38276de9fab07c934a2cdaf8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348809"
---
# <a name="mssqlserver_3023"></a>MSSQLSERVER_3023
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|3023|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|DB_IN_USE_DUMP|
|Testo del messaggio|Le operazioni di backup e di manipolazione di file (ad esempio ALTER DATABASE ADD FILE) e le modifiche alla crittografia eseguite su un database devono essere serializzate. Eseguire nuovamente l'istruzione al termine dell'operazione corrente di backup o manipolazione di file.|
||

## <a name="explanation"></a>Spiegazione

Si tenta di eseguire un comando di backup, compressione o modifica del database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e vengono generati i messaggi seguenti:

> Messaggio 3023, livello 16, stato 2, riga 1  
Le operazioni di backup e di manipolazione di file (ad esempio ALTER DATABASE ADD FILE) e le modifiche alla crittografia eseguite su un database devono essere serializzate. Eseguire nuovamente l'istruzione al termine dell'operazione corrente di backup o manipolazione di file.

> Messaggio 3013, livello 16, stato 1, riga 1  
Interruzione anomala di BACKUP DATABASE in corso.

Inoltre, il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene messaggi simili ai seguenti:

> \<Datetime> Errore di backup: 3041, gravità: 16, stato: 1.  
\<Datetime> Backup BACKUP non è in grado di completare il comando BACKUP DATABASE MyDatabase WITH DIFFERENTIAL. Per messaggi più dettagliati, controllare il log dell'applicazione di backup.

Si potrebbe anche notare che questi comandi rilevano `wait_type = LCK_M_U` e `wait_resource = DATABASE: <id> [BULKOP_BACKUP_DB]` quando lo stato di questi comandi viene visualizzato dalle varie viste a gestione dinamica (DMV), ad esempio da `sys.dm_exec_requests` o `sys.dm_os_waiting_tasks`.

## <a name="possible-causes"></a>Possibili cause

Esistono diverse regole per le operazioni consentite o non consentite quando è in corso un backup completo del database per un database. Di seguito sono riportati alcuni esempi:

- È possibile eseguire un solo backup dei dati alla volta (durante un backup completo del database non è possibile eseguire contemporaneamente backup differenziali o incrementali).
- È possibile eseguire un solo backup del log alla volta, ovvero un backup del log è consentito durante l'esecuzione di un backup completo del database.
- Non è possibile aggiungere o eliminare file in un database mentre è in corso un backup.
- Non è possibile compattare i file mentre sono in corso backup del database.
- Sono consentite modifiche al modello di recupero limitate durante l'esecuzione dei backup.

Quando viene eseguita una di queste operazioni in conflitto, i comandi rileveranno le attese di blocco indicate nella sezione "Spiegazione" seguite dalla ricezione dei messaggi 3023 e 3041.

## <a name="user-action"></a>Azione utente

Esaminare le pianificazioni delle diverse attività di manutenzione del database, quindi modificare le pianificazioni in modo che tali operazioni o comandi non siano in conflitto.

## <a name="more-information"></a>Ulteriori informazioni

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra l'ora di inizio e l'ora di fine del backup nel database `msdb`. È possibile esaminare la cronologia di backup per determinare se era in corso un backup completo del database durante il tentativo di eseguire un backup incrementale, causando pertanto l'errore. Per semplificare questo processo, è possibile usare la query seguente:

```sql
select database_name, type, backup_start_date, backup_finish_date
from msdb.dbo.backupset
order by database_name, type, backup_start_date, backup_finish_date
go
```

È anche possibile usare l'evento **User Error Message** nella traccia di SQL Profiler o l'evento **error_reported** negli eventi estesi per ricollegare la segnalazione dei messaggi 3023 all'applicazione che ha avviato il backup o un altro comando di manutenzione.
