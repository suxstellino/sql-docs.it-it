---
description: MSSQLSERVER_15581
title: MSSQLSERVER_15581
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha, VenCher
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15581 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: fd227045a859f81fc6f96ba99cc402032ee00e99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343156"
---
# <a name="mssqlserver_15581"></a>MSSQLSERVER_15581
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|15581|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|SEC_NODBMASTERKEYERR|
|Testo del messaggio|Creare una chiave master nel database o aprire la chiave master nella sessione prima di eseguire questa operazione.|
||

## <a name="explanation"></a>Spiegazione

L'errore 15581 viene generato quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di recuperare un database abilitato per TDE (Transparent Data Encryption). Un messaggio di errore simile al seguente viene registrato nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

> 2020-01-14 22:16:26.47 spid20s Errore: 15581, gravità: 16, stato: 3.  
2020-01-14 22:16:26.47 spid20s Creare una chiave master nel database o aprire la chiave master nella sessione prima di eseguire questa operazione.

## <a name="possible-cause"></a>Possibile causa

Questo problema si verifica quando la crittografia della chiave master del servizio per la chiave master del database nel database master viene rimossa quando viene eseguito il comando seguente:

```sql
Use master
go
alter master key drop encryption by service master key
```

La chiave master del servizio viene usata per crittografare il certificato usato dalla chiave master del database. Qualsiasi tentativo di usare il database abilitato per TDE richiede l'accesso alla chiave master del database nel database master. Per aprire una chiave master non crittografata con la chiave master del servizio, è necessario usare l'istruzione [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md) insieme a una password per ogni sessione che richiede l'accesso alla chiave master. Poiché questo comando non può essere eseguito nelle sessioni di sistema, non è possibile completare il ripristino nei database abilitati per TDE.

## <a name="user-action"></a>Azione utente

Per risolvere il problema, abilitare la decrittografia automatica della chiave master. A tale scopo, eseguire i comandi seguenti:

```sql
Use master
go
open master key DECRYPTION BY PASSWORD = 'password'
alter master key add encryption by service master key
```

Usare la query seguente per determinare se la decrittografia automatica della chiave master con la chiave master del servizio è stata disabilitata per il database master:

```sql
select is_master_key_encrypted_by_server from sys.databases where name = 'master'
```

Se questa query restituisce il valore 0, la decrittografia automatica della chiave master con la chiave master del servizio è stata disabilitata.

## <a name="more-information"></a>Ulteriori informazioni

In alcuni casi, l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe sembrare non rispondere. Se si esegue una query sulla vista a gestione dinamica `sys.dm_exec_requests`, si noterà che il thread di `LogWriter` e gli altri thread che eseguono operazioni DML sono in attesa illimitata con wait_type WRITELOG. Anche altre sessioni potrebbero essere in attesa mentre tentano di ottenere blocchi.