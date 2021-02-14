---
description: MSSQLSERVER_
title: MSSQLSERVER_4214
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4214 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 1ccce1ca406527cfca2485bec41f2fb9d287862c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348768"
---
# <a name="mssqlserver_4214"></a>MSSQLSERVER_4214
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|4214|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|DMPX_NODBBACKUP|
|Testo del messaggio|Impossibile eseguire BACKUP LOG perché non è disponibile un backup di database corrente|
||

## <a name="explanation"></a>Spiegazione

L'errore viene generato quando si tenta di eseguire un backup del log delle transazioni prima di eseguire un backup completo di un database. Prima di provare a eseguire il backup del log delle transazioni per un database, è necessario eseguire un backup completo del database. Nel log degli errori viene inoltre visualizzato il messaggio seguente:

> \<Datetime> Errore di backup: 3041, gravità: 16, stato: 1.  
\<Datetime>  Backup     BACKUP non è in grado di completare il comando BACKUP LOG. \<db_name>. Per messaggi più dettagliati, controllare il log dell'applicazione di backup.

## <a name="user-action"></a>Azione utente

Eseguire un backup completo del database prima di provare a eseguire il backup del log delle transazioni per un database.

## <a name="more-information"></a>Ulteriori informazioni

Per altre informazioni, vedere: [Backup e ripristino di database SQL Server](../backup-restore/back-up-and-restore-of-sql-server-databases.md).